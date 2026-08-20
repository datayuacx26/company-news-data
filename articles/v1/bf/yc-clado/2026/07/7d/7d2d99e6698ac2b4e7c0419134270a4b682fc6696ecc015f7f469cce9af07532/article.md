---
schema_version: "1.0.0"
document_id: "7d2d99e6698ac2b4e7c0419134270a4b682fc6696ecc015f7f469cce9af07532"
company_key: "yc-clado"
company: "Clado"
source_id: "yc-clado-news-import-b41b19aea1cc"
canonical_url: "https://www.clado.ai/blog/reinforcement-learning"
published_at: null
first_seen_at: "2026-07-25T00:07:03.711311+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:c9e8e9f63f659ad54e4379c7803a8435425cf69e8e15cfcba38a89fa7aef2ca8"
---

# How We Train Models at Clado

# How We Train Models at Clado


featured •


Published on November 3, 2025 at 6:00 PM


# How We Train Models at Clado


*03 Nov, 2025*


**TLDR:** reinforcement learning cut our monthly inference costs by 98.32%, improved performance on our internal benchmark, and reduced latency by 66%.


## 1) Context


Clado builds people search infrastructure, which involves indexing hundreds of millions of LinkedIn profiles and training models to first query and then filter through the rest to find the best matches for a natural language prompt. This is done with three models.


1. **Prompt → Criteria** (e.g., find me software engineers in sf → is software engineer && is in sf)
2. **Criteria → DSL** (Elasticsearch Query)


- Our summer intern wrote a[great blog](https://ahamidi.me/clado/) on why the DSL step was necessary instead of just text2sql.


3. **Filtering.** Given a profile, does it fulfill each criterion? Why or why not?


We trained custom models for each step, but I will ignore the first and third for the rest of this, since they were just simple fine-tuning on a GPT OpenSource model.


## 2) Constructing a Golden Dataset


We collect training data by mining real user prompts and turning them into trusted (prompt → DSL) pairs. A large "oracle" model (GPT-5) drafts the DSL, we run it against the database, and throw away anything that errors or returns obviously useless results. What's left are labelled examples with the prompt, the accepted DSL, and a few basic stats (did it run, how many rows came back).


From there, we do a straightforward train-test split (70% for training, 30% held out to watch for overfitting), then fine-tune a GPT OSS 120B model with LoRA adapters. We rerun this workflow whenever the schema or user behavior changes, so the dataset stays fresh without manual labeling. The only metrics we track at this stage are simple: "does it run," "is the result size reasonable," and "did we include the obvious filters?" This gives us a labelled "golden dataset" to evaluate our search model against.


**Side note:** The better alternative to this synthetic approach is a human-labelled dataset. The risk of this approach is that we are blindly trusting the oracle model, with a bit of scaffolding, as the ground truth for this task. Unfortunately, we did not have the resources or time to generate a high-quality human-labeled dataset.


## 3) Supervised Fine Tuning (Cold-Start)


Our RL loop is simple: for each prompt, we sample a few query → dsl pairs, run them, and rank the outputs. That costs real compute: every bad sample still hits the judge and often the database. If we start from an untrained model, most candidates don't even run, so we burn cycles scoring garbage and get noisy learning signals.


Luckily, we can learn from Deepseeks' training runs for the R1; they found huge cost/compute savings by doing light supervised fine-tuning on a base model before starting RL.


*Diagram from[this article](https://makingaieasy.substack.com/p/why-i-think-deepseek-r1-just-revealed) explains this concept.*


By first getting the model to produce mostly runnable, intent-matching DSL (via the golden set), RL spends its budget separating "good" from "better," not "broken" from "barely usable." That means fewer samples per prompt, fewer failed executions, more stable rewards, and faster convergence. Essentially, warming up the model so RL can focus on precision and coverage rather than syntax.


*Rented 8xH100 on Runpod for all training*


## 4) RL: Openpipe ART + RULER for rewards


In a nutshell: sample a few DSLs for a prompt, judge/rank them by how well they run and how well they match the intent, then nudge the model toward the better ones next time.


In practice (with OpenPipe ART), we train on small batches of real prompts (3 prompts/step). For each prompt, the model proposes 8 possible DSLs. For each DSL, we execute it and also collect LLM-as-judge feedback. OpenPipe's RULER combines these signals to rank candidates, and GRPO (group relative preference optimization) updates the model to prefer the higher-ranked ones.


*Sample Trajectory*


The reward is practical: it would take into account whether the DSL was executed (since we solved the cold start problem with SFT, this should mostly be in the clear), whether the results are relevant, and the quantity of results returned during the rollout. The following equation represents the prompt entered in RULER to calculate DSL quality.


```text
final_score = (quantity_weight · quantity_value) + (1 − quantity_weight) · quality_value


quality_value = total_score / max_possible_score


quantity_value = tanh(profiles_above_threshold / 25)


```


### Why RULER:


RULER basically does the boring parts for you. It takes multiple signals (execution success, whether required filters were present, whether the query seems too broad, and an LLM-as-judge score) and combines them into a single ranking between "candidate A" and "candidate B." That plugs directly into the policy optimization algorithm, and in our case, GRPO.


You can skip RULER and just define a numeric reward yourself as a formula that mixes result count, coverage, penalties, etc. That gives you more direct control, and it's nice because you can tune weights and see exactly why a query was rewarded. The downside is you end up rebuilding logic the judge is already doing ("is this actually relevant?"), and you still have to deal with edge cases like "query runs but is useless."


In our case, most of the signal is already "prompt + LLM as a judge." We're not doing something like robotic control where the reward is purely numeric. So, letting RULER bundle those judge signals and rank candidates got us similar performance vs the hand-tuned formula.


*Loss over Training Steps*


We keep the loop honest with lightweight observability in Weights & Biases: per-step win rate vs. the baseline, execution success rate, histograms of result-set sizes (to catch overly broad queries), and simple regex/AST checks for missing WHERE clauses or bad joins.


## 5) Evals: Judgment Labs


We use an LLM as a judge to scale the non-deterministic part of evaluation (i.e., whether the final results match the user query) because obtaining labeled pairs is time-consuming. But every candidate also has to pass hard checks: the SQL must execute, obey the schema, and return a reasonable number of rows relative to the requested number.


The premise of LLM-as-judge seemed sketchy to us at first, since we are basically asking the completions model to grade itself on the task. Initially, you'd expect this to lead to hallucinations, but it never did during our experiments. This is largely explained by the asymmetry of verification: just like in P vs NP problems, where checking a candidate solution is easier than discovering it, it's easier for a model to grade whether an answer is strong than to produce the best response itself. This mirrors the same natural property humans possess - e.g., it is easier for you to check whether a Sudoku solution is valid than to produce it from scratch (credit to Andrew Li from Judgment Labs for this explanation)!


Therefore, an eval is something we run that is powered by an LLM, along with quantitative equations that judge how good the results we returned are, combined with the number of results returned. For each model trained for a different purpose, a separate test is designed to provide a quantitative measure of its performance. This algorithm/scorer can be used in production as well as with a testing set. At the end of every query, we would run a judge on the first five results and combine it with the total hits to understand how well we did for that specific query, then put it into buckets for further development and reference.


In addition, the scorer could also be used in conjunction with a testing set. To construct our evaluation testing set, we embedded all our customer queries, clustered them, and used the centroid of each cluster as our evaluation set to ensure the widest variety of results. This would help us decide whether to run RL on a newer model, since the scorer can also be applied to the base model to assess its performance. Furthermore, the eval can also help us determine whether the post-RL model actually performs better, since, due to inaccurate tuning, the resultant model can sometimes perform worse for reasons such as entropy collapse, KL collapse, or general reward hacking.


*A snapshot of eval scores in Judgment Labs*


We found that keeping a handful of "failed prompts" that once broke the system and plotting their pass rates across SFT → RL → APO was extremely helpful for catching drift.


## 6) Auto Prompt Optimization (DSPy)


After RL, we do a quick clean-up pass with DSPy to tune the system prompt for the Criteria→DSL model. We treat the prompt like parameters: compile new prompts, score them with the same signals we already trust (does the SQL execute, do we cover the required fields, what's the RULER/judge score), and keep changes only if they help. We iterate until the gains plateau, with guardrails to prevent APO from drifting into behaviors RL has already fixed (e.g., re-introducing overly broad queries).


## 7) Results


Model Quality Score Avg Tokens Avg Response Time Avg Run Cost ($)


GPT 120B Raw 0.65 639 10s 0.00016


GPT 120B SFT + RL 0.81 448 7s 0.000112


O3 Raw 0.72 832 20.8s 0.006656


For 300k runs, O3 costs $1996.8; our trained GPT 120B costs $33.6, a **98.32% monthly cost reduction** while improving performance on our internal benchmark and reducing latency by 66%.


---


Written by: Eric Mao, David Shan, Tom Zheng
