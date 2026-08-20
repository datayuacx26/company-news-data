---
schema_version: "1.0.0"
document_id: "8391e8ccf09a8f5548d0cb4df3e8aa9a39ebc20ff37810e34270b45cc8780f50"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/self-reflective-program-search"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:86fbc28f8fd3b25444cfe56137a5e63bde06194b34a81d5a1d52d9429817b8e3"
---

# Recursive Language Models Meet Uncertainty: The Surprising Effectiveness of Self-Reflective Program Search for Long Context

[View publication](https://arxiv.org/abs/2603.15653)


Long-context handling remains a core challenge for language models: even with extended context windows, models often fail to reliably extract, reason over, and use the information across long contexts. Recent works like Recursive Language Models (RLMs) have approached this challenge by agentic way of decomposing long contexts into recursive sub-queries through programmatic interaction at inference. While promising, the success of RLMs critically depends on how these trajectories of context-interaction programs are selected, which has remained unexplored. In this paper, we study this problem and introduce Self-Reflective Program Search for Long Context (SRLM), a framework that augments programming-based context interaction with uncertainty-aware self-reflection. SRLM leverages three intrinsic signals: self-consistency, reasoning trace length, and verbalized confidence. These serve as complementary indicators of a model’s internal uncertainty, and the model uses them to evaluate and compare candidate context-interaction programs. Extensive experiments across diverse benchmark datasets, context lengths, and backbone models, show that SRLM consistently outperforms state-of-the-art baselines, yielding up to 22% improvement over RLMs under the same time budget. Our findings show that recursion itself is not the primary driver of performance in RLMs, and a simple self-reflective program search can match or surpass RLM without requiring self-query or explicit recursion mechanisms. We find that for context lengths within the model’s context window, RLMs with recursion often degrade performance relative to the base model, whereas SRLM yields consistent and robust gains across both short and long contexts. We also find that RLM is less effective in tasks with semantically intensive nature, where heuristic program search is insufficient and broader contextual understanding is required, while self-reflection in SRLM provides a semantic signal that better steers reasoning in these challenging long-context scenarios.


- * Equal contribution
