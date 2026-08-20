---
schema_version: "1.0.0"
document_id: "2b14cf86688e6f59d3be6db7f2c08ed2cb5218b17c0867979c61fad2afb33af6"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/structure-product-data-for-ai-agents"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T22:51:30.785703+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:cd69c89290d95f04884724b53c803dde9d28333a5b5cf838df49c9d99fd91c4f"
---

# How to structure product data for AI agents (the B2B version)

An agent asked to source "150-watt LED high bays for a 30-foot warehouse ceiling, DLC listed, forty of them, on the floor this week" does not browse. It decomposes the phrase into constraints, queries whatever catalogs it can reach, and discards every candidate that fails a filter. A SKU that genuinely meets that spec but keeps its wattage inside a supplier PDF fails in exactly the same way as a SKU that doesn't meet the spec at all. There is no partial credit for having the answer somewhere.


Most published guidance on structuring product data for agents was written for direct-to-consumer retail — sneakers, sizing, dietary claims, Google Shopping feeds. Very little of it survives contact with a distributor running 400,000 SKUs across 900 suppliers, where the[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) is frequently absent, the "product" is really a set of dimensional and electrical constraints, and stock is a per-branch number rather than a single national one. This is the version of that guide for people who sell bearings, breakers, valves, and fasteners.


The underlying behavior is worth taking seriously. Forrester's *State of Business Buying, 2026* reports that generative AI tools were the single most-cited meaningful interaction type for researching purchases — and that while 36% of buyers felt more confident in their decision for having used them,[20% said they were less confident because they hit unreliable or inaccurate information](https://www.digitalcommerce360.com/2026/01/22/forrester-b2b-buying-ai-2026/) . Some meaningful share of that inaccuracy is catalogs answering badly.


## What does an AI agent actually need from a product record?


It needs to answer four questions in sequence: is this the same part, is it the right kind of thing, does it satisfy the stated constraints, and can you actually deliver it. Each question is answered by a different layer of the record, and a null in an early layer means the later layers never get read.


The ordering matters more than any individual field. Teams invest in the layer they can see (description copy, images, a bullet list) and leave identity and specs thin. The agent works in the opposite direction: resolve identity, narrow by class, then filter on values. Good copy on an unidentifiable part gets dropped before anything reads it.


## Why is B2B product data harder to structure than a DTC catalog?


Because the volume is an order of magnitude larger, the source material is unstructured, and the attributes that decide the sale are the ones nobody typed into a field. A Shopify merchant with 2,000 SKUs and manufacturer-supplied content has a content problem. A distributor with 400,000 SKUs and a shared drive of supplier PDFs has an extraction problem.


Four differences do most of the damage. The identity spine is weaker: plenty of industrial SKUs never receive a manufacturer-assigned[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) , so the[MPN](https://www.anglera.com/glossary/mpn-manufacturer-part-number) and manufacturer name carry the whole load, and MPNs get mangled by dashes, spaces, and leading zeros as they move between the ERP, the PIM, and the web catalog.


The spec *is* the product. Nobody searches for a "great" pillow block bearing. They search for a two-bolt flange, 1-7/16 inch bore, cast iron housing, set screw locking. Those values are the identity in practice, and in most catalogs they live in a cut sheet rather than a field.


The commercial shape is messier: sold by the each, priced by the hundred, packed in a box of 50 inside a case of 500, with kit SKUs containing six different components. And availability is plural. "In stock" means nothing without a branch, and an agent quoting national stock against a job shipping out of one warehouse makes a promise that branch can't keep.


## Which attributes actually matter, and what goes wrong with each?


Below is the working set for an industrial or wholesale catalog. It is not a universal twelve-field list; the spec block changes by category. What doesn't change is the job each field does for the agent and the specific way each one tends to rot.


Field What the agent does with it What actually goes wrong


Manufacturer + MPN Resolves identity; dedupes you against everyone else selling the same part Dashes stripped in the ERP and kept in the PIM; house-brand name overwrites the actual manufacturer


GTIN, where one exists Matches to a known product record across feeds and marketplaces A case-level UPC pasted onto an each-level SKU, so quantity math silently breaks


Your SKU /` productID` Stable handle for reorder, history, and repeat citation Reassigned during an ERP migration or a warehouse re-rack


Classification code Narrows the candidate set before any spec comparison runs Filed two levels too shallow, or classified once at onboarding and never revisited


Spec attributes The literal filter conditions in the query Trapped in a PDF, or free-text:` 1/2 in` ,` 0.5"` , and` 12.7mm` treated as three different values


Unit of measure Computes real price and real quantity Price is per case, quantity is per each, and nothing in the record says so


Pack / case hierarchy Works out how many to order to hit forty units One flat "pack size" column doing the work of three levels


Kit / bundle components Decides whether one SKU satisfies a whole request The kit inherits one component's attributes, so a six-piece set shows a single thread size


Parent / child variants Presents one product with options instead of 40 orphan listings Every variant published as a top-level product with near-identical copy


Cross-reference / supersession Answers "what replaces this discontinued part" Lives in a sales rep's spreadsheet, not the catalog


Certifications Hard gate on regulated and spec'd work Written as marketing text — "UL listed" — with no standard number and no issuing body


Linked documents Grounds and verifies the claims you're making A PDF sitting on a CDN with nothing tying it to the SKU


Branch availability + lead time Decides whether you're recommendable for this week A single national number no branch can honor


Application / fitment content Answers "will this work for my situation" The highest-value content in the catalog and the least likely to exist


That last row is where the consumer playbook and the B2B one diverge hardest. The DTC equivalent of enrichment is "does it run small" and "is it dishwasher safe." The B2B equivalent is *what does this fit, what is it rated for, what supersedes it, and what else do I need to install it* . Same instinct, entirely different content, and it is almost never in the supplier's feed. Which of those actually swing a recommendation, and how to publish compatibility and certifications so a machine can use them, is worked through in[the attributes that decide the recommendation](https://www.anglera.com/blog/attributes-that-decide-the-recommendation) .


## What identity should a record carry when there's no GTIN?


Manufacturer plus MPN, treated as a single composite key and normalized to one canonical form everywhere. Google's product data guidance is explicit that GTIN is strongly recommended for products the manufacturer assigned one to, and that where a product genuinely lacks one you should[submit the brand and MPN attributes instead](https://support.google.com/merchants/answer/6324461) .


Normalization is the whole job. Decide once whether` HB-150-50K-U` or` HB150 50K U` is canonical, store the raw supplier string alongside it, and index both so a buyer typing either one still lands on the SKU. Add the[cross-reference](https://www.anglera.com/glossary/part-number-cross-reference) chain — competitor equivalents, superseded predecessors, the part that replaced it — as structured relations rather than free text in a description.` isSimilarTo` and` isAccessoryOrSparePartFor` exist in schema.org for exactly this, and an agent trying to answer "what replaces a discontinued XYZ-40" has nothing else to work with.


The identifier side gets deeper treatment in[GTIN and UPC hygiene](https://www.anglera.com/blog/gtin-upc-identifier-hygiene) and[identifiers in structured data](https://www.anglera.com/blog/identifiers-in-structured-data) . The short version: consistency beats coverage. An MPN that's identical in every system is worth more than a GTIN you sourced from three different places. For the full argument, including what GS1's own allocation rules say about when a GTIN is even appropriate and how to publish a supersession chain a machine can follow, see[the identity spine](https://www.anglera.com/blog/mpn-identity-spine-b2b-catalogs) .


## How should classification work when a shopping category isn't enough?


Run one internal spine and map outward. A retail category tree tells an agent the product is a light fixture.[UNSPSC](https://www.anglera.com/glossary/unspsc-classification) tells a procurement system how to code the spend.[ETIM](https://www.anglera.com/glossary/etim-classification) tells a filter engine which features that class is even supposed to have, and in which units.


[UNSPSC](https://www.anglera.com/glossary/unspsc-classification) is a four-level hierarchy (segment, family, class, commodity) expressed as an[eight-digit code](https://www.commerce.gov/oam/resources/united-nations-standard-products-and-services-codes-unspsc) , which makes it good for spend rollups and useless for filtering.[ETIM](https://www.anglera.com/glossary/etim-classification) does the opposite: every class carries a defined feature set with types (alphanumeric, logical, numeric, range) and required units.[ETIM 10.0](https://www.etim-international.com/new-release-etim-10-0-available/) , released in December 2024, added 119 new classes and introduced feature groups so features can be sectioned into material, electrical, and dimensional blocks.[eCl@ss](https://www.anglera.com/glossary/eclass-classification) plays a similar role in European industrial supply.


So classify against whichever standard governs your category's attribute model, usually ETIM or[eCl@ss](https://www.anglera.com/glossary/eclass-classification) in electrical, HVAC, plumbing and industrial goods, then derive[Google Product Category](https://www.anglera.com/glossary/google-product-category) and UNSPSC from it. Maintaining three hand-curated trees is how they drift apart. More on that in[category taxonomy that scales](https://www.anglera.com/blog/category-taxonomy-that-scales) .


## How do you express UOM, packs, kits, and variants without lying to the agent?


Split the selling unit from the pack, and say which is which. Almost every wrong-quantity and wrong-price failure in agentic B2B traces back to one column trying to carry two meanings.


In schema.org, quantities belong in a` QuantitativeValue` with a` unitCode` drawn from the[UN/CEFACT common code list](https://schema.org/QuantitativeValue) — three-character codes like` WTT` for watt,` FOT` for foot,` EA` for each. Use` unitText` only when no standard code exists. In shopping feeds, Google keeps these concepts deliberately separate:[unit_pricing_measure](https://support.google.com/merchants/answer/6324455) carries the measure and dimension so a cost-per-unit can be computed,[multipack](https://support.google.com/merchants/answer/6324488) means identical items you grouped yourself, and[is_bundle](https://support.google.com/merchants/answer/6324449) means different items sold together for one price. Getting[kit and bundle SKUs](https://www.anglera.com/glossary/kit-and-bundle-sku) wrong is worse than leaving them out, because the agent will confidently compute the wrong total.


For[parent/child variants](https://www.anglera.com/glossary/parent-child-product-variants) , Google's structured data supports a` ProductGroup` with nested` hasVariant` products, a` productGroupID` acting as the parent SKU, and` variesBy` naming the differentiating properties by their full schema.org URL ([product variant structured data](https://developers.google.com/search/docs/appearance/structured-data/product-variants) ). Forty near-identical listings compete with each other; one group with forty options doesn't.


## What does agent-ready markup look like for an industrial SKU?


Like a spec sheet that a parser can read. No GTIN, brand and MPN carrying identity, classification codes as typed properties, every spec value with a unit attached, and certifications naming their issuer.


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "150W LED High Bay, 5000K, 120-277V, 0-10V Dimming",
"sku": "LGT-HB150-50K",
"mpn": "HB-150-50K-U",
"brand": { "@type": "Brand", "name": "Northmark Lighting" },
"manufacturer": { "@type": "Organization", "name": "Northmark Lighting" },
"description": "Round LED high bay for 25-35 ft mounting heights in warehouse and manufacturing space. Replaces 400W metal halide.",
"additionalProperty": [
{ "@type": "PropertyValue", "propertyID": "UNSPSC", "name": "UNSPSC", "value": "39111524" },
{ "@type": "PropertyValue", "propertyID": "ETIM", "name": "ETIM class", "value": "EC002892" },
{ "@type": "PropertyValue", "name": "Input power", "value": 150, "unitCode": "WTT" },
{ "@type": "PropertyValue", "name": "Luminous flux", "value": 21000, "unitCode": "LUM" },
{ "@type": "PropertyValue", "name": "Correlated color temperature", "value": 5000, "unitCode": "KEL" },
{ "@type": "PropertyValue", "name": "Input voltage", "value": "120-277 V AC" },
{ "@type": "PropertyValue", "name": "Dimming protocol", "value": "0-10V" },
{ "@type": "PropertyValue", "name": "Mounting height", "minValue": 25, "maxValue": 35, "unitCode": "FOT" },
{ "@type": "PropertyValue", "name": "Ingress protection", "value": "IP65" }
],
"hasCertification": [
{
"@type": "Certification",
"name": "UL 1598",
"issuedBy": { "@type": "Organization", "name": "UL Solutions" }
},
{
"@type": "Certification",
"name": "DLC Premium",
"issuedBy": { "@type": "Organization", "name": "DesignLights Consortium" }
}
],
"isSimilarTo": { "@type": "Product", "mpn": "HB-150-40K-U", "name": "150W LED High Bay, 4000K" },
"subjectOf": {
"@type": "DigitalDocument",
"name": "Specification sheet",
"url": "https://example.com/docs/HB-150-50K-U.pdf"
},
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "184.00",
"eligibleQuantity": { "@type": "QuantitativeValue", "value": 1, "unitCode": "EA" },
"availability": "https://schema.org/InStock",
"inventoryLevel": { "@type": "QuantitativeValue", "value": 62, "unitCode": "EA" },
"availableAtOrFrom": { "@type": "Place", "name": "Houston Branch" }
}
}


```


Two honest caveats. Google's Product rich-result documentation doesn't consume` additionalProperty` , so don't expect a spec table in a search snippet from this. It's still valid schema.org, and still the cleanest way to hand a model a typed, unit-bearing fact instead of a sentence it has to parse. And the property names need governing: a hundred SKUs using "Input power," "Wattage," and "Power (W)" for one concept reproduce the free-text problem inside your structured data.


Delivery is not negotiable. Google runs crawl, render, and index as distinct phases and will execute your JavaScript before indexing ([JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ). Most AI crawlers won't: analysis of large-scale crawl logs found[no evidence that GPTBot executes JavaScript](https://vercel.com/blog/the-rise-of-the-ai-crawler) . Injected client-side, this block does not exist. See[SSR vs CSR vs pre-rendering](https://www.anglera.com/blog/ssr-vs-csr-prerender-agent-readable) for the rendering choice and[validating agent-readable product data](https://www.anglera.com/blog/validate-agent-readable-product-data) for how to check what you're shipping.


## Where do the agentic protocols fit?


Three separate layers, often conflated. One handles checkout, one handles the whole merchant surface, one handles how an agent reaches tools and data at all. None of them fixes a thin product record; they just determine who gets to read it.


The[Agentic Commerce Protocol](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) , maintained by OpenAI and Stripe as founding maintainers, standardizes the buyer-agent-merchant checkout interaction. It's still labeled beta, with the current stable spec dated 2026-04-17 and the repo pointing toward neutral foundation stewardship as the ecosystem matures. OpenAI separately publishes a[product feed specification](https://developers.openai.com/commerce/specs/spec) covering how merchants hand over structured product data, including a hard requirement that product and variant identifiers stay stable over time. That's exactly the rule an ERP migration breaks.


Google's[Universal Commerce Protocol](https://ucp.dev/) , published on 11 January 2026 with Shopify, Etsy, Wayfair, Target, Walmart and twenty-odd other partners, is broader. Businesses advertise capabilities at a` /.well-known/ucp` discovery endpoint, and the protocol covers catalog search and lookup, cart building, identity linking, checkout, and order management ([under the hood](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/) ). It runs on REST and JSON-RPC with AP2, A2A, and MCP support built in, and has begun extending past retail into lodging and food.


[Model Context Protocol](https://www.anglera.com/glossary/model-context-protocol-mcp) sits underneath both, connecting agents to tools and data sources. Anthropic donated it in December 2025 to the Agentic AI Foundation,[a directed fund under the Linux Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) co-founded with Block and OpenAI, so it now sits under neutral governance rather than one vendor's roadmap.


Catalog lookup being a named, specified capability is the part that matters for a distributor: the quality of your product record is now directly queryable. Spend the integration effort on the record, not the wire format. Wire formats keep changing. The record doesn't.


Which of the four is worth your integration budget is a narrower question than it looks, and for most industrial catalogs the honest answer is not the consumer shopping feed.[Feeds, checkout, and live data](https://www.anglera.com/blog/agent-feeds-protocols-what-to-ship-first) takes each one apart and ranks them.


## Do structured Q&A blocks and llms.txt actually help?


Q&A blocks help a lot;[llms.txt](https://www.anglera.com/glossary/llms-txt) currently doesn't. The distinction is worth drawing sharply, because one of these is cheap busywork and the other is the highest-leverage content you can add to a technical PDP.


Buyers ask application questions, not attribute questions. "Will this pump handle 180°F glycol?" "What size breaker does this need?" "Does this replace a 400W metal halide?" Those map to facts already scattered across your spec sheet, your ETIM features, and your reps' heads. Written as explicit question-and-answer pairs with self-contained answers, they give a retrieval system a passage it can lift wholesale, and give your own site search something to match. Same move as[answer engine optimization](https://www.anglera.com/blog/answer-engine-optimization) , applied at SKU level.


[llms.txt](https://www.anglera.com/glossary/llms-txt) is a different story, and the evidence has gotten clearer. Ahrefs analyzed 137,210 domains with traffic in May 2026: 28% published an llms.txt file, but[97% of those files got zero traffic that month](https://ahrefs.com/blog/llmstxt-study/) , with retrieval bots like PerplexityBot and OAI-SearchBot accounting for about 1.1% of the requests that did land. It costs an afternoon and it does no harm. It just isn't the reason your SKUs aren't being cited. Fix the[fill rate](https://www.anglera.com/glossary/attribute-fill-rate) first.


## In what order should you actually do this?


Narrowest-first, in a loop, with revenue as the scoping rule. The mistake is trying to raise every attribute on every SKU to the same standard simultaneously, which produces a two-year program that reports percentage complete and never ships a readable page.


Audit before you enrich. Pull the actual counts: how many SKUs carry a manufacturer *and* a normalized MPN, how many have a classification code below the family level, how many have values for the five attributes buyers filter on in that category. The missing data is almost never evenly distributed. It concentrates in a handful of suppliers and categories, and that concentration tells you where to start. You don't need to scan the whole catalog to find that out either —[auditing on a stratified sample](https://www.anglera.com/blog/catalog-readiness-audit-before-you-spend) gets you a fundable answer in days rather than a quarter.


Fix the identity spine next, at full catalog breadth, because every later join depends on it. Normalize MPNs, reconcile manufacturers to a controlled list, freeze SKU IDs, capture cross-references. It's the one layer worth completing everywhere before going deep anywhere.


Depth comes category by category, ordered by revenue or quote volume. Define ten to fifteen attributes per category rather than sixty, starting from the ETIM or eCl@ss feature set instead of inventing a vocabulary. High fill rate on the right fifteen beats partial coverage of sixty, and the[fill rate versus accuracy](https://www.anglera.com/blog/fill-rate-vs-accuracy) trap is real: a populated field with a wrong value is worse than a null.


Rendering and distribution only pay off once that exists. Server-rendered JSON-LD, spec values visible on the page as well as in the markup, Q&A blocks, documents joined to the SKU, and every feed reading from the same enriched record instead of a separate export nobody maintains. Shopping feeds have somewhere to put specs: Google's[product_detail](https://support.google.com/merchants/answer/9218260) attribute takes` section_name : attribute_name : attribute_value` triples for technical details no other field covers.


Then keep it. That step gets skipped and it decides whether the rest holds, because suppliers reissue spec sheets, supersede parts, and change pack quantities without telling anyone.


A first category live in weeks is realistic, and that's the shape of implementation we work to. Whole-catalog coverage is not a project with an end date. It's a standing operation, and pretending otherwise is how these programs die in month nine.


## What actually blocks distributors from getting there?


Rarely the technology. Almost always the source material, the ownership, and the fact that the catalog is downstream of systems that were never designed to hold attributes.


The source data is unstructured on purpose. Suppliers publish PDFs because PDFs are what their engineering team already produces. One 40-page cut sheet may cover twelve SKUs, with the distinguishing values sitting in a table cell across three merged columns. That's an extraction problem, and it's why so many enrichment initiatives stall right after the pilot: the demo used the fifty products with clean data.


The ERP has no attribute model. Item master gives you a description field, a class code, a UOM, and pricing. Everything a buyer filters on has to live somewhere else, and if that somewhere else isn't wired back into the storefront and the feed, it becomes a second version of the truth ([more on that here](https://www.anglera.com/blog/erp-migration-attributes) ).


Nobody owns it. Merchandising owns assortment, ecommerce owns the site, IT owns the integrations, and product data falls in the gap between them. The catalogs that get this right have a named owner with a budget, which sounds trivial and is the best single predictor of whether anything ships.


Then there's the arithmetic. Fifteen attributes across 400,000 SKUs is six million values; at a generous thirty seconds each that's roughly fifty thousand hours of typing, before anything supersedes and before anyone checks accuracy. That number is why "we'll get to it" becomes a permanent condition, and why the gaps in[five product-data gaps that get your SKUs filtered out](https://www.anglera.com/blog/5-gaps-that-filter-you-out) keep reappearing in catalog after catalog.


---


Anglera does the completing. Your PIM, ERP, or commerce platform stores the record; we fill the missing attributes at catalog scale, pulling values out of supplier spec sheets, cut sheets, and manufacturer sources, normalizing them against your schema, and writing them back into the system you already run. Typical implementation is 30 days. The rendering and feed work stays with your team, where it belongs.


Sources:


- [Digital Commerce 360: Forrester — B2B buying groups expand as they question AI](https://www.digitalcommerce360.com/2026/01/22/forrester-b2b-buying-ai-2026/)
- [Google Merchant Center: GTIN attribute](https://support.google.com/merchants/answer/6324461)
- [Google Merchant Center: product detail, unit pricing measure, multipack, bundle](https://support.google.com/merchants/answer/9218260)
- [Google Search Central: product variant structured data](https://developers.google.com/search/docs/appearance/structured-data/product-variants)
- [Google Search Central: JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics)
- [Schema.org: QuantitativeValue and the UN/CEFACT common code](https://schema.org/QuantitativeValue)
- [ETIM International: ETIM 10.0 release](https://www.etim-international.com/new-release-etim-10-0-available/)
- [US Department of Commerce: UNSPSC structure](https://www.commerce.gov/oam/resources/united-nations-standard-products-and-services-codes-unspsc)
- [Agentic Commerce Protocol repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol)
- [OpenAI: product feed specification](https://developers.openai.com/commerce/specs/spec)
- [Universal Commerce Protocol](https://ucp.dev/) and[Google Developers Blog: under the hood](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)
- [Linux Foundation: formation of the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)
- [Vercel: the rise of the AI crawler](https://vercel.com/blog/the-rise-of-the-ai-crawler)
- [Ahrefs: we analyzed 137K sites — 97% of llms.txt files never get read](https://ahrefs.com/blog/llmstxt-study/)
