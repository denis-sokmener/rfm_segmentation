# RFM Customer Segmentation — Bank Transactions

An end-to-end customer segmentation project using **RFM (Recency, Frequency, Monetary)** analysis on over **1 million real-world bank transactions**. The project identifies customer segments with different purchasing behaviors and translates analytical findings into actionable business recommendations.

---

# Overview

Customer segmentation is one of the most widely used techniques in CRM and marketing analytics. In this project, I perform a complete RFM analysis on a large-scale banking transaction dataset to identify meaningful customer groups based on their purchasing behavior.

The project covers the entire analytics workflow—from data cleaning and exploratory data analysis to feature engineering, RFM scoring, customer segmentation, visualization, and business interpretation.

Rather than applying standard preprocessing techniques blindly, each decision (missing values, outlier handling, scoring strategy, and segmentation logic) is evaluated against the characteristics of the dataset before being implemented.

---

# Dataset

- **Source:** https://www.kaggle.com/datasets/apoorvwatsky/bank-transaction-data
- **Transactions:** ~1,048,000
- **Unique Customers:** ~880,000
- **Observation Period:** August–October 2016 (~2 months)

### Columns Used

- `CustomerID`
- `TransactionDate`
- `TransactionAmount (INR)`

> **Note:** Since the observation period covers only about two months, customer purchase frequency is naturally limited. As a result, **Recency** becomes the strongest behavioral indicator while **Frequency** has relatively low variability.

---

# Tools

- Python
- pandas
- NumPy
- matplotlib
- seaborn

---

# Project Workflow

## 1. Exploratory Data Analysis

Performed an initial inspection of the dataset including:

- Dataset structure
- Data types
- Summary statistics
- Missing values
- Duplicate records
- Distribution analysis
- Transaction trends over time
- Outlier inspection

---

## 2. Data Preprocessing

Performed data cleaning specifically for RFM analysis.

- Removed rows with missing values only in RFM-critical columns
- Converted transaction dates to datetime format
- Checked for duplicate transactions
- Removed columns not required for customer segmentation:
  - CustomerDOB
  - CustAccountBalance
  - TransactionTime

---

## 3. Outlier Analysis

Transaction amounts were highly right-skewed with several extremely large transactions.

<img src="output/boxplot.png" width="450"/>

The boxplot reveals a compressed interquartile range near zero and a long right tail extending up to approximately **1.56 million INR**.

To better visualize the overall distribution, **99th-percentile capping** was applied.

<img src="output/capping_histogram.png" width="750"/>

The capped distribution preserves the overall shape while making the central distribution easier to interpret.

> **Important:** Outlier capping was performed **only for visualization purposes.** The actual RFM calculations use the original transaction amounts to preserve customers' true monetary value and avoid understating high-value customers.

Transaction trends before and after capping:

<img src="output/transaction_over_time2.png" width="750"/>

*Before capping*

<img src="output/transaction_over_time_after.png" width="750"/>

*After capping*

Although the largest daily spikes become smaller after capping, the underlying transaction pattern remains unchanged.

---

## 4. Feature Engineering (RFM)

Customer-level RFM metrics were calculated using vectorized pandas operations.

- **Recency:** Days since customer's most recent transaction
- **Frequency:** Total number of transactions
- **Monetary:** Total transaction amount

A **snapshot date** was defined as one day after the latest transaction to ensure consistent Recency calculation across all customers.

---

## 5. Customer Segmentation

Customers received **Recency** and **Frequency** scores ranging from **1 to 5** using quintiles.

Since the dataset covers only a short observation period and Frequency contains limited variation, customer segments were intentionally created using **Recency and Frequency only**.

The resulting 25 score combinations were mapped into 10 business-oriented customer segments through an explicit lookup table.

Monetary was then used as an independent validation metric rather than as a segmentation variable.

---

## 6. Segment Analysis

The resulting segments were analyzed using:

- Customer count
- Revenue contribution
- Average Monetary value
- Recommended business actions

---

# Key Findings

<img src="output/customers_per_segment.png" width="700"/>

<img src="output/share_by_segment.png" width="700"/>

| Segment | Customers | Revenue Share | Recommended Action |
|---|---:|---:|---|
| Champions | ~170K | ~26% | Loyalty rewards, referrals, VIP campaigns |
| Can't Lose Them | ~113K | ~13.5% | Immediate retention campaign |
| Loyal Customers | ~106K | ~13% | Personalized promotions |
| Recent Customers | ~133K | ~12.7% | Encourage second purchase |
| Potential Loyalists | ~65–70K | ~6–7% | Increase engagement |
| Promising | ~65–70K | ~6–7% | Nurture with targeted offers |
| About to Sleep | ~75K | ~3–4% | Reminder campaigns |
| Needs Attention | ~37K | ~3.5–4% | Re-engagement offers |
| At Risk | ~37K | ~4% | Win-back strategy |
| Hibernating | ~75K | ~3–4% | Low-cost reactivation |

---

## Business Insights

- **Champions represent only about one-fifth of customers but generate more than one-quarter of total revenue.**
- **Can't Lose Them** customers contribute significantly more revenue than many larger segments, making them the highest-priority retention group.
- Recent and highly active customers consistently generate the greatest business value.
- Segment-level revenue differences suggest that the RFM segmentation successfully identifies economically meaningful customer groups.

---

<img src="output/avg_monetary_by_r_and_f.png" width="500"/>

Although **Monetary** was not used to define customer segments, the heatmap demonstrates that average spending increases substantially among customers with the highest Frequency scores.

The highest average Monetary value appears in the **R=5, F=5** group (Champions), providing independent validation that the segmentation successfully captures high-value customers.

---

# Limitations

- The dataset covers only about **two months**, limiting the ability to observe long-term purchasing behavior.
- Frequency has only a small number of distinct values, reducing its discriminative power.
- Customer segments are based only on **Recency** and **Frequency**. Monetary is used afterward to validate segmentation quality rather than define it directly.
- Results may differ when applied to datasets with longer observation periods.

---

# Project Structure

```
.
├── data/
│   └── bank_transactions.csv
├── output/
│   ├── boxplot.png
│   ├── capping_histogram.png
│   ├── customers_per_segment.png
│   ├── share_by_segment.png
│   ├── avg_monetary_by_r_and_f.png
│   ├── transaction_over_time2.png
│   └── transaction_over_time_after.png
├── rfm_analysis.ipynb
├── requirements.txt
└── README.md
```

---

# Skills Demonstrated

- Data Cleaning
- Exploratory Data Analysis (EDA)
- Feature Engineering
- Customer Analytics
- RFM Segmentation
- Data Visualization
- Business Insight Generation
- pandas
- NumPy
- matplotlib
- seaborn

---

# How to Run

```bash
pip install -r requirements.txt
jupyter notebook rfm_analysis.ipynb
```

Place `bank_transactions.csv` inside the `data/` directory and run the notebook from top to bottom to reproduce all outputs.

---

# Possible Next Steps

- Interactive Streamlit dashboard
- Churn prediction using RFM features
- Customer Lifetime Value (CLV) estimation
- Campaign performance analysis
- A/B testing framework for segment-based marketing

---

# Conclusion

This project demonstrates a complete customer analytics workflow, combining data cleaning, feature engineering, customer segmentation, visualization, and business interpretation to transform raw transaction data into actionable insights.
