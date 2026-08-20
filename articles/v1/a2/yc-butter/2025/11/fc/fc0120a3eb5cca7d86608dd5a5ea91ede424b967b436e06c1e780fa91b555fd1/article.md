---
schema_version: "1.0.0"
document_id: "fc0120a3eb5cca7d86608dd5a5ea91ede424b967b436e06c1e780fa91b555fd1"
company_key: "yc-butter"
company: "Butter"
source_id: "yc-butter-rss-cfe99054af6a"
canonical_url: "https://blog.butter.dev/changelog-0006"
published_at: "2025-11-15T02:59:24+00:00"
first_seen_at: "2026-07-24T22:18:01.434032+00:00"
fetched_at: "2026-07-28T20:55:16.082008+00:00"
content_hash: "sha256:f2641557f969192c8807f74467d0ee2654c25cab31a4370e84c05f5295c913c6"
---

# Changelog #0006

Welcome to Butter’s latest weekly changelog. Today’s log is light, as we strengthen our focus on the R&D-side of templated caching.


This week, we’ve also seen some more signups, which have brought about a higher volume of requests through Butter’s proxy and impressive cache hit rates!


## Unsupported Request Handling


-


Butter now detects and tracks requests with unsupported content (i.e., images, audio, files)


-


Unsupported requests are forwarded directly to the provider (preventing any downtime)


-


Butter’s app transparently shows this with a “bypassed” notice, storing none of the original message content
