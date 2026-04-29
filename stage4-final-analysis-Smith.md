# Stage 4 – Final Analysis, Prompt Engineering & Recommendation

**Prepared by:** Keah Smith
**Date:** April 2026
**Course:** Financial Analysis / Treasury Management
**Audience:** CFO, Director of Treasury – Boeing Corporation

---

## A. Exposure Summary

Boeing Corporation carries two bilateral GBP/USD foreign exchange exposures, each settling in one year:

- **Receivable:** £20,000,000 due from British Airways for export of a 737 landing gear assembly. The current spot rate is **1.1600 USD/GBP**, implying a baseline USD value of $23,200,000. If GBP depreciates before settlement, Boeing receives fewer dollars than anticipated, directly reducing operating cash flow.

- **Payable:** £5,000,000 due to Rolls-Royce for an imported jet engine. The spot rate is **1.8000 USD/GBP**, implying a baseline USD cost of $9,000,000. If GBP appreciates before settlement, Boeing's USD outlay increases, pressuring the import budget.

The two exposures are not perfectly offsetting — they differ in notional size, spot rate environment, and direction of risk — and are therefore modeled as separate standalone scenarios. The primary hedging decision concerns the receivable, where downside GBP depreciation risk is the dominant concern.

---

## B. Summary of Hedge Outcomes

### Receivable (£20,000,000)

All dollar amounts are expressed on a **future-value basis** (premiums compounded one year at R_USD = 5%).

| Strategy | Locked-In / Floor USD | Key Mechanics |
|---|---|---|
| **Forward Hedge** | $23,650,485 (certain) | Sell GBP forward at F0 = 1.1825; no cash upfront |
| **Money Market Hedge** | $23,650,485 (certain) | Borrow £19,417,476; convert at spot; invest at 5% — confirms CIP parity |
| **Put Option (K = 1.35)** | $26,370,000 floor | Exercises below 1.35; expires above; premium FV = $630,000 |
| **No Hedge** | Variable: $20M × S_T | Full exposure to spot rate at settlement |

**Forward Hedge:** Locks in $23,650,485 regardless of where GBP/USD settles. No upside, no downside — maximum certainty. Opportunity cost exists if GBP rallies significantly above 1.1825.

**Money Market Hedge:** Produces an identical $23,650,485 by arbitrage (CIP holds by construction: 1.16 × 1.05 / 1.03 = 1.1825). The economic result is the same as the forward, but this strategy requires drawing on a credit facility to borrow GBP — a liquidity consideration the CFO should weigh against the clean execution of the forward.

**Put Option:** The put strike of 1.35 is set well above the forward rate of 1.1825, providing a floor of $26,370,000 — approximately $720,000 higher than the forward. This protection comes at a cost: the $630,000 premium (FV) is paid regardless of outcome. If GBP settles above 1.35, the option expires and Boeing retains full upside minus the premium. The option is the most expensive strategy at current GBP levels but offers the best floor.

**No Hedge:** The receivable tracks spot at settlement. This is the appropriate baseline for measuring hedge value but is not a recommended strategy given Boeing's sensitivity to FX volatility at contract scale.

### Payable (£5,000,000)

| Strategy | USD Cost | Key Mechanics |
|---|---|---|
| **Forward Hedge** | $8,750,000 (certain) | Buy GBP forward at F0_pay = 1.75 |
| **Money Market Hedge** | $8,957,746 | Borrow USD; invest GBP PV; repay at 6% |
| **Call Option (K = 1.80)** | $9,053,000 cap | Exercises above 1.80; premium FV = $53,000 |
| **No Hedge** | Variable: $5M × S_T | Full exposure at settlement |

The payable forward (1.75) is a market-provided input below the current spot (1.80), offering an immediate cost advantage. The money market hedge costs $8,957,746 — more expensive than the forward due to the rate differential (R_USD = 6%, R_GBP = 6.5%). The call option caps costs at $9,053,000 if GBP appreciates further, but at $53,000 in premium.

---

## C. Sensitivity Interpretation

### Receivable Sensitivity (S_T from 1.14 to 1.66, step 0.01)

**EUR [GBP] depreciation (S_T < 1.1825):** Both the forward and money market hedge outperform the no-hedge outcome, locking in $23,650,485 against falling dollar receipts. The put option also exercises at its floor of $26,370,000, delivering the highest net proceeds — but its premium has already been paid. In severe depreciation (S_T approaching 1.14), the no-hedge outcome deteriorates to ~$22,800,000, roughly $850,000 below the locked-in strategies.

**GBP at or near forward (S_T ≈ 1.1825):** The forward and money market hedges break even against spot. The put option lags by ~$630,000 (the unrecouped premium), since the option is exercised and the floor exceeds spot, but the premium cost erodes the net benefit marginally.

**GBP appreciation (S_T > 1.1825):** The no-hedge outcome improves linearly. The forward and money market hedges forgo this upside — the opportunity cost of certainty. The put option, once S_T exceeds the strike (1.35), expires and Boeing captures upside minus the $630,000 sunk premium. The option hedge surpasses the forward in net proceeds once S_T exceeds approximately 1.214 (the crossover where `20M × S_T − 630,000 > 23,650,485`). Above 1.32, the unhedged position outperforms all hedged strategies in absolute dollar terms.

**Key takeaway:** The option hedge dominates the forward in nearly every scenario within the sensitivity range — delivering either a higher floor below the strike or superior upside above it — at the cost of the $630,000 premium. The forward delivers maximum simplicity and certainty at the lowest execution complexity.

### Payable Sensitivity (S_T from 1.50 to 2.10, step 0.05)

Below S_T = 1.80, the call option expires and the call hedge tracks the no-hedge cost plus the $53,000 premium — slightly worse than going unhedged. Above S_T = 1.80, the call cap locks total cost at $9,053,000, protecting Boeing as GBP strengthens. The forward remains the lowest-cost strategy across the full range due to the favorable forward rate (1.75 vs. spot 1.80).

---

## D. Strategic Recommendation

**Recommended Strategy: Forward Hedge on the Receivable + Forward Hedge on the Payable**

For the receivable, I recommend executing a **one-year GBP forward sale** at 1.1825, locking in $23,650,485 with zero premium outlay and zero residual FX risk. For the payable, I recommend a **one-year GBP forward purchase** at 1.75, locking in $8,750,000 — the lowest available cost outcome.

**Alternative worth considering:** A put option on the receivable could be justified if Boeing's treasury policy or board risk appetite requires preserving upside on large GBP receivables. The put floor of $26,370,000 is $720,000 above the forward and provides meaningful downside protection at a net cost well within the deal margin. This is a defensible secondary recommendation.

The money market hedge produces economically identical results to the forward on the receivable but consumes credit capacity and requires operational steps (borrow, convert, invest) that add execution risk. It is not recommended unless the forward market is inaccessible.

---

## E. Executive Justification

**Cash Flow Stability:** The forward hedge eliminates all FX variability. Boeing's USD receipts and costs are known at contract signing — $23,650,485 in and $8,750,000 out — enabling precise operating cash flow forecasting for the fiscal year.

**Budget Certainty:** With both exposures forward-hedged, the net hedged cash flow is $23,650,485 − $8,750,000 = **$14,900,485**, fixed. Finance and FP&A can plan against this number without scenario buffers, reducing capital held against FX uncertainty.

**Liquidity Impact:** Forwards require no upfront premium, preserving Boeing's cash position. The money market alternative would require tapping the credit facility for ~£19.4M, which — while theoretically equivalent — creates off-balance-sheet borrowing and potential covenant implications.

**Optionality Value:** The put option's upside preservation carries real value in a risk-adjusted sense. However, at current market pricing (3 cents per GBP, or $600,000 total), the premium represents approximately 2.5% of the receivable's forward value. For a corporate treasury with a defined hedge policy, the forward's certainty typically outweighs the theoretical upside optionality.

**Recommendation Summary:** Execute forward hedges on both legs. Reserve the put option strategy as an elective upgrade if management wishes to retain GBP upside exposure at a defined cost.

---

## F. Structured AI Prompt

---

### Appendix: AI Prompt for FX Hedging Spreadsheet Regeneration

```
# GOAL

Create a professional Excel workbook modeling four FX hedging strategies
(No Hedge, Forward, Money Market, Options) for two GBP/USD exposures:
(1) a £20,000,000 receivable from British Airways, and (2) a £5,000,000
payable to Rolls-Royce. The model must use named ranges, color-coded
cells, parity verification checks, sensitivity tables, and payoff charts.
This is a Boeing Corporation treasury model for CFO review.

---

# INPUT VARIABLES

Use the following values exactly. Do not infer or substitute any variables.

## Receivable Case
- FC_AMT_R     = 20,000,000       (GBP receivable notional)
- S0_in        = 1.1600           (spot rate, USD per GBP, at inception)
- F0_in        = derived via CIP  (= S0_in × (1 + R_USD) / (1 + R_GBP))
- R_USD        = 0.05             (U.S. 1-year interest rate, simple annual)
- R_GBP        = 0.03             (U.K. 1-year interest rate, simple annual)
- T_DAYS       = 365              (days to settlement; T_DAYS/365 = 1.0)
- K_PUT        = 1.3500           (GBP put option strike, USD per GBP)
- PREM_PUT     = 0.0300           (put premium per GBP, USD)

## Payable Case
- FC_AMT_P     = 5,000,000        (GBP payable notional)
- S0_pay       = 1.8000           (spot rate, USD per GBP, at inception)
- F0_pay       = 1.7500           (market-provided forward rate, USD per GBP)
- R_USD_P      = 0.06             (U.S. 1-year interest rate, simple annual)
- R_GBP_P      = 0.065            (U.K. 1-year interest rate, simple annual)
- K_CALL       = 1.8000           (GBP call option strike, USD per GBP)
- PREM_CALL    = 0.0100           (call premium per GBP, USD)

---

# SPREADSHEET STRUCTURE

Create three worksheets: "Receivable", "Payable", and "Verification".

## Color Coding (apply to all cells throughout)
- Yellow  (#FFFF00 fill) = User input cells (all named range inputs above)
- Blue    (#ADD8E6 fill) = Assumption cells (rates, T_DAYS, strike prices)
- Green   (#90EE90 fill) = Formula cells (all calculated outputs)
- Gray    (#D3D3D3 fill) = Output summary cells

Use bold headers and freeze the top two rows on each sheet.

---

# MODEL LOGIC

## Sheet 1: Receivable

### Section 1 – Inputs (rows 1–12)
List all named ranges from the Receivable Case above in a two-column
table (Name | Value). Assign Excel Named Ranges using the exact names
FC_AMT_R, S0_in, F0_in, R_USD, R_GBP, T_DAYS, K_PUT, PREM_PUT.

### Section 2 – Derived Forward Rate (row 14)
F0_in = S0_in * (1 + R_USD) / (1 + R_GBP)
Expected result: 1.1825

### Section 3 – Strategy Outputs (rows 16–28)

**Strategy A: No Hedge**
USD_nohhedge_R = FC_AMT_R × S_T  (S_T is a user input for point-in-time calc)

**Strategy B: Forward Hedge**
USD_forward_R = FC_AMT_R × F0_in
Expected result: $23,650,485

**Strategy C: Money Market Hedge (3 steps)**
Step 1 – Borrow GBP PV: Borrow_GBP = FC_AMT_R / (1 + R_GBP)
  Expected: £19,417,476
Step 2 – Convert to USD at spot: USD_spot_R = Borrow_GBP × S0_in
  Expected: $22,524,272
Step 3 – Invest USD: USD_mm_R = USD_spot_R × (1 + R_USD)
  Expected: $23,650,485

**Strategy D: Put Option Hedge**
Premium future value: PREM_FV_R = FC_AMT_R × PREM_PUT × (1 + R_USD)
  Expected: $630,000
Payoff logic (use IF formula):
  If S_T < K_PUT: Net = FC_AMT_R × K_PUT − PREM_FV_R   → floor = $26,370,000
  If S_T ≥ K_PUT: Net = FC_AMT_R × S_T − PREM_FV_R

### Section 4 – Sensitivity Table (rows 30–85)
Create a table with S_T ranging from 1.14 to 1.66 in steps of 0.01
(53 rows). Columns:
  Col A: S_T
  Col B: No Hedge USD = FC_AMT_R × S_T
  Col C: Forward USD  = USD_forward_R (constant)
  Col D: MM Hedge USD = USD_mm_R (constant)
  Col E: Put Option USD = IF(S_T < K_PUT, FC_AMT_R × K_PUT − PREM_FV_R,
                              FC_AMT_R × S_T − PREM_FV_R)
  Col F: Hedge P&L vs No Hedge (Forward − No Hedge)
  Col G: Best Hedged Strategy (INDEX/MATCH or IF to label max of B:D)

### Section 5 – Payoff Chart
Create a line chart plotting Cols B, C, D, E against Col A (S_T on X-axis,
USD on Y-axis). Title: "GBP Receivable – Strategy Payoffs vs. Spot at Maturity".
Add a vertical reference line at S_T = K_PUT = 1.35.
Label crossover points on the chart or in a text box.

---

## Sheet 2: Payable

Replicate the structure of Sheet 1 using the Payable Case named ranges.

### Section 3 – Strategy Outputs

**Strategy A: No Hedge**
USD_nohhedge_P = FC_AMT_P × S_T

**Strategy B: Forward Hedge**
USD_forward_P = FC_AMT_P × F0_pay
Expected result: $8,750,000

**Strategy C: Money Market Hedge (3 steps)**
Step 1 – Invest GBP PV: Invest_GBP = FC_AMT_P / (1 + R_GBP_P)
  Expected: £4,694,836
Step 2 – Borrow USD equivalent: USD_borrowed = Invest_GBP × S0_pay
  Expected: $8,450,704
Step 3 – Repay USD with interest: USD_mm_P = USD_borrowed × (1 + R_USD_P)
  Expected: $8,957,746

**Strategy D: Call Option Hedge**
Premium FV: PREM_FV_P = FC_AMT_P × PREM_CALL × (1 + R_USD_P)
  Expected: $53,000
Payoff logic:
  If S_T > K_CALL: Total Cost = FC_AMT_P × K_CALL + PREM_FV_P → cap = $9,053,000
  If S_T ≤ K_CALL: Total Cost = FC_AMT_P × S_T + PREM_FV_P

### Section 4 – Sensitivity Table
S_T from 1.50 to 2.10 in steps of 0.05 (13 rows).
Same column structure as Sheet 1, adapted for payable (minimize cost).

### Section 5 – Payoff Chart
Line chart with vertical reference line at K_CALL = 1.80.
Title: "GBP Payable – Strategy Costs vs. Spot at Maturity".

---

## Sheet 3: Verification

### Check 1 – Forward vs. MM Parity (Receivable)
Cell: =USD_forward_R − USD_mm_R
Expected: 0 (or within $1 rounding tolerance)
Label: "CIP Parity Check – Receivable: PASS if |diff| < 1"

### Check 2 – Forward Rate CIP Derivation
Cell: =S0_in × (1 + R_USD) / (1 + R_GBP) − F0_in
Expected: 0
Label: "CIP Forward Rate Check"

### Check 3 – Option Floor Confirmation
Cell: =FC_AMT_R × K_PUT − PREM_FV_R
Expected: $26,370,000

### Check 4 – Payable MM Cross-Check
Cell: =USD_mm_P − (FC_AMT_P / (1+R_GBP_P) × S0_pay × (1+R_USD_P))
Expected: 0

### Check 5 – Named Range Audit
List all named ranges with =GET.CELL or manual reference to confirm
they resolve correctly.

---

# VERIFICATION

All formula cells (green) must reference named ranges — no hardcoded
numbers. Verify:
- F0_in is derived, not typed.
- Premium FV uses the named rate, not a literal 0.05.
- Sensitivity table S_T column uses a formula series (=prior + 0.01),
  not manually entered values.
- All parity checks on Sheet 3 return 0 or within tolerance.

---

# EXPORT

Save the file as: Boeing_FX_Hedge_Model_v2.xlsx
Format all USD output cells with Accounting style, 0 decimal places.
Format all rate and S_T cells with Number, 4 decimal places.
Print area: Set for each sheet to cover inputs through sensitivity table.
Freeze top 2 rows and left 1 column on each data sheet.
```

---

## Extra Credit: Areas for Further Study & Improvement

### 1. AI Skills & Automation

The structured prompt in Section F represents a static, single-run model generation workflow. A more powerful implementation would leverage AI tools such as Claude Skills or Code Interpreter to make the model dynamic and continuous. Specifically, a skill could be configured to pull live GBP/USD spot and forward rates from a Bloomberg or Refinitiv API feed, automatically update the FC_AMT and rate inputs, and regenerate the sensitivity table on demand — all without any manual data entry. Monte Carlo simulation would further enhance the model: rather than a deterministic 53-scenario sensitivity table, the AI could run 10,000 draws from a log-normal GBP/USD distribution (calibrated to implied volatility) and output probability-weighted hedge performance statistics, expected shortfall at the 5th percentile, and hedge effectiveness ratios. This moves the deliverable from a textbook grid to a decision-support tool that treasury teams actually deploy ahead of earnings releases or major deal signings.

### 2. Multi-File Reasoning

The three artifacts from this project — the Stage 3 technical specification (`stage3-spec-Smith.md`), the Stage 2 Excel model (`Boeing_FX_Hedge_Model.xlsx`), and the Stage 4 AI prompt — form a self-reinforcing triad. An AI system with multi-file reasoning capability (such as Claude's extended context window with document uploads) could ingest all three simultaneously and perform consistency audits: verify that every named range in the spec matches a named range in the workbook, flag any formula in the Excel that diverges from the calculation logic in the spec, and confirm that the prompt correctly encodes all scenario values. In practice, this means a treasury analyst could upload all three files and ask: *"Is the model consistent with the spec?"* — and receive a structured audit report identifying any drift. This capability also enables automated dashboard generation: the AI reads the Excel outputs, formats them into an executive-ready HTML or PowerPoint summary, and pushes the result to a SharePoint folder — a workflow increasingly available through Microsoft Copilot integrations.

### 3. GitHub & Version Control

Committing each stage artifact to a GitHub repository transforms this project from a folder of files into an auditable, reproducible model lineage. Stage 1 (problem statement), Stage 2 (Excel model), Stage 3 (specification), and Stage 4 (prompt and memo) each become a tagged commit, creating a complete provenance trail: who authored the model, when each assumption was introduced, and how the spec evolved from the initial build. For hedge accounting under ASC 815 or IFRS 9, this version history doubles as documentation evidence — auditors can inspect the `git log` to confirm that the hedge designation (forward vs. option), the hedged item (the British Airways receivable), and the risk management objective were documented before the hedging relationship was established. GitHub Actions could further automate model validation: a CI workflow triggered on each push could run a Python script that reads the Excel file, re-derives all formula outputs, and fails the build if any parity check deviates beyond a tolerance threshold — enforcing model integrity as a pre-commit gate.

---

*End of Stage 4 Deliverable — Boeing Corporation FX Hedge Analysis*
*Prepared by Keah Smith | April 2026 | Version 1.0*
