---
schema_version: "1.0.0"
document_id: "e9b9a287650923ee192a27b6c148e51f01f2e75f159d7bec23e76be75f96e356"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/how-vera-health-achieved-state-of-the-art-clinical-accuracy-using-zeroentropy/"
published_at: "2025-10-14T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:379151f0b9319bdb6d5f1c3d8ceb48524bd1de45d87010f4a2f9176f6aed8e97"
---

# How Vera Health Achieved State-of-the-Art Clinical Accuracy Using ZeroEntropy

TL;DR


Vera Health set a new record on the US Medical Licensing Exam (USMLE) with **97.5% overall accuracy** , surpassing OpenAI, Google, and Anthropic models. Behind this performance is **ZeroEntropy’s Retrieval API** , powering real-time search and retrieval across more than **60 million peer-reviewed papers and clinical guidelines** .


## Overview


Vera Health, an AI platform for medical professionals, set a new record on the US Medical Licensing Exam (USMLE) with **97.5% overall accuracy** , surpassing OpenAI, Google, and Anthropic models. It also led benchmarks such as **NEJM Q&A (84.9%)** and **MedXpertQA (62.2%)** , becoming the top-performing medical reasoning system in the world.


Behind this performance is **ZeroEntropy’s Retrieval API** , powering Vera’s real-time search and retrieval across more than **60 million peer-reviewed papers and clinical guidelines** .


## Challenge


Medical reasoning requires **precision retrieval** , not just memorization.


Traditional LLMs fail when:


- Answers depend on *the latest research or treatment guidelines* .
- Queries are nuanced, context-dependent, or multi-step (e.g., “Compare treatment options for stage 2 hypertension in smokers with COPD”).
- [RAG pipelines](https://www.zeroentropy.dev/concepts/rag/) return hundreds of irrelevant PubMed abstracts that overwhelm the model.


Vera’s goal was to make AI *clinically useful* , providing doctors with answers they can trust, based on the most current and authoritative sources.


## Solution: Agentic Search Powered by ZeroEntropy


Vera integrates **ZeroEntropy’s full-stack retrieval API** : from[hybrid search](https://www.zeroentropy.dev/concepts/hybrid-search/) to[reranking](https://www.zeroentropy.dev/concepts/reranker/) .


#### Dynamic multi-hop retrieval


Vera’s[agent](https://www.zeroentropy.dev/concepts/agent/) decomposes a clinician’s question into sub-queries (e.g., drug interactions, contraindications, dosing) and calls ZeroEntropy multiple times to gather evidence from PubMed and clinical guideline repositories.


#### Precision reranking


ZeroEntropy’s reranker filters and orders results for clinical relevance, ensuring that only the top evidence-based snippets reach the[LLM](https://www.zeroentropy.dev/concepts/large-language-model/) .


#### Latency-optimized search


Designed for real-time use at the point of care — physicians receive reliable answers in seconds.


## Results


**Benchmark** **Previous SOTA** **Vera (w/ ZeroEntropy)** **Improvement**


**USMLE (Steps 1–3)** Pathway (94%) **97.5%** +3.5 pts


**NEJM Q&A** Claude 3 Sonnet (71%) **84.9%** +13.9 pts


**MedXpertQA** GPT-4o (37.3%) **62.2%** +25 pts


Beyond raw accuracy, Vera reports:


- **2× faster inference latency** for[retrieval-augmented queries](https://www.zeroentropy.dev/concepts/agentic-rag/) .
- **>90% reduction in irrelevant citations** returned per query.
- **Stable clinical alignment** with 2024–2025 treatment guidelines, verified by internal physician benchmarks.


Vera Sets New Record for AI on the US Medical Exam - Source: Vera Health


## Why ZeroEntropy


## Impact


Vera is now used by clinicians at institutions such as **Mayo Clinic, Penn, and Yale** , providing real-time, evidence-based recommendations.


By combining **ZeroEntropy’s retrieval intelligence** with Vera’s medical reasoning models and AI[Agents](https://www.zeroentropy.dev/concepts/multi-agent/) , doctors can:


- Instantly verify treatment guidelines and contraindications.
- Generate differentials and dosing calculations backed by the latest literature.
- Improve patient outcomes through faster, safer decision-making.


## About Vera Health


Vera Health builds the world’s most accurate AI medical assistant, trained on live scientific evidence and built with practicing clinicians. Backed by **Y Combinator** and **Gradient Ventures (Google AI Fund)** .


## About ZeroEntropy


ZeroEntropy develops state-of-the-art retrieval models for AI applications. Its **search, reranking, and multi-query orchestration APIs** enable the most advanced context-aware AI systems, from legal research to clinical reasoning.
