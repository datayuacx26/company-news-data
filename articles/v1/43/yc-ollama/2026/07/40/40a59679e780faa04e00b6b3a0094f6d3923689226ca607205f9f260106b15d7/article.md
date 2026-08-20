---
schema_version: "1.0.0"
document_id: "40a59679e780faa04e00b6b3a0094f6d3923689226ca607205f9f260106b15d7"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/openclaw-tutorial"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:a59b9b2b4a5d0c1c38fc789be29e191eb4fa290aeae85b3f96b2d73532cf1358"
---

# The simplest and fastest way to setup OpenClaw

OpenClaw is a personal AI assistant that can clear your inbox, send emails, manage your calendar, and complete other tasks via messaging apps like WhatsApp, Telegram, iMessage, or any chat app you already use.


It all runs on your own hardware, and with Ollama 0.17, it’s now a single command to get started.


### What you’ll need


- [Ollama 0.17 or later](https://ollama.com/download)
- [Node.js](https://nodejs.org/) (npm is used to install OpenClaw)
- Mac or Linux system (Windows users can install OpenClaw via WSL - Windows Subsystem for Linux)


### Step 1: Run the command


Open a terminal, and type in:


```text
ollama launch openclaw --model kimi-k2.5:cloud


```


> Note: Other models can also be configured. See` ollama launch openclaw` for recommended models.


Ollama handles everything from here.


### Step 2: Install OpenClaw


If OpenClaw isn’t already on your system, Ollama detects that and prompts you for installation.


Ollama will automatically install and configure OpenClaw for you.


## Step 3: Start chatting


This is it! OpenClaw will open up in the terminal where you can start chatting with your AI assistant.


## Web search


If you selected a model from Ollama’s cloud, Ollama installs the web search plugin automatically. This allows OpenClaw to perform web searches for the latest and up-to-date information.


Local models work out of the box without additional plugins.


## Connect messaging apps


OpenClaw can connect to messaging platforms like WhatsApp, Telegram, Slack, Discord, or iMessage to chat with your models from anywhere.


```text
openclaw configure --section channels


```


After configuring, make sure you select` Finished` to save your desired configuration.


### Selecting a model


Ollama shows a model selector with recommended and available models. Agents work best with at least 64k context length.


Ollama’s cloud models have full context length, which provide the best experience with agents like OpenClaw.


**Cloud models** :


Model Description


[kimi-k2.5:cloud](https://ollama.com/library/kimi-k2.5:cloud) Multimodal reasoning with subagents


[minimax-m2.5:cloud](https://ollama.com/library/minimax-m2.5:cloud) Fast, efficient coding and real-world productivity


[glm-5:cloud](https://ollama.com/library/glm-5:cloud) Reasoning and code generation


**Local models** (requires GPU VRAM):


Model VRAM Description


[glm-4.7-flash](https://ollama.com/library/glm-4.7-flash) ~25 GB Reasoning and code generation


[qwen3-coder](https://ollama.com/library/qwen3-coder) ~25 GB Efficient all-purpose assistant


## Running securely


OpenClaw has the ability to read files and execute actions when tools are enabled. Ensure that you run OpenClaw in an isolated environment and are aware of the risks with giving OpenClaw access to your system.


See the[OpenClaw security documentation](https://docs.openclaw.ai/gateway/security) for more details.


## Learn more


- [Ollama’s cloud models](https://ollama.com/pricing)
- [OpenClaw documentation](https://docs.openclaw.ai/)
- [Gateway security](https://docs.openclaw.ai/gateway/security)
