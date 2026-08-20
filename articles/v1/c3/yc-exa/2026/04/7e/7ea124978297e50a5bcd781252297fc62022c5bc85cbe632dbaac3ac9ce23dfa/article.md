---
schema_version: "1.0.0"
document_id: "7ea124978297e50a5bcd781252297fc62022c5bc85cbe632dbaac3ac9ce23dfa"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/highlights-for-agents"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:f4dbf5e48734b91c8030cf1e32571da6798ead0fb75f807212175639cc4775b6"
---

# Exa Highlights: Quality, Token-Efficient Search

[The Exa Team](https://x.com/ExaAILabs) Apr 22, 2026


Agents use Exa for web grounding: searching the web and reasoning over the contents from those pages to give informed responses. Recent research efforts at Exa have led us to improvements in highlights. Highlights are snippets from a page, that now offer higher quality results for ~94% fewer tokens on some evals, significantly reducing costs and leading to latency benefits.


We've found this is especially important in agentic search use cases, where doing multiple rounds of search is the norm and reducing context bloat is critical. Dense excerpts allow agents to effectively reason and achieve higher accuracy.


On benchmarks like SimpleQA, 500 characters of Exa's highlights match the accuracy of the first 8000 characters of the page, and use 16x fewer tokens. In addition, we found that 4k characters of highlights is better than 32k characters of full text.


### Extract relevant excerpts


Today's highlights model is the product of many rounds of research. We explored a wide range of architectures and training recipes before landing on the current approach.


Highlights are Exa's in-house method to extract the most relevant excerpts from web pages. They run for every request to maximize relevance to the query (not cached!) and complete in under 100ms. We've recently made significant improvements on technical coding docs and long-context documents like API references, SDK docs, specs, and research papers, where the right answer is a single passage buried in tens of thousands of tokens of boilerplate. On pages that long, Exa's highlights still surface the relevant excerpts, with a significant gap at small context budgets (60% vs 6% at 500 characters).


### The engine behind Exa's agentic products


Highlights don't just power the public Highlights API. They're also the retrieval substrate inside Exa's own agentic endpoints:` /answer` , Deep, and Websets. Every iteration of those agent loops reads highlights, not raw page content. That's one of the reasons our agentic products are Pareto dominant on latency, cost, and quality against competing systems. We have more improvements in the pipeline, on both the evals and training sides.


---


#### Cheers,


#### [The Exa Team](https://x.com/ExaAILabs)


SEE MORE


[SOTA Search Over Academic Publications The Exa Team July 23, 2026](https://exa.ai/blog/publications-search)[Introducing Exa Agent The Exa Team June 16, 2026](https://exa.ai/blog/exa-agent)[Exa raises $250M Series C to build the search engine for AIs Will Bryk May 20, 2026](https://exa.ai/blog/announcing-series-c)
