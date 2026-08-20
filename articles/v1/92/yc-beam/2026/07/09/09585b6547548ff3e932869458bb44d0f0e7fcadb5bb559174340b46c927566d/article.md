---
schema_version: "1.0.0"
document_id: "09585b6547548ff3e932869458bb44d0f0e7fcadb5bb559174340b46c927566d"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/tinker-model-pricing"
published_at: "2026-07-12T18:29:03+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:ba121eaae1a012a0f10555811c775a4e68628f5c3d6f88e962448c5661742287"
---

# Tinker Model Pricing: What Fine-Tuning Costs in 2026

[← All posts](https://www.beam.cloud/blog) Engineering


# Tinker Model Pricing: What Fine-Tuning Costs in 2026


Tim Huynh


July 12, 2026


8 min read


Tinker, the fine-tuning API from Thinking Machines, charges per million tokens instead of per GPU-hour, split across three meters: prefill (input processing), sample (generation), and train (forward + backward pass).


Training Qwen3-8B costs $0.40 per million tokens today and $0.44 after a price increase on July 17, 2026. That structure makes Tinker cheap for bursty experiments and large mixture-of-experts models, and roughly 2–4x more expensive than raw GPU compute for small models you could run yourself.


## How Tinker pricing works


Tinker prices fine-tuning the way most providers price inference: you pay for tokens, not hardware. Every operation in a training run lands on one of three meters.


- **Prefill** — input tokens pushed through a forward pass. The cheapest meter.
- **Sample** — generated tokens (forward pass plus sampling). Used heavily during evaluation and reinforcement learning rollouts.
- **Train** — tokens that go through a forward and backward pass for gradient updates. This is the meter your dataset size multiplies against.


There are four key details that impact the bill more than the prices listed:


- **MoE models are priced on active parameters, not total size.** A 397B mixture-of-experts model with a 17B active subset costs a fraction of what a dense model of similar quality would. This is the biggest cost lever on the platform.
- **Cached prefill is discounted 80%.** Input tokens that hit the prompt cache cost a fifth of the listed prefill rate.
- **Storage is $0.10 per GB per month** , and new users get **$150 in free credits** which is enough to cover several small experimental runs.
- **Prices go up on July 17, 2026.** Prefill and sample rates rise about 50% and train rates about 10%, which Tinker attributes to rising compute costs. RL-heavy workloads, which sample constantly, are the most impacted.


Tinker launched free in October 2025, switched to usage-based pricing that November, went general availability in December, and retired its entire launch lineup (the Llama-3.x family, Qwen3-32B, Qwen3-235B) in June 2026. The prices change often so treat every number here as a July 2026 snapshot and check the live pricing page before making a buying decision.


## Tinker pricing table (July 2026)


Rates in dollars per million tokens, showing current prices and the July 17, 2026 prices.


Model Type Train (now → Jul 17) Sample (now → Jul 17)


Qwen3-8B Dense $0.40 → $0.44 $0.40 → $0.60


GPT-OSS-20B MoE $0.36 → $0.396 $0.30 → $0.45


GPT-OSS-120B MoE $0.52 → $0.737 $0.44 → $0.84


Qwen3.5-9B Dense $1.33 → $1.463 $1.33 → $1.995


Qwen3.6-27B Dense $3.73 → $4.103 $3.73 → $5.595


Qwen3.6-35B-A3B MoE $1.07 → $1.177 $0.89 → $1.335


DeepSeek-V3.1 MoE $3.38 → $3.718 $2.81 → $4.215


Qwen3.5-397B-A17B MoE $6.00 → $6.60 $5.00 → $7.50


Kimi-K2.6 MoE $4.40 → $4.84 $3.66 → $5.49


Nemotron-3-Ultra-550B-A55B MoE $4.98 → $5.478 $4.15 → $6.225


Compare the two 27–35B-class rows: the dense Qwen3.6-27B trains at $3.73/M while the MoE Qwen3.6-35B-A3B, priced on its 3B active parameters, trains at $1.07/M. Extended-context variants (128K/256K) run roughly 2–4x the base-context price.


## What a fine-tuning run actually costs


A small supervised fine-tune is tens of dollars, but a large-MoE run can be in the hundreds. The math is dataset tokens times epochs times the train rate, plus whatever you sample for evaluation.


- **Small SFT** — Qwen3-8B over ~50M training tokens (roughly 10–15k examples for a few epochs): 50 × $0.40 = **$20** today, $22 after July 17.
- **Medium SFT** — Qwen3.6-35B-A3B over 100M tokens: **$107** , rising to $118.
- **Large MoE** — Qwen3.5-397B-A17B over 50M tokens: **$300** , rising to $330.
- **RL costs more than the token math suggests.** Reinforcement learning alternates rollouts (billed at the higher sample rate) with updates (billed at train). A reasoning-RL loop that samples heavily can be dominated by sample tokens, which is the cost rising 50% in July.


Community data points are sparse but consistent with these estimates: one published Qwen3-8B SFT tutorial (LoRA rank 32, 7,244 examples, four epochs, about three hours including failed runs) consumed roughly $25 of the $150 starter credits. Treat these figures as anecdotes, not benchmarks.


Tinker doesn't attribute costs per run, so if you plan to model production economics, track train, sample, and prefill tokens per experiment yourself from day one.


## Why Tinker only does LoRA


Tinker never does full fine-tuning. Every run trains a LoRA adapter (rank 32 by default) on a shared copy of the base model. That constraint is the economic engine, as one set of base weights in GPU memory serves many customers at once, and you only pay for the adapter math.


The lab's own research, the "LoRA Without Regret" paper (Schulman et al., September 2025), argues LoRA matches full fine-tuning when adapters cover all layers, including MLP and MoE layers, and the run isn't capacity-constrained — that is, trainable parameters exceed the information in the dataset. Hugging Face's TRL team reproduced the result independently, reporting that a correctly configured LoRA run matched full fine-tuning while using about two-thirds of the compute.


For most post-training — instruction tuning, style transfer, domain adaptation on modest data — that means the LoRA-only limit costs you little. The workloads it legitimately rules out are continued pretraining and very large-data domain adaptation, which exceed the low-regret regime. If you need full-parameter updates, Tinker isn't an option at any price, and something like the setup in our[Llama 3 fine-tuning guide](https://www.beam.cloud/blog/fine-tuning-llama3) on rented GPUs is the path instead.


## Per-token pricing vs GPU-hours


Per-token pricing shifts utilization risk from you to the provider. On a rented GPU you pay wall-clock time whether the card is computing gradients or sitting idle while your dataloader hiccups, you debug a shape mismatch, or a checkpoint writes. On Tinker, idle time is free — you pay only for tokens that move through a meter.


That allocation favors different teams:


- **Low duty-cycle wins on Tinker.** A research team running short, bursty, frequently interrupted experiments would waste most of a rented GPU-hour. Per-token pricing means a failed run costs only the tokens it consumed.
- **High utilization wins on rented hardware.** The per-token rate has the provider's GPU cost, margin, and everyone else's idle time baked in. If you can keep a card saturated, you're overpaying for insurance you don't need.


The crossover sits around 40–50% sustained GPU utilization for single-node jobs. Below that, Tinker's meter is usually cheaper than the hours you'd burn; above it, the raw compute wins.


## When renting GPUs beats per-token pricing


For any model that fits on one GPU, the raw-compute floor is far below Tinker's rates — the decision is whether the orchestration Tinker removes is worth the spread. A LoRA fine-tune of an 8B model runs comfortably on a single A100, and with optimized kernels like[Unsloth](https://www.beam.cloud/blog/unsloth-fine-tuning) it processes several thousand tokens per second. At a conservative 14M tokens/hour on an A100 80GB rented at $1.30/hr on Beam, DIY works out to about **$0.09 per million tokens against Tinker's $0.40** — the same 50M-token job that costs $20 on Tinker is roughly $4.55 in GPU time.


The heuristic: divide your GPU hourly rate by achievable tokens per hour to get a DIY $/M, and compare it to Tinker's train rate. Self-hosting wins when that number is lower and you have working training code — if you already run Axolotl, Unsloth, torchtune, or TRL scripts, Tinker's managed loop mostly buys you a rewrite against its primitives.


Renting the GPU is also the answer in cases that aren't about price:


- **Full fine-tuning or continued pretraining.** Tinker can't do them at all.
- **Serving the model afterward.** Tinker's sample meter is priced for evaluation, not production traffic. Export the adapter and serve it on an autoscaling[serverless GPU](https://www.beam.cloud/blog/serverless-gpu) endpoint, where per-token cost at volume is far lower and the hardware scales to zero between requests.
- **Compliance today.** Tinker commits to not training on your data, but no public SOC 2 attestation exists yet and enterprise controls (SSO, audit trails, data residency) are still being built. Beam is SOC 2 Type II with a self-hosted/BYOC option that keeps data in your own VPC.


That said, Tinker is the winner for very large MoE models (235B–550B class) that need multi-node InfiniBand orchestration and failure recovery you'd otherwise have to build.


## Lock-in, portability, and what migration costs


Weights are portable, but your training code is not. Tinker exports a standard PEFT adapter (usable directly with vLLM or SGLang) or a merged Hugging Face model, so a fine-tuned model trained on Tinker can be served anywhere — including hot-swapped onto your own GPUs. What doesn't transfer is code written against Tinker's training primitives (` forward_backward` ,` optim_step` ,` sample` ), which you'd re-implement on torchtune or TRL to move training off the platform. Starting on Tinker and migrating recurring jobs later is a real strategy that is worth considering.


## Tinker vs renting GPUs, compared


Tinker On-demand GPUs (e.g. Beam)


Pricing model Per million tokens (train / sample / prefill) Per second of GPU time


Method LoRA only, rank 32 default Anything: LoRA, QLoRA, full FT, pretraining


Model access Curated list, retired on Tinker's schedule Any open-weights model


Idle time Free — provider absorbs it Billed — you own utilization


Small-model cost (8B, 50M tokens) ~$20–22 ~$4.55 in A100 time


Large MoE orchestration Managed, included Yours to build (multi-node reserved)


Compliance No public SOC 2 yet; controls in progress SOC 2 Type II; BYOC/self-host available


Best for Bursty experiments, big MoE, no infra team Recurring jobs, full FT, serving, regulated data


A sensible enterprise strategy uses both: validate a recipe on Tinker's $150 credit, and once a fine-tune becomes a recurring production job on a single-GPU-fittable model at decent utilization, export the adapter and move it to on-demand hardware.


## FAQ


**Is Tinker free to use?** No. It launched free in October 2025 but moved to usage-based pricing that November. New users get $150 in credits, which covers several small experimental runs (a published 8B fine-tuning tutorial used about $25 of it, including failed runs).


**How much does it cost to fine-tune an 8B model on Tinker?** Around $20 for a typical supervised fine-tune of Qwen3-8B over 50M training tokens at the current $0.40/M train rate, rising to about $22 when prices increase on July 17, 2026. Evaluation sampling adds to that at $0.40–$0.60/M.


**Does Tinker support full fine-tuning?** No. Every run trains a LoRA adapter (default rank 32). Tinker's research argues correctly configured LoRA matches full fine-tuning for most post-training, but continued pretraining and large-data domain adaptation are out of scope — for those you need your own GPUs.


**Can I export my fine-tuned model from Tinker?** Yes. Tinker exports a standard PEFT adapter or a merged Hugging Face model, so you can serve the result on vLLM, SGLang, or any GPU platform. Lock-in is limited to training code written against Tinker's API primitives.


**Is Tinker cheaper than renting GPUs?** For small models at sustained utilization, no — DIY LoRA on a rented A100 runs roughly $0.09 per million tokens versus Tinker's $0.40 for an 8B model, a 4–5x spread. Tinker is the cheaper path for bursty low-utilization experimentation and for large MoE models whose multi-node orchestration you'd otherwise have to build and staff.


Fine-tune on your own terms: export the adapter, rent the GPU by the second, and pay for gradients instead of tokens.[Get started free on Beam](https://www.beam.cloud/?utm_source=blog&utm_campaign=tinker-pricing) — new accounts include $30 in monthly credit.


Tim Huynh


Published July 12, 2026


Keep Reading


## More from the Beam blog


[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)[Engineering Sandboxes for Reinforcement Learning Run RL environments as isolated sandboxes: snapshot once, fan out thousands of parallel rollouts, and score verifiable rewards. Tim Huynh](https://www.beam.cloud/blog/sandboxes-reinforcement-learning)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
