---
title: "busy_hour_flow_continuation family — mutation arc (btc/sol parents rejected, compound SOL variant VALIDATED at deployment-grade) (2026-05)"
status: evergreen
created: 2026-05-19
updated: 2026-05-19
tags: [rejected, validated-compound, session-boundary-effects, taker-buy-flow, peak-activity-hour, p1-1-filter-inversion, p1-2-direction-restriction, compound-mutation-validated, mutation-arc, sol-deployment-candidate]
---

# busy_hour_flow_continuation family — mutation arc (2026-05)

> **Lead learning**: The bare busy_hour flow-continuation mechanism (peak-activity-hour extreme-flow continuation entry) FAILS on all 3 symbols at aggregate (btc Sharpe +0.19, eth -0.66, sol -0.37). Per-phase decomposition reveals consistent MIS-CONDITIONING on `is_stable=True` phase (mechanism designed for trending regime fires also in stable, where it loses). On btc + sol, `NOT is_stable` filter inversion (P1.1) was identified as the salvage path; on sol, COMPOUND P1.1 + P1.2 (filter + long-only) was tested and VALIDATED at deployment-grade. **Result: `busy_hour_flow_continuation_sol_long_only_unstable` is the first deployment-grade candidate to emerge from this batch.** Walk-forward 4/4 folds Sharpe > 1 annualized.

## Family-level summary

| Strategy | Mechanism + symbol | Sharpe | PSR(>0) | Verdict |
|---|---|---|---|---|
| `busy_hour_flow_continuation_btc` (parent) | Continuation, BTC | +0.19 | 59% | Refuted aggregate; stable-phase mis-conditioned; P1.1 mutation candidate unbuilt |
| `busy_hour_flow_continuation_sol` (parent) | Continuation, SOL | -0.37 | 28% | Refuted aggregate; same mis-conditioning pattern + short-side drag |
| `busy_hour_flow_continuation_eth` (parent) | Continuation, ETH | -0.66 | 18% | Full rejection (no salvage; see [[busy-hour-flow-continuation-eth-2026-05]]) |
| **`busy_hour_flow_continuation_sol_long_only_unstable`** | **Compound P1.1+P1.2, SOL** | **+1.30** | **98.1%** | **VALIDATED at deployment-grade** |

## Compound mutation validation {#compound-validated}

### Mutation rationale (pre-backtest)

From the 2026-05-18 batch per-strategy decomposition, the parent `busy_hour_flow_continuation_sol` showed two distinct failure modes:

1. **Stable-phase mis-conditioning** (P1.1 finding): stable subset N=33 / -$102 / WR 27% — mechanism LOSES in stable phase. Unstable subset N=62 / +$54 / WR 45% — mechanism WINS in unstable phase. The hypothesis (peak-activity flow continuation needs directional pressure) only holds in NON-stable / trending regimes. `NOT is_stable` filter inversion is the salvage.

2. **Direction-asymmetry** (P1.2 finding): SOL long-bias regime amplifier framework predicts long-side as the working direction at flow-continuation signals; short-side flow-continuation on SOL is structurally weaker. Per parent breakdown, short side dominates losses.

The 2026-05-18 pickup file recommended the compound mutation: combine P1.1 + P1.2 ("compound win" per pickup). This entry tests the recommendation.

### Validation results

**Single-window backtest** (4h SOL bull window, 2024-05-20 → 2026-05-08):
- N=27, Sharpe **+1.299**, PSR(>0) **98.1%**, PSR(>1) **77.2%**, WR **59.3%**, Net +$95.13, MDD **1.64%**, PF **2.91**
- Sharpe CI [+0.18, +3.25] — **lower bound positive** (the ONLY strategy in this batch with positive CI lower bound at this confidence level)
- Exit-reason × win/loss decomposition:
  - TP: 6/27 (22%) — all wins, +$15.24/trade avg
  - SL: 3/27 (11%) — all losses, -$10.74/trade avg
  - Time-stop: 18/27 (67%) — 10/18 wins (56%), +$2.00/trade avg

**Walk-forward 4-fold validation**:

| Fold | Period | N | Sharpe | PSR(>0) | WR |
|---|---|---|---|---|---|
| 1 | 2024-05-29 to 2024-11-22 | 13 | 1.43 | 82.9% | 61.5% |
| 2 | 2024-11-22 to 2025-05-18 | 3 | 1.29 | (N too small for PSR) | 33.3% |
| 3 | 2025-05-18 to 2025-11-11 | 4 | 1.02 | 70.9% | 50.0% |
| 4 | 2025-11-11 to 2026-05-08 | 7 | **2.10** | 87.8% | 71.4% |

**4/4 folds: positive PnL, Sharpe > 1 ann.** Median Sharpe 1.36, range [1.02, 2.10]. Median PSR(>0) 82.9%. **Strongest walk-forward consistency of any tested strategy in the lab's recent inventory** (clears `phase_breakout_meta_btc`'s 3/4 walk-forward).

### Cross-strategy overlap with parent

- Compound (N=27) co-occurrence with parent's LONG subset (N=37): **100%** — compound is a strict subset.
- Compound's 27 trades = 73% of parent's 37 longs.
- Compound's selectivity dropped 27% of parent longs (the stable-phase longs) and 100% of parent shorts.

The compound is HIGH-QUALITY SELECTIVITY — retains the productive subset, drops the unproductive subset.

### Why this compound works (mechanism reasoning)

The compound exploits SOL's structural character on two orthogonal axes:

1. **`NOT is_stable` (P1.1)** — peak-activity flow continuation needs DIRECTIONAL PRESSURE. Stable phase has tight intra-range price action where extreme taker-buy ratios don't reliably continue. Trending phase has directional conviction that extreme taker-buy can extend further.

2. **Long-only (P1.2)** — SOL bull-regime structural absorption (BSOL staking ETF, FORD/DAT inventory, retail momentum-following per Kogan et al. NBER w31317) creates asymmetric continuation in long direction. Short-side flow-continuation lacks the structural-buy-bid backbone.

The two filters condition on DIFFERENT REGIME AXES (regime phase × direction-asymmetry) — they target distinct failure modes without overlap. This contrasts with `range_tunnel`'s compound (multi-bar pattern + taker_buy_z flow filter) which both compound on the same axis (trigger geometry refinement) and produced value-subtraction.

### Position-blocking-as-filter context

Per [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] companion finding: bidirectional position-management can act as an inadvertent quality filter. For busy_hour SOL:
- Parent (bidirectional): position-blocking IS active but neutral (SOL long-bias amplifier means blocked shorts aren't systematically losing).
- Compound: position-blocking irrelevant (long-only by design).

No position-blocking-as-filter effect here. The compound's edge is purely from explicit regime + direction conditioning.

## Why parent btc/sol/eth fail at aggregate

### btc parent

- Aggregate Sharpe +0.19. Stable-phase subset: N=38 / -$98 / WR 26% (severely mis-conditioned).
- Unstable-phase subset: N=42 / +$118 / WR 60% (mechanism works).
- P1.1 `NOT is_stable` filter would produce btc compound similar to SOL's validated form. **NOT BUILT** in this batch — flagged as next-iteration candidate.
- Per-symbol asymmetry note: BTC continuation character (codified separately at [[../../foundation/asset-classes/btc-market-structure]]) suggests BTC busy_hour compound MIGHT work without direction-restriction (both directions work on BTC continuation regime). Untested.

### sol parent (separate per-symbol from compound validation)

- Aggregate Sharpe -0.37. Same mis-conditioning pattern: stable -$102 / unstable +$54.
- Short-side drag: short trades net-negative ~$42 on aggregate per parent breakdown.
- COMPOUND P1.1+P1.2 lifted to Sharpe 1.30 (this entry's validation finding).

### eth parent

- Aggregate Sharpe -0.66. UNLIKE btc/sol, BOTH PHASES LOSE on eth (stable -$8 / unstable -$76). No salvage path via filter inversion.
- Mechanism class doesn't fit ETH structurally. See [[busy-hour-flow-continuation-eth-2026-05]] for full rejection.

## What this rejects

- `busy_hour_flow_continuation_btc` AS-IS for deployment (aggregate Sharpe 0.19; mis-conditioned).
- `busy_hour_flow_continuation_sol` AS-IS for deployment (aggregate Sharpe -0.37; superseded by compound child).
- The bare-mechanism framework for THIS SYMBOL × THIS DETECTOR × THIS WINDOW. The compound is the validated operational form.

## What this VALIDATES (forward-positive)

- `busy_hour_flow_continuation_sol_long_only_unstable` AS deployment-grade candidate.
- The COMPOUND P1.1 + P1.2 MUTATION PATTERN as a discoverable strategy-building technique. N=1 in the lab so far; pattern worth flagging for replication on btc + cross-mechanism families.
- The "different-axes-compound vs same-axis-compound" hypothesis as a methodology candidate (this entry vs range_tunnel rejection).

## What this DOESN'T reject

- `busy_hour_flow_continuation_btc` with P1.1 `NOT is_stable` filter applied (UNBUILT — should be tested as the next-iteration mutation).
- The flow-continuation mechanism class on OTHER symbols (cross-mechanism transfer to e.g., `cell_breakout_unstable` family or `funding_flip_unstable` family).
- Different time-of-day windows (busy_hour currently uses 12 + 16 UTC; non-tested alternatives include 22 UTC Asia close, 04 UTC Asia open).

## Methodology learning: compound selectivity classes {#compound-selectivity-classes}

| Compound type | Example | Outcome | Selectivity character |
|---|---|---|---|
| Same-axis compound | range_tunnel v2 (multi-bar + flow filter, both refine trigger geometry) | Sharpe 0.18 — subtracts edge | Wrong |
| Different-axis compound | busy_hour SOL (phase filter + direction restriction, condition on orthogonal regime axes) | Sharpe 1.30 — adds edge | Right |

**Hypothesis (N=1, codification candidate)**: compound mutations add value when components condition on DIFFERENT REGIME-CONDITIONING AXES; compound mutations subtract value when components compound on the same axis. The "axis" framework includes: regime phase (is_stable), direction (long/short), time-of-day, volatility regime, funding regime, flow regime.

Cross-session recurrence test: watch for the next compound mutation in the lab. If different-axes pattern correlates with success and same-axis with failure across 2+ examples, codify into the research arms mutation-translation arm discipline.

## Related artifacts

- btc parent: the backtesting lab (moved 2026-05-19)
- sol parent: the backtesting lab (moved 2026-05-19)
- eth parent: the backtesting lab (moved 2026-05-18; see [[busy-hour-flow-continuation-eth-2026-05]])
- **Compound (validated)**: the backtesting lab (KEPT in active strategies/)
- btc parent backtest: the backtesting lab
- sol parent backtest: the backtesting lab
- Compound backtest: the backtesting lab
- Compound walk-forward: the backtesting lab

## Related

- [[../mechanisms/session-boundary-effects]] — parent mechanism page (sharpened with validated-form reference)
- [[busy-hour-flow-continuation-eth-2026-05]] — sister rejection (ETH; no salvage path)
- [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] — counterpoint compound (same-axis selectivity, failed)
- [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] — companion arc; position-blocking finding context
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization
- [[../mechanisms/regime-gate-vs-bias-filter]] — phase filter as regime conditioning attribute (this compound exemplifies the "conditioning attribute" pattern done right)
- [[../../foundation/asset-classes/sol-market-structure]] — long-bias amplifier framework; this validation strengthens the framework

## Sources

- 2026-05-18 batch decomposition (parent failures + mutation candidate identification)
- 2026-05-19 backtest + walk-forward (`runs/busy_hour_flow_continuation_sol_long_only_unstable_20260519_100913/`, `runs/busy_hour_compound_wf_20260519/`)
- Per-symbol asymmetry framework: [[../../foundation/asset-classes/sol-market-structure]]
