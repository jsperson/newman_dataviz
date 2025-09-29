# SpaceX Launch Data - Star Schema for Power BI Analysis

## Data Structure
This dataset follows a **star schema** design with:
- **Fact Table**: `fact_launches.csv` - Contains launch events and metrics
- **Dimension Table 1**: `dim_launch_location.csv` - Launch and landing location details
- **Dimension Table 2**: `dim_launch_vehicle.csv` - Launch vehicle specifications

## Key Metrics for Power BI (DAX) Analysis

### 1. Success Metric: Rapid Reusability Score (0-100)
**Column**: `rapid_reusability_score` in fact_launches.csv

This metric represents SpaceX's success in advancing reusable rocket technology:
- **0**: Complete failure (no launch success)
- **10**: Launch successful but no reusability
- **15-30**: Attempted landing/recovery with various outcomes
- **35-50**: Successful component recovery and reuse
- **75**: Crewed missions (highest value due to human rating requirements)
- **85-100**: Starship test flights (cutting-edge technology demonstrations)

**Power BI DAX Measure Example**:
```DAX
Average Reusability Success = AVERAGE(fact_launches[rapid_reusability_score])
Reusability Trend =
    CALCULATE(
        AVERAGE(fact_launches[rapid_reusability_score]),
        DATESINPERIOD(fact_launches[launch_date_actual], LASTDATE(fact_launches[launch_date_actual]), -365, DAY)
    )
```

### 2. Improvement Opportunity Flag
**Column**: `improvement_opportunity_flag` in fact_launches.csv

Binary flag (0 or 1) indicating launches where improvement opportunities were identified:
- **1**: Launch had issues or failures requiring improvement
  - Failed launches
  - Failed landing attempts
  - Partial mission success
  - Early experimental flights
- **0**: Successful execution with minimal improvement needed

**Power BI DAX Measure Examples**:
```DAX
Improvement Rate =
    DIVIDE(
        COUNTROWS(FILTER(fact_launches, fact_launches[improvement_opportunity_flag] = 1)),
        COUNTROWS(fact_launches)
    )

Learning Curve Score =
    VAR ImprovementsByYear =
        SUMMARIZE(
            fact_launches,
            YEAR(fact_launches[launch_date_actual]),
            "Improvements", SUM(fact_launches[improvement_opportunity_flag])
        )
    RETURN AVERAGE(ImprovementsByYear[Improvements])
```

## Additional Analysis Opportunities

### Launch Delays Analysis
- `launch_delay_days`: Calculated as difference between planned and actual launch dates
- Useful for operational efficiency metrics

### Cost Efficiency Metrics
- `cost_million`: Launch cost in millions USD
- Can be analyzed against payload mass and reusability for ROI calculations

### Mission Complexity Score
Combine multiple factors:
- Crew missions (crew_count > 0)
- Orbit type (GTO/TLI more complex than LEO)
- Landing attempts and success
- Payload mass

## Date Relationships
Primary date field: `launch_date_actual` (YYYYMMDD format)
- Supports time intelligence functions in Power BI
- Can create date hierarchy (Year > Quarter > Month > Day)

## Sample Power BI Visualizations
1. **Reusability Evolution**: Line chart showing rapid_reusability_score over time
2. **Improvement Heat Map**: Calendar view of improvement_opportunity_flag
3. **Launch Success Dashboard**: KPIs for success rate, landing rate, reusability rate
4. **Vehicle Performance Matrix**: Compare vehicles by success metrics
5. **Location Efficiency**: Map visualization with launch success by location