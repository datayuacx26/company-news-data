---
schema_version: "1.0.0"
document_id: "434b738660165520f4a8abd81986ba7e4e279248af683c5d614a0ba4eb0f4103"
company_key: "yc-latent"
company: "Latent"
source_id: "yc-latent-news-import-a5344ee3e9d9"
canonical_url: "https://latenthealth.com/blog/adaptive-llms-for-medical-question-answering-via-evolution"
published_at: "2025-05-14T00:00:00+00:00"
first_seen_at: "2026-07-25T11:49:50.012604+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:15adfdb44312d582a13020bfa62b88df2f151e657f8a7e06a0623a496bfc5888"
---

# Adaptive LLMs for Medical Question Answering via Evolution

Rather than blindly adjusting millions of weights, our evolutionary process intelligently perturbs these building blocks—analogous to editing genes, not neurons.


#### How Evolution Works in Practice


1. **Population Diversity:** We implement MAP-Elites (Mouret & Clune, 2015) for our population management, using number of unique actions and advice snippets as our axes—ensuring that we illuminate, not collapse, the search space.
2. **LLM-Powered Mutation:** Using the LLM as a meta-mutator, we intelligently mutate individuals: adding/removing actions, rewording prompts, generating new advice, and combining strategies (“crossover”) from top performers.
3. **Evaluation:** For each mutated individual, we:


- Execute its EHR search actions to gather input
- Build and submit its unique prompt + advice to the answering LLM
- Score outputs for correctness vs. expert labels


4. **Selection:** The best candidates, by both diversity and score, persist; weaker ones are discarded


#### Training & Evaluation Procedure


Using these techniques, we begin with a seed individual and simulate evolutionary dynamics over time. The “individuals” explore novel search spaces while learning the underlying preferences of our retrieval system. Notably, these answerers develop an understanding of pharmacological preferences and clinical biases (e.g. selecting ASCVD over hyperlipidemia in our earlier example).


To robustly quantify gains while avoiding overfitting—a common weakness in prompt-tuning efforts—we use a stratified *train/validation/test* split:


- **Train:** ~200 carefully sampled questions (specific drug class, question type, distribution of edge cases)
- **Validation/Test:** 300/500 similarly diverse questions from the same question space


Fitness is measured over the training set, with individuals having no access to validation or test samples. Overall performance is reported on the validation set, ensuring true generalization rather than memorization. At the conclusion of training, you are left with your optimal individuals over your training set; an interpretable set composed of \[prompt, action, advice\] triplets. Deploying these to production is as simple as subbing in this optimal scaffolding.


### Results and Insights


We’re able to consistently outperform prompt engineering (DSPY) and match or beat distillation baselines. Uniquely, all that is required is the selection of the question subset**, and clicking “run”. No GPUs needed; no more dealing with OOMs, or paying training compute providers half your net worth. A few minutes(!) later, we have a highly specialized scaffolding that has jointly discovered the nuances of the clinical pathways and our search tools.


**Advantages**


- **True Joint Optimization:** Unlike DSPY and similar frameworks, our evolutionary setup adapts both data retrieval *and* LLM reasoning, uncovering strategies like “prefer ASCVD over hyperlipidemia for PCSK9 prior auth” without ever providing that insight to our algorithm.
- **Semantic Interpretability:** Each individual—composed of explicit actions, human-auditable prompts, and explainable example cases—is easily comprehensible and transferrable across related tasks.
- **Rapid Adaptation:** As payer guidelines or clinical priorities change, new advice snippets or search tweaks emerge organically—no “prompt surgery” or system retraining required.
- **Compositionality and Domain Alignment:** The best solutions naturally balance lean, high-yield information gathering, targeted clarifications for edge cases, and increasingly domain-infused prompts—often matching and exceeding, expert-crafted policies.
