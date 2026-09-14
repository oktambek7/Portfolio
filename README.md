# Data Analytics Portfolio — O'ktam Zulfqorov

**Data Analyst & Analytics Engineer** — Python · SQL · Power BI · DAX · Power Query

[LinkedIn](https://www.linkedin.com/in/oktamzulfqorov1/) · [GitHub](https://github.com/oktambek7)

---

Eight end-to-end analytics projects, each taking a real business question from raw data through cleaning, modelling and measure design to an interactive dashboard and a written recommendation. Every project in this repository was built by me — the datasets, the data models, the DAX, and the conclusions.

Each folder has its own README covering the business problem, the pipeline I built, what the analysis found, and the recommendations delivered.

---

## Projects

| Project | Domain | Scale | Core skill demonstrated |
|---|---|---|---|
| **[Customer Behaviour Analysis](./Customer%20behavior%20analysis)** | Retail / CRM | 3,900 transactions · $233K revenue | **Full stack:** Python → SQL Server → Power BI. Separating volume effects from rate effects |
| **[Top Cars Sale — Financial & Pricing Analysis](./Top%20cars%20sale%20financial%20analysis%20report)** | Automotive wholesale | 472,357 vehicles · **$6.47bn** | Benchmark variance modelling against MMR; 5-page report with drill-through |
| **[World Top Companies Revenue](./World%20top%20companies%20revenue%20dashboard)** | Corporate finance | 100 companies · **$12.2tn** | **Disconnected parameter tables** driving dynamic Top-N and custom pagination |
| **[Global Sustainable Energy Analysis](./Global%20sustainable%20energy%20analysis%20%282000-2020%29)** | Energy / ESG | 176 countries · 2000–2020 | Star-schema design with a dedicated measures table; 20-year trend analysis |
| **[Wisabi Bank — ATM Transactions](./Wisabi%20bank%20financial%20analysis)** | Banking | ~2M transactions · ₦39bn | Utilisation & capacity analysis driving a capital-allocation recommendation |
| **[Online Market Sales Analysis](./Online%20market%20sales%20analysis)** | E-commerce | 10K orders · $2.3M sales | Profit-vs-revenue contribution; bookmarks and drill-through |
| **[London Bike Riders Analysis](./London%20bike%20riders%20analysis%20dashboard)** | Transport / mobility | **19.9M rides** · 24 months | Time-series and weather-elasticity analysis; avoiding the average-vs-total fallacy |
| **[World-Class Cars Behaviour Analysis](./World-class%20cars%20behaviour%20analyis%20dashboard)** | Automotive | 1,026 vehicles | **Heavy Power Query engineering** — parsing numerics out of unit-suffixed text |

---

## Selected findings

A portfolio is only as good as the conclusions in it. A few of the more useful ones:

- **Customer Behaviour** — Average spend is flat (~$60) across gender, age, season, subscription status *and* discount status. Every revenue difference between segments is a headcount effect. "Men generate 2× the revenue" is a customer-count artefact — women actually spend slightly *more* per transaction. That single distinction reverses the marketing recommendation.
- **Top Cars Sale** — Selling prices run −1.06% against the MMR benchmark overall, but once the benchmark is adjusted for condition and mileage, mispricing turns out to be the norm rather than the exception across a $6.47bn book.
- **World Top Companies** — Four of the ten largest companies are healthcare or pharmacy distribution, together over **$1.26tn**. Read as a ranked list this is invisible; read as an industry cluster it is the dominant structural fact.
- **Global Sustainable Energy** — Renewables are 22.32% of electricity, but low-carbon generation reaches 36.80% once nuclear is counted. The 14-point gap materially changes how transition progress should be judged.
- **Wisabi Bank** — The ATM network runs at **12.9% utilisation**. The constraint is distribution, not capacity — which turns "build more ATMs" into "relocate the ones we have."
- **London Bike Riders** — Ridership runs 78% higher in summer than winter, and 66% of all journeys happen in clear or lightly clouded conditions. Temperature alone is a usable short-range demand predictor.

---

## Technical skills

**Data engineering & preparation**
Power Query (M) · parsing and type-casting dirty text fields · group-wise median imputation · quartile binning · categorical-to-numeric mapping · deduplication of inconsistent categorical data · Python → SQL Server pipelines via SQLAlchemy

**Data modelling**
Star-schema design · dedicated date dimensions · separated measures tables · **disconnected parameter tables** for user-driven logic · conformed dimensions across multi-page reports

**DAX**
Ratio and percentage-of-total measures with controlled filter context · dynamic ranking · selection-aware measures for responsive titles · calculated classification columns · threshold parameters · conditional-formatting driver measures

**SQL**
CTEs · window functions (`ROW_NUMBER() OVER (PARTITION BY)`) · conditional aggregation · scalar subqueries · `CASE`-based segmentation

**Python**
pandas · matplotlib · seaborn · SQLAlchemy · Jupyter

**BI & reporting**
Multi-page report navigation · drill-through · bookmarks · synchronised slicers · decomposition trees · custom visuals · geospatial mapping · custom theming · executive-to-detail information hierarchy

**Analytical methods**
Benchmark variance analysis · market concentration · cohort and segment comparison · seasonality and time-series analysis · elasticity quantification · utilisation and capacity analysis · price-performance curves · data-quality auditing

---

## How to explore these projects

Most projects include a `.pbix` Power BI source file alongside a PDF or PNG export.

- **PDF / PNG** — the static report, viewable directly on GitHub with no software required.
- **`.pbix`** — open in **[Power BI Desktop](https://powerbi.microsoft.com/desktop/)** (free) to use the slicers, drill-through and navigation, and to inspect the data model, Power Query steps and DAX measures directly.

The interactive features (Top-N controls, pagination, drill-through, bookmarks) only work in the `.pbix` — the static exports show a single filter state.

---

## Repository status

One source file was corrupted during an earlier upload and is currently a 2-byte placeholder — it is flagged in its project README and needs re-committing:

- `Customer behavior analysis/Customer-beh-analysis-Gamma.pptx`

Every other project file is present and complete.
