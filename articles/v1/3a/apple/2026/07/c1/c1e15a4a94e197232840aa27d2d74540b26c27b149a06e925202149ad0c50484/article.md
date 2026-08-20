---
schema_version: "1.0.0"
document_id: "c1e15a4a94e197232840aa27d2d74540b26c27b149a06e925202149ad0c50484"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/gh-esd"
published_at: null
first_seen_at: "2026-07-28T01:14:46.762740+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:0ce55cd8d3fa8962b324597be6cf3bd0e5815340b7f6da54d799c93709ed7616"
---

# GH-ESD: Grounded Hypothesis-Driven Error Slice Discovery for Instance-Level Vision Tasks

[View publication](https://arxiv.org/abs/2512.24592)


Systematic failures of vision models on semantically coherent subsets, known as error slices, reveal limitations in robustness and evaluation. Existing slice discovery approaches largely model slices as clusters in representation space or combinations of predefined attributes. While effective for image-level classification, such formulations are insufficient for instance-level tasks such as object detection and segmentation, where failures often arise from contextual relational and spatially grounded visual patterns. We propose GH-ESD (Grounded Hypothesis-Driven Error Slice Discovery), a generate and verify framework that reformulates slice discovery as grounded hypothesis generation and statistical verification. GH-ESD constructs relational failure hypotheses using LLM priors and grounded visual evidence, discovers hypothesis slices at the instance level via Vision Language Models, and verifies them through statistical trend analysis over instance-level errors. We also introduce GESD (Grounded Error Slice Dataset), a new benchmark for instance-level error slice discovery, providing expert-defined and spatially grounded slices derived from detection and segmentation failures. Extensive experiments demonstrate that GH-ESD consistently outperforms baselines, improving Precision@10 by 0.10 (0.73 vs. 0.63) on the GESD benchmark for detection tasks, while also supporting segmentation scenarios. GH-ESD identifies interpretable slices that facilitate actionable model improvements.


* Equal contribution.
