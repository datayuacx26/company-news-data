---
schema_version: "1.0.0"
document_id: "aa6a3f9e414765a6282d9fb641b42dd6aab1a7a2fe2137080e78b1f3e3b00516"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/new-model-scheduling"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c4be64d4d73259497639bd097db7c4e3dce4ed33a1eaa2a699c25650c00c4ecf"
---

# New model scheduling

Ollama now includes a significantly improved model scheduling system. Ahead of running a model, Ollama’s new engine will now measure the *exact* amount of memory required compared to an estimation in previous versions of Ollama. This has several benefits:


- **Significantly reduced crashes due to out of memory issues:** Because memory management is exact, over-allocations no longer occur meaning fewer out of memory issues.
- **Maximizing GPU utilization:** Ollama’s new memory management allocates more memory to the GPU, increasing token generation and processing speeds
- **Multi-GPU performance:** Ollama will now schedule models more efficiently over multiple GPUs, significantly improving multi-GPU and mismatched GPU performance
- **Accurate reporting:** Measurements in tools like` nvidia-smi` will now match` ollama ps` making it easy to track memory utilization on your system


All models implemented in Ollama’s new engine now have this new feature enabled by default, with more models coming soon as they transition to Ollama’s new engine.


## Examples


### Long context


- GPU: 1x NVIDIA GeForce RTX 4090
- Model:` gemma3:12b`
- Context length: 128k


Old New


52.02 tokens/s token generation speed 85.54 tokens/s token generation speed


19.9GiB of VRAM 21.4GiB of VRAM


48⁄49 layers loaded on GPU 49⁄49 layers loaded on GPU


### Image input


- GPU: 2x NVIDIA GeForce RTX 4090
- Model:` mistral-small3.2`
- Context length: 32k


Old New


127.84 tokens/s prompt evaluation speed 1380.24 tokens/s prompt evaluation speed


43.15 tokens/s token generation speed 55.61 tokens/s token generation speed


19.9GiB of VRAM 21.4GiB of VRAM


40⁄41 layers loaded on GPU 41⁄41 layers loaded on GPU + vision model


## Supported models


All models implemented in Ollama’s new engine use the new memory management features:


- [gpt-oss](https://ollama.com/library/gpt-oss)
- [llama4](https://ollama.com/library/llama4) ,[llama3.2-vision](https://ollama.com/library/llama3.2-vision) (soon:[llama3.2](https://ollama.com/library/llama3.2-vision) ,[llama3.1](https://ollama.com/library/llama3.1) ,[llama3](https://ollama.com/library/llama3) )
- [gemma3](https://ollama.com/library/gemma3) ,[embeddinggemma](https://ollama.com/library/embeddinggemma) ,[gemma3n](https://ollama.com/library/gemma3n)
- [qwen3](https://ollama.com/library/qwen3) ,[qwen2.5vl](https://ollama.com/library/qwen2.5vl) (soon:[qwen3-coder](https://ollama.com/library/qwen3-coder) )
- [mistral-small3.2](https://ollama.com/library/mistral-small3.2)
- [all-minilm](https://ollama.com/library/all-minilm) and other embedding models
