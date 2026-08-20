---
schema_version: "1.0.0"
document_id: "8519d096b72318f888011a39a4759d30f189a5346aa89711791cb8293de8a012"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/search-quality-measurement-automated-at-scale-43a38aaa7ca0"
published_at: "2026-06-09T11:03:01+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-20T03:19:45.384495+00:00"
content_hash: "sha256:2e53cd50ab4ba00398937cf7ec18bd0597853d43273eb3f88540ae357aead5c2"
---

# Search Quality Measurement. Automated. At Scale

Most teams find out their search is broken the same way: a customer complains, a business metric dips, or someone spots a glaring result in a demo. By then, the damage is done.


The problem isn’t that search breaks. It’s that we have no reliable way to know when it does; or why. The tools most teams use to evaluate search quality are either too slow (human labelers), too indirect (click-through rate), or too narrow (head query benchmarks that ignore 80% of the query space).


What follows is a framework built to fix that. It runs automatically, covers the full catalog, and produces a clean diagnostic signal that tells exactly where search is failing at the retrieval level, the ranking level, or both.


> *Retrieval and ranking are different problems. Merging them is how teams spend months fixing the wrong thing.*


### The two distinct ways search fails


Before getting into the framework, it helps to be precise about what “bad search” actually means. There are two fundamentally different failure modes, and they require different diagnostics.


**Retrieval failure** is when the system can’t find a product at all. A user searches for “noise-cancelling headphones for travel” and a perfectly relevant product is never even considered, not because it ranked low, but because it never entered the candidate set. The ranking model never got a chance to surface it.


**Ranking failure** is when the system finds the right products but orders them badly. The relevant product is in the candidate set at position 60, but the user only sees the top 10. It’s retrieved, but effectively invisible.


Most teams measure these together, using business metrics that blend both signals, and end up misattributing failures. You can’t optimize your ranking model out of a retrieval problem.


### The core idea: the QxP pair


The simplest way to think about this framework is through one abstraction: the query–product pair, or QxP.


Instead of asking “is our search good?”, one asks a more answerable question: for this specific query and this specific product, does the system do the right thing?


That’s it. Every metric in this framework; retrieval efficiency, ranking efficiency, catalog coverage flows from asking that question across millions of QxP combinations.


The QxP abstraction is significant because it creates a shared evaluation atom across both phases. A shared QxP unit enables cross-phase diagnostics, it can be asked “was this product retrieved but poorly ranked?” or “was this product simply never retrieved?” in a single unified view.


### Phase 1: Retrieval Efficiency


*Can the system find the product at all?*


The first phase answers one question: if I search for a product using a reasonable query, does my retrieval system surface it?


The insight here is that we can’t rely on real query logs for this, they’re biased toward products that are already being found. We need to probe every product, including the ones nobody is searching for yet: new listings, niche SKUs, recently restocked items. So we generate the queries ourselves.


**Step 1: Iterate over every product in the catalog.** Not a sample; every product. This is what gives the framework catalog-wide coverage. For very large catalogs, stratified sampling by category or traffic tier can be applied, but the goal is full coverage.


**Step 2: Generate N synthetic queries per product using an LLM.** For each product, we prompt a language model to generate N realistic user queries, spanning exact-match queries, category-level queries, use-case queries, and attribute-driven queries. N typically ranges from 5 to 10.


**Step 3: Fire each query at the retrieval system.** We invoke the production retrieval pipeline for each query and capture the top-K candidate set returned. K is set to match whatever depth the ranking model consumes.


**Step 4: Check if the product appeared.** For each QxP pair, we record a binary outcome: hit (product was in the candidate set) or miss (it wasn’t). Misses are the signal we care about.


```text
Retrieval Efficiency = 1 − (Missed QxP pairs ÷ Total QxP pairs)
```


A score of 1.0 means perfect catalog coverage. Lower scores reveal where the retrieval system is failing entire product segments.


The resulting score can be sliced any way one wants: by category, by price band, by days since listing, by brand. What one is looking for are clusters; systematic retrieval failures affecting a cohort of products, not random noise.


**Why synthetic queries?** Real query logs are biased by definition. They only contain queries that were already typed, which means products that were already being found dominate the signal. Synthetic queries let one audit products that have never been searched for. This is especially critical for new listings and long-tail inventory where retrieval failures go completely undetected in log-based evaluations.


### Phase 2: Ranking Efficiency


*When the products are found, are they being shown in the right order?*


The second phase assumes retrieval is working, products are making it into the candidate set. The question now is: are they being ranked in the order that best serves user intent?


Traditionally, this is answered by human raters who look at query-result pairs and assign relevance scores. Slow, expensive, and doesn’t scale. We replace this with an LLM.


**Step 1: Sample real queries; but sample them right.** We pull X queries from production logs, but with a deliberate stratification: a mix of head queries (high frequency, high volume), torso queries (mid-frequency, broad category intent), and tail queries (low frequency, high purchase intent). Most evaluation frameworks over-index on head queries and miss the tail entirely.


**Step 2: Pull the top-K products shown to users for each query.** For each sampled query, we extract the top-K products that the current ranking system surfaces. These are the QxP pairs we’ll evaluate.


**Step 3: Augment with metadata.** Each QxP pair gets enriched with context: query metadata (category, inferred intent, price sensitivity), product attributes (title, brand, specs, price, ratings), behavioral signals (historical CTR on similar queries, return rate), and platform state (availability, promotional status).


**Step 4: Score with an LLM.** Each enriched QxP pair is passed to a language model with a structured prompt asking it to reason about relevance before producing a score on a 0–1 scale. The reasoning step is deliberate; it produces more calibrated scores and auditable outputs.


The relevance scale:


- **0.0–0.2:** Highly irrelevant. Product clearly doesn’t satisfy the intent expressed in the query.
- **0.2–0.4:** Weak relevance. Tangentially related but unlikely to convert.
- **0.4–0.6:** Moderate. Partially satisfies the query, missing key attributes.
- **0.6–0.8:** Strong relevance. Good match for the expressed intent.
- **0.8–1.0:** Highly relevant. Excellent, specific match, this is where top results should be.


```text
Ranking Efficiency = Σ relevance_score(Qᵢ, Pⱼ) / (X × K)
```


Mean relevance across all evaluated QxP pairs. Decomposable by query tier and product category.


### What do you get when you combine them


Run both phases, and one gets something neither phase alone gives: a precise diagnosis of where in the search stack the problem lives.


**Low retrieval, high ranking:** The ranking model is doing its job; but significant catalog coverage gaps exist. The retrieval model or indexing pipeline needs work. Improving ranking won’t help here.


**High retrieval, low ranking:** The system can find the right products but orders them incorrectly. Ranking model training or feature engineering is the lever. Retrieval changes won’t move this metric.


**Both low:** Systemic failure. Usually a query understanding or embedding quality issue upstream of both systems.


**Tail low, head high:** Head query performance is masking tail degradation. Aggregate metrics wouldn’t surface this; stratified sampling does. The best customers are often also tail query users.


This diagnostic clarity is the most useful property of the framework for product and leadership teams. Instead of “search quality degraded by 3%”, one can say “retrieval efficiency for newly listed products in Home Needs dropped 12 points following last week’s embedding model update.”


### Why existing approaches don’t cut it


This isn’t a new problem. There are established approaches, here’s why each falls short at scale.


**Human relevance labeling** doesn’t scale. A catalog of 1M products with 10 queries per product is 10 million QxP pairs. No rater pool touches that. And humans are inconsistent at the margin.


**CTR and CVR as proxies** blend search quality with price, imagery, and UX. A low-click relevant product isn’t a search failure. This metric can’t tell the difference.


**NDCG on log labels** requires historical interaction data. It can’t evaluate cold-start products. It’s heavily biased toward head queries and bakes in historical ranking bias through the feedback loop.


**Embedding similarity** measures text proximity, not intent relevance. A “travel pillow” and a “hotel room pillow” are textually similar but serve completely different purchase intents.


### Run it continuously, not once


The real unlock apart from the evaluation itself is running it on a schedule and treating the output as a production health metric.


Search quality doesn’t fail catastrophically. It degrades. An embedding model update silently hurts retrieval for a product subcategory. A ranking feature gets re-weighted and tail query relevance drops over three weeks. By the time a business metric signals it, the revenue is already lost.


This integrates naturally into the deployment pipeline. Model pushes are gated on retrieval efficiency delta. If a new embedding model drops retrieval efficiency for any product cohort by more than X%, the rollout is blocked. No need to wait for A/B metric readouts.


### Closing Thoughts


Search quality measurement has been stuck between two bad options: human labels that don’t scale, and behavioral metrics that don’t isolate. This framework offers a third path; automated, catalog-complete, and precise about where in the system the failure lives.


Two phases. One shared unit. No human raters. Runs continuously.


Retrieval efficiency tells whether the system can find a product at all. Ranking efficiency tells whether it shows it in the right order. Together, they give a diagnostic view of the search stack that turns vague quality complaints into actionable engineering work.


That’s the goal: less *“search feels bad”* , more *“retrieval efficiency for new listings is at 0.7, here’s the fix.”*


---


[Search Quality Measurement. Automated. At Scale](https://blog.zepto.com/search-quality-measurement-automated-at-scale-43a38aaa7ca0) was originally published in[Zepto TechXPress](https://blog.zepto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
