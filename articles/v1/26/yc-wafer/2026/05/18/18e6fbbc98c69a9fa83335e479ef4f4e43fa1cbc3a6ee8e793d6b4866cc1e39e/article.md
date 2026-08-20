---
schema_version: "1.0.0"
document_id: "18e6fbbc98c69a9fa83335e479ef4f4e43fa1cbc3a6ee8e793d6b4866cc1e39e"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/kimi-k26-nvfp4"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:13:17.137376+00:00"
content_hash: "sha256:f8e08610776912d808f3f4b4aae3ebb8545429dc7d5d4002ffe6d96cb18ddb71"
---

# Quantizing Kimi K2.6 to NVFP4 for Blackwell Inference

Wafer partnered with Parasail to launch` wafer-ai/Kimi-K2.6-NVFP4` , a Blackwell-native FP4 optimization of Moonshot AI’s Kimi K2.6.


Kimi K2.6 NVFP4 is the first release from this partnership. In head-to-head testing on a single **8xB200** node, NVFP4 outperformed INT4 across the metrics that matter for production serving:


- Up to **58% more throughput per node**
- Up to **43% faster token streaming**
- Comparable accuracy across GSM8K and MMLU


## First Release: Kimi K2.6 NVFP4


Kimi K2.6 is a large sparse MoE model built for coding agents, tool use, long-context reasoning, and bursty production traffic. These workloads stress both prefill and decode: long prompts need high throughput, while interactive serving needs fast token streaming under concurrency. The Blackwell series changes the optimization surface by exposing hardware-native FP4 tensor core execution, so FP4 can reduce memory traffic while using the hardware’s native low-precision compute path.


The goal was not just a smaller checkpoint. The goal was a production-serving artifact that improves performance while preserving Kimi K2.6 behavior. At a glance, the released artifact is:


- **Architecture:** Sparse MoE with the Kimi vision tower, roughly 1T total parameters
- **Weights:** NVFP4, using FP4 E2M1 values with 16-element FP8 E4M3 block scales and FP32 per-tensor scales
- **Activations:** NVFP4, with calibrated FP32 per-tensor scale and FP8 E4M3 per-16-element block scale computed at inference
- **KV cache:** FP8 E4M3
- **Storage:** about 590 GB across 119 safetensors shards
- **Serving target:** NVIDIA Blackwell hardware with vLLM; verified on 8x B300 SXM6


To produce it, we:


- **Start from the right source.** We decompressed` moonshotai/Kimi-K2.6` from the W4A16 INT4 source into a BF16 intermediate, then produced a new Blackwell-targeted NVFP4 checkpoint.
- **Calibrate for FP4.** FP4 has a tighter representable range than BF16, so calibration keeps the low-precision representation aligned with the model’s activation distribution.
- **Keep sensitive paths in BF16.** The released config keeps` lm_head` , MLA attention projections, the vision tower, the multimodal projector, and MoE routers out of FP4.
- **Stay on the native FP4 path.** The serving stack uses vLLM with FP8 KV cache and tensor-parallel serving on Blackwell, so the runtime uses Blackwell’s native FP4 path instead of immediately expanding the checkpoint back to higher precision.


The checkpoint is available on Hugging Face as` wafer-ai/Kimi-K2.6-NVFP4` .


## Eval


The public model card evaluates` wafer-ai/Kimi-K2.6-NVFP4` against the upstream` moonshotai/Kimi-K2.6` W4A16 INT4 source on 8x NVIDIA B300 SXM6.


Benchmark Kimi K2.6 NVFP4 INT4 source Delta


GSM8K-CoT 8-shot, strict-match 91.36% ± 0.77 91.51% ± 0.77 -0.15 pp


GSM8K-CoT 8-shot, flexible-extract 92.27% ± 0.74 91.21% ± 0.78 +1.06 pp


MMLU 0-shot 88.63% ± 0.26 89.03% ± 0.25 -0.40 pp


The eval shows NVFP4 is essentially lossless against the INT4 source: each delta is within or near one standard error.


## Get Started


Kimi K2.6 NVFP4 is available through Parasail, with more Wafer-optimized models coming soon. We are also working on Kimi K2.6 with speculative decoding for faster token speeds.


The checkpoint page is on Hugging Face as` wafer-ai/Kimi-K2.6-NVFP4` , but the model is gated for now and requires access approval before the weights are available.


To request access or discuss serving, book an intro call or contact us. You can also read the Parasail launch post and follow our work on LinkedIn.
