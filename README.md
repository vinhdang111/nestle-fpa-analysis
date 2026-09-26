# Nestlé FP&A Case Study

An end-to-end FP&A (Financial Planning & Analysis) portfolio project built on **Nestlé's real, publicly disclosed financials** — segment/category data foundation, a driver-based budget model, actual-vs-budget variance analysis, an interactive dashboard, and a strategic business case.

Author: Thanh Vinh Dang ([LinkedIn](https://linkedin.com/in/thanhvinhdang2001) · [GitHub](https://github.com/vinhdang111))

## Why Nestlé

Nestlé is headquartered in Vevey, Switzerland, and reports segment revenue by geographic Zone (including Asia-Oceania-Africa, which covers Singapore) and by product category, alongside Organic Growth / Real Internal Growth (RIG) / Pricing effect disclosures — a rich, transparent disclosure structure that makes it a strong subject for a realistic FP&A analysis.

## Data sources

All figures are sourced directly from Nestlé's official Investor Relations press releases (no estimates or synthetic data):

- [Full-Year Results 2024 press release](https://www.nestle.com/sites/default/files/2025-02/full-year-results-press-release-2024-en.pdf) (13 Feb 2025) — FY2024 vs FY2023
- [Full-Year Results 2022 press release](https://www.nestle.com/sites/default/files/2023-02/2022-full-year-results-press-release-en.pdf) (16 Feb 2023) — FY2022 vs FY2021 (restated)
- [Half-Year Results 2024 press release](https://www.nestle.com/media/pressreleases/allpressreleases/half-year-results-2024) (25 Jul 2024) — H1-2024 vs H1-2023

## Project status

| Step | Description | Status |
|---|---|---|
| 1 | Data Foundation — segment & category sales/UTOP, FY2021–FY2024, H1/H2 phasing | ✅ Done |
| 2 | Group P&L + Free Cash Flow bridge (simplified, actuals) | ✅ Done |
| 3 | Driver-based Budget Model (FY2025 forecast) | 🔜 Next |
| 4 | Variance Analysis (Actual vs Budget, RIG/Pricing split) | ⬜ Not started |
| 5 | Power BI Dashboard | ⬜ Not started |
| 6 | Business Case (strategic decision + sensitivity) | ⬜ Not started |
| 7 | Packaging & portfolio embed | ⬜ Not started |

## Repository structure

```
nestle-fpa-analysis/
├── data/           # (reserved) raw/processed data exports, if split out of Excel later
├── excel_model/    # Excel workbook(s) — data foundation, P&L/CF, budget model
├── powerbi/        # (reserved) Power BI .pbix file and published dashboard link
├── analysis/       # (reserved) variance analysis write-ups, business case notes
└── README.md
```

## Excel workbook — `excel_model/Nestle_Data_Foundation.xlsx`

Sheets:

- **README** — sheet-by-sheet guide, sources, and key caveats (read this first)
- **Segment_Data** — Sales & Underlying Trading Operating Profit (UTOP) by reporting segment (5 geographic Zones + Nespresso + Nestlé Health Science + Other Businesses), FY2021–FY2024, with the organic growth bridge (RIG / Pricing / Net M&A / FX) for FY2022 and FY2024
- **Category_Data** — Sales & UTOP by product category (7 categories), FY2021–FY2024
- **H1_2024_Phasing** — H1/H2 split of segment sales & UTOP for 2023 and 2024 (H2 derived as FY − H1), the seasonality baseline for the budget model's quarterly phasing
- **Group_PL_CF** — Simplified Group Income Statement (Sales → UTOP → Profit Before Tax → Net Profit) and Free Cash Flow bridge, FY2021–FY2024, built on top of the reported Group totals

**Conventions:** blue text = hardcoded inputs sourced from the press releases above (cited in cell comments where applicable) · black text = formulas · green text = cross-sheet links. All formulas recalculate cleanly (0 errors).

**Known simplifications** (documented in-sheet):
- Segment/category UTOP figures sum to *more* than the Nestlé Group total UTOP — this is expected, since unallocated central costs and eliminations are not broken out by segment in the summary disclosures. The Total row in each sheet uses Nestlé's reported Group figure directly rather than a SUM of segments.
- The `Group_PL_CF` sheet is a simplified bridge (UTOP − Net Financial Expenses → PBT) that excludes restructuring/other trading items sitting between UTOP and Nestlé's full reported Trading/Operating Profit — it will not tie exactly to the full consolidated income statement, by design (project scope is Income Statement + Free Cash Flow, not a full Balance Sheet).
- FY2022's "Cash generated from operations" was disclosed on a different basis than FY2021/2023/2024 and is left blank (`n/a`) rather than mixed silently with the other years.
- Nestlé restructured its reporting segments from 5 Zones to 3 Zones effective 1 January 2025 (Zone Americas, Zone Europe, Zone AOA, with Waters & Premium Beverages spun out as a separate global business). This project uses the as-originally-reported 5-Zone structure for FY2021–FY2024, since the restructuring postdates the data covered here.

## Next step

Building a driver-based FY2025 Budget Model (P&L + Cash Flow) using Nestlé's own published FY2025 guidance (organic sales growth improving vs. 2024, UTOP margin ≥16.0%) as the basis for assumptions, phased using the H1/H2 baseline established in `H1_2024_Phasing`.
