---
schema_version: "1.0.0"
document_id: "215ac7870519fb5c74e63d689f7357b42bdc29e6f52496243dbb68e1e0414ba5"
company_key: "yc-refresh"
company: "Refresh"
source_id: "yc-refresh-news-import-082559068c7a"
canonical_url: "https://refresh.dev/blog/gauntlet-4k-rlvr"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-07-24T00:40:30.303350+00:00"
fetched_at: "2026-07-28T22:19:59.143850+00:00"
content_hash: "sha256:fbe3ea9cd9c817596e53271fd953b9e74bec8112470e31fd768e654bdc80dfd6"
---

# Gauntlet 4K RLVR

An RLVR dataset for teams training agentic coding models with reinforcement learning.


While supervised fine-tuning gets you far, agentic reinforcement learning with verifiable rewards (RLVR) is how you push models to actually solve problems. We've curated[Gauntlet](https://www.refresh.dev/blog/gauntlet-80k) into a focused 4,000-example RLVR dataset, delivered in[Harbor format](https://harborframework.com/) for seamless integration with agentic RL training pipelines.


We trained[EssentialAI's rnj-1-instruct 8B](https://huggingface.co/EssentialAI/rnj-1-instruct) , an agentic coding model that punches well above its weight class, using the[Harbor framework](https://harborframework.com/) , with rollouts pushed to[SkyRL](https://github.com/NovaSky-AI/SkyRL) . We saw a **3x improvement** on Terminal Bench 2.0, an out-of-distribution benchmark the model never saw during training.


01


## What's in Gauntlet 4K RLVR


Gauntlet 4K RLVR is a carefully curated subset of the original[Gauntlet dataset](https://www.refresh.dev/blog/gauntlet-80k) , optimized for reinforcement learning workflows. Each example provides a verifiable reward signal through pytest tests.


4,000


Examples


Curated for RL training efficiency.


Harbor


Format


Ready for RL pipelines.


Pytest


Verification


Binary reward signals.


Harbor Format


Delivered in[Harbor format](https://harborframework.com/) , the dataset integrates directly with modern RL training frameworks. Each example includes the task prompt, verification tests, and pre-configured Docker environments.


02


## Training Setup


We trained[rnj-1-instruct 8B](https://huggingface.co/EssentialAI/rnj-1-instruct) using the **terminus-2 harness** with our Gauntlet 4K RLVR dataset. After GRPO showed instability during training, we switched to the **DAPO algorithm** which provided consistent convergence.


rnj-1-instruct


Base Model


8B parameters


80 hours


Training Time


8x A100 GPUs


LoRA


Method


Rank 64


DAPO


Algorithm


Stable RL optimization


Component Details


Harness terminus-2


Dataset Gauntlet 4K RLVR


Hardware 8x NVIDIA A100


Training Duration 80 hours


LoRA Rank 64


Algorithm DAPO (GRPO was unstable)


03


## Terminal Bench 2.0 Results


We evaluated on **Terminal Bench 2.0** , an out-of-distribution benchmark the model never saw during training. This tests whether the model learned generalizable coding skills, not just pattern matching on the training data.


The trained 8B model shows outsized performance for its size, achieving results comparable to 20B+ parameter models. Below we compare against other models evaluated on the same terminus-2 harness.


3x


Problems Solved


3 → 9 problems


+197%


Pass@1 Rate


3.4% → 10.1%


TERMINAL BENCH 2.0 RESULTS


### Terminal Bench 2.0 Comparison


Terminal Bench 2.0 pass@1 score by model (percent) Category Score


GPT-OSS-20B* 3.1%


Essential AI rnj-1 8B Base 3.4%


GPT-5-Nano* 7.9%


Essential AI rnj-1 8B + Gauntlet 10.1%


Grok Code Fast 1* 14.2%


Score


Model Score Source


GPT-OSS-20B* 3.1% ± 1.5 Leaderboard


Essential AI rnj-1-instruct 8B (Baseline) 3.4% (pass@1) Our run


GPT-5-Nano* 7.9% ± 1.9 Leaderboard


Essential AI rnj-1-instruct 8B + Gauntlet 4K RLVR


10.1% (pass@1)


Our run


Grok Code Fast 1* 14.2% ± 2.5 Leaderboard


*Results pulled from the[Terminal Bench 2.0 leaderboard](https://www.tbench.ai/leaderboard/terminal-bench/2.0) .


**Note:** Terminal Bench 2.0 is an out-of-distribution benchmark with 89 terminal-based coding tasks. The model was never exposed to these tasks during training, demonstrating genuine skill transfer from the Gauntlet RLVR training.


04


## Summary


Key Numbers


Dataset size 4,000 curated examples


Format Harbor


Verification Pytest binary rewards


rnj-1-instruct 8B Terminal Bench pass@1 3.4% → 10.1% (3x)


Training 80h on 8x A100, DAPO + SkyRL + Harbor


RLVR works. With just 4,000 carefully curated examples and verifiable rewards, we achieved a 3x improvement on an out-of-distribution benchmark. The key is quality over quantity, and rewards you can trust.


Interested in training on Gauntlet 4K RLVR?[Book a call](https://calendar.app.google/KXAGzSZpiNqKoX636) .
