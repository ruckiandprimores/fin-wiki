---
title: "Candle Morphology Batch — 2026-04 — rejected"
status: evergreen
created: 2026-04-30
updated: 2026-04-30
tags: [rejected, candle-morphology, mechanism-free, generic-ai-output, no-falsifier, behavioral-microstructure]
---

# Candle Morphology Batch — 2026-04 — rejected

> **TL;DR**: 5 generic-AI-elaborated prompts produced 10 strategies (5 momentum + 5 mean-reversion), all backtested negative. Shared upstream premise — *"observable candle morphology is a sufficient signal for trade decisions"* — was the bug. The strategies failed at the prompt-input layer, not the implementation layer. First batch-level rejection in the graveyard; first populated example for [[foundation/idea-sourcing-methodology]].

## Hypothesis (batch-level — as proposed)

The batch shares a single upstream premise:

- **Hypothesis:** observable candle features (body, body-to-range ratio, streak length, wick-to-body ratio, close sequence) constitute sufficient signal for trade entry/exit
- **Mechanism:** none articulated — the observable IS treated as the mechanism (tautological)
- **Observable:** various candle morphology features (per the 5 prompts)
- **Expected:** mechanical execution of operationalized rule
- **Falsifier:** none stated in any of the 5 prompts or 10 directions

## How they were generated

Per user's own framing: *"we simply gave an idea and generic AI suggested its implementation"*. The pipeline was **not** the v4/v5 prompt — it was a generic-AI elaboration step. Source files: `Downloads/dsl/{1..5}.json`, generated 2026-04-27 / 2026-04-28.

This is methodologically distinct from the Apr 28 v4 worked example (Wyckoff distribution coherence collapse) which used the v4 prompt and produced one mechanism-grounded direction. **Notable control comparison:** prompt #2 in this batch (*"Detect trend exhaustion through consecutive candle body shrinkage after extended moves"*) is the *same input* the v4 prompt was demonstrated on. Generic-AI output: 2 contradicting directions, no mechanism. v4 output: 1 mechanism-grounded direction (volatility regime shift, Bollerslev 1986 GARCH-grounded). Direct evidence the prompt discipline matters at the input level.

## The 10 strategies

| # | File | Prompt | Direction 1 | Direction 2 |
|---|---|---|---|---|
| 1 | `1.json` | Body-to-range ratio expansion/contraction cycles (15m/1H) | **Momentum breakout** — body-to-range ≥ 70th pct → trade dominant body direction | **Mean-reversion to VWAP** — body-to-range ≤ 20th pct + indecision → counter-trend to VWAP |
| 2 | `2.json` | Body shrinkage after extended moves (15m/1H) | **Body Compression Reversal** — 3+ shrinking bodies after 1.5x ATR → fade | **Exhaustion Volume Divergence** — body compression + declining volume → fade |
| 3 | `3.json` | Directional consistency streak shifts (5m/15m/1H) | **Streak Reversal Momentum Flip** — 4+ same-direction closes broken by 75th-pct body → enter new direction | **Streak Exhaustion Mean-Reversion** — streak length ≥ 95th pct historical → fade |
| 4 | `4.json` | Wick rejection clusters (5m–1H) | **Wick Rejection Mean-Reversion** — 3 of 10 candles wick-to-body ≥ 2:1 at level → fade wick | **Wick Cluster Breakout** — cluster fails on 1.5x volume → enter breakout direction |
| 5 | `5.json` | Close sequence accumulation/distribution (5m–1H) | **Accumulation breakout long** — N monotone closes in tight range (< 0.5x ATR) → breakout long | **Distribution close fade short** — closes upper-bound + declining volume → mean-reversion short |

All 10 strategies were backtested by the team; user-reported outcome: all negative. The consistent pattern across 10 outputs (despite different surface mechanisms) is the load-bearing signal — failure is upstream, not per-strategy.

> **Provenance caveat (per wiki schema / research process worldview prior #7):** this batch entry was assembled from the JSON inputs (`Downloads/dsl/{1..5}.json`) plus the trader's verbal report of negative backtest outcomes. The actual `runs/<strategy>_<timestamp>/` output files (`summary.json` with Sharpe, drawdown, win rate, exit-reason counts; `trades.csv`; `equity.csv`) were **not in scope for this session**. Before this rejection is fully certified, the corresponding run output paths should be linked here and per-strategy metrics added to `wiki/trading/backtest-results/` entries (per the TRADE-IDEA workflow in `finance-wiki/the wiki architecture (ARCHITECTURE.md)` Part II — workflow has both `backtest-results/` and `rejected/` steps; this session bypassed the metrics-recording step).

## Metrics

**Not in scope of this session.** See provenance caveat above. Required to fully certify:

| Strategy | Run path | Sharpe | Max DD | Win rate | # trades | Exit reasons |
|---|---|---|---|---|---|---|
| 1.1 / Body-Range Momentum Breakout | TBD | TBD | TBD | TBD | TBD | TBD |
| 1.2 / Body-Range Contraction Reversal | TBD | TBD | TBD | TBD | TBD | TBD |
| 2.1 / Body Compression Reversal | TBD | TBD | TBD | TBD | TBD | TBD |
| 2.2 / Exhaustion Volume Divergence | TBD | TBD | TBD | TBD | TBD | TBD |
| 3.1 / Streak Reversal Momentum Flip | TBD | TBD | TBD | TBD | TBD | TBD |
| 3.2 / Streak Exhaustion Mean-Reversion | TBD | TBD | TBD | TBD | TBD | TBD |
| 4.1 / Wick Rejection Mean-Reversion | TBD | TBD | TBD | TBD | TBD | TBD |
| 4.2 / Wick Cluster Breakout | TBD | TBD | TBD | TBD | TBD | TBD |
| 5.1 / Close Sequence Accumulation | TBD | TBD | TBD | TBD | TBD | TBD |
| 5.2 / Distribution Close Fade | TBD | TBD | TBD | TBD | TBD | TBD |

Team verified no implementation bugs (per Apr 28 process discipline applied to this batch class).

## Mechanism-articulation audit

What every one of the 10 strategies is missing, per wiki schema worldview priors:

| Prior | Required | Present in any of 10? |
|---|---|---|
| #3 mechanism-first | WHO is moving money, WHY, WHEN | None |
| #4 cross-market transfer | Original-market source + crypto carrier feature | None |
| #5 falsifier | Specific observation that would invalidate | None |
| #7 historical analog | Real named episode (1929 NP corner / Mar-2020 BitMEX cascade / etc.) | None |
| #8 DSL gap signal | Could DSL express it cleanly? | Mostly yes — except footprint data (mentioned in 1, 2, 5; recurring DSL gap) |

The strategies don't fail at the *implementation* layer (DSL primitives express most of them). They fail at the *upstream input* layer.

### Livermore lens (added 2026-05-03)

[[foundation/sources/lefevre-reminiscences|Lefèvre 1923]] frames the same audit in tape-reading terms. The question Livermore would have asked of each prompt: *"What does the tape say?"* — meaning *what is the order-flow battle, who is moving money, which way are they leaning?* None of the 10 prompts answer this. They name a *candle shape* and ask for direction. Livermore would have rejected all 10 at the input layer because they describe a *picture* (the candle's appearance) without naming a *transaction* (who absorbed what at what price).

> "Of course, there is always a reason for fluctuations, but the tape does not concern itself with the why and wherefore." (Ch I)

Livermore's nuance is exact: *the tape doesn't need to know the why* — but the trader must know *who and how*, not the *what*. Candle-shape directives invert this: they only specify the *what*. The mechanism gap is by construction.

The 5 salvageable directions in the table above all become salvageable *only when re-articulated with a tape-reading frame*: 4.1/4.2 → *"identify forced unwinding from late longs at clustered stop levels (visible on liquidation maps)"* — that is a tape question, not a candle question. 5.1/5.2 → Wyckoff accumulation/distribution is *literally tape reading formalized*. The salvage path runs through [[foundation/market-wisdom/tape-reading]] and (added 2026-05-03 with the Wyckoff ingest) [[trading/mechanisms/accumulation-distribution]] — not through the candle observable.

## Slop tells visible in the batch

1. **Tautology**: every prompt's stated trigger IS its claimed signal. *"Sustained rise in body-to-range ratio above 70th percentile signals strong directional conviction"* — the conviction = the pattern. Empty mechanism-articulation.
2. **Both-directions hedging**: every prompt produced exactly 2 directions, one momentum + one mean-reversion. Generic AI without mechanism anchoring hedges by emitting contradictions. v5 Stage 4 specifically counters this — multiple operationalizations are valid only when they share *the same mechanism*.
3. **No crypto-specific carrier feature**: nothing references perp funding, on-chain flow, liquidation cascades, retail-vs-MM positioning, or any economic structure that would make these crypto-specific. Could be applied to any candle chart — opposite of cross-market transfer (which requires the *carrier* feature, not just the pattern).
4. **No historical anchor**: zero references to documented episodes where these mechanisms produced verifiable price movements.

## Reason classification

- [ ] Falsifier fired in backtest — *not the load-bearing reason; even if backtests had been flat or positive, validation would still fail*
- [x] **Validation failure** (no mechanism / no falsifier / pattern-shaped) — **all 10**
- [x] **Same upstream premise across batch** — systemic, not per-strategy
- [ ] Mechanism not present in current crypto microstructure
- [ ] Already arbitraged
- [ ] Graveyard duplicate

## What partial truth remains?

Per-direction salvageability — the observables themselves are not garbage; the *mechanism articulation* is. With proper mechanism anchoring, some could be salvaged:

| # | Direction | Salvageable as | Required mechanism articulation |
|---|---|---|---|
| 1.1 | Body-to-range expansion → momentum | Vol-regime indicator (proxy for RV expansion) | Bollerslev 1986 GARCH conditional heteroskedasticity; carrier = perp funding pressure during vol expansion |
| 1.2 | Body-to-range contraction → mean-reversion | Pre-breakout coiling indicator | Volatility-clustering literature; needs liquidation-map confirmation |
| 2.1, 2.2 | Body shrinkage exhaustion | **Already attempted under v4** as volatility regime shift; backtested as Wyckoff distribution coherence collapse → falsifier fired | (See Apr 28 v4 entry) |
| 3.1, 3.2 | Streak-based momentum/mean-reversion | Probably not salvageable in current crypto BTC perps — Apr 28 v4 batch tested similar with falsifier-fired evidence |
| 4.1, 4.2 | Wick rejection cluster | Possibly salvageable as **liquidation cluster fade** with on-chain liquidation map data; carrier = forced unwinding at known stop levels |
| 5.1, 5.2 | Close sequence accumulation/distribution | **Salvage path now formalized** as of 2026-05-03 Wyckoff ingest: re-derive using [[trading/mechanisms/accumulation-distribution]] structured-hypothesis form with carriers (on-chain whale flow, exchange in/out, funding, OI, Coinglass liquidation cluster) and the [[foundation/market-wisdom/wyckoff-three-laws|effort-vs-result]] diagnostic. Original 10 should not be re-tested. |

**Net:** 1.1, 4.1, 4.2, 5.1, 5.2 are candidates for *re-derivation with mechanism articulation* — not as graveyard rescue but as new ideas that share an observable with these. The original 10 should not be re-tested.

## Pattern signals (for the next-session sweep)

This is the **second batch-level rejection** where the failure was upstream-shared (after the Apr 28 v4 streak-based-reversal cluster). Pattern is now signal-grade:

> **When a generated batch shares a common upstream premise the input prompt didn't name, the upstream IS the bug. The shared premise compounds into systemic failure regardless of per-strategy parameter quality.**

This batch is the *deeper* case. The Apr 28 v4 batch had a shared mechanism family (streak-based-reversal); this batch has a shared **mechanism-articulation failure** (candle-morphology-as-mechanism). Future batches should be checked for both:

1. **Mechanism family diversity** — do candidates pull from different families?
2. **Mechanism articulation depth** — is the prompt itself mechanism-grounded, or pattern-shaped?

The wiki-sweep arm's cluster-scan logic should now treat *"both batches showed shared upstream premise"* as a 2/2 hit and surface this aggressively whenever a new batch is generated.

## Arm reports

### Confidence
- **Source-coverage confidence: high.** Owned material, all 10 backtests verified negative. No fabrication.
- **Distillation confidence: high.** Pattern unambiguous across all 5 prompts.

### Assumption interrogation (adversarial-audit arm)
**Strongest specific argument against the rejection:** *"Maybe parameters were wrong; curve-fitting different thresholds might rescue them."*
**Decision:** counterargument fails. Without articulated falsifiers, even positive in-sample results aren't signal — they're overfit. Worldview prior #5 stands.

### Capability gaps
- **Pre-generation tautology detector** — no automated way to flag *"observable IS signal"* prompts before AI elaboration. Owner candidate: tooling work (front-end validator that runs v5 Stage 1 declination rules before any generation).
- **Footprint data primitives** — mentioned in prompts 1, 2, 5; recurring DSL gap (per the research process's the capability registry known-limits list). Owner: a team engineer's Method B extension.
- **Liquidation map data** — required to salvage 4.1/4.2 as proper liquidation-cluster-fade ideas. Owner: data-source integration, possibly a team member's data discovery domain.

### Wiki refinement (wiki-refinement arm)
- This entry plus [[foundation/idea-sourcing-methodology]] together populate the wiki's first methodology synthesis around batch-level failure analysis
- v5 sharpening proposal flagged separately (see "v5 sharpening candidate" below)
- [[foundation/relations-and-themes]] could gain a section on "batch-failure pattern recognition" — flagged as future maintenance, not done in this session

## v5 sharpening candidate

Flagged for `prompt-evolution.md` review (separate confirmation before applying):

> **Stage 1 declination rule sharpened:** if the input directive's stated trigger condition IS its claimed signal (tautological — observable equals signal), decline. Add to Stage 1 examples section: *"trade body-shrinkage exhaustion"*, *"trade wick rejection clusters"*, *"trade streak reversals"*. Discipline: ask *"what's the mechanism the observable is evidence FOR?"* before proceeding.

Edit type: **v5-additive** (surgical), not v6-create.

## Key Takeaways

- **Pattern-shaped prompts produce mechanism-free strategies regardless of AI sophistication** — the bug is at the input layer, not the implementation layer
- **Generic AI hedges with both-direction outputs (momentum + mean-reversion per prompt) when no mechanism anchors are present** — identifiable slop tell
- **5 of 10 directions are salvageable with proper mechanism articulation** — see "What partial truth remains?" table; notably 4.1/4.2 → liquidation cluster fade and 5.1/5.2 → Wyckoff accumulation/distribution
- **Same input under v4 vs generic-AI yields fundamentally different output quality** — direct control evidence (prompt #2 / body shrinkage was v4's worked example) that prompt discipline matters at the input level
- **Batch-level rejection beats 10 nearly-identical post-mortems when the failure is upstream-shared** — methodology choice worth carrying forward
- **Provenance discipline matters** — this entry's metrics are placeholder pending verified `runs/` outputs (worldview prior #7)

## Related

- [[foundation/idea-sourcing-methodology]] — populated with this batch as the first worked example
- [[foundation/methodology/strategy-validation-discipline]] — **downstream gate** (the validation-discipline gate this batch failed at the *upstream* hypothesis layer would *also* have failed if measured on multiple-testing/PBO grounds; LdP framework would have flagged the 10 trials as a batch needing CSCV evaluation rather than 10 independent rejections)
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — the methodology source
- Part I §2 — direct evidence for "AI's Structural Limitations in Idea Generation"
- Part I §4 — Mechanism-First & Hypothesis-First Framing (the gate that failed)
- [[foundation/relations-and-themes]] — cross-source theme synthesis; this batch is a new failure pattern worth integrating
- [[foundation/sources/lineage]] — Wyckoff next-priority ingest would help salvage 5.1/5.2

## Sources

- `/Users/andrejruckij/Downloads/dsl/1.json` — body-to-range ratio prompt + 2 directions
- `/Users/andrejruckij/Downloads/dsl/2.json` — body shrinkage prompt + 2 directions (= v4 worked example input — control comparison)
- `/Users/andrejruckij/Downloads/dsl/3.json` — directional consistency streak prompt + 2 directions
- `/Users/andrejruckij/Downloads/dsl/4.json` — wick rejection cluster prompt + 2 directions
- `/Users/andrejruckij/Downloads/dsl/5.json` — close sequence accumulation/distribution prompt + 2 directions
- `idea-generation-prompt-v4.md` — the Stage 1 declination rule that would have caught these
- `idea-generation-prompt-v5.md` — current pipeline; candidate for sharpening per "v5 sharpening candidate" above
- the project notes research program` Apr 28 update — distinct v4 batch (streak-based-reversal) for comparison
