---
title: "履歷"
url: /zh-tw/resume/
summary: resume
description: "連奕維的履歷 — 台大資工碩士（MIRLab），後端與系統工程師。CMoney、Shopback 實習，GPU 優化與多智能體強化學習研究，2027 年可到職。"
ShowToc: true
TocOpen: true
ShowReadingTime: false
hidemeta: true
---

<div class="resume-actions">
  <a href="https://www.overleaf.com/project/6a55ba2050683eb13b963bf4/share#dd8b9832c4a5f61ed1adc6462f6c3b911505aa8d4cc52bfc" target="_blank" rel="noopener" class="project-btn">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/></svg>
    檢視 CV（Overleaf）
  </a>
</div>

## 學歷

| | |
|---|---|
| **資訊工程系碩士** | 國立台灣大學（MIRLab）· 2025–2027 |
| **資訊工程系學士** | 國立台灣大學 · 2021–2025 · 已畢業 |

## 工作經歷

### AI/ML 工程師實習 · CMoney
*2026 年 9 月 – 現在 · 新北市 · 混合辦公*

`推薦系統` `時間序列預測` `PyTorch` `開源資料集`

- 評估以單任務 / 多任務神經網路取代既有以樹模型為基礎的線上 reranker
- 評估為 CMoney 論壇建立開源資料集的可行性

### 軟體開發工程師實習 · Shopback
*2025 年 5 月 – 11 月 · 台北 · 混合辦公*

`TypeScript` `Kubernetes` `Monorepo` `SQL` `CI`

- 消除 10+ 個 API 的 RPC 開銷，延遲從 ~100ms 降至 10ms 以下
- 強化 5+ 個關鍵 API 的可靠性，錯誤率降低 10–15%

### 後端開發工程師實習 · CMoney
*2024 年 7 月 – 2025 年 4 月 · 新北市 · 混合辦公*

`C#` `ASP.NET` `OOP` `微服務` `MongoDB` `高併發`

- 針對旗艦產品（7M+ 下載量）的 9 個後端 API 實作微服務重構

### 研究實習生 · 中央研究院
*台北 · 混合辦公*

`資安` `資料生成` `Audit Log` `多智能體強化學習` `強化學習`

- **資訊所（IIS）** · 2026 年 6 月 – 8 月：獨立完成 MARL parameter sharing 研究專案（[DRPS](/zh-tw/projects/drps-marl/)，AAMAS 2027 審稿中）
- **資創中心（CITI）**（大學部）· 2023 年 9 月 – 2024 年 6 月：打造資料集生成系統，產出 2M+ 筆研究用資料

### 助教 · 台大資工系
*台北 · 實體*

- **CSIE5434 金融科技導論** · 2026 年 9 月 – 2027 年 2 月
- **CSIE1212 資料結構與演算法** · 2023 年 2 月 – 6 月

## 專案

### Seam Carving 在 V100 上的優化
*2026 年 6 月 · 3 人團隊 · 主要貢獻者 · 台大平行程式設計期末專題 Top 10（共 40 組）*

`CUDA` `GPU` `OpenMP` `Nsight` `平行程式設計`

- 以 Nsight profiling 引導，迭代出五版 CUDA kernel，8K 解析度達 5.73 ms/seam（比調校過的 32 執行緒 OpenMP 快 6.2×）
- 證明剩下的天花板是結構性的，並展示近似 batch 模式可再快 56×（[文章](/zh-tw/posts/seam-carving-gpu/)）

### RL / MARL 投資組合管理 · MIRLab × 玉山金控
*2026 年 6 月 – 現在 · 產學合作計畫*

`強化學習` `多智能體強化學習` `投資組合管理`

### 基於圖論的 RAG 防禦 · AIS Lab
*2026 年 3 月 – 7 月 · 3 人團隊 · 共同第一作者 · ACL ARR（2026 年 10 月）審稿中*

`RAG` `AI Safety` `圖論` `黑箱防禦` `免訓練`

- 提出免訓練的黑箱防禦方法，透過圖的不對稱性偵測 RAG 投毒

### ICC Mem/Gen · MSLab
*2026 年 3 月 – 6 月 · 4 人團隊*

`知識衝突` `PEFT` `LoRA` `Activation Steering`

- 為 ConflictQA/ConflictBank 開發跨格式資料增強 Pipeline

### 量化交易專案 · MIRLab
*2026 年 1 月 – 5 月 · 3 人團隊*

`Quant` `台股` `日策略` `Docker`

### Bubblo · 2D 平台遊戲
*2025 年 4 月 – 6 月 · 6 人團隊 · 專案負責人 · 600+ Commits*

`Unity` `C#` `OOP` `設計模式`

- 在任何遊戲程式碼開始前就設計好元件架構，明確採用 Observer、State Machine 和 Command 模式
- 主導每週站立會議，維護三個月衝刺的任務看板

## 技術能力

**程式語言：** TypeScript · Python · C# · C/C++

**系統與平行：** CUDA · OpenMP · Nsight profiling

**後端：** ASP.NET Core · Node.js · Kubernetes · Docker · MongoDB · PostgreSQL · SQL

**工作方式：** 快速上手新技術 · 敏捷工作流 · 專案管理 · 研究經驗

**語言：** 中文（母語）· 英文（工作能力）
