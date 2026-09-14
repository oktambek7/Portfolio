# 🛍️ Customer Shopping Behavior Analysis

A complete end-to-end data analytics project analyzing shopping patterns, customer segments, and purchase behavior across 3,900 transactions — from raw data to interactive dashboard and presentation.

---

## 📁 Dataset

| Property | Detail |
|----------|--------|
| **Source** | Customer transactional data |
| **Rows** | 3,900 |
| **Columns** | 18 |
| **Key Fields** | Age, Gender, Location, Category, Purchase Amount, Season, Subscription Status, Shipping Type, Review Rating, Payment Method, Frequency of Purchases |
| **Missing Data** | 37 null values in `Review Rating` (handled via median imputation) |

---

## 📌 Project Overview

This project explores customer shopping behavior to uncover actionable insights for business decision-making. It covers the full analytics workflow: data cleaning, exploratory analysis, SQL querying, visual dashboarding, and stakeholder-ready reporting.

**Key questions answered:**
- Which customer segments generate the most revenue?
- How do subscribers vs. non-subscribers compare in spending?
- Which products are most discount-dependent?
- How does revenue vary across age groups and categories?

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Python** (pandas, matplotlib, seaborn) | Data loading, EDA, cleaning, feature engineering |
| **PostgreSQL / SSMS** | SQL-based business analysis |
| **Power BI** | Interactive dashboard |
| **Microsoft Word / PDF** | Project report |
| **Gamma** | Presentation deck |

---

## 🔢 Project Steps

### 1. Data Loading & Exploration (Python)
- Imported dataset using `pandas`
- Inspected structure with `df.info()` and `df.describe()`
- Identified missing values and data types

### 2. Data Cleaning & Feature Engineering (Python)
- Imputed missing `Review Rating` values using **median per product category**
- Renamed columns to **snake_case** for consistency
- Created `age_group` column by binning customer ages
- Created `purchase_frequency_days` from purchase frequency field
- Dropped `promo_code_used` (found redundant with `discount_applied`)

### 3. Database Integration
- Loaded cleaned DataFrame into **PostgreSQL** via Python
- Enabled structured SQL analysis in **SSMS**

### 4. SQL Analysis (Business Queries)

| # | Query | Insight |
|---|-------|---------|
| 1 | Revenue by Gender | Males generated ~2× more revenue than females |
| 2 | High-Spending Discount Users | 839 customers used discounts yet spent above average |
| 3 | Top 5 Products by Rating | Gloves (3.86), Sandals (3.84), Boots (3.82) |
| 4 | Shipping Type Comparison | Express ($60.48) vs. Standard ($58.46) avg. spend |
| 5 | Subscribers vs. Non-Subscribers | Non-subscribers: 2,847 customers, $170K revenue |
| 6 | Discount-Dependent Products | Hat (50%), Sneakers (49.7%), Coat (49.1%) |
| 7 | Customer Segmentation | Loyal: 3,116 · Returning: 701 · New: 83 |
| 8 | Top 3 Products per Category | Jewelry, Blouse, Sandals, Jacket lead each category |
| 9 | Repeat Buyers & Subscriptions | 2,518 repeat buyers are non-subscribers |
| 10 | Revenue by Age Group | Young Adults lead at $62,143 |

### 5. Power BI Dashboard
- Built an interactive dashboard with slicers for Subscription Status, Gender, Category, and Shipping Type
- Visuals: KPI cards, donut chart, bar charts (revenue & sales by category and age group)

### 6. Report & Presentation
- Full written report documenting methodology, findings, and recommendations
- Presentation deck created using **Gamma**

---

## 📊 Dashboard Preview

> Built in Power BI — filters by subscription status, gender, product category, and shipping type.

**KPIs at a glance:**

| Metric | Value |
|--------|-------|
| Total Customers | 3,900 |
| Avg. Purchase Amount | $59.76 |
| Avg. Review Rating | 3.75 / 5 |
| Subscription Rate | 27% |

**Visuals included:**
- % of customers by subscription status (donut chart)
- Revenue by category (bar chart)
- Sales by category (bar chart)
- Revenue by age group (horizontal bar)
- Sales by age group (horizontal bar)

---

## 💡 Key Results & Recommendations

| Finding | Recommendation |
|---------|---------------|
| Only 27% of customers subscribe | Launch exclusive subscriber perks to boost sign-ups |
| 80% of customers are "Loyal" | Introduce a loyalty rewards program to retain them |
| Hat, Sneakers & Coat heavily discounted | Review discount strategy — balance volume vs. margin |
| Young Adults drive the most revenue | Target campaigns toward 18–35 age group |
| Express shipping users spend more | Promote premium shipping as an upsell |

---

## ▶️ How to Run

### Prerequisites
```
Python 3.8+
pandas, matplotlib, seaborn, sqlalchemy, psycopg2
PostgreSQL or SQL Server (SSMS)
Power BI Desktop
```

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/your-username/customer-shopping-analysis.git
cd customer-shopping-analysis

# 2. Install Python dependencies
pip install -r requirements.txt

# 3. Run EDA and data cleaning
python eda_cleaning.py

# 4. Load cleaned data to database
python load_to_db.py

# 5. Run SQL queries
# Open SSMS or pgAdmin and run files in /sql/ folder

# 6. Open dashboard
# Launch Power BI Desktop → Open dashboard.pbix
```

---

## 📂 Project Structure

```
customer-shopping-analysis/
│
├── data/
│   └── shopping_behavior.csv        # Raw dataset
│
├── notebooks/
│   └── eda_cleaning.ipynb           # Python EDA & cleaning
│
├── sql/
│   └── business_queries.sql         # All 10 SQL queries
│
├── dashboard/
│   └── dashboard.pbix               # Power BI file
│
├── report/
│   └── Customer_Shopping_Report.pdf # Full written report
│
├── presentation/
│   └── Customer_Shopping_PPT.pptx   # Gamma presentation
│
└── README.md
```

---

## 👤 Author

**O'ktam Zulfqorov**  
Data Analyst | Python · SQL · Power BI  
[LinkedIn](https://www.linkedin.com/in/oktamzulfqorov1/) · [GitHub](https://github.com/oktambek7)

---

*This project demonstrates a complete analytics workflow suitable for business reporting and portfolio presentation.*
