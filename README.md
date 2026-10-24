# NFP Fundraising Dashboards (Power BI)

Three Power BI dashboards for a not-for-profit's leadership team: **income performance**, **donor engagement** and **grants**. I originally built them for a Melbourne not-for-profit. This repository rebuilds them for a fictional organisation using **synthetic data**, so the design and logic can be shared without exposing any real donor or funding information.

> **All data in this repository is invented.** The figures, funders and trends were generated for demonstration. They do not represent any real organisation.

**Live interactive version:** [rajat-sidana-portfolio.netlify.app/#dashboards](https://rajat-sidana-portfolio.netlify.app/#dashboards)

---

## The dashboards

### 1. Performance
![Performance dashboard](screenshots/1_performance.png)

- Total income and individual donor income by month, compared across years
- Monthly target line from the annual budget
- KPI cards: latest month vs the same month last year (▲/▼ $ and %), year-to-date income, annual target
- Year and month slicers with a "Clear all slicers" button

### 2. Donor engagement
![Donor engagement dashboard](screenshots/2_donor_engagement.png)

- Major donors and regular givers by month, year on year
- New donors and monthly retention rate, with month-on-month change
- Social engagement vs target and social reach
- Other engagement: active email subscribers, new followers, confirmed bequests

### 3. Grants
![Grants dashboard](screenshots/3_grants.png)

- Grants applied and received by month
- Restricted and unrestricted grant income vs monthly targets
- YTD cards: value applied for, income received, annual target, grant success rate

---

## Questions the dashboards answer

| Audience | Question | Where |
|---|---|---|
| Executive Director | Are we on track against budget this year? | Performance: YTD vs annual target |
| Head of Fundraising | Did the EOFY and Christmas appeals land? | Performance: June and December vs prior years |
| Fundraising team | Are we growing our regular giving base? | Engagement: regular givers, retention |
| Comms | Is social activity turning into reach? | Engagement: social engagement vs reach |
| Grants officer | How much is in the pipeline, and how often do we win? | Grants: applied, received, success rate |

---

## Data model

```
Calendar (daily) ──< Income            (Year, Month, Stream, Amount)
        │      ──< Targets           (Year, Month, Metric, Target)
        │      ──< Donor_Engagement  (Year, Month, donor and social metrics)
        └──────< Grants              (DateApplied: active, DateDecided: inactive)
```

- A star schema around one `Calendar` table. Monthly tables get a `Date` column (first of the month) in Power Query.
- Grants relate to the calendar twice. Applications count by `DateApplied`; income and success count by `DateDecided`, switched on with `USERELATIONSHIP`.

Full details:
- [Data dictionary](docs/data_dictionary.md)
- [DAX measures](docs/dax_measures.md)
- [Build it yourself in Power BI](docs/build_in_power_bi.md)

---

## Repository contents

```
data/
  income.csv              monthly income by stream, Jan 2024 – Apr 2026
  targets.csv             monthly budget targets by metric
  donor_engagement.csv    monthly donor and social metrics, Jan 2025 – Apr 2026
  grants.csv              one row per grant application
docs/
  data_dictionary.md
  dax_measures.md
  build_in_power_bi.md
screenshots/
```

## Tools
Power BI Desktop · Power Query · DAX · Python (synthetic data generation)

## About
Built by **Rajat Sidana**, data and AI analyst in Melbourne.
[LinkedIn](https://www.linkedin.com/in/rajat-sidana-48063220b) · [Portfolio](https://rajat-sidana-portfolio.netlify.app)
