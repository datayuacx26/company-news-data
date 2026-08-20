---
schema_version: "1.0.0"
document_id: "76d5a3c5aac52dccba6d0afcce87dca7b33059e5085bf10e614036e9f93dac3e"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/minimax-m2"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:647fb7275a68bccc6629e28da546705e419c9e92948a6a522f00435ff4c8c9b2"
---

# MiniMax M2

MiniMax M2 is now available on Ollama’s cloud. It’s a model built for coding and agentic workflows.


### Get Started


```text
ollama run minimax-m2:cloud


```


### Highlights


**Superior Intelligence** . According to benchmarks from Artificial Analysis, MiniMax-M2 demonstrates highly competitive general intelligence across mathematics, science, instruction following, coding, and agentic tool use. **Its composite score ranks #1 among open-source models globally** .


**Advanced Coding** . Engineered for end-to-end developer workflows, MiniMax-M2 excels at multi-file edits, coding-run-fix loops, and test-validated repairs. Strong performance on Terminal-Bench and (Multi-)SWE-Bench–style tasks demonstrates practical effectiveness in terminals, IDEs, and CI across languages.


**Agent Performance** . MiniMax-M2 plans and executes complex, long-horizon toolchains across shell, browser, retrieval, and code runners. In BrowseComp-style evaluations, it consistently locates hard-to-surface sources, maintains traceable evidence, and gracefully recovers from flaky steps.


**Efficient Design** . With 10 billion activated parameters (230 billion in total), MiniMax-M2 delivers lower latency, lower cost, and higher throughput for interactive agents and batched sampling—perfectly aligned with the shift toward highly deployable models that still shine on coding and agentic tasks.


### Benchmarks


MiniMax M2 comparison with several other mainstream models


Artificial Analysis intelligence index


### Usage with VS Code


First, pull the coding models so they can be accessed via VS Code:


```text
ollama pull minimax-m2:cloud


```


1. Open the copilot chat sidebar
2. Select the model dropdown → **Manage models**
3. Click on **Ollama** under **Provider Dropdown,** then select desired models
4. Select the model dropdown → and choose the model (e.g.` minimax-m2:cloud` )


### Usage with Zed


First pull the coding models so they can be accessed via Zed:


```text
ollama pull minimax-m2:cloud


```


Then, open[Zed](https://zed.dev/download) (now available for Windows!)


1. Click on the agent panel button (glittering stars)
2. Click on the **model dropdown** → **Configure**
3. Select **LLM providers** → **Ollama**
4. Confirm the **Host URL** is **` http://localhost:11434`** , then click **Connect**
5. Select a model under **Ollama**


### Usage with Droid


First,[install Droid](https://docs.factory.ai/cli/getting-started/quickstart) :


```text
curl -fsSL https://app.factory.ai/cli | sh


```


Add the following configuration to` ~/.factory/config.json` :


```text
{
"custom_models": [
{
"model_display_name": "MiniMax-M2",
"model": "minimax-m2:cloud",
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
│ > MiniMax-M2 [current]                              │
│   Qwen3-Coder-480B                               │
│                                                  │
│ ↑/↓ to navigate, Enter to select, ESC to go back │
╰──────────────────────────────────────────────────╯


```


### Integrations


Ollama’s documentation now includes sections on using Ollama with popular coding tools:


- [Codex](https://docs.ollama.com/integrations/codex)
- [Cline](https://docs.ollama.com/integrations/cline)
- [VS Code](https://docs.ollama.com/integrations/vscode)
- [Zed](https://docs.ollama.com/integrations/zed)
- [Droid](https://docs.ollama.com/integrations/droid)
- [Roo code](https://docs.ollama.com/integrations/roo-code)


### Cloud API access


Cloud models such as` minimax-m2:cloud` can also be accessed directly via ollama.com’s cloud API:


First,[create an API key](https://ollama.com/settings/keys) , and set it in your environment


```text
export OLLAMA_API_KEY="your_api_key_here"


```


Then, call ollama.com’s API


```text
curl https://ollama.com/api/chat \
-H "Authorization: Bearer $OLLAMA_API_KEY" \
-d '{
"model": "minimax-m2",
"messages": [{
"role": "user",
"content": "Write a snake game in HTML."
}]
}'


```


For more information see the Ollama’s[API documentation](https://docs.ollama.com/cloud#cloud-api-access) .
