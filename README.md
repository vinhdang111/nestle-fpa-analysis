# Nestlé FP&A Case Study

An end-to-end FP&A (Financial Planning & Analysis) portfolio project built on **Nestlé's real, publicly disclosed financials** — segment/category data foundation, a driver-based budget model, actual-vs-budget variance analysis, an interactive dashboard, and a strategic business case.

## Why Nestlé

Nestlé is headquartered in Vevey, Switzerland, and reports segment revenue by geographic Zone (including Asia-Oceania-Africa, which covers Singapore) and by product category, alongside Organic Growth / Real Internal Growth (RIG) / Pricing effect disclosures — a rich, transparent disclosure structure that makes it a strong subject for a realistic FP&A analysis.

## Data sources

All figures are sourced directly from Nestlé's official Investor Relations press releases and audited Financial Statements (no estimates or synthetic data, except where explicitly labeled as a budget assumption):

- [Full-Year Results 2024 press release](https://www.nestle.com/sites/default/files/2025-02/full-year-results-press-release-2024-en.pdf) (13 Feb 2025) — FY2024 vs FY2023 segment/category data, and FY2025 guidance used for the Budget Model
- [Full-Year Results 2022 press release](https://www.nestle.com/sites/default/files/2023-02/2022-full-year-results-press-release-en.pdf) (16 Feb 2023) — FY2022 vs FY2021 (restated) segment/category data
- [2024 Financial Statements](https://www.nestle.com/sites/default/files/2025-02/2024-financial-statements-en.pdf) (13 Feb 2025) — full Consolidated Income Statement & Statement of Cash Flows, FY2024 vs FY2023
- [2022 Financial Statements](https://www.nestle.com/sites/default/files/2023-02/2022-financial-statements-en.pdf) (16 Feb 2023) — full Consolidated Income Statement & Statement of Cash Flows, FY2022 vs FY2021

Original PDF copies of these documents are kept in [`data/raw/`](data/raw) for traceability/reproducibility.

## Project status

| Step | Description | Status |
|---|---|---|
| 1 | Data Foundation + Full Income Statement & Cash Flow Statement — segment & category sales/UTOP (FY2021–FY2024), and the full line-by-line Consolidated Income Statement and Statement of Cash Flows, all built on Nestlé's actuals | ✅ Done |
| 2 | Driver-based FY2025 Budget Model — full line-item P&L + Cash Flow (~40 assumptions) | ✅ Done |
| 3 | Variance Analysis (Actual vs Budget, RIG/Pricing split) | 🔜 Next |
| 4 | Power BI Dashboard | ⬜ Not started |
| 5 | Business Case (strategic decision + sensitivity) | ⬜ Not started |

## Repo Structure

| Folder | Contents |
|---|---|
| `data/raw/` | Raw source PDFs from Nestlé's Investor Relations site (press releases & full Financial Statements) used to build the Excel workbook — see Data sources above |
| `excel_model/` | Excel workbook(s) — data foundation, P&L/CF, budget model |
| `powerbi/` | Power BI `.pbix` file and published dashboard link |
| `analysis/` | Variance analysis write-ups, business case notes |

## Excel workbook — `excel_model/Nestle_Data_Foundation.xlsx`

Sheets:

- **README** — sheet-by-sheet guide, sources, and key caveats (read this first)
- **1. Segment_Data** — Sales & Underlying Trading Operating Profit (UTOP) by reporting segment (5 geographic Zones + Nespresso + Nestlé Health Science + Other Businesses), FY2021–FY2024, with the organic growth bridge (RIG / Pricing / Net M&A / FX) for FY2022 and FY2024
- **1. Category_Data** — Sales & UTOP by product category (7 categories), FY2021–FY2024
- **1. Income_Statement** — Full Consolidated Income Statement, line by line (Sales, Other revenue, Cost of goods sold, Distribution expenses, Marketing & administration expenses, R&D costs, Other trading/operating income & expenses, Financial income/expense, Income from associates & joint ventures, Taxes, non-controlling interests), FY2021–FY2024 actuals, sourced from Nestlé's full Financial Statements documents. Every subtotal (UTOP, Trading Operating Profit, Operating Profit, Profit Before Taxes, Profit for the Year, Net Profit) is computed by formula from the disclosed line items above it — never hardcoded.
- **1. Cash_Flow_Statement** — Full Consolidated Statement of Cash Flows, line by line across Operating, Investing and Financing activities, plus the cash reconciliation and a Free Cash Flow memo, FY2021–FY2024 actuals. Every subtotal (Cash Generated from Operations, Operating/Investing/Financing Cash Flow) is computed by formula.
- **1. Overview_Analysis** — two additional views built entirely by formula from `1. Segment_Data`, `1. Income_Statement` and `2. Budget_FY2025_PL_CF`: a Group-level Sales Growth Bridge (RIG / Pricing / Net M&A / FX) for FY2022, FY2024 and the FY2025 Budget, and a Cost Structure & Margin trend as % of Sales, FY2021–FY2024 Actual plus FY2025 Budget. Each includes a long-format summary table.
- **2. Assumptions_FY2025** — ~40 line-item FY2025 budget drivers covering the full Income Statement and Cash Flow Statement (organic growth, FX, cost ratios, other trading/operating items, financial income/expense, associates income, tax rate, NCI, D&A, impairment, working capital, capex, investing and financing lines), each cited to Nestlé's own FY2025 guidance where available, or flagged as an explicit own-assumption with rationale where not. Yellow cells are the editable levers.
- **2. Budget_FY2025_PL_CF** — Full-detail FY2025 Budget vs FY2024 Actual, built at the same line-item level as `1. Income_Statement` and `1. Cash_Flow_Statement`, fully driven by `2. Assumptions_FY2025` and linked directly to `1. Income_Statement`/`1. Cash_Flow_Statement` for the FY2024 actuals column. Built entirely from guidance Nestlé disclosed on 13-Feb-2025 — **before** FY2025 actual results were known, deliberately without hindsight, so the eventual gap to real FY2025 results (Step 3) is a genuine variance, not a fitted one.

**Conventions:** blue text = hardcoded inputs sourced from the press releases above · black text = formulas · green text = cross-sheet links · yellow fill = editable budget assumptions. All formulas recalculate cleanly (0 errors).

**Known simplifications** (documented in-sheet):
- Segment/category UTOP figures sum to *more* than the Nestlé Group total UTOP — this is expected, since unallocated central costs and eliminations are not broken out by segment in the summary disclosures. The Total row in each sheet uses Nestlé's reported Group figure directly rather than a SUM of segments.
- `1. Income_Statement`, `1. Cash_Flow_Statement` and `2. Budget_FY2025_PL_CF` are all built at the same full statutory line-item level — every subtotal in all three is a formula over disclosed (or budgeted) line items, not a hardcoded figure. The only place the Budget is deliberately condensed is that "Other trading items" and "Other operating items" are budgeted net (income − expense combined) rather than split, since FY2025 guidance gives no separate visibility into these one-off/restructuring components — everything else follows the full P&L and Cash Flow structure (project scope is Income Statement + Cash Flow, not a full Balance Sheet).
- `1. Cash_Flow_Statement`'s "Free Cash Flow (derived: Operating CF − Capex − Intangible capex)" does not tie out exactly to "Free Cash Flow (as reported by Nestlé)" in any year — Nestlé's own FCF definition includes additional adjustments it doesn't separately disclose. Both figures are shown side by side rather than forcing a match.
- Nestlé restructured its reporting segments from 5 Zones to 3 Zones effective 1 January 2025 (Zone Americas, Zone Europe, Zone AOA, with Waters & Premium Beverages spun out as a separate global business). This project uses the as-originally-reported 5-Zone structure for FY2021–FY2024, since the restructuring postdates the data covered here. The FY2025 Budget is built at Group level only, so it isn't affected by this segment restructuring.
- The FY2025 Budget's ~40 assumptions are a mix of Nestlé's own disclosed guidance (organic sales growth, resulting UTOP margin "at or above 16.0%", no new share buyback) and explicit own-assumptions grounded in historical ratios/trends where the company gave no specific figure (e.g. cost ratios, capex intensity, working capital, financing flows) — every assumption's source is documented in the `2. Assumptions_FY2025` sheet.
- The Budget's ending cash balance rises materially versus FY2024 (CHF 5,558m → CHF 9,240m) mainly because two large FY2024 one-off outflows — the CHF 4,678m share buyback and ~CHF 2,130m of M&A/treasury-investment outflows — are assumed not to repeat in FY2025, per guidance. Operating Cash Flow itself is budgeted slightly *below* FY2024 (CHF 15,666m vs CHF 16,675m); this is flagged in-sheet so the cash build isn't mistaken for an operating improvement.
- In `1. Overview_Analysis`, the Sales Growth Bridge's "Reported Sales Growth" (RIG + Pricing + Net M&A + FX) is off by a few basis points from the actual reported YoY growth in `1. Segment_Data` — this is expected rounding noise from Nestlé's own disclosed bridge components (each rounded to one decimal place), not a formula error; a direct check row is included in-sheet.

## Next step

Step 3 — Variance Analysis: compare `2. Budget_FY2025_PL_CF` against Nestlé's actual FY2025 results, decomposing the sales variance into RIG (volume) vs. Pricing effects (the same framework Nestlé uses in its own disclosures), with written commentary.

## Author

Thanh Vinh Dang — [LinkedIn](https://linkedin.com/in/thanhvinhdang2001)
