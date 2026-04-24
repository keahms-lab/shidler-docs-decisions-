# FX Receivable & Payable Hedging Model – Technical Specification

**Created by:** Keah Smith
**Updated by:** Keah Smith
**Date Created:** April 2026
**Date Updated:** April 2026
**Version:** 2.0
**LLM Used:** Claude (Anthropic)

**Role:** Financial Analyst / Treasury Analyst
**Audience:** CFO or Director of Treasury

**Purpose:** Provide a professional, quantitative specification documenting the Stage 2 Excel hedge model for GBP-denominated exposures and articulating a refined, improved design for Stage 4 implementation.

---

## 1. Problem Statement

Boeing Corporation faces bilateral GBP/USD foreign exchange exposure arising from two transactions: (1) a £20,000,000 receivable due in one year from British Airways for the export of a 737 landing gear assembly, and (2) a £5,000,000 payable due in one year to Rolls-Royce for an imported jet engine. The receivable creates downside risk if GBP depreciates against USD; the payable creates upside risk if GBP appreciates. This specification documents the analytical framework used to quantify and compare four strategies—no hedge, forward hedge, money market hedge, and option hedge—for each exposure, and identifies remaining improvements for a more rigorous successor model.

---

## 2. Inputs (Known Variables)

### Receivable Case

| Named Range   | Description                                    | Unit        | Value    | Source                          |
|---------------|------------------------------------------------|-------------|----------|---------------------------------|
| `FC_AMT_R`    | GBP receivable                                 | GBP         | 20,000,000 | Company data                  |
| `S0_in`       | Spot rate at inception (GBP/USD)               | USD per GBP | 1.1600   | Market data                     |
| `F0_in`       | 1-year forward rate (GBP/USD), CIP-derived     | USD per GBP | 1.1825   | `S0 × (1+R_USD)/(1+R_GBP)`     |
| `R_USD`       | U.S. 1-year interest rate                      | Annual %    | 5.00%    | Macro benchmark                 |
| `R_GBP`       | U.K. 1-year interest rate                      | Annual %    | 3.00%    | Macro benchmark                 |
| `T_DAYS`      | Days to settlement                             | Days        | 365      | Derived                         |
| `K_PUT`       | GBP put option strike                          | USD per GBP | 1.3500   | Analyst choice                  |
| `PREM_PUT`    | Put premium per unit of GBP                    | USD         | 0.0300   | Scenario input                  |

### Payable Case

| Named Range   | Description                                    | Unit        | Value    | Source          |
|---------------|------------------------------------------------|-------------|----------|-----------------|
| `FC_AMT_P`    | GBP payable                                    | GBP         | 5,000,000 | Company data   |
| `S0_pay`      | Spot rate at inception (GBP/USD)               | USD per GBP | 1.8000   | Market data     |
| `F0_pay`      | 1-year forward rate (GBP/USD)                  | USD per GBP | 1.7500   | Market data     |
| `R_USD_P`     | U.S. 1-year interest rate                      | Annual %    | 6.00%    | Macro benchmark |
| `R_GBP_P`     | U.K. 1-year interest rate                      | Annual %    | 6.50%    | Macro benchmark |
| `K_CALL`      | GBP call option strike                         | USD per GBP | 1.8000   | Analyst choice  |
| `PREM_CALL`   | Call premium per unit of GBP                   | USD         | 0.0100   | Scenario input  |

---

## 3. Assumptions & Constraints

- Interest rates are applied on a simple annual basis (no continuous or semi-annual compounding).
- The one-year horizon is treated as exactly 365 days; `T_DAYS/365 = 1.0` throughout.
- Covered interest parity (CIP) holds by construction in the receivable case: the forward rate is derived as `S0 × (1 + R_USD) / (1 + R_GBP) = 1.16 × 1.05 / 1.03 = 1.1825`. This ensures the forward hedge and money market hedge produce identical locked-in USD amounts.
- The payable forward rate (1.75) is a market-provided input and is not independently verified against CIP given the separate rate set for that transaction.
- No transaction costs, bid-ask spreads, or brokerage fees are included.
- No credit risk: the firm can borrow and lend freely at the stated rates.
- Option premiums are paid at inception in USD and compounded forward one year at the domestic rate to place all costs on a future-value basis.
- Exchange rates are expressed as USD per GBP throughout.
- The two transactions use different spot rates (1.16 vs. 1.80), reflecting separate scenarios with distinct market conditions; they are not modeled as a portfolio.

---

## 4. Calculation Flow

### Receivable Case

**1. Forward Hedge**
Derive the CIP-consistent forward rate, then multiply by the notional:
> `F0_in = S0_in × (1 + R_USD) / (1 + R_GBP) = 1.16 × 1.05 / 1.03 = 1.1825`
> `USD_forward_R = FC_AMT_R × F0_in = 20,000,000 × 1.1825 = $23,650,485`

**2. Money Market Hedge**
a. Borrow the present value of the receivable in GBP: divide `FC_AMT_R` by `(1 + R_GBP)`:
> `Borrow_GBP = 20,000,000 / 1.03 = £19,417,476`

b. Convert borrowed GBP to USD at spot:
> `USD_spot = 19,417,476 × 1.16 = $22,524,272`

c. Invest USD proceeds for one year at `R_USD`:
> `USD_mm_R = 22,524,272 × 1.05 = $23,650,485`

Under CIP, `USD_mm_R = USD_forward_R` exactly — confirmed in the fixed model. ✓

**3. Option Hedge (GBP Put)**
a. Compute total put premium cost and compound to future value:
> `Premium_FV = FC_AMT_R × PREM_PUT × (1 + R_USD) = 20,000,000 × 0.03 × 1.05 = $630,000`

b. For each scenario `S_T`:
- If `S_T < K_PUT` (1.35): exercise put; net proceeds = `20,000,000 × 1.35 − 630,000 = $26,370,000` (floor)
- If `S_T ≥ K_PUT`: let put expire; net proceeds = `20,000,000 × S_T − 630,000`

**4. Sensitivity Table**
For each `S_T` from 1.14 to 1.66 in steps of 0.01 (53 scenarios): compute USD received under all four strategies and hedge profit/loss relative to the no-hedge baseline. The put strike (1.35) falls within the range, so both the floor-active and floor-inactive regions are fully visible.

### Payable Case

**1. Forward Hedge**
> `USD_forward_P = FC_AMT_P × F0_pay = 5,000,000 × 1.75 = $8,750,000`

**2. Money Market Hedge**
a. Compute PV of payable in GBP: `5,000,000 / 1.065 = £4,694,836`

b. Borrow equivalent USD at spot: `4,694,836 × 1.80 = $8,450,704`

c. Invest GBP PV for one year; it grows to exactly £5,000,000, settling the payable.

d. Repay USD loan with interest: `8,450,704 × 1.06 = $8,957,746`

**3. Option Hedge (GBP Call)**
a. Total call premium FV: `5,000,000 × 0.01 × 1.06 = $53,000`

b. For each scenario `S_T`:
- If `S_T ≤ K_CALL` (1.80): let call expire; total cost = `5,000,000 × S_T + 53,000`
- If `S_T > K_CALL`: exercise call; total cost = `5,000,000 × 1.80 + 53,000 = $9,053,000` (cap)

---

## 5. Outputs

| Output             | Description                                          | Format     | Purpose                                 |
|--------------------|------------------------------------------------------|------------|-----------------------------------------|
| `USD_forward_R`    | Locked-in USD proceeds, receivable forward hedge     | Scalar USD | Certainty benchmark; $23,650,485        |
| `USD_mm_R`         | USD proceeds, receivable money market hedge          | Scalar USD | Parity cross-check; equals forward ✓   |
| `USD_put_table`    | Net USD proceeds by `S_T`, receivable put hedge      | Table      | Floor = $26,370,000 when S_T < 1.35    |
| `USD_forward_P`    | Locked-in USD cost, payable forward hedge            | Scalar USD | Certainty benchmark; $8,750,000         |
| `USD_mm_P`         | USD cost, payable money market hedge                 | Scalar USD | Cross-check; $8,957,746                 |
| `USD_call_table`   | Net USD cost by `S_T`, payable call hedge            | Table      | Cap = $9,053,000 when S_T > 1.80       |
| `Hedge_PnL_R`      | Profit/loss of each hedge vs. no-hedge, receivable   | Table      | Strategy comparison across scenarios    |
| `Hedge_PnL_P`      | Profit/loss of each hedge vs. no-hedge, payable      | Table      | Strategy comparison across scenarios    |
| `Chart_Receivable` | USD proceeds by strategy vs. `S_T`, receivable       | Line chart | Payoff curves; crossover at S_T ≈ 1.32 |
| `Chart_Payable`    | USD cost by strategy vs. `S_T`, payable              | Line chart | Visual comparison of hedge cost curves  |
| `Winner_Column`    | Best strategy at each `S_T` (vs. hedges; vs. all)    | Text column | Decision-support summary               |

---

## 6. Model Review — What Worked & What to Improve

**What is working correctly in the fixed model:**
- Forward and money market hedges now produce identical locked-in proceeds of **$23,650,485** in the receivable case, confirming CIP holds. The forward rate is derived by formula (`=S0*(1+R_USD)/(1+R_GBP)`) rather than hardcoded, so it updates automatically if rates change.
- Option payoff logic (put floor for receivable, call cap for payable) is correctly implemented, with premiums expressed on a future-value basis for apples-to-apples comparison.
- The receivable sensitivity table now runs from **S_T = 1.14 to 1.66** (53 rows, 0.01 step), spanning the put strike of 1.35 — the floor and no-floor regions are both visible and the strategy crossover is observable.
- The payable money market hedge correctly models the borrow-USD / invest-GBP structure, confirming the £5,000,000 payment is fully funded at $8,957,746.
- Winner columns correctly identify the dominant strategy at each scenario for both hedges-only and all-strategies comparisons.

**What remains to improve in Stage 4:**
- **Payable parity is not verified:** The payable forward (1.75) is a hardcoded market input. CIP with the payable rates implies `1.80 × 1.06 / 1.065 = 1.7915`, not 1.75. A rigorous model should either derive the forward from CIP or explicitly document the basis for the divergence.
- **Inconsistent spot rates across sheets:** The receivable uses S0 = 1.16 and the payable uses S0 = 1.80. These should be framed explicitly as separate standalone scenarios, with a header on each sheet making the context unambiguous.
- **Interest compounding basis undocumented:** The model uses simple annual compounding (`× (1 + r)`) but does not specify a day-count convention (ACT/360 or ACT/365). Stage 4 should implement `T_DAYS/360` or `T_DAYS/365` explicitly so formulas generalize to non-annual horizons.
- **No payoff chart on the receivable sheet:** Sensitivity table data is present but no line chart visualizes the four strategy payoff curves. Stage 4 should add a chart for each sheet with the strike price marked as a vertical reference line.
- **Named range spelling and conventions:** The existing workbook contains `recievable` (misspelled) and inconsistent naming length across sheets. Stage 4 should standardize on the named ranges defined in Section 2.

---

## 7. Sensitivity Plan

The receivable sensitivity table varies `S_T` from **1.14 to 1.66** in steps of **0.01**, producing 53 scenario rows. This range spans approximately ±20% around the approximate baseline of S_T = 1.46 and deliberately straddles the put strike (1.35). Below the strike the put exercises and net proceeds are flat at $26,370,000; above the strike the option expires and proceeds rise linearly with `S_T`. The forward and money market hedges produce a constant $23,650,485 across all scenarios. The crossover where the option hedge surpasses the forward occurs at approximately S_T = 1.32, where `20,000,000 × S_T − 630,000 = 23,650,485` solves to `S_T ≈ 1.214` — meaning the option hedge dominates the forward across the entire displayed range below the strike, and the no-hedge outcome dominates above approximately S_T = 1.32.

The payable sensitivity table varies `S_T` from 1.50 to 2.10 in steps of 0.05. The call strike (1.80) sits mid-range; below it the option expires and the call hedge tracks no-hedge cost plus premium; above it the cap locks total cost at $9,053,000.

For both charts, the four strategy lines should be plotted against `S_T` on the x-axis with USD amount on the y-axis, crossover points labeled, and the strike price marked with a vertical reference line.

---

## 8. Limitations & Next Steps

This model excludes dynamic hedging, mark-to-market accounting treatment (ASC 815 / IFRS 9 hedge designation), credit risk, counterparty exposure, and volatility-based option pricing (Black-Scholes or binomial). Option premiums are flat scenario inputs rather than derived from implied volatility, so the model does not reflect the cost of optionality as a function of tenor or moneyness. The two transactions are analyzed independently; a portfolio view netting the receivable and payable exposures is not included.

This specification feeds directly into the Stage 4 AI prompt. The named ranges in Section 2, the step-by-step calculation logic in Section 4, the improvement priorities in Section 6, and the output list in Section 5 will be translated into explicit AI prompt instructions to construct a fully auditable, parity-consistent Excel model with standardized naming and payoff charts for both transactions.
