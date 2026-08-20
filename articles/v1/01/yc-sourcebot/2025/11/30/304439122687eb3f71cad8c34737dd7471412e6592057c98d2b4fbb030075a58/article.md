---
schema_version: "1.0.0"
document_id: "304439122687eb3f71cad8c34737dd7471412e6592057c98d2b4fbb030075a58"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/streamed-search"
published_at: "2025-11-24T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T22:25:08.967255+00:00"
content_hash: "sha256:516d5be86de3b171e379a2bf4cff7f550aa717e635bcb1ecd6cc59f809904456"
---

# Streamed search

Tl;DR: we’ve improved search responsiveness by a order of magnitude on larger Sourcebot deployments by implementing streaming.


When you are looking for code using[code search](https://docs.sourcebot.dev/docs/features/search/overview) , search speed is one of the most important metrics that determines if the experience will be good or not.[Zoekt](https://github.com/sourcegraph/zoekt) (the underlying code search engine Sourcebot uses) is exceptionally fast, delivering sub-second search times on hundreds of repositories. However, it has it’s limits - on larger Sourcebot deployments of 5k+ repositories, we noticed that certain searches could take upwards of **10 to 20 seconds** to complete.


To help mitigate this, we’ve added support for streaming in search results as they are found. Previously, results would only appear after the entire search completes. With streaming, results appear as they are found.
