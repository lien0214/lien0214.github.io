---
title: "我們試著讓 Seam Carving 在 GPU 上跑更快——然後我們證明了為什麼它不能"
date: 2026-06-14
description: "在 NVIDIA V100 上對 exact seam carving 做 profiling 驅動的優化，從 Naive → Fused → Prefetch → BytePtr → Tiled，在 8K 解析度達到 5.73 ms/seam，並証明這個天花板是結構性的。"
tags:
  - cuda
  - parallel-programming
  - research
  - gpu
  - systems
categories:
  - Engineering
showToc: true
cover:
  image: /images/posts/seam-carving.png
  alt: "原始圖片、標示出的 seam 路徑（紅色）以及 content-aware 裁切後的結果"
  relative: false
  hidden: false
  hiddenInSingle: false
---

> 完整論文：[seam_carving.pdf](https://github.com/YuXiangLo/NTUPDP2026/blob/main/paper/seam_carving.pdf) · 程式碼：[YuXiangLo/NTUPDP2026](https://github.com/YuXiangLo/NTUPDP2026)  
> 與吳雅蓁、羅宇翔合作，台大平行程式設計，Spring 2026。

**本專案獲得台大平行與分散式程式設計期末競賽 Top 10（共 40 組）。** 發表影片嵌入如下，我們的上台時間從 38:30 開始。

<iframe width="100%" style="aspect-ratio:16/9" src="https://www.youtube.com/embed/1fGVECimgnw?start=2310" title="NTU PDP 2026 Final Project Showcase — Seam Carving GPU Study" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 起點

Seam carving 是一種 content-aware 的圖像縮放演算法。它不是裁切或縮放，而是移除圖片中「最不重要」的像素路徑——seam——同時保留視覺上重要的區域。效果出乎意料地好。

演算法主要有兩個沉重的步驟：先計算 energy map（梯度大小，對整張圖掃一遍）；再執行動態規劃（DP）找出從上到下代價最小的連通路徑。移除那條 seam，重複。

在 CPU 上，這對大圖來說很慢。單核心處理一張 8K 圖（7680×4320，約 3300 萬像素）每條 seam 可能要幾百毫秒。我們有 V100。Energy 計算是完美的平行問題。問題看起來很明顯：能快多少？

我們原本預期找到一個新的優化方式，拿到一個漂亮的數字。最後我們證明了為什麼漂亮的數字在結構上不可能達到——而這才是更有趣的結果。

## DP 的問題

用 Nsight Compute 對 1428×968 的圖做 profiling，問題立刻清楚了：DP kernel 佔每條 seam 牆上時間的 ~93%。其他全是雜訊。

DP 的遞推關係是：

```
M[i][j] = e[i][j] + min(M[i-1][j-1], M[i-1][j], M[i-1][j+1])
```

每一行依賴上一行。全部 H 行都是串行的。每行內部的 W 列可以平行——但在第 `i-1` 行完成之前無法開始第 `i` 行，沒有任何重新排序能消除這個依賴。這就是我們一直碰到的牆。

## 優化路徑

我們在每個步驟以 Nsight 為導引，建立了五個逐步改良的 kernel。

**Naive** 每行啟動一個 CUDA kernel。在 4K（H=2160）時，每條 seam 要啟動 2159 次 kernel，每次都有完整的 global memory 往返。小圖上 overhead 主導，大圖上 global memory 流量主導。

**Fused** 用 shared-memory ping-pong buffer 把所有 H 行折疊進單一 kernel。行之間只用一次 `__syncthreads()`，2159 次 kernel 啟動變成 1 次，cost array 在行與行之間不再碰 DRAM，住在 96 KB 的 shared memory 裡。這是正確的結構；問題是能跑多快。

**Prefetch** 來自閱讀 Nsight 輸出，不是直覺。Fused 的主要 stall 是 `long_scoreboard`——每個 warp 等 ~200 cycles 等 global load 讀 `e[i][·]`。修法是 software pipelining：在計算第 `i` 行（只讀 shared memory cost）的同時，每個 thread 把 `e[i+1][·]` 的 global load 提前發出存進 register。等 `__syncthreads()` 結束，load 已經 resolve 了。運算和記憶體傳輸重疊。**這給出了最大的單步提升：kernel 層面比 Fused 快 1.67×。**

**BytePtr** 注意到 back-pointer——值只有 `{-1, 0, +1}`——卻用 4-byte `int` 儲存。改成 `signed char` 讓 back-store 流量縮減 4×，在 4K（3840×2160）時 back-pointer array 從 32 MB 縮到 8 MB per seam pass。

**Tiled** 是終於讓另外 79 個閒置 SM 動起來的設計。做法：把 W 列分成 K=60 條 strip（每個 SM 一條）。每條 strip 兩側各有 T 個 halo 列。在 T 行的 tile 內，一個 block 完全在 shared memory 裡工作，只在 tile 邊界把 center strip 寫回 global memory。

正確性靠 seam-drift bound：在 T 行內，seam 最多偏移 T 列。任何誤差都在 halo 內。中心是 bit-exact。

結果：kernel 啟動次數從 H 降到 ⌈H/T⌉（8K、T=64 時是 68 次），60 個 SM 同時活躍。輸出與單 block 設計 bit-identical。

## 數字

在 8K（7680×4320），Tiled 達到 **5.73 ms/seam**——是我們所有 exact 設計中最快的。

| 方法 | ctrl | 1080p | 2K | 4K | 6K | 8K |
|------|------|-------|----|----|----|-----|
| CPU 單核 | 5.00 | 21.6 | 37.5 | 83.6 | 212 | 325 |
| CPU 最佳（32 執行緒） | 1.14 | 3.59 | 5.63 | 9.88 | 25.6 | 35.4 |
| Naive | 1.37 | 3.10 | 3.77 | 5.92 | 10.3 | 13.9 |
| Tiled（我們的） | **0.314** | **0.917** | **1.25** | **2.16** | **4.16** | **5.73** |
| 對比 CPU 最佳 | 3.6× | 3.9× | 4.5× | 4.6× | 6.2× | **6.2×** |

比單核 CPU 快 57×。比調優過的 32 執行緒 NUMA-aware OpenMP 快 6.2×。最後這個數字是天花板——為什麼是天花板才是有趣的部分。

## 天花板是結構性的：三個理由

到這裡，合理的問題是：我們只是不夠努力嗎？我們花了相當多時間問自己同樣的問題。答案是否定的，而且可以用三種方式說明。

**1. DP 的關鍵路徑是不可化簡的。**  
依賴圖的最長路徑長度是 H——每行一個節點。任何排程都至少需要 H 個串行的 min-step。GPU 可以在每行內部平行化（W 列），但無法減少 H 個串行步驟。行內最佳平行化就是 Tiled 已經做到的。

**2. Transpose 幫不上忙。**  
直覺上的想法：把圖旋轉，換個方向跑 DP。但 transpose 是 storage 地址上的雙射——只是重新標記了儲存，依賴圖不變，關鍵路徑還是 H。如果 transpose 後再 carve，算的是水平 seam（深度 W 而非 H），那是正交任務，不是原任務的更快版本。我們用實驗確認：對寬圖來說，transposed 方向更慢，因為 W > H。

**3. 把 DP 整個移掉也沒有提速。**  
這是最令人驚訝的結果。我們實作了 greedy non-cumulative selection——沒有 cost matrix、沒有串行依賴、完全平行。它在每個解析度都和 Tiled 差在 3% 以內。為什麼？因為每條 seam 的 pipeline，不是 DP，才是端到端的瓶頸。8K 下，5.73 ms 分成 ~4.3 ms DP 和 ~1.4 ms preprocessing。Preprocessing 的下界（energy 計算 + seam 移除）需要讀取每個像素的 energy、寫出輸出——在 8K 是 ~398 MB，對 V100 的 ~900 GB/s 頻寬來說，無論用哪種 selection 方法都要花 ~1.4 ms。Greedy 沒有 DP，但同樣要付這個頻寬下界的代價。它贏不了。

## 唯一有效的槓桿

有一個出口：改變問題本身。

Approximate batch removal 把一次 DP pass 攤銷到 K=60 條 seam。一次前向 pass 產生 60 條 strip-local seam 候選；單一 kernel 在一次 image-wide 讀寫中移除全部 60 條。每條 seam 的代價從 Θ(HW) 變成 Θ(HW/K)。

結果：**8K 下 0.103 ms/seam——比 exact single-seam pipeline 快 56×。** 代價是 strip-local 而非 global 最優：找到的 seam 是各自 strip 內的最佳，不保證是全域最小值。

這就是正式產品 pipeline 實際採用的 batch mode。Exact per-seam 的設法在工業上不是重要的情境；我們研究它是因為結構性限制在這裡最乾淨。

## 我們真正學到的

進去的時候，我們期望用新技巧超越前人的 GPU 實作。我們確實建出了我們找得到最強的 exact pipeline——Tiled 在每個解析度都比 Kim et al. 的 exact single-block DP 更快，Nsight 引導的 latency hiding 讓他們放棄的方案重新有競爭力。但比調優 CPU 快 6.2× 並不是他們近似方法達到的 76×。

更有價值的輸出是理解「為什麼」。DP 的關鍵路徑不可化簡。Transpose 沒幫助。移掉 DP 沒幫助。頻寬下界跟演算法無關。這些不是實作的失敗——這些是問題的性質。

有時候，最有用的結果是對可達前沿的嚴謹刻畫，加上解釋為何更多工程力氣也推不動它的不可能性論證。我們花在「證明天花板是真實的」的時間，比花在「撞上天花板」的時間還多。我認為這是讀這篇 paper 的正確方式。

唯一的大槓桿是攤銷。如果你願意用 strip-local 最優換 global 最優，batch mode 給你 56×。如果你需要 exact 結果，在 V100 上 6.2× 已接近可達的最大值——而這個值和頻寬下界之間的差距（~1.3×）告訴你所剩不多了。
