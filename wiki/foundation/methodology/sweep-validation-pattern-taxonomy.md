---
title: "Sweep-Validation Pattern Taxonomy — reading a parameter sweep's train-val structure"
status: growing
created: 2026-06-08
updated: 2026-06-11 (added §bounds-buffer — grid-narrowing discipline after a per-axis diagnostic)
tags: [foundation, methodology, validation, parameter-sweep, overfitting, selection, train-val, DSR, day-swing]
---

# Sweep-Validation Pattern Taxonomy

> **TL;DR**: A parameter sweep can show **strongly negative train-val correlation while ALSO having high both-positive % and high DSR** — and that combination is **robust edge with overfit-prone *selection***, NOT "no edge." The naive reading ("negative train-val correlation ⇒ overfit ⇒ reject") conflates two distinct failure modes and wrongly throws away a deployable strategy. The fix is to read three numbers jointly — train-val correlation, both-positive %, and the Sharpe distribution / DSR — against a four-pattern table, and to **select configs by Sharpe (or a both-positive + combined-PnL filter), not by train-PnL rank**, whenever both-positive % is high. Train-PnL ranking under negative correlation actively *anti-selects* (picks the overfit tail). Canonical evidence: the regime_v3 sweep (6561 configs) — train-val corr −0.593 yet 81.8% both-positive and DSR 87.7%; ranking by train PnL put the *worst* validation cohort on top.

## Why this page exists

The lab's sweep-validation step needs a single, reusable rule for "does this sweep validate?" The previous heuristic — *"train-val correlation > 0.3 = transferable edge, else reject"* — is too blunt. It correctly rejects pure overfit (corr ~0, both-positive ~0%) but **incorrectly rejects** a robust edge whose *optimization surface* is just adversarial to train-PnL ranking. The two look identical on the correlation axis alone; they separate cleanly once both-positive % and DSR are read alongside.

This is a **generalizable** contribution: it applies to *any* parameter sweep with a train/validation split, independent of the (now-concluded) regime discovery track it was surfaced on. It is the selection-step companion to the statistical-significance machinery in [[strategy-validation-discipline#8-psr-and-dsr-adjusting-the-sharpe-ratio|strategy-validation-discipline §8 (PSR/DSR)]] and the small-N operational thresholds in [[small-sample-validation-discipline]].

## The core insight {#core-insight}

Train-val correlation answers *"does ranking configs by train performance preserve their validation ranking?"* — a question about the **selection method**, not about whether edge exists. Both-positive % and the validation-Sharpe distribution answer *"does edge exist across the parameter region at all?"* — the question that actually gates deployment.

When edge is **robust** (most configs work in both windows) but the optimization surface is **noisy/non-monotone** (the single best train config is a fluke), you get the seemingly-paradoxical signature: **negative train-val correlation + high both-positive %**. Edge is everywhere; train-PnL just isn't the coordinate that finds the best of it. The mistake is reading the negative correlation as "no edge" when the both-positive % already proved edge exists — the only thing broken is the *selection rule*.

## Four-pattern taxonomy (the durable contribution) {#taxonomy}

| Pattern | Train-val corr | Both-positive % | Sharpe dist | DSR | Verdict |
|---|---|---|---|---|---|
| **No edge** | ~0 | <1% | wide, mean ≤ 0 | <10% | Fails; stop. (worked example: regime_v2) |
| **Overfit-only edge** | <0.3 | 5–30% | concentrated, mean < threshold | <30% | Edge only at specific configs; won't generalize |
| **Robust edge, overfit-prone optimization** | **strongly negative** | **>50%** | tight, mean > threshold | >50% | **Validates**; *selection method* matters more than param values (worked example: regime_v3) |
| **Robust edge** | >0.3 | >30% | tight, mean > threshold | >50% | Validates; train-rank selection works |

The **third row is the new one.** "Negative correlation = bad" is wrong when the means are positive and both-positive % is high. The first and third rows are *both* negative/low-correlation but are opposite verdicts — the discriminator is both-positive % (and DSR), never correlation alone.

## Recommended sweep-validation pass criteria (replaces the bare correlation gate) {#pass-criteria}

```
PASS if ALL of:
- Best Sharpe          > deployment threshold (1.5)
- DSR                  > 50%   (multiple-testing adjusted; per §8.3)
- BOTH-positive %      > 30%   (train > 0 AND val > 0)
- Top-quantile validation > 0
```

**Selection rule that goes with it**: if both-positive % is high but train-val correlation is negative → **do NOT falsify.** Select the deployable config by **Sharpe**, or by a *both-positive filter then combined-PnL rank* — **never** by train-PnL rank, which under negative correlation picks the overfit tail.

The DSR gate (not raw best-Sharpe) is what keeps this honest: a high best-Sharpe found across 6561 configs is exactly the multiple-testing trap DSR exists to deflate. Both-positive % proves *breadth* of edge; DSR proves the *best* config isn't a selection artifact. They are complementary, not redundant.

## Bounds-narrowing after a per-axis diagnostic — leave ≥1 grid-step buffer past the inflection {#bounds-buffer}

A sweep-iteration companion to the selection rule above: when a per-axis diagnostic (per [[backtest-analysis-diagnostics]]) locates the parameter value where an axis flips from predictive to anti-predictive, and the next sweep's bounds are narrowed around the productive side, **the new bound must sit ≥1 grid-step PAST the observed inflection, not AT it.**

The reason is a resolution limit, not a heuristic: the diagnostic only tests discrete grid values, so it locates the inflection **between two tested values**, never exactly. Narrowing TO the last-good observed value still includes the uncertain interval — and part of the new grid lands in the anti-predictive zone.

**Worked example (ETH v5→v6, 2026-05-11):** v5 per-stop diagnostic showed corr +0.81 at `vt.stop ≥ 0.321` and corr −1.000 at `≤ 0.279` — inflection somewhere in (0.279, 0.321). The v6 production bounds were set to [0.28, 0.40] — lower bound right at the suspected inflection. Result: 2 of 8 stop levels (0.28, 0.297) were still in the anti-predictive zone, dragging the cohort validation mean to −$9 — which *looks* like sweep failure although the top region was clean. Buffered bounds ([0.31, 0.40]) would have avoided the cohort-level bifurcation entirely. The narrowing was directionally correct but off by one grid step.

**The failure mode this prevents**: a structurally sound strategy reads as a failed sweep because the bounds re-imported the known-bad edge — a *bounds* artifact masquerading as an *edge* verdict, the same genre of misreading as Pattern-3 above (where a *selection* artifact masquerades as no-edge).

*Open question*: the ≥1-step rule assumes the grid step is small relative to the productive region; for coarse grids the right buffer may be a fraction of the inter-value gap rather than a full step — unresolved at N=1.

## Relationship to the prior "negative train-val correlation = overfit" reading {#refines-prior}

This taxonomy **refines, and partially corrects**, the framing carried in [[strategy-validation-discipline#93-walk-forward-critique|strategy-validation-discipline §9.3]] (walk-forward critique) and its [[strategy-validation-discipline#12-concrete-linkage-to-wiki-failures|§12 linkage table]] — the *"May 4 train→val correlation negative for 7/8 strategies"* row, which read negative correlation as an overfit signature.

Per the wiki architecture (ARCHITECTURE.md) Knowledge Evolution Protocol, the contradiction is named and resolved rather than left implicit:

- **Not a reversal — a disambiguation.** The May-4 strategies were negative-correlation **with low both-positive %** (Pattern 1 / 2 — genuine non-transfer). The regime_v3 case is negative-correlation **with high both-positive %** (Pattern 3 — robust edge, overfit-prone selection). The earlier finding was correct *for its cases*; it over-generalized the correlation axis into a standalone falsifier.
- **Resolution**: negative train-val correlation is **ambiguous on its own**. It is a falsifier *only when paired with low both-positive % and low DSR*. Read the three numbers jointly; correlation is never the sole gate.

## Evidence (N=1, regime_v3 sweep 2026-05-21) {#evidence}

6561 configs, 70/30 split:
- Best Sharpe **1.638**, DSR **87.7%**, train-val corr **−0.593**, **81.8%** of configs positive in both windows.
- Ranking by train PnL anti-selects: top-train Q5 avg val **$12**; bottom-train Q1 avg val **$231**. The configs that looked best in-sample were the worst out-of-sample — the textbook overfit tail — yet edge was present in >80% of the parameter space.

This is a single worked example. The recurrence path is dead (the discovery/sweep workflow it arose on is concluded), so the taxonomy is landed at N=1 as **generalizable methodology**, not promoted to a worldview prior — its value is as a reusable decision table for any *future* sweep, not as a claim about this track.

## Key Takeaways

- **Read three numbers, not one**: train-val correlation, both-positive %, and DSR. Correlation alone cannot distinguish "no edge" from "robust edge, wrong selection rule" — both-positive % and DSR are what separate them.
- **Negative train-val correlation is ambiguous**, not damning. With high both-positive % + high DSR it means *robust edge, overfit-prone optimization* — select by Sharpe, deploy.
- **Drop the standalone correlation>0.3 gate.** Replace with the four-criterion PASS block (best Sharpe > 1.5, DSR > 50%, both-positive > 30%, top-quantile val > 0).
- **Never rank by train PnL under negative correlation** — it actively anti-selects the overfit tail.
- **When narrowing bounds after a per-axis diagnostic, leave ≥1 grid-step buffer past the observed inflection** — the diagnostic locates the flip *between* two tested values; narrowing AT the last-good value re-imports the anti-predictive edge and can make a sound strategy read as a failed sweep.

## Related

- [[strategy-validation-discipline]] — the LdP synthesis this sharpens; its §8 (PSR/DSR) supplies the DSR gate; §9.3 + §12 carry the prior "negative corr = overfit" reading this page disambiguates
- [[small-sample-validation-discipline]] — operational PSR thresholds at small N; the sibling lab-pipeline discipline (this page is its sweep-selection counterpart)
- [[backtest-analysis-diagnostics]] — supplies the per-axis correlation diagnostic that §bounds-buffer's narrowing rule consumes
- [[path-quality-diagnostics]] — the *other* "good aggregate metric hides a problem" lens: path-quality reads P&L concentration; this page reads train-val selection structure
- [[deployment-edge-threshold]] — the economic-significance gate that sits downstream of a passing sweep

## Sources

- regime_v3 sweep (2026-05-21): 6561 configs, 70/30 split; DSR 87.7%, train-val corr −0.593, 81.8% both-positive. Research process self-improvement task `2026-05-21-selection-methodology-anti-correlation-pattern` (preserved here at track-conclusion).
- ETH v5→v6 bounds off-by-one (2026-05-11): per-stop diagnostic corr +0.81 at ≥0.321 / −1.000 at ≤0.279; v6 bounds [0.28, 0.40] re-imported the anti-predictive edge (2/8 levels; cohort val mean −$9, top region clean). Research process wiki task `2026-05-11-v6-bounds-safety-buffer` (held N=1; landed here 2026-06-11 at track-conclusion — same preservation rationale as the regime_v3 entry above).
- López de Prado, *Advances in Financial Machine Learning*, Ch. 14 (PSR/DSR) — the multiple-testing adjustment behind the DSR gate.
