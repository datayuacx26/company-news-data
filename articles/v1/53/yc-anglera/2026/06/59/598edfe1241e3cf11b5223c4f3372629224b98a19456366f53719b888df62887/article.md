---
schema_version: "1.0.0"
document_id: "598edfe1241e3cf11b5223c4f3372629224b98a19456366f53719b888df62887"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/size-curve-fit-feedback"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:08.422826+00:00"
content_hash: "sha256:e9ce84ce52fd763a93d11307295aa852fef7bc9f9fe727a98854dcae8f56c9a6"
---

# When 'runs small' should change the size curve

A buyer orders a style in the standard 2-6-12-16-6-2 curve across sizes XS-XXL. Three weeks after launch, the size L is selling out in six days and the size M is barely moving, except through returns. Nobody planned it that way. The curve was built off the brand's standard block, and the block assumed this style would fit like the last one. It didn't. Reviews said so within the first fifty units, in language like "runs a full size small in the shoulders." Nobody on the planning side ever saw that sentence.


This is the gap between two systems that should talk to each other and mostly don't. Fit feedback lives in review text, customer service transcripts, and return-reason codes. Size curves live in a planning tool that only understands numbers: units by size, by store, by week. The bridge between them, an attribute that says "this style runs small," almost never gets built, so the two systems keep operating on different versions of reality.


## Fit is the largest, least-modeled driver of apparel returns


Sizing and fit issues account for the majority of apparel returns industry-wide. Zalando, which tracks this closely enough to run a dedicated Size & Fit team, reports that[one third of its overall returns are size-related](https://corporate.zalando.com/en/about-us/what-we-do/how-zalando-leverages-technology-help-customers-find-right-size) , and its algorithm flags items as running large or small by combining brand measurements, return history, and return reasons before customer-facing size advice ever gets shown. Retailers overall are on pace to absorb[roughly $850 billion in returned merchandise in 2025](https://www.digitalcommerce360.com/2025/10/24/nrf-consumers-return-850-billion-merchandise-in-2025/) , and NRF's own commentary points at fit guidance, sizing tools, and imagery as the levers retailers are actually pulling to bring that number down.


A meaningful share of that volume isn't even a true return in the sense of "wrong choice." It's bracketing: a shopper orders two or three sizes of the same style, keeps one, sends the rest back, because the size chart can't be trusted and the reviews are the only real signal. Bracketing shows up across a large share of online apparel shoppers and drives a double-digit share of returned units at multi-brand retailers. None of that demand disappears from the P&L. It shows up as inflated gross orders, deflated sell-through, and processing cost; industry estimates put returns at[$10 to $20 per unit](https://www.readycloud.com/info/statistics-on-ecommerce-bracketing-the-apparel-returns-effect) once shipping and restocking are counted.


Here's the part that matters for planning specifically. Bracketing and fit-driven exchanges distort the demand signal at the size level, which is exactly the level a size curve is built on. If M appears to sell through fast because half of the M orders are bracket-and-return, and L appears slow because true M-fit customers gave up and sized down, the curve for next season gets rebuilt on a corrupted read of what actually fit.


## The signal already exists, it's just unstructured


Reviews are noisy at the individual level, one person's "runs small" is another person's "true to size," but they're not noisy in aggregate. Across a few dozen reviews, a consensus emerges: this style runs narrow through the toe box, that jacket runs long in the sleeve, this dress is true to size everywhere except the bust. Shoppers already read reviews for exactly this reason, scanning past the size chart for fit language before they buy. The information is there. What's missing is a step that turns "runs small," "true to size," and "tight through the shoulders" into a structured attribute attached to the style, at the size-and-fit-point level, that a planning tool or a PDP can actually read.


That's an extraction and normalization problem, not a new-data problem. Review text, return-reason free-text fields, and CS transcripts all carry the same underlying signal in different vocabularies. "Runs small," "sized up and it was perfect," "wish I'd ordered a size up," and "tight in the chest" are four ways of saying the same thing about the same size point. Turning that into something usable means:


- Classifying each mention against a[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) : runs-small, runs-true, runs-large, plus a dimension tag (chest, waist, length, width, toe box) where the text supports it
- Weighting by volume and recency, so a pattern from 200 reviews this season outranks a stray complaint from a discontinued colorway
- Attaching the resulting attribute to the style and, where the feedback is size-specific, to the size point itself, not just a blanket "fits small" flag on the whole product
- Flagging disagreement, if reviews split roughly down the middle between runs-small and runs-large, that's a real signal (probably a fit variance issue, not a fluke) and it should surface as low-confidence, not get averaged into a false "true to size"


Signal source Raw form Structured output


Review text "Ordered my usual M, way too tight, exchanged for L" fit_consensus: runs-small; dimension: chest


Return reason code "Too small" (23% of returns on this SKU) fit_consensus: runs-small; confidence: high


CS transcript "Customer said sleeves run long, asked for hem advice" dimension_note: sleeve-length-long


Exchange data Net exchange flow M-to-L on 61% of M orders fit_consensus: runs-small; corroborating


Once that attribute exists on the style, it's usable in two directions at once, and this is the part worth sitting with. Planning uses it to shift the size curve before the next buy, buy fewer M, more L, or add a between-size option if the runs-small pattern is severe enough. E-commerce uses the same attribute to set expectations on the PDP before the order happens: a size-guidance note that says "runs small, most reviewers sized up" is cheaper than the return it prevents. Same underlying data, two different consumers of it, generated once.


## What this changes for the next buy, not just the current one


The immediate win is fewer returns on the current style, size guidance on the PDP catches some share of shoppers before they order wrong. The bigger win is upstream. If a factory or a fabric consistently produces a runs-small result, and reviews say so across three consecutive drops, that's a signal a technical design or product team can act on before the next tech pack goes to production, not just a customer service problem to manage after the fact. And if planning has fit-consensus as a structured attribute on every style, at every size point, they can start segmenting the curve by fit behavior, not just historical sell-through, catching a runs-small pattern in week two of a launch instead of learning it from a season-end sell-through report.


None of this requires ripping out a size chart, a PIM, or a planning tool. It requires the fit signal buried in review text and return data to get extracted, normalized against a shared vocabulary, and attached as a structured attribute where planning and e-commerce can both reach it. Anglera does that extraction and validation work on top of whatever systems already hold your product and return data, so the size curve for next season and the size guidance on today's PDP are finally reading from the same evidence.
