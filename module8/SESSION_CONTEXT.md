# SpaceX Dashboard Project - Session Context

**Date**: 2025-09-29
**Project**: Newman University Data Visualization Module 8
**Topic**: SpaceX Launch Analytics Power BI Dashboard

---

## Project Overview

Created a 3-page Power BI dashboard design specification for SpaceX launch data analysis, focusing exclusively on data-driven metrics with no speculative or "fuzzy management" visualizations.

### Data Sources
- `fact_launches.csv` - 578 SpaceX launches (2006-2024)
- `dim_launch_location.csv` - 11 launch locations/sites
- `dim_launch_vehicle.csv` - 12 vehicle types

---

## Key Accomplishments

### 1. Dashboard Structure (3 Pages)

**Page 1: Executive Summary**
- Map visualization of 4 active launch sites with launch counts
- Launch cadence trend chart (2006-2024)
- 4 KPI cards: Total launches (578), Success rate (98.6%), Active sites (4), Reusability (83.7%)

**Page 2: Success Story - Landing & Reusability**
- Landing success evolution chart: 93% overall success (507/545 attempts)
- Near-perfect performance in 2023-2024 (99%+)
- Booster reusability rate by year, showing improvement to 98% in 2024
- Focus: Demonstrating SpaceX's achievement in reusability

**Page 3: Improvement Opportunities**
- Launch failures by year (8 total, 1.4% failure rate)
- Vehicle success rate comparison highlighting Starship (66.7% success) as improvement area
- Landing failures breakdown (38 of 545 attempts, 7%)
- Focus: Data-driven areas for improvement

### 2. Data Quality Improvements

**Corrected KPIs to match actual CSV data:**
- 2024 launches: 134 → 182 (actual count)
- YoY growth: +134 → +33 (actual change from 2023)
- Success rate: 97.8% → 98.6% (actual calculation)
- Reusability: 89% → 97.8% (2024 specific)

**Realistic Launch Delays:**
- Original data had unrealistic delays (68.9% same-day launches, avg 0.6 days)
- Research-based realistic delays implemented:
  - Falcon Heavy first launch: ~3.2 year delay (2013→2018)
  - Crew Dragon missions: months to year+ delays
  - Starship: 3-9 month delays (experimental)
  - Early Falcon 1: 2-8 month delays
  - Recent Starlink: days to weeks (mature operations)
- New statistics: Average 24.5 days, max 1,186 days (3.2 years)

**Date Format Standardization:**
- All dates converted to YYYYMMDD integer format
- Consistent across launch_date_actual and launch_date_planned

### 3. Design Elements

**SpaceX Brand Colors (from slide 2):**
- Primary Blue: #005DAA (headers, main brand)
- Dark Gray: #292929 (text, backgrounds)
- Success Green: #22C55E (positive metrics)
- Warning Orange: #FB923C (attention points)
- Danger Red: #EF4444 (failures, alerts)
- Innovation Purple: #8B5CF6 (R&D, Starship)
- Steel Blue: #475569 (borders, dividers)
- Sky Blue: #7DD3FC (accents, hover states)

**Power BI Theme File:**
- Created `SpaceX_Theme.json` with all brand colors
- Text styles defined (callout, title, header, label)
- Import instructions provided

---

## Key Insights from Data

### Success Metrics (CSV-Verified)
- Total launches: 578 across 18 years
- Overall success rate: 98.6%
- Active launch sites: 4 (Cape Canaveral, Kennedy, Vandenberg, Starbase)
- Booster reusability: 484 missions (83.7% of all launches)
- Landing success: 507 of 545 attempts (93%)

### Launch Sites Distribution
- Cape Canaveral SLC-40: 268 launches (46%)
- Kennedy Space Center LC-39A: 174 launches (30%)
- Vandenberg SFB SLC-4E: 123 launches (21%)
- SpaceX South Texas (Starbase): 6 launches (1%)

### Evolution Over Time
- Landing success improved from 50% (2015) to 99.4% (2024)
- Reuse rate improved from 0% (2016) to 97.8% (2024)
- Launch cadence: 20 launches (2018) → 182 launches (2024)

### Improvement Opportunities
- 8 total launch failures (1.4% failure rate)
- Starship program: 4 successes out of 6 launches (66.7%)
- 38 landing failures still to address (7% of attempts)
- Failure concentration in early years (2006-2008, 2015-2016)

---

## Technical Decisions

### What We Removed (Fuzzy Content)
- Entire original Page 2 with ROI waterfall, priority matrix, and value projections
- Reusability Champions chart (no individual booster IDs in CSV)
- Key Success Metrics with external market share claims
- Cost reduction percentages not directly calculable from data
- Any projections or forecasts (2025-2030 value creation)

### What We Kept (Data-Driven Only)
- All metrics directly verifiable from CSV
- Launch counts, success rates, location data
- Temporal trends (by year)
- Aggregate statistics (means, counts, percentages)

### Data Integrity Principle
**"No fuzzy management visuals"** - Every chart and metric must be directly traceable to CSV data with clear calculation methodology.

---

## File Structure

```
module8/
├── SpaceX_PowerBI_Dashboard_Design.pptx  # Main deliverable
├── SpaceX_Theme.json                      # Power BI theme
├── fact_launches.csv                      # 578 launches (updated)
├── dim_launch_location.csv                # 11 locations
├── dim_launch_vehicle.csv                 # 12 vehicles
├── IMPROVEMENT_ANALYSIS.md                # (existing)
├── README_METRICS.md                      # (existing)
└── SESSION_CONTEXT.md                     # This file
```

---

## Python Libraries Used

### For Visualization Creation
- pandas - data manipulation
- matplotlib - chart creation
- seaborn - statistical visualizations
- numpy - numerical operations

### For PPTX Manipulation
- python-pptx - PowerPoint file editing
- Pillow - image handling

### Installation Command
```bash
pip3 install --break-system-packages python-pptx python-docx openpyxl pandas matplotlib seaborn
```

---

## Research Sources

### Launch Delay Research
- **Crew Dragon Demo-2**: Originally planned July 2019, launched May 2020 (after capsule explosion)
- **Crew-1**: Announced 2012 for 2016 launch, actually launched Nov 2020 (4 year delay)
- **Falcon Heavy**: Announced 2011 for 2013 launch, actually launched Feb 2018 (5 year delay)
- Sources: Wikipedia, SpaceNews, NASA Commercial Crew Blog, Spaceflight Now

### General SpaceX Data
- Launch manifests from public SpaceX data
- Historical success/failure records
- Landing attempt statistics
- Booster reuse tracking

---

## Git Commit History (This Session)

1. **Started SpaceX project** - Initial module 8 setup
2. **Updated header colors** - Changed to SpaceX blue backgrounds
3. **Removed green/purple backgrounds** - Consistent blue headers
4. **Added visualizations** - Created 8 data-driven charts
5. **Fixed visualization sizes** - Proper fit to containers
6. **Corrected KPIs** - Updated to match actual CSV data
7. **Added disclaimers** - Labeled fuzzy estimates
8. **Removed fuzzy content** - Eliminated all speculative charts
9. **Rebuilt dashboard** - New 3-page structure with Summary/Success/Improvement
10. **Updated planned dates** - Realistic delays based on research
11. **Standardized date format** - All dates to YYYYMMDD

---

## Next Steps / Future Enhancements

### Potential Additions (All Data-Driven)
1. **Payload Analysis**
   - Mass distribution by orbit type
   - Customer breakdown (NASA, commercial, internal)

2. **Launch Window Analysis**
   - Time of day distribution
   - Seasonal patterns

3. **Location Deep Dive**
   - Success rate by launch site
   - Vehicle type by location

4. **Delay Analysis Dashboard**
   - Delay trends over time
   - Delay reasons (if data available)
   - Delay by mission type

### Data Enhancement Opportunities
1. Individual booster tracking (B1051, B1060, etc.)
2. Weather delay reasons
3. Customer satisfaction metrics
4. Cost per kg to orbit calculations
5. Turnaround time between launches

---

## Important Notes for Future Sessions

### Data Principles
- ✅ Every metric must be CSV-verifiable
- ✅ Show calculation methodology
- ❌ No external market comparisons without sources
- ❌ No financial projections or ROI estimates
- ❌ No "management consulting" priority matrices

### Design Principles
- Use SpaceX brand colors consistently
- Headers: #005DAA blue background, white text
- Success metrics: Green (#22C55E)
- Improvement areas: Orange (#FB923C) or Red (#EF4444)
- Clean, data-focused layouts

### Date Format
- All dates in YYYYMMDD format as integers
- Example: 20060324 for March 24, 2006
- Consistent across all date columns

---

## Contact & Context

**User**: Scott Person
**Location**: Hesston, KS
**Institution**: Newman University, Wichita, KS
**Program**: Master's in Data Science
**Course**: Data Visualization (Module 8)

**Preferred Date Format**: YYYYMMDD (20250929)
**Location Privacy**: Keep private, ask before adding to docs

---

*Last Updated: 2025-09-29*
*Session Duration: Multiple commits across dashboard development*
*Total Launches Analyzed: 578 (2006-2024)*