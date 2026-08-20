---
schema_version: "1.0.0"
document_id: "d923c8e95e51020256e9c5534d44147ec083bdb9b2413d1cf84f1ebf9f04573b"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-agent-reasoning-effort"
published_at: "2026-07-04T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:ca34c685be9808303c714aad37c2a9c88d024beca7c05e4e4d88d4abd519f6a3"
---

# Reasoning effort for AI agent steps

### [Reasoning effort for AI agent steps](https://www.windmill.dev/changelog/ai-agent-reasoning-effort)


AI


[Docs](https://www.windmill.dev/docs/core_concepts/ai_agents)


AI agent steps can now use extended thinking, configured with a model-adaptive reasoning effort dropdown in the provider/model picker. Supported on Anthropic Claude, OpenAI o-series and GPT-5, Azure OpenAI, Google AI Gemini, AWS Bedrock Claude, OpenRouter, DeepSeek, and Mistral, with an explicit off option. Reasoning tokens are billed, and thinking is streamed into a collapsible Thinking box in chat mode.


#### New features


- A reasoning effort dropdown appears below the model in the AI agent step provider/model picker, with model-adaptive levels (low / medium / high, plus minimal / xhigh / max on some models) and an explicit off option.
- Supported on Anthropic Claude, OpenAI o-series and GPT-5, Azure OpenAI, Google AI Gemini, AWS Bedrock Claude, OpenRouter, DeepSeek, and Mistral; available levels depend on the specific model.
- Reasoning tokens are billed and counted in the step token usage; thinking is streamed into a collapsible Thinking box in the flow chat interface during execution.
