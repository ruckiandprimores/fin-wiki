---
title: "Value vs. Growth — A False Dichotomy"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [framework, valuation, buffett, resolves-contradiction]
---

# Value vs. Growth — A False Dichotomy

> **TL;DR**: Buffett's framing dissolves the apparent Graham-vs-Lynch contradiction (flagged in the [[log|2026-04-29 lint]]). Growth and value are not separate strategies — growth is *one input* into the value calculation. Calling something "value investing" is a redundancy ("all investing is investing in something whose price is below its value") and calling something "growth investing" without a value framework is just speculation. The right question is never "growth or value" but *"is this growth correctly priced relative to what it produces?"*

## The Problem

The wiki has carried this tension since the [[foundation/sources/one-up-on-wall-street|Lynch ingest]]:

- Graham (in [[investment/allocation/enterprising-investor]]): "Obvious prospects for physical growth do not translate into obvious profits for investors" — the airline-stocks example. Growth investing is treated as a category to avoid.
- Lynch (in [[foundation/market-wisdom/lynch-six-categories]]): Fast growers are his favorite category. Tenbaggers, 40-baggers, even 200-baggers come from there.

The 2026-04-29 lint flagged this as a productive contradiction with a partial resolution: Graham warns against paying for *priced-in* growth; Lynch's PEG-style discipline avoids that. But the framing was still "two strategies that overlap when applied carefully."

Buffett's framing (1992 letter, repeated throughout the essays) **dissolves the dichotomy entirely** rather than carefully managing it.

## Buffett's Framing

> "Most analysts feel they must choose between two approaches customarily thought to be in opposition: 'value' and 'growth.' Indeed, many investment professionals see any mixing of the two terms as a form of intellectual cross-dressing.
>
> We view that as fuzzy thinking... In our opinion, the two approaches are joined at the hip: Growth is *always a component in the calculation of value*, constituting a variable whose importance can range from negligible to enormous and whose impact can be negative as well as positive."
> — Buffett, *1992 letter*

Three claims in the quote:

1. **Growth is a variable in the value calculation, not a separate strategy.**
2. **Growth's contribution to value ranges from negligible to enormous.**
3. **Growth can subtract value as well as add it.**

The third point is the underappreciated one. Growth is value-positive only if it earns returns above the cost of capital. Growth that earns less than the cost of capital is value-*destructive* — the company is shrinking owners' real wealth even as the headline numbers grow.

## The Math

For a business with sustainable competitive advantage:

```
Value = Owner earnings × (1 + g) / (r − g)     [Gordon growth model]

where:
  g = sustainable growth rate
  r = required return on capital
```

Growth (g) appears twice:
- In the numerator (raises distributable cash next period)
- In the denominator (compresses the discount factor — but only when r > g)

**The hidden assumption**: each unit of growth requires retained earnings. The *retained* earnings could have been distributed instead. So:

- If the business reinvests at returns > r → growth adds value
- If the business reinvests at returns ≈ r → growth is neutral (same value as if it had distributed)
- If the business reinvests at returns < r → growth *subtracts* value (owners would prefer the dividend)

This is the formal version of Graham's airline warning: airline industry capex earned below cost of capital, so airline growth was value-destructive even as the industry grew.

## Resolving the Graham-Lynch Tension

With Buffett's framing in hand, the apparent contradiction dissolves:

**Graham's airline example** — airline industry has poor economics; capex earns below cost of capital; growth in passenger miles is value-destructive even though it happened. Graham's framing ("growth doesn't translate to profits") is correct for this case because the growth's economic quality was poor.

**Lynch's Wal-Mart example** — Wal-Mart had unique cost-position advantages (no competition in early markets, scale buying, low-rent real estate). Each new store earned returns *well above* cost of capital. So growth multiplied value, and the stock multiplied with it.

Both authors, in the Buffett frame, are saying the *same* thing: **the question is not whether growth is happening, but whether the growth is occurring at returns above cost of capital.** Graham's pessimism reflects his expectation that most growth in commodity industries doesn't pass that bar. Lynch's optimism reflects his focus on niche-protected fast growers where it does.

## When Growth Adds Maximum Value

Growth is most value-creating when:

1. **The business has economic [[foundation/market-wisdom/economic-goodwill|moats]]** — pricing power, network effect, brand, regulatory franchise — that protect the high return on capital
2. **The growth is funded by retained earnings** (so no dilution / leverage cost)
3. **The growth has *runway*** — not at saturation
4. **Capital required per unit of growth is small** (capital-light models — software, asset-light brands, franchises)

Combinations matter. Tech in 2000 had high growth but no moat → growth couldn't be sustained at profitable rates → value collapsed. Procter & Gamble has moats but slow growth → value compounds slowly. The intersection (moat + growth + low capital intensity + runway) is the rare zone where growth produces multibaggers.

## When Growth Subtracts Value

Growth is value-destructive when:

1. **No moat** — competition arbitrages returns down to cost of capital or below
2. **High capital intensity** — each unit of growth requires major capex with deferred returns
3. **Funded by dilution / debt** — the cost of capital exceeds the return earned on the growth
4. **Growth-to-survive** rather than growth-to-prosper — capex required just to maintain unit volume (this is Buffett's [[foundation/market-wisdom/owner-earnings|owner-earnings]] case)

Buffett's framing forces the analyst to ask: **what's the marginal return on retained earnings?** If management retains $X and book value goes up by $X but market cap doesn't rise proportionally, the retention destroyed value. This is the inverse of Lynch's [[foundation/market-wisdom/perfect-stock-attributes|attribute #13 (share buybacks)]] — when no good reinvestment exists, returning capital is the value-creating action.

## The Implication: There's Just Investing

> "What is investing if it is not the act of seeking value at least sufficient to justify the amount paid?"
> — Buffett, 1992

If "value investing" means "buying for less than what you get," then *all* investing is value investing — the alternative is speculation. Calling oneself a "value investor" is, as Buffett says, a redundancy. Calling oneself a "growth investor" is admitting one isn't doing the value calculation.

Three categories collapse into two:

| Old framing | Buffett's framing |
|------------|------------------|
| Value investor | Investor |
| Growth investor | Investor (with growth as variable in calc) — or speculator if no calc |
| Speculator | Speculator |

## Crypto Translation

The dichotomy maps to crypto's "fundamentals vs narrative" debate:

- **"Fundamentals" investing** ≈ value framing — buying tokens for less than discounted future fees / value-accrual
- **"Narrative" investing** ≈ growth framing — buying tokens because total addressable market / hype is growing

Buffett's resolution applies: narrative without value calculation is speculation. Fundamentals without growth consideration miss tokens whose growth genuinely creates value (an L1 with rising fees + non-dilutive emissions schedule).

The right framing in crypto:

```
Token value = Discounted (future fee revenue − necessary emissions) ÷ tokens outstanding

Growth term: rate of fee revenue growth
Value-destruction term: dilution from emissions and unlocks
```

The two-variable nature is more visible in crypto than equities because emissions are explicit and on-chain. Yet most crypto investors still operate in either pure-narrative mode (no value calc) or pure-current-revenue mode (no growth model). Both are incomplete.

## Updating the Existing Lint Notes

This page **resolves** the contradictions flagged in the 2026-04-29 lint at a deeper level than the per-page resolutions provided then.

The cleaner statement to apply going forward:
- All investment requires a value calculation
- Growth is a variable in that calculation
- Growth can be positive, neutral, or negative for value depending on returns earned on retained capital
- "Growth" and "value" as separate strategies is a category error — they're inputs to the same calculation

Pages affected (which still link back here):
- [[foundation/market-wisdom/lynch-six-categories]] — the resolution applies; fast growers are great when growth is value-positive
- [[investment/allocation/enterprising-investor]] — Graham's airline warning is a special case of this principle
- [[investment/allocation/lynch-portfolio-design]] — "watering the weeds" makes sense when winners are still adding value through growth
- [[investment/allocation/defensive-investor-portfolio]] — Graham's mechanical rebalancing happens at a higher abstraction level (asset class), not in tension with this

## Key Takeaways

- Growth is a *variable in value calculation*, not a separate strategy
- Growth can add or subtract value, depending on returns vs. cost of capital
- "Value investing" is a redundancy; "growth investing" without value calc is speculation
- The Graham-vs-Lynch tension dissolves: both are about whether *this* growth is correctly priced
- In crypto, the value calculation must account for emissions/dilution as a value-destruction term
- The framing makes "is this growth value-creating?" the central question, not "is this growth or value?"

## Related

- [[foundation/sources/buffett-essays]] — source
- [[foundation/market-wisdom/owner-earnings]] — the metric that should drive the value calc
- [[foundation/market-wisdom/economic-goodwill]] — what makes growth's return on capital sustainable
- [[foundation/market-wisdom/lynch-six-categories]] — fast growers are value-positive when moat + low capital intensity + runway align
- [[investment/allocation/enterprising-investor]] — Graham's anti-growth airline case is value-destructive growth
- [[glossary/intrinsic-value]] — the formal output of the value calculation

## Open Questions

- **What's the right way to estimate "marginal return on retained earnings" for a crypto protocol?** The on-chain analog is observable (treasury deployment → measurable outcomes), but messy.
- **Is there a crypto category where growth is reliably value-destructive (analogous to Graham's airlines)?** Candidates: copy-cat L1s, late-stage memecoins, protocols with dilutive tokenomics.

## Sources

- Buffett, *1992 Berkshire Hathaway Letter* — original formulation of "growth and value joined at the hip."
- Cunningham (ed.), *Essays of Warren Buffett* (1997), Section II.D ("'Value' Investing: A Redundancy"), pp. 82–88.
