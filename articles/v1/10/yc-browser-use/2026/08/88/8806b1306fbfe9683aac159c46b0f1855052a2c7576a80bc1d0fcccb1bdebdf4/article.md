---
schema_version: "1.0.0"
document_id: "8806b1306fbfe9683aac159c46b0f1855052a2c7576a80bc1d0fcccb1bdebdf4"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/10-8-2026"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-14T04:57:39.353244+00:00"
fetched_at: "2026-08-14T04:57:42.194635+00:00"
content_hash: "sha256:633cab034c174661ae926c7f9711202557f91a4afcb974495fc0601c68596dd8"
---

# Session Sharing, Usage Analytics & X402 Pricing

## Sessions


- Delete and rename sessions directly from the sidebar.
- Share a session via a public link at` /share/v4` .
- Leave thumbs up/down feedback on individual run outputs.


## X402 API


- Pay-what-you-want top-ups, from $0.01 up to $100.
- Managing a session you already opened — polling, reading messages, stopping, deleting, sending follow-ups — is now free. Payment is only required to open a new browser.


## Usage


Usage and analytics moved to a single redesigned page, with Agents, Browsers, and LLM Gateway views.


## Model Improvements


- Reasoning output now shows consistently across all model providers, streamed in as it's generated.
- Per-run cost caps now scale per model (up to $100) instead of a flat $25, so pricier models aren't starved of usable runs.


## Fixes


- Streaming stalls that could hard-fail a run with no output now retry automatically.
- The Files tab no longer appears empty for workspaces with many files.
- Recording tabs are closable, and recordings live in a dedicated Recordings folder in Files.
- Email attachments received via AgentMail are retrievable again.
- 401 errors on the API now link to API key signup instead of a dead end.
- The first-party app gets the same defaults as web chat — cost caps, recording, and skills.
