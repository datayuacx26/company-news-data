---
schema_version: "1.0.0"
document_id: "6791434333753c20acbdfe5074a46ccfc5c4bf6ececf2c0750a9c8bc5fbfd549"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/mpn-identity-spine-b2b-catalogs"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T22:51:30.785703+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:35ed92182f06263f01ff478a36c7045854cd6d1bd222a91bfe1bbdba3d253780"
---

# The identity spine: why MPN and manufacturer, not GTIN, is the real primary key in B2B catalogs

A maintenance planner emails a photo of a nameplate. It came off a rotary screw compressor installed in 2011, and the number stamped on the inlet filter housing reads` E1-4230` . He wants two of them by Friday.


Your system returns nothing. The manufacturer superseded` E1-4230` to` E1-4230A` in 2018, and in 2023 rolled the element into a service kit with its own number. Somewhere in your ERP all three exist as separate records with no relationship between them, and the customer-facing catalog only carries the newest one. Nothing in that story involves a barcode, which is the first clue about what a B2B catalog's primary key actually is.


This is the identifier layer beneath[how to structure product data for AI agents](https://www.anglera.com/blog/structure-product-data-for-ai-agents) . Get it wrong and everything above it — attributes, feeds, protocols — is attached to a record nothing else in the world can point at.


## Why isn't the GTIN the primary key here?


Because in industrial distribution most items never get one, and you are not allowed to invent one. The[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) is allocated by the party that warrants the product's declarations, which is the brand owner, and the number space is licensed through GS1 company prefixes. A distributor sits downstream of both facts.


GS1's General Specifications are blunt about this. Section 4.2.3 states that["no downstream party (e.g., distributor, wholesaler, importer, merchant) may assign a different GTIN to a trade item that already has a GTIN"](https://documents.gs1us.org/adobe/assets/deliver/urn:aaid:aem:afbf55ad-0151-4a0c-8454-d494c0dc9527/GS1-General-Specifications.pdf) , and that allocation "is the responsibility of the party that warrants the trade item declarations, known as the[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) allocator." GS1's own support portal puts it more plainly still:[the brand owner "is normally responsible for the allocation"](https://support.gs1.org/support/solutions/articles/43000734414-who-is-responsible-for-numbering-trade-items-) .


Google says the same thing from the other end. Its Merchant Center guidance on the GTIN attribute reads:["Only provide a GTIN if you are sure it is correct. When in doubt do not provide a GTIN (for example, do not guess or make up a value)"](https://support.google.com/merchants/answer/6324461) , and it acknowledges directly that "some products don't have a GTIN assigned, and so you don't need to submit one."


Which means the recurring instinct to "just generate GTINs for the catalog so the feed validates" is not a shortcut. It is fabricating identity for someone else's product, and it will eventually collide with a real allocation.


## What is the key, then?


Brand plus[manufacturer part number](https://www.anglera.com/glossary/mpn-manufacturer-part-number) . That pair is the sanctioned fallback in Google's identifier hierarchy: MPN becomes[required specifically when a product has no manufacturer-assigned GTIN](https://support.google.com/merchants/answer/7052112) , and Google's overview states that["to help identify your products without a GTIN, you can use the MPN and brand attributes"](https://support.google.com/merchants/answer/160161) . Schema.org carries` mpn` ,` brand` , and` manufacturer` as first-class` Product` properties.


The pair has one failure mode, and it is almost always the brand side rather than the number side.` E1-4230` is unambiguous once you know whose part it is. But if your catalog holds that manufacturer as "Ingersoll Rand" in one record, "Ingersoll-Rand" in another, and "IR" in a third, the pair resolves to three different manufacturers and matching quietly fails. Manufacturer belongs in a[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) with an ID, not in a free-text column that inside sales can type into.


The other half of the discipline is not overloading MPN with your own numbering. Google is explicit:["Use the MPN assigned by the manufacturer. Unless you're the manufacturer, don't use a value that you've created."](https://support.google.com/merchants/answer/6324482) Your internal SKU has a home. It is the` sku` field, and it identifies your record, not the product.


## What happens when the manufacturer supersedes the part?


The identity moves, and if you have modeled part numbers as immutable strings, your catalog silently stops tracking the item. Supersession is not an edge case in distribution — it is the normal lifecycle of anything with a service history, and it is where most identifier hygiene actually breaks.


GS1's GTIN Management Standard frames these as replacement products:["Changes to existing products are considered 'replacement products' (the previous version will no longer exist once the replacement product has flowed through, as determined by the brand owner)."](https://ref.gs1.org/standards/gtin-management/) The standard then enumerates what forces a new number. A new product, obviously. But also a change to declared net content, a change of more than 20% to a physical dimension or gross weight, a change to the primary brand, and — the one that catches industrial parts constantly — adding or removing a certification mark.


That last rule is why supersession events cluster around regulatory cycles. When a component picks up or loses a UL or CE mark, the manufacturer is required to issue a new number, which means a wave of superseded parts arrives in your feed with no obvious trigger. The element in our compressor story went from` E1-4230` to` E1-4230A` for exactly this kind of reason.


## Why does the old GTIN keep mattering after it's retired?


Because it does not get recycled, and because the field still refers to it. GS1's non-reuse rule is unambiguous:["An allocated GTIN SHALL NOT be reallocated to another trade item."](https://documents.gs1us.org/adobe/assets/deliver/urn:aaid:aem:afbf55ad-0151-4a0c-8454-d494c0dc9527/GS1-General-Specifications.pdf) The exceptions are narrow — numbers never published outside your own systems, and withdrawn products reintroduced with no qualifying changes.


Worth flagging, because it is one of the most-repeated stale facts in product-data writing: the old forty-eight-month GTIN reuse window is deprecated. It has been since 1 January 2019, and it now survives in the General Specifications only in a section explicitly labelled as deprecated. Plenty of guidance published since then still repeats it as live practice.


GS1's own rationale for permanence is the useful part for a distributor. The specification notes that data associated with the original GTIN gets used by trading partners "for statistical analysis or service records, which may continue long after the original trade item was last supplied," and gives the example of steel beams sitting in storage for years before entering the supply chain. That is precisely our compressor. The nameplate is not going to update itself because the manufacturer issued a new number in 2018.


So retired identifiers are permanent search keys. Treat them as data to keep, not data to clean up.


## How do you publish a chain a machine can follow?


Two moves. Keep the retired numbers as fields on the current product, and make sure a query for a retired number resolves to a live page rather than a 404. Neither is exotic. Both are skipped constantly, because the retired numbers usually live in the ERP and never make it to the catalog.


The automotive aftermarket is the one corner of distribution that formalized this. Auto Care's PIES standard —["the aftermarket industry data standard for the management and communication of product information"](https://www.autocare.org/data-standards/product-information-exchange-standard-%28pies%29) — includes interchanges among the data it transmits, and handles the one-to-many shape properly. Auto Care describes it directly: when a product interchanges with several OEM or competitor parts,["you are looping the PIES™ element <PartInterchange> multiple times for each interchange you provide for the product"](https://www.autocare.org/detail-pages/blog/what-the-tech/2023/04/10/the-scoop-on-the-loop-in-pies) . Stibo's public implementation docs describe the same segment as relaying["interchange data for the PIES Item in relation to alternative Brand Owner Part Numbers"](https://doc.stibosystems.com/doc/version/latest/web/content/solenabl/auto/reference_guide/importers/standard_ac/pies/sample_autocare_pies_file_structure.html) , each entry carrying a brand identifier alongside the part number.


Note the shape: brand plus number, repeated. Not a comma-separated string in a notes field.


Two things worth separating that most catalogs merge. Supersession is vertical — same manufacturer, this part replaced that one, and only the current number is in production.[Cross-reference](https://www.anglera.com/glossary/part-number-cross-reference) is horizontal — a different manufacturer's part that does the same job. Collapsing both into one "related items" bucket is how a customer ends up quoted a competitor's discontinued number as a current replacement.


Schema.org will not solve this for you. It offers` isSimilarTo` ,` isAccessoryOrSparePartFor` , and` isConsumableFor` , and none of them mean "replaced by." There is no supersession property. That is a real gap in the vocabulary, and the practical workaround is a named attribute on the product carrying the retired numbers, published as text a crawler can read, plus` isSimilarTo` for genuine equivalents.


## When should you go source a GTIN?


When a channel you actually sell through requires one, and not otherwise.[GDSN](https://www.anglera.com/glossary/gdsn-global-data-synchronization-network) publication, most grocery and retail POS integrations, and some marketplace listings are built on GS1 keys and will reject you without them. Industrial and MRO channels largely are not.


If the manufacturer assigned a GTIN, get it from the manufacturer or from GS1's registry — Google notes that products with an assigned GTIN submitted without one["may have limited visibility"](https://support.google.com/merchants/answer/160161) . If no GTIN exists, use brand and MPN and move on. If a product genuinely has no identifiers at all — a fabricated assembly, a cut-to-length item, a private-label good you manufacture — Google's` identifier_exists` attribute exists to say so honestly, and self-assignment is permitted in the MPN field only when you are the manufacturer.


One thing that is not a substitute: classification.[UNSPSC](https://www.anglera.com/glossary/unspsc-classification) ,[ETIM](https://www.anglera.com/glossary/etim-classification) , and[eCl@ss](https://www.anglera.com/glossary/eclass-classification) codes answer what kind of thing a product is, and by design every competitor in the class shares the code. ETIM says so about its own model, which is[not a final product in itself but a structure for "standardized (technical) product data exchange between parties"](https://www.etim-international.com/classification/) . Classification makes a catalog filterable. Identity makes it matchable. You need both, and one will not stand in for the other.


## What can an agent do without a GTIN?


More than the panic suggests. Given a clean manufacturer name and a correct MPN, a retrieval system can identify the part, line it up against the manufacturer's own datasheet, compare it on attributes, and follow a published cross-reference to an equivalent.[Product matching](https://www.anglera.com/glossary/product-matching) on brand plus part number is how technical buyers have always worked, and it is how a well-built agent works too.


What it cannot do without a GTIN is match your listing to the same physical item at another seller with certainty, or clear the identifier gates on channels that are built on GS1 keys. Those are real limits and worth naming for your own team. They are also, for most industrial distributors, not the constraint that is costing money this quarter.


The thing costing money this quarter is that` E1-4230` returns nothing.


Sources:


- [GS1 General Specifications (Release 25.0) — GTIN allocation and non-reuse](https://documents.gs1us.org/adobe/assets/deliver/urn:aaid:aem:afbf55ad-0151-4a0c-8454-d494c0dc9527/GS1-General-Specifications.pdf)
- [GS1 GTIN Management Standard](https://ref.gs1.org/standards/gtin-management/)
- [GS1: Who is responsible for numbering trade items?](https://support.gs1.org/support/solutions/articles/43000734414-who-is-responsible-for-numbering-trade-items-)
- [Google Merchant Center: GTIN attribute](https://support.google.com/merchants/answer/6324461)
- [Google Merchant Center: MPN attribute](https://support.google.com/merchants/answer/6324482)
- [Google Merchant Center: About unique product identifiers](https://support.google.com/merchants/answer/160161)
- [Auto Care Association: Product Information Exchange Standard (PIES)](https://www.autocare.org/data-standards/product-information-exchange-standard-%28pies%29)
- [ETIM International: Classification](https://www.etim-international.com/classification/)
