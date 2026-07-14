---
title: "busy_hour_flow_continuation (ETH) — REJECTED; mechanism class doesn't fit ETH (2026-05)"
status: evergreen
created: 2026-05-18
updated: 2026-05-19
tags: [rejected, flow-continuation, session-boundary-effects, peak-activity, eth-fakeout-fade-character, mechanism-class-symbol-mismatch, both-phases-lose, day-swing]
---

# busy_hour_flow_continuation (ETH) — REJECTED; mechanism class doesn't fit ETH (2026-05)

> **Lead learning**: The peak-activity-hour flow-continuation mechanism (extreme one-sided taker flow at busy-hour bars = real directional pressure to trade with) fails on ETH in BOTH stable AND unstable phases — Sharpe -0.66, N=94, both phase-conditioning splits negative. Unlike the BTC and SOL siblings (which fail in stable phase but profit in unstable phase, giving `NOT is_stable` filter inversion as a salvage path), ETH has no phase-conditioning rescue. **The mechanism class doesn't fit ETH structurally**, plausibly because ETH's fakeout-fade character means peak-activity flow extremes get FADED rather than continued. **Verdict: REJECT on ETH; the inverse mechanism (`busy_hour_flow_FADE_eth`) is a future-candidate that this rejection's evidence directly motivates.**

## What we learned

1. **Per-symbol asymmetry has an EXCLUSION axis, not just a direction axis.** The codification (per [[../../foundation/asset-classes/sol-market-structure]]) established that mechanisms work asymmetrically per symbol. This rejection shows that some mechanism classes don't transfer to ETH AT ALL — there's no phase-conditioning salvage when the mechanism fails on both stable AND unstable phases.

2. **`NOT is_stable` filter inversion is NOT a universal salvage.** The 4 misconditioned strategies in the 2026-05-18 batch (busy_hour_btc, busy_hour_sol, halving_cycle, options_expiry) all expected to gain from filter inversion in P1.1 free-wins. But the busy_hour_ETH variant DOESN'T qualify — both phase splits are negative. Filter inversion is a salvage when one phase works; it isn't a salvage when neither does.

3. **Mechanism-class-symbol-mismatch produces both-phases-lose pattern.** When a mechanism class fundamentally doesn't fit a symbol's structural character, no phase / regime / time-of-day filter rescues it. The rescue requires the inverse mechanism (faded direction), not a tighter trigger.

4. **The inverse mechanism candidate is named** — `busy_hour_flow_FADE_eth`: enter in the OPPOSITE direction of the peak-activity taker flow extreme. If ETH's fakeout-fade character holds at session-boundary bars too, this should work. Untested.

## Hypothesis (Structured Format)

```
Hypothesis:    At peak-activity hours (high taker-flow imbalance bars),
               extreme one-sided aggressor flow indicates real
               directional pressure. Enter long when taker_buy >> taker_sell
               (continuation of the buy pressure); enter short when
               taker_sell >> taker_buy (continuation of the sell pressure).

Mechanism:     Microstructure flow-continuation. Heavy taker pressure
               at peak-activity bars (London open, NY open, Asia close)
               reflects committed institutional / large-flow execution
               that will not reverse within typical 4h-bar horizon.

Observable:    ETHUSDT 4h, 2024-05-20 → 2026-05-08. Entry at HOUR_OF_DAY
               windows matching peak-activity hours, when taker_ratio_z
               above (long) or below (short) thresholds. SL / TP via
               ATR multipliers.

Expected:      Cross-symbol generalization from BTC/SOL siblings (where
               mechanism works in unstable phase at +$118 / +$54
               respectively) would suggest ETH Sharpe 0.4-1.0 in
               unstable phase. Aggregate Sharpe 0.3-0.7 expected.

Falsifier:     Sharpe < 0.3 on ETH aggregate AND no positive phase-split
               salvage. (Per BTC/SOL pattern, "stable-phase loses" is
               okay if "unstable-phase wins" — only both phases losing
               kills the mechanism on this symbol.)
```

**Result**: Sharpe -0.66, N=94, WR 41%, Long 36 / -$42, Short 58 / -$42. PSR(>0) 18.4%, PSR(>1) 1.2%, MDD 11.5%. Stable split: stable N=41 / -$8 / WR 44%, unstable N=53 / -$76 / WR 40%. **Falsifier fires on both phases.**

**Cause of failure (named)**: ETH's structural character makes peak-activity flow CONTINUATION the wrong direction. Hypothesis: ETH's fakeout-fade character (codified per per-symbol asymmetry framework — strong N=2 evidence from cell_bull_exhaustion_eth + phase_breakout_fakeout_fade ETH) extends to session-boundary peak-activity bars. Peak-activity flow extremes on ETH probably REPRESENT exhaustion / institutional unwind moments rather than continuation moments. Trading WITH the flow → trading INTO the unwind → losses.

The short side carries marginally MORE loss than long side (-$42 each in this case; on the BTC sibling, short side ALSO dominates losses -$42). The pattern across siblings is: short-side at peak-activity bars loses worst on both ETH and BTC's bull-regime windows. This may reflect bull-regime structural-bid absorption that fades shorts more aggressively than longs across both symbols. ETH-specific failure is the long-side ALSO losing, which BTC unstable-phase doesn't show.

## Cross-symbol comparison {#cross-symbol}

| Strategy | Symbol | Sharpe | N | Stable | Unstable | Salvage path |
|---|---|---:|---:|---|---|---|
| busy_hour_flow_continuation_btc | BTC | +0.19 | 80 | -$98 / WR 26% | **+$118 / WR 60%** | `NOT is_stable` filter (P1.1 pending) |
| busy_hour_flow_continuation_sol | SOL | -0.37 | 95 | -$102 / WR 27% | **+$54 / WR 45%** | `NOT is_stable` filter (P1.1 pending) |
| busy_hour_flow_continuation_eth | ETH | **-0.66** | 94 | **-$8 / WR 44%** | **-$76 / WR 40%** | **NONE — both phases lose** |

ETH is the structural outlier. BTC and SOL both show "stable-phase loses, unstable-phase wins" — meaning the mechanism works when there's directional pressure in the market (unstable phase) but fires the wrong direction during quiet ranges. ETH's failure mode is different: BOTH phases lose, suggesting the mechanism direction itself is wrong for ETH regardless of regime context.

## What this DOESN'T reject {#preserved-not-rejected}

- **The BTC/SOL siblings** — `NOT is_stable` filter inversion salvage path remains open and pending P1.1 free-wins testing (subsequently landed; see [[busy-hour-flow-continuation-family-2026-05]] for the family arc).
- **The peak-activity-hour identification mechanism** — HOUR_OF_DAY primitive and the session-boundary framing are validated; the trigger fires at correct hours.
- **The inverse-direction candidate** — `busy_hour_flow_FADE_eth` is a future-test candidate, not a re-proposal of this rejection. When peak-activity taker flow is extreme on ETH, FADE the direction (enter opposite). Worldview prior #6 obligation met by direction inversion.

## What WOULD make a busy-hour mechanism work on ETH (not tested) {#mutation-directions}

1. **Inverse direction** (highest-EV) — `busy_hour_flow_FADE_eth`. Trade OPPOSITE the peak-activity taker flow extreme. ETH's fakeout-fade character predicts this should work; mechanism class is the same family as cell_bull_exhaustion_eth and phase_breakout_fakeout_fade ETH (both work on ETH).

2. **Different time windows** — current ETH variant uses same HOUR_OF_DAY windows as BTC/SOL siblings. ETH's institutional flow timing may differ (more DeFi / European-trading-hour weighted than BTC's institutional / US-trading-hour weighted). A custom ETH-specific session window might isolate moments when continuation DOES work.

3. **Cross-regime test** (bear ETH, 2022-04 → 2023-09) — bear regimes have different flow dynamics; the mechanism MIGHT work in bear ETH where flush-and-continue patterns are more common. Gated on bear ETH parquet with phase columns merged.

4. **Drop continuation framing on ETH entirely** — the per-symbol asymmetry codification + this rejection together suggest ETH's structural character is fade-dominant. Continuation mechanism class capacity better spent on BTC (where N=3 evidence supports it).

## Methodological notes

- **Both-phases-lose is the diagnostic signature for mechanism-class-symbol mismatch.** When phase-conditioning splits both negative, no regime filter or direction restriction within the mechanism rescues it. The rescue path requires inverting the mechanism's directional logic OR moving to a different symbol where the character matches.
- **Cross-symbol simultaneous testing surfaces exclusions cleanly.** Without the BTC and SOL siblings' stable/unstable splits showing the working salvage pattern, ETH's both-phases-lose would have looked like a parameter-tuning problem. The cross-symbol context made it clear that ETH is structurally excluded from this mechanism class.

## Related artifacts

- Strategy package: the backtesting lab (moved to tested 2026-05-18)
- BTC sibling (salvage path open): the backtesting lab (P1.1 `NOT is_stable` pending)
- SOL sibling (salvage path open): the backtesting lab (P1.1 `NOT is_stable` + P1.2 long-only pending)
- Backtest artifacts: the backtesting lab

## Related

- [[../mechanisms/session-boundary-effects]] — mechanism family page; ETH exclusion noted
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization codification
- [[failed-breakout-phase-fade-btc-2026-05]] — companion per-symbol asymmetry exclusion (BTC fails fade family; ETH fails continuation family)
- [[busy-hour-flow-continuation-family-2026-05]] — family-level arc (BTC parent rejected + SOL P1.1+P1.2 compound VALIDATED at deployment grade)
- [[../../foundation/asset-classes/sol-market-structure]] — per-symbol asymmetry framework sister page
- [[../../foundation/asset-classes/btc-market-structure]] — BTC continuation character codification

## Sources

- 2026-05-18 14-strategy batch (the backtesting lab)
- Cross-symbol comparison: the research task queue Tier 4 section + cross-cutting "Stable-phase enrichment" table
- Per-symbol asymmetry codification: [[../../foundation/asset-classes/sol-market-structure]]
