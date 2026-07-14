---
title: "halving_cycle_post_halving_btc — REJECTED (narrative effect pre-priced or absent in 2024-25 cycle) (2026-05)"
status: evergreen
created: 2026-05-19
updated: 2026-05-19
tags: [rejected, calendar-effects, cyclical-timing, narrative-effect, halving-cycle, btc, stable-phase-mis-conditioned, sma-30-dip-buy, day-swing]
---

# halving_cycle_post_halving_btc — REJECTED (narrative effect pre-priced or absent in 2024-25 cycle) (2026-05)

> **Lead learning**: The halving-cycle dip-buy strategy on BTC — "buy when price dips below SMA(30) within 0-540d post-halving window (2024-04-19 reference)" — fails empirically on 2024-25 data. N=80, Sharpe -0.21, WR 39%. **Halving narrative effect either pre-priced before halving date or absent in 2024-25 cycle.** Per-phase decomposition shows STABLE-phase mis-conditioning (stable -$69/WR 34% / unstable +$29/WR 42% — strategy loses in stable, marginal in unstable). `NOT is_stable` filter inversion (P1.1) is the named mutation candidate but unbuilt. **Verdict: REJECT for deployment; cycle-effect mechanism family preserved for other operationalizations.**

## What we learned

1. **The "halving cycle dip-buy" narrative is not a clean tradeable pattern in 2024-25 BTC.** Pre-1980s wisdom prior says cycle effects exist; the SPECIFIC operationalization (post-halving window × SMA(30) dip-buy) doesn't capture tradeable edge.

2. **Stable-phase mis-conditioning recurs across mechanism classes.** Same diagnostic pattern as `busy_hour_flow_continuation_*` parents: strategy fires uniformly across is_stable / NOT is_stable bars; per-phase split shows mechanism mostly works (or works less-badly) in unstable phase only. `NOT is_stable` filter inversion is the standard salvage candidate; unbuilt for halving as of 2026-05-19.

3. **TP/SL geometry was reasonable; the edge wasn't there.** TP fired 29/80 (36% — healthy band), SL fired 49/80, time-stop 2/80. The geometry isn't broken; the WR at 39% (with breakeven WR depending on R:R) doesn't clear the deployment bar.

4. **Halving narrative analysis is now N+1 evidence in the 2024-25 BTC backtest.** Multiple prior cycles (2016, 2020) had visible post-halving rallies but those rallies started before SMA(30) dip-buy signals would fire reliably. The narrative isn't false; the operationalization doesn't capture the rally entry timing.

## Hypothesis (Structured Format)

```
Hypothesis:    Within the 0-540d post-halving window (BTC halving
               2024-04-19), dips below SMA(30) on 4h chart are buying
               opportunities. The halving narrative produces "buy the
               dip" institutional + retail flow that supports recovery.

Mechanism:     Calendar-effect / cyclical-timing. Narrative-driven
               dip-buy flow. Phase-data-layer regime conditioning
               implicit via SMA threshold.

Observable:    BTCUSDT 4h, post-halving window 2024-04-19 + 540d,
               long-only entries on SMA(30) crossbelow + close-back-
               above. SL 1.0 ATR / TP 2.5 ATR / time stop 8h.

Expected:      Sharpe 1.0-1.5 if narrative produces tradeable flow.

Falsifier:     Sharpe < 0.5 → reject.
```

**Result**: N=80, Sharpe **-0.21**, WR 39%, Long-only, Net -$39, MDD 9.9%, PSR(>0) 36.3%, PSR(>1) 4.2%, Sharpe CI [-1.78, +1.13]. **Falsifier fires.**

**Per-phase split** (cross-cutting finding):
- Stable: N=35 / -$69 / WR 34% — mechanism loses in stable phase
- Unstable: N=45 / +$29 / WR 42% — mechanism marginal positive in unstable phase

**TP/SL fire**: TP 29/80 (36% — healthy), SL 49/80 (61%), time_stop 2/80 (2%). TP geometry is workable; the win rate is just below the breakeven needed for the R:R.

**Cause of failure (named)**: halving narrative effect is either:
- Pre-priced (the 2024-04 halving's expected effect was already in price before the halving date; dip-buys in the post-window don't capture incremental flow)
- Absent in 2024-25 cycle (different from 2016/2020 cycles due to ETF infrastructure changing institutional flow dynamics)
- Captured by other operationalizations not tested here (e.g., halving-date-anchored accumulation rather than SMA-dip-buy)

## What WOULD make halving cycle work (untested mutations) {#mutation-directions}

1. **`NOT is_stable` filter (P1.1)** — restrict entries to trending unstable-phase bars where the marginal-positive subset is. UNBUILT in this batch.

2. **Different dip-buy condition** — distance from halving date as continuous variable rather than binary 0-540d window. E.g., months 1-3 post-halving might have higher dip-buy edge than months 12-18.

3. **Different threshold** — SMA(30) is one calibration; SMA(50) or RSI-based dip-buy might capture different dips.

4. **Cycle-anchored entries with multi-cycle data** — train on 2016 + 2020 cycles, test on 2024. UNBUILT; would require multi-cycle parquet ingest (currently lab has bull-window 2024-26 only).

5. **Different cycle metric entirely** — on-chain hodl-wave continuation, miner-flow indicators, supply-shock-after-halving accumulation. The halving narrative has multiple operational expressions; this rejection covers only the SMA-dip-buy expression.

## What this rejects

- The SPECIFIC operational form: SMA(30) dip-buy in 0-540d post-halving window on 4h BTC.
- The naive "halving narrative IS tradeable via dip-buy" prior on 2024-25 BTC bull-window evidence.

## What this DOESN'T reject

- The halving-cycle MECHANISM CLASS broadly. Multi-cycle data + different operationalizations remain valid future candidates.
- The calendar-effects mechanism family (per [[../mechanisms/calendar-effects]]). Halving is one calendar effect among many; this rejection is operational-form-specific.
- The BTC dip-buy mechanism class. Non-halving-conditioned dip-buys (SMA breakout buys, support-level buys) are separate hypotheses.
- The `DAYS_SINCE_DATE` primitive (validated mechanically — 80 entries fired correctly in the post-halving window). The primitive works; the strategy hypothesis doesn't.

## Methodological notes

- **Single-cycle backtest is intrinsically limited for cycle-effect strategies.** Pre-1980s wisdom prior says cycle effects vary across cycles (1929 vs 1949 vs 2000 vs 2008 — same broad framework, different operational manifestations). One cycle (2024-25 BTC post-halving) is N=1 evidence for the cycle-effect framework. Robust falsification of the cycle hypothesis would require multi-cycle evidence.
- **The stable-phase mis-conditioning is a recurring pattern across 2026-05-18 batch.** Halving + busy_hour_btc + busy_hour_sol + options_expiry all showed the same per-phase split direction (loses stable / wins unstable). The pattern suggests the `is_stable` detector identifies fundamentally different market regimes for narrative/continuation strategies. Worth flagging as cross-strategy pattern in the research arms backtest-decomposition arm (potential codification).

## Related artifacts

- Strategy: the backtesting lab (moved 2026-05-19)
- Backtest: the backtesting lab
- 2026-05-18 batch decomposition: the research task queue Tier 4 section

## Related

- [[../mechanisms/calendar-effects]] — parent mechanism family page; halving is one calendar effect
- [[../mechanisms/cyclical-timing]] — cyclical-effect framework
- [[../mechanisms/regime-gate-vs-bias-filter]] — stable-phase mis-conditioning; `NOT is_stable` mutation candidate
- [[../../foundation/asset-classes/btc-market-structure]] — BTC-specific cycle behavior context (separate codification task)
- [[options-expiry-friday-btc-2026-05]] — sibling rejection (similar mis-conditioning pattern, different mechanism family)

## Sources

- 2026-05-18 batch backtest (`runs/halving_cycle_post_halving_btc_20260518_*/`)
- Per-strategy decomposition: the research task queue
