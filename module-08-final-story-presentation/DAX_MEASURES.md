# DAX Measures for SpaceX Dashboard

## Launch Success Metrics

### Launch Success Rate %
```dax
Launch Success Rate % =
DIVIDE(
    COUNTROWS(
        FILTER(
            fact_launches,
            fact_launches[launch_success] = TRUE()
        )
    ),
    COUNTROWS(fact_launches),
    0
) * 100
```

**Description**: Calculates the percentage of successful launches out of all launches.
**Returns**: Percentage value (e.g., 98.6)
**Usage**: Display in KPI card or gauge visual

---

### Launch Failure Rate %
```dax
Launch Failure Rate % =
DIVIDE(
    COUNTROWS(
        FILTER(
            fact_launches,
            fact_launches[launch_success] = FALSE()
        )
    ),
    COUNTROWS(fact_launches),
    0
) * 100
```

**Description**: Calculates the percentage of failed launches out of all launches.
**Returns**: Percentage value (e.g., 1.4)
**Usage**: Display in KPI card or visual highlighting improvement areas

---

### Alternative: Success Rate % (Simpler)
```dax
Success Rate % =
DIVIDE(
    CALCULATE(COUNT(fact_launches[launch_id]), fact_launches[launch_success] = TRUE()),
    COUNT(fact_launches[launch_id]),
    0
) * 100
```

**Description**: Simpler version using CALCULATE and COUNT
**Returns**: Same as above

---

### Total Successful Launches
```dax
Total Successful Launches =
COUNTROWS(
    FILTER(
        fact_launches,
        fact_launches[launch_success] = TRUE()
    )
)
```

**Description**: Count of all successful launches
**Returns**: Integer count (e.g., 570)
**Usage**: Use in card visual or as part of other calculations

---

### Total Failed Launches
```dax
Total Failed Launches =
COUNTROWS(
    FILTER(
        fact_launches,
        fact_launches[launch_success] = FALSE()
    )
)
```

**Description**: Count of all failed launches
**Returns**: Integer count (e.g., 8)
**Usage**: Use in card visual or failure analysis page

---

## Related Metrics

### Landing Success Rate %
```dax
Landing Success Rate % =
DIVIDE(
    COUNTROWS(
        FILTER(
            fact_launches,
            fact_launches[landing_attempted] = TRUE() &&
            fact_launches[landing_success] = TRUE()
        )
    ),
    COUNTROWS(
        FILTER(
            fact_launches,
            fact_launches[landing_attempted] = TRUE()
        )
    ),
    0
) * 100
```

**Description**: Success rate for landing attempts only
**Returns**: Percentage value (e.g., 93.0)
**Verified Value**: 507 successful out of 545 attempts = 93.0%

---

### Reusability Rate %
```dax
Reusability Rate % =
DIVIDE(
    COUNTROWS(
        FILTER(
            fact_launches,
            fact_launches[booster_reused] = TRUE()
        )
    ),
    COUNTROWS(fact_launches),
    0
) * 100
```

**Description**: Percentage of launches using reused boosters
**Returns**: Percentage value (e.g., 83.7)
**Verified Value**: 484 missions with reused boosters out of 578 total = 83.7%

---

## Usage Notes

### Boolean Field Format
Ensure your CSV boolean fields are loaded correctly:
- Power BI typically recognizes TRUE/FALSE (case insensitive)
- May need to convert from text if imported as strings
- In Power Query: `Table.TransformColumnTypes(#"Previous Step", {{"launch_success", type logical}})`

### DIVIDE Function
The DIVIDE function is recommended over `/` operator because:
- Handles division by zero gracefully
- Third parameter (0) is the alternate result when denominator is zero
- Prevents errors in visuals

### Filtering TRUE() vs TRUE
Both `= TRUE()` and `= TRUE` work in Power BI
- `TRUE()` is a function that returns true
- `TRUE` is the boolean value
- Use whichever is consistent with your data model

### Performance Considerations
For large datasets, consider creating calculated columns instead of measures if:
- The calculation is used in multiple visuals
- The result doesn't change based on context
- You need to use it in slicers or filters

Example calculated column:
```dax
Success Flag = IF(fact_launches[launch_success] = TRUE(), "Success", "Failure")
```

---

## Verification Against CSV Data

Based on `fact_launches.csv` (578 launches):
- Total launches: 578
- Successful launches: 570
- Failed launches: 8
- Success rate: 570 / 578 = 98.6%
- Failure rate: 8 / 578 = 1.4%

Landing specific:
- Landing attempts: 545
- Successful landings: 507
- Landing success rate: 507 / 545 = 93.0%

Reusability:
- Launches with reused boosters: 484
- Reusability rate: 484 / 578 = 83.7%

---

## Year-over-Year (YoY) Metrics

### YoY Change in Launches
```dax
YoY Change in Launches =
VAR CurrentYearLaunches = COUNTROWS(fact_launches)
VAR PreviousYearLaunches =
    CALCULATE(
        COUNTROWS(fact_launches),
        DATEADD(fact_launches[launch_date_actual], -1, YEAR)
    )
RETURN
    CurrentYearLaunches - PreviousYearLaunches
```

**Description**: Calculates the difference in number of launches from previous year
**Returns**: Integer (e.g., +33 for 2024: 182 launches vs 149 in 2023)
**Usage**: Use in KPI card with year slicer

---

### YoY Change in Launches %
```dax
YoY Change in Launches % =
VAR CurrentYearLaunches = COUNTROWS(fact_launches)
VAR PreviousYearLaunches =
    CALCULATE(
        COUNTROWS(fact_launches),
        DATEADD(fact_launches[launch_date_actual], -1, YEAR)
    )
RETURN
    DIVIDE(
        CurrentYearLaunches - PreviousYearLaunches,
        PreviousYearLaunches,
        0
    ) * 100
```

**Description**: Calculates the percentage change in launches from previous year
**Returns**: Percentage (e.g., 22.1% for 2024: (182-149)/149 * 100)
**Usage**: Display as percentage change indicator

---

### Alternative: YoY Change (Using Year Column)
If you have a separate Year column or date hierarchy:

```dax
YoY Change in Launches (Simple) =
VAR CurrentYearLaunches =
    CALCULATE(
        COUNTROWS(fact_launches),
        YEAR(fact_launches[launch_date_actual]) = YEAR(TODAY())
    )
VAR PreviousYearLaunches =
    CALCULATE(
        COUNTROWS(fact_launches),
        YEAR(fact_launches[launch_date_actual]) = YEAR(TODAY()) - 1
    )
RETURN
    CurrentYearLaunches - PreviousYearLaunches
```

**Note**: This version is simpler but less flexible with time intelligence

---

### YoY Comparison Card Format
For a KPI card showing current year with YoY change:

```dax
Launches This Year =
CALCULATE(
    COUNTROWS(fact_launches),
    YEAR(fact_launches[launch_date_actual]) = YEAR(TODAY())
)
```

**Display Format**:
- Value: Launches This Year (182)
- Trend: YoY Change in Launches (+33)
- Or as text: "182 launches (+33 YoY)"

---

## Important Notes for Time Intelligence

### Date Table Requirement
For DATEADD and other time intelligence functions to work properly:
1. Create a Date table (or use Auto Date/Time feature)
2. Mark it as a Date table in Power BI
3. Create relationship between Date table and fact_launches[launch_date_actual]

### Creating a Date Table
```dax
Date Table =
ADDCOLUMNS(
    CALENDAR(DATE(2006, 1, 1), DATE(2025, 12, 31)),
    "Year", YEAR([Date]),
    "Month", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Quarter", "Q" & QUARTER([Date]),
    "Day of Week", FORMAT([Date], "dddd")
)
```

### Without Date Table (Alternative)
If you can't use time intelligence functions, use this approach:

```dax
YoY Change (No Date Table) =
VAR CurrentYear = YEAR(MAX(fact_launches[launch_date_actual]))
VAR CurrentYearLaunches =
    CALCULATE(
        COUNTROWS(fact_launches),
        YEAR(fact_launches[launch_date_actual]) = CurrentYear
    )
VAR PreviousYearLaunches =
    CALCULATE(
        COUNTROWS(fact_launches),
        YEAR(fact_launches[launch_date_actual]) = CurrentYear - 1
    )
RETURN
    CurrentYearLaunches - PreviousYearLaunches
```

---

## Verification Against CSV Data

YoY Changes by Year (most recent):
- 2024: 182 launches vs 2023: 149 launches = **+33** (22.1% increase)
- 2023: 149 launches vs 2022: 76 launches = **+73** (96.1% increase)
- 2022: 76 launches vs 2021: 49 launches = **+27** (55.1% increase)
- 2021: 49 launches vs 2020: 39 launches = **+10** (25.6% increase)
- 2020: 39 launches vs 2019: 12 launches = **+27** (225.0% increase)

---

*Last Updated: 2025-09-29*
*Data Source: fact_launches.csv (578 launches, 2006-2024)*