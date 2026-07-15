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

### 1. Data Exploration
Initial inspection of structure, types, and summary statistics.

### 2. Data Preprocessing
Scoped null-handling (only RFM-critical columns), duplicate check, date parsing,
dropping columns not needed for RFM (`CustomerDOB`, `CustAccountBalance`,
`TransactionTime`).

### 3. Outlier Analysis
Investigated zero-amount and extreme-value transactions in `TransactionAmount (INR)`.

<img src="output/boxplot.png" width="450"/>

The boxplot shows the interquartile range compressed near zero, with a long tail of
extreme values reaching up to 1.56M INR — confirming a heavily right-skewed
distribution.

<img src="output/capping_histogram.png" width="750"/>

99th-percentile capping was applied **for visualization and distribution analysis
only**. The overall shape is preserved after capping — only the extreme tail is
pulled in — and the visible spike at 20,000 INR in the "After Capping" chart is the
direct signature of that operation. **The actual RFM calculation uses the original,
uncapped values**, to avoid understating top customers' true value.

<img src="output/transaction_over_time2.png" width="750"/>

*Before capping.*

<img src="output/transaction_over_time_after.png" width="750"/>

*After capping.* The overall trend and timing of peaks/dips is identical between the
two — capping mainly reduces the magnitude of the highest spikes (e.g., ~47M → ~40M
INR on the largest day) without distorting the underlying pattern.

### 4. RFM Transformation
Computed Recency, Frequency, and Monetary per customer using vectorized
`groupby().agg()` 

### 5. Segmentation
Scored each customer 1–5 on R and F via quintiles, then mapped all 25 (R, F)
combinations to 10 named segments using an explicit lookup table 
### 6. Segment Analysis & Recommendations
Customer count, revenue share, and suggested action per segment.

## Key Findings

<img src="output/customers_per_segment.png" width="700"/>

<img src="output/share_by_segment.png" width="700"/>

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

<img src="output/avg_monetary_by_r_and_f.png" width="500"/>

This heatmap validates the segmentation choice: average Monetary value stays flat
(~1500–1650 INR) across F_Score 1–4 regardless of Recency, and only jumps sharply
(2400–3200 INR) at F_Score = 5 — with the top-right cell (R=5, F=5) reaching the
highest value overall. This confirms that customers scored as "Champions" (high R and
F) are genuinely the highest-value group, even though Monetary itself isn't used to
define the segments directly.

## Limitations

- **Frequency has limited discriminative power.** The dataset spans only ~2 months, so
  the vast majority of customers made just 1–2 transactions (Frequency has only 6
  distinct values dataset-wide). This caps how much genuine repeat-purchase behavior
  the Frequency score can capture — segments described as "high frequency" should be
  interpreted with this in mind. A longer observation window would likely produce a
  more discriminating Frequency signal.

- **Segments are defined on R and F only** (a common RFM convention, since a full
  R×F×M grid produces 125 hard-to-label combinations). Monetary is used afterward to
  validate segment quality rather than to define segments directly — in this dataset,
  the top R+F segment does show the highest average Monetary value (see heatmap
  above), supporting this simplification, but it isn't guaranteed to hold on other
  datasets.

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook rfm_analysis.ipynb
```

Place `bank_transactions.csv` in a `data/` folder relative to the notebook, then
**Restart & Run All** to reproduce all outputs from scratch.

## Possible Next Steps

- Interactive dashboard (Streamlit) for exploring segments by filter
- Churn prediction model using RFM scores as features
- A/B testing framework to measure the impact of segment-targeted campaigns
