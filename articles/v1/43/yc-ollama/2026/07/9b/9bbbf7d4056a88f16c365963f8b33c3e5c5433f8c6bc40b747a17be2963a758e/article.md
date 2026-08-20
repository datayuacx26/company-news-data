---
schema_version: "1.0.0"
document_id: "9bbbf7d4056a88f16c365963f8b33c3e5c5433f8c6bc40b747a17be2963a758e"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/coding-models"
published_at: null
first_seen_at: "2026-07-24T07:14:45.991039+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:aae2a1b7135a31845e8eabc99d1c0e021673b082999344bf206f400bac4a013f"
---

# New coding models & integrations

[GLM-4.6](https://ollama.com/library/glm-4.6) and[Qwen3-coder-480B](https://ollama.com/library/qwen3-coder) are available on Ollama’s cloud service with easy integrations to the tools you are familiar with. Qwen3-Coder-30B has been updated for faster, more reliable tool calling in Ollama’s new engine.


## Get started


**GLM-4.6**


```text
ollama run glm-4.6:cloud


```


**Qwen3-Coder-480B**


```text
ollama run qwen3-coder:480b-cloud


```


For users with more than 300GB of VRAM,[qwen3-coder:480b](https://ollama.com/library/qwen3-coder) is also available locally.


**Qwen3-Coder-30B**


```text
ollama run qwen3-coder:30b


```


### Example prompts


```text
Create a single-page app in a single HTML file with the following requirements:


Name: Ollama's Adventure
Goal: Jump over obstacles to survive as long as possible.
Features: Increasing speed, high score tracking, retry button, and funny sounds for actions and events.


The UI should be colorful, with parallax scrolling backgrounds.
The characters should look cartoonish, related to alpacas and be fun to watch.
The game should be enjoyable for everyone.


```


[Example code](https://gist.github.com/mchiang0610/32bce599bcf926ad4989ee8136bd35ec) by GLM-4.6 in a single prompt


## Usage with VS Code


First, pull the coding models so they can be accessed via VS Code:


```text
ollama pull glm-4.6:cloud
ollama pull qwen3-coder:480b-cloud


```


1. Open the copilot chat sidebar
2. Select the model dropdown → **Manage models**
3. Click on **Ollama** under **Provider Dropdown,** then select desired models
4. Select the model dropdown → and choose the model (e.g.` glm-4.6` )


## Usage with Zed


First pull the coding models so they can be accessed via Zed:


```text
ollama pull glm-4.6:cloud
ollama pull qwen3-coder:480b-cloud


```


Then, open[Zed](https://zed.dev/download) (now available for Windows!)


1. Click on the agent panel button (glittering stars)
2. Click on the **model dropdown** → **Configure**
3. Select **LLM providers** → **Ollama**
4. Confirm the **Host URL** is **` http://localhost:11434`** , then click **Connect**
5. Select a model under **Ollama**


## Usage with Droid


First,[install Droid](https://docs.factory.ai/cli/getting-started/quickstart) :


```text
curl -fsSL https://app.factory.ai/cli | sh


```


Add the following configuration to` ~/.factory/config.json` :


```text
{
"custom_models": [
{
"model_display_name": "GLM-4.6",
"model": "glm-4.6:cloud",
"base_url": "http://localhost:11434/v1",
"api_key": "not-needed",
"provider": "generic-chat-completion-api",
"max_tokens": 16384
},
{
"model_display_name": "Qwen3-Coder-480B",
"model": "qwen3-coder:480b-cloud",
"base_url": "http://localhost:11434/v1",
"api_key": "not-needed",
"provider": "generic-chat-completion-api",
"max_tokens": 16384
}
]
}


```


Then run Droid and type` /model` to change to the model:


```text
╭──────────────────────────────────────────────────╮
│ > GLM-4.6 [current]                              │
│   Qwen3-Coder-480B                               │
│                                                  │
│ ↑/↓ to navigate, Enter to select, ESC to go back │
╰──────────────────────────────────────────────────╯


```


## Integrations


Ollama’s documentation now includes sections on using Ollama with popular coding tools:


- [Codex](https://docs.ollama.com/integrations/codex)
- [Cline](https://docs.ollama.com/integrations/cline)
- [VS Code](https://docs.ollama.com/integrations/vscode)
- [Zed](https://docs.ollama.com/integrations/zed)
- [Droid](https://docs.ollama.com/integrations/droid)
- [Roo code](https://docs.ollama.com/integrations/roo-code)


## Cloud API access


Cloud models such as` glm-4.6` and` qwen3-coder:480b` can also be accessed directly via ollama.com’s cloud API:


First,[create an API key](https://ollama.com/settings/keys) , and set it in your environment


```text
export OLLAMA_API_KEY="your_api_key_here"


```


Then, call ollama.com’s API


```text
curl https://ollama.com/api/chat \
-H "Authorization: Bearer $OLLAMA_API_KEY" \
-d '{
"model": "glm-4.6",
"messages": [{
"role": "user",
"content": "Write a snake game in HTML."
}]
}'


```


For more information see the Ollama’s[API documentation](https://docs.ollama.com/cloud#cloud-api-access) .
