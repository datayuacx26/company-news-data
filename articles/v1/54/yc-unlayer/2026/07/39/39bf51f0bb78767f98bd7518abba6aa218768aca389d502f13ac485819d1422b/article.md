---
schema_version: "1.0.0"
document_id: "39bf51f0bb78767f98bd7518abba6aa218768aca389d502f13ac485819d1422b"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/ai-in-unlayer-whats-new-mXWeMiOu"
published_at: "2026-07-27T13:00:00+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:361dbc68abd205675078fef2c32b0f11450e70a57a260e24d193b667e9e96586"
---

# AI in Unlayer: What's New

**🆕 AI Assistant** *(available in the stable release)*


New chat-based AI assistant inside the builder — describe a change and it edits the design in place. See docs.


**🆕 AI Importer** *(available in the stable release)*


Convert an existing HTML email or a screenshot into a fully editable template in one click. See docs.


**🆕 AI Credits API** *(available now)*


Programmatically manage and monitor AI Assistant credit consumption for your end-users:


-


**Balance & usage** — retrieve total/used/remaining credits and per-user, per-feature usage breakdowns


-


**Settings** — configure exhaustion behavior, threshold alerts, and your webhook endpoint


-


**Webhooks** — real-time` usage_recorded` ,` threshold_reached` , and` exhausted` events, signed with HMAC-SHA256


Read the docs →[https://docs.unlayer.com/server/cloud-api/ai-credits](https://docs.unlayer.com/server/cloud-api/ai-credits)


> **Note:** In-editor credit enforcement — hiding AI features (` disable` ) or showing an exhausted-credits message (` show_error` ) once credits run out — is available on the **latest** editor version. On **stable** , usage is still metered and all webhooks fire, but AI generations won't be blocked at exhaustion until you move to latest.
