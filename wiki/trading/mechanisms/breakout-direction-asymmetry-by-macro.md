---
title: "BTC 4h breakout-direction asymmetry by macro regime"
status: growing
created: 2026-05-21
updated: 2026-05-21
tags: [trading, mechanisms, breakouts, regime-conditional, btc, asymmetry, day-swing-applicable, rr-by-family, range-exit]
---

# BTC 4h breakout-direction asymmetry by macro regime

> **TL;DR**: On BTC 4h, range breakouts (price moving out of an anchored stable range) **extract tradeable edge in BULL-dominant macro at R:R ~1:2** (mean-reversion to next range) but **FAIL in BEAR-dominant macro at any R:R**. Symmetric long/short breakout strategies should EXPECT asymmetric per-trade edge favoring the long side in bull regimes and producing drain in bear regimes. N=2 bull-window evidence on the R:R 1:2 finding (regime_v2 phase_change_target sweep + regime_v3 break_entry_target sweep both converge at val-best=2.0); N=2 regime-conditionality confirmation via Phase 4 bear-window validation (same Run 5649 config: +$149 bull breakout PnL → -$109 bear). The "BTC 4h breakouts are mean-reversion at 1:2" framing was real but **bull-window-specific** — the unconditional claim over-generalized from single-regime data.

## Empirical evidence (N=2 bull-window + N=2 bear-window) {#empirical-evidence}

### Source 1 — regime_v3 sweep 2026-05-21 bull window

- 6,561-config grid sweep on BTC 4h bull window (2024-05 → 2026-05)
- `break_entry_target_mult` swept across `{2.0, 3.0, 5.0}`
- **Val-best: 2.0** (mean val PnL **+$206** vs **+$79** at 5.0)
- Top configs by Sharpe all used `break_entry_target=2.0`

### Source 2 — regime_v2 sweep 2026-05-21 (independent N)

- Same bull window; `phase_change_target_mult` swept
- **Val-best: 2.0** — convergent with regime_v3 on R:R 1:2 for breakout-class triggers
- Two independent sweep angles, same val-optimum

### Source 3 — regime_v3 Phase 4 bear-window 2026-05-21

Run 5649 (Sharpe-optimal config from regime_v3 sweep; params include `break_entry_target_mult=2.0`) tested on `data/btcusdt_bear/futures` (2022-04 → 2023-09):

| Sub | Bull window | Bear window |
|---|---|---|
| `breakup_entry_long` | +$93 (101 trades, 36% WR) | **-$18** (67 trades, 33% WR) |
| `breakdown_entry_short` | +$56 (92 trades, 34% WR) | **-$91** (95 trades, 24% WR) |

**Combined breakout PnL**: bull **+$149**, bear **-$109**. The "R:R 1:2 mean-reversion" finding was real BUT **bull-window-specific**. In bear window, breakouts FAIL at any R:R.

## Hypothesis (Structured Format)

```
Hypothesis:    On BTC 4h, range breakouts (price moving out of an
               anchored stable range, where "anchored" means the
               team's bar-level validated stability — see
               team-stable-range-detector-methodology) extract
               tradeable edge at R:R 1:2 in BULL-dominant macro
               regimes and produce net drain at any R:R in
               BEAR-dominant macro regimes. The asymmetric edge
               favors the long side in bull windows.

Mechanism:     BULL-dominant macro — BTC's 4h intra-day range
               structure steps UPWARD; successive stable ranges
               sit at progressively higher price levels.
               Breakouts UP are absorbed quickly by the next
               higher range's liquidity (~12-24h). Wide targets
               (R:R 1:3+) miss the absorption point; tight 1:2
               targets capture the move.

               BEAR-dominant macro — BTC tends to FIND support at
               prior range levels (institutional bids, treasury-
               vehicle accumulation, ETF flow absorbing supply).
               Breakup_entry fails because the breakout-up signal
               is rare and weak in bear; breakdown_entry fails
               because the breakdown-down move is interrupted by
               those structural bids. The "merged-bear artifact"
               pattern (analogous to merged-bull in bull window)
               doesn't mirror — bear consolidations are choppier,
               less clean. The mechanism's geometric assumption
               (clean break + follow-through to next range) is
               violated in both directions.

Observable:    BTCUSDT 4h, post-2026-05-21 refined data layer.
               Breakout entries fire when bar-level state is
               `team_is_stable_breaking_up` (for long entries) or
               `team_is_stable_breaking_down` (for short entries).
               Stop = 1×ATR; Target = 2×ATR.

Expected:      Bull window: +$50-150 net per ~100-trade window;
               Sharpe 0.4-0.8; WR 33-38% (mean-reversion-like).
               Bear window: -$50 to -$100 net per ~100-trade
               window; Sharpe negative; WR 24-33%.

Falsifier:     If bull-window breakout subs produce net <-$0 OR
               bear-window breakout subs produce net >+$50, the
               regime-conditionality framing is wrong. Run 5649
               currently sits cleanly inside both gates
               (+$149 bull, -$109 bear).
```

## Mechanism — why R:R 1:2 wins in bull, fails in bear {#mechanism}

### Why R:R 1:2 wins in bull-dominant macro

BTC's 4h intra-day range structure in bull-dominant macro:

- Successive stable ranges step **UPWARD** in price level
- Breakouts UP are **absorbed quickly** by the next higher range's liquidity (typical absorption window ~12-24h)
- Wide targets (R:R 1:3+) **miss** the absorption point — the trade closes at trailing stop or time-stop near the new range without ever reaching wide target
- Tight 1:2 targets **capture** the move — TP fires inside the absorption window before drift back to entry

### Why breakouts fail in bear-dominant macro

Bear-window evidence (Phase 4 Run 5649):

- `breakup_entry_long` fails — only 33% WR; the breakout-up signal is rare and weak in bear macro (price tends to retrace immediately into the prior range)
- `breakdown_entry_short` fails harder — 24% WR; BTC's tendency to **find support at prior range levels** interrupts the downside follow-through

Three structural factors anchor the bear-window failure:

1. **Institutional bids** at prior range lows — ETF authorized-participant + market-maker layer prices BTC against a multi-week absorption schedule that doesn't honor the 4h breakout's geometric assumption
2. **Treasury-vehicle accumulation** when price approaches structural levels (per [[../../foundation/macro/treasury-vehicle-cascade-watch]] N=2+ codification)
3. **ETF flow absorbing supply** — even in bear regimes, weekly flow has been net-positive on average (April 2026 strongest month per substrate); the supply-clearing flow violates the breakout-class mechanism's continuation assumption

### Companion mechanism: [[failed-breakout-reversal]]

The `failed-breakout-reversal` mechanism documents the **inverse pattern**: breakouts that fail and reverse. In bear-dominant macro, BTC 4h breakouts are predominantly failed-breakouts — this page provides the regime context for when to expect that. The two mechanism pages are reciprocal: bull-macro → use breakout; bear-macro → use failed-breakout-reversal where applicable, or skip the family entirely.

## When to use the mechanism

Use `breakup_entry` / `breakdown_entry` strategies on BTC 4h when:

- **Macro is BULL-DOMINANT** (regimes 11-14 per [[../../foundation/macro/btc-regime-taxonomy-2020-2025|BTC regime taxonomy]])
- **Stable phase has confirmed end** (per [[../../foundation/methodology/team-stable-range-detector-methodology|team detector]])
- **Price has cleanly crossed `range_high`** (for breakup) or **`range_low`** (for breakdown) — i.e., bar-level state is `stable_breaking_up` / `stable_breaking_down` per [[../../foundation/methodology/bar-level-validation-overlay]]

DO NOT rely on the mechanism in:

- **BEAR-DOMINANT macro** (regimes 6-7, possibly current Regime 15 if post-peak Distress)
- **Mixed / transition regimes** — backtest evidence too thin
- **Pre-bar-level-validation data layers** — without the refined parquet, breakout entries fire in the wrong bar-state population per [[../../foundation/methodology/bar-level-validation-overlay#worked-example-team-detector]]

## Implications for strategy design {#strategy-design}

For symmetric breakout strategies on BTC 4h:

- **Size breakout subs CONDITIONALLY on macro direction** — full size in bull-dominant; quarter-size or zero in bear-dominant
- **Or restrict breakout subs to operate only when team's daily/weekly classification is bull** — same effect, expressed in DSL
- **Don't expect symmetric edge** — the long-side has structural advantage in bull windows; symmetric framing is a backtest-window-luck artifact when the window happens to be bull-dominant

For **mixed-regime backtest windows** (typical 2y windows spanning 2024-05 → 2026-05 are bull-dominant; the 2022-23 bear is rare in production windows):

- Expect breakout sub PnL to be **net-positive overall** but with **significant bear-window drain**
- Per-window decomposition matters more than aggregate metrics for these subs (per [[../../foundation/methodology/path-quality-diagnostics|path-quality-diagnostics]])

## Honest caveats {#caveats}

- **N=2 bull-window R:R evidence + N=2 bear-window regime-conditionality** is moderate — independent sweep angles (phase_change_target + break_entry_target) converged on R:R 2.0, and Phase 4 confirmed bear failure. But all evidence is from one 2y bull window + one bear window. **Single-window-per-regime** is the load-bearing limitation.
- **The initial 2026-05-21 morning finding** (bull-only data) over-generalized to an unconditional "BTC 4h breakouts are 1:2 mean-reversion" claim. Phase 4 bear-window check corrected this within the same day. The discipline lesson: **regime-conditional claims need cross-regime test before being treated as structural**.
- **ETH and SOL extensions are open questions** — the mechanism is BTC 4h-specific. ETH per-symbol structure is **fakeout-fade** (per [[failed-breakout-reversal]] N=3 BTC + N=1 ETH cross-symbol asymmetry findings); SOL is **long-bias-on-both-directions** (per [[../../foundation/asset-classes/sol-market-structure]]). The bull-regime R:R 1:2 finding may transfer at the R:R level but with different per-side asymmetry. Untested.
- **Bear-window data is 2022-04 → 2023-09**, which has its own regime structure (post-Terra collapse + FTX + SVB). A different bear window (e.g., 2018) might have different breakdown-direction asymmetry. The bear-window failure is N=1 at the window level.
- **The mechanism assumes the refined data layer** (per [[../../foundation/methodology/team-stable-range-detector-methodology]]). Pre-refinement (raw `is_stable`) breakout entries would fire in `stable_anchored` bars (where the period claim holds) — a different and untested population.

## Cross-symbol open questions

This is BTC 4h-specific. ETH / SOL extension would test:

1. **Does the bull-regime R:R 1:2 hold cross-symbol?** Similar market structure suggests yes, but ETH's smaller basis-trade infrastructure + SOL's nascent CME futures may produce different absorption dynamics.
2. **Does the bear-regime failure hold cross-symbol?** ETH and SOL have different drawdown depths in 2022-23 bear; the institutional-bid + treasury-vehicle factors that anchor BTC's bear failure may not have ETH/SOL analogs.
3. **Does the breakup/breakdown asymmetry pattern hold cross-symbol?** SOL's long-bias-on-both-directions per substrate suggests SOL bear-window breakdown_entry_short may fail even harder than BTC's.

## Comparison with universal stop-target priors

The [[universal-stop-target-prior]] page's family-conditional R:R signatures (2026-05-06) are calibrated against bull-window-dominant data. The R:R 1:2 finding here:

- **Sits in the "range fade at boundary" family default** (R:R 1:1.5 stop/target) but at slightly wider target — closer to mean-reversion-at-mid-range than range-fade-at-boundary
- **Does NOT fit the trend continuation family default** (R:R 1:3 with stop 1.0-2.5×ATR) — the failing wide-target (R:R 1:5) evidence rules out trend-continuation framing for this trigger
- **Does NOT fit the right-tail-of-mean-reversion family** (R:R 9:1-14:1 target ≥7×ATR) — those mechanisms capture regime-state-change moves; this mechanism captures local range-step-up moves

See [[universal-stop-target-prior#btc-4h-breakout-class-exception]] for the regime-conditional exception subsection clarifying that the universal trend-continuation 1:3 R:R prior does NOT apply to BTC 4h breakouts, and that the alternative R:R 1:2 finding is itself regime-conditional.

## Related

- [[universal-stop-target-prior]] — base R:R prior framework; this page documents a regime-conditional exception
- [[../../foundation/methodology/team-stable-range-detector-methodology]] — the data layer this builds on; bar-level state gates the trigger
- [[../../foundation/methodology/bar-level-validation-overlay]] — generalized validation pattern; this mechanism uses `team_is_stable_breaking_up` / `breaking_down` per the overlay
- [[../../foundation/macro/btc-regime-taxonomy-2020-2025]] — regime context for bull-vs-bear classification at the macro layer
- [[failed-breakout-reversal]] — inverse mechanism (failed breakouts that reverse); applies in bear-regimes where this page's mechanism fails
- [[regime-gate-vs-bias-filter]] — regime-gating-as-implementation pattern; this page's strategy-design guidance is a specific case
- [[../../foundation/asset-classes/btc-market-structure]] — continuation-character substrate; breakout mechanisms ARE the continuation triggers documented there

## Sources

- regime_v2 grid sweep (bull window): the backtesting lab
- regime_v3 grid sweep (bull window): the backtesting lab
- regime_v3 Phase 4 bear runs: the backtesting lab, `runs/regime_v3_bear_run1649/`
- Underlying analysis: the research task queue
- Mechanism context: [[../../foundation/asset-classes/btc-market-structure]] continuation-character codification (N=3 bull-window evidence + bear-regime pending)
