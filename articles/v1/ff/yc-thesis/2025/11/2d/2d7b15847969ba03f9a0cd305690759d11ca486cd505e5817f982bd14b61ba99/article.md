---
schema_version: "1.0.0"
document_id: "2d7b15847969ba03f9a0cd305690759d11ca486cd505e5817f982bd14b61ba99"
company_key: "yc-thesis"
company: "Thesis"
source_id: "yc-thesis-rss-df316ac2148b"
canonical_url: "https://thesislabs.ai/research/sota-mle-bench"
published_at: "2025-11-10T00:00:00+00:00"
first_seen_at: "2026-08-10T04:04:12.843518+00:00"
fetched_at: "2026-08-10T04:04:13.995165+00:00"
content_hash: "sha256:61d5462d25274fd35e07942aba255afd7087f6a75c3b46943f493ae71aaf3bf2"
---

# Thesis is State-of-the-Art on MLE-bench

We're happy to share that Thesis has reached state-of-the-art performance on[MLE-bench](https://github.com/openai/mle-bench) , OpenAI's benchmark for evaluating AI agents on machine learning engineering.


## What is MLE-bench?


[MLE-bench](https://arxiv.org/abs/2410.07095) consists of 75 real Kaggle competitions spanning image classification, tabular data, time series, and more. Agents are given a task description, a dataset, and 24 hours to produce a submission. Performance is measured by how often an agent earns a medal, using Kaggle's own medal thresholds.


It's one of the hardest and most realistic benchmarks for AI agents because it requires the full ML engineering loop: understanding the problem, processing data, training models, and iterating on results.


## Our Results


When it launched, the best result was o1-preview with AIDE scaffolding, which medaled on about 17% of competitions. Thesis medaled on **48.44% ± 3.64%** of all competitions.


Difficulty Medal Rate


Low / Lite 65.15% ± 1.52%


Medium 45.61% ± 7.18%


High 31.11% ± 2.22%


**All** **48.44% ± 3.64%**


---


*MLE-bench paper:[arxiv.org/abs/2410.07095](https://arxiv.org/abs/2410.07095)*
