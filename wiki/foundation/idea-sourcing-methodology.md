---
title: "Idea Sourcing Methodology"
status: seedling
created: 2026-04-30
updated: 2026-04-30
tags: [methodology, idea-sourcing, seed-library, mechanism-first, problem-1]
---

# Idea Sourcing Methodology

> **TL;DR**: How to find good initial ideas for crypto day/swing trading strategies. The meta-process upstream of v5 — turning ad-hoc human-written prompts into curated mechanism-grounded seeds. v5 takes a directive and produces hypotheses; this page is about *where good directives come from*. Maps to Problem #1 in the team's 4-problem framing — "agent set that generates well-founded ideas."

## The problem this page addresses

The `idea-generation-prompt-v5` discipline (lives in `finance/`, not in the wiki) is applied at the directive-input layer: validate (Stage 1), enumerate within a family (Stage 2), score on 4 axes (Stage 3), adversarial pre-pass (Stage 3.5), elaborate (Stage 4). But v5 is **downstream of seed quality**. If a directive is pattern-shaped, v5 declines it (Stage 1) — but only if the workflow actually runs v5. When the team feeds prompts directly to a generic-AI implementation step (per the Apr 2026 [[trading/rejected/candle-morphology-batch-2026-04|candle-morphology batch]]), v5's discipline is bypassed.

The deeper question: *what makes a good seed in the first place?* This page is the meta-page on **how good ideas get sourced** — the curated seed library this catalog will grow to become.

## The 7 sourcing-method categories

Initial taxonomy. Each gets populated as research surfaces examples. (Cross-market transfer is dominant among these — see [[ARCHITECTURE|wiki architecture]].)

### 1. Cross-market transfer (currently dominant per worldview prior #4)

Take a mature-market strategy (FX, commodities, equities, hedge fund lit) and translate or mutate for crypto. Faithful translation requires identifying *why* it works in the source market — built-in plausibility filter.

- **Examples already running**: funding-rate carry = forex carry analog, P2P lending = repo analog, grid bot = market-making analog (cross-market transfer; see [[ARCHITECTURE|wiki architecture]])
- **Honest caveat**: famous transfers are gone. Opportunity is in (a) mid-tier literature mechanisms, (b) crypto-native mutations using funding/on-chain/liquidation/basis data, (c) revival of dead-in-equities patterns in low-cap crypto
- **Source priority**: Quantpedia (700+ categorized strategies), AQR working papers, SSRN q-fin, Schwager Market Wizards, Chan, Harris, Grinold-Kahn

### 2. Microstructure observation

Direct observation of order flow, footprint patterns, liquidation maps, on-chain whale movement. Mechanism = observable participant behavior visible in current data.

- **Crypto advantage**: 24/7 markets, on-chain visibility (legal here, illegal in equities), exchange-specific microstructure, liquidation cascades visible via Coinglass/Skew
- **Source priority**: Glassnode, CryptoQuant, Coinglass, Deribit Insights, Larry Harris *Trading and Exchanges*

### 3. Failure-mutation

Take a backtest that partially succeeded or failed cleanly, apply one of the 7 mutation operators (see [[ARCHITECTURE|wiki architecture]]), generate the next idea. Mechanism survives across mutation; operationalization changes.

- **Discipline**: mutation without re-articulated mechanism is parameter fishing. The `operator_justification` field is mandatory (per the research process's the research output conventions registry, shape #4)
- **Best worked example**: when this page grows, the mutation history of an operator's running strategies becomes the canonical example here

### 4. Event-anchored

Funding settlement, options expiry, halving cycles, regime shifts (BTC dominance rotation). Mechanism = predictable flow around scheduled events.

- **Day/swing-relevant**: 8-hour funding cycles, monthly options expiry, weekly rebalancing
- **Source priority**: CME Futures Guides (term structure), Deribit Insights (vol/options), team's own a funding-carry strategy experience

### 5. Behavioral classics revival

Apply pre-1980s behavioral-market wisdom (Livermore, Wyckoff, Edwards & Magee, Kindleberger, Chancellor) to current crypto under worldview prior #1 (crypto ≈ pre-regulation US equities).

- **Status**: ✅ [[foundation/sources/lefevre-reminiscences|Lefèvre]], ✅ [[foundation/sources/wyckoff-method|Wyckoff]], and ✅ [[foundation/sources/kindleberger-manias-panics-crashes|Kindleberger]] all ingested 2026-05-03/04. Murphy already ingested as bridge. Pre-regulation/behavioral cluster's core complete.
- **Caveat from current evidence**: Apr 28 v4 batch tested Wyckoff distribution coherence collapse → falsifier fired (mechanism not present in current BTC perps under tested parameterization). Behavioral wisdom doesn't auto-transfer — needs the same v5 discipline applied to translation
- **From Lefèvre ingest (5 mechanisms newly available as input candidates)**: tape reading [[foundation/market-wisdom/tape-reading]], sit-tight discipline [[foundation/market-wisdom/sit-tight-discipline]], pivotal points [[trading/mechanisms/pivotal-points]], group-leader divergence [[trading/mechanisms/group-leader-divergence]], line-of-least-resistance (section in [[trading/mechanisms/support-resistance]]). Each is mechanism-grounded with a structured hypothesis or hypothesis-shaped section — usable as v5 input candidates
- **From Wyckoff ingest (3+1 frameworks newly available)**: Composite Operator [[foundation/market-wisdom/composite-operator]], Three Laws [[foundation/market-wisdom/wyckoff-three-laws]], Accumulation/Distribution cycle with spring/upthrust [[trading/mechanisms/accumulation-distribution]]. The accumulation-distribution page has *two* structured hypotheses (spring + upthrust) directly usable as v5 input candidates.
- **From Kindleberger ingest (3 frameworks newly available)**: 5-stage bubble cycle [[foundation/market-wisdom/bubble-cycle-stages]], Minsky three-finance taxonomy [[foundation/market-wisdom/minsky-three-finance]], 2020-2022 worked example [[foundation/macro/crypto-2020-2022-cycle]]. **The bubble-cycle stage is a regime-conditioning input — it tells you which mechanism family to use**, not a mechanism itself. This is meta-level methodology: stage identification → strategy family selection → mechanism choice.

### 6. Academic literature → translation

Read mid-tier finance / market-microstructure / behavioral-economics papers, translate to crypto. Different from #1 (which is mature-market practitioner strategies); #6 is academic mechanism findings.

- **Source priority**: arXiv q-fin, SSRN, Journal of Financial Economics, Journal of Finance
- **Yield expectation**: lower per-paper than #1, but more novel mechanisms when they hit. Substance rubric required (much pure noise in academic finance)

### 7. Data-mining-then-mechanism (a team member's approach — risk-flagged)

Find positive backtests in historical data, then articulate a mechanism that explains them. Reverse of mechanism-first.

- **Risk**: pattern-mining without mechanism grounding produces overfit false positives (per design discipline; this is the Apr 28 risk flagged in the project notes research program`)
- **Mitigation**: every data-mined pattern must articulate a candidate mechanism + falsifier + walk-forward / OOS check before becoming a strategy
- **Worth tracking** as one of the 7 because a team member owns this stream and it may surface real mechanisms — just needs the discipline check

## Failure patterns (worked examples)

This is where the page compounds — each failed batch reveals a sourcing-methodology principle.

### Pattern A: candle-morphology-as-mechanism

**Source:** [[trading/rejected/candle-morphology-batch-2026-04]] (5 prompts × 2 directions = 10 strategies, all backtested negative).

**Diagnosis:** every input prompt was *pattern-shaped* — it named an observable candle feature and asked for a strategy. None articulated the underlying mechanism. Generic AI elaborated each into two contradicting directions (momentum + mean-reversion) — the "both-directions-hedge" slop tell.

**Operational rule:** a prompt where the observable IS its claimed signal is **tautological** and should fail v5 Stage 1 declination.

- **Examples that fail**: *"trade wick rejection clusters"*, *"trade body-shrinkage exhaustion"*, *"trade streak reversals"*
- **Examples that would pass**: *"fade late-long stop-loss clusters identified via liquidation maps after a 1.5x ATR move"* (mechanism: forced unwinding; observable: ATR move + liq cluster proximity; falsifier: testable via liq map data)

**Why this matters at the source:** input prompts must articulate at least one of {mechanism / cited source / crypto carrier feature}. The observable can come second.

### Pattern B: monocultural upstream premise

**Source:** Apr 28 v4 batch — 5 hypotheses (Livermore last-thrust, Wyckoff Phase B distribution, FX session-boundary fade, FX carry + crypto funding crowding, Kahneman/Edwards-Magee anchor gravity). All shared an unstated streak-based-reversal premise. Hypothesis #2 backtested with falsifier fired (equity curves negative across all 5 PNL quantiles).

**Diagnosis:** the batch failed at the upstream layer, not the per-hypothesis layer. The shared premise was the bug. Two hypotheses that look surface-different but answer the same upstream question are not two hypotheses — they are one hypothesis operationalized twice.

**Operational rule:** when emitting a batch from a single family input, explicitly check whether candidates share an unstated upstream premise the input didn't name. Surface as adversarial-audit input.

**Distinction from Pattern A:** Pattern A is mechanism-articulation failure at the prompt level (no mechanism stated). Pattern B is mechanism-family monoculture *despite* mechanism articulation (each hypothesis had a mechanism — they just shared an upstream assumption).

### Pattern C: tip-as-hope-delivery (named by Livermore 1923; structurally distinct from A and B)

**Source:** [[foundation/sources/lefevre-reminiscences|Lefèvre]], Ch XVI ("Tips: the universal disease") and Ch XXIV (insider distribution-via-favor catalogue).

> "It has always seemed to me the height of damfoolishness to trade on tips… Men do not take tips because they are bally asses but because they like those hope cocktails… To be told precisely what to do to be happy in such a manner that you can easily obey is the next nicest thing to being happy." (Ch XVI)

> "Acting on 'inside' tips will break a man more quickly than famine, pestilence, crop failures, political readjustments or what might be called normal accidents." (Ch XXIV)

**Diagnosis:** when a sourcing input is functionally a tip ("a credible person told me X is going up"), its primary function is *hope-delivery to the recipient*, not information-transfer. The recipient feels good (someone has given them a plan); they don't need to think (the work is outsourced); they have someone to blame if it fails (the tipper). This is *exactly the failure mode* Pattern A and B don't capture — A is mechanism-articulation failure, B is monoculture, *C is the absence of independent diagnosis*.

**Crypto application — the load-bearing concern.** Most "good ideas" reaching a crypto trader come through tip-shaped channels: CT influencer recommendations, podcast guest plugs, Telegram pump groups, paid newsletter signals, "alpha leaks" from Discord servers. The hope-delivery mechanism is identical to the 1923 dinner-party tip; only the medium changed.

The Wisenstein/Borneo Tin operation (Ch XVI) is the canonical hostile case: the manipulator engineered a tip-delivery vector specifically to feed Livermore false information through a trusted intermediary. *Crypto exhibits this at scale* — coordinated tip-feeding through trader social networks targeting known whale accounts is documented and ongoing.

**Operational rule:** any sourcing candidate that arrives in a *tip shape* (someone-told-me-X) must be either (a) discarded, or (b) treated as raw material for *independent* mechanism-articulation. The tip itself is not a hypothesis. The trader's job is to extract whatever observable underlies the tip and re-derive a mechanism + falsifier independently.

**Distinction from Patterns A and B:** A and B are about *prompt structure*. Pattern C is about *prompt provenance*. A prompt can be perfectly mechanism-articulated *and still be a tip* if the trader didn't reach the mechanism independently. The provenance failure is structurally upstream of the structural failure.

**Per the bear-raid-inversion rule** (catalogued in [[foundation/market-structure/market-manipulation-taxonomy]]): when the tip delivery is accompanied by a "shorts are attacking" / "FUD attack" framing during a sustained decline, the rule says *sell harder*, because the framing function is to keep the recipient from selling while the actual seller (often the tipper or their associates) distributes. This is a specific high-leverage application of Pattern C.

## Operational rules emerging (consolidated)

1. **Pre-generation tautology check**: any prompt whose stated trigger IS its claimed signal should be declined. Ask *"what mechanism is the observable evidence FOR?"*
2. **Pre-generation mechanism check**: prompt must articulate at least one of {mechanism / cited source / crypto carrier feature} before generation runs
3. **Batch-pattern check**: when emitting a batch, explicitly check for shared unstated upstream premise. 3+ candidates with same hidden premise = stop, diversify, regenerate
4. **v4-vs-generic-AI evidence**: same input under v4 vs generic-AI yields fundamentally different output quality. Discipline at the prompt level matters more than discipline at the implementation level
5. **Provenance check (Pattern C)**: prompts whose origin is a tip-shape (someone-told-me-X) must be either discarded or used only as raw material for *independent* mechanism articulation. The tip is not a hypothesis. Per Livermore Ch XVI: tips are hope-delivery vehicles, not information-delivery vehicles.
6. **Bear-raid-inversion rule**: when a sourcing input arrives during a sustained decline accompanied by "shorts attacking / FUD attack" framing, sell harder rather than treat the framing as protective. Per Livermore Chs XV and XXIII; catalogued in [[foundation/market-structure/market-manipulation-taxonomy]].

## Open questions

- What's the highest-yield methodology source for crypto-specific day/swing? Hypothesis: Quantpedia (categorized) + Schwager Market Wizards (interview methodology extraction) + AQR working papers (firm-level approach articulation). Untested as of session 1.
- How to operationalize the "tautology check" — what makes *"observable IS signal"* detectable algorithmically vs. requiring human judgment?
- Can the team's existing strategies (P2P lending, a funding-carry strategy, grid bot) be reverse-engineered to extract the sourcing methodology that *worked*? They're documented working analogs (carry, market-making, repo) — the methodology behind their selection should populate Category 1.
- What's the relationship between sourcing-method category and capability-gap class? Some methods (#2 microstructure, #6 academic) require capabilities this research program currently lacks (on-chain decoding, paper search). Worth mapping.

## Cross-pollination with other workflows

- **Wiki-sweep arm** (defined in `../../../the research process`) reads this page when validating new directives — the failure patterns serve as graveyard signals at the methodology level
- **Mechanism-translation arm** invokes this page's Category 1 (cross-market transfer) when v5 runs — `mechanism-transfers ≥ 4` requires the carrier feature this page catalogs
- **Capability-gap arm** flags missing capabilities tied to specific sourcing methods (e.g., #2 needs on-chain decoding capability)
- **Wiki-refinement arm** updates this page when new failure patterns surface OR when worked examples populate categories

## Key Takeaways

- **v5 is downstream of seed quality** — this page addresses what makes a good seed in the first place
- **7 sourcing-method categories** form the seed-library scaffolding (cross-market transfer remains dominant per worldview prior #4)
- **Pattern A (mechanism-articulation failure), Pattern B (mechanism-family monoculture), and Pattern C (tip-shaped provenance, named by Livermore 1923)** are distinct upstream-failure modes
- **Tautology check**: any prompt where the observable IS its claimed signal should be declined at v5 Stage 1
- **Batch-pattern check**: shared unstated upstream premises across candidates indicate the upstream IS the bug
- **Provenance check**: tip-shaped inputs are hope-delivery, not information; must be re-derived independently or discarded
- **v4-vs-generic-AI control evidence**: same input under disciplined prompt yields one mechanism-grounded direction; under generic AI yields two contradicting directions with no mechanism
- **Existing-strategy reverse-engineering** (P2P / a funding-carry strategy / grid bot) is an untapped source for populating Category 1 retrospectively

## Related

- [[foundation/market-structure/crypto-carrier-features]] — sibling synthesis page; the carrier-feature catalog that Pattern A's "no crypto-specific carrier feature" tell points to. Every Category 1 (cross-market transfer) and Category 2 (microstructure observation) candidate must satisfy the carrier-feature requirement using that catalog.
- **Mechanism family aggregator pages** (the seed library, ingested 2026-05-04): [[trading/mechanisms/behavioral-positioning-unwind]], [[trading/mechanisms/liquidation-cascade-aftermath]], [[trading/mechanisms/funding-cycle-dynamics]], [[trading/mechanisms/session-boundary-effects]], [[trading/mechanisms/calendar-effects]]. **These replace "human writes pattern-shaped directive" with "human picks from curated family"** per the original v5 design intent. Each family page provides operational checklist for designing new strategies in that family.
- [[trading/rejected/candle-morphology-batch-2026-04]] — first populated worked example (Pattern A)
- [[foundation/sources/lefevre-reminiscences]] — Pattern C source (Chs XVI, XXIV); also fills the #1 source for Category 5 (behavioral classics revival)
- [[foundation/market-structure/market-manipulation-taxonomy]] — bear-raid-inversion rule (catalogued there); ties Pattern C to manipulation defense
- [[ARCHITECTURE|wiki architecture]] — AI's structural limitations (the underlying constraint); mechanism-first framing (the gate this page operationalizes); cross-market transfer (Category 1's basis); mutation operators (Category 3's toolkit)
- [[foundation/sources/lineage]] — source priority queue feeding Categories 1, 5, 6
- [[foundation/relations-and-themes]] — cross-source synthesis (sibling synthesis page)
- `idea-generation-prompt-v5.md` — the downstream pipeline this page seeds

## Sources

- April 2026 candle-morphology batch (Pattern A worked example) — `Downloads/dsl/{1..5}.json`
- v4 worked example (Pattern B worked example) — Apr 28 in the project notes research program`
- Lefèvre, Edwin. *Reminiscences of a Stock Operator* (1923), Chs XV, XVI, XXIII, XXIV (Pattern C source). Source page: [[foundation/sources/lefevre-reminiscences]].
- `idea-generation-prompt-v4.md` and `idea-generation-prompt-v5.md` — current state of the downstream pipeline
- Team's currently-running strategies (P2P lending, a funding-carry strategy, grid bot) — Category 1 retrospective (untapped, future session)
