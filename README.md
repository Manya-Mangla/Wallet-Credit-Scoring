DeFi Wallet Credit Scoring
Overview
This project calculates a credit score for each user wallet based on their historical DeFi lending protocol activity. The scoring is completely data-driven, using features like borrow/repay behavior, speed of repayment, protocol usage diversity, and other transaction patterns. The main goal is to flag which wallets are most trustworthy and which may pose credit or protocol risk.

Data Source
JSON file of DeFi protocol transactions (Aave v2 format).

Key data fields: wallet address, timestamp, action (deposit, borrow, repay, etc.), token symbol, amount, and USD price.

Processing & Modeling Pipeline
Data Loading & Cleaning

Flatten the nested JSON to tabular format.

Standardize token amounts, correcting for decimals.

Remove malformed, incomplete, or blank asset transactions.

Feature Engineering

Aggregate wallet-level totals: deposits, borrows, repays, redeems.

Compute behavioral metrics:

Repayment ratio (total repaid / total borrowed)

Repayment delay (days from first borrow to first repay)

Number of each action type

Number of unique tokens interacted with

Transaction count and protocol usage diversity

Asset mismatch indicator (if borrowed and repaid with different tokens)

Activity span (days active on protocol)

Credit Scoring

Start all wallets at 1000 points.

Subtract points for late, partial, or non-repayment, asset mismatch, or bot-like behaviors.

Add points for timely/full repayments, usage diversity, or long activity span.

Final scores are between 0—1000, then mapped to classes:

Prime (AAA): 900–1000

Strong: 800–899

Moderate: 700–799

Fair: 600–699

Weak: 400–599

High Risk: 200–399

Critical: 0–199

Export & Reporting

Results saved to wallet_credit_scores.csv with all features, final score, and label.

Visualizations summarize score distribution and behavior by score class.

How to Use
Place your input transaction file in the data/ directory.

Run the main notebook or script (wallet_credit_score.py).

Review the output CSV and use the provided visualization code for analysis.

Customization & Extensions
You can adjust score logic or asset decimal mappings as needed.

For improved accuracy, the model can be extended with supervised machine learning (if you have labeled outcomes), anomaly detection, or time-series features typical in industry deployment.
