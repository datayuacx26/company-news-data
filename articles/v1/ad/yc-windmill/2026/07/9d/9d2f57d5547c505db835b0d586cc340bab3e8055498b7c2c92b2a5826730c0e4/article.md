---
schema_version: "1.0.0"
document_id: "9d2f57d5547c505db835b0d586cc340bab3e8055498b7c2c92b2a5826730c0e4"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/azure-ai-foundry"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:d7cc542d4050f7628fed67e90798bcea161710bbbaa1b42e08fd13490cb734ca"
---

# Azure AI Foundry support for Windmill AI

### [Azure AI Foundry support for Windmill AI](https://www.windmill.dev/changelog/azure-ai-foundry)


AI


AI Chat


AI agents


[Docs](https://www.windmill.dev/docs/core_concepts/ai_generation/#models)


Azure AI Foundry is now natively supported as an AI provider in Windmill, exposing the broader Foundry catalog (OpenAI models plus Llama, DeepSeek, Mistral, Phi and more) over an OpenAI-compatible API in both AI chat and AI agent steps.


#### New features


- Azure AI Foundry now available as a native AI provider via the azure_foundry resource type
- Access to the broader Foundry model catalog beyond Azure OpenAI: Llama, DeepSeek, Mistral, Phi and more
- Uses the same Azure authentication (api-key header) and base URL configuration as Azure OpenAI
- Available in both AI chat (copilot) and AI agent flow steps
