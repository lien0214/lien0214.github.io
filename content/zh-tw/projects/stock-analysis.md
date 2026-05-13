---
title: "量化交易演算法 — 台股"
description: "台灣證券交易所的系統化日交易策略：訊號生成、回測、紙交易評估，以及支援多券商的執行 Dashboard。"
date: 2026-01-01
period: "2026 年 1 月 – 6 月"
team: "MIRLab · 3 人團隊"
category: "Research"
github: "https://github.com/ntumirlab/stock-analysis"
stack: ["Python", "TWSE", "量化", "FinLab", "Docker", "Dash", "Flask"]
---

## 系統架構

基於 FinLab 即時金融資料的完整交易系統，涵蓋從策略開發到自動執行的完整 Pipeline。

**策略層**：在日 OHLCV 資料上的動量和均值回歸訊號。台股是散戶主導的市場，微觀結構與美股有顯著差異——許多西方量化文獻的規律無法直接移植，因此我們研究台灣特有的訊號。

**執行層**：透過 crontab 自動化。三個排程工作分別處理帳戶資料抓取、獨立的回測報告，以及透過元大 Fugle API 和永豐 Shioaji API 的下單執行。透過 `config.yaml` 支援多用戶和多券商。

**Dashboard**：Web UI（Dash + Bootstrap + Gunicorn）顯示交易資訊、帳戶餘額圖表和月報酬率視覺化。以 Docker Compose 部署。

**目前**：正在評估以 ML 進行訊號組合。紙交易持續運行中。

## 技術棧

FinLab · shioaji · ta-lib · pandas · Dash · Flask · Docker

*GitHub 9 顆星 · v3.1.0 發布於 2026 年 1 月*
