---
schema_version: "1.0.0"
document_id: "4383508601203128a646af73622dd89ed860ada8db3cb461febb989d42ad5ceb"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/reviews-returns-risk-planning"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:faf57c3ca72c365c02ff7bdace8fad1982c00eaa1dfb14c3256cef928f206530"
---

# Mining reviews for returns risk — and feeding it back into the buy

"Runs narrow, sized up and it fit perfect." "Hits weird at the knee if you're under 5'4"." "Sole wore through in about six weeks of regular walking." Every one of those sentences is a data point. None of them live anywhere a planner can query.


That's the gap. Retailers sit on millions of review lines that describe, in plain language, exactly which SKUs are going to come back. Fit complaints, durability complaints, color-mismatch complaints — customers write the returns-risk report themselves, for free, before the return even happens. Almost nobody reads it as planning input. Most retailers route reviews to the CX or moderation team, who care whether the review is fake or abusive, not whether it's telling merchandising to shift a size curve.


## Returns are already a planning problem, not just a service cost


The scale makes this hard to ignore.[NRF projects total U.S. retail returns will hit $849.9 billion in 2025](https://nrf.com/research/2025-retail-returns-landscape) , with an estimated 19.3% of online sales returned. Apparel sits well above that average — online apparel return rates commonly run[25 to 40% of orders](https://eightx.co/blog/average-ecommerce-return-rate) , the highest of any retail category. Fit and sizing alone drive a large share of that: industry estimates put fit-related causes behind[up to 70% of apparel returns](https://eightx.co/blog/apparel-returns-true-cost) . The fully loaded cost of processing a single apparel return — shipping, inspection, restock, customer service, markdown on stock that can't go back on the floor at full price — lands somewhere in the[20 to 45 dollar range per item](https://eightx.co/blog/apparel-returns-true-cost) , enough that a 25% return rate can erase most of a category's unit contribution margin.


Most of that spend is treated as a fixed cost of doing business online. It isn't. A meaningful share of it is a known, named, written-down-by-the-customer problem that never gets fed back upstream.


## The double cost: cash out the door, and a corrupted forecast


Here's the part that matters more to planning than to finance. A return doesn't just cost money — it corrupts the signal the next forecast is built on. Standard demand models are trained on what sold, and a lot of "what sold" quietly includes units that came back. A shopper who orders three sizes and keeps one (bracketing, now a routine habit for online shoppers) shows up in the system as three units of demand at three sizes, not one real sale and two false positives. Feed that into a size-curve model or an allocation engine and you're forecasting against noise, not demand. The industry's own return-forecasting research increasingly treats returns as a signal that needs to be modeled and stripped out of demand history, not folded into it — traditional approaches that simply extrapolate from past return volumes struggle to predict which specific items are actually coming back, because the volume number hides the reason.


Reviews are one of the few sources that carry the reason in plain sight. That's the opportunity: mine the reason, structure it, and use it twice — once to fix the size curve, once to clean the sales history it was built from.


## Turning prose into planning attributes


The mechanics aren't exotic. A review-mining pass reads free text at the SKU and variant level and extracts specific, structured signals: fit direction (runs narrow, runs long, true to size), body-type context (petite, plus, tall), durability complaints tied to a wear window, and color or material accuracy versus the photographed sample. Each extracted claim gets tied to a source review and a confidence score, so a single outlier complaint doesn't get treated the same as a pattern across two hundred reviews.


Review language (raw) Structured attribute Planning action


"Runs narrow, sized up a full size"` fit_variance: narrow, +1 size` Shift size curve toward larger sizes for reorder


"Petite reviewers say it hits below the knee"` fit_by_body_type: petite = longer hem` Add PDP fit guidance; hold before markdown


"Sole wore through in 6 weeks"` durability_flag: sole, <60 days` Route variant to QA/rework; pause reorder


"Color looks more mustard than gold in person"` color_accuracy: mismatch vs. sample` Fix imagery/swatch; suppress until corrected


"True to size, fits like the store sample"` fit_variance: none` Reinforce current curve; safe to reorder as-is


None of this requires the retailer to guess. The values come from what customers actually wrote, validated against volume and recency, with conflicting signals flagged rather than averaged away — a handful of complaints about a size that's otherwise praised shouldn't silently overwrite a working curve.


## Where the signal actually lands


Once fit and durability signals exist as attributes rather than paragraphs, three teams can act on the same extraction without re-reading a single review. Planning adjusts the size curve or buy quantity for the next PO before the pattern shows up as a return-rate spike. Product or sourcing gets a rework flag on a specific style or supplier lot when a durability complaint clusters around a wear window. And e-commerce surfaces the fit guidance directly on the PDP — "runs narrow, consider sizing up" — which is the cheapest possible intervention because it prevents the return from being placed at all instead of processing it after the fact.


That last point deserves emphasis: fixing the buy is the long-term play, but exposing known fit guidance at the point of purchase is the fastest lever, because it acts before the box ever ships back.


None of this asks a retailer to trust an AI's opinion about what a garment fits like. It asks the system to read what customers already said, structure it consistently across every SKU, and route it to the systems and teams already making the buy, the rework call, and the PDP copy. Your PIM stores the SKU and its attributes. Anglera does the work of turning the free text sitting in your reviews into fit-variance, durability, and color-accuracy fields those systems can actually use — extracted from real customer language, quality-scored, and flagged rather than silently overwritten when reviews disagree. That's a cleaner forecast and fewer returns from the same underlying fix.
