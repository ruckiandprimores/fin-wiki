---
title: "Circle of Competence"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [framework, behavioral, buffett, epistemics, risk-management]
---

# Circle of Competence

> **TL;DR**: Buffett's principle: only invest in businesses you can correctly evaluate. The circle's *size* doesn't matter; *knowing its boundary* does. The most expensive investment errors come from operating outside the circle while believing you're inside it. Direct test for any candidate: can you write down what would make this position *fail*, and would you recognize the failure when it happened?

## The Principle

> "What an investor needs is the ability to correctly evaluate selected businesses. Note that word 'selected': You don't have to be an expert on every company, or even many. You only have to be able to evaluate companies within your circle of competence. The size of that circle is not very important; knowing its boundaries, however, is vital."
> — Buffett, 1996 letter

The principle is **negative**, not positive: it tells you what *not* to do. The strict reading: even excellent investments outside your circle are off-limits, because you can't reliably distinguish them from deceptive lookalikes.

## Why It Matters

Investing rewards *correct* analysis, not *clever* analysis. Within the circle, you can be approximately right. Outside, you may be precisely wrong without knowing it.

The asymmetry:
- **Inside the circle**: small information edge → small/medium return → repeatable
- **Outside the circle**: large information *deficit* → large losses → not repeatable (you only have one bankroll)

The cost of staying small (giving up potential opportunities outside the circle) is bounded. The cost of overreaching (a single catastrophic outside-circle bet) can be unbounded.

## Distinguishing Circle of Competence From Other Concepts

| Concept | What it is | What it's not |
|---------|-----------|---------------|
| **Circle of competence** (Buffett) | Honest scope of what you can evaluate | Smartness or IQ |
| **Amateur edge** ([[foundation/market-wisdom/amateur-edge\|Lynch]]) | Real-world observation as research input | Substitute for analysis |
| **Margin of safety** ([[foundation/market-wisdom/margin-of-safety\|Graham]]) | Pricing buffer in case you're wrong | Replacement for being right |

Lynch and Buffett complement each other:
- Lynch's amateur edge is a *generator* of candidates
- Buffett's circle of competence is a *filter* on candidates
- Both should be applied; neither alone is sufficient

## The Operational Test

Buffett doesn't give a formal definition. The de-facto test, distilled from his letters:

For a candidate investment, can you answer all of these?

1. **What is the business?** (Not "what's the ticker," but: what does it sell, to whom, why, how often?)
2. **What is its sustainable competitive advantage?** (Or honest answer: it has none — meaning it's a commodity business, and you're buying for a different reason.)
3. **What could disrupt it in 5–10 years?** (List the specific threats, not vague ones.)
4. **What does management do well? Badly?** (Specifics, not platitudes.)
5. **What's a reasonable range for its earnings 5 years from now?** (If your range is "$0 to ∞," you're outside the circle.)
6. **What price would you pay confidently? Sell confidently?** (Both, with reasoning, before the position is established.)
7. **What would prove your thesis wrong?** (If you can't articulate a falsifier, see Part I Section 4 — you're not analyzing, you're confessing.)

If any of these is "I don't really know," the candidate is outside the circle. Pass.

## Common Failure Modes (Outside-the-Circle Errors)

Each is a way of operating outside the circle while believing you're inside.

### 1. Confusing Familiarity With Understanding

> "I use the product / I work in the industry / I read the news"

Familiarity ≠ ability to evaluate. A doctor knows medicine; that doesn't mean she can evaluate a pharmaceutical company's pipeline NPV. A retail customer knows products they like; that doesn't mean they can evaluate the manufacturer's competitive moat.

### 2. Borrowing Conviction From Smart People

> "Munger said it's good, so I bought it"

Conviction is non-transferable. If you bought because Munger said so, you'll *sell* on the next 30% drawdown because Munger isn't there to hold your hand. Without your own analysis, you have no anchor for any price.

### 3. Pattern-Matching to Past Winners

> "This looks like Amazon in 1998 / Bitcoin in 2013"

Pattern-matching is the surface signal of understanding without any of its substance. Most things that "look like" past winners turn out to be the failures that didn't get remembered. The corpus is survivor-biased.

### 4. Industry/Hype Tourism

> "Everyone's buying this, I should learn about it"

Buffett famously avoided tech in the 1990s — costing him decades of compounding. He defended the choice as honest scope. The choice is between **missing winners outside your circle** (limited regret) and **owning losers you didn't understand** (unlimited regret). Buffett picked the first.

### 5. Sliding Outside the Circle Mid-Position

A position that started inside the circle can drift outside as the business changes. A railroad that becomes a real estate / data center / hyperscale company is no longer the railroad you originally analyzed.

## Crypto Adaptation

Circle of competence is arguably the **single most-violated principle** in crypto investing. Common patterns:

| Pattern | Why it's outside-the-circle |
|---------|-----------------------------|
| Holding ERC-20 tokens whose value-accrual mechanism the holder can't articulate | The literal definition of "I don't know what makes this go up." |
| Trading L1 tokens based on TVL without understanding how TVL is computed (and gameable via wash-deposits) | Numerical metric mistaken for fundamental |
| Investing in DeFi without reading the protocol's source code or having a competent reader's report | The "code is law" market ≠ the "code I haven't read is law" market |
| Yield farming positions held without understanding the source of yield (often: emissions of the same token) | Yield from emissions is a return *of* capital, not *on* capital |
| Buying a token because "everyone's bullish on \[narrative\]" | Pattern-matching to narrative ≠ analysis of value accrual |

**A useful crypto-specific operationalization** of the circle test, applied per token:
1. Where does the token capture value? (Fee revenue? Burn mechanism? Governance? Speculative narrative?)
2. What's the supply schedule and emissions through the next 24 months?
3. Who's the marginal seller? (VCs unlocking? Validators? Treasury sales?)
4. What does revenue look like, net of token emissions used to generate it?
5. What price would you confidently buy? Sell?

If any answer is "I don't know," the token is outside the circle.

## The "Knowing the Boundary" Discipline

Buffett's harder claim: knowing where your circle *ends* is more important than how big it is. This requires honest negative scoping.

Examples Buffett gives in the letters:
- **He doesn't understand tech businesses well enough** (1990s–2000s position; partially reversed for Apple later) → he avoided them, conceding the missed gains
- **He doesn't understand commodities well enough** (cocoa, silver, oil drilling exceptions noted) → mostly avoids them
- **He understands consumer brands, insurance, and capital-intensive franchises with pricing power** → that's where Berkshire's holdings concentrate

The discipline: write down the categories you *don't* understand, and refuse to invest in them — *especially* when they're going up and you feel left out. The fear-of-missing-out is a circle-violation generator.

## Key Takeaways

- The principle is negative: it constrains, it doesn't generate
- Boundary-knowledge > circle-size
- Familiarity ≠ understanding
- Borrowed conviction is the most common failure mode
- The circle test is operational: a list of questions, all of which must be answerable
- In crypto, the most-violated principle — value-accrual mechanism opacity is endemic
- The cost of staying small (missed outside-circle winners) is bounded; the cost of overreaching (catastrophic outside-circle losses) is not

## Related

- [[foundation/sources/buffett-essays]] — source
- [[foundation/market-wisdom/amateur-edge]] — Lynch's complementary input-side principle (observation as candidate generator)
- [[foundation/market-wisdom/margin-of-safety]] — Graham's price-side risk management
- [[foundation/market-wisdom/perfect-stock-attributes]] — Lynch's "user of technology" attribute relates: prefer being a user inside your circle than a maker outside
- [[foundation/methodology/behavioral-structure-design]] — the structure-filter counterpart to this scope-filter; both filters must apply (what to consider × how to hold what passes)
- [[glossary/circle-of-competence]] — short definition

## Open Questions

- **What's a structured way to expand a circle of competence over time?** Buffett's implicit answer: years of focused study in one domain. Is there a more efficient curriculum?
- **In crypto, is the proper circle "all of crypto" or "specific verticals" (e.g., DEXes, lending, infra, L1s)?** The latter probably; the former is too broad to genuinely understand.

## Sources

- Buffett, *Berkshire Hathaway 1996 Letter to Shareholders* (Intelligent Investing essay), as collected in Cunningham (1997).
- Buffett, *1989 Letter* — "Mistakes of the First Twenty-Five Years" — sub-section on the institutional imperative includes the related theme of staying within competence.
