---
schema_version: "1.0.0"
document_id: "65aadbf61b70005f8dea13b24c3684e8842dda7321616f7fb690a977de0ce944"
company_key: "yc-dot"
company: "Dot"
source_id: "yc-dot-news-import-8b97b36288bf"
canonical_url: "https://www.getdot.ai/blog/ai-made-the-semantic-layer-cheap"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T16:52:13.925276+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:405e6df1603fed3e7d5181376c8fd1d5c3e2fb40ed44d43e2ec2d14e3b60e7e6"
---

# AI Didn't Make the Semantic Layer Necessary. It Made It Cheap.

**The data industry has argued for three years about whether AI analytics needs a semantic layer. It was the wrong question all along.**


One camp says an AI analyst without a semantic layer is a liability that will confidently report wrong numbers to your board. The other camp says models are now so good you can point an agent at raw tables and skip modeling entirely.


I run an AI analytics company, so discount everything I say accordingly. But both camps are arguing about the wrong variable. The question was never whether you need a semantic layer. It was what it costs to have one. And that number just changed.


## You never strictly needed one


You can run an AI analyst without a semantic layer. We do it all the time. When we configured an agent for Adyen's DABstep benchmark, accuracy went[from 10% to 90%](https://www.getdot.ai/blog/from-10-to-90-percent-on-adyens-dabstep) , and none of that work was building a semantic layer. It was maintaining a clean data model and writing down what things mean: table descriptions, column descriptions, the business rules people usually carry in their heads.


That's what the "semantic layer or death" camp gets wrong. The layer was never the requirement. The requirement is that meaning is recorded somewhere a machine can read it. A semantic layer is one container for that. A well-documented warehouse is another.


## So why did almost nobody build one?


Because it was expensive. Every metric got defined twice: once in the transformation code, once in the layer. Keeping the two in sync was nobody's actual job, so it rotted. Consultants got hired. Projects stalled at 60% coverage. The benefit was real but moderate, the cost was high and recurring, and skipping it was a rational decision.


We wrote about the promise of[pairing AI assistants with semantic layers](https://www.getdot.ai/blog/analytics-llm-assistants-semantic-layer-easy-data) back in 2023. The honest summary of the years since: the teams that had one benefited, and most teams still didn't build one. The economics didn't work.


LLMs attacked the cost side. A model that can read your dbt project and your query history can draft the layer, flag definitions that drifted, and keep the YAML in sync as the code changes. Work that used to take a data team several quarters now takes days plus review. The benefit didn't move much. The cost collapsed. Trades flip when that happens.


That leads to a position both camps might dislike: a semantic layer in 2026 is a good purchase, not because agents made it necessary, but because agents made it affordable.


## The part that matters more: who owns it


Agents changed one more thing, and in the long run it's the bigger deal. When an agent answers hundreds of questions a day, the semantic layer stops being plumbing. It becomes the place where humans can see, and decide, what the company considers true. The more you delegate to agents, the more you need a governed, human-readable record of what the data means.


Which is why the most important question isn't which semantic layer technology you pick. It's who owns the thing. Snowflake wants your semantics in Snowflake. Databricks wants them in Unity Catalog. Every BI and AI vendor, us included, has an incentive to become the place where your definitions live.


Resist all of us.


Your definitions encode how your business works: what counts as a customer, when revenue is recognized, which orders count as churned. That's not vendor configuration. That is the business. Tools should read your context, help you refine it, and stay minimally opinionated about where it lives. The moment your meaning exists only inside one vendor's product, you've outsourced something you can't afford to outsource.


## Retire the old debate


You never needed a semantic layer to make AI analytics work. You need a maintained data model and meaning that's written down. But for the first time, having a real semantic layer costs less than the ambiguity it removes. Build it like an asset you intend to own.


Because you do.


*If this excites you, we'd love to hear from you.*[Get in touch](https://www.getdot.ai/) *.*
