---
schema_version: "1.0.0"
document_id: "90bd25b172d0b0f75bbb1eb44e1cb8c82d2b1f7a7fa341f83bc600b67f88232a"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/exa-api-2-1"
published_at: "2025-11-24T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:ddf59d0386b6eac40ca546f2c97afb69bf18303c579945883c612c03a281633b"
---

# Introducing Exa 2.1

[Will Bryk](https://x.com/WilliamBryk) CEO & Co-founder


·


Nov 24, 2025


### Exa 2.1


Today we're announcing Exa 2.1!


We scaled our pre-training and test-time compute by an order of magnitude, unlocking frontier search API performance for both fast and agentic search.


Exa 2.1 includes two major product updates:


1. All Exa search API endpoints have significantly improved in quality -- Exa Fast, Exa Auto, and Exa Deep.
2. The Exa MCP quality has dramatically improved with a new deep search tool.


### Low-Latency Evals


#### Fast search evals


Most of our customers need low-latency search. Exa Fast is now the most accurate search API that’s sub 500ms.


Most search APIs wrap Google search under the hood, which is why they can’t get faster than ~1000ms. We can go faster because we spent years building an independent document index and retrieval algorithm from scratch.


#### Agentic search evals


Other customers just want the highest quality search and don’t mind a few seconds of latency.


Our “Deep” search is now the highest accuracy search API on the market. It agentically searches multiple times to get the optimal results.


You can try out Exa Deep now in the API, or in our MCP server.


#### Eval Methodology


In order to obtain representative benchmarks for the search API's, we ran evals with two different harnesses.


In the fast setting, we ran each API with a minimal \`SingleStep\` harness. This harness, given a search API, LLM query, and expected answer, searches the prompt directly with the search API with the API's default settings. We pass the LLM query and 10 search results to GPT-5 mini, and use gpt-4o-mini to grade the LLM output for correctness against the expected answer.


In the MCP setting, we test how well a simple but powerful agent performs when given access to the MCP of the respective search API. This harness passed the LLM query to GPT-5, which is allowed to call the MCP up to 10 times in sequence with an appropriate query and default settings, and the final output is again graded with gpt-4o-mini against the expected answer.


Low-Latency evals were run 11/23/25 and MCP evals were run as of 11/20/25.


### Building our own search engine


Building a search engine is one of the hardest technical challenges in software. It takes years of research and development to design retrieval algorithms over an index that’s many petabytes. Only a handful of companies have actually been able to do it.


On that path, we’ve needed to build semantic+lexical databases from scratch and massive-scale crawling infrastructure. We’ve rediscovered and then gone beyond retrieval techniques that only Google and Bing used to know.


### What’s coming next


We scaled up our training and index infrastructure in the past couple months and are seeing consistent gains in search quality.


We’ll keep scaling up our techniques. In other words, Exa 2.2 is coming soon :)


If you're interested in building one of the largest ML systems in the world, come[join the squad](https://exa.ai/careers) !


---


#### Cheers,


#### [Will Bryk](https://x.com/WilliamBryk)


SEE MORE


[SOTA Search Over Academic Publications The Exa Team July 23, 2026](https://exa.ai/blog/publications-search)[Introducing Exa Agent The Exa Team June 16, 2026](https://exa.ai/blog/exa-agent)[Exa raises $250M Series C to build the search engine for AIs Will Bryk May 20, 2026](https://exa.ai/blog/announcing-series-c)
