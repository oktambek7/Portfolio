# Global Sustainable Energy Analysis (2000–2020)

**A Power BI report tracking the energy transition across 176 countries over 20 years — measuring who actually decarbonised, who still lacks electricity, and whether climate finance is reaching the countries that need it.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Star-schema modelling · Dedicated measures table

---

## Business problem

Energy-transition reporting is usually either a single headline number or an unreadable wall of country statistics. A policy, ESG or development-finance audience needs to answer specific questions:

- Is the world's electricity mix actually shifting from fossil fuels to renewables, or just growing?
- Which countries generate the most renewable electricity — and is that the same as being *clean*?
- Where are people still without electricity access at all?
- Does international climate finance correlate with improved electricity access?
- Does wealth (GDP per capita) predict renewable share?

## Dataset

| | |
|---|---|
| Source | Global Data on Sustainable Energy |
| Grain | One row per country per year |
| Coverage | **176 countries**, **2000–2020** (21 years) |
| Key fields | Electricity from fossil fuels / nuclear / renewables (TWh), access to electricity (% of population), CO₂ emissions (kt), energy intensity, low-carbon electricity %, renewable share %, financial flows to developing countries ($), GDP per capita |

## What I built

**1. Ingest & shape (Power Query).** Cleaned and typed the raw country-year dataset, handling the missing-value patterns common in multi-decade international statistics, and standardised country entity names for reliable grouping.

**2. Model.** Built a proper **star schema**: a dedicated `Date` dimension joined to the country-year fact table, plus a separate **`Measure table`** holding all calculations. Separating measures from the data tables is a deliberate modelling choice — it keeps the model navigable and prevents measures from being tied to any single column's context.

**3. Measures (DAX).** Eight core measures power the entire report:

| Measure | What it answers |
|---|---|
| `Renewable Share (%)` | How much of the mix is renewable |
| `Low Carbon Electricity (%)` | Renewables **plus** nuclear — the true decarbonisation figure |
| `Total Electricity (TWh)` | Absolute generation scale |
| `Renewable Electricity (TWh)` | Absolute renewable generation |
| `Total CO2 Emissions (kt)` | Emissions outcome |
| `Avg Access to Electricity (%)` | Energy poverty |
| `Avg Energy Intensity` | Energy efficiency per unit of output |
| `Total Financial Flows ($)` | Climate finance to developing countries |

**4. Report design.** A single dense analytical canvas with **8 synchronised slicers** (year, country, and clear-all controls), KPI cards, a stacked-area generation-mix chart, dual-axis combo charts pairing financial flows against access, a **scatter plot of renewable share against GDP per capita**, and ranked bar charts for both leaders and laggards.

## What the analysis found

**Headline position (2000–2020):**

| Metric | Value |
|---|---|
| Renewable share of electricity | **22.32%** |
| Low-carbon electricity (renewables + nuclear) | **36.80%** |
| Average access to electricity | **78.93%** |
| Total CO₂ emissions | **514.93M kt** |
| Countries covered | 176 |

**Renewable *share* and renewable *volume* are different stories.** Renewables sit at 22.32% of the mix, but low-carbon electricity reaches 36.80% once nuclear is included — a 14-point gap that vanishes from any report looking only at renewables. Nuclear is doing a third of the decarbonisation work and is routinely omitted from the headline.

**Renewable generation grew steadily but from a fossil-dominated base.** Renewable electricity climbs from ~2.5–2.6K TWh in 2000 to **7.0K TWh by 2020** — near-tripling. But fossil generation grew in absolute terms over the same window, which is why the *share* moved far less than the volume. The transition added clean capacity faster than it displaced dirty capacity.

**Scale leaders are not efficiency leaders.**

| Top renewable generators (TWh) | | Top CO₂ emitters (kt) | |
|---|---|---|---|
| China | 19.7K | China | 153M |
| United States | 10.2K | United States | 107M |
| Brazil | 8.5K | India | 33M |
| Canada | 8.2K | Japan | 24M |
| India | 2.6K* | Germany | 15M |

China leads the world on renewable generation **and** on emissions — absolute renewable investment does not equal a clean grid when total demand is growing faster.

**Energy poverty is geographically concentrated.** The bottom 10 countries for electricity access are almost entirely Sub-Saharan African: South Sudan, Burundi, Chad, Malawi, Central African Republic, Liberia, Niger, Burkina Faso, Guinea-Bissau and Sierra Leone, with the group averaging under ~42% access against a global mean of 78.93%.

**Wealth does not cleanly predict renewable share.** The GDP-per-capita scatter shows renewable share spread across the full income range rather than rising with it — hydro-rich lower-income countries can post very high renewable shares, while wealthy fossil-endowed economies post low ones. Income is not the explanatory variable; resource endowment and policy are.

**Climate finance and access move together, but not proportionally.** Plotting total financial flows against average access reveals periods where flows rose while access gains flattened — the dual-axis view makes the decoupling visible, which a single-series chart would hide.

*\* India's renewable generation figure reflects its position in the ranked chart; the top four are the dominant generators.*

## Recommendations delivered

1. **Report low-carbon share, not renewable share, as the headline decarbonisation metric.** The 22.32% vs 36.80% gap materially changes how progress is judged.
2. **Target climate finance at the access bottom-10, not at the largest renewable markets.** The countries generating the most renewable electricity are also among the best-resourced; the ten countries with the worst access receive a fraction of the attention.
3. **Track absolute fossil displacement, not just renewable additions.** Growth in renewable TWh alongside flat renewable share is the signal that capacity is being *added* rather than *replaced*.
4. **Stop using GDP per capita as a proxy for transition readiness** — the scatter shows it has little predictive power.

## Skills demonstrated

- **Analytical:** longitudinal trend analysis across 21 years, share-vs-absolute decomposition, correlation testing (renewable share vs GDP), leader/laggard ranking, dual-axis relationship analysis
- **Data engineering:** star-schema design with a dedicated date dimension, a separated measures table, missing-data handling in international panel data
- **DAX:** percentage-of-total and weighted-average measures with correct filter context across year and country grain
- **BI/UX:** synchronised multi-slicer filtering with a clear-all reset, dense-but-legible executive canvas design

## Files in this folder

| File | Description |
|---|---|
| `Sustainable energy (2000-2020).pdf` | Static export of the full report |
| `Open with power bi or alternative visualization tools.pbix` | Power BI source file — full data model and measures |

## How to explore

Open the `.pbix` in **Power BI Desktop** (free) to use the year and country slicers and inspect the model and DAX directly. The PDF gives the static overview.
