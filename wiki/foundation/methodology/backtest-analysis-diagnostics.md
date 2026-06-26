---
title: "Backtest-Analysis Diagnostics — decomposition, asymmetric-N overlap, ablation baseline"
status: growing
created: 2026-06-08
updated: 2026-06-08
tags: [foundation, methodology, backtest-analysis, decomposition, ablation, diagnostics, day-swing]
---

# Backtest-Analysis Diagnostics

> **TL;DR**: Three reusable backtest-interpretation diagnostics, each correcting a way that **aggregate metrics mislead**. (1) **Exit-reason × win/loss × side decomposition** — when aggregate Sharpe is borderline [0.3, 0.7] and the time-stop fires a lot, the per-exit-reason table distinguishes "mechanism doesn't work" from "mechanism works but the exit config clips it." (2) **Asymmetric-N overlap** — when one strategy's entries are a strict subset of another's, per-bar Pearson reads near-zero even for same-family pairs; use co-occurrence overlap instead. (3) **Same-side ablation baseline** — a direction-restriction ablation must be compared to the parent's *same-side subset*, not the parent aggregate, because bidirectional position-management can be an inadvertent quality filter. All three are general backtest hygiene, reusable in any future interpretation.

## Why this page exists

These are **backtest-reading** diagnostics, not strategy mechanisms — the layer between a raw `summary.json` and a verdict. They were surfaced as N=1 worked examples during the (now-concluded) mutation/discovery track, but each generalizes to any future backtest interpretation, so they are preserved here independent of that workflow. They complement the *statistical*-significance gate ([[strategy-validation-discipline]]), the *path-quality* gate ([[path-quality-diagnostics]]), and the *selection* gate ([[sweep-validation-pattern-taxonomy]]): those tell you whether a result is real / well-distributed / well-selected; these tell you **why** a borderline result reads the way it does and **what to mutate next**.

## 1. Exit-reason × win/loss × side decomposition (borderline-Sharpe attribution) {#exit-reason-decomposition}

**When**: aggregate Sharpe in **[0.3, 0.7]** AND time-stop fire rate **> 50%**. Aggregate metrics here can't separate "mechanism doesn't produce the predicted character" from "mechanism works but the exit config clips it before the move completes."

**The diagnostic** — split every trade by exit reason × win/loss × side:

| | TP | Time-stop | SL |
|---|---|---|---|
| **Long** | N, wins, net, avg | N, wins, net, avg | N, wins, net, avg |
| **Short** | … | … | … |

Read the **time-stop bucket's win rate**:
- **Time-stop WR ≥ 70%** → the mechanism produces its predicted character; the exit is clipping it mid-move → **mutate the exit config, not the entry.**
- **Time-stop WR ≈ 50%** → random walk during the hold → no exit mutation rescues it → **reject.**

**Worked example**: `range_tunnel_bidirectional_eth_wyckoff_test` v1 — aggregate Sharpe 0.40 / PSR 19.6% reads as "noise," but the time-stop bucket was **75–76% WR** (mechanism real; the 12h horizon was clipping it).

**Caveat — this is step 1 of a two-step chain.** If a follow-up exit-config mutation passes its sub-falsifiers but the *leg-level* falsifier still fails, the mechanism has **edge-saturation** (K-bar drift then mean-reversion), not exit-clipping. Do **not** codify exit-reason decomposition as a standalone verdict — it scopes the next mutation, it doesn't close the verdict.

## 2. Asymmetric-N overlap (the scaffold-as-mechanism Pearson exception) {#asymmetric-n-overlap}

**When**: comparing two strategies' timing relation where the trade counts are lopsided — **N ratio > 2× or < 0.5×** (one strategy's entries are roughly a strict subset of the other's).

**The problem**: the ≥+0.6 per-bar Pearson rule in [[../../trading/mechanisms/scaffold-as-mechanism]] **understates** same-trigger-family relation under asymmetric N — the all-zero bars (both strategies flat) dominate the Pearson and pull it toward zero, so a genuine same-family pair reads "different mechanism."

**The fix**: compute **BOTH** per-bar Pearson **AND** co-occurrence overlap within **±N bars**. The load-bearing metric is:
- **overlap** for asymmetric-N pairs,
- **Pearson** for symmetric-N pairs.

**Worked example**: `range_tunnel` (N=65) vs `failed_breakout_phase_fade_eth` (N=166) — same-direction co-occurrence **77–80%** but Pearson **0.013–0.031**. Pearson alone would misclassify them as independent mechanisms; the overlap reveals the shared trigger family. This is the sharpening landed on [[../../trading/mechanisms/scaffold-as-mechanism#threshold-framework|scaffold-as-mechanism §threshold-framework]].

## 3. Same-side ablation baseline (direction-restriction comparisons) {#same-side-ablation}

**When**: evaluating a direction-restriction ablation (e.g. dropping bidirectional position-management to test a single-leg version).

**The trap**: comparing the single-leg ablation to the **parent aggregate** is the wrong baseline. Bidirectional position-management (no new signal while an opposite position is open) can be an inadvertent **quality filter** — ablating it surfaces previously-blocked trades that may be net-losing. The correct baseline is the parent's **same-side subset standalone**.

**The diagnostic before ablating**: compute the parent's **per-side standalone Sharpe**. If the same-side subset materially beats the parent aggregate, the bidirectional architecture has filter value — and a naive ablation will look like it "added trades" while quietly degrading per-trade economics.

**Worked example**: `failed_breakout_phase_fade_eth` — parent short-subset standalone Sharpe **+0.91**; single-leg ablation **+0.49**; the 13 newly-unblocked shorts contributed **−$4.60/trade**. (SOL same check: position-blocking was value-neutral — the filter value is **symbol-character-dependent**, so run the per-side baseline per symbol, don't assume.)

## Key Takeaways

- **Borderline Sharpe + high time-stop rate → decompose by exit reason × win/loss × side.** Time-stop WR ≥ 70% = mutate the exit; ≈ 50% = reject. But it's step 1 of a chain, not a verdict.
- **Asymmetric trade counts break per-bar Pearson** — switch to ±N-bar co-occurrence overlap; Pearson is for symmetric-N pairs only.
- **Direction-restriction ablations need the parent's same-side subset as baseline**, not the aggregate — bidirectional position-management can be a hidden quality filter, and its value is symbol-dependent.
- All three correct the same failure: **an aggregate number hiding the structure that determines the next action.**

## Related

- [[../../trading/mechanisms/scaffold-as-mechanism]] — diagnostic #2 is the asymmetric-N exception to its per-bar Pearson threshold
- [[path-quality-diagnostics]] — sibling "aggregate metric hides structure" lens (P&L concentration vs these interpretation diagnostics)
- [[sweep-validation-pattern-taxonomy]] — sibling sweep-selection diagnostic (train-val structure)
- [[strategy-validation-discipline]] — the statistical-significance gate these feed
- [[small-sample-validation-discipline]] — PSR/CI context for the borderline-Sharpe cases #1 addresses
- [[../../trading/mechanisms/regime-gate-vs-bias-filter]] — direction-restriction operator context for #3

## Sources

- Worked examples preserved from research process self-improvement task `2026-05-19-exit-config-binding-diagnostic` (range_tunnel + failed_breakout_phase_fade arc, 2026-05-18/19; all N=1) at track-conclusion.
- The other three signals from that task (edge-saturation in Wyckoff-to-4h, premature-family-rejection framing, compound-axis-orthogonality) were tied to the concluded mutation-cycle workflow and are not generalized here.
