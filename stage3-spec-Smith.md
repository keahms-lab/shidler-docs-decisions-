# FX Receivable & Payable Hedging Model – Technical Specification

**Created by:** Keah Smith
**Updated by:** Keah Smith
**Date Created:** April 2026
**Date Updated:** April 2026
**Version:** 1.0
**LLM Used:** Claude (Anthropic)

**Role:** Financial Analyst / Treasury Analyst
**Audience:** CFO or Director of Treasury

**Purpose:** Provide a professional, quantitative specification documenting the Stage 2 Excel hedge model for GBP-denominated exposures and articulating a refined, improved design for Stage 4 implementation.

---

## 1. Problem Statement

Boeing Corporation faces bilateral GBP/USD foreign exchange exposure arising from two transactions: (1) a £20,000,000 receivable due in one year from British Airways for the export of a 737 landing gear assembly, and (2) a £5,000,000 payable due in one year to Rolls-Royce for an imported jet engine. The receivable creates downside risk if GBP depreciates against USD; the payable creates upside risk if GBP appreciates. This specification documents the analytical framework used to quantify and compare four strategies—no hedge, forward hedge, money market hedge, and option hedge—for each exposure, and identifies improvements for a more rigorous successor model.

---

## 2. Inputs (Known Variables)

### Receivable Case

| Named Range   | Description                              | Unit       | Value          | Source            |
|---------------|------------------------------------------|------------|----------------|-------------------|
| `FC_AMT_R`    | GBP receivable                           | GBP        | 20,000,000     | Company data      |
| `S0_in`       | Spot rate at inception (GBP/USD)         | USD per GBP| 1.1600         | Market data       |
| `F0_in`       | 1-year forward rate (GBP/USD)            | USD per GBP| 1.0935         | Market data       |
| `R_USD`       | U.S. 1-year interest rate                | Annual %   | 5.00%          | Macro benchmark   |
| `R_GBP`       | U.K. 1-year interest rate                | Annual %   | 3.00%          | Macro benchmark   |
| `T_DAYS`      | Days to settlement                       | Days       | 365            | Derived           |
| `K_PUT`       | GBP put option strike                    | USD per GBP| 1.35           | Analyst choice    |
| `PREM_PUT`    | Put premium per unit of GBP              | USD        | 0.0300         | Scenario          |

### Payable Case

| Named Range   | Description                              | Unit       | Value          | Source            |
|---------------|------------------------------------------|------------|----------------|-------------------|
| `FC_AMT_P`    | GBP payable                              | GBP        | 5,000,000      | Company data      |
| `S0_pay`      | Spot rate at inception (GBP/USD)         | USD per GBP| 1.8000         | Market data       |
| `F0_pay`      | 1-year forward rate (GBP/USD)            | USD per GBP| 1.7500         | Market data       |
| `R_USD_P`     | U.S. 1-year interest rate                | Annual %   | 6.00%          | Macro benchmark   |
| `R_GBP_P`     | U.K. 1-year interest rate                | Annual %   | 6.50%          | Macro benchmark   |
| `K_CALL`      | GBP call option strike                   | USD per GBP| 1.8000         | Analyst choice    |
| `PREM_CALL`   | Call premium per unit of GBP             | USD        | 0.0100         | Scenario          |

---

## 3. Assumptions & Constraints

- Interest rates are applied on a simple annual basis (no continuous or semi-annual compounding).
- The one-year horizon is treated as exactly 365 days; `T_DAYS/365 = 1.0` throughout.
- Covered interest parity (CIP) is assumed to hold: the forward rate equals the spot rate times the ratio `(1 + R_USD) / (1 + R_GBP)`. This implies the forward hedge and the money market hedge should produce identical locked-in USD amounts.
- No transaction costs, bid-ask spreads, or brokerage fees are included.
- No credit risk: the firm can borrow and lend freely at the stated rates.
- Option premiums are paid at inception in USD and are compounded forward one year at the domestic rate to express cost on a future-value basis.
- Exchange rates are expressed as USD per GBP throughout.
- Sensitivity scenarios vary the future spot rate `S_T` in increments of 0.01 (receivable) and 0.05 (payable) across a range of approximately ±15–20% around the baseline spot.

---

## 4. Calculation Flow

### Receivable Case

**1. Forward Hedge**
Multiply `FC_AMT_R` by `F0_in` to obtain locked-in USD proceeds:
> `USD_forward_R = FC_AMT_R × F0_in = 20,000,000 × 1.0935 = $21,870,000`

**2. Money Market Hedge**
a. Borrow the present value of the receivable in GBP: divide `FC_AMT_R` by `(1 + R_GBP)`:
> `Borrow_GBP = 20,000,000 / 1.03 = £19,417,476`

b. Convert borrowed GBP to USD at spot: multiply by `S0_in`:
> `USD_spot = 19,417,476 × 1.16 = $22,524,272`

c. Invest USD proceeds for one year at `R_USD`:
> `USD_mm_R = 22,524,272 × 1.05 = $23,650,485`

Under CIP this should equal `USD_forward_R`; the divergence observed in the Stage 2 model (~$1.78M) signals that the spot rate used (1.16) is inconsistent with the provided forward (1.0935), violating parity. This is addressed in Section 6.

**3. Option Hedge (GBP Put)**
a. Compute total put premium cost: `FC_AMT_R × PREM_PUT = 20,000,000 × 0.03 = $600,000`

b. Compound premium to future value: `600,000 × 1.05 = $630,000`

c. For each scenario `S_T`:
- If `S_T ≥ K_PUT` (1.35): let put expire; net proceeds = `FC_AMT_R × S_T − 630,000`
- If `S_T < K_PUT`: exercise put; net proceeds = `FC_AMT_R × 1.35 − 630,000 = $26,370,000`

**4. Sensitivity Table**
For each `S_T` from approximately 1.40 to 1.52: compute USD received under all four strategies and hedge profit/loss relative to the no-hedge baseline.

### Payable Case

**1. Forward Hedge**
> `USD_forward_P = FC_AMT_P × F0_pay = 5,000,000 × 1.75 = $8,750,000`

**2. Money Market Hedge**
a. Compute PV of payable in GBP: `5,000,000 / 1.065 = £4,694,836`

b. Convert to USD at spot to determine USD borrowing: `4,694,836 × 1.80 = $8,450,704`

c. Invest GBP PV amount for one year; it grows to exactly £5,000,000 to settle the payable.

d. Repay USD loan with interest: `8,450,704 × 1.06 = $8,957,746`

**3. Option Hedge (GBP Call)**
a. Total call premium: `5,000,000 × 0.01 = $50,000`; FV = `$53,000`

b. For each scenario `S_T`:
- If `S_T ≤ K_CALL` (1.80): let call expire; total cost = `FC_AMT_P × S_T + 53,000`
- If `S_T > K_CALL`: exercise call; total cost = `5,000,000 × 1.80 + 53,000 = $9,053,000`

---

## 5. Outputs

| Output               | Description                                               | Format        | Purpose                                  |
|----------------------|-----------------------------------------------------------|---------------|------------------------------------------|
| `USD_forward_R`      | Locked-in USD proceeds, receivable forward hedge          | Scalar USD    | Certainty benchmark for receivable       |
| `USD_mm_R`           | USD proceeds, receivable money market hedge               | Scalar USD    | Parity cross-check                       |
| `USD_put_table`      | Net USD proceeds by `S_T`, receivable put hedge           | Table         | Scenario sensitivity, floor protection   |
| `USD_forward_P`      | Locked-in USD cost, payable forward hedge                 | Scalar USD    | Certainty benchmark for payable          |
| `USD_mm_P`           | USD cost, payable money market hedge                      | Scalar USD    | Parity cross-check                       |
| `USD_call_table`     | Net USD cost by `S_T`, payable call hedge                 | Table         | Scenario sensitivity, cap protection     |
| `Hedge_PnL_R`        | Profit/loss of each hedge vs. no-hedge, receivable        | Table         | Strategy comparison                      |
| `Hedge_PnL_P`        | Profit/loss of each hedge vs. no-hedge, payable           | Table         | Strategy comparison                      |
| `Chart_Receivable`   | USD proceeds by strategy vs. `S_T`, receivable            | Line chart    | Visual comparison of hedge payoff curves |
| `Chart_Payable`      | USD cost by strategy vs. `S_T`, payable                   | Line chart    | Visual comparison of hedge payoff curves |
| `Winner_Column`      | Best strategy at each `S_T` (hedge and no-hedge)          | Text column   | Decision-support summary                 |

---

## 6. Model Review — What Worked & What to Improve

**What worked correctly:**
- Forward hedge calculations are accurate for both receivable and payable cases.
- Option payoff logic (put floor for receivable, call cap for payable) is correctly implemented, including the future-value premium cost.
- Sensitivity tables span a relevant range of `S_T` scenarios, and the winner columns correctly identify the dominant strategy at each scenario.
- The payables sheet correctly models the money market hedge for a payable (borrow USD, convert to GBP, invest GBP, settle payable).

**What should be improved:**
- **Parity violation:** The receivable case uses a spot rate of 1.16 and a forward rate of 1.0935 with 5% USD and 3% GBP rates. CIP implies a forward of `1.16 × (1.05/1.03) = 1.1825`, not 1.0935. The Stage 4 model should use a single consistent set of rates and derive the forward from CIP, or use a market-provided forward and back out the implied rate differential. All three strategies—forward, money market, and no-hedge—must share the same rate inputs to produce a valid comparison.
- **`#NAME?` error in option sheet:** Cell `[a] Buy puts` displays a `#NAME?` error in the receivable options section, indicating a broken named range or formula reference. This must be corrected.
- **Naming conventions:** Variable names are inconsistent across sheets (e.g., "1-Year Receivable" vs. no standardized range names). The improved model should use the named ranges defined in Section 2 above.
- **Sensitivity range and step size:** The receivable sensitivity runs from 1.40 to 1.52 (all above the 1.35 put strike), meaning the option hedge never exercises in the table. The range should be extended to include `S_T` values below the strike to illustrate the put floor.
- **The payable spot rate (1.80) does not match the receivable spot rate (1.16):** These sheets appear to represent different time periods or scenarios. A unified model should document whether these are distinct transactions or use a single consistent spot rate baseline.
- **Interest compounding basis:** Stage 2 uses simple annual rates (`× 1 + r`). The improved model should document whether ACT/360 or ACT/365 day-count conventions apply and implement `T_DAYS/360` or `T_DAYS/365` accordingly.
- **No chart for receivable case:** The model produces comparison data but does not appear to include a payoff curve chart for the receivable sheet. A line chart should be added.

---

## 7. Sensitivity Plan

The sensitivity analysis varies the future GBP/USD spot rate (`S_T`) across a range spanning approximately ±15% of the baseline spot rate. For the receivable case, `S_T` should run from `0.85 × S0_in` to `1.15 × S0_in` in steps of 0.01 USD/GBP, generating approximately 40 scenario rows. For the payable case, the same ±15% range applies. At each `S_T`, USD proceeds (receivable) or USD cost (payable) are calculated for all four strategies, and hedge profit/loss relative to the no-hedge outcome is recorded.

The sensitivity chart presents four lines (no hedge, forward, money market, option) against the x-axis of `S_T`. For the receivable, the most informative comparisons are: (1) the option hedge vs. no hedge, illustrating the downside floor provided by the put, and (2) the forward/money market hedge, which locks in a flat USD amount regardless of `S_T`. The crossover points—where the option hedge outperforms the forward—should be clearly labeled. For the payable, the call option cap and the forward lock-in are the key visual reference points.

---

## 8. Limitations & Next Steps

This model excludes dynamic hedging, mark-to-market accounting treatment (ASC 815 / IFRS 9 hedge designation), credit risk, counterparty exposure, and volatility-based option pricing (Black-Scholes). Option premiums are treated as flat scenario inputs rather than derived from implied volatility. The model also does not account for the time value of money on the premium at sub-annual intervals.

The parity inconsistency identified in Section 6 is the most material analytical limitation. Until resolved, the money market hedge results in the receivable case should not be used for decision-making.

This specification feeds directly into the Stage 4 AI prompt. The named ranges in Section 2, the step-by-step calculation logic in Section 4, the improvement priorities in Section 6, and the output list in Section 5 will be translated into explicit AI prompt instructions to construct a corrected, auditable Excel model.
