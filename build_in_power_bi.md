# Build it yourself in Power BI

About 45 minutes in Power BI Desktop.

## 1. Load the data
1. **Home > Get data > Text/CSV.** Load the four files in `data/`.
2. **Transform data.** In `income`, `targets` and `donor_engagement`, add a custom column `Date = #date([Year], [Month], 1)` and set its type to Date.
3. In `grants`, set `DateApplied` and `DateDecided` to Date, and the amounts to Fixed decimal.
4. **Close & Apply.**

## 2. Model
1. **Modeling > New table.** Create `Calendar` (see [dax_measures.md](dax_measures.md)) and mark it as the date table.
2. Relate `Calendar[Date]` one-to-many to `income[Date]`, `targets[Date]`, `donor_engagement[Date]` and `grants[DateApplied]`.
3. Add a second, **inactive** relationship from `Calendar[Date]` to `grants[DateDecided]`.

## 3. Measures
Create a `_Measures` table and add the measures in [dax_measures.md](dax_measures.md).

## 4. Visuals (per page)
- **Slicers:** `Calendar[Year]` and `Calendar[Month]` (tile style, multi-select), plus a "Clear all slicers" button.
- **Charts:** clustered column chart with `Month` on the x-axis, `Year` as the legend and the measure as values. For targets, use a line and clustered column chart with the target measure on the line.
- **Cards:** latest-month value with the change label underneath, a YTD card and an annual target card.

## 5. Theme
Blue header band, light-blue cards and one colour per year (2024 orange, 2025 light blue, 2026 navy) keep year-on-year comparisons easy to read.
