---
schema_version: "1.0.0"
document_id: "12c1c998671861d58c12b9172d99e07f6a6d2194e6456af78aeef6494529f462"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/qwen3.5-on-overshoot"
published_at: "2026-03-03T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:dd64eef8c51430981dbeb15f627be5a66cca0a9426a80e26156098e99264a3a3"
---

# Qwen 3.5: Architecture and Model Selection

[← Back to all posts](https://www.overshoot.ai/blogs)


The Qwen 3.5 family is the first major open-weight release where every model is natively multimodal. Five models from 2B to 35B parameters, all trained on text, images, and video from the start through early fusion.


We deployed all five and dug into the published benchmarks. This post covers the hybrid attention architecture (Gated DeltaNet + Gated Attention), per-model benchmark analysis against GPT-5-mini and Claude Sonnet 4.5, and practical guidance on which model to pick for different workloads.


## What Changed: Native Multimodal


Previous generation models like Qwen3-VL bolted vision on after the fact. A pretrained language model would get a vision encoder attached to it, converting images and video frames into tokens that fit alongside text. It worked, but it was a workaround.


Qwen 3.5 does something different. Every model in the family is trained on text, images, and video from the start through early fusion on trillions of multimodal tokens. There is no separate “-VL” variant. The same model that handles your text prompts also understands your video streams.


The practical consequence is dramatic. The Qwen3.5-9B, a 9 billion parameter model, outperforms the previous generation's Qwen3-VL-30B-A3B on every single vision benchmark. Not most. All of them. A model one-third the size, seeing better across the board, because vision was there from the beginning.


### Qwen 3.5 vs Previous Generation


Every Qwen 3.5 model beats the previous generation model above it in size:


#### Qwen 3.5 vs Previous Generation


Model


Benchmark


Qwen3.5-9B vs Qwen3-VL-30B (3x the size)


Qwen3.5-9B


78.4


Qwen3-VL-30B


76


The 2B is the only exception where some benchmarks are close or behind, and even there it wins on OCR (85.4 vs 80.8) and medical imaging (SLAKE 74.4 vs 65.9). The pattern is clear: native multimodal training delivers a generational leap in vision capability.


## The Architecture: Why These Models Are Fast


Every dense Qwen 3.5 model (27B, 9B, 4B, 2B) uses a hybrid attention mechanism with a 3:1 ratio of Gated DeltaNet layers to standard Gated Attention layers. In practical terms, 75% of the model's layers use linear attention, which scales as O(n) with sequence length instead of the O(n²) of standard attention.


This matters for video. Longer video means more tokens. O(n²) attention means quadratic slowdown. Linear attention means the model handles longer video contexts without the latency penalty you'd expect.


The 35B-A3B takes a different approach: it's a Mixture of Experts (MoE) model with 256 experts, of which only 8 plus 1 shared expert are active per token. That's 35 billion total parameters but only 3 billion doing work on any given token. The tradeoffs between these architectures matter, and we cover them below.


## The Models


### Qwen3.5-27B: The All-Rounder


The 27B is the only dense model in the medium tier, meaning all 27 billion parameters are active on every token. No routing, no experts, just a large dense model doing its best on every input.


**Where it leads:**


- Video understanding (VideoMME 87.0) beats GPT-5-mini (83.5), Claude Sonnet 4.5 (81.1), and the previous gen Qwen3-VL-235B (83.8)
- Visual math (MathVision 86.0) is 14 points above GPT-5-mini (71.9) and tied with the much larger 122B
- Document understanding (OmniDocBench 88.9) beats Claude Sonnet 4.5 by 3 points and GPT-5-mini by 12
- Instruction following (IFEval 95.0) beats GPT-5-mini (93.9), the highest in the family
- Coding (SWE-bench 72.4) matches GPT-5-mini (72.0), the only open-weight model at this level
- Screen understanding (ScreenSpot Pro 70.3) is nearly 2x Claude Sonnet 4.5 (36.2)
- Medical imaging (SLAKE 80.0, PMC-VQA 62.4) leads all comparisons by wide margins


**Where it doesn't:**


- CodeForces (1899) trails GPT-5-mini (2160). Competition-level coding is still a gap.
- It's the most resource-intensive of our five models since every parameter is always active.


**Who should use it:** Teams that want one model for everything and don't want to think about which model to route to. The 27B is the safest choice for production workloads spanning vision, video, coding, and reasoning.


### Qwen3.5-35B-A3B: The Throughput Machine


The 35B-A3B is the only MoE model in our lineup. 35 billion total parameters, 3 billion active per token. Vision quality is remarkably close to the 27B while activating 9x fewer parameters.


**Where it leads:**


- UI and mobile agents (AndroidWorld 71.1) is the best score in the entire medium tier, beating even the 122B (66.4) and the 27B (64.2)
- Document processing (OmniDocBench 89.3, OCRBench 91.0) is within 1-2 points of the 122B
- Low-level visual perception (VlmsAreBlind 97.0) is the highest in the entire family
- Video (VideoMME 86.6) is within 0.4 points of the 27B


**Where it doesn't:**


- Coding craters. VERTU's analysis found this model “suffered the most in agentic coding tests.” SWE-bench (69.2) and LiveCodeBench (74.6) both trail the 27B by meaningful margins. With only 3B parameters active, the model struggles to hold the complex mental map that multi-file software engineering requires.
- Instruction following (IFEval 91.9) is good but trails the 27B (95.0).


**Who should use it:** If your workload is vision-heavy and throughput matters. Document processing pipelines, video analysis at scale, UI automation agents. If you need reliable coding, use the 27B instead.


### Medium Tier vs GPT-5-mini


#### Medium Tier vs GPT-5-mini


Benchmark


27B


35B-A3B


GPT-5-mini


Claude Sonnet 4.5


27B


87


35B-A3B


86.6


GPT-5-mini


83.5


Claude Sonnet 4.5


81.1


Both Qwen 3.5 medium models beat GPT-5-mini on vision across the board. The gap is largest on visual math and document understanding. GPT-5-mini holds its ground on coding and competitive programming.


### Qwen3.5-9B: The Sweet Spot


The 9B is a dense model where every parameter is active. It's the largest of the “small series” that launched on March 2, and it represents the best balance of capability and efficiency in the family.


**Where it leads:**


- Beats the previous gen Qwen3-VL-30B on every single vision benchmark, with 3x fewer parameters
- Video (VideoMME 84.5) beats GPT-5-Nano by 12.8 points and is only 2.5 points below the 27B
- Long video (MLVU 84.4) is within 1.5 points of the 27B and 35B. Long-form video comprehension barely degrades at this size.
- Object counting (CountBench 97.2) matches the 397B flagship exactly. This capability appears fully saturated at 9B.
- Medical imaging (SLAKE 79.0) is within 1 point of the flagship (79.9)
- Documents (OmniDocBench 87.7) beats GPT-5-Nano by 32 points


**Where it doesn't:**


- Language benchmarks (MMLU-Pro 82.5) are solid but not leading against larger models
- LiveCodeBench (65.6) shows coding is not its strength


**Who should use it:** The default starting point for most developers. Strong enough for production vision work, efficient enough to run multiple instances.


### Qwen3.5-4B: The One That Shouldn't Be This Good


The 4B is a dense model, and its vision scores are hard to believe at this parameter count. Video understanding is within 1 point of the 9B on most benchmarks.


**Where it leads:**


- Video (VideoMME 83.5) is only 1 point below the 9B. MLVU (82.8) for long video is 1.6 points below. The gap between 4B and 9B on video is negligible.
- Visual math (We-Math 75.4) ties the 9B (75.2). This capability saturates early in the Qwen 3.5 family.
- Agent benchmarks (TAU2-Bench 79.9) actually beat the 9B (79.1), an unusual inversion where the smaller model makes more decisive decisions
- Beats the previous gen 30B on MMMU (77.6 vs 76.0), MathVision (74.6 vs 65.7), and VideoMME (83.5 vs 79.9)


**Where it doesn't:**


- SimpleVQA (43.4) is a notable weakness. Real-world visual Q&A trails even GPT-5-Nano (46.0).
- Coding is limited at this size (LiveCodeBench 55.8).
- Thinking mode can produce loops. Non-thinking mode may be more reliable for production.


**Who should use it:** When you need strong video and vision at the lowest latency. A 4B model that genuinely outperforms last generation's 30B on the tasks that matter for real-time vision applications.


### Qwen3.5-2B: The Specialist


The 2B is the smallest model in the family and the one where you feel the size constraints the most on general tasks. But it has genuine strengths that make it more than just a toy.


**Where it leads:**


- OCR (OCRBench 85.4) beats Qwen3-VL-4B (80.8), a model twice its size. Document text recognition is a standout.
- Visual perception (VlmsAreBlind 75.8) is a 52% improvement over the previous gen Qwen3-VL-2B (50.0). A generational leap in basic visual understanding.
- Medical imaging (SLAKE 74.4) beats Qwen3-VL-4B (65.9) by 13%. Strong for medical image analysis at this scale.
- Video (MLVU 76.2) matches Qwen3-VL-4B (75.7) despite being half the size
- Agent/tool use (BFCL-V4 43.6, TAU2-Bench 48.8) beats the larger Qwen3-4B on both


**Where it doesn't:**


- General language (MMLU-Pro 66.5) is well behind text-focused models at this size
- Thinking mode is prone to loops. The model card explicitly warns about this, and our deployment experience confirms it. Use non-thinking mode for production.
- Interestingly, non-thinking mode actually scores *better* on OCR tasks (OCRBench 85.4 vs 84.5, CC-OCR 75.8 vs 72.9). More thinking does not always mean better vision.


**Who should use it:** OCR, document scanning, and medical image triage. When you need vision intelligence at the fastest possible response time and can live with weaker general reasoning.


## Video Understanding: The Family's Superpower


Video is where the Qwen 3.5 family punches hardest.


### Video Understanding Across All Five Models


#### Video Understanding Across All Five Models


Benchmark


27B


35B-A3B


9B


4B


2B


27B


87


35B-A3B


86.6


9B


84.5


4B


83.5


2B


75.6


The degradation from 27B to 4B is remarkably gentle. On VideoMME with subtitles, the 4B retains 96% of the 27B's score. On MLVU (long-form video), the 4B retains 96.4%. For most real-world video applications, the difference between a 4B and a 27B is smaller than you'd expect.


The 2B drops more noticeably, but even there, MLVU (76.2) matches the previous generation's Qwen3-VL-4B (75.7) at half the parameters.


### What Better Video Understanding Actually Means


These benchmarks measure how well a model understands video content: actions, temporal sequences, scene comprehension. But benchmarks are run on short clips. In production, video comes from cameras, screen shares, and live feeds — continuous streams, not isolated files.


The practical implication of these scores is that smaller models can now handle video tasks that previously required much larger ones. A 4B model that retains 96% of the 27B's video score means you can run real-time video analysis on significantly less hardware. Security cameras, accessibility tools, sports analysis, and computer-use agents all benefit directly from better temporal understanding at lower parameter counts.


### How This Compares to Proprietary Models


#### VideoMME (w/ sub) — Proprietary Comparison


Qwen3.5-27B


Live video on Overshoot, <200ms


87


Qwen3.5-35B-A3B


Live video on Overshoot, <200ms


86.6


GPT-5-mini


3-6s per single image frame


83.5


Qwen3-VL-235B


Previous gen, 9x the parameters


83.8


Claude Sonnet 4.5


1.1-1.5s per single image frame


81.1


We tested these latencies ourselves. We sent the same image to GPT-5-mini and Claude Sonnet 4.5 with the prompt “Describe what you see in this image in 5 words.” GPT-5-mini took 3-6 seconds to respond, spending 128-256 tokens on internal reasoning before producing any visible output. Claude Sonnet 4.5 took 1.1-1.5 seconds. Both for a single still image.


Self-hosted, the Qwen 3.5 35B scores higher on vision benchmarks than both proprietary models while responding to video in under 200ms on an H200. The open-weight advantage is not just cost — it's latency.


## Picking the Right Model


27B


No compromises


Video, coding, instruction following, medical


35B-A3B


High throughput


Near-27B vision at a fraction of compute


9B


Best balance


Beats last-gen 30B on everything


4B


Lowest latency


96% of 27B video score


2B


OCR & triage


OCR beats models 2x its size


All five models are available for inference on **Overshoot** . Try them in the Playground or integrate via the SDK.


[Try it in the Playground →](https://playground.overshoot.ai/)[Read the SDK Docs](https://docs.overshoot.ai/)
