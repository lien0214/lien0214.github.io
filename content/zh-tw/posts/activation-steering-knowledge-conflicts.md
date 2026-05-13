---
title: "在微調模型間的 Activation Steering：ICC Mem/Gen 的發現"
date: 2026-05-10
description: "能從 LoRA 微調模型中提取行為方向，並應用到凍結的基礎模型嗎？可以——而且可遷移性出乎意料地強健。ICC Mem/Gen 研究的筆記。"
tags:
  - 研究
  - LLM
  - activation-steering
  - LoRA
  - knowledge-conflict
  - MSLab
categories:
  - Research Notes
showToc: true
---

## 背景

MSLab 的 ICC Mem/Gen 計畫始於一個看似簡單的問題：**當語言模型的微調和預訓練發生分歧時，它「相信」什麼？**

直接的實務動機是 ConflictQA 和 ConflictBank——專門設計來浮現模型在訓練於某組事實後，遇到探測衝突事實問題時的行為的 Benchmark。在這些 Benchmark 上進行標準微調，會得到一個在訓練分布上表現良好，但在改寫或格式轉換的變體上行為不一致的模型。

我們的角度不同。我們不只是想衡量衝突，而是想*操控*行為——更具體地說，理解微調引入的方向性訊號是否可以和它所攜帶的任務知識分離。

---

## 核心想法：行為向量作為可遷移的方向

概念框架來自 *Representation Engineering*（Zou et al., 2023）。核心主張：某些高層次行為（誠實、毒性、諂媚）對應於激活空間中的方向，這些方向在不同層和情境中相對穩定。你可以透過對比引發該行為的刺激和不引發的刺激的激活值來找到這些方向，然後把這些方向用作 Steering Vector。

我們把這個框架應用到微調的情境：

1. 取基礎模型 $M_{\text{base}}$ 和一個在 ConflictQA 某個切分上以 LoRA 微調的變體 $M_{\text{ft}}$
2. 對一組會引發衝突的 Prompt，從兩個模型的第 $L$ 層收集殘差流的激活值
3. 計算均值差：$\vec{v} = \mathbb{E}[h_L^{\text{ft}}(x)] - \mathbb{E}[h_L^{\text{base}}(x)]$
4. 在推理時將 $\vec{v}$ 應用於基礎模型的前向傳播：$h_L' = h_L + \alpha \cdot \hat{v}$

問題是：這是否能讓基礎模型在衝突問題上的行為更像微調模型——**在不更新任何權重的情況下**。

---

## 我們的發現

簡短答案：可以，而且比我們預期的更有效。

**遷移依賴於層。** Steering Vector 在中到後段的層（我們測試的模型深度約 40–70%）提取和應用時最有效。早期的層似乎承載更多語法/位置資訊；後期的層太靠近詞彙投影而難以泛化。中間層是行為方向所在之處。

**方向對改寫具有魯棒性。** 我們在一組改寫上計算 Steering Vector，並在同一衝突的持出改寫上測試。遷移效果保持良好。這才是非顯而易見的部分——如果向量在捕捉特定 Prompt 的表面特徵，你會預期在改寫上效果退化。魯棒性表明這個方向編碼了更語義性的東西。

**$\alpha$ 的尺度比我們預期的更重要。** $\alpha$ 太小看不到效果。$\alpha$ 太大模型輸出不連貫（激活空間被推到未探索的區域）。有大約 3 倍的 $\alpha$ 範圍效果是乾淨的。在沒有持出集的情況下校準這個值很麻煩。

**不同衝突類型的反應不同。** 遷移在事實衝突（衝突的命名實體關聯）上效果最好。在推理衝突上效果較差——微調模型學到了不同的推理模式，而不只是不同的事實。這是有啟示性的：事實知識在激活空間中可能比推理策略更局部化。

---

## 資料增強的部分

Steering 實驗需要一個受控的資料集。我們在 ConflictQA 和 ConflictBank 上建立了跨格式增強 Pipeline：

- **改寫擴展**：對每個 QA 對，使用獨立的 LLM 生成 4–6 個語義等效的改寫（我們用 Claude Sonnet 作為改寫器）。用 BLEURT 分數過濾以確保語義等效但表面不重疊。
- **格式轉換**：把問題改格式為填空、多選和開放式，測試衝突處理是否在不同格式間遷移。
- **衝突類型標記**：手動將每個衝突分類為事實-實體、事實-屬性、推理或常識。這對於理解上述差異化的遷移結果是必要的。

Pipeline 生成的資料量約為基礎 Benchmark 的 6 倍，大幅提升了評估的魯棒性。

---

## 開放問題

研究期間我們沒有解決，但一直在思考的幾件事：

**遷移是因果的還是相關的？** 我們的實驗顯示應用向量會改變輸出行為。但這不能告訴我們這個向量是否在以和微調相同的方式*導致*行為改變。也許我們只是把激活值推進一個碰巧產生相似輸出的區域。這個區別對可解釋性的主張很重要。

**這能跨模型族泛化嗎？** 我們只在一個模型族（特定的開放權重 LLM 系列）內測試。Representation Engineering 文獻有一些跨模型的證據，但大多在同族內。跨架構遷移會更有趣，也更困難。

**安全性意涵是什麼？** 如果行為 Steering Vector 可以從微調模型遷移到基礎模型，這潛在地意味著你可以從安全調整的模型中提取「對齊方向」，然後嘗試在基礎模型上反轉它。我們思考過這個問題。實際的障礙是對齊相關行為似乎需要更大的 $\alpha$（它們更深層地嵌入），所以你需要更用力地推動模型，品質退化可能是不可接受的。但這值得仔細思考。

---

## 參考文獻

- Zou, A., et al. (2023). [Representation Engineering: A Top-Down Approach to AI Transparency](https://arxiv.org/abs/2310.01405)
- Xie, S., et al. (2023). [Adaptive Chameleon or Stubborn Sloth: Revealing the Behavior of Large Language Models in Knowledge Conflicts](https://arxiv.org/abs/2305.13300)
- Hu, E., et al. (2021). [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)
