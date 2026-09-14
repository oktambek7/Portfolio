# World Top Companies — Revenue & Employment Dashboard

**A Power BI dashboard profiling the 100 largest companies by revenue — $12.2 trillion in combined revenue and 16.3 million employees — built with dynamic Top-N controls and custom pagination so one canvas serves both a 10-row summary and a 100-row deep dive.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Disconnected parameter tables · Decomposition tree · Custom visuals (Word Cloud)

---

## Business problem

Corporate-league-table data is normally consumed as a static ranked list, which answers almost nothing. An investor, strategist or market-research audience actually wants to interrogate it:

- Which **industries** concentrate the most revenue, and which merely concentrate headcount?
- How **geographically concentrated** is corporate revenue — which cities, not just states?
- Which companies are **growing**, and which are shrinking?
- What is the **revenue-per-employee** picture — who is capital-efficient and who is labour-intensive?
- And crucially: let the user set how many rows they want to see, rather than hard-coding "top 10".

## Dataset

| | |
|---|---|
| Grain | One row per company |
| Volume | **100 brands**, **37 industries**, **71 headquarters locations** |
| Totals | **$12,234,609M revenue** (~$12.2 trillion), **16,267,793 employees** |
| Key fields | Rank, Name, Industry, Headquarters, Revenue (USD millions), Employees, Growth % |

## What I built

**1. Ingest & shape (Power Query).** Cleaned the source league table, standardised industry labels, and split headquarters into city/state components so revenue could be aggregated geographically rather than only by company.

**2. Model — the technically interesting part.** Beyond the `Companies` fact table, I built **two disconnected parameter tables**, `# Items` and `# Pages`. These aren't joined to the data; they exist purely to feed user-controlled values into measures. This is what powers:

- a **dynamic Top-N control** — the user sets `# Items` on a slicer and the ranked table resizes live
- **custom pagination** — the `# Pages` table drives a 1–8 page selector on the *Revenue by states* visual, so 71 locations are browsable inside a fixed-height card instead of a scrollbar

Disconnected tables driving measure logic is a genuinely advanced modelling pattern and the backbone of this report.

**3. Measures (DAX).**

| Measure | Purpose |
|---|---|
| `Item Rank` | Dynamic ranking that re-computes against the current filter set |
| `Item Count` | Reads the user's `# Items` selection to control Top-N |
| `Value Indicator` | Drives the conditional up/down growth arrows in the table |
| `rev + $` | Formatted revenue display for card and label contexts |
| `Dummy` | Axis/layout helper for the paginated bar visual |

**4. Report design.** KPI cards for the five headline figures; a **ranked table with in-cell data bars and conditional growth indicators**; a **decomposition tree** for guided root-cause exploration (States → Industry → Brand → Revenue); a **Word Cloud custom visual** sizing every brand by revenue; a paginated horizontal bar chart for revenue by headquarters; a combo chart pairing employees against revenue by industry; and three synchronised slicers (Brand, Industry, States).

## What the analysis found

**Headline position:**

| Metric | Value |
|---|---|
| Combined revenue | **$12,234,609M** (~$12.2tn) |
| Combined employees | **16,267,793** |
| Industries | 37 |
| Headquarters locations | 71 |
| Companies | 100 |

**Retail sits at the top, but the top 10 is industry-diverse.**

| Rank | Industry | Brand | Revenue ($M) | Growth |
|---|---|---|---|---|
| 1 | Retail | Walmart | — | +0.06% |
| 2 | Retail and cloud computing | Amazon | 574,785 | +0.12% |
| 3 | Electronics | Apple | 383,482 | **−0.03%** |
| 4 | Healthcare | UnitedHealth Group | 371,622 | +0.15% |
| 5 | Conglomerate | Berkshire Hathaway | 364,482 | +0.21% |
| 6 | Healthcare | CVS Health | 357,776 | +0.11% |
| 7 | Petroleum | ExxonMobil | 344,582 | **−0.17%** |
| 8 | Technology and cloud computing | Alphabet | 307,394 | +0.09% |
| 9 | Health | McKesson | 276,711 | +0.05% |
| 10 | Pharmacy wholesale | Cencora | 262,173 | +0.10% |

**Healthcare is the real concentration story.** Four of the top ten — UnitedHealth, CVS, McKesson and Cencora — are healthcare or pharmacy distribution, together exceeding **$1.26 trillion**. No other sector places four companies in the top ten. A ranking read as a list makes this invisible; the decomposition tree makes it obvious.

**Two of the ten largest companies are shrinking.** Apple (−0.03%) and ExxonMobil (−0.17%) post negative growth while the other eight grow. Scale and momentum are not the same thing, and the conditional indicators in the table surface this at a glance.

**Revenue is geographically concentrated in a handful of cities.**

| Rank | Headquarters | Revenue ($M) |
|---|---|---|
| 1 | New York City, New York | **1,180,312** |
| 2 | Bentonville, Arkansas | 648,125 |
| 3 | Houston, Texas | 584,165 |
| 4 | Seattle, Washington | 574,785 |
| 5 | Cupertino, California | 383,482 |
| 6 | Minnetonka, Minnesota | 371,622 |
| 7 | Omaha, Nebraska | 364,482 |
| 8 | Woonsocket, Rhode Island | 357,776 |
| 9 | Atlanta, Georgia | 347,429 |
| 10 | Spring, Texas | 344,582 |

New York City alone accounts for **~9.6%** of all revenue across 100 companies. More strikingly, several top-10 locations — Bentonville, Woonsocket, Spring, Minnetonka — are small towns that rank purely because one giant is headquartered there. **Corporate revenue geography is company-driven, not city-driven**, which is the opposite of the intuition most maps of economic activity encourage.

**Revenue and headcount decouple sharply by industry.** The employees-vs-revenue combo chart shows Retail carrying by far the largest workforce (~4.7M) against its revenue, while Technology and Pharmaceuticals generate comparable revenue on a fraction of the headcount. Retail's revenue is bought with labour; tech's is not.

## Recommendations delivered

1. **Read the league table by industry cluster, not by rank.** Healthcare's $1.26tn across four top-ten firms is the dominant structural fact and is invisible in rank order.
2. **Use revenue-per-employee, not revenue, to compare across sectors.** The 4.7M-employee retail bloc and the far leaner tech bloc are not comparable on revenue alone.
3. **Treat HQ-city revenue rankings with care in any regional-economy analysis.** Small towns rank because of single-company domiciling, not local economic activity — a trap for location-based investment screening.
4. **Watch the negative-growth majors.** Two of the ten largest companies are contracting; in a concentrated index, that has outsized effect.

## Skills demonstrated

- **Advanced DAX modelling:** disconnected parameter tables driving user-controlled Top-N and custom pagination, dynamic ranking that recomputes under filter context, conditional-formatting driver measures
- **Analytical:** market concentration analysis, industry clustering, revenue-per-employee efficiency comparison, growth-vs-scale decoupling, geographic concentration analysis
- **Data engineering:** league-table cleaning, industry label standardisation, headquarters city/state parsing
- **BI/UX:** decomposition tree for guided exploration, custom visual integration (Word Cloud), in-cell data bars and KPI indicators, solving a fixed-canvas space problem with pagination rather than a scrollbar

## Files in this folder

| File | Description |
|---|---|
| `Top companies revenue (1).png` | Full-dashboard screenshot |
| `Interactive version in Power BI (3).pbix` | Power BI source file — model, parameter tables, measures and custom visual |

## How to explore

Open the `.pbix` in **Power BI Desktop** (free). The `# Items` Top-N control, the 1–8 pagination selector and the decomposition tree are all interactive-only — the PNG shows a single static state.
