---
schema_version: "1.0.0"
document_id: "3939663fa3b89c20417319a0706aa2dc1d9f72842c7b076042db4104979ce211"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/perf-improvements-31-10-25"
published_at: "2025-10-31T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:d0d366d7f1a8b43bb064c981064772ab55c5b62ee34e4bec2728d482086bf8d6"
---

# Performance improvements

Happy Halloween! 🎃 We’ve been heads down solving some deep technical problems in order to make Sourcebot scalable and performant for larger instances. Here are some of the highlights of our efforts:


-


Improved search performance by as much as **41x** for unbounded queries ([#555](https://github.com/sourcebot-dev/sourcebot/pull/555) ). Performance improvements will be most notable when searching for common search terms (e.g.,` function` or` log` ). We are continuing to focus on improving search performance, and will have more updates soon.


-


Improved web app responsiveness, including reducing the Time to Interactive (TTI) by as much as **10x** for larger deployments. ([#563](https://github.com/sourcebot-dev/sourcebot/pull/563) )


-


Improved repository indexing stability with stronger concurrency primitives ([#563](https://github.com/sourcebot-dev/sourcebot/pull/563) ). In our testing of indexing 10k repositories, we reduced the error rate from 5% down to **0%** .


-


Added additional UI to view in progress jobs for repository indexing and connection syncing. ([#579](https://github.com/sourcebot-dev/sourcebot/pull/579) )
