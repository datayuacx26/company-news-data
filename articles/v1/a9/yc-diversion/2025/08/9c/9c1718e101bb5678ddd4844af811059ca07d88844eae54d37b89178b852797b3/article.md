---
schema_version: "1.0.0"
document_id: "9c1718e101bb5678ddd4844af811059ca07d88844eae54d37b89178b852797b3"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/introducing-diversion-webhooks"
published_at: "2025-08-26T00:00:00+00:00"
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:0fc1e5db3b1ac66c47312ec34dec4472e2aa46c12fba36403a77987f1872e2f8"
---

# Introducing Diversion Webhooks!

You asked for it and we delivered! Introducing webhooks for your diversion repositories!


**TL;DR** You told us about the importance of webhooks in your development processes, and we recently launched the feature for diversion.


## Key Features


- **Event-driven notifications:** Real-time notifications for repository events.
- **Secure delivery:** HMAC-SHA256 signature verification for request authenticity.
- **Reliable delivery:** Automatic retries with exponential backoff.
- **Flexible configuration:** Subscribe to specific event types per webhook.
- **Admin-only management:** Repository admin access required.
‍


## Webhooks… why?


Webhooks are a key component of your CI/CD (Continuous Integration / Continuous Delivery) pipeline for games. They’re useful at any scale, but essential for professional game studios.


**The idea is simple:** every time an event is triggered (for example, when a user commits to a repository), a corresponding action is triggered as well. This could include starting a new build, running tests, or notifying other users.
‍
Check out[diversion’s docs](https://docs.diversion.dev/webhooks) to learn more about webhooks.
