---
title: "turtle_soup_4h — rejected on first test (2026-05); short-asymmetry partial truth"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, failed-breakout-reversal, raschke, schwager, falsifier-fired, short-asymmetry, behavioral-positioning-unwind, day-swing]
---

# turtle_soup_4h — rejected on first test (2026-05); short-asymmetry partial truth

> **TL;DR**: First real test of the wiki's [[trading/mechanisms/failed-breakout-reversal|failed-breakout-reversal]] structured hypothesis after Apr-window bug fix. Strategy-level falsifier fired on **3 of 3 criteria** (medSh −0.77, medWR 33.3%, medATP −$0.70). Mechanism does not show in 2025-26 BTC perp window with default Raschke-style parameterization. **Partial truth retained on short side**: long sleeve med −$74.50, short sleeve med +$19.70 — asymmetry consistent with the 2025-26 bearish-dominant regime, not a permanent mechanism feature. Could be revisited as short-only / bearish-regime mutation when bearish regime is *demonstrably* present in a future window.

## Hypothesis (Structured Format)

```
Hypothesis:    Failed-breakout-reversal — trade reclaims of recently-broken
               N-bar extremes; trapped breakout cohort's forced cover IS
               the move (Raschke "Turtle Soup", Schwager NMW 1992).

Mechanism:     Channel breakouts attract stop-buy + stop-cover flows.
               When breakout fails to extend within K bars, those flows
               were *all* the buying — sellers absorb; reverse trade
               catches the forced-cover unwind. Crypto carrier:
               liquidation maps make stop clusters visible in real time.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp,
               $1000 capital, fee 0.04%, 3072 parameter combinations.
               Pre-registered strategy-level falsifier:
                 (a) Median Sharpe ≥ 0
                 (b) Median win rate ≥ 45%
                 (c) Median ATP > random baseline (~0)

Expected:      Per pre-registration: medSh +0.30 to +0.70,
               medWR 50-65%, medATP ≥ $0.20 in window with confluence.

Falsifier:     If ANY of medSh < 0, medWR < 45%, medATP ≤ 0,
               mechanism does not transfer to current BTC perp 4h window
               under default parameterization.
```

Pre-registered in the backtesting lab § Falsifier.

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | **−0.77** | < 0 | **YES** |
| Median win rate | **33.3%** | < 45% | **YES** |
| Median ATP | **−$0.70** | ≤ random (≈0 baseline) | **YES** |
| Median PF | 0.88 | < 1 | YES (decoration) |
| Bug-fix verified | opportunities now firing across full grid (was 0/3072 pre-fix) | — | — |

## Falsifier verdict: FIRED on 3 of 3 criteria

Hypothesis-level paired falsifier (vs no-recent-breakout-required mirror) was **not built** per Apr task notes — the strategy went into testing without its full instrumentation. Strategy-level falsifier fired regardless on three independent metrics, which is sufficient for rejection without requiring the paired-mirror evidence.

## Partial truth retained — short-side asymmetry

| Direction | Median P&L |
|-----------|---:|
| Long (failed breakdown → reclaim → buy) | **−$74.50** |
| Short (failed breakout → reclaim → sell) | **+$19.70** |

Short-side asymmetry consistent with cross-strategy pattern across the 2025-26 batch. **A short-only OR bearish-regime-conditional mutation could be revisited if/when this window's bearish dominance is demonstrably present in a future window.** Asymmetry alone isn't the mechanism; it's the regime feature.

This is the same pattern documented in [[trading/backtest-results/direction-restriction-short-only-2026-05|direction-restriction controls]]:
- bollinger_squeeze short_only: +1.02 Sharpe vs symmetric parent's −1.01
- funding_flip_recovery short_only: +0.50 Sharpe vs symmetric parent's −0.11
- turtle_soup short asymmetry: +$19.70 vs −$74.50 (untested as short_only mutation; partial-truth retained)

## Cross-strategy pattern context

Long-fails / short-positive asymmetry repeats across `bollinger_squeeze` (parent), `funding_flip_recovery` (parent), and `turtle_soup` in this window. Strategies with **embedded EMA-bias filter** (`ema_bias_breakout_4h_v2`) had the **inverse** pattern (long positive in this same window). Combined finding: directional asymmetry is a **regime feature**, not a mechanism feature — the mechanism here just inherits the regime's bias.

This is what [[trading/mechanisms/regime-gate-vs-bias-filter|the new design-pattern page]] articulates with N=4 supporting cases.

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥13 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far. Trial count recorded for downstream DSR adjustment if/when [[glossary/deflated-sharpe-ratio|DSR]] is pipeline-implemented. Falsifier-fired-with-3-of-3 result is robust to multiplicity correction (negative across all three metrics, not borderline).

## Reason classification

- [x] **Falsifier fired** — pre-registered 3/3 criteria all fired on the negative side
- [x] **Mechanism not present in current crypto microstructure under default parameterization** — bug-fix verified; opportunities fire; outcomes negative
- [ ] No falsifier articulated (pre-registration was clean)
- [ ] Already arbitraged (cannot distinguish from "regime-absent")
- [x] **Partial truth: short-side asymmetry** — regime feature, not mechanism feature
- [ ] Graveyard duplicate

## What partial truth remains?

| Component | Status | Evidence | Salvage condition |
|-----------|--------|----------|-------------------|
| **Failed-breakout-reversal mechanism (full bidirectional)** | ❌ Rejected in 2025-26 BTC 4h window | medSh −0.77; 3/3 falsifier criteria | Re-test in window with confirmed *non-bearish-dominant* regime; default parameterization |
| **Short-only / bearish-regime-conditional mutation** | Plausible / partial truth retained | Short sleeve +$19.70 median | Build short_only mutation; pre-register Sharpe gap vs symmetric ≥+0.30; only run when window is *demonstrably* bearish-dominant |
| **Confluence-required version (Coinglass + funding + on-chain)** | Untested | The default test was bare-mechanism per the wiki page §142-156 specifies confluence | Build confluence-required mutation; require ≥3 of liquidation cluster / volume / whale flow / funding flip / RS divergence |
| **Wyckoff-upthrust phase-context version** | Untested | The wiki page distinguishes operational-trigger (Raschke) from phase-context (Wyckoff) | Build phase-context-conditioned mutation; only fire when end-of-distribution criteria met |

The bare-mechanism rejection is the *cleanest* result (the wiki page already calls out that the bare mechanism is *self-defeating risk* per §184; this is consistent with that disclosure). The salvage paths require additional instrumentation; not pursued in this rejection but documented for future revisit.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Hypothesis-level paired falsifier missing** — the no-recent-breakout-required mirror was specified in pre-registration but not built. Strategy-level falsifier fired anyway and is sufficient for this rejection, but the full instrumentation discipline was not honored. Owner: tooling work (pipeline ergonomics for paired-falsifier construction).
- **Single-window backtest** — 2025-05 → 2026-04 only. Out-of-window validation absent. Per [[foundation/methodology/strategy-validation-discipline]] §13 crypto-specific concerns: regime non-stationarity. The 2025-26 bearish dominance is the load-bearing context; salvage path requires window selection discipline.
- **Default parameterization tested** — the wiki page §141-156 specifies confluence requirements and macro filter; the default backtest used neither. The test was of *bare-mechanism applicability*, not of the wiki page's full operational specification. The rejection is therefore against the bare mechanism, not against the spec'd version.
- **PSR/DSR not pipeline-implemented** — confidence intervals not computed. Direction of all three metrics is robust to multiplicity correction; magnitude is not certified.
- **Walk-forward / multi-window absent** — pipeline limitation.

## Key Takeaways

- **Bare failed-breakout-reversal mechanism does not transfer to 2025-26 BTC perp 4h under default parameterization.** Falsifier fired 3/3.
- **Short-side asymmetry is real but is a regime feature.** Long sleeve −$74.50 vs short sleeve +$19.70 is consistent with the 2025-26 bearish dominance, not the mechanism's behavior in regime-neutral conditions.
- **Bug fix worked** — the prior 0/3072 opportunities issue is resolved; the mechanism *can* fire across the grid. The rejection is on outcome, not on instrumentation.
- **The wiki's failed-breakout-reversal page is updated** from "open hypothesis" (seedling) to "first test rejected; spec'd-version untested" — see [[trading/mechanisms/failed-breakout-reversal]]. Future testing should follow the wiki's full operational spec (confluence + macro filter), not the bare default.
- **Salvage paths exist but require additional instrumentation** (short_only mutation, confluence-required version, phase-context-conditioned version) — none built in this batch; documented as future work.

## Related

- [[trading/mechanisms/failed-breakout-reversal]] — wiki mechanism page; status updated to reflect this test
- [[foundation/sources/schwager-market-wizards]] — Raschke chapter (mechanism source)
- [[trading/mechanisms/behavioral-positioning-unwind]] — sibling family
- [[trading/mechanisms/accumulation-distribution]] — Wyckoff upthrust as phase-context (untested salvage path)
- [[foundation/market-structure/zero-sum-game]] — every winning trade requires an identifiable loser; the failed-breakout cohort is named
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — companion entry; the short-only mutation pattern these would be subject to
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — the design-pattern page synthesizing the regime-asymmetry observations
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law + falsifier discipline
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — companion 2026-05-05 rejection (different mechanism class, same window)

## Sources

- the backtesting lab — strategy files + pre-registration
- the backtesting lab — strategy README with falsifier
