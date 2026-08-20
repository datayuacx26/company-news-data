---
schema_version: "1.0.0"
document_id: "375af6b1513d6c2ee4556e7dabb9164c54d5da09e07dc9640959e4b4a330178d"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/zeroentropy-versus-jina-ai-reranker/"
published_at: "2025-08-07T00:00:00+00:00"
first_seen_at: "2026-07-26T06:27:17.647384+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:251f5d8489aa265ddf4e21dc23e7b430bfb24e228269e65ef638cf4321c7e689"
---

# ZeroEntropy's zerank-1 vs. Jina AI's jina-reranker-m0

TL;DR


ZeroEntropy’s **zerank-1** outperforms Jina AI’s **jina-reranker-m0** across the board: ~+4% higher NDCG@10, up to ~12x faster latency, and 2x cheaper pricing—all on text-only reranking workloads. Jina’s model wins if you need true multimodal (image + text) reranking.


## What Is a Reranker and Why You Might Need One


A reranker is a cross-encoder neural network that rescores and reorders an initial set of candidate documents based on query–document relevance. By processing each query–document pair together, it picks up subtle semantic signals that keyword or bi-encoder methods miss. Rerankers slot in after your first-stage search, whether BM25, vector search, or hybrid, to maximize precision in your top k results. Learn more in our guide to rerankers and why they matter:[What Is a Reranker and Do I Need One?](https://www.zeroentropy.dev/articles/what-is-a-reranker-and-do-i-need-one)


## TLDR: Jina AI’s vs ZeroEntropy’s latest rerankers


Model NDCG@10 Latency (12 kb) Latency (75 kb) Price


**jina-rerank-m0** 0.7279 547.14 ± 66.84 ms 1990.37 ± 115.91 ms $0.050/1M tokens


**zerank-1** **0.7683** **149.7 ms ± 53.1** **156.4 ms ± 94.6** **$0.025/1M tokens**


**Ratio** ~+4% ~3.7x faster ~12x faster 2x cheaper


You can read a more thorough benchmark of zerank-1 and its open-source counterpart zerank-1-small[here](https://www.zeroentropy.dev/articles/zerank-1-benchmark) .


## Breakdown of the comparison


### Accuracy


Normalized Discounted Cumulative Gain at cutoff 10 (NDCG@10) evaluates ranking quality by rewarding highly relevant documents in early positions. It combines a relevance score (e.g. graded 0–3) with a logarithmic discount on rank, then normalizes against the ideal ordering. Values range from 0 (poor) to 1 (perfect).


Because NDCG@10 applies a steep logarithmic discount to top‐ranked items and then normalizes against the perfect ordering, even a single highly relevant document slipping from position 1 to 2 can slash its contribution and send your overall score tumbling. Errors compound across the top ten slots, so maintaining near-perfect ordering on diverse datasets makes squeezing out every fraction of a percent extremely challenging.


### Latency


Latency measurements show that ZeroEntropy’s zerank-1 processes a 12 KB payload in under 150 ms on average—about 4 times faster than Jina’s m0—and sustains response times below 315 ms even for 150 KB inputs. These improvements stem from optimizations in our inference engine that minimize overhead in cross-encoder scoring and make real-time reranking at scale practical for large payloads.


Payload size jina-reranker-m0 latency zerank-1 latency Ratio


12 KB 547.14 ± 66.84 ms **149.7 ms ± 53.1** **ZeroEntropy ~4x faster**


75 KB 1990.37 ± 115.91 ms **156.4 ms ± 94.6** **ZeroEntropy ~12x faster**


### Price


A reranker request consumes bytes based on the number of documents and the total length of the input. The formula is:


```text
Total bytes   =   150
+   len  (query.  encode  (  "utf-8"  ))
+   len  (document.  encode  (  "utf-8"  ))
```


This is calculated per document, so the query is counted once for each document you pass in.


For example, if you send a request with 10 documents, the total usage is:


```text
10   ×   len  (query.  encode  (  "utf-8"  ))
+   ∑   len  (document_i.  encode  (  "utf-8"  )) for i   in   1  …  10
```


Our pricing is simple and transparent. We charge` $0.025/1M tokens` .


Jina AI’s pricing is calculated in the exact same fashion, however, they charge` $0.050/1M tokens` , which is twice the cost.


### What the Models Target


#### jina-reranker-m0


**Purpose:** Multilingual + multimodal reranking for visually rich documents (pages, figures, tables, infographics) and code-search tasks


**Inputs:** Query + up to 29-language document images or text blocks


**Use cases:** Visual document search, long-form multimodal text reranking


#### zerank-1


**Purpose:** High-precision text-only reranking to boost any first-stage retrieval (BM25, vector search)


**Inputs:** Query + candidate text documents


**Use cases:** Enterprise search, RAG pipelines, Voice AI, customer-facing search improvements


### Which to Choose?


**Pick jina-reranker-m0 if** you need true multimodal reranking (images + text)


**Pick zerank-1 if:**


- Your use case is text-only and you need maximum top-k precision
- You prefer an API with low latency and cheap token-based pricing
- You require enterprise SLA or on-prem support
