# Nestlé FP&A Case Study

An end-to-end FP&A (Financial Planning & Analysis) portfolio project built on **Nestlé's real, publicly disclosed financials** — segment/category data foundation, a driver-based budget model, actual-vs-budget variance analysis, an interactive dashboard, and a strategic business case.

## Why Nestlé

Nestlé is headquartered in Vevey, Switzerland, and reports segment revenue by geographic Zone (including Asia-Oceania-Africa, which covers Singapore) and by product category, alongside Organic Growth / Real Internal Growth (RIG) / Pricing effect disclosures — a rich, transparent disclosure structure that makes it a strong subject for a realistic FP&A analysis.

## Data sources

All figures are sourced directly from Nestlé's official Investor Relations press releases and audited Financial Statements (no estimates or synthetic data, except where explicitly labeled as a budget assumption):

- [Full-Year Results 2024 press release](https://www.nestle.com/sites/default/files/2025-02/full-year-results-press-release-2024-en.pdf) (13 Feb 2025) — FY2024 vs FY2023 segment/category data, and FY2025 guidance used for the Budget Model
- [Full-Year Results 2022 press release](https://www.nestle.com/sites/default/files/2023-02/2022-full-year-results-press-release-en.pdf) (16 Feb 2023) — FY2022 vs FY2021 (restated) segment/category data
- [Half-Year Results 2024 press release](https://www.nestle.com/media/pressreleases/allpressreleases/half-year-results-2024) (25 Jul 2024) — H1-2024 vs H1-2023
- [2024 Financial Statements](https://www.nestle.com/sites/default/files/2025-02/2024-financial-statements-en.pdf) (13 Feb 2025) — full Consolidated Income Statement & Statement of Cash Flows, FY2024 vs FY2023
- [2022 Financial Statements](https://www.nestle.com/sites/default/files/2023-02/2022-financial-statements-en.pdf) (16 Feb 2023) — full Consolidated Income Statement & Statement of Cash Flows, FY2022 vs FY2021

## Project status

| Step | Description | Status |
|---|---|---|
| 1 | Data Foundation + Full Income Statement & Cash Flow Statement — segment & category sales/UTOP (FY2021–FY2024), H1/H2 phasing, and the full line-by-line Consolidated Income Statement and Statement of Cash Flows, all built on Nestlé's actuals | ✅ Done |
| 2 | Driver-based FY2025 Budget Model (P&L + Cash Flow) | ✅ Done |
| 3 | Variance Analysis (Actual vs Budget, RIG/Pricing split) | 🔜 Next |
| 4 | Power BI Dashboard | ⬜ Not started |
| 5 | Business Case (strategic decision + sensitivity) | ⬜ Not started |
| 6 | Packaging & portfolio embed | ⬜ Not started |

## Repo Structure

| Folder | Contents |
|---|---|
| `data/` | Raw/processed data exports, if split out of Excel later |
| `excel_model/` | Excel workbook(s) — data foundation, P&L/CF, budget model |
| `powerbi/` | Power BI `.pbix` file and published dashboard link |
| `analysis/` | Variance analysis write-ups, business case notes |

## Excel workbook — `excel_model/Nestle_Data_Foundation.xlsx`

Sheets:

- **README** — sheet-by-sheet guide, sources, and key caveats (read this first)
- **Segment_Data** — Sales & Underlying Trading Operating Profit (UTOP) by reporting segment (5 geographic Zones + Nespresso + Nestlé Health Science + Other Businesses), FY2021–FY2024, with the organic growth bridge (RIG / Pricing / Net M&A / FX) for FY2022 and FY2024
- **Category_Data** — Sales & UTOP by product category (7 categories), FY2021–FY2024
- **H1_2024_Phasing** — H1/H2 split of segment sales & UTOP for 2023 and 2024 (H2 derived as FY − H1), the seasonality baseline for phasing
- **Income_Statement** — Full Consolidated Income Statement, line by line (Sales, Other revenue, Cost of goods sold, Distribution expenses, Marketing & administration expenses, R&D costs, Other trading/operating income & expenses, Financial income/expense, Income from associates & joint ventures, Taxes, non-controlling interests), FY2021–FY2024 actuals, sourced from Nestlé's full Financial Statements documents. Every subtotal (UTOP, Trading Operating Profit, Operating Profit, Profit Before Taxes, Profit for the Year, Net Profit) is computed by formula from the disclosed line items above it — never hardcoded.
- **Cash_Flow_Statement** — Full Consolidated Statement of Cash Flows, line by line across Operating, Investing and Financing activities, plus the cash reconciliation and a Free Cash Flow memo, FY2021–FY2024 actuals. Every subtotal (Cash Generated from Operations, Operating/Investing/Financing Cash Flow) is computed by formula.
- **Assumptions_FY2025** — FY2025 budget assumptions (organic growth, FX, UTOP margin, tax rate, FCF margin, share count), each cited to Nestlé's own FY2025 guidance where available, or flagged as an explicit own-assumption where guidance wasn't disclosed. Yellow cells are the editable scenario levers.
- **Budget_FY2025_PL_CF** — FY2025 Budget vs FY2024 Actual, fully driven by `Assumptions_FY2025` and linked directly to `Income_Statement`/`Cash_Flow_Statement` for the FY2024 actuals column. Built entirely from guidance Nestlé disclosed on 13-Feb-2025 — **before** FY2025 actual results were known, deliberately without hindsight, so the eventual gap to real FY2025 results (Step 3) is a genuine variance, not a fitted one.

**Conventions:** blue text = hardcoded inputs sourced from the press releases above · black text = formulas · green text = cross-sheet links · yellow fill = editable budget assumptions. All formulas recalculate cleanly (0 errors).

**Known simplifications** (documented in-sheet):
- Segment/category UTOP figures sum to *more* than the Nestlé Group total UTOP — this is expected, since unallocated central costs and eliminations are not broken out by segment in the summary disclosures. The Total row in each sheet uses Nestlé's reported Group figure directly rather than a SUM of segments.
- `Income_Statement` and `Cash_Flow_Statement` are the full statutory statements — every subtotal is a formula over disclosed line items, not a hardcoded figure. The simplification lives one layer up, in `Budget_FY2025_PL_CF`: its Budget column uses a condensed bridge (UTOP − Net Financial Expenses → PBT) that skips restructuring/other trading and other operating items, since FY2025 guidance wasn't detailed enough to budget those individually — a deliberate scope decision, not a data gap (project scope is Income Statement + Free Cash Flow, not a full Balance Sheet).
- `Cash_Flow_Statement`'s "Free Cash Flow (derived: Operating CF − Capex − Intangible capex)" does not tie out exactly to "Free Cash Flow (as reported by Nestlé)" in any year — Nestlé's own FCF definition includes additional adjustments it doesn't separately disclose. Both figures are shown side by side rather than forcing a match.
- Nestlé restructured its reporting segments from 5 Zones to 3 Zones effective 1 January 2025 (Zone Americas, Zone Europe, Zone AOA, with Waters & Premium Beverages spun out as a separate global business). This project uses the as-originally-reported 5-Zone structure for FY2021–FY2024, since the restructuring postdates the data covered here. The FY2025 Budget is built at Group level only, so it isn't affected by this segment restructuring.
- The FY2025 Budget's Organic Growth (3.0%), UTOP margin (16.5%), Net Financial Expenses growth (+5.0%), tax rate (24.0%), and FCF margin (11.5%) assumptions are a mix of Nestlé's own disclosed guidance ranges and explicit own-assumptions where the company gave no specific figure — every assumption's source is documented in the `Assumptions_FY2025` sheet.

## Next step

Step 3 — Variance Analysis: compare `Budget_FY2025_PL_CF` against Nestlé's actual FY2025 results, decomposing the sales variance into RIG (volume) vs. Pricing effects (the same framework Nestlé uses in its own disclosures), with written commentary.

## Author

Thanh Vinh Dang — [LinkedIn](https://linkedin.com/in/thanhvinhdang2001)
