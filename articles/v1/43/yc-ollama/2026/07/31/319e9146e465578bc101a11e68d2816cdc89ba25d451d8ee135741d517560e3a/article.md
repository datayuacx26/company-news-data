---
schema_version: "1.0.0"
document_id: "319e9146e465578bc101a11e68d2816cdc89ba25d451d8ee135741d517560e3a"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/codex"
published_at: null
first_seen_at: "2026-07-24T07:14:45.991039+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:c7f124b37a1d062a11f3d1f7d807bc2d767a280fc44e13e15510b429c7e842b5"
---

# OpenAI Codex with Ollama

Open models can be used with OpenAI’s Codex CLI through Ollama. Codex can read, modify, and execute code in your working directory using models such as` gpt-oss:20b` ,` gpt-oss:120b` , or other open-weight alternatives.


### Get started


Install Codex CLI:


```text
npm install -g @openai/codex


```


Start Codex with the` --oss` flag:


```text
codex --oss


```


By default, Codex will use the local` gpt-oss:20b` model.


**Note:** Codex requires a large context window. We recommend at least 32K tokens. See the[documentation](https://docs.ollama.com/context-length) for how to adjust context length in Ollama.


### Changing models


You can switch to a different model using the` -m` flag:


```text
codex --oss -m gpt-oss:120b


```


### Cloud models


All models on Ollama Cloud work with Codex.


```text
codex --oss -m gpt-oss:120b-cloud


```


### Learn more


For more detailed setup instructions and configuration options, see the[Codex integration guide](https://docs.ollama.com/integrations/codex) .
