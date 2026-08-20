---
schema_version: "1.0.0"
document_id: "56ae8e6bc8b6fdbe28ddeff0e45a324b2bf6ab753be6078a5bf2347ee99325fb"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/nemotron-3-ultra"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:77d18e0e145227b0e9ada22ea6294b77d2929d8778e3ddfe9bcf9170524f5acd"
---

# NVIDIA Nemotron 3 Ultra

NVIDIA Nemotron 3 Ultra is now available on Ollama’s cloud. It’s a 550 billion parameter (55B active) open model from NVIDIA built for long-running, agentic workflows with fast and affordable performance across hundreds of tool calls.


### Model highlights


- **Built for long-running agents:** Tuned for agent orchestration, coding agents, deep research, and complex enterprise workflows that run across hundreds of steps.
- **1M token context:** Keep entire codebases, long tool histories, and research trails in context without losing the thread.
- **Frontier reasoning, high efficiency:** 550B total parameters with only 55B active per token, and optimized for NVFP4—NVIDIA’s 4-bit floating point format that packs the model into less memory and runs faster.


### Get started


Download[Ollama](https://ollama.com/download) , then run Nemotron 3 Ultra with your tool of choice.


**Claude Code**


```text
ollama launch claude --model nemotron-3-ultra:cloud


```


**Hermes Agent**


```text
ollama launch hermes --model nemotron-3-ultra:cloud


```


**OpenClaw**


```text
ollama launch openclaw --model nemotron-3-ultra:cloud


```


**General chat**


```text
ollama run nemotron-3-ultra:cloud


```


See[more integrations](https://ollama.com/library/nemotron-3-ultra) .


### Benchmarks


Nemotron 3 Ultra leads on accuracy across agent productivity, instruction following, and long-context tasks, while delivering leading throughput—saving up to 30% on costs compared to other leading open models.


*Figure 1: Nemotron 3 Ultra leads among open models on agentic benchmarks for agent productivity, coding, and instruction following.*


*Figure 2: Nemotron 3 Ultra is in the most attractive quadrant with leading accuracy and leading throughput among open models.*


*Figure 3: Nemotron 3 Ultra saves up to 30% in costs and leads on the cost efficiency frontier.*


### Reference


- [NVIDIA Nemotron 3 Ultra blog](https://developer.nvidia.com/blog/nvidia-nemotron-3-ultra-powers-faster-more-efficient-reasoning-for-long-running-agents/)
- Ollama[model page](https://ollama.com/library/nemotron-3-ultra)
