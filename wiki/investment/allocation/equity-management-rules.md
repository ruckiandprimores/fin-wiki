---
title: "Equity Management Rules — Risk Sizing and Expectancy"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [allocation, risk-management, position-sizing, martinez, day-swing-applicable]
---

# Equity Management Rules — Risk Sizing and Expectancy

> **TL;DR**: The mechanical risk-management framework the wiki was missing. Three core rules: (1) risk per trade ≤ 3-5% of trading capital, (2) minimum 1:1 risk:reward (target 1:1.5), (3) win rate is meaningless without expectancy — a 30% win rate at 6:1 R:R is profitable; a 70% win rate at 1:2 R:R is not. Combined, these rules produce *staying power* — the ability to absorb losing streaks without account destruction. Translates directly to crypto perps where the same math operates with even larger amplification.

## Why This Page Exists

The [[foundation/relations-and-themes|relations-and-themes page]] flagged "risk management / position sizing" as a wiki gap — no source had been ingested for it. The Martinez book ([[foundation/sources/forex-10-essentials]]) provides the most concrete rules among the ingested sources, even though the book is otherwise a tactical retail-FX text rather than a foundational source.

This page extracts the mechanical rules and combines them with rigor from the broader wiki framework. It applies to both day/swing trading (where it's most directly load-bearing) and long-horizon investing (where the same math applies at lower frequency).

## Rule 1 — Risk Per Trade ≤ 3-5% of Capital

The **single most important** risk-management rule. Determines how many losses your account can absorb before exhaustion.

| Risk per trade | Losses before account is halved | Losses before account is exhausted |
|----------------|--------------------------------|-------------------------------------|
| 1% | ~70 trades | ~460 trades |
| 2% | ~35 trades | ~230 trades |
| 5% | ~14 trades | ~90 trades |
| 10% | ~7 trades | ~44 trades |
| 25% | ~3 trades | ~16 trades |
| 50% | ~1 trade | ~4 trades |

The math: with R% risk per trade, the account multiplier per loss is (1 − R%). Cumulative losses compound geometrically; the table above is `log(0.5)/log(1−R)` for halving and `log(small)/log(1−R)` for exhaustion.

**Why 3-5% is the practical maximum:**
- Below 1%: losing streak math is fine, but capital efficiency is too low for meaningful returns
- At 5%: still gives 14+ losing trades before halving the account — survivable for typical strategies
- Above 10%: a normal losing streak (5-7 trades, common for any strategy) wipes the account in half

Martinez's specific recommendation: "between 2% and 5%" depending on strategy and risk tolerance. The *Turtle Traders* (Dennis/Eckhardt 1980s) used 2% per trade. Most professional traders converge in the 1-3% range.

## Rule 2 — Minimum 1:1 Risk:Reward, Target 1:1.5

For every dollar you can lose, you must be able to make at least one dollar (1:1) and ideally 1.5 dollars.

> "The risk must, at minimum, equal the reward and, in most cases, must be far less than the reward. The rule for the risk versus reward must be a minimum of 1:1. I say minimum because your habit of success must be 1:1.5."

The mechanism:
- At 1:1 R:R, you need a >50% win rate to be profitable
- At 1:1.5 R:R, you need a >40% win rate
- At 1:2 R:R, you need a >33% win rate
- At 1:3 R:R, you need a >25% win rate

Each step in R:R reduces the win-rate burden. This is why systematic traders prefer asymmetric setups — the win rate doesn't have to be high if the wins are bigger than the losses.

**Operational implementation**: before entering a trade, identify the stop-loss and take-profit *levels* (per [[trading/mechanisms/support-resistance]]). The distance from entry to stop = risk; distance from entry to take-profit = reward. If reward/risk < 1.5, **do not take the trade**, regardless of how attractive the setup looks.

This is what enforces discipline against marginal trades.

## Rule 3 — Win Rate Is Meaningless; Expectancy Is Everything

> "Percentages mean nothing in trading. For example, you can be wrong 70 percent of the time and still make lots of money: Seven losses out of 10 at $400 apiece is $2,800. Three gains at $1,500 apiece equals $4,500 in profit. Subtract the losses from the gains and you have a net gain of $1,700 being wrong 70 percent of the time."

The framework:

```
Expectancy per trade = (Win rate × Average win) − (Loss rate × Average loss)

Profitable iff: Expectancy > 0
```

Concrete examples:

| Strategy | Win rate | Avg win | Avg loss | Expectancy per trade |
|----------|----------|---------|----------|----------------------|
| "Always profit-taking early" | 80% | $300 | $1,500 | (0.8 × 300) − (0.2 × 1,500) = −$60 |
| "Cut losses, let winners run" | 40% | $1,500 | $400 | (0.4 × 1,500) − (0.6 × 400) = +$360 |
| "Lottery ticket strategy" | 5% | $50,000 | $1,000 | (0.05 × 50,000) − (0.95 × 1,000) = +$1,550 |
| "Casino strategy" | 95% | $100 | $5,000 | (0.95 × 100) − (0.05 × 5,000) = −$155 |

The first row is the **most common amateur failure mode**: high win rate from early profit-taking, but the rare losses (the runaway losers held in hope) destroy the account. The trader *feels* like a winner because they win most days but the math is negative.

The second row is the textbook **trend-following profile**: low win rate but large winners. Most successful systematic strategies fit this profile.

The third row is the **option-buying / lottery profile**: very low win rate, very high payoff. Mathematically positive but psychologically very hard to execute (95% loss rate).

The fourth row is the **picking-up-pennies-in-front-of-a-steamroller profile**: feels safe (95% win rate) but one large loss destroys many small wins. Common in negative-skew strategies (selling options without hedge, mean-reversion in trending markets).

**Operational implementation**: track expectancy, not win rate. After 50+ trades of a strategy, you have a real expectancy estimate — use it for position sizing.

## The "Staying Power" Concept

> "The reason you need to risk only a certain percentage of your trading margin is so that you have staying power in the market."

A trading strategy is a *probabilistic process*. Even a positive-expectancy strategy will have losing streaks. The question isn't *whether* you'll have a losing streak — it's *whether you can survive it*.

Staying power is also a *behavioral* prerequisite, not just a mathematical one. Sizing is upstream of the holder's mental frame: a position sized such that a normal drawdown threatens lifestyle or sleep collapses any pre-committed long-horizon frame on the first uncomfortable night. The [[foundation/methodology/behavioral-structure-design|behavioral-structure design discipline]] makes this explicit — feature-design becomes load-bearing only *after* sizing is right.

| Strategy win rate | 95% probability you'll see this losing streak |
|-------------------|------------------------------------------------|
| 60% | 5 consecutive losses |
| 50% | 7 consecutive losses |
| 40% | 9 consecutive losses |
| 30% | 12 consecutive losses |

A 40%-win-rate trend-following strategy will, with 95% probability, see a 9-loss streak somewhere in your trading career. If you risk 10% per trade, that streak draws down 60%+ of your account; you may quit before recovery. If you risk 2% per trade, the same streak draws down ~17% — survivable.

**Position sizing is not about each trade's outcome — it's about whether you'll still be trading after a normal losing streak.**

## Combining the Rules

The three rules combine into a sizing framework:

```
1. Pick max-risk-per-trade (e.g., 2%)
2. Identify entry, stop-loss, and take-profit (must satisfy R:R ≥ 1.5)
3. Position size = (account × max-risk) / (entry − stop-loss distance)
4. Track expectancy over 50+ trades
5. Only scale up size when expectancy is verified positive over a meaningful sample
```

Example for a crypto perp trader:
- Account: $10,000
- Max risk: 2% = $200 per trade
- Setup: long BTC at $50,000, stop at $48,500 (3% stop), target at $54,500 (9% target = 3:1 R:R)
- Position size: $200 / ($50,000 − $48,500) = $200 / $1,500 = 0.133 BTC = $6,650 notional
- At 10x leverage on a $6,650 notional position, margin requirement = $665, well within the account

The trader can take many such setups without account-threatening drawdown.

## Crypto-Specific Adjustments

Crypto markets have features that affect equity management:

1. **Higher volatility than FX** — even at 2% risk, the *price* moves required to hit stops are larger. Position sizing must account for the wider expected stop distance.
2. **Liquidation cascades** — perp leverage means stops can be jumped over during cascades. Use slightly tighter stops than the technical level + tail buffer.
3. **24/7 markets** — losing streaks can play out faster than in equities (no overnight reflection time). Discipline must be even tighter.
4. **Funding costs on perps** — long-held perp positions accumulate funding costs that erode the take-profit. Account for funding in R:R calculations for multi-day holds.
5. **Wider spreads on alts** — the bid/ask spread on lower-cap alts can be 0.1-1%, making short-distance R:R setups unprofitable after costs.

Practical adjustments from FX → crypto:
- 1-3% risk per trade (vs. 2-5% in FX) due to higher volatility
- Minimum R:R 1:2 instead of 1:1.5 (to absorb wider spreads + funding)
- Account for the possibility of a 5-loss streak in any single day (24/7 + frequent setups)

## What This Page Doesn't Cover (Future Wiki Gaps)

This is a **tactical** risk management page. The wiki still needs:
- **Portfolio-level risk** — correlation across positions; what happens when all your positions move together
- **Systematic strategy risk** — overfitting, regime change, parameter drift
- **Tail-risk management** — Taleb-style barbell strategies, optionality
- **Stress testing** — what happens in 50% one-day BTC drawdowns? In stablecoin de-pegs?

These gaps remain for future ingests. The Taleb sources (priority #6 in [[foundation/sources/lineage]]) would address the tail-risk side directly.

## Hypothesis (Structured Format)

```
Hypothesis:    Crypto perp traders applying the 3-rule equity management
               framework (≤2% risk per trade, ≥1:1.5 R:R, expectancy
               tracking) avoid >80% drawdowns over multi-cycle horizons,
               compared to undisciplined sizing which produces frequent
               account-destroying drawdowns.

Mechanism:     The 2% risk cap mathematically bounds drawdown given any
               losing-streak length. The R:R floor ensures positive
               expectancy doesn't require improbably-high win rates.
               Expectancy tracking surfaces strategy decay before
               catastrophic loss.

Observable:    Per trader: account drawdown over time, distribution of
               losses, longest losing streak.

Expected:      Disciplined traders show max drawdown of 20-40% across
               multiple cycles vs. 80-100% common for undisciplined
               traders.

Falsifier:     If disciplined traders show drawdowns indistinguishable
               from undisciplined ones, OR if discipline produces
               negative absolute returns over multi-cycle test, the
               framework is not adding value.
```

## Key Takeaways

- Risk per trade ≤ 3-5% of capital; smaller is safer; below 1% is too capital-inefficient
- Minimum 1:1 R:R, target 1:1.5+ — never enter trades where the asymmetry is wrong
- Win rate is meaningless; expectancy is everything (a 30% win rate at 6:1 R:R beats an 80% win rate at 1:5 R:R)
- The first amateur failure mode: high win rate via early profit-taking + rare runaway losses
- Position sizing is about surviving losing streaks, not optimizing single trades
- Crypto requires tighter discipline than FX due to higher volatility, liquidation risk, funding costs, wider spreads on alts

## Wizards-Style Sizing Refinements (Schwager 2026-05-04 ingest)

The basic 3-5% per-trade rule above is the Martinez baseline. [[foundation/sources/schwager-market-wizards|Schwager's wizards]] articulate refinements that compose with this rule:

### Volatility-Adjusted Position Sizing (Hite, Basso)

> Position size = MIN(risk-budget-size, volatility-budget-size). Risk-budget = capital × max-loss-% / stop-distance. Volatility-budget = capital × volatility-% / asset-volatility. The constrained variable wins.

In high-vol regimes (post-news, weekend wicks, 100%+ IV periods), the vol-budget binds and shrinks position automatically. In compressed regimes, the risk-budget binds. **Particularly important on crypto alts where realized vol can 5x in days** — pure stop-distance sizing leads to over-sizing during vol contractions.

Operationalization for crypto: position size for BTC/ETH perps = MIN(1% capital / stop-distance, 0.5% capital / 30-day-realized-vol). The vol-budget is meaningfully tighter than risk-budget alone in crypto.

### Triple-Confirmation 5x Sizing (Marcus)

> Trade only when fundamentals + technicals + market tone all align. When all three confirm, size 5-6× normal; when fewer confirm, size normal-or-flat.

This is the *binary* version of the more-graduated [[investment/allocation/conviction-tiered-sizing|Druckenmiller conviction tiering]]. Marcus's three-filter rule maps to crypto as: (Fundamentals = on-chain net-flow direction; Technicals = primary trend regime + breakout setup; Market tone = funding rate / OI / sentiment).

### Reverse-Pyramid Sizing (PTJ + Schwartz)

> Increase size during winning streaks; decrease size after extended winning streaks. Worst sizing is martingale (size up while losing) — both wizards explicitly forbid.

Trader's-own-equity-curve as risk regulator. After hitting a new equity high + 3 consecutive winning weeks, halve position size as drawdown insurance (Schwartz's rule). After a drawdown, reduce size until performance stabilizes (PTJ's rule).

### Volatility Circuit-Breaker (Hite)

> When a market's volatility regime shifts to where expected return / risk no longer justifies trading, *suspend* trading that market until volatility normalizes. Product-selection-as-risk-management.

For crypto: mean-reversion strategies on BTC perps suspend during top-decile realized-vol weeks (typically: macro-event weeks, post-FTX-style events). Resume when realized-vol falls back below the 80th percentile.

### Conviction-Tiered Sizing (Druckenmiller)

The meta-mechanism: which trades to size up on. Full structured page at [[investment/allocation/conviction-tiered-sizing]] — the framework decides *across* trades, while this page (equity-management) decides *per* trade. They compose: equity-management's per-trade rules apply *within* each Druckenmiller tier.

## Related

- [[foundation/sources/forex-10-essentials]] — primary source for the baseline 3-5% rule
- [[foundation/sources/schwager-market-wizards]] — source for the wizards-style refinements above (Hite, Basso, Marcus, PTJ, Schwartz, Druckenmiller)
- [[investment/allocation/conviction-tiered-sizing]] — Druckenmiller's meta-mechanism for *which trades* deserve outsized size
- [[foundation/market-wisdom/universal-wizards-lessons]] — Lesson 1 (Risk Management) operationalized here
- [[foundation/relations-and-themes]] — this page fills the "risk management" gap flagged there
- [[trading/mechanisms/support-resistance]] — defines stop-loss and take-profit levels for R:R calculation
- [[trading/mechanisms/cyclical-timing]] — even high-conviction cyclical entries need equity-management discipline
- [[foundation/market-wisdom/modern-finance-theory-critique]] — Buffett's "concentration of mind" complements this; concentration without sizing discipline is destructive
- [[foundation/market-structure/crypto-carrier-features]] — central catalog; vol-regime carriers (realized vol, funding extremes) inform sizing decisions
- [[foundation/methodology/behavioral-structure-design]] — design-time discipline within which these per-trade rules sit; articulates sizing-as-upstream-of-frame as the load-bearing principle for holding behavior

## Open Questions

- **What's the empirically-best risk-per-trade for crypto perps specifically?** Hypothesis: 1-2% given crypto's losing-streak distribution.
- **Does adaptive sizing (Kelly Criterion, fractional Kelly) outperform fixed 2% in crypto?** Theoretically yes; in practice, Kelly assumes accurate edge estimation, which is the bottleneck.
- **How should equity management adapt during regime changes?** When a strategy's expectancy decays (e.g., trend-following in regime-changing markets), should sizing scale down automatically?

## Sources

- Martinez, Jared F. *The 10 Essentials of Forex Trading*, Chapter 12 ("Learning the Rules of Equity Management"), 2007.
- Concept of expectancy: Van Tharp, *Trade Your Way to Financial Freedom* (referenced; not yet in wiki).
- Concept of fixed-fractional sizing: derived from Kelly (1956); Vince *Mathematics of Money Management* (referenced; not yet in wiki).
