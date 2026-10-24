# DAX measures

These are the measures used in the sample model. Table names match the CSV files, and `Calendar` is the date table.

## Calendar table

```dax
Calendar =
ADDCOLUMNS (
    CALENDAR ( DATE ( 2024, 1, 1 ), DATE ( 2026, 12, 31 ) ),
    "Year", YEAR ( [Date] ),
    "Month No", MONTH ( [Date] ),
    "Month", FORMAT ( [Date], "mmm" )
)
```
Sort `Month` by `Month No`. Mark it as a date table.

In Power Query, add a `Date` column to Income, Targets and Donor_Engagement: `#date([Year], [Month], 1)`.

---

## Performance

```dax
Total Income = SUM ( Income[Amount] )

Individual Income =
CALCULATE ( [Total Income], Income[Stream] = "Individual giving" )

Total Income LY =
CALCULATE ( [Total Income], SAMEPERIODLASTYEAR ( 'Calendar'[Date] ) )

Total Income vs LY = [Total Income] - [Total Income LY]

Total Income vs LY % = DIVIDE ( [Total Income vs LY], [Total Income LY] )

Total Income YTD = TOTALYTD ( [Total Income], 'Calendar'[Date] )

Total Income Target =
CALCULATE ( SUM ( Targets[Target] ), Targets[Metric] = "Total income" )

Annual Target =
CALCULATE ( [Total Income Target], ALLEXCEPT ( 'Calendar', 'Calendar'[Year] ) )
```

### Latest-month cards
These cards show the most recent month with data inside the current slicer selection.

```dax
Last Month With Data =
MAXX (
    FILTER ( ALLSELECTED ( 'Calendar'[Date] ), [Total Income] > 0 ),
    'Calendar'[Date]
)

Latest Month Income =
VAR LastDate = [Last Month With Data]
RETURN
    CALCULATE ( [Total Income], REMOVEFILTERS ( 'Calendar' ), 'Calendar'[Date] = LastDate )

Latest Month Income LY =
VAR LastDate = [Last Month With Data]
RETURN
    CALCULATE ( [Total Income], REMOVEFILTERS ( 'Calendar' ), 'Calendar'[Date] = EDATE ( LastDate, -12 ) )

Latest Month Card Title =
FORMAT ( [Last Month With Data], "mmmm yyyy" ) & " Total Income"

Latest Month Change Label =
VAR Chg = [Latest Month Income] - [Latest Month Income LY]
VAR Pct = DIVIDE ( Chg, [Latest Month Income LY] )
RETURN
    IF ( Chg >= 0, "▲ ", "▼ " )
        & FORMAT ( Chg, "$#,##0;-$#,##0" ) & "  "
        & FORMAT ( Pct, "+0.0%;-0.0%" ) & " from last year"
```

---

## Donor engagement

```dax
Major Donors = SUM ( Donor_Engagement[MajorDonors] )
Regular Givers = SUM ( Donor_Engagement[RegularGivers] )
New Donors = SUM ( Donor_Engagement[NewDonors] )

New Donors PM =
CALCULATE ( [New Donors], DATEADD ( 'Calendar'[Date], -1, MONTH ) )

New Donors MoM Label =
VAR Chg = [New Donors] - [New Donors PM]
RETURN
    SWITCH (
        TRUE (),
        Chg = 0, "No change from last month",
        Chg > 0, "▲ +" & Chg & " from last month",
        "▼ " & Chg & " from last month"
    )

Retention Rate = AVERAGE ( Donor_Engagement[RetentionRate] )

Social Engagement = SUM ( Donor_Engagement[SocialEngagement] )

Social Engagement Target =
CALCULATE ( SUM ( Targets[Target] ), Targets[Metric] = "Social engagement" )

Email Subscribers =
CALCULATE ( SUM ( Donor_Engagement[EmailSubscribers] ), LASTNONBLANK ( 'Calendar'[Date], [Social Engagement] ) )
```

---

## Grants

Grants relate to `Calendar` on `DateApplied` (active) and `DateDecided` (inactive).

```dax
Grants Applied # = COUNTROWS ( Grants )

Grants Applied $ = SUM ( Grants[AmountRequested] )

Grants Decided # =
CALCULATE (
    COUNTROWS ( Grants ),
    Grants[Status] <> "Pending",
    USERELATIONSHIP ( Grants[DateDecided], 'Calendar'[Date] )
)

Grants Received # =
CALCULATE (
    COUNTROWS ( Grants ),
    Grants[Status] = "Successful",
    USERELATIONSHIP ( Grants[DateDecided], 'Calendar'[Date] )
)

Grant Income =
CALCULATE (
    SUM ( Grants[AmountReceived] ),
    USERELATIONSHIP ( Grants[DateDecided], 'Calendar'[Date] )
)

Restricted Grant Income = CALCULATE ( [Grant Income], Grants[Type] = "Restricted" )
Unrestricted Grant Income = CALCULATE ( [Grant Income], Grants[Type] = "Unrestricted" )

Grant Income YTD = TOTALYTD ( [Grant Income], 'Calendar'[Date] )

Grant Success Rate YTD =
DIVIDE (
    TOTALYTD ( [Grants Received #], 'Calendar'[Date] ),
    TOTALYTD ( [Grants Decided #], 'Calendar'[Date] )
)
```
