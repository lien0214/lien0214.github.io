---
title: "連奕維 · Yi-Wei Lien"
---

# 連奕維 · Yi-Wei Lien

### **後端 & AI 工程師 · 台大資工碩士 · MIRLab · MSLab**

我是台灣大學資訊工程系的碩士生，同時在兩個研究室進行研究：[MIRLab](https://mirlab.org)（音樂與 AI 研究室，我在這裡研究台股量化交易策略）和 MSLab（機器發現與社交網路探勘，我的論文研究大型語言模型中的知識衝突與記憶問題）。在碩士之前，我在 Shopback 和 Cmoney 累積了約一年半的後端工程師實習經驗，接觸過真實產品、真實流量、以及真實的系統故障。

我在意兩件大多數人分開處理的事：打造**真的不會掛掉**的後端系統，以及在研究層面理解 AI。產業方面，我希望投入 AI 工程與基礎設施——打造能讓 LLM 產品在規模上穩定運行的系統。研究方面，我對 Agentic AI、RAG 系統，以及深度學習在量化交易的應用深感興趣。碩一，論文方向尚未定案。

想了解更多請看 [文章](/zh-tw/posts/)、[專案](/zh-tw/projects/)，或直接 [寄信給我](mailto:ted20030214@gmail.com)。

---

## 工作經歷

### 軟體工程師實習 — **Shopback** *(2025 年 5 月 – 11 月)*
*台北市 · 混合辦公 · TypeScript · Kubernetes · SQL · Monorepo*

Shopback 是東南亞最大的現金回饋平台，用戶數千萬。我在後端團隊涉及多個服務，從客服服務、推播系統、追蹤清單服務，到旅遊相關功能。

主要貢獻：
- **將 10+ 個跨服務 API 呼叫遷移為直接函式呼叫**，作為 monorepo 整合的一部分，於 typescipt monorepo 中實現。驅動原因是容器負載不均——各自獨立的靜態資源容器造成流量尖峰和不穩定的自動擴展，因此合併進共用的 monorepo 結構以平衡負載。副作用：每個跨服務呼叫移除網路跳躍後，延遲從 ~100ms 降至 10ms 以內。
- **將推薦系統接入搜尋頁面**，每月處理 1M+ API 請求。上線後用戶互動行為提升 +30%。
- **解決 8+ 個生產環境後端問題**，相關 API 錯誤率降低 10–15%。多數是降噪工作——對已知的良性模式降低告警等級，並重構周邊的錯誤處理流程。其中兩件涉及第三方 API 不確定行為的調查與處理邏輯更新。

---

### 後端開發工程師實習 — **Cmoney** *(2024 年 7 月 – 2025 年 4 月)*
*新北市 · 混合辦公 · C# · ASP.NET · MongoDB · 微服務*

Cmoney 是台灣最大的金融資訊平台，App 下載量超過 700 萬。我在核心後端積極進行微服務重構期間加入。

主要貢獻：
- **為旗艦產品重構 9 個後端 API 的微服務架構**：從單體架構中抽取共用業務邏輯、定義清晰的服務介面
- 在 C# 和 ASP.NET 環境下搭配 MongoDB 工作，深入理解真實用戶在市場開盤時讀取金融資料時的高並發邊緣案例
- 從艱難處學到 OOP 架構模式——不是從課本，而是從繼承那些沒有使用這些模式的程式碼

> 重構別人的生產系統是我接受過最好的軟體架構教育。當測試全綠但程式碼無法維護時，你才真正理解「好設計」能帶來什麼。

---

### 研究助理（大學部）— **中研院，Citi Lab** *(2023 年 9 月 – 2024 年 6 月)*
*南港 · 遠端 · Python · 資安 · 合成資料 · GAN*

在中研院資創中心 Citi 實驗室工作。研究目標是透過生成逼真的合成 Linux audit log 來擴增 GAN 入侵偵測模型的訓練資料。

主要貢獻：
- **建立處理 2 億筆以上 Linux audit log 的資料 Pipeline**，將原始 audit 資料解析、正規化，並組織成適合序列到序列 GAN 訓練的格式
- 探索資安合成資料品質的挑戰：能生成「真實」log 的 GAN，只有在合成攻擊能騙過以真實資料訓練的偵測器時才有意義
- 第一次真正體會學術 Benchmark 與資安 ML 實務落差有多大

---

### 教學助理 — **台大資工系，資料結構與演算法** *(2023 年 2 月 – 6 月)*

主持每週實驗課、辦公室時間答疑、批改作業和考試。學到了一件事：把一件事解釋清楚比自己理解它難多了。

---

## 研究與專案

### ICC Mem/Gen — LLM 中的知識衝突 *(2026 年 3 月 – 6 月)*
*MSLab · 4 人團隊 · LoRA · PEFT · Activation Steering*

**核心問題**：當語言模型被微調後相信 X，但預訓練時學到的是 Y，哪個會贏？能否在不重新訓練的情況下控制這個結果？

- 為 ConflictQA 和 ConflictBank 建立**跨格式資料增強 Pipeline**，生成語義等效的問題變體，測試模型在換句話說和格式轉換後的知識衝突處理能力
- 使用 **Activation Steering** 從 LoRA 微調模型中提取行為向量，並應用於凍結的基礎模型。關鍵發現：行為方向的可遷移性出乎意料地強健——從 LoRA 訓練模型提取的 Steering Vector 即使不更新任何權重，也能將基礎模型的輸出推向預期方向
- 更廣泛的意涵：微調在殘差流中注入了方向性偏置，且這種偏置在某種程度上可以和任務知識分離

*論文審閱中，NeurIPS 2026。*

---

### 量化交易演算法 — 台股日策略 *(2026 年 1 月 – 6 月)*
*MIRLab · 3 人團隊 · Python · TWSE · 量化*

為台灣證券交易所開發系統化日交易策略，包含訊號生成、回測基礎設施和紙交易評估。

- 研究日 OHLCV 資料上的動量和均值回歸訊號。台股是散戶主導的市場，微觀結構與美股有顯著差異——許多西方量化文獻的規律無法直接移植
- 目前正在以 ML 進行訊號組合

---

### Bubblo — 2D 平台遊戲 Demo *(2025 年 4 月 – 6 月)*
*6 人團隊 · 專案負責人 · Unity · C# · OOP · 設計模式 · 600+ commits*

帶領 6 人團隊在一學期內用 Unity 完成完整的 2D 平台遊戲 Demo，累計 600+ commits。

- 在任何遊戲程式碼開始之前就設計好元件架構，明確採用 Observer、State Machine 和 Command 模式——因為 6 個人共用同一個 codebase，隱含耦合會直接殺死開發速度
- 最終 Demo 穩定跑在 60fps，有 5 個可玩關卡、完整音效、以及完整的遊戲循環（生命值與計分）

🎮 [在瀏覽器玩](/zh-tw/game/)

---

### E-MoTchi — Emoji 轉中文 NLP *(2024 年 11 月 – 12 月)*
*5 人團隊 · Taiwan-LLaMA · Google mT5 · LoRA · RLHF*

針對 Emoji 序列轉換為自然中文表達的任務，對多語言 LLM 進行微調。從頭設計訓練資料集。

- 透過群眾外包收集 emoji 對應中文的映射，再進行規則增強。核心挑戰：emoji 意義高度依賴情境，因平台、年齡層和台灣網路文化而異
- 使用 LoRA 和 RLHF 風格的偏好優化微調 **Taiwan-LLaMA** 和 **Google mT5**；mT5 因其明確的多語言預訓練而表現優於 LLaMA
- 最有趣的結果：BLEU 分數高的模型在「聽起來自然」的人工評估上表現明顯更差——評估指標偏離目標的典型案例

---

## 技術能力

**語言：** `TypeScript` `Python` `C#` `SQL` `C++` `Vibe-Coding`

**後端 & 基礎設施：** `ASP.NET Core` `Node.js` `Kubernetes` `Docker` `MongoDB` `PostgreSQL` `Redis` `Microservices` `REST APIs` `CI/CD` `Agentic Workflow`

**ML & AI：** `PyTorch` `Transformers (HuggingFace)` `LoRA / PEFT` `Activation Steering` `RAG` `RLHF` `Fine-tuning` `Taiwan-LLaMA` `mT5` `Optimization`

**工具：** `Git` `GitHub Actions` `Unity` `Linux` `Vim` `Grafana` `Datadog`

---

## 聯絡我

歡迎研究合作、工作洽談（目標 AI 工程師職位，預計 2027 年畢業），或單純想聊 LLM 和量化金融。

**Email**: [ted20030214@gmail.com](mailto:ted20030214@gmail.com)
**GitHub**: [github.com/lien0214](https://github.com/lien0214)
**LinkedIn**: [linkedin.com/in/yi-wei-lien](https://www.linkedin.com/in/yi-wei-lien/)
