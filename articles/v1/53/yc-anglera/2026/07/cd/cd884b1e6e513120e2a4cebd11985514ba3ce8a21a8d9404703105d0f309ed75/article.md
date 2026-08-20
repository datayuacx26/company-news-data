---
schema_version: "1.0.0"
document_id: "cd884b1e6e513120e2a4cebd11985514ba3ce8a21a8d9404703105d0f309ed75"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/ai-search-glossary-b2b-catalogs"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T22:51:30.785703+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:4b8e4969e1f401fe7898f6bb521f3a792f9bde96c55dbd337f1d63bfb1e14f3b"
---

# The AI search glossary for B2B catalogs, in ten layers

Most AI search glossaries are alphabetical, which is the least useful possible order. Alphabetical puts` llms.txt` next to[long-tail SKU](https://www.anglera.com/glossary/long-tail-sku) and leaves you no idea that one of them will change your revenue and the other almost certainly will not.


So this one is organized by where a term bites. Ten layers, roughly in the order a buyer's question travels through your catalog: can a crawler get in, does it receive anything, how does retrieval work on what it got, which surface is the buyer using, is the record good enough to survive the question, how does an agent transact, can anyone tell who you are, and finally, how would you know any of this is working.


Every term links to a full definition. If you only read one section, make it the second and third — access and rendering are where most catalogs are quietly losing, and both are diagnosable in an afternoon.


For the underlying argument about how a B2B product record should be built in the first place, start with[how to structure product data for AI agents](https://www.anglera.com/blog/structure-product-data-for-ai-agents) .


## 1. The names for the work


Four acronyms, one discipline, and a great deal of confusion sold at a premium. Knowing which term has a real origin and which is a coinage tells you how much weight to put on anyone using it.


- [Answer engine optimization (AEO)](https://www.anglera.com/glossary/answer-engine-optimization) — predates LLMs, grew out of featured-snippet work
- [Generative engine optimization (GEO)](https://www.anglera.com/glossary/generative-engine-optimization-geo) — from a 2024 KDD paper; the only one with an academic origin
- [LLMO and AISO](https://www.anglera.com/glossary/llm-optimization-llmo) — vendor coinages, no canonical definition
- [Share of search](https://www.anglera.com/glossary/share-of-search) — the pre-AI measure this all descends from


The honest summary: if a vendor's methodology differs from another's, it will show up in what they actually do to your data, not in which four letters they put on the deck.


## 2. Who gets in


This is the layer with the highest ratio of consequence to effort, and the one most often configured by inheriting somebody's robots.txt from 2024. Training crawlers, retrieval crawlers and user-triggered fetchers are three different things with three different tokens, and the vendor names look deceptively similar.


- [AI crawler](https://www.anglera.com/glossary/ai-crawler) — the umbrella entry, with a reference table of every major agent
- [GPTBot](https://www.anglera.com/glossary/gptbot) — OpenAI's *training* crawler
- [OAI-SearchBot](https://www.anglera.com/glossary/oai-searchbot) — OpenAI's *retrieval* crawler, the one that decides ChatGPT citations
- [ClaudeBot](https://www.anglera.com/glossary/claudebot) — Anthropic's training crawler, alongside Claude-SearchBot and Claude-User
- [PerplexityBot](https://www.anglera.com/glossary/perplexitybot) — retrieval, honours robots.txt; Perplexity-User is user-initiated and documented as generally ignoring it
- [Google-Extended](https://www.anglera.com/glossary/google-extended) — not a crawler at all, and does not opt you out of AI Overviews
- [llms.txt](https://www.anglera.com/glossary/llms-txt) and[llms-full.txt](https://www.anglera.com/glossary/llms-full-txt) — cheap, harmless, and not the reason you are not being cited


Blocking[GPTBot](https://www.anglera.com/glossary/gptbot) while leaving[OAI-SearchBot](https://www.anglera.com/glossary/oai-searchbot) open is a coherent position. Blocking both by accident, then commissioning an AI visibility audit, is a more common one.


## 3. What actually gets read


The second diagnosable-in-an-afternoon layer, and the one that catches teams by surprise because Google has been covering for them. Google executes JavaScript before indexing. AI crawlers, on the available evidence, do not.


- [Server-side rendering for AI crawlers](https://www.anglera.com/glossary/server-side-rendering-ssr) — including the hydration gap and the one-line check
- [JSON-LD](https://www.anglera.com/glossary/json-ld) — the delivery format for machine-readable facts
- [Product schema markup](https://www.anglera.com/glossary/product-schema-markup) — what to put in it for an industrial SKU
- [Structured data validation](https://www.anglera.com/glossary/structured-data-validation) — and why passing the Rich Results Test proves less than you think


A storefront that injects its spec table and[JSON-LD](https://www.anglera.com/glossary/json-ld) client-side is serving an app shell to every agent in section 2. The markup validates perfectly in a tool that runs JavaScript. It is absent from what a crawler receives.


## 4. How retrieval works


You cannot control any of this machinery, which is exactly why it is worth understanding: it explains *why* a thin record loses, in mechanical terms rather than as an article of faith.


- [Retrieval-augmented generation (RAG)](https://www.anglera.com/glossary/retrieval-augmented-generation-rag) — the architecture
- [Vector embedding](https://www.anglera.com/glossary/vector-embedding) — why a sparse record embeds to a generic point and wins nothing
- [Vector database](https://www.anglera.com/glossary/vector-database) — the index, and why hybrid search is non-negotiable for part numbers
- [Content chunking](https://www.anglera.com/glossary/content-chunking) — the chunk is the unit, not the page
- [Semantic search](https://www.anglera.com/glossary/semantic-search) — matching by meaning
- [Grounding](https://www.anglera.com/glossary/grounding) — sourced rather than remembered, and why it does not mean correct
- [Query fan-out](https://www.anglera.com/glossary/query-fan-out) — one buyer question becomes several retrievals


The through-line: retrieval extracts passages and compares vectors. A specification that exists only in a PDF is not a weak signal, it is not a signal. And a passage that names a value without naming the product cannot be attributed back to your SKU no matter how accurate it is.


## 5. The surfaces


Where the buyer actually is. Each behaves differently, and the differences matter more for a distributor than for a DTC brand, because most of these were designed around consumer retail assumptions that industrial catalogs violate.


- [Google AI Overviews](https://www.anglera.com/glossary/google-ai-overviews) — summaries above the results, no separate eligibility path
- [Google AI Mode](https://www.anglera.com/glossary/google-ai-mode) — conversational, so buyers add constraints one turn at a time
- [Google Shopping Graph](https://www.anglera.com/glossary/google-shopping-graph) — where identifier hygiene decides whether your offer attaches to anything
- [Rufus, now Alexa for Shopping](https://www.anglera.com/glossary/rufus-alexa-for-shopping) — renamed in May 2026; most published guidance still says[Rufus](https://www.anglera.com/glossary/rufus-alexa-for-shopping)
- [The digital shelf](https://www.anglera.com/glossary/digital-shelf) — the older framing this all extends


AI Mode is the one to think hardest about. In a conversation, each additional constraint is a filter applied to what the system knows about you, and a null field is elimination rather than a lower rank. Categories where your fill rate is thin are categories where you drop out on turn three and never find out.


## 6. The record: identity and classification


Now we are inside your own data, and the layer everything else depends on. An agent resolves identity first and narrows by class second. A product it cannot identify never gets its specs compared.


- [GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) and[UPC](https://www.anglera.com/glossary/upc-universal-product-code) — where they exist
- [MPN](https://www.anglera.com/glossary/mpn-manufacturer-part-number) — which in B2B usually carries the entire identity load alone
- [Product matching](https://www.anglera.com/glossary/product-matching) — how your offer attaches to a known product node
- [Part number cross-reference](https://www.anglera.com/glossary/part-number-cross-reference) — supersessions and competitor equivalents, usually trapped in a rep's spreadsheet
- [UNSPSC](https://www.anglera.com/glossary/unspsc-classification) ,[ETIM](https://www.anglera.com/glossary/etim-classification) ,[eCl@ss](https://www.anglera.com/glossary/eclass-classification) — classification with different jobs
- [Google product category](https://www.anglera.com/glossary/google-product-category) — required by shopping feeds
- [Golden record](https://www.anglera.com/glossary/golden-record) — one authoritative version, which is what stops a grounded answer being grounded in the wrong row


The B2B-specific point: consistency beats coverage. An MPN formatted identically in the ERP, the PIM and the web catalog is worth more than a[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) you sourced from three places and cannot reconcile.


## 7. Completeness: surviving the constraint


The layer where most enrichment budget should go and where the least of it usually does. Every constraint in a buyer's question is a field you either have or do not.


- [Attribute fill rate](https://www.anglera.com/glossary/attribute-fill-rate) — the measurable thing, and the honest substitute for[semantic completeness](https://www.anglera.com/glossary/semantic-completeness)
- [Product attributes](https://www.anglera.com/glossary/product-attributes) — the filter conditions themselves
- [UOM](https://www.anglera.com/glossary/uom-unit-of-measure) — separating the selling unit from the pack
- [Kit and bundle SKU](https://www.anglera.com/glossary/kit-and-bundle-sku) — and how a kit inheriting one component's attributes lies to an agent
- [Parent-child variants](https://www.anglera.com/glossary/parent-child-product-variants) — one product with options, not forty competing orphans
- [Long-tail SKU](https://www.anglera.com/glossary/long-tail-sku) — where the data is thinnest, by construction
- [Product data enrichment](https://www.anglera.com/glossary/product-data-enrichment) and[SKU enrichment](https://www.anglera.com/glossary/sku-enrichment) — the work of closing the gap


One note on[semantic completeness](https://www.anglera.com/glossary/semantic-completeness) , because it is circulating with a correlation coefficient attached to it. The underlying writing principle is sound. The number is not traceable to any published study, and fill rate against a defined per-category attribute set is the measurable version of the same idea.


## 8. Feeds, protocols, and transacting


The layer that gets the most conference attention and warrants the least urgency for most distributors. It is worth tracking. It is rarely worth being early on.


- [Product feed](https://www.anglera.com/glossary/product-feed) and the[Google Shopping feed specification](https://www.anglera.com/glossary/google-shopping-feed-specification)
- [OpenAI product feed](https://www.anglera.com/glossary/openai-product-feed) — including the identifier-stability requirement your next ERP migration will break
- [Model Context Protocol (MCP)](https://www.anglera.com/glossary/model-context-protocol-mcp) — the plumbing, now under Linux Foundation governance
- [Agentic Commerce Protocol (ACP)](https://www.anglera.com/glossary/agentic-commerce-protocol-acp) — checkout; still OpenAI and Stripe maintained, still beta
- [Universal Commerce Protocol (UCP)](https://www.anglera.com/glossary/universal-commerce-protocol-ucp) — broader, Google-led, with a discovery endpoint
- [Agentic checkout](https://www.anglera.com/glossary/agentic-checkout) — and the B2B assumptions it currently breaks
- [PunchOut catalog](https://www.anglera.com/glossary/punchout-catalog) — what already does this job in enterprise procurement


Governance is worth reading carefully here, because it gets reported sloppily. MCP was donated to the Agentic AI Foundation, a Linux Foundation directed fund, in December 2025. ACP has not been donated to anything; its repository describes foundation stewardship as a future path. Those are different levels of commitment and they should inform different integration decisions.


The part of this section that actually matters to a distributor is not checkout. It is that UCP names catalog search and lookup as a specified capability, which makes the quality of your product record directly queryable and directly comparable against a competitor's, inside a single agent turn.


## 9. Entity trust


Smaller layer, frequently oversold, occasionally decisive. The question it answers is whether a machine can tell who you are and whether your claims are checkable.


- [E-E-A-T](https://www.anglera.com/glossary/e-e-a-t) — a rater framework, explicitly not a ranking factor, with trust as the load-bearing letter
- [sameAs](https://www.anglera.com/glossary/sameas-property) — disambiguation, not endorsement
- [Wikidata](https://www.anglera.com/glossary/wikidata) — useful for the manufacturers you carry, largely unavailable for your own company
- [Data governance](https://www.anglera.com/glossary/data-governance) — the unglamorous prerequisite


The distributor version of E-E-A-T is not author bios. It is naming the standard number and issuing body on a certification instead of writing "UL listed," linking the manufacturer's spec sheet to the SKU so a claim can be checked, and leaving a field blank rather than guessing at it. First-hand application knowledge — what actually fails in coastal installations, which published supersession does not work in practice — is the content a competitor cannot copy and a model cannot synthesize from anywhere else.


## 10. Measurement


Last, deliberately. Every instrument here is a proxy, all of them are noisy, and the vocabulary exists partly so vendors can put a number on something no provider reports.


- [AI citation](https://www.anglera.com/glossary/ai-citation) — the closest thing to a ranking position
- [AI referral traffic](https://www.anglera.com/glossary/ai-referral-traffic) — the only one that lands in your own analytics, and a floor rather than a measure
- [Share of model](https://www.anglera.com/glossary/share-of-model) — sampled, not reported; state the method beside the number
- [Prompt testing](https://www.anglera.com/glossary/prompt-testing) — how those numbers are produced
- [Content health score](https://www.anglera.com/glossary/content-health-score) — the internal data-quality counterpart


Three instruments, three blind spots. Analytics sees clicks and misses every answer that resolved without one. Prompt panels see answers and sample a non-deterministic system. Server logs see retrieval and tell you nothing about what was said. Run all three, report trends across a quarter, and treat single-week movement as noise, because that is what it is.


## The short version


If you read the whole list and want one sequence: check what your product pages return without JavaScript, check which crawlers you are actually allowing, then measure attribute fill rate on the ten to fifteen specs buyers filter on in your top categories. Everything in sections 4 through 10 is downstream of those three answers.


The vocabulary is genuinely useful for reading vendor claims critically, which is most of what it will be used for this year. The work underneath it has not changed much: the facts have to exist, in fields, in HTML, tied to a part number that means the same thing in every system you run.


Anglera does that middle part. Your PIM stores the record; we fill the missing attributes at catalog scale from supplier spec sheets and manufacturer sources, normalized against your schema and written back into the system you already run. The crawler configuration and the rendering stay with your team.
