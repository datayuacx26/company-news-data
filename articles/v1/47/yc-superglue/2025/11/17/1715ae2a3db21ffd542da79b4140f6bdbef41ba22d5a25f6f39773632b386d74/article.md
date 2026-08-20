---
schema_version: "1.0.0"
document_id: "1715ae2a3db21ffd542da79b4140f6bdbef41ba22d5a25f6f39773632b386d74"
company_key: "yc-superglue"
company: "superglue"
source_id: "yc-superglue-news-import-11c072c5544e"
canonical_url: "https://superglue.ai/blog/documentation-context/"
published_at: "2025-11-21T00:00:00+00:00"
first_seen_at: "2026-07-22T15:16:40.020846+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:20acd36dd140101b5d959d4d8558c3045994dffa4050478679cd8163af8c2e87"
---

# Why Fetching Documentation for Context-Engineering is Hard

In building integrations using AI, one of the toughest engineering tasks is turning third-party documentation into reliable context for agents. Whether the target is a CRM, a payments gateway, or a legacy ERP, the agent needs precise details: valid endpoints, input/output formats, headers, error semantics. If the model can't see the right snippets of docs at runtime, it will guess — and guesses break production.


## The documentation–LLM ingestion challenge


Documentation comes in many shapes: polished Mintlify sites, Swagger UIs, GraphQL playgrounds, old OpenAPI specs, or PDF manuals. For humans, reading docs is intuitive — you scan examples, check authentication, and test an endpoint. For LLMs, every format introduces a new obstacle. It can rely on its training data — thousands of examples for common tasks like creating an invoice in Stripe — but likely has very few references for newer endpoints, like the recently released OpenAI Responses API, or for niche systems such as a decade-old ERP.


It might seem that giving the LLM a web-search tool would solve the problem, but this alone isn't sufficient. Modern documentation has also become increasingly visual and interactive — with dynamic code examples, JS-rendered components, and language toggles. These look great, but make scraping and parsing much harder. Big docs (like AWS's hundreds of pages) can't just be dumped into the context; only the *relevant* parts matter. And with scraping protections or headless-browser blocks, even fetching those parts can fail. Swagger UIs and GraphQL playgrounds are especially tricky since their content loads only after user interaction.


[Context7's](https://context7.com/) community-driven approach to user-generated documentation is an interesting idea - outsourcing the creation of machine-readable docs to the community - promising but at the moment constrained by the quality of uploaded often incomplete documentation.


## Our approach at superglue


At superglue, documentation handling is at the **core** of our system: it directly affects agent performance and tool reliability. Our goal is simple: enable an agent to interact with *any* integration in the world, given only a link to its documentation, a PDF, or an old OpenAPI spec.


To achieve that, we built a layered documentation-fetching and retrieval pipeline:


1. **Adaptive fetchers** — format-aware extractors for Mintlify, Swagger UI, GraphQL introspection pages, static HTML, PDFs, raw OpenAPI specs and many more. Each fetcher includes fallbacks when the straightforward path fails.
2. **Normalize → Markdown** — convert extracted content into clean, LLM-readable markdown segments (titles, examples, authentication, schema).
3. **Chunk & embed** — split markdown into logical chapters mapped to likely developer questions, then embed for retrieval.
4. **Rerank & retrieve** — at runtime, the system reranks chunks against the agent's prompt to surface only the most relevant information. This reranking already integrates chunk usefulness signals — learned from past agent interactions — to prioritize proven, high-value content.
5. **System-awareness tool** — before execution, the agent receives a concise "system card" describing endpoints, auth, and active chunk pointers for narrow, context-rich operation.


Documentation Processing Pipeline


Documentation Website


→


HTML


→


Markdown


→


Chunks


→


Retrieval


Alongside this, we continuously evaluate our system through **closed-loop integration tests** : real or sandboxed API scenarios where the agent must complete tasks using only fetched documentation. Each update to our fetchers or rerankers runs through these evals to measure actual performance gains, not just retrieval metrics.


## Dream: LLM-readable markdown docs


In a better world, every API provider would publish, alongside its human-facing docs, a versioned markdown bundle optimized for LLMs:


- small, labeled chapters (auth, examples, error codes, schema)
- clean, static examples without dynamic elements
- an index file with canonical endpoints and chunk metadata


This approach would make ingestion, chunking and embedding almost effortless while opening integration to every AI agent. We already see early traction, as some teams bake this into their docs. Fireflies.ai for example offers a downloadable` llms.txt` that mirrors their full documentation in markdown form, ready for models to consume[\[ref\]](https://docs.fireflies.ai/getting-started/llm-development) .


## Where we go from here


We're extending fetcher coverage, refining our rerankers, and expanding real-world eval scenarios. The "LLM-readable markdown" vision is our north star — shorter integration cycles, fewer surprises, and more reliable agent behavior.


If you're integrating systems and cursor just does not understand your API quirks, reach out to superglue viaEmail .
