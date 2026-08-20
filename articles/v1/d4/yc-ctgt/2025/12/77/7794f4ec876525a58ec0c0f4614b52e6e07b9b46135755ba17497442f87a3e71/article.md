---
schema_version: "1.0.0"
document_id: "7794f4ec876525a58ec0c0f4614b52e6e07b9b46135755ba17497442f87a3e71"
company_key: "yc-ctgt"
company: "CTGT"
source_id: "yc-ctgt-news-import-358f70d55d44"
canonical_url: "https://www.ctgt.ai/research/introducing-mentat"
published_at: "2025-12-09T00:00:00+00:00"
first_seen_at: "2026-07-27T01:23:44.579296+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:85eb1ca1cbb3afb3910cadba6243f375d195323d1fa974dbf609f97b51265186"
---

# Introducing Mentat: Reliable Runtime Control for LLMs

#### Feature-level intervention. Graph-based verification. Real-time policy enforcement.


Today, we’re excited to introduce Mentat, CTGT’s new OpenAI-compatible endpoint built to deliver something the AI industry has been missing: deterministic control over large language models.


For years, organizations have relied on prompts, filters, and RAG as their primary guardrails. But these are inherently *probabilistic* . Sometimes they work; often they don’t. As LLMs find their way into regulated or high-impact workflows, “works most of the time” is no longer good enough.


With Mentat, we’re introducing a different paradigm - one built not on asking models to behave, but on mathematically guaranteeing that they do.


#### The Problem: External Guardrails Can’t Fix Internal Failure Modes


Enterprises today face two consistent pain points with LLMs:


###### 1. RAG Solves Retrieval, Not Reasoning


Models often fail to integrate retrieved facts. Even with correct context, they can produce incorrect conclusions, inconsistent logic, or spatial/temporal inversion errors.


We repeatedly saw cases where the model had the data but could not apply it.


###### 2. Prompting Can’t Override Statistical Priors


LLMs are trained on huge distributions full of misconceptions. Even strong system prompts can’t reliably override them, not when misinformation is more common in the pretraining data than truth.


Example: On TruthfulQA, large models still confidently produce myth over fact ~80% of the time.


These failures aren’t random: they’re *structural* . And structural problems require structural solutions.


#### The Breakthrough: Runtime Intervention in Feature Space


While studying the DeepSeek-R1 model, our team discovered something important:


models often know the truth, but a set of internal features suppresses or distorts it.


By identifying and manipulating these latent feature vectors, bias vectors, misconception vectors, censorship vectors, we can surface grounded knowledge and suppress confabulation.


This insight became the foundation for Mentat.


#### Mentat: The First Runtime Policy Engine for LLMs


Mentat introduces two core innovations that make deterministic control possible:


##### 1. Feature-Level Intervention (Activation Steering)


Instead of prompting the model to follow rules, we modify behavior directly at the activation level during the forward pass.


We identify the latent vector *v* responsible for a behavior (e.g., misconception, bias, refusal), then apply a controllable intervention:


> ` h′ = h – α * (h @ v) * v`


This lets us remediate undesirable behavior in real time by: suppressing confabulation, elevating factual reasoning, removing style or bias features, undoing knowledge suppression. Because this happens as a lightweight arithmetic operation on hidden states, overhead stays under 10ms per token. No fine-tuning. No brittle prompts. No unpredictable “guardrails.” Just deterministic steering.


##### 2. Graph-Based Verification (Deterministic Safety Layer)


Beyond internal steering, Mentat runs each response through a verification pipeline to ensure compliance, accuracy, and internal consistency.


###### ✓ Semantic Entropy Check


Detects uncertainty or babbling before it reaches the user.


###### ✓ Knowledge Graph Cross-Reference


Cross-checks claims against a structured graph to catch hallucinations that RAG typically misses, especially relational errors.


###### ✓ Policy Adjudication


Policies, from brand guidelines to financial regulations, are compiled into weighted vectors. Mentat then adjudicates between them deterministically, ensuring the output always conforms to organizational policies.


This is the backbone of Mentat as a policy engine: a system that enforces your standards automatically and consistently, across every generation.


#### Results: Accuracy, Robustness, and Reliability at Scale


Across internal and third-party evaluations, Mentat significantly improves factual accuracy and reasoning robustness:


- TruthfulQA (Misconceptions): *21% → 70% accuracy* after suppressing misconception features.


- HaluEval-QA (RAG/Reasoning): *96.50% accuracy* , solving the spatial inversion errors where base models failed.


- Real-world Inputs: Mentat infers entities from noisy queries (“David Of me” → *David Icke* ) where base models break.


For enterprise workflows, legal, finance, healthcare, compliance, the improvement is transformational.


#### Introducing the Mentat Endpoint


A drop-in compatible replacement for /v1/chat/completions.


Swap your base URL and immediately upgrade your reliability.


api.ctgt.ai/v1/chat/completions


*(Full docs available in the dashboard.)*


The public endpoint ships with our Base Policy, optimized for hallucination reduction, factuality, and corporate safety.


#### Try It in the Mentat Playground


We built a visual playground where you can:


- compare “Ungoverned” vs. “CTGT Governed” outputs side-by-side,
- inspect the intervention deltas in real time,
- test different policy personas (e.g., Investment Banking Analyst),
- explore how feature-level intervention remediates model behavior.


No signup required.[Try it here.](https://playground.ctgt.ai/)


Here’s a quick 2-minute demo video showing the process:[Watch video here](https://video.ctgt.ai/video/ctgt-ai-compliance-playground-cfnl)


#### Why This Matters


As LLMs move into high-risk environments, organizations can’t rely on best-effort guardrails. They need deterministic, inspectable, and enforceable systems. Mentat gives teams the ability to remediate incorrect or noncompliant model behavior with: real-time corrections, policy-consistent output, predictable internal logic, and transparent reasoning. This is how enterprise AI should work.


#### We’re Just Getting Started


We’re excited to finally share Mentat with the world.


If you’re building where correctness, compliance, or trustworthiness matter, we want to hear from you.


Validate the solution. Stress-test our interventions. Try to break the system.


Your feedback will help us refine the next wave of deterministic AI infrastructure.
