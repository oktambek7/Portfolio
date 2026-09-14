# Top Cars Sale — Financial & Pricing Analysis (2014–2015)

**A five-page Power BI report that measures $6.47bn of used-vehicle wholesale sales against the MMR market benchmark to expose where cars are systematically mispriced.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Data modelling · Report navigation & drill-through

---

## Business problem

A wholesale vehicle remarketing business moves hundreds of thousands of cars a year through auction channels. Every vehicle carries an **MMR (Manheim Market Report) valuation** — the industry benchmark for what a car *should* fetch. The commercial questions were:

- Are we actually selling above or below the market benchmark, and by how much?
- Which brands, body types and sellers destroy margin, and which ones earn a premium?
- Which consignors (Ford Credit, Hertz, Avis, the captive finance arms) drive volume versus value?
- How does condition and odometer reading feed through into realised price?

Without a single source of truth, pricing decisions were being made brand-by-brand on gut feel.

## Dataset

| | |
|---|---|
| Grain | One row per vehicle sale |
| Volume | **472,357 vehicles**, **$6.47bn** in realised revenue |
| Period | Jan 2014 – Jul 2015 (sparse 2014 coverage — see *Data quality* below) |
| Coverage | **52 brands**, model years 1996–2015, all US states |
| Key fields | Brand, model, body type, transmission, condition grade, odometer, state, colour, seller, selling price, MMR |

## What I built

**1. Ingest & shape (Power Query).** Standardised brand and model text, derived a clean `YearMonth` key for time intelligence, bucketed body types, and normalised inconsistent seller names (the raw feed contains near-duplicates such as `nissan-infiniti lt` and `nissan infiniti lt`).

**2. Model.** A dimensional model built on a dedicated **`DateTable`** (with `Year`, `Month Name` and `YearMonth` columns) joined to the sales fact, so every slicer filters consistently across all report pages.

Rather than dumping every measure into one place, I organised the DAX into **three purpose-built measure tables** — a pattern that keeps a model of this size navigable:

| Measure table | Contains |
|---|---|
| `KPI` | The executive headline measures |
| `Advanced DAX measures` | Derived analytics such as `% Above MMR` and `Sales Volume` |
| `MMR Margin %` | The entire pricing-variance layer and its threshold parameter |

**3. Measures (DAX).** The analytical core is the pricing-variance layer:

- `Total sales revenue`, `Total cars sold`, `Average selling price`
- `Avg car condition`, `Avg Odometer read`
- **`Selling Price vs MMR`** — realised price variance against the benchmark
- **`% Above MMR`** — share of units clearing above benchmark
- **`Adjusted MMR`** and **`Price vs Adjusted MMR`** — a benchmark corrected for condition and mileage, so a worn car isn't flagged as underpriced purely because it's worn
- **`Price Status`** — a calculated classification putting every sale into *Fair*, *Underpriced* or *Overpriced*
- `MMR Margin %` as a tunable threshold, so the business can widen or tighten what counts as "fair"

**4. Report design.** Five themed pages — a **Welcome page** plus **Executive Summary**, **Brand Analysis**, **Time Intelligence** and **Geography & Sellers** — wired together with custom navigation buttons, a collapsible filter panel, and a **drill-through page** that takes any brand/model straight to vehicle-level detail.

## What the analysis found

**Pricing is not centred on the benchmark.** Overall the book sells at **−1.06% versus MMR** with **47.09% of units clearing above** benchmark. That near-symmetry hides the real story: once the benchmark is condition- and mileage-adjusted, the largest price-status bucket covers **248K cars (52.52%)** of the book, against 175K (37.15%) and 49K (10.33%) in the other two — mispricing is the norm, not the exception.

**Volume and value sit in different places.**

| Top 5 brands by volume | Units |
|---|---|
| Ford | 81,018 |
| Chevrolet | 54,153 |
| Nissan | 44,043 |
| Toyota | 35,315 |
| Dodge | 27,184 |

The average selling price across the whole book is **$13,690**, but the luxury tier — Rolls-Royce, Ferrari, Lamborghini, Bentley, Tesla, Aston Martin — averages up to **~$153K per unit**. A handful of vehicles carries margin that tens of thousands of mass-market units do not.

**Body mix is overwhelmingly conventional.** Sedans are **53.63%** of units and SUVs **30.7%** — together 84% of the book. Hatchback, minivan and coupe share the remaining ~16%.

**Seller concentration is high.** Captive finance arms and rental fleets dominate consignment:

| Seller | Revenue | Units |
|---|---|---|
| Ford Motor Credit Company LLC | $314.3M | 17,756 |
| The Hertz Corporation | $225.0M | 16,286 |
| Nissan-Infiniti LT | $216.9M | 15,777 |

Losing any one of the top three consignors would remove roughly 3–5% of total revenue.

**Data quality finding.** The transaction feed is not continuous: 2014 is represented only by January, February and December, while 2015 runs unbroken January–July. Any year-over-year comparison built on this data would be wrong. I surfaced this on the Time Intelligence page rather than hiding it, and scoped trend analysis to the contiguous 2015 window.

## Recommendations delivered

1. **Re-anchor pricing on Adjusted MMR, not raw MMR.** Raw benchmark comparison mislabels high-mileage and low-condition stock; the adjusted measure isolates genuine pricing error from expected depreciation.
2. **Attack the overpriced bucket first.** It is the largest segment and represents unsold-days and markdown risk, not realised profit.
3. **Protect the top-three consignor relationships** and diversify — revenue concentration in three sellers is a structural risk.
4. **Treat the luxury tier as a separate business line.** Its price behaviour, volume and variance profile share nothing with the sedan/SUV core, and blending them into one average hides both.

## Skills demonstrated

- **Analytical:** benchmark variance analysis, price-elasticity framing, cohort/segment comparison, concentration risk, data-quality auditing
- **Data engineering:** Power Query cleaning and deduplication of dirty categorical fields, date-key derivation, star-schema modelling
- **DAX:** multi-step calculated measures, calculated classification columns, threshold parameters, correct filter-context handling across a four-page report
- **BI/UX:** multi-page navigation, drill-through, filter panels, executive-to-detail information hierarchy

## Files in this folder

| File | Description |
|---|---|
| `Car sales analysis report (2014-2015).pdf` | Full static export of all four report pages |
| `Car sales analysis report (2014-2015).pbix` | Power BI source file — full data model, DAX measure tables and all five report pages |

## How to explore

Open the `.pbix` in **Power BI Desktop** (free) to use the page navigation, slicers and drill-through, and to inspect the organised measure tables (`KPI`, `Advanced DAX measures`, `MMR Margin %`) directly. The PDF shows the static export of the analysis pages.
