---
title: "Owner Earnings — Buffett's GAAP Correction"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [accounting, valuation, buffett, mechanism, crypto-translatable]
---

# Owner Earnings — Buffett's GAAP Correction

> **TL;DR**: GAAP earnings systematically overstate the cash that can be taken out of most businesses, because depreciation/amortization charges (b) are usually less than the actual capex (c) required to maintain the business. Buffett's correction: subtract maintenance capex from reported earnings + non-cash charges to get *owner earnings* — the cash actually available to owners. Wall Street's "cash flow" number (which adds back non-cash charges but doesn't subtract maintenance capex) is *worse* than GAAP earnings, not better.

## The Formula

```
Owner earnings = (a) reported earnings (after tax)
              + (b) depreciation, amortization, depletion, other non-cash charges
              − (c) average annual capex required to fully maintain
                    long-term competitive position and unit volume
                    (incl. working capital increase if needed for unit volume maintenance)
```

The key claim: for most businesses, **(c) > (b)**. The accounting depreciation schedule is set by tax law and historical cost; the actual maintenance capex must be set in inflation-adjusted current dollars. The gap is usually meaningful.

## What's Wrong With GAAP Earnings

GAAP depreciation charges are based on historical cost and tax-driven schedules, not on what it actually costs to keep the business running today.

**Example (Buffett's Scott Fetzer case, *1986 letter*):**

| Item | "Old" company (pre-acquisition) | "New" company (post-acquisition) |
|------|-------------------------------|-----------------------------------|
| Reported earnings | $40.2M | $28.6M |
| Depreciation (b) | $8.3M | $19.9M |

The reported earnings *fell* $11.6M when Berkshire bought the company — but only because the purchase price triggered new amortization charges. Real economics didn't change.

Buffett's question: "What did Berkshire shareholders buy — a business earning $40.2M or one earning $28.6M?"

Answer: neither. Owner earnings is what matters, and for Scott Fetzer, **(c) ≈ pre-acquisition (b) ≈ $8.3M** (much less than post-acquisition (b)). So owner earnings ≈ pre-acquisition reported number.

## What's Wrong With "Cash Flow"

Wall Street's "cash flow" number = (a) + (b). This systematically *overstates* economic reality more than GAAP does, because it adds back depreciation without subtracting the real capex.

> "All of this points up the absurdity of the 'cash flow' numbers that are often set forth in Wall Street reports. These numbers routinely include (a) plus (b) but do not subtract (c). Most sales brochures of investment bankers also feature deceptive presentations of this kind. These imply that the business being offered is the commercial counterpart of the Pyramids — forever state-of-the-art, never needing to be replaced, improved, or refurbished."

The deception is most severe in capital-intensive industries:
- **Oil & gas** — drilling/exploration capex is huge; flat or declining production requires continuous spending
- **Telecom** — network upgrades constant; copper-to-fiber, 4G-to-5G
- **Airlines** — fleet replacement
- **Heavy industrials** — plant maintenance, equipment refresh

For these, "cash flow" can be 2–5× true owner earnings. Investors paying multiples of "cash flow" (EV/EBITDA, EV/CFO) systematically overpay.

## When (b) and (c) Are Approximately Equal

For *some* businesses — typically asset-light, brand-driven, or services businesses — maintenance capex is genuinely close to depreciation. Buffett cites See's Candies as an example where the gap is small (See's needs ~$0.5–1M/year of capex above depreciation just to maintain competitive position — manageable).

In these cases, GAAP earnings ≈ owner earnings. But the analyst still has to verify (c), not assume it.

## What Counts as "Maintenance" Capex?

The hardest part of the formula is (c). Buffett's definition:

> "The average annual amount of capitalized expenditures for plant and equipment, etc. that the business requires to fully maintain its long-term competitive position and its unit volume."

This is *not* total capex (which includes growth capex). It's only the portion needed to keep the business running at the same scale.

Practical decomposition: total capex = maintenance capex + growth capex.

Useful proxies for maintenance capex:
- Multi-year average capex during periods of zero unit volume growth
- Industry consultant estimates of replacement cycles for plant and equipment
- Management's own statements on capex split (treat with skepticism — managers prefer to call all capex "growth")

> "Our owner-earnings equation does not yield the deceptively precise figures provided by GAAP, since (c) must be a guess — and one sometimes very difficult to make."

The acknowledgment matters. Owner earnings is *vaguely right*, while GAAP earnings is often *precisely wrong*. (Quoting Keynes via Buffett.)

## Relationship to Free Cash Flow

| Metric | Formula | Comment |
|--------|---------|---------|
| GAAP earnings | (a) | Distorted by tax-driven depreciation |
| Wall Street "cash flow" | (a) + (b) | Adds back non-cash charges; ignores capex entirely |
| Free cash flow (corp finance textbook) | (a) + (b) − total capex | Subtracts ALL capex (maintenance + growth) |
| **Owner earnings** | (a) + (b) − maintenance capex only | Buffett's formula |

The textbook FCF metric *over*-corrects: it subtracts growth capex, which is value-creating. Owner earnings is the cleanest measure of cash actually distributable to owners while keeping the business at the same scale.

## Crypto Translation — The Stronger Application

The cash-flow fallacy is *more* severe in crypto than in equities, because the analog of (c) — the cost required to keep the protocol running and competitive — is often paid in tokens rather than cash, and is therefore not visible in any "protocol revenue" number.

### The crypto owner earnings formula

```
Crypto owner earnings = (a) protocol revenue (fees, MEV, etc.)
                     + (b) any non-cash treasury inflows (incl. proper accounting)
                     − (c) tokens distributed to maintain protocol operation:
                          - liquidity mining emissions
                          - validator rewards (above what minimum security requires)
                          - ongoing security/audit/bounty spending
                          - DAO operating budget
                          - tokens redirected to fund development
```

Most "protocol revenue" numbers reported by analytics dashboards (DeFiLlama, Token Terminal) report **only (a)**. They don't subtract (c). For DeFi protocols where emissions are 2–5× revenue, the *real* owner earnings number is *negative* — but the dashboard shows positive revenue.

### Concrete examples (illustrative, not investment recommendations)

**A DEX with $50M annual fees, $80M annual liquidity-mining emissions:**
- Reported "revenue": $50M
- Owner earnings: $50M − $80M = −$30M
- The protocol is paying $30M/year (in stock) to maintain its competitive position. Not a positive-cash-flow business.

**A lending protocol with $30M fees to token holders, $0M emissions:**
- Reported revenue: $30M
- Owner earnings: $30M − ε ≈ $30M
- Real positive-cash-flow business. (Subject to the (c) being honestly small.)

The discipline forces honest accounting: a protocol that pays its expenses in newly-minted tokens is *not* a positive-cash-flow business, regardless of how its dashboard renders. The token holders are funding the operating losses through dilution.

### Why this matters more in crypto than equities

In equities, hidden capex shows up eventually when depreciation catches up to reality. In crypto, hidden capex (emissions) shows up *immediately* in the price as supply expands — but only the holders not selling experience it. The narrative around revenue can persist for years even as long-term holders bleed value.

## Test Plan / Operationalization

For a target candidate (equity or crypto), produce:
1. Reported earnings or revenue (a)
2. Non-cash charges (b)
3. Best-effort estimate of maintenance capex/emissions (c) — show your reasoning
4. Owner earnings = (a) + (b) − (c)
5. Compare to current valuation: is EV / owner-earnings reasonable?
6. **Falsifier**: if (c) is genuinely much smaller than your estimate, the position is undervalued; if (c) is larger, overvalued.

This is a structured-hypothesis exercise per Part I Section 4.

## Key Takeaways

- GAAP earnings systematically overstate distributable cash for most businesses
- Wall Street "cash flow" numbers are worse than GAAP, not better
- Owner earnings = reported earnings + non-cash charges − maintenance capex
- (c) is the hard part; "vaguely right" beats "precisely wrong"
- Crypto translation is stronger than equity application: token emissions are the (c) that protocol-revenue dashboards omit
- A protocol with revenue < emissions is *not* a positive-cash-flow business

## Related

- [[foundation/sources/buffett-essays]] — source
- [[foundation/market-wisdom/economic-goodwill]] — companion concept; what makes (a) − (c) > 0 sustainably
- [[foundation/market-wisdom/circle-of-competence]] — without this, (c) cannot be honestly estimated
- [[foundation/market-wisdom/lynch-six-categories]] — Lynch's "cyclicals" are the worst victims of cash-flow-fallacy: peak-cycle (a) + (b) looks great while (c) is being deferred
- [[glossary/intrinsic-value]] — owner earnings is the input to intrinsic value calculation
- [[investment/allocation/wonderful-business-fair-price]] — high-quality businesses are partly defined by (c) being small relative to (a)

## Open Questions

- **What's the most-defensible methodology for estimating crypto (c)?** Token emissions are easier to see than equity maintenance capex (on-chain), but the *necessary* portion (vs. discretionary/growth) is debatable.
- **Are there any equity sectors where GAAP earnings > owner earnings (i.e., understated)?** Buffett implies yes for some asset-light franchises.
- **Does owner-earnings analysis work for cyclicals at all?** During cycle troughs, all of (a)/(b)/(c) are distorted.

## Sources

- Buffett, *1986 Berkshire Hathaway Letter* — Scott Fetzer case study, original formulation of owner earnings.
- Cunningham (ed.), *Essays of Warren Buffett* (1997), Section V.D ("Owner Earnings and the Cash Flow Fallacy"), pp. 180–187.
