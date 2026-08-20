---
schema_version: "1.0.0"
document_id: "c9a41ab09c9862e2598c6950e34bc32adadf8a264ab81934afcba11380de4cc3"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/nobody-searches-your-category-name"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T17:36:10.661019+00:00"
fetched_at: "2026-07-31T17:36:17.979173+00:00"
content_hash: "sha256:f1248140d5d74d868d2db66e2ab4259a873e7d392cae3b92b6772318273d2972"
---

# Nobody searches for the name you gave the category

A distributor I spoke with had a category called **Fluid Handling — Rotary** .


Their buyers searched for "pump." Not once, not occasionally. It was the top query on their site, and it returned a page of results assembled from whatever the search engine could scrape out of product titles, ranked by nothing in particular.


Nobody had done anything wrong. **Fluid Handling — Rotary** is a perfectly reasonable name. It matches how the ERP is structured, how the buying group organises its content, and how the supplier's own catalog is laid out. It is precise, it is defensible, and it appears in exactly zero customer searches.


## Catalogs are written in the language of the people who stock them


This is close to universal, and it isn't a failure of taste. Product data enters a catalog from suppliers, and suppliers write for their own distribution network. Categories inherit ERP structure because that's where the SKUs come from. Attribute names inherit whatever the first supplier called them. The vocabulary is internally consistent and completely disconnected from the phrasing of the person trying to buy.


Some examples that recur across catalogs:


What the catalog says What the buyer types


Fluid Handling — Rotary pump


Overcurrent Protective Devices breaker


Personal Protective Equipment — Hand work gloves


Threaded Fasteners — Hex Head bolts


Luminaires — Linear Recessed shop light


Facility Maintenance — Absorbents spill kit


The right side isn't more correct. It's more searched. And when your catalog only contains the left side, a search for the right side either returns nothing or returns whatever fuzzy match the engine can manage — which is how a null-result rate ends up at 15% while everyone believes the catalog is complete.


## Three places the gap shows up


**Site search.** Null-result queries are the most under-read report in commerce. They are a literal transcript of demand you failed to serve, written by the customer, timestamped. Most teams look at them once a year.


**Facets.** A facet nobody clicks is usually not a useless facet — it's a facet labelled in vocabulary the buyer doesn't recognise, or one populated so sparsely that filtering on it removes most of the assortment. Both are fixable and both are invisible in a conversion dashboard.


**AI retrieval.** This is the newer and sharper version. When someone asks an answer engine "what do I need to seal a 2-inch threaded joint on a gas line," the model is matching that question against[product attributes](https://www.anglera.com/glossary/product-attributes) . It is not matching against **Pipe Sealants — Anaerobic** . If your record contains only the manufacturer's vocabulary and none of the application language, the match is weaker for reasons that have nothing to do with whether your product is the right one.


## The fix is structural, not editorial


The instinct is to rewrite descriptions with more customer-friendly words. That helps a little and misses most of the value, because the systems that decide whether a product is *findable* read fields, not paragraphs.


What actually moves the numbers:


**A synonym layer on the taxonomy.** Keep **Fluid Handling — Rotary** as the internal node — it's load-bearing for purchasing. Attach` pump` ,` centrifugal pump` ,` transfer pump` as searchable alternates. Nothing about the ERP relationship changes; the search index gains the words people use.


**Application attributes as structured values.** Not prose about what the product is for, but fields:` Application` ,` Used With` ,` Replaces` ,` Compatible With` . These are what turn a question phrased as a task into a match against a specification.


**Buyer-facing attribute labels.** The field can be` AMPS_RTG` in the source system and display as "Amperage" with a unit. One is for the integration, one is for the human.


**Cross-references and supersessions.** In trade categories, a large share of searches are for a competitor's part number or a discontinued one. A catalog that can't resolve those is answering "no" to a customer who was ready to buy.


Every item on that list is[attribute work](https://www.anglera.com/glossary/product-attributes) . Which is the point: the vocabulary problem looks like a merchandising or SEO problem and lives in the data layer.


## Where the words come from


You don't have to guess, and you shouldn't. The vocabulary is already sitting in systems you own:


- **Null-result site searches** — the highest-signal source, already collected, rarely read
- **Quote and RFQ text** — how customers describe what they want when a human is reading
- **Inbound call and chat logs** — the same, less filtered
- **Marketplace search suggestions** — Amazon's and Grainger's autocomplete is a demand map for your categories
- **Competitor facet labels** — someone else already did this research and published the answer


Collect a few hundred phrases per category and the pattern is obvious within an afternoon. The hard part was never discovering the words. It's getting them into structured fields across a hundred thousand SKUs, which is where this stops being a workshop and becomes an[enrichment](https://www.anglera.com/glossary/product-data-enrichment) programme.


## Don't rename. Add.


One caution, because the overcorrection is worse than the original problem.


Teams who discover this sometimes restructure the whole taxonomy into customer language. That breaks supplier alignment, confuses purchasing, complicates ERP integration, and irritates the internal users who navigate the catalog fifty times a day and knew exactly where everything was.


The internal taxonomy usually exists for good reasons. What's missing is a layer alongside it. Keep the structure, add the vocabulary, and let search resolve between them.


The measurement is straightforward: null-result rate on site search, facet engagement, and the share of top queries that land on a relevant category page. Baseline them before you start, because this is one of the few catalog investments where the before-and-after is unambiguous within a quarter.


If you want to see it on your own data, the market maps for[enrichment platforms](https://www.anglera.com/best/product-data-enrichment-platforms) cover who does this kind of work and how the models differ. And the fastest diagnostic costs nothing: pull last quarter's null-result searches and read the top fifty. The list is usually a very direct message from your customers about the words missing from your catalog.
