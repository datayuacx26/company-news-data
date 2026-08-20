---
schema_version: "1.0.0"
document_id: "fe86ebb36f4544230dd53855d322a2d3a236d639722197ff47da1be6af8e37a4"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/webhook-api-key-selection"
published_at: "2026-01-26T00:00:00+00:00"
first_seen_at: "2026-07-21T12:42:16.969450+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:c57e04734e3418408af7975164ce5b08f8529217551590bbfd1cbb0b1e2b196e"
---

# Webhook API Key selection

[Back to All](https://docs.castle.io/changelog)


Added


Castle now lets you choose which API Secret is used for webhook signing. This makes it easier to isolate integrations and rotate secrets independently.


Available on Enterprise plans.


If you don’t make a selection, webhooks will continue using the API Secret that was previously in use.
