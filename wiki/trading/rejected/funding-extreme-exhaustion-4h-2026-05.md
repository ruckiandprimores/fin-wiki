---
title: "funding_extreme_exhaustion_4h — gate compound never fired (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, funding-cycle, exhaustion, design-form-failure, mechanism-untested, compound-gate-too-tight, BIS-WP-1087, behavioral-positioning-unwind, day-swing]
---

# funding_extreme_exhaustion_4h — gate compound never fired (2026-05)

> **Lead learning**: Strategy spec required four-axis compound gate (top-decile funding RANK over 90d × 30+ persistence cycles × 2×ATR price extension × bar reversal) — a regime that essentially **never occurred** in 2025-26 with these gate values. **0 of 11,664 grid runs eligible**: 10,260 runs produced 0 trades, 1,314 produced 1, 90 produced 2. Mechanism never operationally fired. **This is not a falsifier verdict on the mechanism** — it's a strategy-design verdict on the gate compound. The mechanism (BIS Working Paper 1087 carry exhaustion + Wyckoff Spring/Upthrust analog) may or may not be tradable; we couldn't test it in this form. Verdict: REJECT (design-form failure; mechanism intact, untested).

## What we learned

1. **Compound gates with multiple top-decile-or-stricter axes nearly always produce zero events in any single 1y window.** Each top-decile gate cuts ~90% of bars; four such gates compound to ~0.0001 base rate. Even if mechanism is real, the test is impossible to run in one window without relaxing gates.

2. **Spec-design failure ≠ mechanism failure.** Distinguishing these is critical for the graveyard. The wiki's [[foundation/methodology/strategy-validation-discipline|validation discipline]] §10 pre-registration protocol should include a *base-rate-of-firing* sanity check: if the compound gate fires <20 times/yr by historical estimation, the spec is design-flawed before backtest runs. funding_extreme_exhaustion's spec didn't include this check; we discovered the design flaw via 0/11,664 eligible runs.

3. **The mechanism story is intact and grounded.** BIS Working Paper 1087 carry-exhaustion framework + Wyckoff Spring/Upthrust analog + a funding-carry strategy's production performance all support the mechanism. The strategy-design failure shouldn't update mechanism credibility downward — only the operational form is rejected.

4. **Windows with documented extreme-funding regimes exist.** 2021 has multiple — May pre-cascade, Q4 euphoria, May 2022 pre-Terra. Higher base rate of triggering events. The cleanest path to test mechanism without confound is a 2021 window backtest (different data ingest required).

5. **Sibling strategy `funding_flip_recovery_4h` covers a related-but-distinct mechanism**: short-side mean-reversion after funding flip rather than top-of-distribution extreme. The flip mechanism is testable with looser gates and *did* fire in 2025-26 (rejected for different reasons; see [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05|funding-flip short-only]]).

## Hypothesis (Structured Format)

```
Hypothesis:    Persistent funding-rate extremes + price extension +
               bar reversal mark carry-trade exhaustion; reversal entry
               on bar reversal captures forced-unwind move.

Mechanism:     BIS Working Paper 1087 carry-exhaustion framework: when
               funding extremes persist long enough, late-cycle entrants
               accumulate margin pressure; price extension exhausts
               directional flow; bar reversal triggers cascade unwind.
               Wyckoff Spring/Upthrust analog: end-of-distribution test
               followed by reversal. Crypto carrier: funding rates
               directly observable; bar reversal signals real-time.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp 4h,
               $1000 capital, fee 0.04%. Pre-registered:
                 (a) Strategy-level: net ≥ $120/yr deployment-grade
                 (b) Hypothesis-level (paired): correlation with
                     `funding_flip_recovery_4h_short_only` < +0.4
                     (different sub-mechanism: extreme-persistence vs
                     flip-recovery)

Compound gate: top-decile funding rank over 90d × 30+ persistence
               cycles × 2×ATR price extension × bar reversal.

Expected:      Sharpe robust if mechanism present and regime cooperates;
               spec acknowledged regime-conditional firing rate. 20+
               trades/yr expected base-rate.
               expected_net_edge:
                 source: BIS WP 1087 + Wyckoff literature + a funding-carry strategy
                         production analog
                 value: +$80-150/yr 2021 window (regime-rich);
                        unknown 2025-26
                 independence: mechanism literature independent;
                              effect-size estimate cross-mechanism-
                              analog (a funding-carry strategy) — research-grade-only
                              verdict default per Part I §4

Falsifier:     Strategy-level: net < $120/yr → deployment fail
               Operational: < 20 trades/yr → compound gate too tight
               (operational-form failure, not mechanism failure)
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 4h, $1000, fee 0.04%)

| Metric | Value | Falsifier | Fired? |
|---|---:|---|:---:|
| n_eligible_runs | **0 of 11,664** | < 20 trades/yr | **YES (operational)** |
| Runs producing 0 trades | 10,260 (88%) | — | — |
| Runs producing 1 trade | 1,314 (11%) | — | — |
| Runs producing 2 trades | 90 (1%) | — | — |
| Runs ≥3 trades | 0 | — | — |
| Median Sharpe | n/a | — | mechanism never fired |
| Net/yr | n/a | — | n/a |

## Falsifier verdict: OPERATIONAL FALSIFIER FIRED — mechanism not falsifiable in this form

The pre-registered operational falsifier `<20 trades/yr` fired immediately. Per spec, this is the operational form of "compound gates too tight."

**This is a critically important distinction from a mechanism rejection**: the mechanism (carry exhaustion at funding extremes) was never tested. The strategy-design specification — the *compound gate* — is what failed. The mechanism remains an open hypothesis.

## What partial truth remains?

| Component | Status | Evidence | Salvage condition |
|-----------|--------|----------|-------------------|
| **Carry-exhaustion mechanism (BIS WP 1087)** | ✅ Untested in this form | Mechanism literature intact; a funding-carry strategy production performance is dealer-side analog | Test in window with documented extreme-funding regimes (2021) |
| **Compound gate "top-decile + 30+ persistence + 2×ATR extension + bar reversal"** | ❌ Rejected (operational form) | 0/11,664 eligible runs | Loosen gates: persistence [5, 10, 15] cycles instead of [15, 30, 90]; drop extension axis |
| **Wyckoff Spring/Upthrust phase-context analog** | Untested | The spec uses operational-trigger (funding extreme + bar reversal), not phase-context | Build phase-context-conditioned mutation gated on Wyckoff distribution criteria |
| **a funding-carry strategy dealer-side translation in production** | ✅ Working | a live dealer-side funding-carry deployment is working | Different strategy (dealer-side) but mechanism-related; the **edge IS being captured** by a funding-carry strategy, just not via this trader-side spec |

The strongest **path-forward** result: a funding-carry strategy production data shows the funding-cycle edge IS being captured, just by a dealer-side rather than trader-side mechanism. funding_extreme_exhaustion was attempting trader-side capture; a funding-carry strategy does dealer-side capture. Both can coexist; but trader-side requires the sharper carry-exhaustion event, which wasn't present in 2025-26 at the spec's gate compound.

## Mutation candidates (spec rework)

Per stocktake §"funding_extreme_exhaustion_4h" mutation list:

1. **Loosen persistence**: `[5, 10, 15]` cycles instead of `[15, 30, 90]`. Tests whether shorter persistence + extension fires more frequently in 2025-26.
2. **Drop extension requirement**: persistent extreme funding alone, without the 2×ATR price-extension confluence.
3. **Test on 2021 window**: multiple documented extreme-funding regimes (May pre-cascade, Q4 euphoria, May 2022 pre-Terra).

If v2 (this batch) fires sufficient trades AND timing-distinct from `funding_flip_recovery_4h_short_only`, mechanism survives op-form rework.

## Reason classification

- [x] **Operational falsifier fired** — compound gate produced 0 eligible runs
- [x] **Mechanism untested, NOT falsified** — strategy-design failure, not mechanism failure
- [x] **Mechanism literature intact** — BIS WP 1087 + Wyckoff + a funding-carry strategy production analog
- [ ] No falsifier articulated (clean pre-registration including operational falsifier)
- [ ] Already arbitraged
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥18 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far. funding_extreme_exhaustion adds a *grid-eligible-zero* result; trial count recorded but doesn't add to multiplicity adjustment for Sharpe ratios since no Sharpe computed.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Pre-registration discipline gap surfaced**: spec lacked a base-rate-of-firing sanity check. Future strategy specs should include estimated trades/yr from historical compound-gate firing rate before backtest runs. Documented as future-discipline addition.
- **Mechanism credibility unchanged.** This rejection only updates *spec-design* credibility, not mechanism credibility. a funding-carry strategy in production is the load-bearing positive evidence on the mechanism family.
- **Single-window backtest** — 2025-05/2026-04 only. The mechanism's target regime (extreme persistent funding) is window-specific; 2021 retest is the cleaner test.
- **Coinglass liquidation cluster data missing** — would sharpen exhaustion detection per stocktake's capability-gap note. Owner: a team member per Apr 28 division of labor.

## Key Takeaways

- **0/11,664 grid eligible runs = operational-design failure, not mechanism failure.** Critical distinction for the graveyard.
- **Compound top-decile gates compound to ~0.0001 base rate**: any spec with 4+ such gates needs a base-rate sanity check before backtest commits.
- **Mechanism literature intact**: BIS WP 1087 + Wyckoff + a funding-carry strategy production analog. The mechanism family is real.
- **a funding-carry strategy in production captures the funding-cycle edge dealer-side**; the trader-side capture this strategy attempted requires sharper exhaustion events than 2025-26 provided.
- **Loosen-gate mutations + 2021-window retest** are the salvage paths. Mechanism not closed.
- **Discipline lesson**: pre-registration should include estimated trades/yr from historical compound-gate firing rate.

## Related

- [[trading/mechanisms/funding-cycle-dynamics]] — mechanism family page (this strategy's parent class)
- [[trading/mechanisms/behavioral-positioning-unwind]] — sibling family (carry-exhaustion is a behavioral-positioning-unwind sub-mechanism)
- [[trading/mechanisms/accumulation-distribution]] — Wyckoff Spring/Upthrust phase context (untested salvage path)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — funding_flip short_only (related funding-cycle strategy that did fire; rejected for different reasons)
- [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]] — sibling funding-cycle rejection
- [[foundation/sources/wyckoff-method]] — Wyckoff Spring/Upthrust source
- Performance section — a funding-carry strategy dealer-side production data (mechanism-family-positive)
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law; pre-registration discipline gap surfaced

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"`funding_extreme_exhaustion_4h`"
- the backtesting lab — strategy spec + pre-registration
- BIS Working Paper 1087 — carry-exhaustion mechanism source
- Wyckoff *Method of Trading* — Spring/Upthrust phase-context analog
- a funding-carry strategy production performance — Performance Feb-Mar 2026
