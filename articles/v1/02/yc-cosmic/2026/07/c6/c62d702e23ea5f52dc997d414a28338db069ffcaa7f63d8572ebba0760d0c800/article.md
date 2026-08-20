---
schema_version: "1.0.0"
document_id: "c62d702e23ea5f52dc997d414a28338db069ffcaa7f63d8572ebba0760d0c800"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/semantic-search-find-content-by-meaning"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:fa717dd072144ae92ddb6a014017c8d1887db36921da47076e3933858b19b2cb"
---

# Semantic Search: Find Content by Meaning

Cosmic now understands what your content is *about* . **Semantic search** (Content RAG) embeds your objects into a vector index so a natural-language query returns the most relevant content, even when the wording is different from what's stored. It's a new API endpoint, and it also powers your Agents.


Your content is embedded on write and kept in sync automatically as you create, edit, publish, unpublish, and delete objects, and embedding is free: only queries consume AI tokens. Semantic search is in Beta, on by default for new projects and workspaces, and an admin or manager can turn it on for existing ones under **Settings → Semantic search** .


## What's New


- **Semantic search endpoint.** embeds your query and returns the most semantically relevant objects in the bucket, ranked by score, with optional , , , , and filters.
- **Agents that retrieve by meaning.** With CMS read access, Agents get a new tool so they can find the right content by meaning instead of scanning titles, making support and Q&A answers more accurate. This is especially useful for[lean teams running their content operations through Slack-connected AI Agents](https://www.cosmicjs.com/blog/ai-agents-slack-lean-teams-content-operations) , where an agent needs to surface the right post or product detail quickly without a developer wiring up search logic.
- **Always in sync.** Your content is embedded on write and kept current as you create, edit, publish, unpublish, and delete objects. Embedding your content is free.


## Why This Matters


Keyword and structured queries are great when you know the exact field or phrase. But real questions ("how do refunds work on annual plans?") rarely match the exact words in your content. Semantic search bridges that gap: it matches intent, so your apps, support bots, and Agents surface the right objects the first time.


For teams where[marketers and non-technical editors own the content](https://www.cosmicjs.com/blog/marketing-team-edit-content-without-code) , this is also a practical quality-of-life improvement. When content is queried by meaning rather than exact keyword, editors don't need to obsess over the precise title or slug of a post for it to be findable. The system understands the topic.


## How It Works


1. When semantic search is enabled for your account, Cosmic embeds your objects' searchable text into a vector index and keeps it in sync automatically.
2. Call with a natural-language (bucket write key required) to get ranked results with a text snippet for each match.
3. Give an Agent CMS read access and it can use to ground its answers in your content.


Notes:


- Embedding your content on write is free. Query-time search consumes AI tokens like other AI features.
- Semantic search complements[structured queries](https://www.cosmicjs.com/docs/api/queries) : use queries for exact filters, semantic search for meaning. See the[Semantic search docs](https://www.cosmicjs.com/docs/api/content-rag) .
- In Beta. On by default for new projects and workspaces; existing ones can turn it on under Settings → Semantic search.


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)
