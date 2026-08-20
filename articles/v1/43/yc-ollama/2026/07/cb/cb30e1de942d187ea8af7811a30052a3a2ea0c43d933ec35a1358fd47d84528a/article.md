---
schema_version: "1.0.0"
document_id: "cb30e1de942d187ea8af7811a30052a3a2ea0c43d933ec35a1358fd47d84528a"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/openjarvis"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:dc9032dcb48ccf31c7b3ce3704a4c0dc0095e1512677df1ad3bf41b2f762c472"
---

# OpenJarvis: a local-first personal AI is now available to run with Ollama

[OpenJarvis](https://github.com/open-jarvis/OpenJarvis) is an open-source framework for building personal AI agents that run on your own hardware. It’s built by Stanford’s[Hazy Research](https://hazyresearch.stanford.edu/) and[Scaling Intelligence](https://scalingintelligence.stanford.edu/) labs, as part of their “Intelligence Per Watt” research into efficient local AI.


Local models can already handle most day-to-day chat and reasoning, yet most personal AI still sends every request to the cloud. OpenJarvis makes local-first the default. Models run locally, and the cloud is optional. Energy, cost, and latency are tracked alongside accuracy.


Version 1.0 is[now available](https://github.com/open-jarvis/OpenJarvis/releases) with built-in support for Ollama.


## Get started


**Install Ollama**


[Download Ollama](https://ollama.com/download) for macOS, Windows, or Linux.


**Install OpenJarvis**


On macOS or Linux, the install script sets up everything you need and auto-detects your existing Ollama installation:


```text
curl -fsSL https://open-jarvis.github.io/OpenJarvis/install.sh | bash


```


On Windows, run that command inside[WSL2](https://learn.microsoft.com/windows/wsl/install) , or install the[desktop app](https://github.com/open-jarvis/OpenJarvis/releases) .


Then run` jarvis` to start.


## Choosing models


The install script sets up a starter model so you can begin right away, but you can choose your own. Pull any model through Ollama and use it for a query:


```text
jarvis model pull qwen3.5:35b
jarvis ask -m qwen3.5:35b "Your prompt"


```


To set a default model, add it to` ~/.openjarvis/config.toml` :


```text
[intelligence]
default_model = "qwen3.5:35b"
preferred_engine = "ollama"


```


## Built-in agents


OpenJarvis ships with ready-to-run presets. Each one bundles an agent with the engines and tools it needs to run.


**Morning briefing**


Generate a morning briefing agent using your calendar, email, and the day’s news:


```text
jarvis init --preset morning-digest-mac
jarvis connect gdrive
jarvis digest --fresh


```


**Research across files**


Ask a question to perform research across the web and your local documents, returning an answer with citations:


```text
jarvis init --preset deep-research
jarvis memory index ./docs/
jarvis ask "Summarize all emails about Project X"


```


**Local coding agent**


A code agent that writes and runs Python on your machine to get tasks done:


```text
jarvis init --preset code-assistant


```


## Read more


- [OpenJarvis](https://github.com/open-jarvis/OpenJarvis) on GitHub
- [OpenJarvis documentation](https://open-jarvis.github.io/OpenJarvis/)
- [v1.0.0 release notes](https://github.com/open-jarvis/OpenJarvis/releases/tag/v1.0.0)
