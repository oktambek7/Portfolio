# World-Class Cars — Performance & Pricing Behaviour Analysis (2025)

**A Power BI dashboard profiling 1,026 production vehicles across price, power, torque, speed and acceleration — built on a raw dataset where every performance figure arrived as unusable text and had to be engineered into numbers before a single chart could be drawn.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Calculated columns & binning · Gauge and matrix visuals

---

## Business problem

An automotive market-intelligence audience — a manufacturer benchmarking a new model, a dealer group planning inventory, or a buyer's guide publisher — needs to place any given car against the global field:

- **Where does a given price point actually sit** in the market? What does $50K buy versus $150K?
- **Which countries compete in which segments**, and is national reputation matched by the data?
- **What is the price of performance** — how much does an extra 100 hp or 50 km/h actually cost?
- **How concentrated is the ultra-premium tier**, and who owns it?
- Can a user filter by brand, country, fuel type, price band and seat count to find comparable vehicles instantly?

## Dataset

| | |
|---|---|
| Source | Cars Datasets 2025 |
| Grain | One row per vehicle model/variant |
| Volume | **1,026 vehicles** |
| Countries | Germany, Japan, USA, UK, France, Italy, Sweden, South Korea, India |
| Key fields | Company name, model, country, engine, CC/battery capacity, horsepower, torque, top speed, 0–100 km/h time, fuel type, seats, price |

## What I built

**1. The data engineering problem (Power Query).** This was the bulk of the work. The source dataset stored every performance metric as **text with embedded units** — top speed as `"420 km/h"`, acceleration as `"7.1 sec"`, torque as `"1600 Nm"`, price with currency symbols and thousands separators. None of it could be aggregated, sorted, or plotted.

I built a cleaning layer in Power Query that:
- parses numeric values out of unit-suffixed text into dedicated typed columns (`Total Speed - Copy`, `Cars Prices - Copy`, `Performance(0 - 100)KM/H`), preserving the original text columns for display
- standardises inconsistent brand casing (the raw feed mixes `Bugatti` and `LAMBORGHINI`)
- handles multi-value and ranged entries in the engine and capacity fields
- creates a **`Merged column`** concatenating brand and model into a single readable identifier for ranking visuals

**2. Model & derived fields.** On top of the cleaned data I built:
- **`Price Range`** — a binned classification splitting the field into `<$20k`, `$20k–$50k`, `$50k–$150k`, `$150k–$500k`, `>$500k`
- **`Car Price $500k> & $500k<`** — a binary ultra-premium flag for fast segment comparison
- **`selected value`** — a dynamic measure that reflects the user's current slicer selection back into titles and cards, so the report reads correctly in every filter state

**3. Measures (DAX).** `Total cars`, `AVG price`, average horsepower, `averag torque`, average and maximum top speed, and average 0–100 km/h time.

**4. Report design.** A single interactive canvas with **six slicers** (brand, country, fuel type, price range, seat count, plus a **clear-all-slicers** reset button), KPI cards, a country distribution column chart, an average-price-by-country bar chart, a **donut chart** for price-band composition, two **gauge visuals** for average speed and average acceleration against the field maximum, and a ranked matrix of the fastest cars with full spec detail.

## What the analysis found

**Headline position:**

| Metric | Value |
|---|---|
| Vehicles analysed | **1,026** |
| Average horsepower | **329 hp** |
| Average torque | **478 Nm** |
| Average top speed | **223 km/h** (field max 446) |
| Average 0–100 km/h | **7.11 sec** (field range 0–14) |

**Germany dominates on volume by a factor of three.**

| Country | Vehicles |
|---|---|
| Germany | **282** |
| UK | 81 |
| France | 64 |
| Italy | 33 |
| Sweden | 12 |

Germany fields more models than the UK, France, Italy and Sweden **combined**. Japan, USA, South Korea and India also feature, but German manufacturers have by far the broadest model coverage in the global field.

**Volume and price are inversely related by country.** Average price by country spans an enormous range — from **$947.07K** and **$612.48K** at the top down through **$285.56K**, **$97.08K**, **$82.93K**, **$56.23K**, **$55.71K**, and **$35.06K** to **$13.41K** at the bottom. The countries with the highest average prices are precisely those fielding the fewest models: low-volume, ultra-premium specialists (France's Bugatti, Italy's Lamborghini and Ferrari) versus high-volume mainstream producers. **National average price is a measure of business model, not of engineering quality** — a distinction that a naive country ranking would completely invert.

**The market is bimodal, not evenly spread.**

| Price band | Vehicles | Share |
|---|---|---|
| Largest band | 447 | **43.57%** |
| Second band | 361 | 35.19% |
| $150k–$500k | 82 | 7.99% |
| Remaining band | 105 | 10.23% |

Roughly **79% of the field sits in the two mainstream bands**, while the $150K–$500K tier holds just 7.99%. The market is a large mainstream mass and a very thin luxury spike, with a sparse middle — meaning a manufacturer positioning at $200K is competing in the *emptiest* part of the market.

**Bugatti owns the top-speed leaderboard outright.** Every one of the fastest vehicles is a Bugatti running the same **8.0L Quad-Turbo W16** engine:

| Model | Top speed |
|---|---|
| Chiron | **500 km/h** |
| Chiron Noire | 490 km/h |
| Bolide / Divo / Chiron Sport / Chiron Super Sport | 420 km/h |

One powertrain, one manufacturer, and a **500 km/h ceiling against a 223 km/h field average** — the top of the market is more than double the mean. Lamborghini's V12 flagships (Veneno Roadster, Sian, Aventador SVJ) follow at 350–356 km/h with 750–819 hp, showing that a 12-cylinder naturally-aspirated approach plateaus well below the quad-turbo tier.

**Performance scales with price far faster than linearly.** The Bugatti tier commands $3–18M for roughly double the field-average top speed, while the $150K–$500K tier delivers a large share of that performance. Beyond roughly the $500K mark, **price buys exclusivity and engineering prestige rather than proportional performance** — the gauges make this visible by showing how close the field average already sits to practically usable limits.

## Recommendations delivered

1. **Position new models in the mainstream bands or the ultra-premium spike — avoid the $150K–$500K gap.** At only 7.99% of the field it looks like white space, but thin occupancy here reflects weak demand between aspirational and attainable, not an unserved opportunity.
2. **Don't read national average price as a quality signal.** France's high average reflects Bugatti's low-volume, high-price model; it says nothing about French vehicles generally. Benchmark within price band, not within country.
3. **Benchmark against the 223 km/h / 329 hp / 7.11 sec field average, not against the halo cars.** The Bugatti tier is a statistical outlier that distorts any mean-based target.
4. **Use the price-per-performance curve for spec decisions.** Since returns on performance flatten sharply above the premium band, engineering spend beyond that point should be justified by brand positioning rather than by measurable capability gains.

## Skills demonstrated

- **Data engineering (the core of this project):** parsing numeric values out of unit-suffixed text fields, type conversion, handling inconsistent casing and multi-value entries, building clean typed columns alongside preserved display columns — turning an unusable raw dataset into an analysable model
- **Feature engineering:** custom binning into price bands, binary segment flags, concatenated composite identifiers
- **Analytical:** market segmentation and distribution analysis, price-performance curve analysis, country-level competitive benchmarking, outlier identification and its effect on averages
- **DAX:** dynamic selection-aware measures for responsive titles, multi-metric aggregations
- **BI/UX:** six-way synchronised slicer design with a reset control, gauge visuals framing averages against field maxima, matrix detail views

## Files in this folder

| File | Description |
|---|---|
| `World-class cars analysis.pdf` | Static export of the dashboard |
| `Interactive version in Power BI.pbix` | Power BI source file — full model, cleaning steps and measures |

## How to explore

Open the `.pbix` in **Power BI Desktop** (free) to use the six slicers and inspect the Power Query cleaning steps, which are the most substantial part of this project. The PDF shows the default unfiltered view.
