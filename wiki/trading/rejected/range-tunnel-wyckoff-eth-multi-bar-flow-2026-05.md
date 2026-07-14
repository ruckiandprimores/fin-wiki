---
title: "range_tunnel ETH (Wyckoff spring/upthrust + taker_buy_z flow filter) — specific operational form REJECTED after v1→v2 mutation cycle (2026-05)"
status: evergreen
created: 2026-05-19
updated: 2026-05-19
tags: [rejected, wyckoff-spring, wyckoff-upthrust, mutation-cycle, edge-saturation-character, operational-form-rejection, multi-bar-pattern, taker-buy-z-flow-filter, phase-conditioned, auction-theory-range-fade-mutation-direction-1, eth, day-swing]
---

# range_tunnel ETH (Wyckoff spring/upthrust + taker_buy_z flow filter) — specific operational form REJECTED after v1→v2 mutation cycle (2026-05)

> **Lead learning**: One specific operationalization of the auction-theory-range-fade rejection's mutation-direction #1 ("retest + rejection") — Wyckoff spring/upthrust with 2-bar pattern + `taker_buy_base_vol / volume` z-score flow filter — **fails on ETH `is_stable=True` phase × 4h × bull window across two exit-config iterations**. v1 (12h time-stop / 2.5 ATR TP): leg-level Sharpe 0.18-0.57; exit-reason decomposition showed 76% WR at time-stop bucket → mechanism-real-but-clipped initial diagnosis. v2 (48h time-stop / 1.5 ATR TP, short-only): exit-config sub-falsifiers all passed, but leg-level Sharpe collapsed to 0.18 with walk-forward aggregate −0.14. **Deeper diagnosis: edge-saturation character** — mechanism produces real K≈3-bar drift then mean-reverts. **Verdict: REJECT this specific operational form on the tested corner.** *NOT* a family-level rejection of mutation-direction #1 — only one flow-filter variant + one multi-bar pattern + two exit configs were tested. See "Mutation directions NOT yet tested" section for what remains valid as future candidates.

## What we learned

1. **Edge-saturation character is a discoverable mechanism-class failure mode.** Distinct from (a) mechanism-doesn't-work (random signal, no positive drift) and (b) mechanism-works-but-needs-tuning (positive but suboptimal exit/entry params). Edge-saturation = mechanism produces real K-bar positive drift, then mean-reverts. Symptoms: exit-config sub-falsifiers pass under mutation, but leg-level Sharpe degrades because the extended window converts captured K-bar wins into losses.

2. **Wyckoff's primary-source multi-day reversal window may not translate to 4h-crypto intra-stable-range mechanics.** Wyckoff describes spring/upthrust reversal windows in DAYS in primary sources. 4h crypto translation at the operational form tested here produced ~3-bar drift then noise — not multi-day continuation. The pre-1980s wisdom prior (#1) needs nuance: timeframe semantics matter, and operational form should be empirically calibrated rather than borrowed from the source's native timeframe. Whether this caveat is OPERATIONAL-FORM-specific (this taker_buy_z + 2-bar combination) or BROADER (any 4h Wyckoff translation) — untested.

3. **Exit-config mutation discipline works mechanically but doesn't rescue edge-saturating mechanisms.** v2's pre-specified sub-falsifiers (TP fire < 20%, time-stop > 40%, SL > 50%) were all met — the mutation did exactly what it was designed to do. This is GOOD methodology surfacing a real limitation: sub-falsifiers validate mechanical operation, not edge magnitude. Both layers must clear for deployment.

4. **Strict-subset filter strategies don't escape the parent strategy's edge ceiling.** v1 and v2 both have 78-80% same-direction co-occurrence with `failed_breakout_phase_fade_eth` (parent strategy, no flow/multi-bar gates). v2 selects fewer trades from the same population without changing per-trade edge character. **The flow filter + multi-bar gate didn't discover a separable "high-quality" subset — they discovered a numerically-smaller subset with similar quality.** Pearson-on-strict-subset note: co-occurrence overlap is the load-bearing metric for asymmetric-N pairs, not per-bar Pearson which understates subset relations.

5. **Side-opinion walk-forward corroborates single-window trade-level decomposition.** v2 4-fold aggregate Sharpe −0.14, only 1/4 folds positive, median fold Sharpe ≈ −0.84. Two independent analyses (exit-reason decomposition vs walk-forward) converge on rejection from different evidence types — meaningful corroboration pattern. Walk-forward = temporal-consistency test (harder bar); trade-level decomposition = mechanism-character diagnosis (deeper attribution).

## Hypotheses (Structured Format)

### v1 hypothesis

```
Hypothesis:    During ETH `is_stable=True` phase, when bar t-1 wicks
               through `range_min_so_far[t-2]` (long) or `range_max_so_
               far[t-2]` (short) and closes back inside, AND bar t
               confirms with price-holding + flow-confirmation
               (`taker_buy_ratio_z > 0.5` long / < -0.5 short),
               informed counterparties have stepped in at the
               stop-cluster level. Trade with them.

Mechanism:     Wyckoff spring (long) + upthrust (short) + Effort-vs-
               Result aggressor flow filter as genuineness test.
               Crypto carriers: liquidation cluster visibility +
               taker_buy_base_vol z-score (one specific modern
               Wyckoff Effort-vs-Result operationalization; others
               possible).

Observable:    ETHUSDT 4h, 2024-05-20 → 2026-05-08. SL 1.0 ATR,
               TP 2.5 ATR, time stop 12h, risk_pct(0.01).

Expected:      Differentiated trigger (multi-bar + flow gate) separates
               Wyckoff "major test" springs/upthrusts from "minor test"
               wicks captured by failed_breakout_phase_fade_eth's single-
               bar trigger. Long-side expected ~0.5-1.0 Sharpe if
               separation works; short-side 1.0-1.5 (inheriting from
               failed_breakout_eth short +$133/N=77).

Falsifier:     Per-leg: SHORT Sharpe < 1.0 / LONG Sharpe < 0.5 on
               N ≥ 30. Cross-strategy correlation ≥+0.6 with
               failed_breakout_phase_fade_eth same-direction = same
               mechanism via different expression.
```

**v1 result**: SHORT Sharpe 0.574 / LONG Sharpe 0.017 (combined Sharpe 0.40, N=65, MDD 9.1%). **Both legs falsifier-fire.** Co-occurrence overlap with failed_breakout_phase_fade_eth: 77-80% same-direction within ±4h (per-bar Pearson +0.013-+0.031 — Pearson rule understates strict-subset relations; capability gap).

**v1 initial diagnosis** (later revised after v2): exit-reason × win/loss × side decomposition showed 76% WR at time-stop bucket — initially read as "mechanism real, exit clipping." Mutation candidate v2 designed to test the time-stop-extension hypothesis. v2 result revealed the deeper diagnosis (edge-saturation; see v2 hypothesis below).

### v2 hypothesis (mutation)

```
Hypothesis:    Same entry trigger as v1 short leg (long dropped per v1
               per-leg falsifier). Mutation: time_stop 12h → 48h to give
               the spring/upthrust mechanism more time to develop;
               TP 2.5 → 1.5 ATR to capture more winners as TP exits
               instead of time-stop.

Mechanism:     Same as v1 short. Hypothesis: 12h time-stop was binding
               constraint — winners CAPPED at bar 3 (75-76% WR per v1
               exit-reason decomposition). Extended window lets the
               slow drift continue to TP magnitude.

Observable:    Same data + capital + fees. SL unchanged at 1.0 ATR.

Expected:      TP fire rate lifts from v1's 7.7% to 25-40%. Time-stop
               fire rate drops from 57% to 30-45%. Sharpe band 0.8-1.4
               if time-stop hypothesis holds.

Falsifier:     Sharpe < 1.0 at N ≥ 30 → leg rejected.
               Sub-falsifiers: TP fire < 20%, time-stop > 40%, SL > 50%.
```

**v2 result**: Sharpe **0.177**, N=32, WR 46.9%, PF 1.08, MDD 3.55%, PSR(>0) 58.3%, PSR(>1) 12.0%. Walk-forward 4-fold aggregate Sharpe −0.14, 1/4 folds positive.

**v2 sub-falsifier outcomes (ALL PASSED — exit-config diagnosis mechanically correct):**

| Sub-falsifier | Bar | v1 → v2 | Verdict |
|---|---|---|---|
| TP fire rate | < 20% fails | 7.7% → 40.6% | PASS |
| Time-stop fire rate | > 40% fails | 57% → 9.4% | PASS |
| SL fire rate | > 50% fails | 37% → 50.0% | PASS (at threshold) |

**v2 cause of failure (named, mechanism-revising)**: edge-saturation character. The v1 bar-3 time-stop wins (76% WR / +$6.76 avg short side) were NOT the beginning of a multi-day continuation drift — they were the local MAXIMUM of mean-reverting noisy drift. v2's extended 48h window converted them:
- 13 trades reached the 1.5 ATR TP at +$14.65 avg ((a) continuation scenario)
- 16 trades reverted past entry and hit SL at -$10.63 avg ((c) reversal scenario per v2's pre-specified adversarial-audit)
- 3 trades stayed time-stop at -$1.90 avg ((b) mean-revert scenario)

**Net per-trade: v1 +$1.62 → v2 +$0.46. Net Sharpe: 0.574 → 0.177.** The (a) gains (+$144 from TP conversion) were almost fully offset by (c) losses (+$57 from SL conversion) and time-stop bucket collapse (−$121). **Within this operational form, the directional edge saturates at K≈3 bars.**

## Coverage summary {#coverage-summary}

This rejection joins the prior auction-theory-range-fade rejection (2026-05-15) as part of a broader mutation-direction-#1 evidence base. Coverage **on the same detector × ETH × 4h × 2024-05/2026-05 bull window**:

| # | Iteration | Trigger family | Exit config | Verdict | Date |
|---|---|---|---|---|---|
| 1 | auction-theory-range-fade v1 (VA-edge-to-POC fade) | Naive single-bar | 12h, POC-based TP | REJECTED | 2026-05-15 |
| 2 | auction-theory-range-fade v2 (literal-range-extreme fade) | Naive single-bar (geometric correction) | 12h, opposite-extreme TP | REJECTED | 2026-05-15 |
| 3 | range_tunnel_bidirectional v1 | Multi-bar + taker_buy_z flow filter | 12h / 2.5 ATR | REJECTED (mechanism-real-but-clipped diagnosis later revised) | 2026-05-18 |
| 4 | range_tunnel short-only v2 | Multi-bar + taker_buy_z flow filter | 48h / 1.5 ATR (short-only) | REJECTED (edge-saturation character; revises v3 diagnosis) | 2026-05-19 |

**Read carefully**: this is **4 strategies tested across 2 distinct trigger families** (naive single-bar + Wyckoff multi-bar + taker_buy_z flow filter), each iterated twice. **NOT 4 independent points in the mutation-direction-#1 space.** The two iterations within each family share most parameters; the four strategies share symbol/detector/TF/window completely.

**What this evidence supports**: "naive single-bar fade fails on this corner" + "multi-bar + taker_buy_z form fails on this corner with edge-saturation character" — strong evidence on these two operational forms, on this corner.

**What this evidence does NOT support**: "mutation-direction #1 is dead at the family level" — that claim would require coverage across the variants enumerated below.

## Mutation directions NOT yet tested {#untested-mutations}

The mutation-direction-#1 space ("retest + rejection") is multi-dimensional. The 4 strategies above cover one operational corner of it. Untested directions that remain valid as future candidates:

### Within the multi-bar + flow-filter operational form

- **Different flow filter** — `volume_z` alone, CVD divergence, OI-delta, funding-rate divergence, on-chain whale flow, taker_buy_z + volume_z stacked. Only `taker_buy_z` was tested.
- **Different multi-bar pattern** — 3-bar (touch + 2-bar recovery), 5-bar, body-vs-wick definitions, intra-bar timing. Only 2-bar (touch + 1-bar recovery) was tested.
- **Different SL geometry** — ATR-adaptive (tighter in low-vol), regime-aware. Only 1.0 × ATR_14 fixed.
- **Regime-break exit** — exit on `~is_stable` transition instead of fixed time-stop. Named in v2 .md as "next mutation candidate"; not implemented.
- **Scale-out partial profits** — e.g., 50% at 1.0 ATR + 50% at 2.5 ATR. Untested.
- **Mid-range time-stops** — 24h, 36h between v1's 12h and v2's 48h. Untested.
- **Patient SL** — no SL until K-bar confirmation. Untested.

### Beyond the operational form

- **Different timeframe** — 1h, 2h (faster than v1's K=3-bar saturation might suggest the mechanism's natural timeframe), or daily ETH (closer to Wyckoff primary timeframe). Only 4h tested.
- **Different symbol** — BTC + SOL not tested at this operational form. BTC has continuation character (predicts fade should fail for opposite reasons); SOL is long-bias regime amplifier (different framing); but the FAMILY-level rejection assumes detector character is the binding constraint, not the symbol — assumption untested.
- **Bear-regime ETH** — 2022-04 → 2023-09, gated on bear ETH parquet with phase columns merged. Bull-only window means this rejection is bull-regime-specific.
- **Different detector tuning** — re-tuned `RollingZigzag` params (the original auction-theory-range-fade rejection page names this as team-side work, not research process mutation). Could find reversion-character periods that contradict the continuation-character finding.
- **Alternate detector entirely** — VWAP-based, volatility-based, market-profile-based. Could identify a different consolidation-character regime where range-fade works.

### Honest scope of the rejection

Future range-fade proposals on the detector × ETH × 4h × bull-window corner should fire prior #6 against the SPECIFIC operational forms tested:

- Naive single-bar fade (covered by [[auction-theory-range-fade-2026-05]])
- Multi-bar + taker_buy_z flow filter at 12h-48h ATR-based exits (this page)

Future proposals operationalizing mutation-direction #1 via UNTESTED variants from the list above do NOT fire prior #6 — they are legitimate next-iteration candidates.

## What this DOESN'T reject {#preserved-not-rejected}

- **`failed_breakout_phase_fade_eth` (single-bar wick-failure trigger)** — separate parent strategy with Sharpe +0.60 / N=166 / short-side +$133. **Operationally distinct from the range_tunnel multi-bar form** — single-bar geometric trigger vs multi-bar + flow gated. The single-bar form's short side is the empirical anchor that motivated range_tunnel; range_tunnel didn't surpass it. The P1.2 short-only ablation of failed_breakout_phase_fade_eth (see [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]]) tests the single-bar form's standalone ceiling — independent of this rejection.

- **The phase data layer itself** — `is_stable`, `range_min/max_so_far`, ATR + flow columns. Mechanically clean; not the failure source.

- **Wyckoff spring/upthrust on OTHER detectors, symbols, timeframes, OR with different multi-bar + flow filter combinations.** This rejection is specific to the operational corner tested. Different detector / symbol / timeframe / flow-filter / multi-bar variants might yield different results. **Future re-proposals MUST articulate which operational corner they target and why it's different.**

- **Wyckoff three laws as analytical framework** — Supply/Demand reading at extremes, Effort-vs-Result diagnostic, Cause-and-Effect range-width-to-move ratio. These are diagnostic frameworks for tape reading; the failure is in the **specific operational form** of automating Effort-vs-Result via `taker_buy_z` z-score at 4h, not in the underlying analytical content.

- **Edge-saturation character as a discoverable signal** — the v1→v2 mutation arc IS the discovery method. Generalizable. Research process task `2026-05-19-exit-config-binding-diagnostic.md` carries this as N=1 codification candidate.

## Cross-symbol non-generalization {#cross-symbol}

Per established codification, ETH findings do NOT auto-generalize to BTC/SOL. For range_tunnel multi-bar + flow-filter form:

- **BTC**: continuation character (per [[../../foundation/asset-classes/btc-market-structure]] codification) — fade-direction is structurally wrong for BTC; `failed_breakout_phase_fade_btc` rejection (N=161) at this batch confirms via single-bar form.
- **SOL**: long-bias regime amplifier — short upthrust mechanism class likely wrong frame; not tested at range_tunnel level.

**No BTC/SOL variant of range_tunnel was built.** The ETH-only rejection of this operational form is symbol-specific; cross-symbol generalization untested in this operational form.

## Mutation directions tested within this operational form {#tested-mutations}

The 2-iteration cycle within `multi-bar + taker_buy_z + ATR exit` form:

1. **v1 trigger differentiation (vs auction-theory-range-fade)** — added multi-bar pattern + flow filter. Result: strict subset of `failed_breakout_phase_fade_eth` parent without edge improvement.
2. **v2 exit-config extension (vs v1)** — extended time-stop, tightened TP. Result: sub-falsifiers passed, edge saturates at K≈3 bars.

For untested mutation directions BEYOND these 2 iterations, see "Mutation directions NOT yet tested" section above.

## Methodological notes

- **The exit-reason × win/loss × side decomposition is a load-bearing diagnostic for mechanism-vs-exit-config attribution.** v1's decomposition correctly surfaced "mechanism real, exit clipping" as the FIRST diagnostic — exit-config sub-falsifiers were the right test. v2 outcome revised this to "edge-saturates at K bars" — the diagnostic chain is: exit-reason decomp → exit-config sub-falsifier passes + leg falsifier still fails → edge-saturation character. **The decomposition is N=1 codification candidate (see research process task).**

- **Sub-falsifier + leg-falsifier two-layer falsifier discipline.** v2's 3 exit-config sub-falsifiers all passing is informative — it validates the v1 diagnostic + the mutation execution. The leg-level falsifier failing on top is a SECOND-LAYER failure: not "mutation didn't work" but "mutation worked, mechanism doesn't have enough edge." **Two-layer falsifier shape is generalizable** for any mutation-cycle proposal. Possible output-shape addition.

- **Walk-forward + trade-level decomposition together is stronger than either alone.** v2 walk-forward (4-fold, side-opinion) gave aggregate Sharpe −0.14 / 1/4 folds positive — temporal-consistency failure. Trade-level decomposition (this entry) gave per-exit-reason × win/loss split — mechanism-character failure. Convergence is corroboration.

- **Premature family-level rejection is a discoverable framing failure.** Initial draft of this rejection claimed N=4 family-level evidence on auction-theory-range-fade. User pushback corrected this to N=4 strategies on one operational corner of the mutation space. **The discipline check: before escalating to family-level claims, enumerate the mutation-direction space breadth tested vs untested.** Codification candidate; N=1 instance noted in research process task queue.

## Related artifacts

- **v1 source**: the backtesting lab (moved 2026-05-19)
- **v1 run**: the backtesting lab
- **v2 source**: the backtesting lab (moved 2026-05-19)
- **v2 run**: the backtesting lab
- **Mutation arc analysis** (the research log 2026-05-18/19)
- **Walk-forward side-opinion**: external (user-provided; 4-fold aggregate Sharpe −0.14)
- **Research process task** (N=1 codification candidates): the research task queue

## Related

- [[auction-theory-range-fade-2026-05]] — parent rejection; this page covers ONE additional operational form within the parent's mutation-direction #1 space, not a family-level escalation
- [[failed-breakout-phase-fade-btc-2026-05]] — sibling rejection from same 2026-05 batch (BTC variant, different cause-of-failure: BTC continuation character)
- [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] — companion arc on ETH; position-blocking discovery completes the why-range_tunnel-failed explanation
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization + strict-subset filter overlap pattern + Pearson-on-asymmetric-N capability gap
- [[../mechanisms/failed-breakout-reversal]] — parent mechanism page; this entry adds nuance about operational-form variants
- [[../mechanisms/regime-gate-vs-bias-filter]] — `is_stable` as conditioning attribute holds within this rejection
- [[../../foundation/market-wisdom/wyckoff-three-laws]] — operational diagnostic; primary-source multi-day timeframe vs 4h-crypto translation caveat (operational-form specific, broader generalization untested)
- [[../../foundation/asset-classes/btc-market-structure]] — per-symbol asymmetry codification; ETH variant has its own asymmetry profile distinct from BTC's continuation character

## Sources

- v1 backtest run + trades.csv exit-reason decomposition (2026-05-18 session)
- v2 backtest run + trades.csv exit-reason decomposition + sub-falsifier verdicts (2026-05-19 session)
- v2 walk-forward analysis (4-fold, user-side-opinion): aggregate Sharpe −0.14, 1/4 folds positive, median fold Sharpe ≈ −0.84
- v1 strategy package: `strategies/tested/range_tunnel_bidirectional_eth_wyckoff_test/range_tunnel_bidirectional_eth_wyckoff_test.md`
- v2 strategy package: `strategies/tested/range_tunnel_short_only_eth_wyckoff_v2/range_tunnel_short_only_eth_wyckoff_v2.md`
- Parent rejection: [[auction-theory-range-fade-2026-05]]
- Wyckoff primary sources for the multi-day-reversal-window framing caveat: [[../../foundation/market-wisdom/wyckoff-three-laws]]
