---
schema_version: "1.0.0"
document_id: "e68327684a9c0f4c4b7cdfdee812d8c36bffe168f8237894488b4e7ce95f7433"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/introducing-kernelarena"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:5fe11f8a5ea78f3fc8afc853e46cb0d3821d507962dfb8611260171c3288cbb0"
---

# Introducing KernelArena

Frontier models are getting surprisingly good at writing GPU kernels. We at Wafer have used agents to develop many SOTA kernels. A year ago this felt impossible; now it’s[becoming routine](https://x.com/marksaroufim/status/2009497284418130202) . The question has shifted from “can they do it?” to “which one does it best?”


Most people answer that question by feel. You try a model for a few tasks, form an opinion, and move on. The industry at large does roughly the same thing. This is lossy at best.


At Wafer, we’ve found that no single frontier model consistently dominates kernel generation. The performance landscape is the cross product of hardware target, kernel type, precision format, tensor shapes, and more: a space large enough that vibes-based evaluation misses most of it. A model that writes excellent FP4 GEMMs on B200s might fumble fused normalization kernels on MI355X, and you won’t know unless you measure.


The problem is that proper measurement is expensive. You need long-running, fault-tolerant agent harnesses. You need access to the actual target hardware. And you need a benchmarking suite strict enough to catch the many creative ways models[reward-hack](https://kernelarena.ai/resources) their way to inflated scores: returning identity kernels, calling cuBLAS instead of writing CUDA, manipulating timing infrastructure, and more. All of these are ongoing industry problems.


Individual researchers shouldn’t have to shoulder all of that just to pick a model.


## KernelArena


Today we’re launching[KernelArena](https://kernelarena.ai/) , an open platform for benchmarking AI-generated GPU kernels across frontier models, hardware targets, and kernel categories. Live leaderboards and per-kernel breakdowns all in one place.


## Initial Benchmarks


KernelArena launches with two benchmark suites:


- **WaferBench NVFP4:** 6 fused FP4 inference kernels (Add+RMSNorm+Quantize, SiLU+Mul+Quantize, standalone quantization) evaluated on NVIDIA B200 GPUs against FlashInfer references. Bitwise-exact correctness checking with static analysis to reject reward hacks before they hit the timer. The first round covers GPT-5.4 (OpenAI), Claude-4.6-Opus (Anthropic), Gemini-3.1-Pro (Google), and Composer-1.5 (Cursor), all run through Cursor’s agentic coding harness.
- **KernelBench HIP:** LLM-generated HIP kernels on AMD MI300X, covering a wider field of models from Anthropic, OpenAI, Google, xAI, Moonshot, and Z.ai.


Scores reflect both correctness (pass rate) and performance (geometric mean speedup over baseline). No single model sweeps either suite; the leaderboards are tighter than you’d expect.


## Reward Hacking Catalog


We’re also publishing a[catalog of reward hacking patterns](https://kernelarena.ai/resources) we’ve encountered (and defended against) while building these suites. It covers timing attacks, semantic attacks, and benign shortcuts, each with example code and the defense we use. If you’re building your own kernel benchmarks, this is the part worth stealing.


## What’s Next


KernelArena will continue to expand to more kernel families, hardware targets, and community-submitted benchmarks. The platform is open; if you have a kernel problem suite or evaluation methodology you’d like to contribute, reach out.


[Check out the leaderboard.](https://kernelarena.ai/eval)
