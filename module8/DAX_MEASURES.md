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

*Last Updated: 2025-09-29*
*Data Source: fact_launches.csv (578 launches, 2006-2024)*