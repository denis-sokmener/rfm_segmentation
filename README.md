# RFM Customer Segmentation — Bank Transactions

An RFM (Recency, Frequency, Monetary) analysis of ~1M bank transactions, segmenting
customers into actionable groups (Champions, Loyal Customers, Can't Lose Them, etc.)
to support retention and marketing decisions.

## Overview

This project walks through a full analytics pipeline — data cleaning, outlier handling,
RFM score calculation, and customer segmentation — on a real-world bank transaction
dataset. The goal is not just to compute RFM scores, but to validate them: every major
decision (what to drop, how to handle outliers, how to define segments) is checked
against the data rather than applied by default.

## Dataset

- **Source:** [Bank Transaction data - India](https://www.kaggle.com/datasets/apoorvwatsky/bank-transaction-data)
- **Size:** ~1,048,000 transactions, ~880,000 unique customers
- **Time span:** ~2 months (August–October 2016)
- **Key columns used:** `CustomerID`, `TransactionDate`, `TransactionAmount (INR)`

## Tools

Python · pandas · NumPy · matplotlib · seaborn

## Workflow

1. **Data Exploration** — initial inspection of structure, types, and summary statistics
2. **Data Preprocessing** — scoped null-handling (only RFM-critical columns), duplicate
   check, date parsing, dropping columns not needed for RFM (`CustomerDOB`,
   `CustAccountBalance`, `TransactionTime`)
3. **Outlier Analysis** — investigated zero-amount and extreme-value transactions;
   applied 99th-percentile capping *for visualization only* — the actual RFM
   calculation uses uncapped values, to avoid understating top customers' true value
4. **RFM Transformation** — computed Recency, Frequency, and Monetary per customer
   using vectorized `groupby().agg()`
5. **Segmentation** — scored each customer 1–5 on R and F via quintiles, then mapped
   all 25 (R, F) combinations to 10 named segments using an explicit lookup table
6. **Segment Analysis & Recommendations** — customer count, revenue share, and
   suggested action per segment

## Key Findings

| Segment | Customers | Revenue Share | Recommended Action |
|---|---|---|---|
| Champions | ~170,000 | ~26% | Reward with loyalty perks; use as referral source |
| Can't Lose Them | ~113,000 | ~13.5% | Priority win-back campaign; personal outreach |
| Loyal Customers | ~106,000 | ~13.0% | Maintain engagement with regular promotions |
| Recent Customers | ~133,000 | ~12.7% | Onboarding push to encourage a second purchase |
| Promising / Potential Loyalists | ~65,000–70,000 | ~6–7% each | Targeted campaigns to move toward Loyal/Champion |
| About to Sleep / Hibernating | ~150,000 combined | ~7% each | Low-cost re-engagement; avoid over-investing |
| At Risk / Needs Attention | ~37,000 each | ~3.5–4% each | Quick, low-cost check-in to prevent churn |

**Champions represent ~19% of customers but generate ~26% of revenue** — confirming
that the segmentation captures a real, disproportionate concentration of value rather
than an arbitrary split. **Can't Lose Them** stands out as a priority: fewer customers
than Recent Customers, but higher revenue contribution, meaning each customer in this
group is worth significantly more on average.


## How to Run

```bash
pip install pandas numpy matplotlib seaborn
jupyter notebook rfm_analysis.ipynb
```

Place `bank_transactions.csv` in a `data/` folder relative to the notebook, then
**Restart & Run All** to reproduce all outputs from scratch.
