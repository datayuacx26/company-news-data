---
schema_version: "1.0.0"
document_id: "12c3a99a414b2435f3b931866b28cb0c4adff20940d5472f415ee4c614464ba6"
company_key: "yc-ritivel"
company: "Ritivel"
source_id: "yc-ritivel-news-import-f2b2dd915304"
canonical_url: "https://www.ritivel.com/blog/retrieval-benchmark-confusable-drug-names"
published_at: "2026-01-17T00:00:00+00:00"
first_seen_at: "2026-07-23T23:10:20.911041+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:6fd455a0bb19395a008cf77830049b2e7a48bfa41ffe67a14a4af1aac2549783"
---

# Why Drug Name Confusion Breaks RAG Systems — And How to Fix It

Retrieval-augmented generation (RAG) is transforming how pharmaceutical companies search regulatory documents. But there's a critical failure mode that most benchmarks miss: **confusable drug names** .


When a regulatory professional asks about “Abacavir Sulfate,” how often does the retrieval system return information about the similarly-named “Abametapir” instead? We built a benchmark to find out—and discovered that standard embedding-based retrieval fails on nearly half of such queries.


The good news: with the right techniques, failure rates drop by 80%. Here's what we learned.


0


Validated benchmark questions


0%


Failure rate reduction with context enrichment


0.0%


Best Recall@20 achieved


## The Problem: Drug Names Are Designed to Sound Similar


Pharmaceutical naming follows conventions that make drugs within the same class sound alike. This helps clinicians, but creates a nightmare for retrieval systems. Consider these pairs:


#### Examples of Confusable Pharmaceutical Names


Products identified via Levenshtein distance (1-5 character edits)


Product A Product B Edit Distance Different Uses


Abacavir Abametapir 4


Antiviral vs Antiparasitic


Acitretin Acyclovir 5


Retinoid vs Antiviral


Acetaminophen Acetazolamide 5


Analgesic vs Diuretic


Albuterol Atenolol 4


Bronchodilator vs Beta-blocker


Amlodipine Amitriptyline 5


Calcium blocker vs Antidepressant


Note: Similar names, entirely different regulatory requirements. A retrieval error could return wrong drug information.


Each pair shares the same first letter and differs by only 4-5 characters. **To a traditional embedding model, these names look almost identical.** But the regulatory requirements for each product are completely different. A retrieval error doesn't just return the wrong document—it returns information about an entirely different drug.


⚠️


Real-world consequences


In regulatory work, retrieving the wrong guidance can lead to incorrect study designs, wrong testing protocols, or missed requirements. The FDA's Product-Specific Guidance documents contain drug-specific details that don't transfer between products.


## Building a Benchmark for Confusability


To systematically test this problem, we built a benchmark specifically designed to stress-test retrieval systems on confusable pharmaceutical names. The construction pipeline has four stages:


#### Benchmark Construction Pipeline


1


📄


##### Corpus Preparation


Chunk FDA guidance documents & compute embeddings


1,024-dim vectors


→


2


🔍


##### Hard Negative Mining


Find confusable products via Levenshtein + embedding similarity


Top-8 per chunk


→


3


💬


##### Question Generation


LLM generates discriminative questions for each chunk


2-3 per chunk


→


4


✓


##### Validation


LLM agent verifies question exclusivity using tools


84.2% pass rate


The key innovation is **hard negative mining** : for each document chunk, we identify the most semantically similar chunks from products with similar names. Then we generate questions that should retrieve the target chunk—not its confusable neighbors.


## Question Generation & Validation


We used GPT-4o-mini to generate 2-3 discriminative questions per chunk, then validated each question using a Claude-based agent with access to search tools. The agent determined whether each question could be answered by exactly one chunk in the corpus.


#### Question Validation Results


LLM agent determined if each question is answerable by exactly one chunk


84.2%


15.6%


EXCLUSIVE


Unique answer in target chunk


931


84.2%


NOT_EXCLUSIVE


Multiple chunks can answer


173


15.6%


AMBIGUOUS


Query too vague


2


0.2%


0


Validated Questions


in final benchmark


High exclusivity rate (84.2%) indicates effective discriminative question generation.


The 84.2% exclusivity rate indicates that our question generation approach successfully creates queries that require precise retrieval. The 15.8% rejection rate caught questions where multiple products genuinely share the same requirement (e.g., identical dissolution testing protocols).


## Sample Benchmark Queries


The benchmark contains natural-sounding questions that a regulatory professional might ask. Here are examples showing where standard retrieval succeeds and fails:


#### Example Benchmark Queries


Natural language questions that test retrieval precision


“What is the study design for Abacavir Sulfate?”


Expected:


One in vivo bioequivalence study with pharmacokinetic endpoints


✓ Retrieved at position 1


“What analyte should be measured for Abacavir Sulfate?”


Expected:


Abacavir in plasma


✓ Retrieved at position 2


“What are the dissolution testing requirements for Abacavir Sulfate?”


Expected:


Determined upon ANDA review (standardized language)


❌ Failed without context


The third query fails because “dissolution testing requirements” uses nearly identical language across many product guidances. Without context about which product the chunk belongs to, the embedding can't distinguish between them.


## Evaluation Results: Standard Retrieval Fails


We evaluated six retrieval strategies across three embedding models. The results reveal a stark divide between standard and context-enriched approaches:


#### Retrieval Strategy Performance


Recall@20 across 931 benchmark questions (Google Gemini embeddings)


Embedding Only


MRR: 0.336


48.2% fail


51.8%


BM25 Only


MRR: 0.295


54.6% fail


45.4%


Hybrid


MRR: 0.331


48.4% fail


51.6%


Contextual Embedding


Context-enriched


MRR: 0.546


11.0% fail


89%


Contextual Hybrid


Context-enriched


MRR: 0.538


10.6% fail


89.4%


Contextual + Rerank


Context-enriched


MRR: 0.582


9.7% fail


90.3%


Failure Rate@20 = percentage of queries where correct chunk is NOT in top 20 results.


**The baseline results are sobering.** Standard embedding retrieval achieves only 51.8% Recall@20—meaning nearly half of queries fail to retrieve the correct chunk in the top 20 results. BM25 (keyword search) performs even worse at 45.4%.


✓


Context enrichment is the breakthrough


Adding contextual prefixes to chunks before embedding transforms performance. Contextual embedding achieves 89.0% Recall@20—a 72% relative improvement over the baseline.


## Why Context Enrichment Works


The dramatic improvement comes from a simple technique: prepending contextual information to each chunk before computing embeddings. Here's what that looks like in practice:


#### Why Context Enrichment Works


Generic text becomes product-specific with contextual prefix


Original Chunk


❌ Ambiguous


“Dissolution testing requirements will be determined upon review of the abbreviated new drug application.”


This text appears in 50+ product guidances


Contextualized Chunk


This chunk is from the FDA Product-Specific Guidance for Abacavir Sulfate tablets. It describes dissolution testing requirements.


Dissolution testing requirements will be determined upon review of the abbreviated new drug application.


Adding context prefix...


The contextual prefix anchors the embedding in product-specific space. When a user queries about “Abacavir Sulfate dissolution testing,” the contextualized chunk's embedding is much closer to the query than the raw chunk—even though the actual content is identical.


## Model Comparison: Context Matters More Than Embeddings


We tested three embedding models to see if choice of model affects performance:


#### Cross-Model Comparison: Best Strategy


Contextual Hybrid + Rerank performance across embedding models


##### Google Gemini


90.3%


Recall@20


Failure Rate


9.7%


vs Baseline


80.0% ↓


##### OpenAI 3-small


90%


Recall@20


Failure Rate


10%


vs Baseline


80.5% ↓


##### Amazon Titan


89.9%


Recall@20


Failure Rate


10.1%


vs Baseline


80.8% ↓


Near-identical performance across models suggests context enrichment is the dominant factor.


A striking finding: **the choice of embedding model has minimal impact once context enrichment is applied** . All three models achieve 89.9-90.3% Recall@20 with the best strategy. This suggests that context enrichment—not embedding quality—is the dominant factor for pharmaceutical retrieval.


## Key Takeaways


#### Key Findings


📊


##### Context is the dominant factor


Context enrichment accounts for 75-80% of the failure rate reduction. The specific embedding model used has relatively minor impact once context is applied.


🎯


##### Reranking provides consistent gains


Adding a reranking stage improves results by 1-3 percentage points across all configurations, with the largest impact on OpenAI embeddings.


⚠️


##### ~10% of queries remain hard


Even the best strategy fails on queries where products have near-identical regulatory requirements or highly similar content across chunks.


🔄


##### Hybrid provides marginal benefit


Without context enrichment, hybrid (embedding + BM25) adds 1-3 points. With context enrichment, the additional benefit shrinks to under 1 point.


## Implications for RAG System Design


For teams building retrieval systems for pharmaceutical or other specialized domains, this benchmark highlights several design principles:


- **Entity disambiguation is critical.** Domain-specific retrieval often involves entities with similar names but different meanings. Standard embeddings conflate these; context enrichment separates them.
- **Invest in context enrichment before embedding models.** Switching from OpenAI to Gemini embeddings gains 3 percentage points. Adding context enrichment gains 37 points. The priorities are clear.
- **Reranking is worth the latency cost.** For high-stakes applications, the consistent 1-3 point improvement from reranking justifies the additional compute.
- **Test on domain-specific hard negatives.** Generic retrieval benchmarks (MTEB, BEIR) don't capture entity confusion. Domain-specific evaluation is necessary to understand real-world performance.


## Limitations


Several limitations should be noted:


- **Domain specificity:** Results are from FDA pharmaceutical guidance documents. Performance on other regulatory corpora requires separate validation.
- **Static corpus:** The benchmark reflects a point-in-time snapshot. As FDA updates guidance documents, the benchmark would need refreshing.
- **Levenshtein-based confusability:** Our approach captures orthographic similarity but may miss phonetic or semantic confusability.


> The remaining 10% failure rate represents cases where products have genuinely similar regulatory requirements—a fundamental ambiguity that retrieval alone cannot resolve.


---


#### 📚 Reference


Ritivel Labs Inc.. “Confusable Pharmaceutical Product Names: A Retrieval Benchmark.” *Technical Report* (2026).


Tags


RAG


Retrieval


AI


Drug Names


Benchmark


Embeddings


NLP


Written by


### Nirmit Arora


Co-Founder & CTO


Building AI-native regulatory automation at Ritivel. Passionate about accelerating life-saving therapies through technology.
