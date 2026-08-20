---
schema_version: "1.0.0"
document_id: "80dc893a6dafc3ae8a65a362c0db396654fe17fd9f91c25f0fa933b5256147d2"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/exa-agent"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:0385b2b5c6f77a738535dffde42bff21fa4159b4a41954df60445da8d1c04be6"
---

# Introducing Exa Agent

[The Exa Team](https://x.com/ExaAILabs) Jun 16, 2026


We're releasing Exa Agent: a single API to access frontier web research capabilities at a fraction of the cost.


Exa Agent combines top language models with Exa's state-of-the-art web search tools to achieve the most exhaustive and accurate results now used in a variety of agentic products.


### Exa Agent capabilities


Exa Agent is highly effective on deep research, list-building, and entity enrichment tasks.


When working with large datasets, Exa Agent divides the task into many subtasks and assigns subagents to research various domains at once. When researching, it uses a fusion of frontier and cost-effective models to find the most cost-effective methodology for the given research task.


To quantify performance, we used an array of public benchmarks that measure quality, latency, cost and token efficiency.


Our goal was to produce the best web research agent using methodologies like model fusion and Exa's token-efficient highlights model[(which have shown up to 94% reductions in token usage)](https://exa.ai/blog/highlights-for-agents) . The result is significantly lower cost and latency for frontier performance.


WideSearch


Row-F1 / Cost


Row-F1 (%)


5


15


25


35


45


55


Low


Medium


Exa Agent High


Perplexity Agent Fast


Perplexity Agent Pro


Perplexity Agent Deep


Perplexity Agent Advanced


Parallel Task Core


Parallel Task Pro


Parallel Task Ultra


Parallel Task Ultra 2x


Opus 4.8


GPT 5.5


$0.00


$0.50


$1.00


$1.50


$2.00


$2.50


Cost per query ($)


WideSearch Methodology


[WideSearch](https://widesearch-seed.github.io/) was introduced in August 2025 to evaluate agents' abilities to aggregate and structure atomic information about entities from across the web. The expected output is always a table, structured as a list of enriched entities.


The English version contains 100 tasks, with the number of required columns varying from 3 to 14. In our setup, we evaluate only the agent's final answer. We compute F1 over rows: a row is counted as successful only if the matched entity and all required enrichment columns are valid. Otherwise, it is counted as a miss.


We experimented with cell-level F1, but found it too permissive because it rewards isolated enrichment values even when the agent failed to ground them to the correct entity.


### How teams are using Exa Agent


Companies building their own agents can plug into the Exa Agent API directly.


Finance Agents can take advantage of Exa Agent's ability to retrieve real-time data from across the web, aggregating it into any output format required.


Go-to-market agents can bring their own list of accounts or prospects to enrich, or use Exa Agent's list-building capabilities to generate lists of tens or hundreds of entities.


Research all publicly available information on Databricks: recent funding, product launches, partnerships, executive hires, conference talks, and public GitHub activity.


Exa Agent API result


Run completed June 16, 2026


Compiled a compact Databricks company brief across funding, launches, partnerships, hires, events, and public GitHub activity. The main signal is momentum from a $134B February financing and a product narrative around governed enterprise AI agents, Lakebase, Genie, and OpenSharing.


Area


Finding


Evidence


1


Recent funding


>$7B announced in February 2026 at a $134B valuation


Databricks announced more than $7B total investment, including about $5B equity and about $2B additional debt capacity. Reports also cited discussions for a possible $165B-$175B round in June.


Databricks


MarketScreener


2


Product launches


OpenSharing, Agent Bricks, Lakebase, Genie, AI Gateway, and Lakeflow


Recent launches cluster around governed enterprise agents, model access, serverless Postgres for AI agents, conversational analytics, open data/AI asset sharing, and data engineering.


Databricks


Docs


3


Partnerships


Frontier model, SI, open-source, and data ecosystem partnerships


The partner map includes Anthropic, Accenture, OpenAI, Linux Foundation OpenSharing, and ecosystem partners such as SAP, Stripe, Atlassian, Retool, LlamaIndex, Collibra, Deloitte, and Wipro.


Databricks


Linux Foundation


+4 more rows


Notes


Compiled from Databricks press releases, blogs, docs, GitHub pages, Reuters/MarketScreener reporting, Linux Foundation materials, and conference pages as of 2026-06-16. The June 2026 valuation item is reported discussion, not confirmed closed financing.


### Try the API today


Exa Agent using` auto` effort will dynamically scale to the amount of compute required by the given task. Developers can also set an effort level for fixed costs:


Effort Cost Best for


` minimal` $0.012 / request Lightweight tasks, lowest cost


` low` $0.025 / request Simple lookups, narrow factual tasks, short answers


` medium` $0.10 / request Default starting point for most standard research tasks


` high` $0.50 / request Harder research, more citations, stricter completeness


` xhigh` $1.00 / request High-value tasks where completeness matters more than cost/latency


The API also allows developers to use structured outputs via the` outputSchema` parameter, and to bring their own data using` input.data` . Exa Agent uses many tools to operate efficiently over large datasets.


Exa Agent is available today. Read the[docs](https://exa.ai/docs/reference/agent-api-guide) or try it in the Exa API Playground:[dashboard.exa.ai/playground/agent](https://dashboard.exa.ai/playground/agent)


---


#### Cheers,


#### [The Exa Team](https://x.com/ExaAILabs)


SEE MORE


[SOTA Search Over Academic Publications The Exa Team July 23, 2026](https://exa.ai/blog/publications-search)[Exa raises $250M Series C to build the search engine for AIs Will Bryk May 20, 2026](https://exa.ai/blog/announcing-series-c)[Exa and Google Partnership The Exa Team April 28, 2026](https://exa.ai/blog/exa-google-cloud)
