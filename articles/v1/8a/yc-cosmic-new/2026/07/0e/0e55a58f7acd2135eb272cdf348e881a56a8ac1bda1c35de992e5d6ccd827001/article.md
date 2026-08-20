---
schema_version: "1.0.0"
document_id: "0e55a58f7acd2135eb272cdf348e881a56a8ac1bda1c35de992e5d6ccd827001"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/semantic-search-cms-tutorial"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:2a923243383063410afef38a1a354246a4571ec826c76782cbea3efe2037ddea"
---

# How to Add Semantic Search to Your App with Cosmic

Keyword search has a ceiling. A user types "pricing for small teams" and your search returns nothing, because your pricing page says "plans for startups." The words don't match even though the meaning does.


Cosmic's new[semantic search](https://www.cosmicjs.com/blog/semantic-search-find-content-by-meaning) feature solves this at the infrastructure layer. Your content is automatically indexed as vector embeddings when you write or update it. At query time, you send a natural-language string and get back the objects whose *meaning* is closest to your query, ranked by a relevance score. No extra infrastructure, no separate vector database, no embedding pipeline to maintain.


This tutorial walks you through enabling the feature, making your first search call, filtering results, and wiring it into a real UI.


> **Beta note:** Semantic search is currently in beta. Embeddings are generated for free. Queries consume AI tokens from your plan's allocation.


---


## Prerequisites


- A Cosmic account and bucket ([sign up free](https://app.cosmicjs.com/signup) )
- Your bucket slug and **write key** (semantic search requires the write key, not just the read key)
- Node.js 18+ if following the JS examples


---


## Step 1: Enable Semantic Search


In your Cosmic dashboard, go to *Project Settings → Semantic Search* and toggle the feature on. New projects have it enabled by default.


Once enabled, Cosmic begins generating embeddings for your existing content in the background. For large buckets this may take a few minutes. Any new or updated objects are indexed immediately on write.


---


## Step 2: Your First Semantic Search Call


The endpoint lives at the Cosmic workers layer:


```text


```


It requires your **write key** in the header.


### Request


```text


```


### Response shape


```text


```


The field is a cosine similarity value between 0 and 1. Higher scores mean closer semantic match. The scores in this example are illustrative output, not guaranteed minimums.


Note that the response returns lightweight references (, , , , , , ), not full Cosmic objects. To get full content for display, you'll need a follow-up SDK call (see Step 3).


---


## Step 3: Fetch Full Object Data After Search


The search response returns values and scores. To get full content for display, fetch the matched objects using the :


```text


```


> **Note on the write key:** Keep in a server-side environment variable only. Never expose it in client-side code or a public bundle.


---


## Step 4: Filter by Object Type, Status, or Locale


You can scope results to a specific content type or status to avoid surfacing draft content or unrelated types:


```text


```


Useful combinations:


- *Support search box:*
- *Internal knowledge base:*
- *Localized product search:*


---


## Step 5: Build a Search UI in Next.js


Here's a minimal server action + component pattern for a Next.js App Router project:


```text


```


```text


```


---


## Step 6: Semantic Search vs Structured Queries


Semantic search and Cosmic's standard structured queries solve different problems. Use the right tool for each:


*Use semantic search when:*


- Users type natural-language questions or descriptions
- You want fuzzy, intent-based matching across your entire content library
- The query words won't necessarily appear verbatim in the content


*Use structured queries when:*


- You're filtering by a known metadata value ()
- You need exact-match lookups (by slug, ID, or tag)
- You're paginating a known content type


The two approaches compose well: use semantic search to find candidate values, then a structured call to fetch and filter the full objects.


*Docs:*[Cosmic REST API queries](https://www.cosmicjs.com/docs/api/queries) |[JavaScript/TypeScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk)


---


## Step 7: Power an AI Agent with Semantic Search


Semantic search pairs naturally with the Cosmic MCP server. When an AI agent (Claude, GPT, Cursor, etc.) connects to your Cosmic bucket via MCP, it can call with a natural-language query to retrieve relevant objects before generating a response. This is the RAG pattern: Retrieval-Augmented Generation using your own CMS content as the knowledge base.


See the[Cosmic MCP server docs](https://www.cosmicjs.com/docs/mcp-server) to wire this up.


---


## What's Next


Semantic search is live in beta for all Cosmic buckets. Here's what to do next:


1. Enable it in *Project Settings → Semantic Search*
2. Try a query against your existing content using the example from the[docs](https://www.cosmicjs.com/docs/api/content-rag)
3. Wire it into your app following the patterns above
4. [Book a demo](https://calendly.com/tonyspiro/cosmic-intro) if you want a walkthrough with your specific content model


[Start building for free on Cosmic →](https://app.cosmicjs.com/signup)
