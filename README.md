# Cost-Aware_DRL_Risk_Control
RL-第17組期末專題
影片連結:https://youtu.be/MzEsZwPEp0o
# 基於 DRL 之動態代價感知銀行風控優化系統 🏦🤖
**Dynamic Cost-Aware Bank Risk Control Optimization using Deep Reinforcement Learning**

> **RL-第17組期末專題**
> 本專案致力於將防欺詐從「靜態二元分類」升級為「動態序列決策」。

---

## 📺 專案展示影片
[![Watch the video](https://img.shields.io/badge/YouTube-Video-red?style=for-the-badge&logo=youtube)](https://youtu.be/MzEsZwPEp0o)

---

## 📌 專案簡介
本專案解決現代銀行風控的核心痛點：**如何在「客訴代價」與「攔截效益」間取得平衡**。 傳統風控系統多將詐騙偵測視為靜態問題，容易導致正常交易被誤判（False Positive），觸發帳戶凍結並造成資金流斷裂。

我們將風控決策重構為 **「序列決策任務 (Sequential Decision Task)」**，透過深度強化學習（DRL）與監督式學習（GBDT）的混合架構，在「放行」、「人工覆核」與「凍結」之間尋求全局成本最低的最佳決策路徑。

---

## 🚀 核心技術優勢
* **混合層級架構 (Hybrid Tiered Architecture)**：結合 GBDT 對非線性特徵的高效捕捉，以及 DQN 對長期策略的優化能力。
* **代價感知設計 (Cost-Aware Decision)**：將誤封鎖代價（False Positive Cost）與人工處理成本直接納入獎勵函數（Reward Function）。
* **Leakage 防範機制**：嚴格剔除 PaySim 原始餘額特徵，改用衍生的一致性行為特徵，確保模型實戰有效性。

---

## 🛠️ 系統架構
本系統分為三個主要層級：
1.  **特徵映射層 (Feature Layer)**：進行 30 天 PaySim 數據的特徵工程。
2.  **機率估計層 (Base Scorer)**：以 XGBoost 輸出風險評分 $P_{fraud}$。
3.  **RL 策略層 (Strategic Agent)**：DQN 代理人依據風險分數與上下文狀態，從三維動作空間中選擇最優行動。

---

## 📊 決策邏輯：馬可夫決策過程 (MDP)

### 狀態 (State)
包含當前交易特徵、帳戶歷史行為一致性、對手方互動特徵、風險分數及近期決策上下文。

### 動作 (Action)
* `0`: **Approve** (放行)
* `1`: **Review** (人工覆核)
* `2`: **Freeze** (凍結)

### 獎勵函數 (Reward)
目標是最大化長期累計回報： $$R = \alpha(Detected\\_Loss) - \beta(False\\_Block\\_Penalty) - \gamma(Operational\\_Cost)$$

| 情境 | 動作 (Action) | 獎勵 (Reward) |
| :--- | :--- | :--- |
| **合法交易 (Legit)** | Approve | **+1** (正常營運)
| **合法交易 (Legit)** | Review | **-0.5** (成本支出)
| **合法交易 (Legit)** | Freeze | **-5** (誤封鎖重罰)
| **詐騙交易 (Fraud)** | Approve | **-8** (嚴重漏失損失)
| **詐騙交易 (Fraud)** | Review | **+1** (成功識別)
| **詐騙交易 (Fraud)** | Freeze | **+5** (攔截成功)

---

## 📈 預期結果
* **顯著降低誤封鎖率**：在不犧牲偵測率（AUPRC）的前提下，有效保護正常帳戶資產與資金流動性。
* **營運效能極大化**：透過動態選擇 Review 行動，優化人工覆核的工作量。

---

## 📝 快速開始

### 1. 安裝依賴
```bash
pip install -r requirements.txt
