---
schema_version: "1.0.0"
document_id: "ce8caa93caae2db6454a9cba68cd52b8558a12069534ff528ac4279f4374b4ba"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/appliances-guide"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:aacce1c1d1c942285bc7e121d83429ba138273f3d0d21925ec7b837b19e050a7"
---

# A retailer's guide to dimension, capacity, and feature data in appliances

A French door refrigerator is a two-person delivery, a torn-out door frame if it doesn't fit, and a $2,500 decision made from a phone screen. Shoppers can't open the doors or run a tape measure through the checkout flow, so the product page has to do that work for them. Most appliance listings still don't.


## What a refrigerator shopper actually needs answered


Before a shopper adds a French door refrigerator to cart, they're quietly trying to answer questions that have nothing to do with color or brand:


- Will it fit through my front door, my hallway, and the doorway into the kitchen?
- Will it fit in the cutout between my cabinets, and does the door need clearance to swing open?
- Is this counter-depth or standard-depth, and how far will it stick out past my counters?
- How much usable capacity is there, and does that number account for shelves, bins, and the ice maker?
- Does it need a water line, and where does the connection sit on the back panel?
- Is the icemaker left-hinge or right-hinge, and can the doors be reversed?


Standard French door refrigerators run roughly 30 to 36 inches wide, 29 to 35 inches deep, and 67 to 70 inches tall, while counter-depth models sit around 24 to 25 inches deep so they sit flush with cabinets instead of jutting into the walkway, according to[Whirlpool's sizing guide](https://www.whirlpool.com/blog/kitchen/french-door-refrigerator-sizes.html) . That six-to-ten-inch depth difference is the whole ballgame for a lot of kitchens, and it's exactly the kind of attribute that gets flattened into one vague "dimensions" field on too many product pages.


## Where the feed breaks down


Here's what a typical manufacturer feed hands a retailer for a 25 cu ft French door refrigerator, next to what the shopper actually needs to see on the page:


Attribute Raw feed Enriched listing


Dimensions` 35.75 x 34.5 x 70` Width 35.75 in, depth 34.5 in (36.25 in with door open 90°), height 70 in (69.5–70.5 in with adjustable rollers)


Depth type (missing) Standard-depth — extends about 3 in past most cabinets


Capacity` 25 cu ft` 25 cu ft total: 17.5 cu ft fresh food, 7.5 cu ft freezer, door bins and deli drawer included


Door swing (missing) Reversible doors, 22 in clearance needed to open bins fully


Water/ice` icemaker: Y` Requires 1/4 in water line hookup; icemaker produces up to 4 lb of ice per day


Doorway fit (missing) Case depth (doors removed) 29 in — passes standard 30 in interior doorways


Every row in the "missing" column is a return waiting to happen, or a sale that quietly dies at the measuring-tape stage before the shopper ever reaches checkout.


## The returns math retailers already know


Fit and sizing issues are the single biggest driver of e-commerce returns, responsible for roughly 45% of them across categories, per[Return Prime's 2025 e-commerce return data](https://www.returnprime.com/blog/e-commerce-return-trends) . Items not matching their description account for another 22 to 31% of returns industry-wide, according to the same analysis. Appliances make that worse than average, because a mis-measured refrigerator isn't a $30 return-label problem — it's a two-person truck, a restocking fee, a torn-out cabinet, and in some cases a doorway or stairwell that simply won't accommodate the unit at all, a scenario appliance delivery teams describe as[one of the most common causes of failed deliveries](https://www.aztecappliance.com/blog/properly-measure-for-new-appliances) .


That failure mode almost never shows up as a "return reason: wrong size" line item, because the box never technically shipped wrong — the data on the page just wasn't complete enough for the shopper to self-select correctly. It shows up instead as a cancelled order, a rescheduled delivery fee, or a one-star review about "inaccurate dimensions," none of which get traced back to the missing case-depth attribute that caused it.


## The AI-shopping test


Increasingly, that shopper isn't scrolling your PDP at all — they're asking ChatGPT or Google's AI Mode to "recommend a counter-depth French door refrigerator with an ice maker that fits a 30-inch doorway." An AI agent answering that query needs case depth, door-open clearance, and doorway compatibility as structured, queryable attributes, not buried in a PDF spec sheet or a paragraph of marketing copy. A listing with a single blended "dimensions" string and no depth-type flag simply won't surface for that query, no matter how good the product is.


## A dimension, capacity, and feature checklist


Run every appliance listing against this before it goes live:


1. **Three dimensions, not one string** — width, depth, and height as separate fields, plus door-open depth.
2. **Depth type called out explicitly** — counter-depth vs. standard-depth, not just implied by the number.
3. **Capacity broken down** — total cu ft, plus fresh food and freezer split where applicable.
4. **Doorway/case depth for delivery** — the crated or door-removed dimension that determines whether it clears a standard interior doorway.
5. **Hookups and hinge behavior** — water line requirements, door reversibility, and clearance needed to open drawers and bins fully.
6. **ENERGY STAR and energy-use data** where it applies, since it's now a routine filter shoppers and comparison engines both use.


Anglera runs this checklist against a retailer's live catalog automatically, flagging appliance listings with missing depth type, blended dimension fields, or absent capacity breakdowns, then filling the gaps from verified spec data. It plugs into whatever PIM or feed a retailer already runs — no rip-and-replace — so the fix shows up on the page without a re-platforming project attached to it.
