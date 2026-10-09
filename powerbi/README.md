# Power BI Dashboard — Nestlé FY2025 Performance vs Budget

A Power BI dashboard (`Nestle_FPA_Dashboard.pbix`) built on the Excel model in [`excel_model/`](../excel_model). The data, data model, DAX measures and report pages are all inside this one file.

## How to open

1. Download **`Nestle_FPA_Dashboard.pbix`**: click the file above, then the **Download raw file** button (top right of the file view).
2. Open it in **Power BI Desktop** (free from Microsoft, Windows only).
3. That's it: the data is stored inside the file, so the dashboard opens straight away. No refresh, file paths or credentials are needed.

No Windows or Power BI Desktop? The same dashboard is published online: [open the live dashboard](https://app.powerbi.com/view?r=eyJrIjoiMGQwODRlMjEtNDY5Zi00M2MxLTgyYWUtZDEwZWM5OGY1ZmI1IiwidCI6Ijk2OTJhM2QzLTJhMDgtNGVjOC1hMGJkLTFkYjM1NWViNDIzMCIsImMiOjh9&pageName=overview) (no account needed).

**Map:** the Zone map uses the built-in **Azure Maps** visual (File → Options → Security → "Use Azure Maps visual", on by default).

## Report pages

The pages follow the project's own timeline: first the historical data foundation, then a budget built before FY2025 was known, then the FY2025 actuals, and finally the FY2026 reforecast.

| Page | Project step | What it shows |
|---|---|---|
| **1. Overview** | Step 1: Data foundation (FY2021–FY2024 actuals only) | Sales by geographic Zone on a map (one colour per Zone, bubble size = sales), sales by product category (donut), global businesses table, and a combined chart of sales (columns) and net profit margin (line). **Year buttons** switch the map, donut and table between FY2021 and FY2024 (FY2024 by default); the combined trend chart always shows all four years. FY2021 net profit includes the one-off gain on the partial disposal of Nestlé's L'Oréal stake |
| **2. Budget Scenarios** | Step 2: FY2025 Budget | Base-case KPIs with the Adverse–Favorable range; sales, net profit and free cash flow for FY2024 Actual vs the three FY2025 scenarios; the ten flexed assumptions; income statement by scenario |
| **3. Variance Analysis** | Step 3: FY2025 Actual vs Budget (Base) | FY2025 KPIs vs Budget; income statement variance table; net profit waterfall from Budget to Actual; sales growth bridge (RIG / Pricing / Net M&A / FX), Budget vs Actual |
| **4. Backtest** | Step 3: How good was the budget? | Drivers outside their tested range (count cards); sales, net profit and free cash flow for each scenario vs Actual; how far each driver moved relative to its Adverse/Favorable range; driver-level backtest table with colour-coded results |
| **5. FY2026 Reforecast** | Step 4: FY2026 Reforecast & Sensitivity | Two sliders (what-if parameters) for FX impact on sales (-7% to +1%) and COGS as % of sales (52.1% to 55.1%), starting at the Base values (FX -3.0%, Nestlé guidance of 23-Jul-2026; COGS 53.6%, H1-2026 level). They re-run the FY2026 income statement (FY2025 Actual, FY2026 Base, current scenario, change vs Base) and the FY2026 bar in the sales chart and in the net profit / net profit margin chart (FY2021–FY2026). The text above the charts shows the current scenario, its net profit and net profit margin, and the Base |

## Data model (star schema)

| Table | Content |
|---|---|
| `Dim_Year` | FY2021–FY2025 |
| `Dim_Scenario` | Budget - Adverse, Budget - Base, Budget - Favorable, Actual |
| `Dim_LineItem` | 53 income statement and cash flow lines, with statement, sort order, subtotal flag and net-profit-bridge sign |
| `Fact_PL`, `Fact_CF` | Year × Scenario × Line item values (CHF m): FY2021–FY2025 Actual and the three FY2025 Budget scenarios |
| `Fact_EPS` | Basic EPS by year and scenario |
| `Fact_GrowthBridge` | RIG, Pricing, Net M&A, FX, organic growth and reported growth for FY2022, FY2024, FY2025 Budget (Base) and FY2025 Actual |
| `Fact_Segment`, `Fact_Category` | Sales and UTOP by segment / category, FY2021–FY2024 (segments also carry type and approximate regional coordinates for the map) |
| `Fact_BudgetView` | Key metrics for FY2024 Actual and the three FY2025 Budget scenarios (from `2. Budget_FY2025_PL_CF`) |
| `Fact_Drivers` | The ten flexed drivers: Adverse / Base / Favorable assumption, FY2025 actual, result, and a range score (share of the Adverse or Favorable range used) |
| `RF_IS`, `RF_Drivers` | FY2026 reforecast: income statement lines (FY2025 Actual, FY2026 Base) and the Base drivers from `4. Assumptions_FY2026`; the measure `RF Scenario` recomputes every line from the two sliders |
| `RF_FX`, `RF_COGS` | What-if parameters behind the two sliders (calculated tables) |
| `RF_Years` | FY2021–FY2026F axis for the two reforecast charts |

Measures are grouped in display folders (the reforecast measures sit in **FY2026 Reforecast**): **Base** (Actual, Budget by scenario, variance), **Overview** (selected-year measures for the year buttons), **Budget scenarios**, **Budget vs Actual** (FY2025 variance measures and the net profit bridge), **Trend**, **KPI**, **Scenarios** and **Formatting** (colour measures for conditional formatting).

## Data lineage

Every value comes from `excel_model/Nestle_Data_Foundation.xlsx`, and ultimately from Nestlé's published press releases and Financial Statements (see the main README). Values were exported from the recalculated workbook with automatic checks: the FY2024 figures reconcile line by line to `2. Budget_FY2025_PL_CF` (52/52 lines), and the net profit bridge components sum exactly to the FY2025 net profit variance. The DAX reforecast reproduces `4. Reforecast_FY2026` exactly at the Base FX and COGS. The same tables are also saved as CSV files in [`data/`](data) for transparency.

## Notes

- Segment data uses the as-originally-reported 5-Zone structure (FY2021–FY2024); Nestlé reorganised its segments from FY2025, so segments are not shown for FY2025.
- Segment and category UTOP excludes unallocated central items, so segment totals exceed the Group UTOP.
- Design: pale cream report background with lighter cream cards, in the same Nestlé-oak palette as the Excel workbook (custom theme `Nestle_Cream_Theme.json`, validated against Microsoft's official Power BI theme schema).
- Independent analysis, not affiliated with or endorsed by Nestlé S.A.
