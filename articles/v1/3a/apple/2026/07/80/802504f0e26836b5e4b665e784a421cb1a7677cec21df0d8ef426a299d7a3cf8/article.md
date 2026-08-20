---
schema_version: "1.0.0"
document_id: "802504f0e26836b5e4b665e784a421cb1a7677cec21df0d8ef426a299d7a3cf8"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/uncertainty-quantification-function-calling"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:aad8bc3251e7af993e5edcd3bb020ca392c83e03be8c6672d3f7b48c2c17f6fe"
---

# Uncertainty Quantification for LLM Function-Calling

[View publication](https://arxiv.org/abs/2604.22985)


Large Language Models (LLMs) are increasingly deployed to autonomously solve real-world tasks. A key ingredient for this is the LLM Function-Calling paradigm, a widely used approach for equipping LLMs with tool-use capabilities. However, an LLM calling functions incorrectly can have severe implications, especially when their effects are irreversible, e.g., transferring money or deleting data. Hence, it is of paramount importance to consider the LLM’s confidence that a function call solves the task correctly prior to executing it. Uncertainty Quantification (UQ) methods can be used to quantify this confidence and prevent potentially incorrect function calls. In this work, we present what is, to our knowledge, the first evaluation of UQ methods for LLM Function-Calling (FC). While multi-sample UQ methods, such as Semantic Entropy, show strong performance for natural language Q&A tasks, we find that in the FC setting, it offers no clear advantage over simple single-sample UQ methods. Additionally, we find that the particularities of FC outputs can be leveraged to improve the performance of existing UQ methods in this setting. Specifically, multi-sample UQ methods benefit from clustering FC outputs based on their abstract syntax tree parsing, while single-sample UQ methods can be improved by selecting only semantically meaningful tokens when calculating logit-based uncertainty scores.


- † University of Oxford
- * Equal contribution
- ‡ Joint senior authorship
