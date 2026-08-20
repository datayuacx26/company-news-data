---
schema_version: "1.0.0"
document_id: "acd82209825915ae83e080520426a7d487b960ecc739572781ff8d17fc70927f"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/launch"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:7c5ad9ebf1e8e7b4f74ce3278ca75b0d9b9d733618f75cb780ab6cbdb5190ea5"
---

# ollama launch

` ollama launch` is a new command which sets up and runs your favorite coding tools like Claude Code, OpenCode, and Codex with local or cloud models. No environment variables or config files needed.


## Get started


Download[Ollama v0.15+](https://ollama.com/download) , then open a terminal and run:


```text
# ~23 GB VRAM required with 64000 tokens context length
ollama pull glm-4.7-flash


# or use a cloud model (with full context length)
ollama pull glm-4.7:cloud


```


## One command setup


**Claude Code:**


```text
ollama launch claude


```


**OpenCode:**


```text
ollama launch opencode


```


This will guide you to select models and launch your chosen integration. No environment variables or config files needed.


### Supported integrations


- Claude Code
- OpenCode
- Codex
- Droid


## Recommended models for coding


**Note:** Coding tools work best with a full context length. Update the context length in Ollama’s settings to at least 64000 tokens. See the[context length documentation](https://docs.ollama.com/context-length) on how to make changes.


**Local models:**


- ` glm-4.7-flash`
- ` qwen3-coder`
- ` gpt-oss:20b`


**Cloud models:**


- ` glm-4.7:cloud`
- ` minimax-m2.1:cloud`
- ` gpt-oss:120b-cloud`
- ` qwen3-coder:480b-cloud`


## Extended coding sessions


If you have trouble running these models locally, Ollama also offers a cloud service with hosted models that has full context length and generous limits even at the free tier.


With this update Ollama now offers more usage and an extended 5-hour coding session window. See[ollama.com/pricing](https://ollama.com/pricing) for details.


## Configure only


To configure a tool without launching it immediately:


```text
ollama launch opencode --config


```
