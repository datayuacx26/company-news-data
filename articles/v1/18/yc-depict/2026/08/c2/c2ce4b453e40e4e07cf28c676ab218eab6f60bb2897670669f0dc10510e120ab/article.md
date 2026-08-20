---
schema_version: "1.0.0"
document_id: "c2ce4b453e40e4e07cf28c676ab218eab6f60bb2897670669f0dc10510e120ab"
company_key: "yc-depict"
company: "Depict"
source_id: "yc-depict-news-import-180f3238b1a3"
canonical_url: "https://depict.ai/resources/blog/hybrid-search-vs-algolia-benchmark"
published_at: "2026-08-10T07:00:00+00:00"
first_seen_at: "2026-08-10T10:44:45.109650+00:00"
fetched_at: "2026-08-10T10:44:46.025132+00:00"
content_hash: "sha256:c50660e24a9e03e5261540cae60ae3d5e34357dd6b6e90220dafcba89b5a05bd"
---

# Good search knows when to stop: our hybrid search vs a hand-tuned Algolia

Type a query into most on-site search bars and you get somewhere between too much and nothing. This August we studied a live storefront running Algolia, the market leader in site search, hand-tuned for years. A single shopper query returned 373 products, including items from categories that had nothing to do with the question. Two other queries on the same store returned zero results. A shopper who hits either outcome does the same thing: leaves.


The industry grades search with averages, and averages hide exactly this. A results list that starts strong and rots into filler can post the same average score as a short list that is simply right. Shoppers do not experience averages. They experience the shape of what comes back.


Here is that shape, drawn to scale from the benchmark this post is about. One tile is one product: dark means the right product, pale means filler.


*Live storefront benchmark, August 2026.*


## Three shapes tell you everything


You can judge a search engine on three questions, and none of them need a dashboard.


**Where does the first right product show up?** Not the first result, the first *right* one. If someone searches for a specific garment in a specific color, and the first true match sits at position 9 behind eight loosely related sets and bundles, the search failed, whatever the average says.


**How does the list end?** Every query has a natural number of right answers in your catalog. A keyword engine keeps matching words long after the right answers run out, so the tail of the list fills with noise. Knowing when to stop does not mean hiding inventory; broad queries should return big lists. It means a specific query gets a list that ends where the right answers end, instead of trailing off into products that merely share a word with the question.


**Does it ever dead-end?** A zero-results page is the single worst screen in e-commerce. The shopper told you exactly what they wanted, in writing, and got told to leave. There is no reason for this screen to exist: even when the literal thing is not in the catalog, the closest things are.


## A fair fight with a tuned Algolia


We got the chance to measure all three shapes properly, on a European fashion brand's live storefronts, in August 2026.


The setup was about as fair as a comparison gets, and it was not tilted our way. One of the brand's stores runs Algolia, live for years, manually tuned with the synonym rules and merchandising touches a mature deployment collects. Their other regional store runs Depict's hybrid search, configured for a few weeks. Same brand, near-identical catalog of about 1,100 products, 96% shared between the two stores.


We ran 36 real shopper queries against both, across seven categories: broad head terms, color and garment combinations, materials, fit vocabulary, occasion phrases, natural language, synonyms, and typos. Then we scored results the way a shopper would: every results page rendered in a browser and every product judged one by one, with a frontier AI model doing the visual evaluation and a second, rules-based scoring layer run independently against full product data. The two layers agreed on every headline finding.


A vendor benchmarking itself deserves your suspicion, ours included. Two things are true anyway. This reflects one brand's Algolia deployment, not every one, measured on one day, and we say so; it is also what years of paid tuning actually bought on a real store, side by side with a few weeks of ours. We keep the receipts: the full 36-query run, raw result lists from both engines included, is on the table in any demo. And the test itself is free to steal. Run those three questions against your own search bar today and you will know more about it than any vendor post can tell you.


## The numbers


Out of the box, Depict matched the tuned deployment on overall result relevance. Everywhere shoppers stopped using the catalog's exact vocabulary and started describing what they wanted, it pulled ahead:


- **Occasion and natural-language queries** ("something for X" phrasing, gift language, event dressing): Depict's top ten was 97% relevant, Algolia's 71%.
- **Color and garment combinations** : 91% versus 74%.
- **Synonyms and typos** , including American versus British product terms: 86% versus 75%.
- **Dead ends** : across all 36 queries, the keyword search returned two zero-result pages. Depict returned zero results zero times.


*Top-ten relevance by query type, share of each engine's top ten that was the right product. Live storefront benchmark, August 2026.*


The shape difference is even clearer than the tier scores.


On the flagship color-and-garment query, the brand's merchandising team had already answered the question themselves: they curate a collection for exactly that theme. Depict returned 25 results, every one the right garment in the right color, and put four of the merchandiser's five picks in the top ten. The search agreed with the person who knows the catalog best. The keyword engine returned 83 results for the same intent: the first true match arrived at position 9, only 29% of the list was actually right, and none of the curated picks made the top ten.


On one American-English query for a product the catalog names in British English, the keyword engine found 4 products. Depict found 49, covering 81% of the merchandiser's own curated collection for that category, with nine of its top ten coming straight from the collection she built. Nobody wrote a synonym rule to make that happen.


That flagship query, drawn dot by dot. A filled dot is the right product; an orange ring is one of the merchandiser's five curated picks landing in the top ten.


*Live storefront benchmark, August 2026.*


And the tails: the keyword engine's median list was 47 products long against Depict's 25, it produced seven lists of more than 200 products against Depict's three (all three of ours on broad head terms, where big is correct), and from rank 21 to 50 its lists were 53% relevant against our 63%. One fabric-and-garment query summed it up: 90% relevant in the top ten, 16% relevant across its full 74 results. The average looked fine. The shape was rotten.


*How the lists end: median length and lists longer than 200 products. Live storefront benchmark, August 2026.*


## Why keyword search decays into filler


None of this is an accident, and little of it is Algolia's fault specifically. It is what word-matching does.


A keyword engine can only rank what shares words with the query. So it over-returns whenever a word is common, which is how a query about one garment fills up with sets, bundles, and accessories that merely mention it. It under-returns whenever the shopper's word differs from the catalog's word, which is how an American term found 4 of a collection's 37 products. And it returns garbage with total confidence when letters collide: on one test, the number one result for a dress query was a bottle of care fluid, because the word "dressing" contains the word "dress".


Brands patch this with synonym dictionaries, rules, and boosts. The tuned deployment we tested against is the best case for that approach, and to be fair, the tuning showed: it was genuinely strong wherever queries used the catalog's own naming. That is what the tuning is for. But every one of those patches was written by a person, one gap at a time, over years, and the gaps never stop coming, because shoppers never stop inventing new ways to describe what they want.


## Search that knows when it is right


Depict's search is hybrid: exact keyword matching and an AI layer that understands what the words mean, weighed together per query. The practical difference is confidence. Because the engine understands the intent, it knows when the right answers have run out, so a specific query gets a short list that is simply correct, and then stops. No filler tail.


The same understanding removes the dead end. In our test, four queries asked for products the catalog does not carry at all. All four times, Depict returned a grid of the closest real alternatives, and flags that fallback in its response so you can see it happening. The keyword engine's answers to those four queries were a zero page, a literal "we found 0 results" page, a wrong product at number one, and a 238-item category dump.


*What each engine did when the exact product was not in the catalog. Live storefront benchmark, August 2026.*


And the system improves while you sleep. Every night it runs an evaluation pass on its own result quality and retunes, so the search you have in month three is better than the one you launched with. Your merchandising rules still win: a pin is a pin, a bury is a bury, and the automation never overrules them.


## The full-time job hiding in enterprise search


Anyone who has run enterprise site search knows the true cost is not the license. It is the person. The dictionaries, the rules, the quarterly relevance reviews: the traditional model quietly assumes someone tends the engine as part of their week, forever. That work does not make search understand shoppers. It compensates, gap by gap, for the fact that it does not.


That era is ending, and not because the work stops mattering. The judgment calls move up. When nobody has to write a rule to teach the engine what a garment is called in American English, the time goes where it shows: curating the collections, the drops, and the stories only a person who knows the brand can build, with a search engine that surfaces that work instead of burying it. Strong on day one, better every night, and the taste stays yours.


## Try it on your catalog


The queries your shoppers type are better tests than any demo script. Bring the ten searches you most fear, run them on your own catalog with Depict, and look at the shape of what comes back: where the first right product sits, how the list ends, whether it ever dead-ends.


Depict Search runs today with brands on Centra and headless storefronts, and pricing is public on our site. On Shopify it is not in the App Store yet: we are onboarding Shopify brands hands-on, one at a time, so the way in is to talk to us.[Book a demo](https://depict.ai/demo) and we will run your queries with you, full benchmark on the table.


*Benchmark: 36 shopper queries, two live storefronts of the same European fashion brand, measured August 2026. Relevance scored per rendered results page, product by product, with two independent scoring layers. Methodology and the full query set available on request.*
