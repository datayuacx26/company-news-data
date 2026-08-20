---
schema_version: "1.0.0"
document_id: "19a86512e2e8ff733655e007f49fb7586fab3b941a95e8a230a623f01b3f3820"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/multiple-api-keys"
published_at: "2025-12-02T00:00:00+00:00"
first_seen_at: "2026-07-21T12:42:16.969450+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:4a16633ef49ad90079a679b8f4041efb015cbf8596612b2cbad01ae52bd45430"
---

# Multiple API keys

[Back to All](https://docs.castle.io/changelog)


Added


Create multiple Publishable API Keys and API Secrets. Rotate or revoke one key without breaking your other integrations.


**Available on Enterprise plans.**


The workflow: create a new key, migrate traffic gradually, delete the old one when ready. Label each key by purpose (Production Web, Mobile iOS, Events API) so you know which is which.


Your current keys keep working, auto-labeled as "Default". No migration needed.
