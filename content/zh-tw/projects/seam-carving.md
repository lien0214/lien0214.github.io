---
title: "Seam Carving × V100"
description: "以 profiling 驅動的 exact seam carving CUDA 優化：從 Naive 到 Tiled 五版 kernel，8K 達 5.73 ms/seam，並證明剩下的天花板是結構性的。40 組中獲選 Top 10。"
date: 2026-06-14
period: "2026 年 6 月"
team: "3 人團隊 · 主要貢獻者"
category: "Project"
status: "Top 10 / 40 組 · 台大平行程式設計"
github: "https://github.com/YuXiangLo/NTUPDP2026"
stack: ["CUDA", "C++", "OpenMP", "Nsight", "GPU", "平行程式設計"]
cover:
  image: /images/posts/seam-carving.png
  alt: "原始圖片、標示出的 seam 路徑（紅色）以及 content-aware 裁切後的結果"
---

![Seam carving：原圖、seam、裁切結果](/images/posts/seam-carving.png)

## 這是什麼

Seam carving 透過反覆移除能量最低的連續像素路徑來縮放圖片。每條 seam 有兩個重的階段：可完全平行的能量圖計算，以及列與列之間互相依賴的動態規劃。我們想知道在 NVIDIA V100 上，exact 版本最快能跑多快。

## 我們做了什麼

五版逐步加速的 kernel，每一步都由 Nsight 指出的瓶頸驅動：

- **Naive**：每一列一次 kernel launch，每列都要來回 global memory
- **Fused**：所有列合併成一個 kernel，用 shared memory 雙緩衝
- **Prefetch**：軟體管線化預先載入下一列能量，隱藏 `long_scoreboard` stall（單步最大提升，1.67×）
- **BytePtr**：back-pointer 從 `int` 改成 `signed char`，該部分流量降為 1/4
- **Tiled**：切成帶 halo 的直條，讓所有 SM 同時工作，結果仍然 bit-exact

## 成果

- **8K 解析度 5.73 ms/seam**：比單核 CPU 快 57×，比調校過的 32 執行緒 NUMA-aware OpenMP 快 6.2×
- 證明天花板是結構性的：DP 的 critical path 無法縮短、轉置沒有幫助、就算完全拿掉 DP 也會撞上記憶體頻寬下限
- 近似 batch 模式把一次 DP 攤提到 60 條 seam，達到 **0.103 ms/seam**（快 56×）

在台大平行與分散式程式設計期末專題競賽中獲選 **40 組中的 Top 10**。

[閱讀完整文章 →](/zh-tw/posts/seam-carving-gpu/) · [論文（PDF）](https://github.com/YuXiangLo/NTUPDP2026/blob/main/paper/seam_carving.pdf)
