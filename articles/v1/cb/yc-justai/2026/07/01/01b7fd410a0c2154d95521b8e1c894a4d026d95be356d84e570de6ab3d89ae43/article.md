---
schema_version: "1.0.0"
document_id: "01b7fd410a0c2154d95521b8e1c894a4d026d95be356d84e570de6ab3d89ae43"
company_key: "yc-justai"
company: "JustAI"
source_id: "yc-justai-news-import-e4de3da632ca"
canonical_url: "https://www.getjust.ai/blog/how-justai-uses-reinforcement-learning-and-llm-auto-tune-to-power-smarter-marketing"
published_at: null
first_seen_at: "2026-07-25T10:24:54.291702+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:7b9998fb132a5ecfbf733e20333f66996df51fbc454baafe7b6fdd4d58d89e09"
---

# How JustAI uses reinforcement learning to power AI marketing

Modern marketing moves too fast for slow, one-off experiments. Campaign performance changes daily, audiences shift weekly, and new creative ideas drop constantly. If your optimization strategy can’t keep up, you’re leaving money on the table.


At Just Words, we’ve built an AI-first decisioning platform that **continuously learns and adapts** , in real time, using reinforcement learning (RL) and large language model (LLM) optimization. This means better personalization, faster learning cycles, and measurable lift without additional manual testing overhead.


### The Core Problem: Exploration vs. Exploitation


Marketers face the classic **multi-armed bandit problem** :


-


**Exploitation:** Show the best-known message, based on past performance, to maximize short-term results.


-


**Exploration:** Test new or less-proven ideas to uncover tomorrow’s winners.


Traditional A/B testing fixes traffic splits and runs for weeks. That’s fine for academic experiments, but in the real world it:


-


**Misses short-term opportunities** when a creative starts winning mid-test.


-


**Fails to adapt** when audience behavior changes.


-


**Treats all audiences the same** , ignoring differences in context.


We solve this with two complementary RL approaches:


#### 1. Weighted Thompson Sampling - Statistical Optimization


WTS is our **default scorer** for campaigns where audiences are well-defined and we care about global winners.


**How it works:**


-


**Bayesian Modeling:** Each creative’s success rate is modeled as a Beta(α, β) distribution, updated as we see opens, clicks, or conversions.


-


**Multi-Metric Weighting:** We can blend multiple metrics (open rate, CTR, downstream conversions) into a single weighted score.


-


**Bias Correction:** Median-based adjustments normalize performance across campaigns with different baselines.


-


**Graceful Degradation:** Neutral priors ensure that sparse data doesn’t cause wild swings.


**When to use it:**
Best for global campaigns or well-segmented lists where creative performance is relatively stable.


#### 2. Disjoint Linear Thompson Sampling- Contextual Optimization


DLTS is a **contextual bandit** , it doesn’t just learn which creative is best overall; it learns which creative is best for **this** user **right now** .


**How it works:**


-


**Feature Encoding:** Compress user attributes and campaign context into a high-dimensional vector (e.g., user type, lifecycle stage, device, time of day).


-


**Per-Variant Models:** Each creative variant gets its own linear model:
` Reward ≈ θᵀx + noise`


-


**Bayesian Updates:** We maintain uncertainty in each model’s parameters using normal-inverse-gamma priors.


-


**Posterior Sampling:** For each decision, we sample from the posterior to balance exploration and exploitation.


**When to use it:**
High-cardinality or high-variability campaigns, e.g., the same subject line might perform differently for SMB vs. enterprise, or for morning vs. evening sends.


#### 3. Auto-Tune for LLM Optimization - Closing the Loop


Picking the right creative variant is only half the story. We also need to **generate** new, high-performing ideas, without human bottlenecks.


That’s where **Just Words Auto-Tune** comes in.


**The Challenge:**
Even the smartest bandit will plateau if the creative pool never changes. Manually crafting new variants is slow, and guessing what will work is risky.


**Our Solution:**
We use LLMs to automatically generate and refine content, guided by live performance data from our bandit models.


**How it works:**


1.


**Performance Feedback Loop:**
The RL layer sends “winning patterns” to the LLM — e.g., tone, length, keywords, emotional framing that are over-indexing.


2.


**Prompt Engineering with Guardrails:**
We feed the LLM structured prompts with brand voice, compliance rules, and campaign goals.


3.


**Controlled Variant Generation:**
Auto-Tune produces new candidates that differ enough to explore, but stay on-brand and compliant.


4.


**Live Testing:**
New variants are inserted into the bandit’s pool for real-time evaluation.


5.


**Iterative Refinement:**
The process repeats — only better-performing styles survive.


**Result:**
Continuous creative evolution, fully automated, with the bandit + LLM working as a closed-loop optimization engine.


### Why This Matters for Marketing, Data, and Eng Teams


**For Marketing:**


-


No more waiting weeks to declare a winner.


-


Messaging adapts to audience segments automatically.


-


Creative refreshes happen continuously without manual effort.


**For Data:**


-


True causal lift measurement in production.


-


Multi-metric scoring aligns optimization with business goals.


-


Contextual models capture non-obvious audience behaviors.


**For Engineering:**


-


Sub-100ms decision latency in production.


-


Scalable to millions of daily decisions.


-


Modular architecture supports plugging in new scoring strategies or content generators.


### The Impact in Production


Across millions of daily content decisions, our RL + Auto-Tune system has delivered:


-


**Double-digit engagement lifts** vs. static control groups.


-


**Faster time-to-winner** - often hours instead of weeks.


-


**Higher creative diversity** without off-brand risk.


It’s marketing experimentation on autopilot, but with the rigor of data science and the scale of modern engineering.
