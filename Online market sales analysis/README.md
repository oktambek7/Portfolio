# Online Market Sales Analysis

**A Power BI sales report over 10K orders and $2.3M revenue that separates the customers, regions and products that generate profit from the ones that only generate volume.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Data modelling · Bookmarks & drill-through · Custom theme

---

## Business problem

An online retailer was tracking revenue but not profit contribution. Revenue growth was masking a margin problem: discounting was being applied inconsistently, and nobody could say which product lines or regions were actually earning money after discount. The questions to answer:

- Which customer segment drives revenue, and which drives *profit*?
- Which regions and states over- and under-perform?
- Which sub-categories are worth stocking, and which are dead weight?
- Where is discounting eroding margin instead of buying volume?
- How seasonal is demand, and are we resourced for the peak?

## Dataset

| | |
|---|---|
| Grain | One row per order line |
| Volume | **10K orders**, **38K units**, **$2.3M sales**, **$599K profit** |
| Dimensions | Customer segment, region, state, city, product sub-category, order date |
| Key fields | Order ID, Order Date, Sales, Quantity, Discount, Profit, Segment, Region, State, City, Sub-Category |

## What I built

**1. Ingest & shape (Power Query).** Typed and cleaned the orders feed, built a date hierarchy (Year → Month → Day) off `Order Date`, and derived a **`PosProfit`** column to isolate positive profit contribution from loss-making lines — so profit analysis isn't silently netted out by heavily discounted orders.

**2. Model.** A single-fact orders model with date, geography, customer-segment and product hierarchies, so every visual on the canvas responds to the same filter set.

**3. Measures (DAX).** `Total Sales`, `Total Orders` (distinct order count, not row count), `Total Units sold`, `Profit`, `Total Discount` — plus **`Isblank_selected_val`**, a dynamic-title measure that makes the detail panel respond to whatever the user has selected and show a sensible default when nothing is selected. That's the difference between a report that reads correctly in every state and one that shows "Total" headings over filtered numbers.

**4. Report design.** A branded canvas (custom `RevGenix_Dark` theme) with KPI cards, donut charts for segment and region mix, dual line charts for monthly trend, **two map visuals** for geographic distribution, a ranked bar chart of sub-categories, and — the interactive core — a **bookmark-driven panel** plus a **drill-through page** that takes any state or city to line-level detail.

## What the analysis found

**Headline position:**

| Metric | Value |
|---|---|
| Total sales | **$2.3M** |
| Profit | **$599K** (≈26% margin) |
| Orders | 10K |
| Units sold | 38K |

**Consumer is half the business.**

| Segment | Sales | Share |
|---|---|---|
| Consumer | $1.16M | **50.56%** |
| Corporate | $0.71M | 30.74% |
| Home Office | $0.43M | 18.70% |

The Consumer segment alone outsells Corporate and Home Office combined — but it is also the segment most exposed to discounting, which is exactly why the discount view matters more here than anywhere else.

**Regional performance spans nearly 2x.**

| Region | Sales | Share |
|---|---|---|
| West | $725.46K | 31.58% |
| East | $678.78K | 29.55% |
| Central | $501.24K | 21.82% |
| South | $391.72K | 17.05% |

West and East together carry **61%** of revenue. South generates barely half of West's revenue — a coverage and go-to-market gap, not a demand gap.

**Product revenue is extremely top-heavy.** **Phones ($330.01K)** and **Chairs ($328.45K)** are the two largest sub-categories and are nearly tied, followed by Storage and Tables. The long tail — Art, Envelopes, Labels, Fasteners — falls to single-digit and low-double-digit thousands, with the smallest sub-category at **$3.02K**. That's a **100x spread** between the best and worst performing product lines, which makes the stocking decision straightforward.

**Demand is sharply seasonal.** Monthly sales range from a trough of **$60K** to a peak of **$352K** — a near **6x swing** — with the strongest months concentrated in the back half of the year (September through December all clearing $300K+ or approaching it). Any flat-capacity staffing or inventory plan is wrong for eight months of the year.

**Drill-through reveals discount pressure at state level.** Drilling into Michigan, for example, surfaces $76.3K sales across 946 units against $24.5K profit — with the discount column exposing exactly how much margin was traded away to close those units. This is the view that lets a category manager act, rather than just observe.

## Recommendations delivered

1. **Stop stocking the long tail.** The bottom sub-categories contribute a rounding error against Phones and Chairs; the working capital tied up in them is better deployed into the top four.
2. **Investigate the South, don't discount into it.** South's underperformance is structural (coverage) rather than price-driven — more discounting there will cost margin without fixing reach.
3. **Plan capacity against the 6x seasonal swing.** Inventory and fulfilment staffing should be built around the Q3/Q4 peak with a deliberately lean H1, not an annual average.
4. **Put a discount ceiling on the Consumer segment.** It's half of revenue and the most discount-exposed; a modest cap protects the largest profit pool in the business.

## Skills demonstrated

- **Analytical:** profit-vs-revenue contribution analysis, segment and regional benchmarking, product portfolio / long-tail analysis, seasonality quantification, discount-erosion analysis
- **Data engineering:** date hierarchy construction, derived profit columns, dimensional modelling from a flat orders feed
- **DAX:** distinct-count order measures, dynamic conditional title measures (`Isblank_selected_val`), percentage-of-total with correct filter context
- **BI/UX:** bookmark-driven interactivity, drill-through to line-level detail, geospatial map visuals, custom corporate theming

## Files in this folder

| File | Description |
|---|---|
| `Online markets sales trends.pdf` | Static export of the report |
| `Interactive version in Power BI (2).pbix` | Power BI source file — full model, measures, bookmarks and drill-through |

## How to explore

Open the `.pbix` in **Power BI Desktop** (free) — the bookmark panel and the state/city drill-through only work interactively. The PDF shows the default unfiltered view.
