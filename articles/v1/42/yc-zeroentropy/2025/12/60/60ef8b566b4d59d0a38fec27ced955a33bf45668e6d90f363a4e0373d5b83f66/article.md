---
schema_version: "1.0.0"
document_id: "60ef8b566b4d59d0a38fec27ced955a33bf45668e6d90f363a4e0373d5b83f66"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/latency-performance-assessment-of-zerank-2/"
published_at: "2025-12-09T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:25:04.749708+00:00"
content_hash: "sha256:3121c249b35049d08a77f7c6bb0ac50c98261ea424493e97e248b397f7153d88"
---

# Latency Performance Assessment of zerank-2

TL;DR


zerank-2 delivers consistent, low-latency performance under realistic production conditions. In our testing, 97.3% of requests completed under 500ms with zero failures. This document presents our latency measurements and explains how to properly benchmark[reranker](https://www.zeroentropy.dev/concepts/reranker/) performance.


## Why Proper Latency Testing Matters


Production[RAG](https://www.zeroentropy.dev/concepts/rag/) pipelines depend on the reranker staying off the user-facing critical path. When evaluating reranker latency, it’s critical that your testing reflects actual production usage patterns. Real user traffic doesn’t arrive at uniform intervals. It comes in bursts and clusters — exactly the dynamic that drives[tail latency](https://www.zeroentropy.dev/concepts/latency-tail/) . Testing with sequential requests or artificial patterns will give you misleading results that don’t predict real-world performance.


Our tests use Poisson arrival patterns because they model the random, bursty nature of production traffic. This approach reveals how systems behave under realistic load conditions, including queueing effects,[throughput](https://www.zeroentropy.dev/concepts/throughput/) , and concurrent request handling.


## Testing Methodology


All tests conducted using:


- Poisson arrival patterns at 1-10 requests/second
- 60-second test duration
- 50 documents per request
- Payload size ≤2KB per document


## Performance Results


### ZeRank-2 Latency Distribution


Latency Threshold Requests Exceeding Threshold


>75ms 100.0%


>100ms 100.0%


>150ms 50.5%


>200ms 21.2%


>250ms 11.3%


>500ms 2.7%


>750ms 1.4%


>1s 0.9%


>3s 0.0%


>5s 0.0%


>10s 0.0%


>30s 0.0%


**Failed** **0.0%**


### Comparative Performance


Against other industry[cross-encoder rerankers](https://www.zeroentropy.dev/concepts/cross-encoder/) :


Threshold zerank-2 Cohere rerank-3.5 Jina reranker m0 Voyage rerank-2.5


>150ms 50.5% 34.3% 100.0% 80.5%


>500ms 2.7% 14.3% 70.8% 10.9%


>1s 0.9% 11.6% 57.4% 9.7%


>10s 0.0% 6.4% 55.7% 9.2%


**Failed** **0.0%** **0.0%** **55.7%** **9.2%**


## Key Metrics


- **Zero failures** across all test conditions
- **97.3%** of requests completed under 500ms
- **99.1%** of requests completed under 1 second
- **100%** of requests completed under 3 seconds


zerank-2 maintains consistent performance across the entire latency distribution, with no requests exceeding 3 seconds.
