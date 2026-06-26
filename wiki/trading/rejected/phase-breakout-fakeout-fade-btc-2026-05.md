---
title: "phase_breakout_fakeout_fade (BTC) — REJECTED at N=11; companion to phase_breakout_meta_btc continuation (2026-05)"
status: evergreen
created: 2026-05-18
updated: 2026-05-19
tags: [rejected, fakeout-fade, phase-exit-trigger, btc-continuation-character, tiny-N, companion-finding, mechanism-class-symbol-mismatch, day-swing]
---

# phase_breakout_fakeout_fade (BTC) — REJECTED at N=11; companion to phase_breakout_meta_btc continuation (2026-05)

> **Lead learning**: On the same phase-EXIT trigger bars where `phase_breakout_meta_btc` produces the strongest PSR clearer in the 14-strategy batch (Sharpe 1.65, PSR 97.5%), the opposite-direction fade variant fails — Sharpe −0.37, N=11, 7 of 11 trades stop out, no TP fires. **The A/B at the trigger level is the load-bearing finding**: at phase exits on BTC, continue with the break; never fade it. Direct contributor to BTC continuation character codification (N=3 evidence, this is contributing-but-not-primary; primary is [[failed-breakout-phase-fade-btc-2026-05]] at N=161).

## What we learned

1. **Phase-exit on BTC is unambiguously a continuation moment.** Same trigger bars (phase ENDING, stable period transitioning to expansion), same direction logic (the OBSERVED break direction), opposite trade direction → opposite outcome. Cleanest possible A/B at the trigger level.

2. **Sample size is tiny but the prior is strong.** N=11 is below the falsifier-floor for newly-proposed strategies (N=30 default). The rejection is partial evidence on its own; combined with `failed_breakout_phase_fade_btc` (N=161) and `busy_hour_flow_continuation_btc` (stable-phase inversion at N=80), the BTC continuation character claim is N=3 cross-mechanism evidence.

3. **Per-symbol mechanism asymmetry confirmed across two independent mechanism classes.** Both `failed_breakout_phase_fade` and `phase_breakout_fakeout_fade` work on ETH and fail on BTC. The pattern is repeatable across mechanism class, not noise from one strategy's specific implementation.

4. **The companion success is what makes this rejection informative.** `phase_breakout_meta_btc`'s 97.5% PSR(>0) on continuation at the SAME trigger bars is the contrast that makes "phase-exit on BTC is continuation, not fade" a clean structural finding rather than just "this particular strategy didn't fire enough".

## Hypothesis (Structured Format)

```
Hypothesis:    When a stable-phase period ENDS (transition to expansion),
               the breakout direction is often a fakeout — price returns
               to range mid in the following bars. Fade the break
               direction.

Mechanism:     Wyckoff exhaustion-at-trend-end (terminal phase of stable
               accumulation = distribution before reversal). Crypto
               carrier: liquidation-cascade-on-breakout that fizzles
               out without follow-through.

Observable:    BTCUSDT 4h, 2024-05-20 → 2026-05-08. Entry: when stable_just_
               ended fires (phase transition), enter OPPOSITE the
               breakout direction. SL / TP / time stop matching companion
               continuation strategy.

Expected:      If BTC at phase exits has fakeout-fade character (like
               ETH), Sharpe 0.5-1.5 expected. Per ETH companion's
               working variant (phase_breakout_fakeout_fade_eth Sharpe
               +1.32), generalization could plausibly carry.

Falsifier:     Sharpe < 0.5 on BTC at N ≥ 10 → reject. Specifically test
               whether BTC has fade or continuation character at phase
               exits (cross-symbol asymmetry hypothesis).
```

**Result**: Sharpe −0.37, N=11, WR 27%, Long 4 / +$10 / WR 50%, Short 7 / -$28 / WR 14%. TP 0 / SL 7 / time_stop 4. MDD 3.9%. Falsifier fires.

**Cause of failure (named)**: BTC at phase EXITS has continuation character (per N=3 codification). The fade direction is structurally wrong. The 4 long trades (fading down-breaks) marginally make money (+$10/WR 50%) — probably because phase-exit down-breaks on BTC during bull regime have some reversion bias from structural ETF buy demand. But the 7 short trades (fading up-breaks) lose hard (-$28/WR 14%) — phase-exit up-breaks on BTC continue strongly, consistent with the continuation character finding.

The asymmetry in the failed sample (long-side marginally positive, short-side strongly negative) suggests a RESIDUAL salvage candidate: long-only at phase-exit down-breaks during bull-regime BTC. But N=4 on the long side is far too small; not actionable on this evidence.

## Why this DOESN'T contradict the ETH companion {#eth-asymmetry-explained}

The ETH variant of phase_breakout_fakeout_fade (`phase_breakout_fakeout_fade` on ETH, tested in earlier batches at Sharpe ~1.32) WORKS. This isn't a contradiction — it's the per-symbol asymmetry codification doing what it predicts:

- **ETH fakeout-fade character** → fade variant works on ETH
- **BTC continuation character** → continuation variant works on BTC (phase_breakout_meta_btc at PSR 97.5%); fade variant fails on BTC (this rejection)

Same trigger bars, opposite character per symbol, opposite winning direction per symbol. This is exactly what the per-symbol mechanism asymmetry framework predicts. Cross-symbol non-generalization codification reinforced.

## What WOULD make a BTC phase-exit fade work (not tested) {#mutation-directions}

1. **Long-only variant on bull-regime BTC at down-break events** — the N=4 long-side trades in this rejection were marginally positive (+$10/WR 50%). Possible salvage path: structural ETF buy demand may produce some reversion-upward on phase-exit down-breaks. Gated on N≥30 evidence; not actionable from this sample alone.

2. **Cross-regime test in bear BTC** — bear regimes have more flush-and-recover behavior; phase exits in bear regimes might fade more often. Gated on bear BTC parquet with phase columns merged.

3. **Drop the family on BTC** — BTC's continuation character is N=3 evidence. The fade variant is a wrong-symbol mechanism class. Capacity better spent on continuation triggers (phase_breakout_meta_btc family expansion).

## Methodological notes

- **Tiny-N rejections are still useful** when paired with companion success at the same trigger. The cleanest A/B test in the 2026-05-18 batch was this strategy vs phase_breakout_meta_btc — same trigger bars, opposite direction. Even at N=11, the comparison surfaces the continuation finding cleanly.
- **Per-direction asymmetry within a tiny rejected sample is investigable but not actionable.** N=4 long-side marginally positive is INSUFFICIENT for a long-only salvage variant deployment, but it's a useful hypothesis seed.

## Related artifacts

- Strategy package: the backtesting lab (moved to tested 2026-05-18)
- Companion success: the backtesting lab (continuation direction; PSR 97.5%)
- Backtest artifacts: the backtesting lab
- ETH variant (separate sibling): tested earlier, Sharpe ~1.32 — proves the per-symbol asymmetry

## Related

- [[../../foundation/asset-classes/btc-market-structure]] — codification this contributes to (N=3 evidence)
- [[failed-breakout-phase-fade-btc-2026-05]] — N=161 sister rejection; same BTC-character finding via different mechanism
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization
- [[../mechanisms/failed-breakout-reversal]] — mechanism family page

## Sources

- 2026-05-18 14-strategy batch (the backtesting lab)
- Companion success: the backtesting lab
- Per-symbol asymmetry codification: [[../../foundation/asset-classes/sol-market-structure]]
- Batch decomposition: the research task queue Tier 4 section
