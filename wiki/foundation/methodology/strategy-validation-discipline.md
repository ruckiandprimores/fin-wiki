---
title: "Strategy Validation Discipline"
status: growing
created: 2026-05-05
updated: 2026-05-05
tags: [foundation, methodology, validation, backtest, multiple-testing, selection-bias, lopez-de-prado, day-swing]
---

# Strategy Validation Discipline

> **TL;DR**: Most backtests presented as evidence are false discoveries — not because of buggy code, but because of **selection bias on multiple trials**. This page is the wiki's operational discipline for distinguishing signal from noise: pre-registration of hypothesis + falsifier, purged cross-validation with embargo, sample-uniqueness corrections for non-IID labels, Probabilistic and Deflated Sharpe Ratios for finite samples and trial multiplicity, and Probability of Backtest Overfitting (PBO) for batch evaluations. Synthesized from [[foundation/sources/lopez-de-prado-advances-fin-ml|López de Prado 2018]] Ch 4, 7, 11, 14. **The load-bearing rules are the two laws (Sections 2.1, 7.1) and the pre-registration protocol (Section 9), not the math.**

## 1. The Core Problem

A backtest can be 100% bug-free, perfectly conservative on costs, and still produce **false discoveries with high probability**. The reason is **selection bias on multiple testing**:

- Every parameter sweep is a trial.
- Every direction-flip ("if long fails, does short work?") is a trial.
- Every threshold tuned post-hoc on results is a trial.
- Every prior strategy you've ever tried on this dataset counts toward the trial count.

When N trials are conducted, the *expected maximum Sharpe* under the null hypothesis (true SR = 0) **is greater than zero**, and grows with N. The classical 5% significance threshold (SR > some cutoff) was never adjusted for this — and the field has been publishing un-adjusted results for decades.

> **Key claim from [[foundation/sources/lopez-de-prado-advances-fin-ml|López de Prado 2018]] Ch 11.3**: *"The maddening thing about backtesting is that, the better you become at it, the more likely false discoveries will pop up. Beginners fall for the seven sins. Professionals may produce flawless backtests, and will still fall for multiple testing, selection bias, or backtest overfitting."*

This page exists because the wiki has been asserting "no overfit worship" and "falsifiers required" as principles (Part IV) without backing them with statistical method. **That backing is now here.**

## 2. The Two Laws (Adopt These First)

### 2.1 Marcos' Second Law of Backtesting

> **"Backtesting while researching is like drinking and driving. Do not research under the influence of a backtest."**
>
> — López de Prado 2018, Ch 11.5

**Operational meaning**: Hypothesis specification, parameter ranges, falsifier, and stop-conditions must be locked **before** the backtest runs. Adjusting any of these *based on backtest results* is data-snooping with extra steps.

**Why this is hard in practice**: when a backtest comes back negative, the immediate temptation is to *understand why* by sweeping parameters or flipping directions. Each sweep is a new trial. Two or three rounds of this and the original sample has been worn through.

**Operationalization in the wiki**:
- The TRADE-IDEA workflow step "Propose using structured hypothesis format" must precede any backtest invocation.
- Parameter ranges should be specified as a discrete list with explicit count, not "we'll see what works."
- Failed strategies go to [[trading/rejected]] **without** "second attempt" — see Recommendation 6 below.
- **Expected-edge estimates must use data independent of the test window** — same-data round-trips are *researching under the influence of a backtest* via implicit data peeking. See [[foundation/methodology/deployment-edge-threshold|deployment-edge §4.5]] for predictive vs descriptive distinction, acceptable independent sources, and worked failure/clean cases.

### 2.2 Marcos' Third Law of Backtesting

> **"Every backtest result must be reported in conjunction with all the trials involved in its production. Absent that information, it is impossible to assess the backtest's 'false discovery' probability."**
>
> — López de Prado 2018, Ch 14.7

**Operational meaning**: A reported Sharpe ratio without a trial count is **uninterpretable**. The same SR has different meaning if it was the only test ever run vs. selected from 100 attempts.

**Operationalization in the wiki**:
- Every entry in `wiki/trading/backtest-results/` must include `# of trials on this dataset` field.
- Every entry in `wiki/trading/rejected/` should include the trial count too — failed-trial counts compound over time and inform future DSR adjustments.
- Cross-strategy: the wiki itself is a research-trial log; the [[trading/rejected]] graveyard is part of the trial count for any future strategy on the same dataset.

## 3. The Seven Sins (Baseline Hygiene)

Per Luo et al. 2014, the basic errors *every backtest* must rule out before any statistical sophistication helps:

| Sin | Definition | Crypto-perp adaptation |
|-----|-----------|------------------------|
| **Survivorship bias** | Universe = currently-listed instruments only | **Real**: delisted alts, exchange-failed pairs (FTX-listed instruments) |
| **Look-ahead bias** | Using data not public at decision time | **High risk**: timestamp drift, exchange-side backfill corrections, on-chain-data publication delays |
| **Storytelling** | Ex-post mechanism narrative for random pattern | **Critical**: candle-morphology batch is a worked example ([[trading/rejected/candle-morphology-batch-2026-04]]) |
| **Data mining / snooping** | Training the model on the test set | Migrate to purged CV (Section 4) |
| **Transaction costs** | Underestimating execution slippage | **Less binding for crypto**: deep-orderbook majors (BTC/ETH) have low slippage; alts have high slippage with exchange-specific patterns |
| **Outliers** | Strategy P&L driven by 1-2 extreme events | **Real for crypto**: liquidation cascades, March-2020 Black Thursday, May-2021, Jun-2022 — single-event-driven returns are common |
| **Shorting** | Cost/availability of borrow | **Largely solved in crypto**: perps make shorting frictionless; this sin is mostly retired |

Net: 6/7 sins remain load-bearing in crypto; "shorting" is the one that is structurally easier than in equities.

## 4. Why Standard k-fold CV Fails in Finance

Standard k-fold CV assumes **observations are IID**. Financial data violates this in two ways:

### 4.1 Serially correlated features

Most price-derived features (returns, volatility, RSI, etc.) have `X_t ≈ X_{t+1}` to some degree. When `t` and `t+1` are placed in different folds, information leaks — the model learns a feature value at training that closely resembles a feature value at test.

### 4.2 Overlapping label horizons (the bigger issue)

Triple-barrier labeling, holding-period labels, "next-N-bars return" labels — all label `Y_i` over an interval `[t_{i,0}, t_{i,1}]`. When `t_{i,1} > t_{i+1,0}`, two consecutive labels share a common return component, so `Y_i ≈ Y_{i+1}` *by construction of the label*.

Random k-fold sampling spatters such overlapping labels into both train and test, leaking outcome information.

### 4.3 Consequence

Standard k-fold CV will report inflated accuracy / inflated Sharpe ratio precisely because of this leakage. The model "learned" something that is just a copy of label data leaking through. **Worse**: as `k → T` (number of bars), leakage *increases* — more folds means more boundary-overlap.

> **From Ch 7.3**: *"By now you may have read quite a few papers in finance that present k-fold CV evidence that an ML algorithm performs well. Unfortunately, it is almost certain that those results are wrong."*

**Operationalization status: relevant.** The the current backtesting lab pipeline uses a single 70/30 split rather than k-fold, which is itself critiqued in Section 7. Migrating to *purged* k-fold CV is the recommended path.

## 5. Purged k-fold CV with Embargo

The corrective is **purged k-fold CV** (López de Prado 2018, Ch 7.4):

### 5.1 Purging

For each test fold, **drop from the training set every observation whose label horizon overlaps the test horizon**.

Concretely: a label `Y_i` spanning `[t_{i,0}, t_{i,1}]` is *concurrent* with `Y_j` spanning `[t_{j,0}, t_{j,1}]` if any of:
1. `t_{j,0} ≤ t_{i,0} ≤ t_{j,1}`
2. `t_{j,0} ≤ t_{i,1} ≤ t_{j,1}`
3. `t_{i,0} ≤ t_{j,0}` and `t_{j,1} ≤ t_{i,1}`

Any training observation `i` concurrent with any test observation `j` is purged.

### 5.2 Embargo

Even after purging, **serially correlated features** can leak across the test→train boundary. Example: a Garch-style volatility feature shows a slow-moving regime; the model trains on observations *immediately after* the test set and effectively learns a feature value still informative about test-set outcomes.

The fix: drop training observations within an **embargo window** of size `h ≈ 0.01 * T` *after* the test set (no embargo needed before — pre-test training labels are by definition known at test time).

### 5.3 Validation that purging worked

If purging is sufficient, performance should *plateau* (not increase) as `k → T`. If performance keeps improving with larger `k`, leakage is still present and the embargo window needs widening.

### 5.4 Why this matters more than walk-forward

The trader-memory rule "no multi-year walk-forward for day/swing crypto due to regime non-stationarity" — see context — names what *not* to do. **Purged k-fold CV is what *to do* instead**. It preserves chronological structure (no lookahead within a fold), avoids leakage (purge + embargo), and does not assume multi-year stationarity (each fold is a window of similar regime). It is the constructive answer to a previously open question.

**Operationalization status: not implemented.** Strategy-lab pipeline uses single 70/30 split. Engineering work required to implement purged k-fold splitter.

## 6. Sample Uniqueness (Non-IID Label Correction)

When labels overlap (Section 4.2), **even within a single training fold**, the effective sample size is smaller than the count of observations.

### 6.1 The concept

For each label `i`, define the **uniqueness** at time `t`:
- `u_{t,i} = 1 / c_t`, where `c_t` is the number of labels concurrent at `t`

Then **average uniqueness of label `i`**:
- `ū_i = average over i's lifespan of u_{t,i}`

A label that overlaps heavily with neighbors has `ū_i << 1`. A label that stands alone has `ū_i ≈ 1`.

### 6.2 Why it matters

Standard bootstrap (used in random forests, bagging) assumes IID. With overlapping labels, the bootstrap **oversamples** redundant information — each overlapping label is effectively counted multiple times. The result: in-bag observations are redundant, out-of-bag observations are too similar to in-bag observations, and OOS accuracy is grossly inflated.

### 6.3 The fix

Three options:
1. **Drop overlapping observations entirely** — wastes information; LdP advises against
2. **Set bagging `max_samples = mean(ū_i)`** — corrects sample-frequency
3. **Sequential bootstrap** — sample one observation at a time with probability inversely proportional to its overlap with already-sampled observations

For DSL/rule-based strategies (no bagging), sample uniqueness is less directly used, but **the underlying claim — overlapping labels reduce effective sample size** — still applies. A backtest with 1000 nominal trades but heavy holding-period overlap may have effective N closer to 200. Confidence intervals should be widened accordingly.

**Operationalization status: not implemented.** Pipeline does not currently track or apply sample uniqueness. Most directly relevant if/when ML strategies enter the pipeline; less directly relevant for current rule-based DSL strategies, but informs honest interpretation of trade counts.

## 7. The Six Anti-Overfitting Recommendations

From Ch 11.5, with crypto-perp adaptations:

### 7.1 Develop strategies for asset classes / universes, not specific securities

LdP: *"Investors diversify, hence they do not make mistake X only on security Y."*

**Crypto adaptation**: develop for "BTC perps + ETH perps + top-10 alt perps" rather than "BTC alone." If a mechanism works only on a single instrument, suspect overfitting unless the mechanism *theoretically* should be instrument-specific (e.g., BTC dominance ratio is BTC-specific by definition).

**Constraint per Section 8**: the backtesting lab is single-symbol today. This is a known structural gap; cross-instrument testing is currently impossible. Until then, treat single-symbol successful backtests with extra suspicion and prefer mechanisms theoretically grounded across the asset class.

### 7.2 Apply bagging

Bagging reduces variance of the forecasting error. *If bagging deteriorates performance, the strategy was likely overfit to a small number of observations or outliers.*

**Crypto adaptation**: relevant only when ML enters the pipeline. For rule-based strategies, the analog is **regime-conditioned testing** (per [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC regime taxonomy]]) — split the dataset by regime and check that performance is consistent across regimes, not concentrated in one.

### 7.3 Don't backtest until research is complete

**Operational gate**: hypothesis + falsifier + parameter ranges + exit rules locked in a wiki page (mechanism page or candidate-strategy page) **before** any DSL strategy file is written. The DSL author does *implementation*; they do not invent.

### 7.4 Record every backtest conducted on a dataset

Marcos' Third Law in operational form. The wiki *is* this log — `trading/rejected/` + `trading/backtest-results/` + `trading/active/` together constitute the trial-count record.

**Implication for DSR (Section 8.3)**: trial count for any future strategy on BTC perps includes everything previously tested on BTC perps — not just the current research sprint. The graveyard compounds.

### 7.5 Simulate scenarios, not just history

LdP: *"History is just the random path that was realized; it could have been entirely different."*

**Crypto adaptation**: BTC has had ~5 distinct regime types since 2020 ([[foundation/macro/btc-regime-taxonomy-2020-2025]]) — bull-vol-blowoff, range-chop, accumulation, capitulation, recovery. A strategy that works in the *historical sequence* of these regimes may fail in a different sequence. Methods:
- **Bootstrap regimes**: resample regime blocks rather than days
- **Synthetic data**: simulate price paths with realistic vol, jump, and serial-correlation structure (LdP Ch 13, not extracted here)
- **Monte Carlo from realized distributions**: paths drawn from historical return distributions per regime

**Operationalization status: not implemented.** Pipeline uses historical-only backtests.

### 7.6 If the backtest fails, start from scratch

LdP: *"Resist the temptation of reusing those results. Follow the Second Law."*

**Operational meaning**: if hypothesis H1 produces negative backtest, do NOT try H1' (similar mechanism, tweaked parameters) on the same data. The negative result *informs the rejection*; it does not inform a "v2." If a related mechanism is worth testing, it should be:
- Pre-registered separately (new hypothesis + falsifier)
- Counted as a new trial in the trial log
- Subject to DSR multiplicity correction reflecting the prior trial

This is the Recommendation that most directly contradicts the natural research instinct ("let me just tweak this..."). It is also the highest-leverage one.

## 8. PSR and DSR — Adjusting the Sharpe Ratio

The Sharpe ratio is a noisy estimator. Two corrections from Ch 14:

### 8.1 The standard SR

`SR = μ / σ` where `μ`, `σ` are mean and stdev of excess returns. Assumes IID Gaussian returns. **Both assumptions fail in finance** — returns are typically negatively skewed, fat-tailed, and serially correlated.

### 8.2 Probabilistic Sharpe Ratio (PSR)

PSR = `Pr(true SR > benchmark SR* | observed SR_hat)`

Estimated as `Z[ (SR_hat - SR*) * sqrt(T-1) / sqrt(1 - skew*SR_hat + ((kurt-1)/4)*SR_hat^2) ]`

where `Z[.]` is the standard normal CDF, `T` is sample length (in original sampling frequency, not annualized), `skew` is skewness, `kurt` is kurtosis.

**Decision rule**: PSR > 0.95 means *95% confidence the true SR exceeds the benchmark*. Standard threshold for "passing."

**Behavior**:
- Higher observed SR → higher PSR (good)
- Longer track record `T` → higher PSR
- Positive skew → higher PSR; **negative skew → lower PSR**
- Fatter tails (higher kurtosis) → lower PSR

**Crypto-specific concern**: crypto strategies frequently have **strong negative skew** (long winners, occasional big losers from cascades) and **high kurtosis**. A naïve SR of 1.5 might correspond to a PSR of 0.7 or lower once skew/kurtosis are properly accounted for.

### 8.3 Deflated Sharpe Ratio (DSR)

DSR = PSR with the benchmark `SR*` raised to reflect **multiplicity of trials**.

When N independent strategies are tested, the expected maximum SR under the null `H_0: SR = 0` is greater than zero — and grows with N and with the variance V[SR_n] across trials.

`SR* = sqrt(V[SR_n]) * ((1-γ) * Z^{-1}(1 - 1/N) + γ * Z^{-1}(1 - 1/(N*e)))`

where `γ` is the Euler-Mascheroni constant (~0.5772) and `V[SR_n]` is the variance of SRs across the N trials.

**Decision rule**: DSR > 0.95 means *after correcting for the number of trials, 95% confidence the strategy's true SR is non-zero*.

**Behavior**:
- More trials N → higher SR* → harder to pass
- Higher variance across trials → higher SR* → harder to pass
- N=1 with V[SR_n]=0 → DSR ≈ PSR

**Operationalization status: not implemented.** Pipeline does not emit PSR or DSR. Engineering work to compute skewness, kurtosis, and N (trial count) per strategy run, then surface PSR/DSR in summary outputs.

## 9. Probability of Backtest Overfitting (PBO)

For evaluating a *batch* of strategies (or a single strategy with parameter sweeps), Ch 11.6 provides **Combinatorially Symmetric Cross-Validation (CSCV)**:

### 9.1 The procedure (high-level)

1. Form a `T × N` matrix where columns are the N trial PnL series and rows are time observations
2. Partition into `S` equal-size submatrices by row (e.g., S=16)
3. For every combination of S/2 submatrices forming a "training" set and the complement forming a "test" set (~12,780 combinations for S=16):
   - Find the trial with best in-sample (training) performance
   - Compute its rank in out-of-sample (test) performance
4. **PBO = fraction of combinations where the IS-best trial underperforms the OOS median**

PBO = 0.5 means the IS-best is no better than median OOS — pure overfitting. PBO ≈ 0 means IS performance is genuinely informative about OOS.

### 9.2 Why this matters for the wiki

Direct application: **the candle-morphology batch ([[trading/rejected/candle-morphology-batch-2026-04]])** was 10 trials on a shared upstream premise. CSCV computed across those 10 trial PnL series would have produced a PBO estimate. If PBO ≈ 0.5, that's *evidence* the entire batch is overfit-to-noise rather than a coincidence of 10 truly-bad mechanisms — different conclusion, different next-step.

For any future batch evaluation (parameter sweep, mechanism family test, "test 5 thresholds"), CSCV → PBO is the discipline.

### 9.3 Walk-forward critique

LdP Ch 11.6 critiques single-path walk-forward:

> *"One disadvantage of the walk-forward method is that it can be easily overfit. The reason is that without random sampling, there is a single path of testing that can be repeated over and over until a false positive appears."*

This is the exact pattern flagged by the May 4 train→val analysis. Single 70/30 splits (or single chronological walk-forward paths) are weakest because there is one OOS path — easy to tweak parameters until it passes.

> **Refinement (2026-06-08) — negative train-val correlation is ambiguous, not a standalone falsifier.** The May-4 reading ("negative train→val correlation = overfit") over-generalized the correlation axis. A sweep can show **strongly negative train-val correlation AND high both-positive % AND high DSR** — that is *robust edge with overfit-prone selection*, not no-edge (e.g. regime_v3: corr −0.593, 81.8% both-positive, DSR 87.7%; ranking by train-PnL anti-selects). Read correlation jointly with both-positive % and DSR; select by Sharpe, not train-PnL rank. The May-4 strategies were negative-corr **with low both-positive %** (genuine non-transfer) — the finding was right for its cases but isn't a universal gate. Full four-pattern decision table: [[sweep-validation-pattern-taxonomy]].

**Operationalization status: not implemented.** No CSCV implementation in pipeline. Methodology defined here for future engineering work. Highest leverage for *batch* evaluations.

## 10. Pre-Registration Protocol (Operational Discipline)

This is the *operational* implementation of Marcos' Second Law. **Adopt this even before any of the math.**

For each strategy proposed for backtesting, a wiki page is written **before the backtest runs**. The page must contain:

| Field | Required content |
|-------|-----------------|
| **Hypothesis** | What is true about the market (per Part I §4 structured hypothesis schema) |
| **Mechanism** | Whose behavior, what structure, why now |
| **Observable** | How the hypothesis condition is measured |
| **Expected** | Direction, magnitude range, time horizon |
| **Falsifier** | What would convince you the hypothesis is wrong |
| **Parameter ranges** | Discrete list (e.g., "RSI thresholds: {25, 30, 35}; lookback: {10, 14, 20}; total: 9 combinations"). NOT "we'll see what works." |
| **Exit rules** | Stop-loss, take-profit, time-stop — pre-specified |
| **Trial budget** | "We will run N parameter combinations; if all fail, we reject the family" |
| **Trial count on this dataset** | All prior strategies tested on this dataset (BTC perps, ETH perps, etc.) |
| **PSR/DSR threshold** | Pass criterion in advance, not selected post-hoc |

After the backtest:
- **If passed**: page moves to `trading/backtest-results/` with full statistics, trial-count update for future DSR
- **If failed**: page moves to `trading/rejected/` with post-mortem; trial count incremented permanently
- **No "v2"** unless a *genuinely new* hypothesis is articulated — see Recommendation 6 above

This protocol is **not implemented as tooling** — it is a *discipline*. The discipline is the load-bearing thing; the tooling is optional.

## 11. Operationalization Status Summary

A snapshot of which methodology elements are *implemented* vs *aspirational*:

| Element | Wiki encoded | Pipeline implemented |
|---------|:------------:|:-------------------:|
| Pre-registration protocol (Section 10) | ✅ | ⚠️ Partial — wiki exists but discipline not enforced |
| Marcos' Second Law (Section 2.1) | ✅ | ⚠️ Partial — depends on pre-registration discipline |
| Marcos' Third Law (Section 2.2) | ✅ | ❌ Trial counts not tracked systematically |
| Seven sins (Section 3) | ✅ | ⚠️ Partial — survivorship/look-ahead checked manually |
| Purged k-fold CV (Section 5) | ✅ | ❌ Pipeline uses single 70/30 split |
| Embargo (Section 5.2) | ✅ | ❌ Not implemented |
| Sample uniqueness (Section 6) | ✅ | ❌ Not implemented; less load-bearing for current rule-based DSL strategies |
| Asset-class universes (Section 7.1) | ✅ | ❌ Pipeline single-symbol per Section 8 |
| Bagging / regime-conditioning (Section 7.2) | ✅ | ⚠️ Regime taxonomy exists; conditioning not automated |
| Scenario simulation (Section 7.5) | ✅ | ❌ Historical-only backtests |
| PSR (Section 8.2) | ✅ | ❌ Not in summary statistics |
| DSR (Section 8.3) | ✅ | ❌ Not in summary statistics |
| **Small-sample-validation operational layer** ([[foundation/methodology/small-sample-validation-discipline]]) | ✅ wiki encoded | ❌ Sample-size verdict-line not in pipeline; PSR-threshold table not emitted; portfolio-trim aggregate-test rule not automated |
| PBO via CSCV (Section 9) | ✅ | ❌ Not implemented |
| **Net-of-friction edge calculation** (sister methodology [[foundation/methodology/deployment-edge-threshold]]) | ✅ wiki encoded | ⚠️ partial pipeline — manual post-hoc per 2026-05-05 stocktake; not yet in engine output |
| **Forecast-vs-tradeable-gap diagnostic** (sister methodology [[foundation/methodology/forecast-vs-tradeable-gap]]) | ✅ wiki encoded | ❌ Not implemented; manual at strategy-design stage. Closes the L1→L2 translation-loss gap that even data-independent forecasting effects can fail. |

**Net**: every concept on this page is **wiki encoded but mostly not pipeline implemented**. This page is therefore **aspirational discipline** — the gap between what we know we should do and what we currently do. The gap is honest; closing it is engineering work, owner candidates per Apr 28 division of labor.

**Companion methodology pages on the validation axis** (statistical-significance gate ↔ economic-significance gate ↔ translation-loss gate ↔ path-quality gate):
- [[foundation/methodology/strategy-validation-discipline]] (this page) — Gate 1 statistical-significance (LdP synthesis)
- [[foundation/methodology/small-sample-validation-discipline]] — Gate 1 **operational layer** (PSR/CI/sample-size at lab-pipeline scale; portfolio-vs-individual-fragility distinction)
- [[foundation/methodology/forecast-vs-tradeable-gap]] — Gate 2 translation-loss (L1 forecasting → L2 tradeable strategy)
- [[foundation/methodology/deployment-edge-threshold]] — Gate 3 economic-significance (friction floor + opportunity-cost)
- [[foundation/methodology/path-quality-diagnostics]] — Gate 4 path-quality (event-concentration / per-sub-PASS / split-timing / val-period scrutiny)

A strategy must clear all gates to be deployment-grade.

## 12. Concrete Linkage to Wiki Failures

| Failure | What this methodology adds |
|---------|--------------------------|
| **[[trading/rejected/candle-morphology-batch-2026-04|Candle-morphology batch]]** | Section 9 (PBO via CSCV) would have produced a quantitative confidence in the batch-level rejection beyond "all 10 negative." Section 10 (pre-registration) would have caught the missing falsifiers and missing trial budgets at the input layer. |
| **Apr 28 Wyckoff distribution test** | The hypothesis + falsifier discipline already worked — this was a successful application of the *behavior* this methodology codifies. PSR/DSR would have allowed reporting confidence in the rejection rather than just sign. |
| **May 4 train→val correlation negative for 7/8** | Section 5 (purged CV) is the direct corrective. Section 9.3 (walk-forward critique) names the structural pattern: single 70/30 splits are easily overfit because the path is unique. **Refined 2026-06-08**: these were negative-corr *with low both-positive %* (genuine non-transfer); negative correlation is a falsifier only when both-positive % and DSR are also low — see §9.3 refinement note + [[sweep-validation-pattern-taxonomy]] Pattern 1 vs Pattern 3. |
| **[[trading/rejected/auction-theory-range-fade-2026-05\|May 15 auction-theory range-fade family (v1 + v2 both reject)]]** | Illustrates the **semantic-then-mechanism rejection sequence**: v1 failed on VA-vs-range geometric confusion (R:R 0.52 broken before mechanism tested); v2 with corrected geometry failed on continuation-character at new period extremes; v2b ablation ruled out alternative failure modes. **Generalizable methodology learning**: new data layers warrant a **semantic-verification pass** (column-meaning vs assumed-meaning) before mechanism-translation — alongside the existing look-ahead / column-alignment checks per §3. Cross-symbol non-generalization discipline (BTC/ETH/SOL simultaneous test) made the v2 rejection robust against SOL's marginal positive (+0.28) looking like a deployment candidate. |

## 13. Crypto-Specific Concerns Not Addressed by LdP

The book has **no crypto examples**. The translation requires the trader to handle:

1. **Regime non-stationarity** — BTC regimes change every ~6-18 months; multi-year backtests average across structurally different markets. **Mitigation**: regime-conditioned testing per [[foundation/macro/btc-regime-taxonomy-2020-2025|regime taxonomy]] within deploy windows; this is the existing user-memory rule that motivated the no-walk-forward position.
2. **Public-domain idea pool** — crypto strategies discussed on Twitter/Reddit reach saturation faster than equity strategies. Trial count for any "obvious" mechanism (RSI mean-reversion, breakout, etc.) is effectively *much higher* than the trial count of the local research session — possibly thousands of attempts globally.
3. **Funding-rate label leakage** — perp funding rates are public 8 hours in advance; using funding as a feature requires care about *which* funding rate (current settlement vs. next settlement) is in the feature engineering.
4. **On-chain data publication delays** — Glassnode metrics typically lag actual block time by 1-24 hours; "real-time" on-chain features have look-ahead bias if treated as same-bar.

These are wiki-side homework, not LdP-side gaps.

## 14. What This Methodology Does NOT Replace

- **Mechanism articulation** — see Part I §4 and [[foundation/idea-sourcing-methodology]]. Validation discipline is *downstream* of mechanism. A strategy with mechanism but bad statistics fails this gate. A strategy with no mechanism fails the *upstream* gate. Both gates are needed.
- **Cross-market transfer thinking** — see [[foundation/market-structure/crypto-carrier-features]]. Validation does not invent strategies; it filters them.
- **Regime classification** — see [[foundation/macro/btc-regime-taxonomy-2020-2025]]. Validation tests within a regime; classification picks the regime.
- **Position sizing / risk** — see [[investment/allocation/equity-management-rules]]. Validation tests entry/exit logic, not capital allocation.

This page sits between the upstream mechanism pages and the downstream sizing pages. It is the *gate that filters mechanism-grounded ideas into deployment-eligible strategies.*

## Key Takeaways

- **The two laws are the highest-leverage rules** — pre-register hypothesis+falsifier+parameter-budget; report every result with all trials. Adopt before any of the math.
- **Standard k-fold CV is broken in finance** — labels overlap; features are serially correlated; leakage is structural, not avoidable without purging + embargo.
- **Sample uniqueness controls effective N** — overlapping labels reduce statistical power; honest interpretation widens confidence intervals accordingly.
- **The seven sins are baseline hygiene** — necessary but insufficient. Selection bias on multiple testing is the deeper failure mode.
- **PSR adjusts SR for finite-sample non-Normal returns** — useful when track records are short or skew is negative (very common in crypto).
- **DSR adjusts PSR for trial multiplicity** — every parameter sweep, every direction-flip, every prior strategy on the same dataset counts. The graveyard compounds the trial count.
- **PBO via CSCV evaluates batches** — the right tool when 5+ similar strategies are tested; produces a single overfit-probability statistic.
- **Walk-forward single-path is easily overfit** — randomization across paths is required; this is what purged CV provides and what single 70/30 splits do not.
- **The wiki is the trial log** — `trading/rejected/` + `trading/backtest-results/` + `trading/active/` together are the audit trail Marcos' Third Law requires.
- **Operationalization gap is real and named** — wiki encoded ≠ pipeline implemented. The gap is engineering work, not methodology debate.

## Related

- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — primary source for this page
- [[foundation/methodology/deployment-edge-threshold]] — **sister page (added 2026-05-05 PM)**: this page is statistical significance; deployment-edge is economic significance; **both gates must pass** for a strategy to be deployment-grade
- [[foundation/macro/regime-conditional-edge-2025-26]] — empirical W2 finding: 13/13 strategies' edge concentrated in 2025-Q4 + 2026-Q1; operationalizes §7.2 (regime-conditioning) with concrete cross-strategy evidence
- Part IV Quality Standards — the principles this page operationalizes
- Part I §4 Mechanism-First Framing — upstream gate (this is the downstream gate); §4 sharpened 2026-05-05 PM to require `expected_net_edge` (forces deployment-grade thinking upstream of backtest)
- [[foundation/idea-sourcing-methodology]] — sister methodology page (idea sourcing); both are wiki-sweep arm targets
- [[foundation/methodology/investment-thesis-protocol]] — sister page on long-horizon side; trading-side equivalent is this page + idea-sourcing + deployment-edge-threshold
- [[foundation/methodology/bar-level-validation-overlay]] — **added 2026-05-21**: pre-deployment check for strategies gating on period-level signals (regime cells, vol regimes, range membership); applies under §7 anti-overfitting (data-layer pathology is one mechanism by which "good aggregate metrics" mask bar-level mismatch)
- [[foundation/methodology/team-stable-range-detector-methodology]] — worked example of period-level data layer + bar-level overlay refinement; regime_v1 (-$472) → regime_v3 (+$384) is the canonical falsification → fix arc
- [[foundation/methodology/sweep-validation-pattern-taxonomy]] — **added 2026-06-08**: the selection-step companion; four-pattern table for reading a sweep's train-val structure; refines §9.3's "negative correlation = overfit" reading (ambiguous unless both-positive % + DSR also low)
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime classification; validation tests within regimes
- [[trading/rejected/candle-morphology-batch-2026-04]] — concrete failure pattern this methodology addresses
- [[glossary/purged-cross-validation]]
- [[glossary/deflated-sharpe-ratio]]
- [[glossary/probability-of-backtest-overfitting]]
- [[glossary/sample-uniqueness]]

## Sources

- López de Prado, M. (2018). *Advances in Financial Machine Learning*. Wiley. Chapters 4 (sample uniqueness §4.2-4.4), 7 (purged CV), 11 (dangers of backtesting + CSCV/PBO), 14 (PSR/DSR/backtest stats).
- Bailey, D. and López de Prado, M. (2012, 2014b) — PSR, DSR foundation papers (links in source page).
- Bailey, Borwein, López de Prado, Zhu (2014, 2017a) — backtest overfitting / PBO foundation papers (links in source page).
- Luo, Y. et al. (2014) — Seven sins of quantitative investing, Deutsche Bank Markets Research.
- Harvey, Liu, Zhu (2016) — multiple-testing critique of factor zoo.
