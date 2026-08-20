---
schema_version: "1.0.0"
document_id: "0196db8d3df1eb4d0e5ef0d5c4fcd0a2ce95dc56381811eda1b612fe4caaa7a4"
company_key: "yc-goodfin"
company: "goodfin"
source_id: "yc-goodfin-news-import-71985a80f281"
canonical_url: "https://blog.goodfin.com/blog/how-goodfin-orchestrates-multi-model-ai-for-cfa-level-reasoning"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-23T10:39:33.550295+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:e4487ac068bdb099fbd78f5f92db24cc5b67ee5e4b205c5daadbe40f82d31de4"
---

# How Goodfin Orchestrates RAG-Enhanced Multi-Model AI for CFA-Level Reasoning

## Building Reliable AI Systems for High-Stakes Financial Judgment


Large language models have made remarkable progress in recent years. However, most real-world financial failures do not stem from a lack of knowledge — they stem from a lack of **structure, grounding, and evaluability** .


The CFA Level III exam is a representative example of this challenge. It requires candidates to:


-


Reason under uncertainty;


-


Apply judgment rather than recall;


-


Explain decisions clearly and defensibly; and


-


Meet strict grading and formatting expectations.


These requirements expose the limits of single-model, single-pass LLM deployments.


At Goodfin, we approached the CFA exam not as a benchmark to optimize for, but as a systems problem: how do you design an AI system that reasons with domain context, evaluates itself against professional standards, and produces outputs that are reliable under expert grading *?*


## Research Foundations: From Benchmarks to System Design


This work builds on prior research conducted in collaboration with New York University, where we benchmarked 23 state-of-the-art large language models on mock CFA Level III exams across prompting strategies, grading approaches, and cost–latency tradeoffs ([Ref](https://www.cfabenchmark.com/) ).


Two core insights from this research directly informed our system architecture:


1.


**Task-specialized model selection** : Different CFA tasks reward different strengths — constrained precision for MCQs, structured synthesis for essays — motivating a role-based, multi-model design rather than a single-model approach.


2.


**Evaluation-first orchestration** : High-stakes reasoning requires rubric-aligned evaluation to be treated as a first-class system component, not a post-hoc check.


These insights shaped a production-oriented architecture that assigns explicit roles to models, grounds reasoning in CFA-specific knowledge, and incorporates evaluation into the execution loop.


## From Single Models to Agentic and Deterministic Retrieval-First Systems


Most LLM applications rely on a single model prompted to generate an answer in one pass. While effective for low-risk tasks, this approach struggles when:


-


Reasoning paths matter as much as final answers


-


Outputs must be defensible and traceable


-


Errors must be explainable under professional review


Goodfin’s CFA framework adopts an agentic approach composed of **multiple specialized models coordinated through a deterministic orchestration layer** . The system explicitly separates:


-


Question understanding and classification


-


Context assembly through retrieval


-


Reasoning and answer generation


-


Evaluation and selection


Reliability emerges not from larger models, but from **controlled interaction between grounded components** .


Determinism is a design choice, not a constraint. Before any answer generation occurs, each CFA question is classified by format (essay vs. multiple choice) and routed through a predefined reasoning pathway. This orchestration governs:


-


Which retrieval pipeline is invoked


-


Which model is selected


-


How reasoning is structured


-


How outputs are selected or rejected


In high-stakes systems, deterministic orchestration is a prerequisite for repeatability, debuggability, and predictable behavior across runs.


## Context Assembly: Retrieval-Augmented Generation as a First-Class Stage


All CFA questions pass through a **retrieval-augmented context assembly stage** before any reasoning begins. Rather than relying solely on model priors, the system uses **Retrieval-Augmented Generation (RAG)** to surface relevant CFA concepts, frameworks, and grading expectations that inform downstream reasoning.


The vignette and question are combined into a structured search query, and the retrieved context is injected directly into the reasoning prompts. This grounding step ensures that both essay and multiple-choice answers reflect CFA-specific expectations while avoiding unnecessary prompt inflation.


By treating RAG as a dedicated system stage — separate from reasoning and generation — Goodfin ensures that context grounding is consistent, auditable, and reusable across question types.


## Essay Questions: RAG-Enhanced Chain-of-Thought Reasoning


CFA Level III essay questions demand structured explanation, synthesis, and defensible judgment. To address this, Goodfin employs a **RAG-enhanced reasoning pipeline** optimized for consistency and professional grading standards.


##### Execution Flow


1.


**RAG-Based Context Assembly** : Relevant CFA knowledge is retrieved and provided to the reasoning model.


2.


**Direct Reasoning Generation** : A reasoning-optimized model (` o4-mini` ) generates essay answers using explicit chain-of-thought instructions.


3.


**Self-Consistency Sampling** : Multiple independent reasoning samples are generated to reduce single-pass failure modes.


4.


**Deterministic Selection** : The first successful, valid response is selected, ensuring predictable latency and cost.


The reasoning prompts explicitly instruct the model to focus on the exact question asked and to use retrieved context only when it is relevant to the answer.


## Multiple-Choice Questions: Precision Through Constraint and RAG Grounding


Multiple-choice questions present a different challenge: over-explanation and ambiguity. Goodfin treats MCQs as a constrained decision problem **explicitly grounded through Retrieval-Augmented Generation (RAG)** .


##### Execution Flow


1.


**RAG-Based Context Retrieval** : Relevant CFA knowledge is retrieved prior to reasoning to ground decision-making in domain-specific expectations.


2.


**Robust Option Parsing** : Options are extracted using a layered parsing strategy with multiple fallbacks and strict validation.


3.


**RAG-Enhanced Reasoning** : A general-purpose model (` Gemini 2.5 Pro` ) performs chain-of-thought reasoning with self-consistency sampling, conditioned on retrieved CFA context.


4.


**Deterministic Output Enforcement** : The system enforces single-letter answer selection with strict formatting guarantees.


By combining RAG grounding with strict output constraints and input validation, the system avoids ambiguity, formatting drift, and hedging — key failure modes in professional grading contexts.


## Evaluation Aligned With Professional Standards


Evaluation is embedded directly into the system design rather than applied after generation.


-


MCQs are graded against ground-truth answers


-


Essays are graded using CFA-style rubrics on a 0–4 scale


-


Structural coverage, content alignment, and rubric adherence are measured


This closed-loop evaluation framework enables continuous iteration and transforms reasoning quality into a measurable system property.


## Results and Broader Implications


Under exam-aligned evaluation, the Goodfin CFA framework achieves:


-


**91.95% rubric-based essay performance**


-


**86.67% MCQ accuracy**


-


An overall score exceeding the CFA Level III passing threshold of 65%


More importantly, this system demonstrates a broader principle: **reliable financial AI is built through orchestration, retrieval, constraint, and evaluation — not by scaling a single model** .


While the CFA exam serves as a proving ground, the architecture generalizes to any domain where judgment, explanation, and accountability matter.
