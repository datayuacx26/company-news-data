---
schema_version: "1.0.0"
document_id: "ae2020df1cdbdbf277444f854617bf649e4147e7c1181b83d012f813827e6ead"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-chat-web-search-sources"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-27T09:58:44.512719+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:6d5eb76f0bf88800c34e0b82de59708d7f9154f2cdf44bd2264fcf1a00f8e067"
---

# Web search sources in AI chat

### [Web search sources in AI chat](https://www.windmill.dev/changelog/ai-chat-web-search-sources)


AI


[v1.764.0](https://github.com/windmill-labs/windmill/releases/tag/v1.764.0)


[Docs](https://www.windmill.dev/docs/core_concepts/ai_generation#web-search)


When the AI chat searches the web (OpenAI and Anthropic providers), the tool card now shows the search query as it is typed and expands with the list of consulted sources (favicon, page title and link) as each search completes. Sources are kept with the conversation history.


#### New features


- The web search tool card shows the search query live in its label while the model types it.
- The card auto-expands with the list of consulted pages (favicon, title and link) as soon as each search completes, while the model continues its answer.
- Sources persist with the conversation history.
