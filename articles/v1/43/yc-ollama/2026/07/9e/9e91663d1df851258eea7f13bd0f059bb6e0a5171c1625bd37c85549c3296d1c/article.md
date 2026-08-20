---
schema_version: "1.0.0"
document_id: "9e91663d1df851258eea7f13bd0f059bb6e0a5171c1625bd37c85549c3296d1c"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/openclaw"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c10366cdc222463c48437e971138bc195c805cc94cb5e74312490622c87a200c"
---

# OpenClaw

[OpenClaw](https://openclaw.ai/) is a personal AI assistant that bridges your favorite messaging platforms to AI coding agents through a centralized gateway. It runs locally on your own devices, keeping your conversations and code private.


OpenClaw integrates with WhatsApp, Telegram, Slack, Discord, iMessage, and other messaging services, allowing you to interact with AI coding agents from anywhere.


## Get started


Start by installing[OpenClaw](https://docs.openclaw.ai/)


```text
curl -fsSL https://openclaw.ai/install.sh | bash


```


**Windows**


```text
iwr -useb https://openclaw.ai/install.ps1 | iex


```


## Running with Ollama


Once installed, you can launch OpenClaw directly with Ollama to connect local/cloud models:


```text
ollama launch openclaw


```


If you want to configure OpenClaw without immediately starting the service:


```text
ollama launch openclaw --config


```


The gateway will auto-reload if it’s already running.


##


## Recommended models


OpenClaw requires a larger context length to complete tasks. It is recommended to use a context length of at least 64k tokens.


Here are some models that work well with OpenClaw:


Model Description


[qwen3-coder](https://ollama.com/library/qwen3-coder) Optimized for coding tasks


[glm-4.7](https://ollama.com/library/glm-4.7) Strong general-purpose model


[glm-4.7-flash](https://ollama.com/library/glm-4.7-flash) Balanced performance and speed


[gpt-oss:20b](https://ollama.com/library/gpt-oss:20b) Balanced performance and speed


[gpt-oss:120b](https://ollama.com/library/gpt-oss:120b) Improved capability


Ollama’s cloud models are available for free to start. Search for models on[ollama.com](https://ollama.com/search) .


Model Description


[kimi-k2.5](https://ollama.com/library/kimi-k2.5) Powerful 1T parameter model for agentic tasks


[minimax-m2.1](https://ollama.com/library/minimax-m2.1) Excellent multilingual capabilities


[glm-4.7](https://ollama.com/library/glm-4.7) Strong general-purpose model


[gpt-oss:120b](https://ollama.com/library/gpt-oss:120b) Improved capability


Please see more models on[ollama.com](https://ollama.com/search) .


## Example screen captures


Connecting to WhatsApp


## Note


OpenClaw was previously known as Clawdbot and Moltbot. The` clawdbot` command still works as an alias.
