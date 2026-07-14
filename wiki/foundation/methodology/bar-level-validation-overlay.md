---
title: "Bar-level validation overlay — methodology for period-level signal consumers"
status: growing
created: 2026-05-21
updated: 2026-05-21
tags: [foundation, methodology, data-validation, regime, strategy-discipline, period-level-signals, design-pattern, day-swing]
---

# Bar-level validation overlay

> **TL;DR**: When a strategy gates on a **period-level signal** (regime cell, vol regime, phase classification, range membership), the label can be massively wrong at the **bar level** inside the period — often >50% of bars don't match the period's stated geometry. Before trusting a period-level label, validate that THIS BAR matches what the period claims. The pattern is generic: any retrospectively-computed period-level signal can have label-time vs reality drift at the bar level. Canonical worked example: the team's stable-range detector ([[team-stable-range-detector-methodology]]) where only 25-37% of bars inside "stable" periods are actually in `[range_low, range_high]`. The bar-level overlay rescued `regime_v3` from `regime_v1`'s catastrophic falsification.

## The pattern

Period-level signals are computed retrospectively over a window. They describe what the window WAS as a whole. But at the bar level inside the window, individual bars may have drifted away from the window's stated geometry.

**Three common failure modes:**

1. **Merged-window artifact.** Multiple sub-windows are joined into one (e.g., the team stable-range detector's `merge_gap_days=1` merges adjacent stable sub-ranges that may have very different price levels). The merged window reports the FIRST sub-window's geometry; later bars are at different price levels but inherit the merged label.

2. **Label-time drift.** Regime labels computed at window-start may not reflect bar-time conditions weeks later within the window. The label was true at label-time; the world moved on.

3. **Threshold proximity.** A regime classification that just barely crossed a threshold at window-start may have drifted back across by mid-window without re-classification.

All three produce the same downstream symptom: **the gating signal claims X for the whole period; some bars inside the period don't actually satisfy X.**

## The remedy — bar-level validation overlay

For each bar inside a period-level signal, apply a per-bar check:

- Does THIS BAR's price / volume / feature match what the period's geometry claims?
- If yes → bar inherits the period label
- If no → bar gets a `breaking_*` or `drifted_*` sub-label

The strategy then gates on the **bar-level label**, not the raw period label.

```
for bar in period[start:end]:
    if bar.matches(period.geometry):
        bar.label = f"{period.label}_anchored"   # truly matches
    else:
        bar.label = f"{period.label}_drifted"    # period claim; bar disagrees
```

Strategies that need TRUE adherence to the period claim gate on `*_anchored`. Strategies that want to capture the drift as a directional signal can gate on `*_drifted` (or directionally-specific sub-flavors).

## Worked example — team stable-range detector {#worked-example-team-detector}

The team's stable-range detector ([[team-stable-range-detector-methodology|full methodology page]]) is the canonical case:

- **Period-level label**: `is_stable` flag on 3,177 4h bars in BTC bull window 2024-05 → 2026-05
- **Bar-level check**: `close ∈ [range_low, range_high]` for this bar?

| Bar-level state | Count | Share |
|---|---:|---:|
| `stable_anchored` (PASS) | **808** | **25.4%** |
| `stable_breaking_up` (close > range_high) | 1,062 | 33.4% |
| `stable_breaking_down` (close < range_low) | 1,307 | 41.1% |

**74.6% of "stable"-labeled bars fail bar-level validation.** Cross-regime: bear window 2022-04 → 2023-09 shows 37.1% anchored / 62.9% breaking — pattern holds.

### Strategy-level consequences

**Without bar-level validation** — `regime_v1` (fade strategies gated on raw `is_stable`):
- Result: **-$472 net**, catastrophic falling-knife trades
- Mechanism: fade fires when price near `range_low`; in merged-bear "stable" periods, price was near `range_low` because it was **mid-crash**, not because it was bouncing within a range

**With bar-level validation** — `regime_v3` (fade strategies gated on `team_is_stable_anchored`):
- Result: **+$451 bull / +$57 bear**, cross-regime positive
- Mechanism: fade fires only on bars where close is actually in the reported range — the mean-reversion mechanism is applicable

Same period-level label, opposite outcomes. **Bar-level validation IS the load-bearing fix.**

## When bar-level validation is necessary

Period-level labels need bar-level validation when:

- **The period detector uses MERGING / SMOOTHING** that hides intra-period drift (canonical case: time-based or hysteresis-based adjacency joining)
- **The period's claim is about CURRENT BAR STATE** (stable, in-range, low-vol) rather than HISTORICAL FACT ("this region was once high-vol")
- **The strategy's gating ASSUMES the period claim holds for each bar** (most fade / mean-reversion / range-bound strategies)

Period-level labels DO NOT need bar-level validation when:

- The label describes a **structural fact** independent of current bar (e.g., "this is a Wyckoff distribution zone" — the label is meta-historical and refers to the formation, not the current bar)
- The strategy uses the period label as **ENTRY TIMING ONLY** — fires once at period start, exits before bar drift matters
- The period detector evaluates **per-bar with no smoothing** — the label IS already bar-level (no overlay needed)

## The validation overlay pattern (generic) {#generic-pattern}

For any period-level signal `P` claiming a property `prop` for bars in `[start, end]`:

```
for bar in period[start:end]:
    if bar.matches(prop):
        bar.label = f"{P}_anchored"
    else:
        bar.label = f"{P}_drifted"
```

Sub-flavors can be added when the drift direction is itself informative:

```
if bar.matches(prop):                  bar.label = f"{P}_anchored"
elif bar.exceeds(prop.upper_bound):    bar.label = f"{P}_breaking_up"
elif bar.below(prop.lower_bound):      bar.label = f"{P}_breaking_down"
```

Strategies then choose which bar-level state to gate on based on the mechanism class:

- **Mean-reversion / fade strategies** → gate on `*_anchored` only
- **Breakout / momentum strategies** → gate on `*_breaking_up` or `*_breaking_down`
- **Strategies indifferent to bar-level state** → gate on the raw period label (rare; should justify)

## Implications for pre-deployment checks {#pre-deployment-checks}

Per [[strategy-validation-discipline]], pre-deployment audits should include a **bar-level validation check** for any strategy gating on a period-level signal:

1. **Identify** every period-level signal the strategy gates on
2. **Compute** the bar-level pass rate inside labeled periods (close ∈ claimed geometry?)
3. **If pass rate < 80%** — strategy is implicitly betting that the bar-level mismatch doesn't affect the mechanism. Default to bar-level overlay unless mechanism justification is explicit.

The 80% threshold is calibrated to the team-detector case (75% bar-level FAIL was catastrophic). Lower thresholds may be tolerable for mechanisms that work despite the mismatch — but the discipline is to **make the choice explicit**, not inherit it by ignoring the question.

## Open empirical questions

1. **Does the pattern apply to vol-regime classifications?** The wiki's [[../../trading/mechanisms/vol-regime-detection]] page documents period-level vol regimes (LV / MV / HV). Bar-level audit not yet run — would inform whether vol-regime gating needs the same overlay discipline.
2. **What is the bar-level pass rate for regime classifications based on slower indicators?** EMA-crossover regime gates have inherent timing decoupling per [[../../trading/mechanisms/regime-gate-vs-bias-filter]]; the bar-level overlay is a different lens on the same phenomenon. Quantitative cross-check pending.
3. **Cross-asset stability** — the team-detector worked example is BTC-specific. ETH and SOL data layers have the same detector logic; bar-level audit pending.

## Related

- [[team-stable-range-detector-methodology]] — canonical worked example; data-layer audit + refinement specification
- [[../../trading/mechanisms/regime-gate-vs-bias-filter]] — bar-level state IS the validated bias filter layer (per-bar match check) vs raw regime gate (period-level claim)
- [[strategy-validation-discipline]] — pre-deployment audit checklist now includes bar-level validation per §"Implications for pre-deployment checks" above
- [[forecast-vs-tradeable-gap]] — period-level forecast quality ≠ bar-level trade quality; this overlay pattern is the L1→L2 translation gate for period-level signals
- [[path-quality-diagnostics]] — bar-level mismatch is one mechanism by which "good aggregate metrics" can hide bar-level pathology
- [[online-regime-detection]] — the **strategy-level analog** of this pattern: its §measured-haircut shows the smoothed→filtered regime-label haircut (regime_v3 Sharpe 1.638→1.289) localizing entirely to the directional label-dependent subs, exactly as bar-level mismatch localizes to label-load-bearing bars; the §implemented-baseline causal detector is the filtering-mode upgrade this overlay's "validate THIS bar" discipline points toward
- [[../../trading/mechanisms/breakout-direction-asymmetry-by-macro]] — applied case; uses `team_is_stable_breaking_up` / `team_is_stable_breaking_down` (drifted-direction flavors) as breakout-direction gates per the overlay's mechanism-class mapping
- [[../../trading/mechanisms/failed-breakout-reversal]] — applied case; fade/mean-reversion mechanism class should gate on `team_is_stable_anchored` per the overlay's mechanism-class mapping (forward-prescriptive; existing test results pre-refinement used the raw `is_stable` layer)
- [[../../trading/mechanisms/vol-regime-detection]] — candidate next application; vol-regime period labels haven't been bar-level-audited yet (open question §3)

## Sources

- Worked example data: the backtesting lab + audit findings in the research task queue
- Falsification evidence: `regime_v1` (-$472, raw layer) vs `regime_v3` (+$451 bull / +$57 bear, bar-level overlay)
- Phase 4 cross-regime validation: BTC bear window 2022-04 → 2023-09 confirms pattern holds (37% anchored / 63% breaking)
