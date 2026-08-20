---
schema_version: "1.0.0"
document_id: "b7a4151f55ee35483b6e5007e23ee044939f1e5d905b76f32c45b903d9293ead"
company_key: "yc-weave-3"
company: "Weave"
source_id: "yc-weave-3-news-import-b8160165addf"
canonical_url: "https://weaveos.com/blog/your-agents-are-paying-frontier-prices-to-read-tool-output"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T10:55:01.273348+00:00"
fetched_at: "2026-08-05T10:55:03.414925+00:00"
content_hash: "sha256:2248b218d5682dbea0758c36f1353819fa89e900672e725dabf4839cba5d2761"
---

# Your agents are paying frontier prices to read tool output

# Your agents are paying frontier prices to read tool output


Every turn in an agent loop asks for your best model. Most turns don't need it. Reading a tool result, classifying an intent, naming a conversation, probing a capability: the frontier model does that work flawlessly and bills you accordingly.


Weave puts each request on the cheapest model that handles it, and leaves the frontier model in place when the frontier model is the answer.


Metric


Result


Cost cut on the median request routed down


**88.5%**


Cost cut across all 151,475 routed-down requests


**68.7%**


Cost cut on our largest routed-down flow


**96.6%**


Frontier requests served below the frontier tier


**60%**


Median routing decision overhead


**9 ms**


Failover requests that still succeeded


**98.8%**


## Cheap is enough far more often than anyone expects


Same failure rates. **0.88% of small-tier requests needed failover against 0.89% of frontier-tier. Invalid tool calls: 0.00% on both.** The cheap tier is not a downgrade you absorb. On the work it gets, it is indistinguishable.


That is the whole argument for routing, and it only appears if you measure production traffic instead of benchmarks.


## Why cheap-model routing usually fails, and why Weave doesn't


Anyone can read a price list. The reason naive routing underdelivers is the prompt cache. A cached prefix belongs to one provider and one model, and it dies the moment you route elsewhere. Switch models mid-session to save on paper and you pay full freight to re-send a 150,000-token context.


**The model was cheaper. The request was not.**


Weave routes sessions, not requests. **68.6%** of requests are served by a session pin that protects the working cache. Weave computes the fresh per-request optimum on every turn and **overrides it 64.5% of the time** on pinned traffic, because holding the cache is worth more than the cheaper model.


Routing on list prices finds savings that evaporate on contact with production. Routing on realised cost finds savings that survive.


## Faster, not a tradeoff


Small models start streaming sooner. The routing decision that gets you there costs nine milliseconds against a first token that takes two seconds.


## A quality instrument, not a spend cap


Weave routes up as well as down. Our largest single routing flow is an escalation: **37,145 requests** that specified Sonnet 4.6 and were upgraded to Opus 4.8, because the task was under-provisioned for the model the client had picked.


You are not buying a discount that degrades your agents. You are buying a system that puts every request on the right model, which is usually cheaper and occasionally better.


## Reliability you get for free


Once a request can go to nine providers, provider outages stop being your problem. **98.8% of requests that required failover still succeeded** , automatically, with no retry logic in your code.


## Ready for a market that ships weekly


-


**Two days** from Claude Sonnet 5's first appearance to half its peak traffic share. Adopting it was a config change, not a migration.


-


**Seven policy versions in six weeks** , retrained around the release cadence


-


**~10 candidate models** scored per decision across nine providers


-


Classifiers, title generation and capability probes pinned to near-free models automatically


When the next frontier model ships, your agents are on it in days without touching your code.


## Getting started


Point your existing client at Weave. No SDK change, no prompt rewrite, no eval suite to re-run. Requests that need the frontier model still get it. Requests that don't, stop costing like they do.


> Weave production router telemetry, 315,533 routed requests across 19 organisations, 11 May to 27 July 2026, predominantly agentic coding traffic. Savings measured against the router's own cache-aware repricing of identical tokens at the model the client originally requested. Figures above describe routed-down and per-tier traffic; net savings across all traffic, which includes deliberate escalations, are 11.1%, and outcomes vary by configuration from −170% to +69% per organisation. Complete methodology, full distributions and the underlying dataset are published in our research primer *The Router Is a Quality Instrument* , available on request.
