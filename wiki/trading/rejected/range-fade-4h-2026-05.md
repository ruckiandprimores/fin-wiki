---
title: "range_fade_4h — regime absent, mechanism untested (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, mean-reversion, bollinger-fade, compressed-vol-regime, regime-conditional, mechanism-untested-in-window, partial-truth-mechanism-class-distinct, day-swing]
---

# range_fade_4h — regime absent, mechanism untested (2026-05)

> **Lead learning**: Mean-reversion at Bollinger envelope in compressed-vol regime — the mechanism literature is intact (Connors / Bollinger / GARCH); the strategy fails in this window because **the regime the mechanism targets was absent in 2025-26**. The strategy spec's own historical analog correctly flagged "limited compressed-vol periods in 2025-26 (mostly trending bear regime)"; the test confirmed the spec's prediction. This is **mechanism untested**, not mechanism falsified — the regime didn't show up. **Useful partial truth**: range_fade × `bollinger_squeeze_4h_short_only` correlation = −0.023 confirms range-fade-in-compression and squeeze-breakout-in-expansion are **genuinely different mechanism classes** at the timing level. Verdict: REJECT (regime-conditional within this window) with mutation deferred to compressed-vol-rich window (2023-mid).

## What we learned

1. **Mechanism rejection within a regime ≠ mechanism rejection.** The 2025-26 window was bearish-trending-dominant with limited compressed-vol periods. Range-fade is a compressed-vol mechanism; the regime didn't show up. The strategy's failure here is **regime-absence**, not mechanism-absence. The wiki's [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC regime taxonomy]] would identify 2023-mid ($25-31K range for ~6 months) as a compressed-vol-rich window where this mechanism could be properly tested.

2. **Both directions negative across the grid is not asymmetric-mechanism evidence.** %both- = 66.7%; long ATP −$1.66; short ATP −$2.32. Unlike `bollinger_squeeze_4h_short_only` or `funding_flip_recovery_4h_short_only` where directional asymmetry was load-bearing, range_fade's negative result holds in both directions — no salvageable direction-restriction mutation. This is consistent with mechanism-absence (random walk in non-compressed regime) rather than direction-asymmetric edge in regime.

3. **Train→val correlation +0.296 is "consistently bad", not signal transfer.** A high train→val correlation can mean (a) signal transfers across windows or (b) noise pattern is consistent across both partitions. For range_fade, %val>0 = 21.9% — well below 50% — meaning the val partition is *uniformly negative*. The +0.296 reflects "uniformly negative across both partitions", not signal stability. **Reading train→val correlation requires checking the absolute level, not just the correlation magnitude.**

4. **Partial-truth: mechanism-class distinction confirmed.** range_fade × bb_short_only = −0.023. Range-fade-in-compression and squeeze-breakout-in-expansion are genuinely different mechanism classes — they don't fire in the same bars. Useful for future *composite* portfolio designs when compressed-vol regime returns and *both* mechanism classes can contribute.

## Hypothesis (Structured Format)

```
Hypothesis:    Mean-reversion at Bollinger envelope is profitable in
               compressed-vol regime (low realized vol; tight BB band
               width). When price tags BB extreme in compressed-vol,
               revert to BB middle.

Mechanism:     Connors/Bollinger framework: in low-vol regime, BB
               extremes mark statistically improbable bar moves;
               mean-reversion to mid-band has positive expectancy.
               GARCH grounding: vol clustering means low-vol periods
               persist; the mean-reversion bet is implicitly a vol-
               regime continuation bet.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp 4h,
               $1000 capital, fee 0.04%. Pre-registered:
                 (a) Strategy-level: net ≥ $120/yr deployment-grade
                 (b) Hypothesis-level (paired): correlation with
                     `bollinger_squeeze_4h_short_only` < +0.4 confirming
                     different mechanism class

Expected:      medSh +0.30 to +0.60 in compressed-vol-rich window;
               signal scarce in 2025-26 trending regime per spec's
               own historical analog (load-bearing acknowledgment).
               expected_net_edge:
                 source: Connors framework + GARCH literature
                         + spec's own analog flagging regime
                         scarcity
                 value: +$30/yr 2025-26 (regime-discounted);
                        +$80-150/yr 2023-mid window (regime-rich)
                 independence: mechanism literature independent;
                              effect-size estimate test-window-
                              derived → research-grade-only
                              verdict default

Falsifier:     Strategy-level: net < $120/yr → deployment fail (expected)
               Hypothesis-level: correlation with bb_short_only > +0.4
               would falsify the mechanism-class-distinct claim.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 4h, $1000, fee 0.04%)

| Metric | Value | Falsifier | Fired? |
|---|---:|---|:---:|
| Median Sharpe | **−1.79** (worst in batch) | < 0 | **YES (deployment-grade)** |
| Long ATP | **−$1.66** | both directions negative | confirms regime-absence not asymmetric-mechanism |
| Short ATP | **−$2.32** | both directions negative | same |
| %both- | **66.7%** | — | confirms uniform negative |
| %val>0 | 21.9% | — | uniformly bad val partition |
| corr(train, val) | +0.296 | — | "consistently bad", NOT signal transfer |
| trades/yr | 40.2 | — | low but not zero |
| gross/yr | −$81 | — | — |
| friction (1bp×$5K, $1/trade) | −$40 | — | — |
| **net/yr** | **−$122** | ≥ $120 | **FAIL** |

| Hypothesis-level paired falsifier | Predicted | Observed | Verdict |
|---|---|---:|:---:|
| corr(range_fade, bb_short_only) | < +0.4 (different mechanism class) | **−0.023** | **PASS** |

## Falsifier verdict: Strategy-level FIRED; hypothesis-level PASSED

- **Strategy-level (deployment-grade gate)**: net −$122/yr → **FIRED**
- **Hypothesis-level (mechanism-class distinction vs bb_short_only)**: correlation = −0.023 → **PASSED** — confirms range-fade-in-compression and squeeze-breakout-in-expansion are genuinely different mechanism classes

This is an unusual falsifier pattern: strategy fails deployment, but the *hypothesis* (mechanism-class distinction) is *confirmed*. The strategy doesn't work in this window for regime reasons; the mechanism-class claim survives independently.

## What partial truth remains?

| Component | Status | Evidence | Salvage condition |
|-----------|--------|----------|-------------------|
| **Range-fade mechanism in 2025-26 BTC 4h** | ❌ Rejected (regime-absent) | medSh −1.79; both dirs negative | Re-test in compressed-vol-rich window (2023-mid) |
| **Mechanism-class distinction from squeeze breakout** | ✅ Confirmed | range_fade × bb_short_only = −0.023 | Useful for composite portfolio designs when both regimes contribute |
| **Connors / Bollinger / GARCH literature grounding** | ✅ Intact | Mechanism is well-documented | Window-selection discipline; regime classifier (W3) would gate this strategy |
| **Composite portfolio with regime-aware mechanism switching** | Untested | Different mechanism classes confirmed at timing level | Build regime classifier; route to range_fade in compression, bb_short_only in expansion |

The strongest **future-tense** result is the composite-portfolio path: when a regime classifier exists, range_fade and bb_short_only are genuinely complementary — they cover different regimes with low timing overlap.

## Reason classification

- [x] **Falsifier fired** — strategy-level deployment-grade gate
- [x] **Regime-absent in test window** — mechanism's target regime (compressed-vol) didn't show up
- [x] **Mechanism literature intact** — Connors / Bollinger / GARCH; the rejection is window-specific
- [x] **Partial truth: mechanism-class distinction confirmed** at timing level (different scaffold per [[trading/mechanisms/scaffold-as-mechanism]])
- [ ] No falsifier articulated (clean pre-registration)
- [ ] Already arbitraged (regime-absence is the cleaner explanation)
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥18 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far. range_fade joins inventory with falsifier-fired result; trial count recorded.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05/2026-04 only. The mechanism's target regime (compressed-vol) is window-specific; testing it in a trending-dominant window confounds regime-presence with mechanism-validity.
- **Multi-window backtesting required to distinguish regime-absence from mechanism-failure.** Pipeline limitation; [[foundation/methodology/strategy-validation-discipline|§13]] flags non-stationarity.
- **OOS test on 2023-mid deferred** — would resolve regime-vs-mechanism question. Owner: data ingest + W3 regime classifier deployment.
- **PSR/DSR not pipeline-implemented** — significance not certified.
- **The +0.296 train→val correlation is a discipline lesson**: high train→val correlation alone is *not* signal-transfer evidence; check whether absolute level is positive on the val partition. range_fade's correlation reflects "uniformly negative both partitions", not "signal stable across windows".

## Key Takeaways

- **Mechanism rejection within a regime ≠ mechanism rejection globally.** range_fade is rejected in 2025-26 BTC 4h because the compressed-vol regime didn't show up. 2023-mid retest deferred.
- **Both directions negative is regime-absence, not asymmetric-mechanism.** No salvageable direction restriction.
- **Mechanism-class distinction from squeeze breakout confirmed** (corr = −0.023). Composite portfolio path remains open when regime classifier exists.
- **Train→val correlation interpretation discipline**: +0.296 with uniformly-negative val partition = "consistently bad", not "signal transfers". Check the absolute level.
- **The wiki's existing range-fade mechanism context is intact** — Connors / Bollinger / GARCH literature unmoved; the rejection is window-specific.

## Related

- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — bb_short_only (the paired-falsifier comparator; mechanism-class distinction confirmed)
- [[trading/mechanisms/scaffold-as-mechanism]] — companion framework; this rejection's mechanism-class distinction confirms cross-scaffold independence
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — embedded bias-filter discipline (range_fade had no bias filter; future composite would need scaffold-routing)
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime taxonomy; 2023-mid identified as compressed-vol-rich for retest
- [[foundation/macro/regime-conditional-edge-2025-26]] — W2 cohort regime context (trend cluster's edge concentrated 2025-Q4 / 2026-Q1; range_fade's target regime absent same window)
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law; train→val interpretation discipline

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"`range_fade_4h`"
- the backtesting lab — strategy spec + pre-registration (with regime-scarcity acknowledgment)
- Connors / Bollinger volatility-mean-reversion literature
- GARCH (Bollerslev 1986) — vol-clustering grounding
