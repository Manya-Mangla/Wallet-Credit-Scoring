# 🏦 DeFi Wallet Credit Scoring

## 🔍 Overview

This project calculates a **credit score for each user wallet** based on their historical activity on a DeFi lending protocol (Aave v2).  
The scoring is **completely data-driven**, using behavior such as:

- Borrow/repay history
- Speed of repayment
- Protocol usage diversity
- Other transaction patterns

### 🎯 Goal
To **flag wallets** based on risk:
- Identify trustworthy participants
- Detect wallets that may pose **credit** or **protocol risk**

---

## 📁 Data Source

- **Format:** JSON (Aave v2 protocol transaction history)
- **Key fields:**
  - `wallet_address`
  - `timestamp`
  - `action` (e.g., deposit, borrow, repay, redeem)
  - `token_symbol`
  - `amount`
  - `usd_price`

---

## 🛠️ Processing & Modeling Pipeline

### 1. 🧼 Data Loading & Cleaning
- Flatten nested JSON into a tabular format
- Standardize token amounts (correct for decimals)
- Remove malformed, incomplete, or blank transactions

### 2. 🧮 Feature Engineering
- **Aggregate wallet-level totals:**
  - Deposits, Borrows, Repays, Redeems
- **Behavioral metrics:**
  - Repayment ratio = total repaid / total borrowed
  - Repayment delay = days between first borrow and first repay
  - Number of each action type
  - Number of unique tokens interacted with
  - Total transaction count
  - Protocol usage diversity
  - Asset mismatch indicator (borrowed vs repaid tokens)
  - Activity span = total active days on protocol

### 3. 📊 Credit Scoring
- All wallets start at **1000 points**
- **Point deductions for:**
  - Late/partial/non-repayments
  - Asset mismatch
  - Bot-like behavior
- **Point additions for:**
  - Timely & full repayments
  - Long activity span
  - High token/protocol usage diversity

#### 🧷 Final Score Ranges
| Score Range | Credit Class |
|-------------|--------------|
| 900–1000    | Prime (AAA)  |
| 800–899     | Strong       |
| 700–799     | Moderate     |
| 600–699     | Fair         |
| 400–599     | Weak         |
| 200–399     | High Risk    |
| 0–199       | Critical     |

---

## 📤 Export & Reporting

- Results saved to: `wallet_credit_scores.csv`
  - Includes all features, final credit score, and classification
- Visualizations provided to:
  - Analyze score distribution
  - Understand wallet behavior by credit class

---

## 🚀 How to Use

1. Place your input transaction file in the `data/` directory.
2. Run the main script or notebook:
   ```bash
   python wallet_credit_score.py
