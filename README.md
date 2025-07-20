# 💳 DeFi Wallet Credit Scoring Project

## 📌 Overview

This project delivers an end-to-end pipeline for analyzing **DeFi user wallet transactions** (from an Aave V2-like protocol), deriving **wallet-level behavioral features** from on-chain data, and assigning **interpretable credit scores** and **risk class labels** to each wallet.

The entire process is built to be **transparent, auditable, and extensible** for future improvement or new datasets.

---

## 📚 Table of Contents

1. [Problem Statement](#1-problem-statement)  
2. [Data Source & Preparation](#2-data-source--preparation)  
3. [Exploratory Data Analysis (EDA)](#3-exploratory-data-analysis-eda)  
4. [Feature Engineering](#4-feature-engineering)  
5. [Scoring Methodology](#5-scoring-methodology)  
6. [Architecture & Processing Pipeline](#6-architecture--processing-pipeline)  
7. [Output & Deliverables](#7-output--deliverables)  
8. [Extensibility](#8-extensibility)  
9. [How to Run the Project](#9-how-to-run-the-project)  

---

## 1. Problem Statement

**🎯 Objective:**  
Assign a **transparent, data-driven credit score (0–1000)** and **risk label** to each DeFi wallet based solely on historical transaction behavior.
**Goal:**  
Identify **reliable users** vs. **risky wallets**, including bots, exploiters, and non-repayers—without using any pre-labeled data.

---

## 2. Data Source & Preparation

**📦 Input Data:**  
JSON file containing raw transaction logs from a DeFi lending protocol (Aave V2 format).

**🔑 Important Fields:**
- `userWallet`
- `action` (deposit, borrow, repay, redeem, etc.)
- `rawAmount`
- `assetSymbol`
- `assetPriceUSD`

**🧹 Data Cleaning Steps:**
- Loaded and flattened nested JSON structure
- Normalized token amounts using decimals
- Removed invalid rows or blank tokens
- Ensured all numerical fields were consistent

---

## 3. Exploratory Data Analysis (EDA)

Conducted to explore user behavior, token usage, and engagement levels.

**📊 Key Insights:**
- **Token Validation:** Verified token decimals
- **Action Frequency:** Most common = deposit, repay, redeem
- **User Types:** Identified bots, high-frequency users, and passive wallets
- **Repayment Patterns:** Calculated ratios and delays
- **Segmentation:** Distinguished between passive, active, and suspicious users

---

## 4. Feature Engineering

**🧠 Wallet-Level Features:**

| Feature                | Description                                               |
|------------------------|-----------------------------------------------------------|
| `total_deposit_usd`    | Total USD value of deposits                               |
| `total_borrow_usd`     | Total USD value of borrows                                |
| `total_repay_usd`      | Total USD repaid                                          |
| `total_redeem_usd`     | Total withdrawals (redeemunderlying)                     |
| `tx_count`             | Total number of transactions                              |
| `action_*_count`       | Number of each action type                                |
| `action_types_used`    | Number of unique action types used                        |
| `repay_ratio`          | total_repay_usd / total_borrow_usd                        |
| `repay_delay_days`     | Days between first borrow and first repay                 |
| `days_active`          | Days between first and last protocol use                  |
| `unique_assets`        | Number of unique tokens interacted with                   |
| `asset_mismatch`       | Boolean flag for mismatch in borrowed vs. repaid tokens   |

---

## 5. Scoring Methodology

Since outcome labels were unavailable, a **rule-based scoring** method was implemented.

### ✅ Positive Factors
- **Full and timely repayments**
- **Quick repayment** (≤ 7 days)
- **Diverse protocol usage**
- **Sustained protocol engagement**

### 🚫 Penalty Factors
- **Late/partial/no repayments**
- **Asset mismatch**
- **Low diversity / short activity**
- **Bot-like behavior**

### 🏷️ Risk Classes

| Credit Score Range | Risk Category |
|--------------------|---------------|
| 900–1000           | Prime (AAA)   |
| 800–899            | Strong        |
| 700–799            | Moderate      |
| 600–699            | Fair          |
| 400–599            | Weak          |
| 200–399            | High Risk     |
| 0–199              | Critical      |

---

## 6. Architecture & Processing Pipeline

### 🔄 Workflow

1. **Load & Clean:**  
   - Read raw JSON  
   - Clean and standardize data

2. **Feature Calculation:**  
   - Normalize token decimals  
   - Convert amounts to USD  
   - Compute wallet-level metrics

3. **EDA & Visualization:**  
   - Repayment delays, asset mismatch, tx counts  
   - Score distribution, action diversity

4. **Scoring Engine:**  
   - Rule-based logic  
   - Assign score and class

5. **Export:**  
   - Save to `wallet_credit_scores.csv`  
   - Generate charts and reports

---

## 7. Output & Deliverables

- `wallet_credit_scores.csv`: Includes all features, credit scores, and classes
- **Score Distribution Chart:** Histogram of wallet count by score range
- `analysis.md`: Summary of wallet behaviors by credit class, plus visual insights

---

## 8. Extensibility

This system is **modular** and easy to update:

- 🔄 **Add more tokens**: Update token decimal mappings
- 🧩 **Customize rules**: Modify scoring logic as needed
- 🤖 **Plug in ML models**: Use supervised/unsupervised learning if labels become available
- 🧠 **Incorporate time series or anomaly detection** for real-time scoring

---

## 9. How to Run the Project

1. **Clone the repo** (ensure folders like `scripts/`, `notebooks/`, `data/` exist).  
2. **Install dependencies**: `pip install pandas matplotlib seaborn`  
3. **Run** the main notebook or script to generate `wallet_credit_scores.csv`.  
4. **Visualize & analyze** using the notebook or `analysis.md`.  
5. *(Optional)* Customize scoring logic, add new features, or extend for more protocols.

