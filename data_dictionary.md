# Data dictionary

All four files are synthetic. Months are calendar months (1 = January). Amounts are in Australian dollars.

## income.csv
One row per month and income stream. Runs from January 2024 to April 2026.

| Column | Type | Description |
|---|---|---|
| Year | Integer | Calendar year |
| Month | Integer | 1–12 |
| Stream | Text | `Individual giving`, `Corporate partners`, `Community & events`, `Grants - restricted`, `Grants - unrestricted` |
| Amount | Currency | Income received in the month |

Total income is the sum of all streams. From 2025, grant income matches `grants.csv` (received amounts by decision month).

## targets.csv
One row per month and metric, from the annual budget. Covers full years, including future months.

| Column | Type | Description |
|---|---|---|
| Year | Integer | Calendar year |
| Month | Integer | 1–12 |
| Metric | Text | `Total income`, `Individual giving`, `Grants - restricted`, `Grants - unrestricted`, `Social engagement` |
| Target | Number | Target for that month |

## donor_engagement.csv
One row per month. Runs from January 2025 to April 2026.

| Column | Type | Description |
|---|---|---|
| Year, Month | Integer | Period |
| MajorDonors | Integer | Active major donors at month end |
| RegularGivers | Integer | Active regular (monthly) givers at month end |
| NewDonors | Integer | First-time donors in the month |
| RetentionRate | Decimal | Share of last month's active donors who gave again (0–1) |
| SocialEngagement | Integer | Likes, comments and shares across channels |
| SocialEngagementTarget | Integer | Monthly target (also in targets.csv) |
| SocialReach | Integer | Unique accounts reached |
| NewFollowers | Integer | Net new followers in the month |
| EmailSubscribers | Integer | Active email subscribers at month end |
| ConfirmedBequests | Integer | Bequests confirmed in the month |

## grants.csv
One row per grant application.

| Column | Type | Description |
|---|---|---|
| GrantID | Text | Unique ID |
| Funder | Text | Fictional funder name |
| DateApplied | Date | Submission date |
| AmountRequested | Currency | Amount applied for |
| Type | Text | `Restricted` (tied to a program) or `Unrestricted` (general operations) |
| Status | Text | `Successful`, `Unsuccessful` or `Pending` |
| DateDecided | Date | Date the funder's decision arrived (blank if pending) |
| AmountReceived | Currency | Amount awarded (0 if unsuccessful or pending) |
