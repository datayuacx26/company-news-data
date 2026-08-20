---
schema_version: "1.0.0"
document_id: "1d1b0be69baab4d57918a1cd427aa912872ab83c97f95ac1bb8d670ae28353e4"
company_key: "yc-cofactr"
company: "Cofactr"
source_id: "yc-cofactr-news-import-06c16db4e4eb"
canonical_url: "https://www.cofactr.com/articles/beyond-the-quote-how-cofactr-calculates-true-landed-cost"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-27T08:34:13.149498+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:927af01467c7362c5366d8993136542cfdc52b2073e9ed55b93a0be6bc87cd17"
---

# Beyond the Quote: How Cofactr Calculates True Landed Cost

##


Ask most procurement teams how they pick a supplier, and you'll hear some version of "we go with whoever quotes the lowest unit price." or “I have a good relationship with this supplier”. It's an understandable shortcut, but it's usually wrong. The quoted unit price is one line in a much longer equation that includes shipping, payment fees, tariffs, customs brokerage, minimum-order penalties, and lead time. Get any one of those wrong and a "cheap" part turns into an expensive delay or an unbudgeted duty bill.


Cofactr's price optimization engine exists to solve that equation properly, every time, for every line on every order. Here's what's actually happening when Cofactr selects a supplier.


## The first problem isn't finding a price. It's finding the right lead time to price against.


Most sourcing tools compare supplier offers and quotes as if lead time were fixed. Cofactr’s doesn't, because it isn't. A supplier might have parts in stock today, or available in three weeks from a backorder, or six weeks out from a factory run — and each of those options has a completely different cost structure once you factor in shipping method, payment terms, and how far out you're willing to push a project.


So instead of picking a lead time and then finding the cheapest offer, Cofactr's engine works the other way: for every realistic lead-time scenario on a line, it evaluates every viable combination of offers and finds the one that meets demand for the lowest true cost at that timeline. That means you don't just get "the cheapest part" — you get the cheapest way to hit *your* schedule, and visibility into what it would cost to loosen that schedule by a week or a month.


Critically, cost isn't judged on unit price alone. Every candidate offer is evaluated on its fully loaded cost — unit price at the quantity you actually need (including MOQ and Multiple), shipping (or $0 once an overall order clears a supplier's free-shipping threshold), wire transfer and escrow fees, tariffs, customs brokerage, Cofactr's services, and any minimum-order shortfall penalty — all rolled together before offers are ever compared against each other. That's what makes the recommendation trustworthy: the option that looks cheapest is cheapest, landed cost included, not just cheapest on the line item.


## What goes into the model


The reason this actually works in aerospace, defense, and robotics supply chains — where country of origin matters, backorders are common, and a "similar" part isn't an acceptable substitute — is that the engine isn't just looking at price. It's synthesizing data from six different layers before a single number gets compared.


**The demand itself.** Quantity needed, the primary part and any approved alternates, and your purchase rules — which suppliers are allowed, banned, preferred, or require authorization. Your preferences for that line also come into play directly: target and max lead time, whether backorders are acceptable, destination country, whether parts need to ship on reels, and whether you want Cofactr to manage the purchase or handle it yourself.


**Every individual offer.** Price breaks at different quantities, reported stock, MOQ and multiple-purchase-quantity rounding, lead time components, backorder status, and whether the offer is authorized under your purchase rules.


**Part attributes.** Country of origin and HTS classification drive tariff matching — Cofactr checks all applicable codes and uses the worst applicable rate, so you're never surprised by a duty that should have been priced in from the start. Cofactr gathers HTS and COO data for all your parts automatically. Manufacturer and packaging data flag restricted suppliers and detect reel packaging.


**Supplier records** , pulled dynamically for every supplier on every eligible offer. This is where a lot of the real sophistication lives: buyability, supplier reliability, drop-ship support, military-use restrictions, non-buyable manufacturer lists, reseller and master distributor relationships (so the optimizer can merge offers from the same underlying source rather than double-counting them), shipping options and thresholds, minimum order sizes, and supplier-specific tariff adjustments.


**Tariff and import economics.** For cross-border orders, Cofactr calculates applicable tariffs directly — matching country of origin and HTS classification against current rates, including special programs such as Section 301, and using the worst applicable rate across all matching codes, so duties are priced in up front rather than discovered later. Tariffs aren't only an import problem, though: domestic suppliers whose own costs are affected by tariffs on their inputs often pass a portion of that cost through to customers, and Cofactr accounts for that pass-through too, even on orders that never cross a border. On top of that, de minimis thresholds and country-specific exceptions (certain countries are excluded from de minimis treatment entirely) determine whether an order qualifies for simplified customs brokerage or gets the standard rate.


**Your organization's context.** If your org is military end-use, suppliers that restrict those end uses are dropped before pricing even starts. Your organization's custom quoted pricing, sourcing restrictions, and supplier preferences are factored in.


## Lead time you can actually plan around


Quoted lead time and real-world lead time are rarely the same number, and treating them as identical is one of the most common ways procurement teams get burned on schedule. A supplier might quote a two-week lead time on paper but consistently ship closer to three or four weeks in practice.


To close that gap, Cofactr applies a ships-in-lead offset to every supplier's quoted lead time — an adjustment derived from that supplier's actual historical shipping performance across tens of thousands of past orders, not just what's printed on the quote. Stock orders and backorders are treated separately, since a supplier's reliability on parts they hold versus parts they have to build or source can look very different. That real-world timing is then combined with the applicable shipping method to build out each candidate delivery window, so the lead time you see reflects how a supplier actually ships, not just how they say they will.


## Filtering before the comparison even starts


Before any pricing comparison happens, your purchase rules take a first pass: authorized-only restrictions, allowed/preferred/banned/last-resort supplier lists, and a soft preference layer that drops weaker options entirely when a preferred, non-backorder, on-target-lead supplier already covers demand on its own. Reseller and master distributor relationships get deduplicated, so the same underlying stock isn't evaluated twice under two different supplier names.


## What shows up in your landed cost


Because every cost component is built into the comparison from the start rather than tacked on afterward, the landed cost you see reflects everything that actually affects what you pay and when you get the part.


**Parts:** Price breaks at MOQ/multiple-adjusted quantity


**Import tariffs:** Worst matching rate across country of origin and HTS codes, for cross-border orders


**Tariff pass-through:** Supplier-specific surcharges passed through on domestic orders when a supplier's own costs are tariff-affected, even though the order itself isn't crossing a border


**Explicit duties:** Duty amounts stated directly on the quote


**Custom reeling:** Applied when your destination requires reels and the supplier supports custom reeling


**Shipping:** Selected shipping option, or $0 above the supplier's free-shipping threshold


**Payment fees:** Wire fee above a set order value, percentage-based processing below it, for suppliers Cofactr can't buy from directly


**Customs brokerage:** De minimis vs. standard rate, based on order value and country


**Cofactr services:** Applied and adjusted for any supplier-specific discount


**Minimum-order shortfall:** Priced in directly whenever an order falls below a supplier's minimum


## The scale of it


It's worth pausing to consider just how much is being evaluated to arrive at a single recommended supplier for a single line. For one part, Cofactr's engine is weighing dozens of individual data points — pricing at multiple quantity breaks, stock and lead-time signals, MOQ and packaging details, country of origin and HTS classification, and a full profile of every competing supplier's buyability, shipping terms, tariff exposure, and historical shipping performance.


Now multiply that by a realistic BOM. A typical 100-line bill of materials, with several competing offers per line, means Cofactr's engine is factoring in well over ten thousand individual data points to produce one set of sourcing recommendations, and this is happening for every possible lead time scenario, not just once. That's the difference between "we compared a few quotes" and a system that's actually accounting for everything that determines what an order really costs and when it really arrives — for every line, every time.


## Why this matters


None of this is visible in a typical quote comparison, which is exactly the point. A distributor's line-item price tells you almost nothing about what an order actually costs to land at your dock, on your schedule, under your compliance requirements. By modeling price, shipping, payment, tariffs, and lead time together from the start, Cofactr gives procurement teams a number they can actually trust, and a clear view of the tradeoffs behind it.


That's the difference between sourcing software that shows you offers, and one that actually tells you what to buy.
