---
schema_version: "1.0.0"
document_id: "70f94d369b582f9c5c5bf4a084bb06eaffcc28421b9f19fd620e70f334cd5fba"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/mlx"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:6bf0b75b1d1a5cdfe4802e358358b7f72f7520115f53786ef01724ee52d5cca3"
---

# Ollama is now powered by MLX on Apple Silicon in preview

Today, we’re previewing the fastest way to run Ollama on Apple silicon, powered by MLX, Apple’s machine learning framework.


This unlocks new performance to **accelerate your most demanding work on macOS:**


- Personal assistants like OpenClaw
- Coding agents like Claude Code, OpenCode, or Codex


Accelerate coding agents like Pi or Claude Code


OpenClaw now responds much faster


### Fastest performance on Apple silicon, powered by MLX


Ollama on Apple silicon is now built on top of Apple’s machine learning framework, MLX, to take advantage of its unified memory architecture.


This results in a large speedup of Ollama on all Apple Silicon devices. On Apple’s M5, M5 Pro and M5 Max chips, Ollama leverages the new GPU Neural Accelerators to accelerate both time to first token (TTFT) and generation speed (tokens per second).


Prefill performance


Decode performance


Testing was conducted on March 29, 2026, using Alibaba’s Qwen3.5-35B-A3B model quantized to NVFP4 and Ollama’s previous implementation quantized to Q4_K_M using Ollama 0.18. Ollama 0.19 will see even higher performance (1851 token/s prefill and 134 token/s decode when running with int4 quantization).


### NVFP4 support: higher quality responses and production parity


Ollama now leverages NVIDIA’s[NVFP4](https://developer.nvidia.com/blog/introducing-nvfp4-for-efficient-and-accurate-low-precision-inference/) format to maintain model accuracy while reducing memory bandwidth and storage requirements for inference workloads.


As more inference providers scale inference using NVFP4 format, this allows Ollama users to share the same results as they would in a production environment.


It further opens up Ollama to have the ability to run models optimized by NVIDIA’s[model optimizer](https://github.com/NVIDIA/Model-Optimizer) . Other precisions will be made available based on the design and usage intent from Ollama’s research and hardware partners.


### Improved caching for more responsiveness


Ollama’s cache has been upgraded to make coding and agentic tasks more efficient.


-


**Lower memory utilization:** Ollama will now reuse its cache across conversations, meaning less memory utilization and more cache hits when branching when using a shared system prompt with tools like Claude Code.


-


**Intelligent checkpoints:** Ollama will now store snapshots of its cache at intelligent locations in the prompt, resulting in less prompt processing and faster responses.


-


**Smarter eviction:** shared prefixes survive longer even when older branches are dropped.


### Get started


[Download Ollama 0.19](https://ollama.com/download)


This preview release of Ollama accelerates the new[Qwen3.5-35B-A3B](https://ollama.com/library/qwen3.5) model, with sampling parameters tuned for coding tasks.


Please make sure you have a Mac with more than 32GB of unified memory.


**Claude Code:**


```text
ollama launch claude --model qwen3.5:35b-a3b-coding-nvfp4


```


**OpenClaw:**


```text
ollama launch openclaw --model qwen3.5:35b-a3b-coding-nvfp4


```


**Chat with the model:**


```text
ollama run qwen3.5:35b-a3b-coding-nvfp4


```


### Future models


We are actively working to support future models. For users with custom models fine-tuned on supported architectures, we will introduce an easier way to import models into Ollama. In the meantime, we will expand the list of supported architectures.


### Acknowledgments


Thank you to:


- The MLX contributor team who built an incredible acceleration framework
- NVIDIA contributors to NVFP4 quantization, NVFP4 model optimizer, MLX CUDA support, Ollama optimizations and testing
- The GGML & llama.cpp team who built a thriving local framework and community
- The Alibaba Qwen team for open-sourcing excellent models and their collaboration
