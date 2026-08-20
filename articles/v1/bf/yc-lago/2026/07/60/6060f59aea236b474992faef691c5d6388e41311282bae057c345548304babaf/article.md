---
schema_version: "1.0.0"
document_id: "6060f59aea236b474992faef691c5d6388e41311282bae057c345548304babaf"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/agent-sdk-bill-the-dollar-cost-of-llm-calls-margin-built-in"
published_at: "2026-07-21T00:40:05+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:6019df2df66b21eef41c3f5eab717608cf81bf6375ad5b44dee1a11ac222267c"
---

# Agent SDK: bill the dollar cost of LLM calls, margin built in

Reselling LLM access used to mean building a pricing service.


Your app knew how many tokens a customer used. Turning that usage into a dollar amount was your problem: fetch provider price lists, keep them current, calculate every call, apply your margin, and handle missing prices without losing usage.


That pricing layer is now built into the Lago Agent SDK.


## One cost event per call


The Agent SDK already turns LLM token usage into billing events. Switch on **price mode** and it also prices each call.


The SDK looks up the model price, calculates the dollar cost from the token usage, applies your markup, and sends Lago one` llm_cost` event. That event is ready to bill through a dynamic charge.


Set` markup: 1.2` , for example, and the customer pays your cost plus 20%.


Your LLM costs stay centralized in Lago. You decide what to resell them for.


## No price file to maintain


Prices come from Lago-maintained public price lists:


- OpenRouter for native OpenAI, Anthropic, Mistral, and Google Gemini models
- The AWS Bedrock public price list for models called through Bedrock


The SDK refreshes prices in the background. You do not need provider API keys or a price file of your own.


## Missing prices do not become missing revenue


A new model or a cold cache should not make usage disappear.


If the SDK cannot find a price, it falls back to the normal token-count events and calls your error handler. The usage still reaches Lago, and your team gets a visible signal to investigate.


Price mode is opt-in. Token mode remains the default, so existing integrations do not change until you enable it.


Price mode is available in the open-source[Python SDK](https://github.com/getlago/lago-agent-sdk-python) and[JavaScript / TypeScript SDK](https://github.com/getlago/lago-agent-sdk-js) .


[Read the documentation](https://getlago.com/docs/guide/ai-agents/agent-sdk#bill-in-tokens-or-in-dollars) ·[See the changelog](https://getlago.com/docs/changelog/product#agent-sdk-bill-the-dollar-cost-of-llm-calls-with-your-margin-built-in)
