# Customer Shopping Behavior Analytics

An end-to-end data analytics project examining 3,900 retail transactions to answer core business questions about revenue drivers, discounting, loyalty, and customer segmentation — using **Python** for data cleaning, **SQL** for analysis, and **Power BI** for visualization.

---

## 📌 The Problem

A retailer selling across four categories (Clothing, Accessories, Footwear, Outerwear) had transaction-level data sitting unused — no one had turned it into answers. Leadership had no quantified view of:

- Which customers actually drive revenue, and why
- Whether the paid subscription program was earning its keep
- Whether discounting was strategic or just being applied everywhere
- Whether loyal, repeat customers were being converted into higher-value relationships
- Whether shipping type, category, or seasonality meaningfully affected spend

Decisions on marketing spend and loyalty investment were being made without data to back them.

## 🛠️ How I Solved It

**1. Data Cleaning (Python / pandas)**
- Loaded the raw CSV (3,900 rows, 18 columns) and audited it for missing values
- Filled 37 missing `Review Rating` values using the **median rating per category** (preserves category-level rating patterns instead of pulling toward one global average)
- Standardized column names for MySQL compatibility (lowercase, underscores)
- Engineered an `age_group` feature using **quantile binning** (`pd.qcut`, 4 equal-sized customer groups: `young_adult`, `adult`, `middle_age`, `senior`)
- Engineered a `purchase_frequency_days` feature by mapping purchase-frequency labels to day counts
- Verified `discount_applied` and `promo_code_used` were 100% identical and dropped the redundant column
- Loaded the cleaned dataset into a MySQL database via SQLAlchemy

**2. Business Analysis (SQL)**
Wrote 10 targeted SQL queries against the cleaned MySQL table to answer specific business questions — using `GROUP BY`, subqueries, `CASE` statements for segmentation, and window functions (`ROW_NUMBER() OVER (PARTITION BY ...)`) for top-N-per-category analysis.

**3. Visualization (Power BI)**
Built a dashboard summarizing the key metrics for non-technical stakeholders — revenue breakdowns, segment comparisons, and product performance — so findings didn't require reading SQL to consume.

**4. Reporting**
Consolidated findings into a written business report with quantified answers to each question and concrete recommendations.

## ✅ The Solution / Key Findings

| Question | Finding |
|---|---|
| Revenue by gender | Male customers generate **2.1x** female revenue ($157.9K vs $75.2K) — driven by order volume, not higher spend per order |
| Subscription impact | Subscribers spend **no more** than non-subscribers ($59.49 vs $59.87 avg) — the program isn't currently a revenue differentiator |
| Loyalty segmentation | **~80% of customers are "Loyal"** (11+ purchases), but only 27% are subscribed — a large untapped conversion opportunity |
| Discount behavior | 839 discount-using customers still spend above average — discounting doesn't just attract bargain hunters |
| Repeat buyers vs. subscription | Purchase frequency **does not predict** subscription enrollment — the two are independent, not a natural funnel |
| Top-rated products | Ratings are tightly clustered (3.78–3.86 for the top 5) — no single standout product |
| Age group revenue | Revenue is close across all quantile-based age groups (23.9%–26.7%) |

**Headline recommendation:** the ~2,500 customers with 6+ purchases who remain unsubscribed represent the single largest, lowest-risk growth opportunity identified in this analysis. Subscription perks should also be re-evaluated, since they aren't currently driving higher spend.

Full quantified findings and recommendations are in the [Business Analysis Report](./Customer_Shopping_Behavior_Business_Report.docx). Project scope and objectives are documented in the [Business Problem Statement](./Customer_Shopping_Behavior_Business_Problem_Statement.docx).

## 🧰 Tech Stack

`Python` (pandas, NumPy) · `SQL` (MySQL) · `SQLAlchemy` · `Power BI` · `Jupyter Notebook`

## 📂 Repo Contents

- `customer_shopping_behavior.csv` — raw dataset (3,900 rows, 18 columns)
- `customer_shopping_behavior_analysis.ipynb` — data cleaning notebook (Python)
- `customer_shopping_behavior_analysis_Sql.sql` — the 10 business-question SQL queries
- `costumer_behavior_dashboard.pbix` — Power BI dashboard
- `Customer_Shopping_Behavior_Business_Problem_Statement.docx` — problem statement & scope
- `Customer_Shopping_Behavior_Business_Report.docx` — full findings & recommendations report

## ⚠️ Data Quality Note

While cleaning, I found the `purchase_frequency_days` mapping dictionary didn't match the dataset's actual label casing (e.g., `"Every 3 month"` vs. the data's `"Every 3 Months"`), silently nulling ~29% of that column. It doesn't affect any of the 10 business questions above, but it's documented as a known issue and fix in the full report.
