---
title: "options_expiry_friday_btc — REJECTED (gamma release in stable regime doesn't carry; needs trending pressure) (2026-05)"
status: evergreen
created: 2026-05-19
updated: 2026-05-19
tags: [rejected, calendar-effects, options-gamma-perp-effects, options-expiry, day-of-week, friday-effect, btc, stable-phase-mis-conditioned, day-of-week-primitive-validated]
---

# options_expiry_friday_btc — REJECTED (2026-05)

> **Lead learning**: Friday 08 UTC post-expiry release on BTC was hypothesized to produce Sharpe 0.7-1.2 (gamma decay + dealer rehedge flow into the post-expiry trading window). **Actual: Sharpe +0.15, N=47, WR 43%** — well below expectation. Per-phase decomposition reveals the mis-conditioning: stable N=25 / -$35 / WR 36% (loses), unstable N=22 / +$49 / WR 50% (wins). Gamma release needs UNDERLYING DIRECTIONAL PRESSURE (trending unstable regime) to translate into tradeable move; in stable / quiet regime, the gamma effect is too subtle to capture. **Verdict: REJECT bare strategy; `NOT is_stable` filter (P1.1) is the unbuilt salvage candidate.**

## What we learned

1. **Gamma release effects need trending substrate**. The hypothesis assumed options expiry would produce intrinsic directional flow regardless of underlying regime. Reality: the effect compounds with existing directional pressure (unstable regime = trending = real flow direction to ride). In stable / range-bound regime, the gamma release happens but doesn't escape the range.

2. **DAY_OF_WEEK primitive validated**. 47 Friday-bar entries over 2024-05-20 to 2026-05-08 = consistent with weekly cadence. The calendar mechanism fires correctly; the strategy hypothesis doesn't.

3. **Stable-phase mis-conditioning is the recurring 2026-05-18 batch pattern**. Same direction as halving_cycle + busy_hour_btc + busy_hour_sol: loses stable / wins (or marginal) unstable. `NOT is_stable` filter inversion is the unbuilt P1.1 mutation candidate for all of these.

4. **Options expiry as a TradFi-overlapped mechanism class** — institutional flow + dealer rehedge is well-known to options market makers; retail-edge claim on the bare Friday-release effect is unrealistic per worldview prior #2.

## Hypothesis (Structured Format)

```
Hypothesis:    Friday 08 UTC post-expiry bar on BTC releases dealer
               rehedge flow + gamma decay positioning in bull-regime
               direction. Long entry at Friday 08 UTC captures the
               release momentum.

Mechanism:     Options gamma effects + dealer rehedging post-expiry.
               Calendar effect with weekly cadence.

Observable:    BTCUSDT 4h, every Friday 08 UTC bar over 2024-05-20 to
               2026-05-08. Long-only entries. SL 1.0 ATR / TP 1.5 ATR /
               time stop 4h.

Expected:      Sharpe 0.7-1.2 per pre-registered hypothesis.

Falsifier:     Sharpe < 0.5 → reject.
```

**Result**: N=47, Sharpe **+0.15**, WR 43%, Long-only, Net +$14, MDD 5.7%, PSR(>0) 56.4%, PSR(>1) 12.0%, Sharpe CI [-1.48, +1.55]. **Falsifier fires.**

**Per-phase split**:
- Stable: N=25 / -$35 / WR 36% — strategy loses in stable phase
- Unstable: N=22 / **+$49** / WR 50% — strategy wins in unstable phase

**TP/SL fire**: TP 6/47 (13% — low), SL 18/47 (38%), time_stop 23/47 (49%). Healthy TP rate would be 15-40%; 13% suggests the TP geometry is just at the edge of reachable but the firing pattern doesn't reliably hit it.

**Cause of failure (named)**: gamma release in stable / quiet regime doesn't carry. The effect is real (institutional gamma + dealer rehedge IS visible in microstructure) but is too SUBTLE to escape stable-range bounds. In unstable / trending regime, the gamma flow compounds with existing pressure and reaches the 1.5 ATR TP often enough to be tradeable.

## What WOULD make options expiry work (untested mutations) {#mutation-directions}

1. **`NOT is_stable` filter (P1.1)** — restrict entries to trending unstable-phase Fridays where unstable N=22 / +$49 / WR 50% lifts standalone Sharpe to ~0.6. UNBUILT.

2. **ETH variant** — Deribit also lists ETH options; ETH may have different microstructure response to expiry. Untested.

3. **Different time-of-day** — Wednesday and Tuesday have OI peaks before Friday expiry; pre-expiry entries might capture flow before noise diffusion.

4. **Different week of month** — month-end Fridays have larger OI than weekly Fridays; institutional flow concentrated.

5. **Different exit window** — current 4h time stop may be too short for gamma release; 12-24h horizons untested.

## What this rejects

- The bare `entry on Friday 08 UTC BTC with no regime filter` operational form.
- The naive "weekly expiry produces tradeable retail edge" prior on bare-mechanism evidence.

## What this DOESN'T reject

- The OPTIONS GAMMA mechanism class broadly. Other operational forms (intraday OI-anchored entries, dealer-rehedge-window timing, ETH variant, month-end-Friday concentration) remain valid future candidates.
- The DAY_OF_WEEK primitive (validated mechanically).
- The calendar-effects mechanism family (per [[../mechanisms/calendar-effects]]) — Friday expiry is one calendar effect; others (CME settlement windows, month-end rebalancing, quarterly options-vs-monthly options dynamics) untested at this operational rigor.

## Methodological notes

- **Pre-registered Sharpe expectation 0.7-1.2 was substantially above realized 0.15**. The gap quantifies the bare-mechanism realization-gap. Per LdP framework: expected_net_edge should come from INDEPENDENT data (theory + literature + cross-mechanism evidence); the pre-registered estimate here may have been over-optimistic via implicit data peek.

- **Stable-phase mis-conditioning across 4 strategies in 2026-05-18 batch** (busy_hour_btc, busy_hour_sol, halving_cycle, options_expiry) suggests a cross-strategy pattern worth flagging. The detector's `is_stable=True` regime is one structural type; mechanism hypotheses that don't explicitly condition on regime end up firing across both, with stable bars systematically losing. **Pattern is N=4 in this batch alone; codification candidate for the research arms backtest-decomposition arm: "when mechanism hypothesis doesn't explicitly condition on regime, MANDATORY per-phase split before verdict."**

## Related artifacts

- Strategy: the backtesting lab (moved 2026-05-19)
- Backtest: the backtesting lab
- 2026-05-18 batch decomposition: the research task queue Tier 4 section

## Related

- [[../mechanisms/calendar-effects]] — parent mechanism family page
- [[../mechanisms/options-gamma-perp-effects]] — options-gamma mechanism page
- [[../mechanisms/regime-gate-vs-bias-filter]] — stable-phase mis-conditioning
- [[halving-cycle-post-halving-btc-2026-05]] — sibling rejection (similar mis-conditioning pattern)
- [[busy-hour-flow-continuation-family-2026-05]] — companion rejection cluster (same mis-conditioning, validated compound mutation on SOL)

## Sources

- 2026-05-18 batch backtest (`runs/options_expiry_friday_btc_20260518_*/`)
- Per-strategy decomposition: the research task queue
