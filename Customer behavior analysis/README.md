# Customer Shopping Behaviour Analysis

**An end-to-end analytics project over 3,900 transactions — Python cleaning → SQL Server → Power BI → stakeholder deck — whose central finding is that this business has no spending problem, it has a customer-acquisition problem.**

**Tools:** Python (pandas) · SQL Server / T-SQL · SQLAlchemy + pyodbc · Power BI · Jupyter · Gamma

---

## Business problem

A retailer had transactional data but no view of *who* was driving revenue or *what* actually moved spend. Marketing was running discounts and a subscription programme without evidence that either worked. The questions:

- Which customer segments generate the most revenue — and **why**?
- Do subscribers actually spend more than non-subscribers?
- Is discounting buying incremental basket size, or giving away margin?
- Which products are discount-dependent?
- Where should acquisition and retention spend go?

## Dataset

| | |
|---|---|
| Grain | One row per transaction |
| Volume | **3,900 transactions**, 18 columns |
| Revenue | **$233,081** total |
| Key fields | Age, gender, location, item, category, purchase amount, season, review rating, subscription status, shipping type, discount applied, promo code used, previous purchases, payment method, purchase frequency |
| Data quality | **37 nulls in `Review Rating`**; `promo_code_used` found to be fully redundant |

## What I built — the full pipeline

### 1. Cleaning & feature engineering (Python / pandas — `cleaning.ipynb`)

- **Median imputation by category, not global median.** Nulls in `review_rating` were filled using `groupby('Category')['Review Rating'].transform(lambda x: x.fillna(x.median()))` — a global median would have flattened genuine differences in how categories are rated.
- **Proved a column was redundant before dropping it.** Rather than assuming, I tested `(df['discount_applied'] == df['promo_code_used']).all()` → `True`, confirming the two columns were identical, then dropped `promo_code_used`. Dropping a column on a hunch is how you lose signal; dropping it on a proof is data engineering.
- **Quartile-based age binning.** `pd.qcut(df['age'], q=4)` into `Young Adult / Adult / Middle aged / Senior` — quartiles give four evenly-populated groups (≈940–1,030 each) rather than arbitrary cut-points that produce unbalanced, uncomparable segments. Resulting boundaries: 18–31, 32–44, 45–57, 58–70.
- **Converted a categorical into a numeric.** `frequency_of_purchases` ("Weekly", "Bi-Weekly", "Quarterly", "Annually") was mapped to `purchase_frequency_days` (7, 14, 90, 365) so purchase cadence could be measured and sorted rather than just grouped.
- Normalised all column names to `snake_case`.

### 2. Database load (SQLAlchemy → SQL Server)

Loaded the cleaned DataFrame into **SQL Server** via `create_engine` with the ODBC Driver 17 connection string and `df.to_sql()` — moving the analysis off a local notebook and into a queryable database that other tools and analysts can reach.

### 3. SQL analysis (`business_questions_query.sql`)

Ten T-SQL business queries, including window functions and CTEs:

| # | Question | Technique |
|---|---|---|
| 1 | Revenue by gender | `GROUP BY` aggregation |
| 2 | High spenders who still used a discount | Scalar subquery against the average |
| 3 | Top 5 products by review rating | `TOP` + ordered aggregate |
| 4 | Express vs Standard shipping spend | Filtered comparison |
| 5 | Subscribers vs non-subscribers | Multi-metric aggregation |
| 6 | Most discount-dependent products | Conditional aggregation (`SUM(CASE WHEN...)`) as a rate |
| 7 | New / Returning / Loyal segmentation | `CASE` banding on purchase history |
| 8 | Top 3 products *per category* | **CTE + `ROW_NUMBER() OVER (PARTITION BY ...)`** |
| 9 | Do repeat buyers subscribe? | Filtered cohort comparison |
| 10 | Revenue by age group | Aggregation on the engineered `age_group` |

### 4. Power BI dashboard (`Customer behavior dashboard.pbix`)

Measures: `Number of customers`, `Average Purchase Amount`, `Average Review Rating`, plus revenue aggregations. Built with KPI cards, a donut chart for subscription mix, clustered bar charts for revenue and sales by category and age group, a line chart for seasonal trend, and **four slicers** (subscription status, gender, category, shipping type).

### 5. Stakeholder deck

A presentation deck summarising methodology, findings and recommendations for a non-technical audience.

---

## What the analysis found

### Headline metrics *(independently verified against the raw CSV)*

| Metric | Value |
|---|---|
| Transactions | 3,900 |
| Total revenue | **$233,081** |
| Average purchase amount | **$59.76** |
| Average review rating | **3.75 / 5** |
| Subscription rate | **27.0%** (1,053 of 3,900) |

### The central finding: spend per customer is flat across *every* dimension

This is the insight the whole project turns on. Average purchase amount barely moves no matter how the data is cut:

| Dimension | Segment A | Segment B | Gap |
|---|---|---|---|
| Gender | Male **$59.54** | Female **$60.25** | 1.2% |
| Subscription | Subscriber **$59.49** | Non-subscriber **$59.87** | 0.6% |
| Discount | Discounted **$59.28** | Full price **$60.13** | 1.4% |
| Age group | Young Adult **$60.45** | Senior **$59.07** | 2.3% |
| Season | Fall **$61.56** | Summer **$58.41** | 5.1% |

Every segment sits within a few percent of the $59.76 average. **Revenue differences between segments are driven almost entirely by how many customers are in them, not by how much those customers spend.**

### This reframes the obvious conclusions

**"Men generate 2× the revenue" is misleading.** Male customers produce $157,890 against female customers' $75,191 — but that is a *headcount* effect (2,652 men vs 1,248 women), not a spending one. Women actually spend **slightly more per transaction** ($60.25 vs $59.54). The correct read is not "target men" — it's "the female customer base is under-acquired relative to its value."

**Young Adults lead revenue for the same reason.** At **$62,143** they top the age groups, but with 1,028 customers against ~944 in each other quartile. Their per-transaction spend advantage is under $1.40.

### The subscription programme is not working

| | Customers | Revenue | Avg spend |
|---|---|---|---|
| Non-subscribers | **2,847** | $170,436 | $59.87 |
| Subscribers | 1,053 | $62,645 | $59.49 |

Subscribers spend **less** per transaction than non-subscribers. At 27% penetration, the programme is neither acquiring broadly nor lifting spend among those it does reach.

### Discounting is pure margin give-away

**1,677 of 3,900 transactions (43%) carried a discount**, and those transactions averaged **$59.28** against **$60.13** at full price. Discounting is not buying larger baskets — it is reducing revenue on purchases that would have happened anyway. The most discount-dependent products (Hat ~50%, Sneakers ~49.7%, Coat ~49.1%) are discounted on roughly half of all their sales.

### Category and loyalty structure

| Category | Revenue | Orders |
|---|---|---|
| Clothing | $104,264 | 1,737 |
| Accessories | $74,200 | 1,240 |
| Footwear | $36,093 | 599 |
| Outerwear | $18,524 | 324 |

Customer segmentation by purchase history: **Loyal 3,116 · Returning 701 · New 83**. Roughly **80% of the base is already loyal**, while new-customer intake is very thin — 83 customers. Combined with the flat-spend finding, this is a business with a healthy retained base and a weak top-of-funnel.

---

## Recommendations delivered

1. **Shift budget from discounting to acquisition.** With spend per customer effectively fixed at ~$60, revenue is a function of customer *count*. The 43% of transactions receiving discounts generate no measurable basket lift — that margin is better spent bringing in new customers.
2. **Fix the funnel: 83 new customers vs 3,116 loyal is not sustainable.** The retained base is strong, so retention spend has low marginal return; the constraint is clearly at the top of the funnel.
3. **Under-acquired female segment is the cheapest growth available.** Women spend marginally more per transaction but make up only 32% of customers. Closing that gap is a pure headcount play against a proven-value segment.
4. **Rebuild or retire the subscription programme.** It currently correlates with slightly *lower* spend. Either redesign the benefits to drive incremental purchase frequency, or stop investing in it.
5. **Cap discounting on the half-price-by-default products.** Hat, Sneakers and Coat are discounted on ~50% of sales — that's not a promotion, it's a permanent price cut that should be reflected in list price instead.

---

## Skills demonstrated

- **Python / pandas:** group-wise median imputation, quartile binning (`qcut`), categorical-to-numeric mapping, redundancy testing before column removal, schema normalisation
- **SQL (T-SQL):** CTEs, `ROW_NUMBER() OVER (PARTITION BY)`, conditional aggregation, scalar subqueries, `CASE`-based segmentation
- **Data engineering:** Python → SQL Server pipeline via SQLAlchemy/pyodbc, reproducible notebook-to-database workflow
- **Analytical rigour:** distinguishing **volume effects from rate effects** — the difference between "men generate more revenue" and "men spend more," which points marketing budget in opposite directions
- **BI & communication:** Power BI dashboard with multi-slicer interactivity, plus a stakeholder deck translating findings into decisions

---

## Files in this folder

| File | Description |
|---|---|
| `cleaning.ipynb` | Python EDA, cleaning, feature engineering and database load |
| `business_questions_query.sql` | All 10 T-SQL business queries |
| `Customer behavior dashboard.pbix` | Power BI dashboard — model, measures and visuals |
| `customer_shopping_behavior.csv` | Raw source dataset (3,900 rows) |
| `Customer-beh-analysis-Gamma.pptx` | ⚠️ **Currently a 2-byte placeholder — the deck did not upload correctly and needs to be re-committed.** |

## How to explore

- **Start with `cleaning.ipynb`** — it renders directly in GitHub and shows the full cleaning and feature-engineering reasoning.
- **`business_questions_query.sql`** contains the ten business queries; run against SQL Server after loading the cleaned data.
- **`Customer behavior dashboard.pbix`** opens in **Power BI Desktop** (free) for the interactive dashboard.

```
pip install pandas matplotlib seaborn sqlalchemy pyodbc
```
