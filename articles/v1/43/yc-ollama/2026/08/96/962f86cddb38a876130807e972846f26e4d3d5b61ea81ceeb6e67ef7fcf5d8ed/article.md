---
schema_version: "1.0.0"
document_id: "962f86cddb38a876130807e972846f26e4d3d5b61ea81ceeb6e67ef7fcf5d8ed"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/muse-glimmer"
published_at: null
first_seen_at: "2026-08-10T21:33:34.799867+00:00"
fetched_at: "2026-08-10T21:33:36.358695+00:00"
content_hash: "sha256:2ceea0f2bfc4be724db7d756d8445673d48e063ce4fd806ccb0b5892d1df9409"
---

# Muse Glimmer from Meta Superintelligence Labs is now available

> Note: Muse Glimmer is currently available via initial support via Ollama’s MLX engine on Apple Silicon. Additional support and optimizations for Apple Silicon, NVIDIA, AMD, and other platforms will be available in the coming days.


**Muse Glimmer** , Meta’s newest open model and the first released by Meta Superintelligence Labs,[is now available on Ollama](https://ollama.com/library/muse-glimmer) . It’s a 30B multimodal model purpose-built for agent workloads that run locally with a 128K+ context length, released under the Apache 2.0 license.


With Ollama, you can now use Muse Glimmer to power coding agent applications such as Claude Code, Codex, Pi and more, as well as long-running personal assistants such as OpenClaw and Hermes.


Ollama’s MLX engine provides state-of-the-art performance on Apple Silicon for this model, with support for DFlash and image input.


To run Muse Glimmer locally,[download the latest release of Ollama](https://ollama.com/download) and run:


```text
ollama run muse-glimmer:30b-mlx


```


## Power coding agents locally


To run Muse Glimmer on Apple Silicon with Claude Code,[download Ollama](https://ollama.com/download) and run:


```text
ollama launch claude --model muse-glimmer:30b-mlx


```


For a lighter-weight coding agent, try[Pi](https://pi.dev/) :


```text
ollama launch pi --model muse-glimmer:30b-mlx


```


For personal assistant frameworks such as OpenClaw and Hermes, use:


```text
ollama launch openclaw --model muse-glimmer:30b-mlx


```


```text
ollama launch hermes --model muse-glimmer:30b-mlx


```


` ollama launch` also works with Codex, OpenCode, GitHub Copilot, and more. See[all integrations](https://docs.ollama.com/integrations) .


Muse Glimmer also supports controllable reasoning strength:` low` ,` medium` ,` high` , and` xhigh` . Use` high` or` xhigh` for complex coding and agentic tasks, and lower strengths when speed matters more.


## State-of-the-art performance on Apple Silicon with new DFlash and image input support


Ollama’s[MLX engine](https://ollama.com/blog/mlx-performance) now supports DFlash, building on previous[multi-token prediction (MTP) support](https://ollama.com/blog/faster-gemma-4-mlx-mtp) . With DFlash, Muse Glimmer runs 1.5×–1.8× faster on Apple Silicon.


Muse Glimmer’s dedicated 1.8B-parameter perception encoder gives it native image understanding. With new image input support in the MLX engine, coding agents can use it for low-latency, back-to-back tool calling on tasks that require images, such as:


- Building websites or applications from a drawing or mockup
- Computer use applications powered by screenshots
- Reading documents, receipts, and charts


## More information


- [Meta’s announcement](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)
- [Model information](https://ollama.com/library/muse-glimmer)
