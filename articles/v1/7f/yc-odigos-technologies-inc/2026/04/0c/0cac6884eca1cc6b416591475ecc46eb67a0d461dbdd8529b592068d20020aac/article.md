---
schema_version: "1.0.0"
document_id: "0cac6884eca1cc6b416591475ecc46eb67a0d461dbdd8529b592068d20020aac"
company_key: "yc-odigos-technologies-inc"
company: "Odigos Technologies Inc."
source_id: "yc-odigos-technologies-inc-rss-eb41174e661a"
canonical_url: "https://odigos.io/blog/llm-calls-are-the-new-blind-spot"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-25T17:02:44.020335+00:00"
fetched_at: "2026-08-20T02:53:26.937902+00:00"
content_hash: "sha256:d9ec441e1c95065fc87310c7e562297c3da0c8b15072cef70a8434d9cb3579c8"
---

# LLM Calls Are the New Blind Spot

Python services calling LLM providers in production typically show up in traces as a generic outgoing HTTP call to api.openai.com. No model, no tokens, no finish reason. Odigos has bundled seven OpenTelemetry GenAI instrumentations into the Python auto-instrumentation distro so those spans show up automatically, with no code changes.
