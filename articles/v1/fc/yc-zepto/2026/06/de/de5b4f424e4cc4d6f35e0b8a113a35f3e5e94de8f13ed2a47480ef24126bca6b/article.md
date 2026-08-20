---
schema_version: "1.0.0"
document_id: "de5b4f424e4cc4d6f35e0b8a113a35f3e5e94de8f13ed2a47480ef24126bca6b"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/building-search-for-a-10-minute-world-43b1992f5dca"
published_at: "2026-06-11T09:45:55+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-20T03:19:45.384495+00:00"
content_hash: "sha256:9b9d01604649c899e12ea528d490f9c3cb03fed90fe160d8dc20f1f1291f8406"
---

# Building Search for a 10-Minute World

Every time a user types into a search box, they’re making an implicit bet — that the system on the other side understands not just what they typed, but what they meant, where they are, and what’s actually available to them right now.


Most search systems get to ignore at least one of those dimensions. Zepto gets to ignore none of them. The query arrives. The clock starts, a structured chain of intelligence has to resolve intent — including attribute understanding, and query rewriting, fulfillment zone resolution, retrieval, ranking, all before the user lifts their finger.


> *“* Algorithms rank documents, but great search understands people. *”*


Here is a search query: “amul doodh 1l”. It arrives from a device in Koramangala, Bengaluru at 11:43 AM on a Wednesday. In the next few hundred milliseconds — before the user lifts their finger — a structured chain of intelligence fires.


- The raw query passes through a correction layer that understands phonetically typed Indian vernacular.[Blog on Query Correction](https://blog.zeptonow.com/lost-in-translation-how-we-fix-misspelled-multilingual-queries-with-llms-173ce00c2ba1)
- It moves into an intent prediction system that classifies it at three levels of hierarchy — L1 category, L2 subcategory, L3 — using KNN vector search rather than static rules.
- Query is further segmented into structured components: brand, product type, volume, color and other attributes.
- For queries within the top n by volume, query understanding — correction, segmentation, intent classification — is pre-computed and served statically; for the long tail beyond top n, a live semantic mapping routes queries to the closest top-3L query to leverage accumulated understanding, with work ongoing to bring fully live query understanding to the long tail.
- Retrieval and ranking are always computed live. Lexical and semantic search run in parallel against a city-level index, each producing both exploit and explore candidate streams.
- Product Assortment Service resolves ground-truth inventory, pricing, and store assignment.[Blog on Product Assortment Service](https://blog.zepto.com/from-bottleneck-to-breakthrough-how-we-rebuilt-product-enrichment-at-scale-d40b9d93c186)
- A Mixture of Experts ranking model — the product of an evolution through heuristics, classical LTR, deep learning, and cohort-specific models — produces first level ranking.
- Secondary reranking applies store-product and query-city signals from a feature store. Results slot into relevancy buckets.
- Ads candidates are placed across those buckets. Filters are computed. The response is assembled.


All of that — at minimal response times — handling over a million requests per minute at the search platform level.


### The constraint that breaks everything


Most search systems operate on a relatively stable catalogue. Products exist. Users query. The hard problems are relevance and scale — but the foundational contract is simple: a product either exists or it doesn’t, and that status doesn’t change mid-query.


**Zepto breaks this contract completely.**


We operate on a delivery hub model. The city is divided into hyperlocal delivery zones, each anchored by a dedicated delivery hubs. At any given moment, a user may fall within range of multiple delivery hubs — each with its own inventory, capacity, and priority, changing continuously as orders arrive. “Amul doodh 1l” from Koramangala is not a question about a global catalogue. It is a question about what is available *right now* at the specific delivery hub serving that delivery address.


This constraint cascades through every architectural decision. Retrieval and ranking are always computed live — the query volume tier determines which components of query understanding are pre-computed vs live.


Full result page caching doesn’t work at all: the combination of inventory state, personalization, session signals, and experiment assignment creates too high a cardinality of variations for any cached page to be valid across requests.


The index has to be granular enough to support hyperlocal filtering at query time without per-delivery hub index duplication. Pricing in the index is an approximation; ground truth is resolved downstream.


And query vocabulary — users typing phonetically in Latin script from a dozen Indian languages — requires an LLM-powered understanding that no edit-distance corrector can provide.


> *“A search box is one of the most deceptively complex systems in your product. The complexity isn’t in the box — it’s in the contract you’ve implicitly made with every user who types into it.”*


### The full architecture


### The architecture: two services, surgically separated


The foundational structural decision was splitting the system into two distinct services with different mandates, different data access patterns, and different deployment lifecycles.


search-platform is a pure retrieval service. It knows nothing about users — no delivery address, no order history, no experiment cohort. It receives a query, city identifier, and eligible delivery hub IDs, and returns candidate product IDs with base relevance scores. That is its entire contract.


**This purity means the retrieval stack evolves independently from ranking and enrichment logic — a new index template, a new embedding model, or a new synonym set deploys without touching the experience layer.**


Search Orchestration takes raw candidates, passes them through the Product Service for authoritative inventory and price resolution, runs the ML ranking stack against pre-materialised features stored into a feature store, slots ads across relevancy buckets, computes live filters, and assembles the response. Session data — pagination cursors, active filters, experiment cohort assignments — lives in in-memory store, read at the start of each request.


### The query understanding layer


> *“People don’t search for what they want. They search for the words they think will lead them to what they want.”*


In quick commerce, the query is a user’s expressed intent at the moment of highest purchase probability. Misunderstanding it — wrong category, wrong language normalization, wrong expansion — means you’re optimizing the entire retrieval stack against the wrong objective.


**Query understanding is where you decide what game you’re playing. Everything else is just execution.**


Before any retrieval happens, every query passes through a query understanding layer composed of three distinct components. These run in sequence and their outputs cascade into every downstream decision — what to retrieve, how to rank, which tier the query belongs to, which rules to fire.


### Query correction


The Query Correction Layer standardises noisy input into a canonical representation before it enters retrieval. The system corrects misspellings, phonetic variations, and vernacular expressions.


Standard edit-distance correctors fail entirely here: “kothimbir” (Marathi/Hindi for coriander), “paal” (Tamil for milk), and “balekayi cheeps” (Kannada-inflected banana chips) share no token overlap with their catalogue equivalents — there is no canonical Latin-script spelling to correct toward. The problem is phonetic intent mapping, not character-level deviation.


The QCL system is built on Meta’s Llama 3–8B, self-hosted on Databricks, running in instruct-tuned mode.


> *The key design decision is grounding the model in the actual Zepto catalogue via a RAG pipeline: the noisy query is embedded and used to retrieve the top-K most semantically similar product names and brand names, which are then passed to the LLM as context. This prevents hallucination of products that don’t exist on the platform.*


Output is structured JSON — a corrected query and a canonical translation. The corrected form goes to retrieval; the raw input displays in the search bar.


### Intent prediction


The corrected query feeds an intent prediction layer that classifies every query across a three-level hierarchy: L1 (broad category: Grocery, Personal Care, Electronics etc.), L2 (subcategory: Dairy, Skincare, Mobile Accessories etc.), and L3 (micro-category: Greek Yogurt, Face Serum, Phone Cases etc).


> *A critical design decision here: intent prediction runs on KNN-based vector search against an embedding index, not on static classifiers. This means the system is completely detached from the live catalogue.*


When new products are added or categories restructure, the intent model doesn’t require retraining — it infers from embedding proximity. This dramatically reduces operational overhead and allows the product taxonomy to evolve without triggering model retraining cycles.


The predicted intent set — which can span multiple L3 categories for ambiguous queries — feeds into the ranking layer, determining which ranking signals to emphasise and which widgets to render in the response layout.


### Query segmentation


Query segmentation decomposes the corrected, intent-classified query into structured components: brand, granular product type, and product attributes (volume, weight, colour, brand, product type etc.).


“Amul milk 1l” segments into brand: Amul, product type: milk, volume: 1L. “Sugar free yogurt 400g” segments into product type: yogurt, attribute: sugar-free, weight: 400g.


Beyond pure decomposition, every query is classified into a pattern type based on which structural components are present: pure L3 (product type only), Brand+L3, Brand+L3+L4, Brand+L4, L3+L4, and so on.


This pattern classification drives different retrieval strategies, different rule sets, and different ranking signal weights downstream. A Brand+L3 query triggers brand-field score boosting in retrieval. A pure L3 query needs broader candidate diversity and different ranking signal weights.


**Segmentation is also the foundation for attribute-aware filtering:** the “volume: 1L” extraction from the query directly powers the “1L” filter option in the response, rather than requiring the user to select it manually.


### Query volume tiers and the live/static boundary


Every query is placed into a volume tier based on its frequency in the query distribution. Segmentation coverage currently extends to queries beyond the 50L+ mark, with the system continuously expanding structured understanding down the tail.


**The tier boundary affects query understanding, retrieval, ranking. For the top N queries** , query understanding — correction output, segmentation into brand/PT/attribute components, intent classification — is pre-computed and served statically.


**Head query distributions are stable** ; “amul butter” and “toned milk” don’t drift meaningfully day-to-day, so serving pre-computed understanding is both correct and efficient.


**For top N+ queries** , the system **uses a semantic mapping layer** to route each long-tail query to its closest top-3L equivalent, then applies rules on top to handle any necessary adjustments. This lets the long tail leverage the accumulated query understanding, segmentation quality, and ranking signal that have been built up for the head — rather than starting from scratch on a sparse signal.


The team is actively building fully **live query understanding and QCL capability** for the long tail.


Why full result page caching doesn’t work at all. The cardinality of valid result variations is too high: inventory state, user cohort, session signals, experiment assignment, and store configuration all vary per request. Two users making the same query at the same moment from the same neighbourhood may be in different experiment cohorts and get different ranker configurations. The only thing that’s stable enough to cache is query understanding for head queries — and even that requires careful staleness management as assortments evolve.


Custom rules can be defined at the query segment level to run pre-ranking (pulling specific candidate sets from the index by property matching — boosting products matching specific attributes, injecting specific SKUs into the candidate pool before ranking runs) or post-ranking (reordering, boosting, deboosting, or burying specific products after ranking has produced its initial order). This gives the search ops team surgical control over specific query patterns at both stages of the pipeline without code changes.


### Hybrid recall: two engines, four candidate streams


> *Lexical search finds what you said. Semantic search finds what you meant. The best systems do both and know when to trust which.*


### Index design: city-pvid with nested store metadata


Both the OpenSearch lexical index and the vector store are indexed at the city-pvid level. Core product information lives at the product level. Store-level metadata — stock status, inventory level, delivery hub-specific signals — is stored as nested documents within each city-pvid record.


This means the index can serve queries filtered to any subset of eligible delivery hubs without duplicating the full product catalogue per delivery hub. At query time, eligible delivery hub IDs (derived from the user’s lat-long) are passed as filters against the nested store metadata.


Out-of-stock products stay in the index for last N days. A product that goes out of stock at all stores doesn’t disappear from search. The index retains it for up to N days, surfacing it in a clearly marked unavailable state for highly relevant queries. This serves an awareness function: users searching for a product they regularly buy should see it even when it’s temporarily out of stock, so they know to check back rather than assuming it doesn’t exist


### Lexical recall — OpenSearch


OpenSearch provides inverted index retrieval scored using a custom scoring function tuned to the platform’s relevance objectives.. The lexical flow produces two candidate streams:


> *A search engine that only shows you what you’ve clicked before isn’t retrieving — it’s confirming.*


**Exploit Lexical** : Products with proven high engagement. These are items with established historical performance for this query — high CTR, strong conversion, reliable presence. The exploit stream optimises for immediate relevance and user satisfaction.


**Explore Lexical** : Products that are on the platform but haven’t accumulated enough engagement to surface naturally through popularity-weighted ranking. New arrivals, items being tested for market fit, products with growth potential. The explore stream counteracts the rich-get-richer dynamic of pure engagement-weighted retrieval.


We are also building a **query-to-product explore/exploit resolution** , as the balance between exploration and exploitation will depend on **query-specific user behavior.**


### Semantic recall — Vector Store with a custom bi-encoder


The semantic retrieval system represents one of the most significant investments in the search stack.


The system was **initially bootstrapped using a large external vector model to solve the first version of semantic retrieval** and, critically, to generate high-quality relevance signals for downstream model development. **Using those generated signals, the team trained a custom bi-encoder model — now the production semantic retrieval engine.**


The bi-encoder embeds user queries and products into a shared vector space, enabling real-time semantic retrieval at scale. At query time, the query vector is compared against pre-indexed product vectors in Vector Store using approximate nearest neighbour search. “Kothimbir” and “coriander” end up near each other. “Something for acidity” surfaces antacid products. Intent-based queries work without manual synonym maintenance.


Similar to the lexical flow, semantic search also produces two candidate streams: **Exploit Semantic** and **Explore Semantic.**


[Blog on Semantic Search](https://blog.zeptonow.com/how-we-built-high-precision-low-latency-semantic-search-in-production-75a6c61dee25)


### Ranking — The ML ranking system: an evolution to Mixture of Experts


Ranking is where the product experience is made. The current system is the product of a sustained evolution through five distinct modelling paradigms, each addressing limitations the previous approach couldn’t solve.


> *The ranking problem looks simple — put the best thing first. The hard part is that ‘best’ is different for every person, every query, every moment.*


### The MoE architecture


The current ranking model is a Mixture of Experts architecture that unifies cohort expertise within a single model, eliminating the operational overhead of maintaining separate per-cohort models. Rather than making hard cohort assignments, a learned gating mechanism dynamically weights each expert network based on contextual and cohort features at query time.


The model embeds rich multi-modal inputs: semantic query and product representations, categorical and continuous product and search features, and learned cohort embeddings. These feed a deep neural backbone that fuses signals and routes them through specialised expert networks, producing relevance scores optimised for both user satisfaction and business impact. The production system is built for real-time performance at scale — custom data loaders, distributed training pipelines, and optimised serving infrastructure ensure low-latency inference under heavy search traffic.


### Tail ranker and overall ranker


The MoE model performs best when interaction data is dense. For query-product pairs below a signal threshold — sparse queries in the long tail — the system routes to a Tail Ranker that leans on semantic relevance between query and product and generalised cohort patterns, rather than sparse query-specific data. Running the main model on thin signal produces noise; the Tail Ranker is purpose-built for this regime.


For new users with no established cohort placement, an Overall Ranker optimises on platform-level signals — global popularity, category trends, catalogue quality. This is preferable to forcing a new user into an arbitrary cohort and producing a ranking that reflects the wrong population’s preferences.


Ranker selection is dynamic and driven by input signals rather than a single global model. Multiple rankers can run in parallel, and new rankers can be onboarded or activated without code changes. This enables ranking strategies tailored to specific user experiences and business use cases. These strategies can be configured dynamically.


[Blog on Ranking](https://blog.zeptonow.com/personalized-search-ranking-the-zepto-way-496d0d405b71)


### Secondary reranking


Primary ranking produces a first-order ordering. Secondary reranking runs expressions over feature store entity data — delivery hub × pvid signals and query × city signals — Adjusting the primary ordering when local signals diverge from global patterns, allowing the primary ranking layer to adapt to different business objectives and user-specific contexts.


The result set then organises into three relevancy buckets: top relevant results, similar results, and more-to-explore (different L3 products). Ads candidates — PLA, PCA, contextual, broad contextual, and brand and various widget placements — are slotted within these buckets at this stage.


### Post-ranking rules


After secondary reranking, a post-ranking rules layer applies ops-configured adjustments: boost, deboost, and bury at the query and city level, effective immediately on save with no code deploy. These run last in the pipeline so they override without interfering with model training dynamics. Finally, “buy again” products for returning users are surfaced at the top of their respective relevancy buckets, completing the result assembly before response building.


### Product Assortment Service: where the index meets ground truth


After ranking, page level candidate set passes through the Product Assortment Service. This is the authoritative resolution step. The index carries inventory and pricing information, but these are not strongly consistent — they’re refreshed continuously but with inherent seconds-to-minutes lag. The index is a fast approximation; ground truth is established here.


For each candidate, the Product Assortment Service performs delivery hub resolution across the user’s eligible delivery hubs, authoritative inventory-aware final check, effective price resolution including and full delivery hub -product property resolution.


[Blog on Store Product Service](https://blog.zeptonow.com/from-bottleneck-to-breakthrough-how-we-rebuilt-product-enrichment-at-scale-d40b9d93c186)


### Ads placement


Ads candidates — PLA (product listing ads), PCA (product contextual ads), contextual, broad contextual, and brand widget placements — are assigned to positions within the relevancy bucket structure.


Relevance thresholds for ads are less strict than organic ranking: an ad for a related but not directly queried product can appear in broader positions. Quality floors still apply regardless of bid.


### Design principles that run through everything


Separate retrieval from ranking. search-platform finds candidates. user-search-service orders them. Different problems, different data requirements, different teams. Conflating them means you can’t improve recall without risking ranking regressions, or vice versa.


Index as approximation, Product Assortment Service as ground truth. The index carries inventory and pricing that’s close but not strongly consistent. Fast candidate retrieval from the index is correct. Using it as the authoritative source of what to show a user is not. Product Assortment Service exists to resolve that gap.


Pre-compute everything that can be pre-computed. The Feature Store exists because computing ML ranking signals synchronously at query time is incompatible with a sub-200ms latency budget. Any signal that can be materialised offline and looked up at serve time should be. The AI Platform handles computation; the serving path handles the read.


Exploit and explore are both first-class retrieval objectives. Pure exploit retrieval — always surfacing the highest-engagement products — creates a rich-get-richer dynamic that starves new products of the exposure they need to generate engagement. The four-stream candidate architecture (Exploit Lexical, Explore Lexical, Exploit Semantic, Explore Semantic) treats discovery as a first-class objective at retrieval, not an afterthought in reranking.


Design for experimental velocity. Every significant component can be enabled, disabled, or swapped via experiment configuration without a code deploy. Running dozens of simultaneous A/B tests on recall strategy, ranker configurations, and UX treatments is how you improve a search system continuously.


Make ops a first-class concern. The Rules Engine, Synonyms, Index Templates, and Merch Configuration exist because the operations team needs to act on what they see in real time. A search team blocked on engineering for every relevance fix is always fighting fires too slowly.


Hyperlocal correctness is non-negotiable. Every layer — city-pvid index, eligible-delivery hubs filtering at retrieval, Product Assortment Service, delivery hub-level ranking signals — is scoped to where the user actually is.


The architecture described here isn’t a finished design — it’s a point in an ongoing evolution. The goal of this series is to expose the decisions and the reasoning behind them, not just the outcomes.


### What’s coming in this series


### Part 2 — Indexing & Write Pipelines


Consumer architecture, P0/P1 priority lanes, consistency model, vector index creation, scylla write pipelines and other flows.


### Part 3 — Autosuggest


LLM-augmented corpus generation, backward expansion, backward traversal, CTR+CVR LTR models, cluster isolation, running autosuggest at scale.


### Part 4 — Browse & Merch


How category pages differ from search — rule contexts, faceted aggregations, sort orders; static and dynamic filter pages, — algorithmic search meets editorial curation


### Part 5 — Search Feature Store


Feature store enables the ingestion of signals from multiple domain entities (e.g., delivery hub products and query cities) and supports real-time optimization across different metrics at Ranking Layer without necessitating code changes.


### Part 6 — Observability & Running Search at Scale


SLOs, latency attribution, Observability — and what we learned operating search through peak traffic events


---


[Building Search for a 10-Minute World](https://blog.zepto.com/building-search-for-a-10-minute-world-43b1992f5dca) was originally published in[Zepto TechXPress](https://blog.zepto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
