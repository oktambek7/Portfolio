# London Bike Riders — Demand & Weather Analysis

**A Power BI dashboard over 19.9 million bike-share journeys that quantifies exactly how much weather, season and day-type drive ridership — the inputs a bike-share operator needs to plan fleet, staffing and rebalancing.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Time-series analysis

---

## Business problem

A bike-share operator's costs are fixed (fleet, docking stations, maintenance crews) while demand is highly variable. Under-supply on a warm clear Saturday means lost revenue and empty docks; over-supply in January means paying to maintain bikes nobody rides. The operator needed to know:

- **How much** does ridership actually vary by season — is it worth running a seasonal fleet?
- **How sensitive is demand to weather**, and which conditions justify pre-positioning or pulling back?
- **Do weekends behave differently from weekdays**, and in which conditions?
- What is the relationship between **temperature and volume**, so demand can be forecast from a weather feed?

## Dataset

| | |
|---|---|
| Grain | One row per time interval |
| Volume | **19,905,972 total rides** |
| Period | **January 2015 – December 2016** (24 months) |
| Key fields | Timestamp, ride count, real temperature (°C), weather code, season, weekend flag, humidity/wind |

## What I built

**1. Ingest & shape (Power Query).** Cleaned the raw time-series feed, built a **date hierarchy** off the timestamp to enable month/season aggregation, and decoded the numeric weather and season codes into readable categories (`Clear`, `Scattered clouds`, `Broken clouds`, `Rain`, `Cloudy`, `Snowfall`, `Rain with thunderstorm`) — a raw integer weather code is useless to a business reader.

**2. Model.** A time-series model with the timestamp-derived date hierarchy as the spine, and weather, season and weekend flags as filterable attributes.

**3. Measures (DAX).** `Total bike riders`, `Average of count` (ride volume normalised per period — essential for fair season comparison, since seasons don't contain equal numbers of observations), `Average of temp_real_C`, and `Sum of is_weekend` for day-type analysis.

**4. Report design.** A single-screen dashboard: a KPI card for total volume, a **ribbon chart** for average riders by season, a line chart tracking average temperature by month, a **pie chart** for ride distribution by weather, and a column chart of weekend activity segmented by weather condition.

## What the analysis found

**Ridership nearly doubles between the best and worst season.**

| Season | Avg riders |
|---|---|
| Summer | **1,464** |
| Autumn | 1,179 |
| Spring | 1,104 |
| Winter | **822** |

Summer runs **78% above winter**. Using average rather than total riders is what makes this comparison valid — it isolates genuine demand intensity from the number of observations in each season.

**Two-thirds of all riding happens in clear or lightly clouded conditions.**

| Weather | Share of rides |
|---|---|
| Clear | **35.90%** |
| Scattered clouds | 30.32% |
| Broken clouds | 21.32% |
| Rain | 7.67% |
| Cloudy | 4.67% |
| Snowfall | ~0.1% |
| Rain with thunderstorm | ~0.1% |

Clear and scattered-cloud conditions alone account for **66.2%** of all journeys. Rain collapses demand to 7.67%, and snow or thunderstorms effectively end it. Weather is not a marginal factor — it is the dominant short-term demand driver.

**Temperature tracks ridership closely.** Average monthly temperature runs from **6.2°C in February** to **19.3°C in July**, and the seasonal ride curve follows the same shape almost exactly:

| Month | Avg temp (°C) | | Month | Avg temp (°C) |
|---|---|---|---|---|
| January | 6.7 | | July | 19.3 |
| February | 6.2 | | August | 19.2 |
| March | 7.8 | | September | 16.6 |
| April | 10.2 | | October | 12.7 |
| May | 13.9 | | November | 10.1 |
| June | 16.7 | | December | 9.9 |

Because temperature and volume move together this tightly, **temperature is a usable single-variable predictor** for short-range demand forecasting — the operator can forecast tomorrow's fleet need from tomorrow's forecast.

**Weekend demand is concentrated in good weather.** Weekend volume by weather peaks sharply under clear conditions (~2,000) and falls away rapidly through the wetter categories — weekend riding is discretionary and leisure-driven, unlike weekday commuting which persists in worse conditions.

## Recommendations delivered

1. **Run a seasonal fleet.** With summer at 1,464 and winter at 822 average riders, maintaining peak fleet size through winter is ~44% over-provisioned. Scale the active fleet to season and use the winter window for maintenance.
2. **Drive rebalancing off the weather forecast, not the calendar.** Since 66% of rides occur in clear/scattered conditions, crews should pre-position bikes ahead of forecast clear days and stand down in rain — a cheap operational change with a direct utilisation payoff.
3. **Use temperature as the forecasting input.** The month-by-month temperature and ridership curves align closely enough to support a simple, explainable forecast model, without needing a complex multivariate approach.
4. **Target promotions at marginal-weather weekends.** Weekend demand is the most weather-elastic, so discounting is most effective on cloudy weekends where riders are undecided — not on clear days where they'd ride anyway, or in rain where nothing converts.

## Skills demonstrated

- **Analytical:** time-series and seasonality analysis, weather-elasticity quantification, average-vs-total normalisation (avoiding a common comparison fallacy), day-type segmentation, single-variable forecasting assessment
- **Data engineering:** timestamp parsing and date-hierarchy construction, categorical decoding of coded fields, cleaning a ~20M-row event feed
- **DAX:** aggregation and averaging measures with correct time-grain filter context
- **BI/UX:** single-screen dashboard design, ribbon chart for ranked seasonal comparison, distribution and trend visuals chosen to match the question

## Files in this folder

| File | Description |
|---|---|
| `London Bike Riders Analysis.pdf` | Static export of the dashboard |
| `Open with Power BI.pbix` | Power BI source file — full model and measures |

## How to explore

Open the `.pbix` in **Power BI Desktop** (free) to cross-filter between the season, weather and month visuals. The PDF shows the default view.
