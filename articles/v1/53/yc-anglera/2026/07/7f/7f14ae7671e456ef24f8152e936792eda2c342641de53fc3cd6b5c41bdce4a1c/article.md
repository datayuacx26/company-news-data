---
schema_version: "1.0.0"
document_id: "7f14ae7671e456ef24f8152e936792eda2c342641de53fc3cd6b5c41bdce4a1c"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/attributes-that-decide-the-recommendation"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T22:51:30.785703+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:dd193b671bacf8bb3753f8303463591b9d00930910fa95c02a006e0790025e2a"
---

# The attributes that decide the recommendation, not the ones that get you listed

A service contractor types this into an assistant on the way to a job:


> "I need a replacement definite purpose contactor for a 480V three-phase condenser pulling about 30 amps. Has to be UL listed. What coil do I need with it?"


That is one sentence with five separate constraints in it, plus a follow-on purchase. Work out what a catalog has to contain to answer it and you have a much better attribute roadmap than any generic completeness checklist will give you. The pillar version of this argument lives in[how to structure product data for AI agents](https://www.anglera.com/blog/structure-product-data-for-ai-agents) ; this post is the attribute layer underneath it.


## What do the required feed fields actually buy you?


Eligibility, and nothing else. Google's merchant listing spec requires only` name` ,` image` , and a nested` offers` carrying` price` and` priceCurrency` . Everything a technical buyer cares about — identifiers, condition, availability, shipping, returns — sits in the[recommended tier](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing) . Required gets you into the room. It does not win the argument.


Run our contactor query against a catalog that stops at required. Every contactor in it is equally eligible and equally unrankable. The agent has a hundred products called "definite purpose contactor" and no basis for preferring any of them, so it does what any reasonable system does with an unresolvable comparison: it falls back on whichever source has the constraint data, which is usually the manufacturer's own site or a competitor who published the specs.


The uncomfortable version of this is that a fill-rate report can read 94% while the query still fails, because the 6% that's blank is the 6% that mattered.


## Which attributes actually carry the decision?


Five families, roughly in the order a technical buyer applies them: application envelope, ratings and tolerances, certifications, install and utility requirements, and companion items. Every one of them is a filter the buyer applies before price ever comes up, and every one of them is commonly missing.


For the contactor, the application envelope is the 480 V, the three phases, and the 30 amps. Those are not descriptive adjectives, they are a range test. A part rated 600 VAC max and 40 FLA passes; one rated 240 VAC does not. That test only runs if` coil voltage` ,` max rated voltage` ,` full load amps` ,` poles` , and` horsepower rating` exist as numeric fields with units stored separately from the number. Stored as the string "600VAC 40A 3P", they are invisible to a range query and about as useful as a scanned catalog page.


Ratings and tolerances behave the same way, one category over. A bearing has a bore, an OD, a width, a dynamic load rating and a speed limit. A valve has a pressure class, a temperature range, and a media compatibility list. In both cases the buyer's constraint is a comparison, not a keyword, and the comparison needs a number and a unit on both sides.


The reason these fields go missing is rarely that nobody knows them. They are printed on the cut sheet. They are just printed *in the cut sheet* , in a PDF table, and the PDF is linked rather than parsed — which is the single most common shape of the "we have the data" problem in distribution.


## How do you publish compatibility when the product isn't a car part?


As an envelope, not a table. The automotive aftermarket solved compatibility by agreeing on a shared vocabulary of vehicles, so a brake pad can enumerate every year, make, model and engine it fits. Almost no other category has that luxury, and trying to copy it produces a combinatorial mess.


Auto Care's PIES standard also handles the adjacent problem — equivalence rather than fitment — with a repeating interchange structure. Auto Care describes it plainly: when a product interchanges with several OEM or competitor parts,["you are looping the PIES™ element <PartInterchange> multiple times for each interchange you provide for the product"](https://www.autocare.org/detail-pages/blog/what-the-tech/2023/04/10/the-scoop-on-the-loop-in-pies) . One product, many published equivalents, each with a brand and a part number.


Outside automotive, the analogue is an envelope of ranges plus an explicit application list. A pump publishes flow, head, port size, seal material, and the fluids it is rated for. A luminaire publishes lumens, CCT, CRI, driver type, dimming protocol, and mounting. The buyer's real question is "will this work in my situation," and an envelope answers it for a situation nobody anticipated, which a fixed compatibility table never can.


Where you *do* hold cross-reference data — the competitor part your item replaces, the OEM number it interchanges with — publish it as a field rather than a hidden keyword block. That's a big enough subject that it gets its own treatment in[the identity spine for B2B catalogs](https://www.anglera.com/blog/mpn-identity-spine-b2b-catalogs) , and it connects directly to[part number cross-reference](https://www.anglera.com/glossary/part-number-cross-reference) as a discipline.


## Where do certifications go?


Into structured fields with an issuer and a number attached. Schema.org defines a[Certification](https://schema.org/Certification) type — "an official and authoritative statement about a subject" — and a` hasCertification` property that applies to` Product` . It carries` issuedBy` ,` certificationIdentification` ,` validIn` ,` validFrom` , and` expires` .


That shape is worth taking seriously rather than collapsing to a "UL Listed: Yes" checkbox. "UL listed" with a file number, an issuing body and a validity window is a verifiable claim. "UL listed" as a bare boolean in a description is a claim that any listing can make, including the ones that shouldn't. Systems that have to weigh competing product data discount unverifiable claims, and they should.


Back to the contractor. His "has to be UL listed" is a hard gate, and in safety-, code- or spec-driven categories it usually is: the NEMA rating on an enclosure, the AWWA approval on a waterworks fitting, the NSF listing on a foodservice component, the ANSI Z87 marking on eyewear. If it gates the purchase, it belongs in a field.


## What answers "what else do I need"?


An explicit product relationship. Schema.org gives you[isAccessoryOrSparePartFor](https://schema.org/Product) ("a pointer to another product (or multiple products) for which this product is an accessory or spare part") and` isConsumableFor` for exactly this, plus` isRelatedTo` and` isSimilarTo` for looser links.


The contactor's follow-on question is a coil voltage — 24 V, 120 V, or 208/240 V, depending on what the existing control circuit runs. A catalog that models the contactor family with coil voltage as a variant attribute answers it. A catalog that lists eleven separate contactor SKUs with the coil voltage buried in the title answers it by accident, at best.


This is the same mechanic behind attachment rate, incidentally. A "frequently bought together" carousel assembled client-side at render time helps a human and is invisible to a crawler that doesn't run JavaScript. The relationship expressed in the data helps both. The human-conversion side of that argument is in[the last inch](https://www.anglera.com/blog/last-inch-conversion-details) .


## Is a Q&A block on a product page still worth shipping?


Yes, but not for the reason it used to be. Google[removed FAQ rich results from Search starting May 7, 2026](https://developers.google.com/search/docs/appearance/structured-data/faqpage) , and` QAPage` markup is explicitly not for this: Google lists "an FAQ page written by the site itself with no way for users to submit alternative answers" and "a product page where users can submit multiple questions and answers on a single page" as[invalid use cases](https://developers.google.com/search/docs/appearance/structured-data/qapage) .


So the rich-result payoff is gone. The retrieval payoff isn't. A short block of buyer questions answered in plain server-rendered text — "Will this contactor work on a 208 V control circuit?", "Does it include the mounting bracket?", "What's the difference between this and the 40 A version?" — gives a retrieval system passages phrased the way buyers actually ask, sitting next to the structured attributes that support the answer. Write them as prose that stands alone without the surrounding page, because that is how a chunk gets retrieved.


The failure mode to avoid is padding: six generated questions per SKU that restate the spec table. That produces near-duplicate text across thousands of pages and helps nobody. Two or three real questions, drawn from what your inside-sales team actually gets asked, is the whole play.


## The contactor, before and after


Before After


Title DP Contactor 3P 40A Definite purpose contactor, 3-pole, 40 FLA, 600 VAC


Coil voltage (in title, sometimes) 24 VAC, 120 VAC, 208/240 VAC (variant attribute)


Max rated voltage — 600 V


Full load amps — 40 A


Horsepower rating — 10 HP @ 480 V, 3-phase


Certification "UL" in description UL Listed, file number, issuer, valid in US/CA


Auxiliary contacts — Field-installable, 2 available


Replaces — 3 OEM part numbers, published as fields


Nothing in the "after" column is exotic. All of it is on the manufacturer's cut sheet. The work is getting it out of the PDF, into fields with units, and onto the page in a form a machine can read — at the scale of a catalog rather than a SKU.


Sources:


- [Google Search Central: How to add merchant listing structured data](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)
- [Google Search Central: Mark up FAQs with structured data (deprecation notice)](https://developers.google.com/search/docs/appearance/structured-data/faqpage)
- [Google Search Central: Schema for Q&A pages (QAPage)](https://developers.google.com/search/docs/appearance/structured-data/qapage)
- [Schema.org: Certification](https://schema.org/Certification)
- [Schema.org: Product](https://schema.org/Product)
- [Auto Care Association: The scoop on the "loop" in PIES](https://www.autocare.org/detail-pages/blog/what-the-tech/2023/04/10/the-scoop-on-the-loop-in-pies)
