---
schema_version: "1.0.0"
document_id: "a2ddbf464a949496338fe10041bba5fb8e8a0755544b208ac4d72fbfeeb7c761"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/waterworks-attributes"
published_at: "2026-05-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:a0eb6f4ef255107b723acb4e3ce5e81b3ae0c85ba81961ce207ba5dbbb18fea3"
---

# Building an attribute schema for Waterworks & Utility that buyers and AI can actually use

A resilient-wedge gate valve isn't a commodity part with a size and a price. It's a bundle of a dozen discrete engineering facts, and a buyer's filter (or an AI answer engine parsing a spec question) checks most of them before the SKU is even eligible to be shown. Get the schema right and the part is findable. Leave half the fields blank, the way most manufacturer feeds do, and the valve effectively doesn't exist for anyone searching by spec instead of by part number.


## Why waterworks can't use a generic attribute template


Most e-commerce attribute thinking comes from categories where "size" and "color" cover most of the filtering. Waterworks doesn't work that way. A` 6-inch` gate valve has a valve design, a body material, an end connection on each side, a pressure class, a stem type, an operating mechanism, a coating system, a seat material, a standard compliance citation, and a certification list — and a municipal engineer or contractor is typically filtering on six or seven of those at once, not browsing.


That filtering is almost always "and" logic, not "or." A buyer searching for an` AWWA C509` -compliant, mechanical-joint, non-rising-stem valve isn't interested in near matches. If the pressure class field is blank, the SKU drops out of that result set entirely — it doesn't rank lower, it disappears. The same logic applies to an AI answer engine: if it can't confirm a field against the query, it moves to a competitor's SKU that made the match easy, which is increasingly the mechanic B2B teams are optimizing for under the banner of[generative engine visibility](https://www.kensium.com/blog/b2b-ecommerce-strategy-generative-engine-optimization-geo) .


## The schema a gate valve actually needs


For resilient-wedge and resilient-seated gate valves specifically — the workhorse isolation valve in most water distribution systems — the attribute set that gates a filter or an AI match looks like this:


Attribute Typical values Why it gates search


Nominal size` 4 in` through` 48 in` , with` DN` metric equivalent Primary filter on every valve page


Valve design Resilient wedge, double disc, solid wedge Buyers and specs distinguish these explicitly; not interchangeable


Body / bonnet material Ductile iron (` ASTM A536` ), gray iron Ductile iron is the de facto spec on most municipal jobs


End connections Mechanical joint (` MJ` ), flanged (` FLG` ), push-on Each end can differ —` MJ x FLG` is common and must be captured per side


Pressure class` Class 150` ,` Class 250` ,` psi` working pressure Determines legal use on a given main; a hard filter for municipal buyers


Stem type Non-rising stem (` NRS` ), rising stem (` RS` ),` OS&Y`` NRS` is standard for buried service;` OS&Y` is required where stem position must be visually verified


Operating mechanism / direction to open` 2-inch` square nut or handwheel; open left or open right Buried valves take a nut and a valve key; direction to open varies by utility standard


Interior / exterior coating Fusion-bonded epoxy per` AWWA C550` Corrosion protection, increasingly specified explicitly


Wedge / seat material` EPDM` or` NBR` encapsulation Governs compatibility with water chemistry and disinfectant residual


Standard compliance` AWWA C509` ,` AWWA C515` Buyers cite the standard directly in RFQs; the two are not interchangeable


Certifications` NSF/ANSI 61` ,` NSF/ANSI 372` (lead-free),` UL` /` FM` listing Potable contact requires` NSF/ANSI 61` ; fire service requires` UL` /` FM` , a qualification most feeds omit


Most manufacturer flat files capture size, end connection, and maybe pressure class. Stem type, standard compliance, and the UL/FM distinction are the fields that consistently go missing — exactly the fields a spec-driven buyer, or a model parsing a spec question, treats as non-negotiable.


## Before and after: one resilient-wedge gate valve


Here's a raw manufacturer feed row for an 8-inch resilient-wedge gate valve, next to what it needs to look like to survive a filtered search and be legible to an answer engine.


**Before (raw supplier feed):**


> "8IN RW GATE VLV FLGXMJ OSY 250# EPOXY UL/FM"


That string is dense with real information, but it's compressed into abbreviations with no labels. A filter can't parse` OSY` as "outside screw and yoke, stem type," or` 250#` as "Class 250 pressure rating," and neither can a language model asked to confirm a spec match.


**After (enriched attribute set):**


Attribute Value


Product type Resilient-wedge gate valve


Nominal size` 8 in` (` DN200` )


End connections Flanged x mechanical joint


Pressure class` Class 250` (` 250 psi` working pressure)


Stem type Outside screw and yoke (` OS&Y` ), rising stem


Operating mechanism Handwheel


Body material Ductile iron,` ASTM A536`


Wedge / seat Ductile iron wedge,` EPDM` -encapsulated


Coating Fusion-bonded epoxy,` AWWA C550`


Standard compliance` AWWA C509`


Certifications` UL` and` FM` listed for fire protection service,` NSF/ANSI 61`


Same physical part, but only one version answers a filter for "OS&Y, Class 250, UL/FM listed, flanged by mechanical joint." The other version is real inventory that never surfaces, because the data never stated the claim in a field a machine can check.


**Ask an answer engine:** a fire protection contractor typing "8-inch OS&Y resilient wedge gate valve, UL and FM listed, flanged by mechanical joint" is describing that table row field by field. If` OS&Y` , the` UL` /` FM` listing, and the split end connections only live inside a compressed ERP string or a scanned cut sheet, most retrieval systems can't verify the match confidently enough to cite it. Pressure-rating and flange-class terminology is confusable enough on its own that ambiguity here is a spec risk, not just a search problem ([Mueller Systems on flanged valve pressure rating standards](https://muellersystems.com/news/standards-clarification-pressure-rating-standards-for-flanged-valves/) ).


## Structuring the schema so it holds up


A few discipline points make this schema durable rather than a one-time cleanup:


- Split` MJ x FLG` -style compound strings into separate end-one and end-two fields, since a valve can have different connections on each side.
- Keep coating, wedge material, and body material as distinct fields rather than one "construction" blob; each answers a different filter or compliance check.
- Treat` AWWA C509` and` C515` as separate, explicit values, not synonyms, since specs often cite one and not the other.
- Carry certification as a list, not a single flag —` NSF/ANSI 61` ,` NSF/ANSI 372` , and` UL` /` FM` are independent qualifications a valve can hold in any combination, which is exactly why NSF maintains a[searchable listing of certified components](https://info.nsf.org/certified/pwscomponents/index.asp?standard=061) rather than a single yes/no badge.


Getting this right at scale is a data problem before it's a search problem. Your PIM, or a flat file if that's what you've got, is where these values should live. Anglera's job is scoring each SKU against a schema like the one above, gap-filling the fields manufacturer feeds routinely drop by extracting from real supplier documentation, and keeping values current as specs change. It plugs into whatever system already runs the catalog, live in weeks rather than a multi-year integration — so a resilient-wedge gate valve gets found because the data described it correctly, not lost because a filter or a model couldn't tell an` OS&Y` from an` NRS` .
