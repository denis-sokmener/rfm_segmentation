# RFM Customer Segmentation on Banking Transactions

An end-to-end customer segmentation project using **RFM (Recency, Frequency, Monetary)** analysis on over **1 million real-world banking transactions**. The project identifies customer groups with different purchasing behaviors and translates analytical findings into actionable business insights.

---

# Project Overview

This project demonstrates a complete customer analytics workflow, including:

- Data cleaning
- Exploratory Data Analysis (EDA)
- Outlier analysis
- Feature engineering
- RFM score calculation
- Customer segmentation
- Segment profiling
- Business recommendations

Instead of blindly applying common RFM techniques, each preprocessing and modeling decision is validated against the characteristics of the dataset.

---

# Dataset

**Source**

https://www.kaggle.com/datasets/apoorvwatsky/bank-transaction-data

**Dataset Summary**

- ~1,048,000 transactions
- ~880,000 customers
- August – October 2016
- Observation period: approximately 2 months

Columns used:

- CustomerID
- TransactionDate
- TransactionAmount (INR)

---

# Technologies

- Python
- pandas
- NumPy
- matplotlib
- seaborn

---

# Workflow

## 1. Exploratory Data Analysis

Performed:

- dataset inspection
- data type validation
- missing value analysis
- duplicate detection
- summary statistics
- distribution analysis
- transaction trend analysis
- outlier detection

### RFM Distributions

<img src="output/rfm_distributions.png" width="1000">

The dataset exhibits:

- Recency values concentrated between 35–80 days.
- Extremely low purchase frequency (most customers made only one transaction).
- A highly right-skewed Monetary distribution.

This immediately suggests that Frequency contains limited information due to the short observation period.

---

## 2. Outlier Analysis

Transaction amounts contain a small number of extremely large values.

<img src="output/boxplot.png" width="450">

The distribution is heavily right-skewed with several very large transactions.

To improve interpretability, a **99th percentile cap** was applied for visualization purposes.

<img src="output/capping_histogram.png" width="900">

The capped histogram preserves the overall distribution while making the central mass easier to inspect.

> **Important:** The capped dataset is **NOT** used for RFM calculations. Original transaction values are preserved to avoid understating high-value customers.

---

## 3. Transaction Trend

Daily transaction amounts were analyzed before and after capping.

### Original Values

<img src="output/transaction_over_time2.png" width="850">

### After Capping

<img src="output/transaction_over_time_after.png" width="850">

Capping reduces only extreme spikes while preserving the underlying transaction trend.

---

## 4. Feature Engineering

Customer-level RFM metrics were calculated using vectorized pandas operations.

- **Recency** → Days since last transaction
- **Frequency** → Number of transactions
- **Monetary** → Total amount spent

A snapshot date was defined as one day after the latest transaction.

---

## 5. Customer Segmentation

Customers were assigned:

- Recency Score (1–5)
- Frequency Score (1–5)

using quintiles.

The resulting 25 score combinations were mapped into ten business-oriented customer segments.

Segments include:

- Champions
- Loyal Customers
- Can't Lose Them
- Recent Customers
- Potential Loyalists
- Promising
- About to Sleep
- At Risk
- Needs Attention
- Hibernating

---

# Segment Distribution

<img src="output/customers_per_segment.png" width="800">

Champions represent the largest customer group, followed by Recent Customers and Can't Lose Them.

---

# Revenue Distribution

<img src="output/share_by_segment.png" width="800">

Although Champions account for only around one-fifth of customers, they generate over one-quarter of total revenue.

This indicates that the segmentation captures meaningful differences in customer value.

---

# Segment Profiles

<img src="output/segment_profiles_dashboard.png" width="1100">

Average RFM values reveal clear behavioral differences between customer groups.

Key observations:

- Champions have both the highest spending and highest purchase frequency.
- Can't Lose Them customers spend heavily but have become inactive.
- Recent Customers show strong recency but relatively low frequency.
- At Risk and Hibernating customers have the weakest recency.

---

# RFM Relationships

<img src="output/rfm_pairplot.png" width="1100">

The pairplot illustrates how customer segments separate across Recency, Frequency, and Monetary dimensions.

Although Monetary was not directly used for segmentation, spending naturally increases among customers with stronger Recency and Frequency characteristics.

---

# Monetary Validation

<img src="output/avg_monetary_by_r_and_f.png" width="500">

Monetary was intentionally excluded from the segmentation rules.

Instead, it was used as an independent validation metric.

The heatmap shows:

- Monetary remains relatively stable across Frequency scores 1–4.
- Customers with Frequency Score = 5 spend substantially more.
- The highest average Monetary value belongs to customers with both high Recency and high Frequency (Champions).

This validates that the segmentation successfully identifies economically valuable customers without directly using Monetary for segment assignment.

---

# Key Business Insights

- Champions generate the largest share of total revenue.
- Can't Lose Them customers represent the highest-priority retention opportunity.
- Most customers completed only a single transaction.
- Monetary value increases sharply only among the most active customers.
- Recent customers represent an excellent opportunity for conversion into loyal customers.

---

# Limitations

- Observation window covers only approximately two months.
- Frequency has only a few distinct values, limiting its discriminative power.
- Segmentation is based only on Recency and Frequency.
- Monetary is used for validation rather than segmentation.

---

# Project Structure

```
.
├── data/
│   └── bank_transactions.csv
│
├── output/
│   ├── avg_monetary_by_r_and_f.png
│   ├── boxplot.png
│   ├── capping_histogram.png
│   ├── customers_per_segment.png
│   ├── rfm_distributions.png
│   ├── rfm_pairplot.png
│   ├── segment_profiles_dashboard.png
│   ├── share_by_segment.png
│   ├── transaction_over_time2.png
│   └── transaction_over_time_after.png
│
├── rfm_analysis.ipynb
├── requirements.txt
└── README.md
```

---

# Skills Demonstrated

- Data Cleaning
- Exploratory Data Analysis
- Feature Engineering
- Customer Analytics
- RFM Segmentation
- Business Analytics
- Data Visualization
- pandas
- NumPy
- matplotlib
- seaborn

---

# Future Improvements

- Customer Lifetime Value (CLV)
- Churn Prediction
- Interactive Streamlit Dashboard
- Campaign Performance Analysis
- Marketing ROI Evaluation

---

# Conclusion

This project demonstrates an end-to-end customer analytics workflow that combines technical implementation with business interpretation to transform raw banking transaction data into actionable customer segments.
