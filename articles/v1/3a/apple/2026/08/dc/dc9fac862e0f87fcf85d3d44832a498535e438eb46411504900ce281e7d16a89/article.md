---
schema_version: "1.0.0"
document_id: "dc9fac862e0f87fcf85d3d44832a498535e438eb46411504900ce281e7d16a89"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/arbitrage-efficient-reasoning"
published_at: null
first_seen_at: "2026-08-08T03:56:15.906765+00:00"
fetched_at: "2026-08-08T03:56:17.231059+00:00"
content_hash: "sha256:ae76d184e296b3e7f12e80ae8bbe97777aa81197804510cfa544511c952dc54d"
---

# Arbitrage: Efficient Reasoning via Advantage-Aware Speculation

[View publication](https://arxiv.org/abs/2512.05033)


Modern Large Language Models achieve impressive reasoning capabilities with long Chain of Thoughts, but they incur substantial computational cost during inference, and this motivates techniques to improve the performance-cost ratio. Among these techniques, Speculative Decoding accelerates inference by employing a fast but inaccurate draft model to auto-regressively propose tokens, which are then verified in parallel by a more capable target model. However, due to unnecessary rejections caused by token mismatches in semantically equivalent steps, traditional token-level Speculative Decoding struggles in reasoning tasks. Although recent works have shifted to step-level semantic verification, which improve efficiency by accepting or rejecting entire reasoning steps, existing step-level methods still regenerate many rejected steps with little improvement, wasting valuable target compute. To address this challenge, we propose ARBITRAGE, a novel step-level speculative generation framework that routes generation dynamically based on the relative advantage between draft and target models. Instead of applying a fixed acceptance threshold, ARBITRAGE uses a lightweight router trained to predict when the target model is likely to produce a meaningfully better step. This routing approximates an ideal ARBITRAGE ORACLE that always chooses the higher-quality step, achieving near-optimal efficiency–accuracy trade-offs. Across multiple mathematical reasoning benchmarks, ARBITRAGE consistently surpasses prior step-level SD baselines, reducing inference latency by up to ∼ 2× at matched accuracy.


- † UC Berkeley
- ‡ ICSI
- § LBNL
- * Equal contribution
