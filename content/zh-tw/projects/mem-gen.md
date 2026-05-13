---
title: "ICC Mem/Gen"
description: "當模型微調後相信 X，但預訓練說 Y——哪個會贏？能否在不重新訓練的情況下控制這個結果？"
date: 2026-03-01
period: "2026 年 3 月 – 6 月"
team: "MSLab · 4 人團隊"
category: "Research"
status: "NeurIPS 2026 審閱中"
github: "https://github.com/ntumslab/mem_gen_following"
stack: ["LLM", "LoRA", "PEFT", "Activation Steering", "Python"]
---

## 核心問題

當語言模型被微調後相信 X，但預訓練時學到的是 Y——哪個會贏？更重要的是，能否在不重新訓練的情況下控制這個行為？

## 我們做了什麼

**跨格式資料增強 Pipeline**：針對 ConflictQA 和 ConflictBank，生成語義等效的問題變體，用以測試模型在換句話說和格式轉換後對知識衝突的處理能力。這對於打造不過擬合到表面形式的 Benchmark 至關重要。

**Activation Steering 實驗**：從 LoRA 微調模型中提取行為向量，並應用於凍結的基礎模型。行為方向的可遷移性出乎意料地強健——從 LoRA 訓練模型提取的 Steering Vector 即使不更新任何權重，也能將基礎模型的輸出推向預期方向。

## 關鍵發現

微調在**殘差流中注入了方向性偏置**，且這種偏置在某種程度上可以和任務知識分離。如果你可以不更新權重就控制行為，那你也有可能*撤銷*微調訊號。這對 Alignment 研究有直接意涵。

## 狀態

論文審閱中——NeurIPS 2026。
