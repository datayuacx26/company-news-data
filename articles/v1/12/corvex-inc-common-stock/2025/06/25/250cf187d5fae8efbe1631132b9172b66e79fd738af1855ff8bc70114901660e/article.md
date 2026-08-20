---
schema_version: "1.0.0"
document_id: "250cf187d5fae8efbe1631132b9172b66e79fd738af1855ff8bc70114901660e"
company_key: "corvex-inc-common-stock"
company: "Corvex Inc."
source_id: "corvex-inc-common-stock-rss-5d3752083407"
canonical_url: "https://www.corvex.ai/blog/llm-inference-cost"
published_at: "2025-06-13T00:00:00+00:00"
first_seen_at: "2026-07-26T12:16:19.295968+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:e6274a3879f2931a6169ac28917cb5f173f26719ebbbfd9f157fe92c531b9280"
---

# Serving LLMs Without Breaking the Bank

## Why Does LLM Inference Get Expensive So Quickly?


---


A 70B-parameter model needs roughly 80 GB of HBM just to sit idle; every second that memory occupies a GPU still incurs the hourly fee. Under low traffic, most of your spend is simply paying for unused capacity.


## What Actually Drives the Cost of Serving Large Models?


---


- **GPU hourly rate** — public clouds list H100 at $3–$9/hr; Corvex’s H200 starts at $2.15/hr.
- **Token throughput (tokens/sec)** — higher TPS means fewer GPU-hours per million tokens. vLLM and TensorRT-LLM reach 250–300 t/s on 70B models.
- **Prompt + generation length** — compute scales almost linearly with total tokens.
- **GPU utilization** — sporadic traffic strands expensive GPUs.
- **Model precision** — 8- or 4-bit weights fit in less memory and decode faster.


## Which Inference Engines Squeeze the Most Out of Each GPU?


---


Engine Key Optimization Llama-2 70B TPS (H200) Memory (GB) Notes


TensorRT-LLM CUDA kernels + speculative decoding 300 t/s 65 C++/Python; ideal on Corvex H200


vLLM Paged-attention + continuous batching 268 t/s 72 Python; easy drop-in


Hugging Face TGI Tensor & pipeline parallel 180 t/s 80 Managed or self-host


## How Much Does 4-bit Quantization Save?


---


Recent papers on GPTQ and QLoRA show weight-only 4-bit models cut VRAM ~75% while preserving accuracy within 1 point on MMLU and TruthfulQA. That shrink lets a single H200 hold Llama-3 70B entirely in memory, boosting throughput 1.5–2×.


## Which Hardware Tier Should I Choose?


---


Tier Best for Example Corvex Option Why It Wins


H200 7–70B models, low-latency chat & RAG 8× H200 HGX node HBM3e + InfiniBand remove IO bottlenecks


B200 High-QPS inference & training 8× B200 HGX Blackwell FP4/F8 modes lift TPS by 3–4×


GB200 NVL72 Massive multi-agent or 100B+ models 72× GB200 super-node NVLink Switch fabric delivers ~870k t/s


**Note:** Corvex does not sell H100 servers; we focus on newer H200 and Blackwell-class GPUs with better price-performance.


## Seven Proven Tactics That Cut Inference Cost


---


- Right-size the model — distill or pick smaller checkpoints
- Quantize to 8- or 4-bit
- Use an efficient engine — TensorRT-LLM, vLLM, or TGI
- Batch + stream — continuous batching keeps utilization high
- Cache embeddings and full responses
- Autoscale to zero — park GPUs during lulls (Corvex supports auto-park)
- Measure **$ per million tokens** — the only KPI that matters


## How Corvex Lowers Total Cost of Ownership


---


- H200 nodes at $2.15/hr with 141 GB HBM3e
- 3.2 Tb/s non-blocking InfiniBand across every rack
- Auto-park policies that spin GPUs down to billing-free idle
- One-click images for vLLM, TensorRT-LLM, and TGI
- White-glove support from engineers who tune inference daily


Customers report up to 40% lower **$ / million tokens** vs. public-cloud endpoints charging more for the same silicon.


## Real-World Cost per Million Generated Tokens


---


Model Engine Hardware Cost on Corvex Typical Public Cloud


Llama-3-8B vLLM 1× H200 $4.7 $9.3


Llama-2-70B TensorRT-LLM 4× H200 $19.2 $36.8


Mixtral-8×7B vLLM 1× B200 $6.8 $13.5


## How Big Should My GPU Be?


---


Allocate ~1 GB of VRAM per billion parameters at FP16, halve for 8-bit, quarter for 4-bit, then add 20% headroom for KV-cache. This prevents mid-request OOMs that waste paid compute.


## Frequently Asked Questions


---


- **Does batching hurt latency for a single user?**
No. Engines like vLLM interleave prompts into micro-batches, keeping first-token latency under 30ms even under high load.
- **Are “serverless” GPU wrappers cheaper for bursty traffic?**
Sometimes. They avoid idle cost, but if bursts last >3 minutes, Corvex auto-parked nodes are usually cheaper.
- **Is 4-bit quantization production-safe?**
Yes for most chat/RAG workloads. Accuracy loss on Llama-2/3 is negligible, but always validate on your domain.


## Next Step


---


Launch a pre-tuned TensorRT-LLM or vLLM endpoint on Corvex in under five minutes, or[talk to our solution architects](https://www.corvex.ai/talk-to-us) for a cost benchmark tailored to your workload.
