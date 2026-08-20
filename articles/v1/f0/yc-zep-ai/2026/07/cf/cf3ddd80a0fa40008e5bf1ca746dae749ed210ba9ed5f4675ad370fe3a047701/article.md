---
schema_version: "1.0.0"
document_id: "cf3ddd80a0fa40008e5bf1ca746dae749ed210ba9ed5f4675ad370fe3a047701"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/evaluating-nemotron-3-embed-for-agent-memory/"
published_at: "2026-07-16T16:00:32+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:9966356b5ebff43c1abc26d650101a820da775bfc5dd734df30e8440499bd1eb"
---

# Evaluating Nemotron 3 Embed for agent memory

## Key takeaways


- **Recall errors compound in Zep's graph retrieval.** Semantic and BM25 full-text search select the entrypoints into a[Context Graph](https://help.getzep.com/searching-the-graph?ref=blog.getzep.com) ; traversal expands from those nodes, so a fact missed at the entrypoint is unreachable downstream.
- **Zep benchmarks every candidate embedding model on a gold dataset** of 5,954 production-faithful recall queries across three retrieval tasks, scored with paired bootstrap confidence intervals and Wilcoxon significance tests.
- [NVIDIA Nemotron 3 Embed 1B](https://huggingface.co/nvidia/Nemotron-3-Embed-1B-BF16?ref=blog.getzep.com) **took first place on every task** , ahead of a 4B competitor with a third of the parameters and ahead of our production incumbent by up to 3.3 points of Recall@10.
- **The accuracy lead costs index storage.** At native 2,048 bf16 dimensions the index runs 4,096 bytes per vector, 2.7× the incumbent's; int8 output quantization, if NVIDIA ships it, would halve that.
- **The**[NVFP4 checkpoint](https://huggingface.co/nvidia/Nemotron-3-Embed-1B-NVFP4?ref=blog.getzep.com) **addresses encode cost.** NVIDIA reports 2× throughput with 99% accuracy retention on Blackwell, and the 4-bit weights cut the model's GPU footprint to roughly a quarter; embedding runs on both the ingestion and retrieval paths, so encode throughput constrains production directly.


---


## Recall accuracy sets the ceiling


Zep's agent memory service extracts entities and facts from conversations and business data, maintains them in a knowledge graph, and serves relevant context back to agents at request time. To offer graph traversal with low latency at significant scale, retrieval starts with search. A recall query runs semantic similarity and BM25 full-text search over the graph's nodes and edges, with results fused by reciprocal rank fusion. This helps identify subgraphs that are then traversed using BFS and other graph algorithms.


Traversal expands from those entrypoints into the surrounding subgraph and retrieves connected facts the query text never mentioned. Because it starts from a small set of entry nodes, each retrieval touches a neighborhood of the graph rather than the whole structure, and the cost of graph operations stays bounded as graphs grow.


Zep's retrieval architecture makes embedding recall a binding constraint.


This architecture makes embedding recall the binding constraint. Traversal can only reach nodes connected to the entrypoints, and reranking can only reorder the candidate set; neither stage adds an entity the initial search missed. We therefore benchmark every candidate embedding model on our own workload before it enters production.


## How we evaluate a search component


We benchmark candidate models against a gold dataset built from production-shaped extraction runs: 5,954 conversations, each paired with the facts and entity summaries Zep extracted from it. The corpus for each task is the global pool of every conversation's items, so a query must surface its own conversation's facts above roughly 30,000 distractors.


The queries are production-faithful. Each is built exactly the way the production recall path builds it: the last four messages of the conversation, compressed to 768 bytes by the same query-compression routine the app runs. Scoring is exact and paired. Search is exact cosine over the full pool, with no approximate-index confound, and models are compared with paired bootstrap 95% confidence intervals plus Wilcoxon significance tests on Recall@10 and nDCG@10. Recall@k is the primary metric because downstream stages can reorder candidates but cannot add them.


Each task probes a different retrieval shape:


- **facts** — short edge-fact sentences (30,093 documents)
- **fact concatenations** — deterministic per-entity strings joining an entity's extracted facts (34,467 documents)
- **summaries** — abstractive entity summaries written by an LLM from the source episodes (34,923 documents)


The LLM-written summaries are the most faithful proxy for real entity summaries and the hardest target; every model drops one to two points there.


The tasks are self-labeled: relevant items derive from the query's own conversation, so absolute recall is optimistic and the relative ordering between models is the signal. The 768-byte query compression drops context for some facts, but the handicap is identical across models, so comparisons remain fair.


## Results for Nemotron 3 Embed 1B


[Today's launch](http://huggingface.co/blog/nvidia/nemotron-3-embed-wins-rteb?ref=blog.getzep.com) covers two sizes,[NVIDIA Nemotron 3 Embed 8B](https://huggingface.co/nvidia/Nemotron-3-Embed-8B-BF16?ref=blog.getzep.com) and[1B](https://huggingface.co/nvidia/Nemotron-3-Embed-1B-BF16?ref=blog.getzep.com) . We evaluated the 1B under NVIDIA's early-access partner program (pre-release weights, RTX PRO 6000 Blackwell) against our production incumbent, EmbeddingGemma 300M, and Perplexity's pplx-embed-v1 models at 0.6B and 4B.


On the hardest Summaries Recall@10 task, Nemotron 3 Embed 1B outperformed a model 4x its size.


Task Nemotron 3 Embed 1B
(2048d, bf16) EmbeddingGemma
300M (768d, bf16) pplx-embed 0.6B
(1024d, int8) pplx-embed 4B
(2560d, int8)


Facts **0.663** 0.645 0.652 0.658


Fact concat **0.655** 0.622 0.641 0.652


Summaries **0.643** 0.617 0.618 0.637


*Recall@10 at native dimensions over 5,954 production-faithful queries. Higher is better. Source: Zep internal benchmark, July 9, 2026.*


[Nemotron 3 Embed 1B](https://huggingface.co/nvidia/Nemotron-3-Embed-1B-BF16?ref=blog.getzep.com) , a 1.3B-parameter model producing 2,048-dimension embeddings from a pruned and distilled Ministral-3, took first place on all three tasks. The margins are fractions of a point, so we test whether each one reflects a real difference between models rather than noise.


A Wilcoxon signed-rank test compares two models' scores query by query over the same 5,954 queries; a small p-value means a difference this consistent is very unlikely if the models actually perform equally. Every pairwise delta clears p ≤ 1e-3, with one exception: on fact concatenations, the Recall@10 delta against the 4B model is not significant (p = 0.06), though the matching nDCG@10 delta is (p = 1.6e-7).


Observed Recall@10 deltas were significant.


## Storage and latency at production scale


Index storage scales with bytes per vector, multiplied across every fact and entity in millions of Context Graphs. At 2,048 dimensions in bf16, Nemotron 3 Embed 1B writes 4,096 bytes per vector, 2.7× the incumbent's 1,536 and four times the bytes of Perplexity's 0.6B configuration (1,024 dimensions at int8, 1,024 bytes). The accuracy lead comes with a larger index, and the footprint falls only if the stored vectors are quantized; int8 output quantization, if NVIDIA ships it, would cut it to 2,048 bytes.


Embedding runs on the ingestion path, where every message is encoded, and on the retrieval path, where every recall query is encoded before search. In our run the BF16 checkpoint encoded roughly 1,535 short facts per second on a single RTX PRO 6000 Blackwell. The 1B also ships with an NVFP4 checkpoint, which quantizes the model's weights and leaves the output vectors unchanged, so the gain lands on encode throughput rather than index size. NVIDIA reports 99% accuracy retention at 2× the throughput of BF16 on Blackwell hardware. At equal accuracy, a doubled encode rate halves the embedding compute per message. The 4-bit weights also cut the model's GPU memory footprint, 2.6 GB in bf16, to roughly a quarter, so more replicas fit per device.


## A compelling model


Nemotron 3 Embed 1B is the strongest embedding model we have tested on our workload: first on all three tasks and statistically significant against every alternative, with a quantized serving path that addresses encode cost at scale.


These numbers come from the July 9 pre-release checkpoint, and GA weights could differ. We look forward to testing the GA model.


Read the[NVIDIA announcement on Hugging Face](http://huggingface.co/blog/nvidia/nemotron-3-embed-wins-rteb?ref=blog.getzep.com) .
