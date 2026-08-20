---
schema_version: "1.0.0"
document_id: "5f5fe240d3c2108c51d2df5d07a0cc34df518d6994a66f8a3b23905cccbc9012"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/exa-deep"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:e4282f6e578aa90ccbdb863a4a5b99c9ba08a8f5d2f03fa2c9c203b9eb31288e"
---

# Introducing Exa Deep: An Agent for Every Search

[The Exa Team](https://x.com/ExaAILabs) Mar 4, 2026


### Introducing Exa Deep: An Agent for Every Search


Today we're launching a revamped **Exa Deep** : our best agentic search endpoint. It uses optimized query expansion and LLM reasoning to get high-quality search results. Deep is already used for detailed people & company search, financial research, news reports and more.


This version is faster, cheaper, and adds structured outputs with field-level grounding.


Behind the scenes, Exa Deep uses our recently launched Exa Instant search and LLM reasoning to understand the intent behind your query, generating multiple search agents in parallel, and synthesizing results with citations.


### Evals


We benchmarked Exa Deep against Perplexity (Sonar Reasoning Pro) and Parallel Task API (Core, Base) on three challenging evals: HLE-Search, Deep Search QA, and FRAMES.


**On every benchmark Exa Deep is up and to the left: faster and more accurate.**


### How are people using Deep?


These are categories of queries where Deep's capabilities — multi-source synthesis, structured outputs, and grounded citations — are especially valuable:


**Financial research.** SEC filings, quarterly reports, company intelligence. Queries that require pulling from multiple documents and synthesizing.


**Scientific literature.** Researchers using it to survey recent publications on a topic. About 20% of Deep queries.


**News monitoring.** Detailed, ongoing monitoring at set cadences. Deep handles the complexity of tracking evolving stories across sources.


**Research agents.** If you're building a deep research agent, Deep is the search primitive you want underneath. One API call replaces a complex orchestration layer.


### Structured Outputs, Enrichments and Grounding


You can define an output schema, and Deep returns structured JSON with field-level citations:


` // Query: Top aerospace companies and their CEO's name "output": { "content": { "companies": \[ { "ceo_name": "Christopher T. Calio", "company_name": "RTX Corporation" }, { "ceo_name": "Kelly Ortberg", "company_name": "Boeing" }, // ... \] }, "grounding": \[ { "field": "companies\[0\].ceo_name", "citations": \[ { "url": "https://fintool.com/app/research/companies/RTX/people/christopher-t-calio", "title": "Christopher Calio - Executive Profile & Compensation" }, // ... \], "confidence": "high" } \] }`


### How to use Exa Deep


Set` type` to` deep` or` deep-reasoning` in your API request. Add an` outputSchema` for structured outputs.


Tier Latency Price


` deep` 4-12s $12/1k requests


` deep-reasoning` 12-50s $15/1k requests


Regular` deep` is 20% cheaper than before.


Test it out at[dashboard.exa.ai](https://dashboard.exa.ai/) . Docs[here](https://docs.exa.ai/) .


### Where search is going


Search and agents are converging. The best way to answer a complex query isn't a single search. It's an agent that breaks down the query, searches in parallel, and iterates until it has what it needs.


AIs will soon search the web more than humans, and these AIs need search that can reason, not just retrieve. Exa Deep is our answer.


We're[hiring](https://exa.ai/careers) . Come help us build it.


---


#### Cheers,


#### [The Exa Team](https://x.com/ExaAILabs)


SEE MORE


[SOTA Search Over Academic Publications The Exa Team July 23, 2026](https://exa.ai/blog/publications-search)[Introducing Exa Agent The Exa Team June 16, 2026](https://exa.ai/blog/exa-agent)[Exa raises $250M Series C to build the search engine for AIs Will Bryk May 20, 2026](https://exa.ai/blog/announcing-series-c)
