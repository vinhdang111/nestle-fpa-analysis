# Nestlé FP&A Case Study

An end-to-end FP&A (Financial Planning & Analysis) portfolio project built on **Nestlé's real, publicly disclosed financials** — segment/category data foundation, a driver-based budget model with Adverse / Base / Favorable scenarios, actual-vs-budget variance analysis, an interactive dashboard, and a strategic business case.

## Why Nestlé

Nestlé is headquartered in Vevey, Switzerland, and reports segment revenue by geographic Zone and by product category, alongside Organic Growth / Real Internal Growth (RIG) / Pricing effect disclosures — a rich, transparent disclosure structure that makes it a strong subject for a realistic FP&A analysis.

## Data sources

All figures are sourced directly from Nestlé's official Investor Relations press releases and audited Financial Statements (no estimates or synthetic data, except where explicitly labeled as a budget assumption):

- [Full-Year Results 2024 press release](https://www.nestle.com/sites/default/files/2025-02/full-year-results-press-release-2024-en.pdf) (13 Feb 2025) — FY2024 vs FY2023 segment/category data, and FY2025 guidance used for the Budget Model
- [Full-Year Results 2022 press release](https://www.nestle.com/sites/default/files/2023-02/2022-full-year-results-press-release-en.pdf) (16 Feb 2023) — FY2022 vs FY2021 (restated) segment/category data
- [2024 Financial Statements](https://www.nestle.com/sites/default/files/2025-02/2024-financial-statements-en.pdf) (13 Feb 2025) — full Consolidated Income Statement & Statement of Cash Flows, FY2024 vs FY2023
- [2022 Financial Statements](https://www.nestle.com/sites/default/files/2023-02/2022-financial-statements-en.pdf) (16 Feb 2023) — full Consolidated Income Statement & Statement of Cash Flows, FY2022 vs FY2021
- [Full-Year Results 2025 press release](https://www.nestle.com/sites/default/files/2026-02/full-year-results-press-release-2025-en.pdf) (19 Feb 2026) — FY2025 vs FY2024 Sales, organic growth (RIG/Pricing split), UTOP, Net Profit, EPS, Free Cash Flow, Net Debt, dividend and FY2026 guidance, used in the Variance Analysis
- [2025 Financial Statements](https://www.nestle.com/sites/default/files/2026-02/financial-statements-2025-en.pdf) (19 Feb 2026) — full Consolidated Income Statement & Statement of Cash Flows, FY2025 vs FY2024, used for the Variance Analysis

Original PDF copies of these documents are kept in [`data/raw/`](data/raw) for traceability/reproducibility.

## Project status

| Step | Description | Status |
|---|---|---|
| 1 | Data Foundation + Full Income Statement & Cash Flow Statement — segment & category sales/UTOP (FY2021–FY2024), and the full line-by-line Consolidated Income Statement and Statement of Cash Flows, all built on Nestlé's actuals | ✅ Done |
| 2 | Driver-based FY2025 Budget Model — full line-item P&L + Cash Flow (~40 assumptions) under Adverse / Base / Favorable scenarios | ✅ Done |
| 3 | Variance Analysis — FY2025 Actual vs the Base Budget, with a RIG/Pricing/Net M&A/FX sales bridge, full Income Statement & Cash Flow variance, where Actual landed within the scenario range, and a backtest of each flexed driver | ✅ Done |
| 4 | Power BI Dashboard | 🔜 Next |
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

- **Cover** — executive summary page: FY2025 Actual vs Budget KPIs (all formula-linked to `3. Variance_Analysis`), key message, and a clickable table of contents
- **README** — sheet-by-sheet guide, sources, and key caveats (read this first)
- **1. Segment_Data** — Sales & Underlying Trading Operating Profit (UTOP) by reporting segment (5 geographic Zones + Nespresso + Nestlé Health Science + Other Businesses), FY2021–FY2024, with the organic growth bridge (RIG / Pricing / Net M&A / FX) for FY2022 and FY2024
- **1. Category_Data** — Sales & UTOP by product category (7 categories), FY2021–FY2024
- **1. Income_Statement** — Full Consolidated Income Statement, line by line (Sales, Other revenue, Cost of goods sold, Distribution expenses, Marketing & administration expenses, R&D costs, Other trading/operating income & expenses, Financial income/expense, Income from associates & joint ventures, Taxes, non-controlling interests), FY2021–FY2024 actuals, sourced from Nestlé's full Financial Statements documents. Every subtotal (UTOP, Trading Operating Profit, Operating Profit, Profit Before Taxes, Profit for the Year, Net Profit) is computed by formula from the disclosed line items above it — never hardcoded.
- **1. Cash_Flow_Statement** — Full Consolidated Statement of Cash Flows, line by line across Operating, Investing and Financing activities, plus the cash reconciliation and a Free Cash Flow memo, FY2021–FY2024 actuals. Every subtotal (Cash Generated from Operations, Operating/Investing/Financing Cash Flow) is computed by formula.
- **1. Overview_Analysis** — two additional views built entirely by formula from `1. Segment_Data`, `1. Income_Statement` and `2. Budget_FY2025_PL_CF`: a Group-level Sales Growth Bridge (RIG / Pricing / Net M&A / FX) for FY2022, FY2024 and the FY2025 Budget, and a Cost Structure & Margin trend as % of Sales, FY2021–FY2024 Actual plus FY2025 Budget. Each includes a long-format summary table.
- **2. Assumptions_FY2025** — ~40 line-item FY2025 budget drivers covering the full Income Statement and Cash Flow Statement, in three columns: **Adverse / Base / Favorable**. Base follows Nestlé's own FY2025 guidance where available, or an explicit own-assumption with documented rationale where not. Adverse and Favorable flex ten key drivers (RIG, FX, COGS %, Marketing & Admin %, other trading items, net financial expense, associates income, tax rate, working capital, capex) using ranges sized from Nestlé's FY2021–FY2024 history; every other driver is held at Base. Yellow cells are the editable levers.
- **2. Budget_FY2025_PL_CF** — Full-detail FY2025 Budget under the Adverse, Base and Favorable scenarios, side by side with FY2024 Actual, built at the same line-item level as `1. Income_Statement` and `1. Cash_Flow_Statement` and fully driven by `2. Assumptions_FY2025`. Built entirely from information Nestlé disclosed on 13-Feb-2025 — **before** FY2025 actual results were known, deliberately without hindsight, so the gap to real FY2025 results (Step 3) is a genuine variance, not a fitted one. The Base scenario is the plan Step 3 measures against.
- **3. Variance_Analysis** — FY2025 Actual versus the Base Budget: (A) a Group Sales Growth Bridge decomposing Budget vs Actual into RIG (Real Internal Growth), Pricing, Organic Growth, Net M&A and FX — the same framework Nestlé uses in its own disclosures; (B) a full line-item Income Statement variance (CHF m and %); (C) a full line-item Cash Flow variance; (D) where Actual landed versus the Adverse / Base / Favorable scenarios; (E) a backtest of each flexed driver against its scenario range; and (F) written commentary on what drove the variance. All FY2025 Actual figures are hardcoded (blue) inputs sourced from Nestlé's FY2025 Full-Year Results press release and Financial Statements (19-Feb-2026).

**Conventions:** blue text = hardcoded inputs sourced from the press releases above · black text = formulas · green text = cross-sheet links · yellow fill = editable budget assumptions. All formulas recalculate cleanly (0 errors).

**Known simplifications** (documented in-sheet):
- Segment/category UTOP figures sum to *more* than the Nestlé Group total UTOP — this is expected, since unallocated central costs and eliminations are not broken out by segment in the summary disclosures. The Total row in each sheet uses Nestlé's reported Group figure directly rather than a SUM of segments.
- `1. Income_Statement`, `1. Cash_Flow_Statement` and `2. Budget_FY2025_PL_CF` are all built at the same full statutory line-item level — every subtotal in all three is a formula over disclosed (or budgeted) line items, not a hardcoded figure. The only place the Budget is deliberately condensed is that "Other trading items" and "Other operating items" are budgeted net (income − expense combined) rather than split, since FY2025 guidance gives no separate visibility into these one-off/restructuring components — everything else follows the full P&L and Cash Flow structure (project scope is Income Statement + Cash Flow, not a full Balance Sheet).
- `1. Cash_Flow_Statement`'s "Free Cash Flow (derived: Operating CF − Capex − Intangible capex)" does not tie out exactly to "Free Cash Flow (as reported by Nestlé)" in any year — Nestlé's own FCF definition includes additional adjustments it doesn't separately disclose. Both figures are shown side by side rather than forcing a match.
- Nestlé restructured its reporting segments from 5 Zones to 3 Zones effective 1 January 2025 (Zone Americas, Zone Europe, Zone AOA, with Nestlé Health Science, Nespresso and Waters & Premium Beverages as separate global businesses) — now confirmed in Nestlé's FY2025 results. This project uses the as-originally-reported 5-Zone structure for FY2021–FY2024, since the restructuring postdates the data covered here. The FY2025 Budget and the Variance Analysis (Step 3) are both built at Group level only, so neither is affected by this segment restructuring — a zone-level variance is deliberately not attempted, since it would not be an apples-to-apples comparison.
- The Budget's 3.0% organic growth is split into RIG (1.5%) and Pricing (1.5%) as an own assumption — Nestlé's FY2025 guidance only covered organic growth as a whole. The split uses only information available on 13-Feb-2025 (H2-2024 RIG momentum, "selectively investing in price") and leaves the 3.0% total unchanged.
- The Step 3 Variance Analysis shows FY2025 was primarily an FX story: actual organic sales growth (3.5%) beat the Budget's 3.0% assumption — though with the opposite mix to plan (RIG 0.8% vs 1.5% budgeted, Pricing 2.8% vs 1.5%) — but a -5.7% actual FX headwind against a flat (0.0%) Budget assumption explains nearly the entire swing from a budgeted +3.0% reported sales growth to an actual -2.0% decline. Net Profit fell further than the FX/sales effect alone would suggest: COGS rose to 54.4% of sales (+151 bps vs Budget) and one-off items (including a higher impairment charge) and lower associates income added to the gap, partly offset by lower Marketing & Administration spend (see `3. Variance_Analysis`, Section D).
- The FY2025 Budget's ~40 assumptions are a mix of Nestlé's own disclosed guidance (organic sales growth, resulting UTOP margin "at or above 16.0%", no new share buyback) and explicit own-assumptions grounded in historical ratios/trends where the company gave no specific figure (e.g. cost ratios, capex intensity, working capital, financing flows) — every assumption's source is documented in the `2. Assumptions_FY2025` sheet.
- The Budget's ending cash balance rises materially versus FY2024 (CHF 5,558m → CHF 9,240m) mainly because two large FY2024 one-off outflows — the CHF 4,678m share buyback and ~CHF 2,130m of M&A/treasury-investment outflows — are assumed not to repeat in FY2025, per guidance. Operating Cash Flow itself is budgeted slightly *below* FY2024 (CHF 15,666m vs CHF 16,675m); this is flagged in-sheet so the cash build isn't mistaken for an operating improvement.
- The Adverse and Favorable scenarios move all ten flexed drivers together, so they form a stress range around the Base plan rather than probability-weighted forecasts. In the Step 3 backtest, FY2025 Net Profit landed between Adverse and Base, Sales came in just below Adverse, and 6 of the 10 flexed drivers fell outside their Adverse–Favorable ranges (FX, COGS, other trading items and associates income worse than Adverse; Marketing & Admin and capex better than Favorable).
- In `1. Overview_Analysis`, the Sales Growth Bridge's "Reported Sales Growth" (RIG + Pricing + Net M&A + FX) is off by a few basis points from the actual reported YoY growth in `1. Segment_Data` — this is expected rounding noise from Nestlé's own disclosed bridge components (each rounded to one decimal place), not a formula error; a direct check row is included in-sheet.

## Next step

Step 4 — Power BI Dashboard: an interactive dashboard built on the Excel model above (Overview, Budget vs Actual variance, the Adverse / Base / Favorable scenario range, and segment/category views).

## Disclaimer

This is an independent portfolio analysis. It is not prepared by, affiliated with, or endorsed by Nestlé S.A. All actual figures come from Nestlé's published Full-Year Results press releases and Financial Statements; budget figures are the author's own assumptions. "Nestlé" is a registered trademark of Société des Produits Nestlé S.A.

## Author

Thanh Vinh Dang — [LinkedIn](https://linkedin.com/in/thanhvinhdang2001)
