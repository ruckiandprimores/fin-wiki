---
title: "cell_distribution_short_btc — REJECTED on bear-window falsifier (Wyckoff distribution mirror fails on BTC stable phases) (2026-05)"
status: evergreen
created: 2026-05-20
updated: 2026-05-20
tags: [rejected, wyckoff-distribution, distribution-short, btc, bear-direction, stable-phase, regime-conditional, mechanism-class-symbol-mismatch, n7-state-type-bear-failures, day-swing]
---

# cell_distribution_short_btc — REJECTED on bear-window falsifier (2026-05)

> **Lead learning**: The Wyckoff-symmetric short on BTC stable phases — mirror of the validated `cell_accumulation_long_btc` (PSR(>0) 93.1%) — **falsifies on N=1 cleanly** in the bear window. With the upstream gate (`bear_macro`) dropped per the double-conditioning diagnosis, the unmasked sub produced **−$56 net / Sharpe −0.74 / 11% WR / 9 trades** in BTC 4h bear window (2022-04 → 2023-09). Single-handedly **flipped bear-window aggregate Sharpe positive→negative** in `meta_btc_v1_loose_dist` (Sharpe +0.58 → −0.19). Three plausible failure-mechanisms identified; the **Wyckoff symmetry has asymmetric base rates** explanation is the most structural. Adds to N≥7 cross-mechanism state-type bear failures (see [[#cross-symbol-pattern-context]]).

## What we learned

1. **Accumulation/distribution Wyckoff symmetry is not realized at 4h on BTC stable phases.** `cell_accumulation_long_btc_v2` validates cleanly (PSR(>0) 93%); its direction-flipped mirror `cell_distribution_short_btc` does not. The asymmetry is mechanism-class-level, not parameter-level — same trigger logic, opposite direction, opposite outcome.

2. **State-type direction-flipping continues to fail on bear-direction at 4h.** This rejection extends the N≥7 evidence stack (see [[#cross-symbol-pattern-context]] table). The worldview-prior candidate that bear-direction tradeable edge lives in **event-type** mechanisms (cascades, failed breakouts, relief-rally exhaustion) and NOT in **state-type** mechanisms (regime-conditional direction-flipping) gets an 8th supporting witness. Pending Phase 2 sweep (`meta_btc_v2` event-bear B1/B2/B3) to determine whether to codify the prior.

3. **Loosening the double-conditioning gate was the falsifier-firing operation.** v1 of the meta had `bear_macro` upstream + `cell_distribution` cell-level + `is_stable=True` bar-level — three conjunction gates that locked the sub OFF for most of the bear window. v1_loose_dist dropped `bear_macro`. The sub then fired 9 times and the falsifier landed clean. **Methodology learning**: when a Wyckoff-symmetric mirror is tested, the gate stack should be designed to FIRE in the regime where falsification is expected, not to hide the sub via over-conditioning.

4. **Wiki Wyckoff mechanism page needs a per-direction asymmetry note.** [[../mechanisms/accumulation-distribution]] currently presents spring and upthrust as symmetric mirror mechanisms with the same operational structure. The N=1 BTC 4h falsification here suggests **distribution has different base rates than accumulation** — specifically, accumulation has structural tailwinds (retail spot accumulation, ETF inflows, basis carry, miner-issuance absorption) that distribution lacks symmetric mirrors of. Annotated as cross-link below; promotion to mechanism-page sharpening pending N=2.

## Hypothesis (Structured Format)

```
Hypothesis:    Wyckoff-symmetric short on BTC stable phases — when
               `cell_distribution` fires (1d HTF flow_sell + phase_range
               + vol_compression with 3-of-5 persistence) AND
               `is_stable=True` at the bar level, enter short. Mirror
               of validated `cell_accumulation_long_btc` (PSR(>0) 93%);
               expected to produce symmetric edge in bear macro.

Mechanism:     Wyckoff distribution mechanics (composite-operator
               unloads inventory at the top of a stable range; weak
               late-stage longs absorb the supply at premium prices).
               In crypto carrier terms — distribution detected via
               sustained sell-flow signal during stable consolidation,
               with low volatility characterizing the
               coordinated-distribution character (versus chaotic
               retail panic-sell). See [[../mechanisms/accumulation-distribution]]
               §upthrust for the source mechanism.

Observable:    BTCUSDT 4h, bear window 2022-04 → 2023-09. Cell fires
               when 1d carriers (flow_sell + phase_range +
               vol_compression) confirm 3-of-5 persistence; bar-level
               state is `team_is_stable_anchored` (post-2026-05-21
               refined layer; for this 2026-05-19 backtest, raw
               `is_stable=True` per data-layer state at run time).

Expected:      Per cell_accumulation_long_btc mirror: PSR(>0) ≥ 90%
               and Sharpe ≥ 0.5 trade-level; trade Sharpe ≥ −0.3 as
               minimum falsification floor (sub generates SOME
               positive expectancy even if not deployment-grade).

Falsifier:     Bear-window trade Sharpe < −0.3 AND win rate < 25%
               AND net PnL < 0 at N ≥ 5 trades.

               **Result**: Trade Sharpe **−0.74**, WR **11%**,
               net **−$56**, N=**9** trades. Falsifier fires
               on all three legs. **Rejected.**
```

## Falsifier evidence {#falsifier-evidence}

Backtest of `meta_btc_v1_loose_dist` on 2026-05-19 (cell_distribution_short with `bear_macro` gate dropped per the double-conditioning diagnosis):

| Window | Trades | Wins | Losses | Net PnL | Trade Sharpe | Win Rate |
|---|---:|---:|---:|---:|---:|---:|
| BTC 4h Bear (2022-04 → 2023-09) | **9** | 1 | 8 (SL) | **−$56** | **−0.74** | **11%** |

**Cross-meta impact**: single-handedly flipped bear-window aggregate Sharpe positive→negative:
- `meta_btc_v1` (gate locked OFF, cell_distribution didn't fire) — bear Sharpe **+0.58**
- `meta_btc_v1_loose_dist` (gate dropped, cell_distribution fires 9×) — bear Sharpe **−0.19**

The +0.77 Sharpe swing from 9 sub-trades demonstrates the sub is **actively destructive** in bear, not merely zero-edge.

## Three plausible failure-mechanisms {#failure-mechanisms}

### 1. Bear-trend continuation through stable consolidations

In sustained bears, "stable" consolidations often **resolve UPWARD** (relief rally) before resuming the downtrend. Distribution shorts get stopped out on the rally. The mechanism's expected directional resolution (down) is statistically out-of-phase with bear-window stable-phase exits (up).

This intersects with the [[../../foundation/methodology/team-stable-range-detector-methodology]] §issue-merged-stable finding — "stable" labels in bear regimes often contain merged sub-ranges where price drifted upward into a relief-rally before resuming the trend. The cell_distribution short fires at the wrong bar-state population (mid-relief-rally close > range_low, not mid-distribution close-in-range).

### 2. cell_distribution mis-fires in bear macro

`flow_sell` signal loses discriminating power when sellers dominate everywhere — the cell label that signals **"informed distribution"** in bull macro signals **"ambient bear pressure"** in bear macro. The same observable (sustained sell-flow during stable consolidation) carries different mechanism content in different macro regimes.

This is consistent with the **stable-phase mis-conditioning cross-strategy pattern** (N=4 supporting strategies from 2026-05-18/19 batch where stable-phase gating reversed productive expectations in regime-shifted windows) — currently tracked as a codification-threshold-watch item in `wiki/questions/index` §D9, not yet promoted to a standalone methodology page pending N=5+ evidence.

### 3. Wyckoff symmetry has asymmetric base rates {#asymmetric-base-rates}

This is the most **structural** explanation. Accumulation has structural long-bias tailwinds that distribution lacks symmetric mirrors of:

| Accumulation tailwind | Distribution mirror? |
|---|---|
| Retail spot accumulation (DCA buyers) | **No symmetric mirror** — retail rarely does coordinated DCA-sell |
| ETF inflows | **No symmetric mirror** — even in bear, ETF flow is net-positive on average (April 2026 strongest BTC-ETF month per substrate). *⚠️ Weakened 2026-06-11: record 13-trading-day net-outflow streak (May 15→early Jun, $4.37B) is the first sustained negative-flow episode — the tailwind is episodic-reversible, not unconditional; the asymmetry argument survives at monthly grain so far* |
| Basis carry (long-spot-short-perp arbitrage in contango) | **No symmetric mirror** — backwardation carry is rare and small |
| Miner-issuance absorption (~$15M/day at current price) | **No symmetric mirror** — miners do not absorb a symmetric supply on the upside |
| Bear capitulation cluster events | Distribution clusters are **smoother and more diffuse** — bear capitulation is event-driven (overnight, weekend, news-driven cascades) not smoothly distributional |

The mirror mechanism (distribution = composite-operator-unloads-at-top) is structurally plausible, but the **counterparty layer** (who absorbs the distributed supply) is weaker on the bear side than the counterparty layer on the bull side (who supplies the accumulated coins). Wyckoff's original framing was equities-and-NYSE 1910 — the counterparty asymmetry was less pronounced. Crypto 2026 has strong structural long-bias counterparties (ETF AP layer, treasury vehicles, miners-as-supply-floor) that don't have bear-side mirrors.

This explains why `cell_accumulation_long_btc` validates cleanly while `cell_distribution_short_btc` falsifies — same mechanism class, asymmetric counterparty base rates, asymmetric outcomes.

**Cross-link target**: [[../mechanisms/accumulation-distribution]] should carry an N=1 caveat noting that the wiki's current symmetric framing may need direction-conditional sharpening. See [[#wiki-action]] below.

## Cross-symbol pattern context {#cross-symbol-pattern-context}

N≥7 evidence that dedicated state-type bear-direction strategies systematically fail at 4h on crypto perps (across BTC + ETH + SOL, across mechanism families):

| Strategy | Symbol | Mechanism class | Outcome |
|---|---|---|---|
| `cell_chop_short_btc` | BTC | Range-internal mean-reversion | Sharpe −0.14 FAIL |
| `cell_chop_short_sol` | SOL | Range-internal mean-reversion | Sharpe −0.53 FAIL |
| `bollinger_squeeze_4h_short_only` | BTC | Vol-expansion direction-flipped | FAIL (per [[bollinger-squeeze-4h-short-regime-2026-05]]) |
| `bollinger_squeeze_4h_short_regime` | BTC | Vol-expansion regime-gated | FAIL (degenerate operating range per [[bollinger-squeeze-4h-short-regime-2026-05]]) |
| `funding_flip_recovery_4h_short_regime` | BTC | Funding-flip direction-flipped | Sharpe −0.11 FAIL (per [[funding-flip-recovery-regime-gate-2026-05]]) |
| `range_fade_poc_v1` | BTC | Range-fade auction-theory | Sharpe −0.67 REJECTED (per [[auction-theory-range-fade-2026-05]]; geometry broken) |
| `range_tunnel` short variants | ETH | Range-fade Wyckoff variant | family-rejection (per [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]]) |
| **`cell_distribution_short_btc`** (this rejection) | BTC | Wyckoff distribution direction-flipped | **Sharpe −0.74 FAIL** |

N=1 PASSING counter-example:

| Strategy | Symbol | Mechanism class | Outcome |
|---|---|---|---|
| `funding_extreme_long_short_btc` | BTC | Funding-extreme bidirectional | Sharpe 0.86 PASS — BUT **bidirectional**, not pure-bear; trades shorts at funding extremes via positioning-extreme mechanism, not bear-trend mechanism |

The N≥7 state-type failures cluster on a single mechanism-class characterization: **direction-flipping a working long-mechanism to test the short side at 4h on crypto perps systematically fails**. The candidate worldview prior in [[#worldview-prior-candidate]] frames this.

## Worldview-prior candidate (pending Phase 2 evidence) {#worldview-prior-candidate}

The N≥7 state-type bear failures cluster suggests a candidate worldview prior:

> *"In crypto perps at 4h, bear-direction tradeable edge lives in **event-type** mechanisms (cascades, failed breakouts, relief-rally exhaustion), NOT in **state-type** mechanisms (regime-conditional direction-flipping, flow-aggregator-based detection)."*

**Status**: held-pending-evidence per the research task queue. Awaits `meta_btc_v2` Phase 2 sweep results on event-bear subs (B1 wyckoff_upthrust_short, B2 turtle_soup_short, B3 htf_lower_low_continuation_short). Codification path:

- **Codify the prior** if B1 + B2 each Sharpe > 0.5 at N ≥ 15 AND B3 (state-type control) at sweep optima Sharpe < 0.3
- **Codify BROADER prior** if all 3 event-bear subs fail (bear-direction at 4h is family-rejected; reserve for capability-unlock — Coinglass + options-skew + on-chain whale-cohort)
- **Kill candidate** if state-type B3 outperforms event-type B1+B2 substantially

## What survives {#what-survives}

- **Mechanism class NOT family-rejected at N=1.** Could try `flow_balanced` variant (cell_distribution's 3A.alt — quieter distribution character) instead of `flow_sell` variant. Untested. Cheap to add to the next meta_btc sweep.
- **`cell_accumulation_long_btc_v2` is unaffected.** The Wyckoff accumulation mirror still validates cleanly — only the distribution-direction half of the symmetric mirror falsifies. Long-side Wyckoff mechanism on BTC stable phases is preserved as a working sub.
- **Event-type bear-native alternatives queued** in meta_btc_v2 (B1 Wyckoff Upthrust short, B2 Turtle Soup short, B3 HTF lower-low). These test whether bear-direction edge lives in event-type mechanisms (worldview-prior candidate above).
- **The team's stable-range data layer is not at fault here.** This rejection is mechanism-direction-asymmetry, not data-layer-pathology. Distinct from the [[../../foundation/methodology/team-stable-range-detector-methodology]] §issue-merged-stable failures where bar-level overlay rescues the strategy — here, even bar-level-anchored entries would produce wrong-direction trades by mechanism class.

## What this DOESN'T reject {#preserved-not-rejected}

- **Wyckoff distribution on BTC at OTHER timeframes.** The 4h test is one window; daily or weekly Wyckoff distribution mechanics might still work via slower-resolution counterparty dynamics. Untested.
- **Wyckoff distribution on OTHER symbols.** ETH might have stronger distribution mirror dynamics (smaller ETF-AP layer; different basis-trade structure). SOL even less likely (long-bias-on-both-directions per [[../../foundation/asset-classes/sol-market-structure]]). Cross-symbol test would clarify whether the failure is BTC-specific or 4h-specific.
- **The full meta_btc_v1 verdict.** `meta_btc_v1` itself (with `cell_distribution` gated OFF) clears bear-window Sharpe +0.58 via accumulation + chop subs. The meta's bear-window performance is rescued by removing the falsifying distribution sub, not by the meta's overall design failing.

## Mutation directions {#mutation-directions}

1. **flow_balanced variant** (highest-EV cheap test) — try `cell_distribution` with `flow_balanced` (3A.alt) instead of `flow_sell`. Test on bear window. If still negative at N≥5, mechanism class is more firmly rejected.
2. **Cross-symbol test** — same trigger on ETH bear window (Terra/3AC/FTX dates). ETH has different counterparty structure; might produce different outcome.
3. **Daily timeframe** — Wyckoff distribution at daily (not 4h) might align with slower counterparty unloading. Capability gap: distribution-cell trigger logic at 1d untested.
4. **DROP THE DIRECTION ENTIRELY on BTC.** Per N=7 state-type bear failures + worldview-prior candidate: BTC 4h bear-direction is not state-type's productive ground. Resources better spent on event-type Phase 2 (B1/B2/B3) to test whether bear-direction edge lives elsewhere.

## Methodological notes {#methodology-notes}

- **Single-handed Sharpe swing diagnostic** — when a meta's aggregate Sharpe flips sign on a 9-trade sub, the sub is the load-bearing falsification target. Future meta-batch analysis should report per-sub Sharpe-contribution-decomposition as standard discipline. Cross-link: [[../../foundation/methodology/path-quality-diagnostics]] §per-sub-contribution-pass.
- **Hypothesis docstring should anticipate Wyckoff-mirror asymmetry** — the strategy spec for `cell_distribution_short` did not flag the structural counterparty asymmetry as a falsifier candidate before backtest. The N=7 prior state-type failures pattern (per [[#cross-symbol-pattern-context]]) was visible at strategy-spec time; the spec could have pre-registered "Wyckoff-symmetric mirror falsification" as the highest-likelihood outcome.
- **Double-conditioning diagnosis matters** — v1 with `bear_macro` upstream + `cell_distribution` cell-level + `is_stable=True` bar-level was over-gated, hiding the sub. v1_loose_dist's gate-loosening was the falsifier-firing operation. Methodology: **the gate stack should be designed to fire the sub in the regime where falsification is expected**, not to suppress it. Symmetric to [[../mechanisms/regime-gate-vs-bias-filter]] §selection-bias diagnostic — over-gating hides mechanism-level failures.

## Wiki action {#wiki-action}

This rejection should be cross-linked from the source mechanism page and the BTC asset-class page:

- [[../mechanisms/accumulation-distribution]] upthrust subsection — add N=1 caveat noting BTC 4h distribution-short variant falsifies; mirror-symmetry framing may need direction-conditional sharpening per [[#asymmetric-base-rates]]
- [[../../foundation/asset-classes/btc-market-structure]] §what-fails — add the cell_distribution_short_btc failure as a state-type bear-direction failure consistent with continuation-character framing (continuation character + asymmetric Wyckoff base rates → distribution-mirror fails)

Pending **N=2 confirmation** before promoting "Wyckoff distribution has asymmetric base rates in crypto" to a mechanism-page sharpening rather than a per-rejection note. The flow_balanced variant + cross-symbol test would provide N=2 if both also fail.

## Related artifacts

- Strategy package: the backtesting lab
- Research process analysis: the research process
- Meta-strategy substrate: the research process cell 3 (deep_stable + bear) → status: **FALSIFIED 2026-05-19**
- Backtest run: the backtesting lab
- Worldview-prior candidate hold: the research task queue

## Related

- [[../mechanisms/accumulation-distribution]] — source mechanism page (Wyckoff cycle; spring + upthrust); BTC distribution-short variant falsifies here
- [[../../foundation/asset-classes/btc-market-structure]] — continuation-character codification; this rejection is consistent with continuation framing (distribution-short ≈ counter-continuation)
- [[bollinger-squeeze-4h-short-regime-2026-05]] — N=7 stack member (vol-expansion direction-flipped)
- [[funding-flip-recovery-regime-gate-2026-05]] — N=7 stack member (funding-flip direction-flipped)
- [[auction-theory-range-fade-2026-05]] — N=7 stack member (range-fade family rejection)
- [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] — N=7 stack member (Wyckoff variant on ETH)
- [[../mechanisms/regime-gate-vs-bias-filter]] — selection-bias diagnostic; over-gating hides mechanism-level failures (symmetric to the v1 double-conditioning issue here)
- [[../../foundation/methodology/path-quality-diagnostics]] — per-sub Sharpe-contribution-decomposition is the diagnostic that surfaced the single-handed swing
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization codification anchor (BTC ↔ ETH/SOL Wyckoff mirror divergence)

## Sources

- Backtest run: the backtesting lab{summary.json, trades.csv, equity.csv}`
- Research process analysis: the research process
- Cross-mechanism pattern context: substrate tally from rejected/ inventory + funding_extreme as N=1 counter-example
- Worldview-prior candidate framing: the research task queue
