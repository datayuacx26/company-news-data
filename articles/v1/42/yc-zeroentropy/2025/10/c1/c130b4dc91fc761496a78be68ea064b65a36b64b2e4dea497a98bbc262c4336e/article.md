---
schema_version: "1.0.0"
document_id: "c130b4dc91fc761496a78be68ea064b65a36b64b2e4dea497a98bbc262c4336e"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/the-latency-myth-why-reranking-is-still-the-smartest-optimization-you-can-make/"
published_at: "2025-10-24T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:3a80adefb28734d475319e4f5e3c0f0960769c1db8801b49fab8f0ba8d28167b"
---

# The Latency Myth: Why Reranking Is Still the Smartest Optimization You Can Make

## The Latency Myth: Why Reranking Is Still the Smartest Optimization You Can Make


TL;DR


[Rerankers](https://www.zeroentropy.dev/concepts/reranker/) add milliseconds to your retrieval pipeline but **dramatically reduce end-to-end latency** by sending fewer, higher-quality results to downstream[LLMs](https://www.zeroentropy.dev/concepts/large-language-model/) . ZeroEntropy’s zerank-1 is ≈12× faster at large payloads, 4% more accurate, and 2× cheaper than leading alternatives—making reranking the smartest optimization for[RAG](https://www.zeroentropy.dev/concepts/rag/) and[agentic](https://www.zeroentropy.dev/concepts/agent/) workloads.


When teams start building retrieval systems, one of the first questions they ask is:


*“Isn’t adding a reranker going to slow down my pipeline?”*


The short answer is: **yes, technically — but it’s a tradeoff that pays off massively** .


The long answer is that **reranking improves both efficiency and quality** once you consider the full retrieval-generation loop.


### Why Latency Alone Is a Bad Metric


Rerankers introduce an additional step in your retrieval pipeline. On paper, that sounds slower — and it is, by tens or hundreds of milliseconds.


But in practice, reranking **lets you send far less irrelevant context** to the downstream LLM or agent.


Instead of giving an LLM 50 low-quality documents (which increases both latency and[token cost](https://www.zeroentropy.dev/concepts/cost-per-token/) ), you can give it **5 highly relevant, semantically optimized results** .


That translates to **shorter prompts, faster inference, and higher accuracy** downstream — meaning the *total* end-to-end latency is often *lower* with a reranker.


### Our Latency Profile


Behind the ZeroEntropy API, our reranker achieves production-grade latency with realistic payloads.


Percentile Reranker (75 KB payload) Retrieval API (205 MB corpus) Retrieval + Reranker


**p50** 129.7 ms 156.1 ms **220.5 ms**


**p90** 146.1 ms 181.4 ms **253.1 ms**


**p99** 193.9 ms 276.2 ms **320.2 ms**


In production deployments for a customer sending billions of tokens per day, we observe:


- **p50:** 75 ms
- **p90:** 125 ms
- **p99:** 238 ms


These are *real-world* latencies measured under live traffic — fully acceptable for both retrieval pipelines and AI agent loops.


### How ZeroEntropy Compares


Benchmarking against leading rerankers:


Model[NDCG@10](https://www.zeroentropy.dev/concepts/ndcg-at-k/) Latency (12 KB)[Latency](https://www.zeroentropy.dev/concepts/latency-tail/) (75 KB) Price


**Jina rerank m0** 0.7279 547 ± 67 ms 1990 ± 116 ms $0.050 / 1M tokens


**Cohere rerank 3.5** 0.7091 172 ± 107 ms 459 ± 88 ms $0.050 / 1M tokens


**ZeroEntropy zerank-1** **0.7683** **149.7 ± 53 ms** **156.4 ± 95 ms** **$0.025 / 1M tokens**


ZeroEntropy’s zerank-1 delivers:


#### Higher accuracy


≈ **4 % higher accuracy (NDCG@10)**


#### Faster inference


≈ **3.7× faster** at small payloads and ≈ **12× faster** at large payloads


#### Lower cost


**2× cheaper** than JinaAI


That’s not an incremental gain — it’s an *order-of-magnitude* improvement for real-time RAG and agentic workloads.


### Self-Hosting Considerations


For teams building latency-sensitive systems, self-hosting is fully supported:


- **zerank-1-small (1.7B)** — Apache 2.0 open weights, easy to run on a single[GPU](https://www.zeroentropy.dev/concepts/gpu-memory-hierarchy/)
- **zerank-1-xl (4B)** — commercial license for on-prem or VPC deployment


Running these locally can cut network round-trip times entirely, bringing median inference to sub-100 ms — ideal for in-house retrieval stacks or compliance-constrained environments.


### Takeaway


Adding a reranker does add a few milliseconds.


But giving your agent a *smarter* search step is still the fastest way to better performance.


In retrieval and[agent loops](https://www.zeroentropy.dev/concepts/agent-loop/) , the real bottleneck isn’t “how long each search takes” — it’s **how many times you have to redo it** .


A better reranker means *fewer passes* , *shorter contexts* , *faster responses* , and *higher accuracy* — an unbeatable tradeoff.
