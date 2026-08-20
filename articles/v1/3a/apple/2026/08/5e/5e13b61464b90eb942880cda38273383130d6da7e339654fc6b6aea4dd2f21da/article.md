---
schema_version: "1.0.0"
document_id: "5e13b61464b90eb942880cda38273383130d6da7e339654fc6b6aea4dd2f21da"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/alignment-multimodal-llms"
published_at: null
first_seen_at: "2026-08-04T04:44:40.936690+00:00"
fetched_at: "2026-08-04T05:15:07.724576+00:00"
content_hash: "sha256:00bb3cb3e492f799f54d45e4d8bd58ba216da40085e7c527d769877f3b191faa"
---

# Understanding Alignment in Multimodal LLMs: A Comprehensive Study

[View publication](https://arxiv.org/abs/2407.02477)


Preference alignment has become a crucial component in enhancing the performance of Large Language Models (LLMs), yet its impact in Multimodal Large Language Models (MLLMs) remains comparatively underexplored. Similar to language models, MLLMs for image understanding tasks encounter challenges like hallucination. In MLLMs, hallucination can occur not only by stating incorrect facts but also by producing responses that are inconsistent with the image content. A primary objective of alignment for MLLMs is to encourage these models to align responses more closely with image information. Recently, multiple works have introduced preference datasets for MLLMs and examined different alignment methods, including Direct Preference Optimization (DPO) and Proximal Policy Optimization (PPO). However, due to variations in datasets, base model types, and alignment methods, it remains unclear which specific elements contribute most significantly to the reported improvements in these works. In this paper, we independently analyze each aspect of preference alignment in MLLMs. We start by categorizing the alignment algorithms into two groups, offline (such as DPO), and online (such as online-DPO), and show that combining offline and online methods can improve the performance of the model in certain scenarios. We review a variety of published multimodal preference datasets and discuss how the details of their construction impact model performance. Based on these insights, we introduce a novel way of creating multimodal preference data called Bias-Driven Hallucination Sampling (BDHS) that needs neither additional annotation nor external models, and show that it can achieve competitive performance to previously published alignment work for multimodal models across a range of benchmarks.


- * Authors contributed equally as first authors.
- † Authors contributed equally.
