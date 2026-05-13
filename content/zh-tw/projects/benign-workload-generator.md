---
title: "Benign Workload Generator"
description: "模擬真實 Linux 伺服器良性負載的工具包——為中研院 Citi Lab 的 GAN 入侵偵測模型生成高品質訓練資料。"
date: 2023-09-01
period: "2023 年 9 月 – 2024 年 6 月"
team: "中研院 · Citi Lab"
category: "Research"
github: "https://github.com/lien0214/Benign-Workload-Generator"
stack: ["Python", "Shell", "資安", "GAN", "合成資料", "Linux"]
---

## 背景

中研院 Citi Lab 入侵偵測研究的一部分。核心目標是透過真實的合成 Linux audit log 擴增 IDS 模型的訓練資料——但要生成有用的合成*攻擊*，首先需要真實的合成*良性*行為作為對照。

## 功能

**伺服器負載腳本**：
- `pure.sh`——生成可配置的重複性 HTTP 請求（GET、POST、PUT）
- `scenario_combine.sh`——隨機化請求類型和並發操作的動態負載，更接近真實流量模式

**系統負載腳本**：
- `main-pure-task.sh`——協調硬碟 I/O、虛擬記憶體壓力、矩陣運算和進程創建壓力測試（各約 2 分鐘）
- 針對非同步 I/O、記憶體分配和計時器操作的個別腳本

## 為什麼重要

能生成「真實」log 的 GAN，只有在合成攻擊能騙過以真實資料訓練的偵測器時才有價值。良性基準的品質直接決定了合成攻擊資料是否值得使用。讓負載分布正確——不只是 log 條目的語法——才是核心挑戰。

第一次真正體會學術 Benchmark 與資安 ML 實務落差有多大。
