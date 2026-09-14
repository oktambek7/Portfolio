# Wisabi Bank — ATM Transaction & Customer Demographics Analysis

**A three-page Power BI report analysing ~2 million ATM transactions worth ₦39bn across five Nigerian states, built to tell the bank where its ATM network is over-built, under-built, and slowing customers down.**

**Tools:** Power BI Desktop · Power Query (M) · DAX · Data modelling · Geospatial analytics

---

## Business problem

Wisabi Bank operates an ATM network across five Nigerian states. ATMs are expensive: each machine carries capital cost, cash-in-transit cost, maintenance and floor rent. Leadership needed evidence to answer:

- **Which ATMs and states are actually being used**, and which are idling?
- **When do customers transact**, so cash replenishment and servicing can be scheduled around demand instead of against it?
- **Who uses the network** — which age groups and occupations, and do they behave differently?
- **Which transaction types are slow**, creating queues and degrading the customer experience?

The strategic question underneath all of it: *where should the next ATM go, and which machines should be retired?*

## Dataset

| | |
|---|---|
| Grain | One row per ATM transaction |
| Volume | **~2M transactions**, **₦39bn** total value, **8,819 customers** |
| Period | Full year 2022 (Jan–Dec) |
| Geography | Lagos, Kano, Enugu, Rivers State, Federal Capital Territory |
| Key fields | Transaction ID, type, amount, duration, timestamp, ATM location, customer ID, age, gender, occupation, "Is Wisabi" flag |

## What I built

**1. Ingest & shape (Power Query).** Joined the transaction fact to customer, ATM-location and calendar dimensions, derived age bands from date of birth, extracted hour-of-day and day-of-week from the transaction timestamp, and flagged weekend activity.

**2. Model.** A star schema — transactions at the centre, with customer demographics, ATM/location and date as conformed dimensions. This is what allows a single slicer (e.g. transaction type) to filter demographics, timing and geography coherently.

**3. Measures (DAX).** `Number of customers`, `Transaction amount`, `Transaction count`, `Average duration`, `% Transactions`, and the headline **`Utilization Rate`** — the measure the whole capacity argument rests on.

**4. Report design.** Three navigable pages:
- **Home** — entry point and navigation
- **Overview** — transaction volume, value, timing and ATM utilisation by state, with a map of the network
- **Demography** — age, occupation and customer-type breakdowns of transaction behaviour

## What the analysis found

**Network utilisation is only 12.9%.** This is the single most important number in the report. Across roughly 2M transactions and ₦39bn in value, the ATM estate is running at a fraction of its capacity — the bank is paying for infrastructure it isn't using.

**Withdrawals dominate everything.**

| Transaction type | Share of transactions |
|---|---|
| Withdrawal | **55.5%** |
| Transfer | 22.2% |
| Balance Inquiry | 11.16% |
| Deposit | 11.14% |

Over half of all ATM interactions are cash-out events, which directly drives cash-replenishment logistics.

**Kano leads on value.** Kano records the highest transaction amount in the network, and the **Sabon Gari ATM alone carries 24.5%** of its state's activity — a single machine doing the work of several. Rivers State sits at the other end of the utilisation range.

**Younger customers are the highest-frequency users.** Transaction frequency peaks in the **16–25** and **26–35** age bands and declines steadily with age, with the over-65 group least active. The 16–25 cohort alone accounts for the largest transaction-count block in the network.

**Duration varies by transaction type, and by state.** Balance inquiries complete in ~2.5–3.0 units, transfers ~3.0–4.0, while **deposits and withdrawals take ~4.0–5.5** — roughly double. Kano is consistently the slowest state across every transaction type, which compounds its already-high volume into queueing.

**Demand is strongly time-shaped.** Transaction counts by hour show a clear daytime concentration with a pronounced midday peak, and monthly value moves between ₦3.0bn and ₦3.5bn against 160K–200K transactions — meaning average ticket size, not just volume, shifts through the year.

## Recommendations delivered

1. **Rebalance the estate, don't just expand it.** At 12.9% utilisation the constraint is distribution, not capacity. Retire or relocate low-utilisation machines and add capacity at proven-demand sites like Sabon Gari rather than opening new low-traffic locations.
2. **Size cash replenishment to the 55.5% withdrawal mix and the midday peak.** Replenishment scheduled off actual hourly demand curves reduces both stock-outs and idle cash.
3. **Investigate Kano's duration problem.** Highest value *and* slowest transactions is the worst combination for queueing — likely a hardware, connectivity or interface issue, and cheap to fix relative to a new ATM.
4. **Design for the 16–35 majority.** Since younger cohorts drive the bulk of volume, they are the right target for migrating routine balance inquiries (11% of all transactions, and the fastest to serve) to mobile — freeing ATM capacity for the cash transactions only an ATM can do.

## Skills demonstrated

- **Analytical:** utilisation and capacity analysis, time-series and hour-of-day demand profiling, demographic cohort segmentation, service-duration/bottleneck analysis, geospatial comparison
- **Data engineering:** multi-table joins into a star schema, date/time feature extraction, age banding, conformed dimensions
- **DAX:** ratio measures with controlled filter context, percentage-of-total calculations, cross-dimensional averages
- **Business translation:** converting a single utilisation metric into a concrete capital-allocation recommendation

## Files in this folder

| File | Description |
|---|---|
| `Wisabi Bank Project (1).pdf` | Full static export of all three report pages |

> **Note:** this project is currently published as the PDF export only. The `.pbix` source file is not in the repository.

## How to explore

Open the PDF for the complete three-page report.
