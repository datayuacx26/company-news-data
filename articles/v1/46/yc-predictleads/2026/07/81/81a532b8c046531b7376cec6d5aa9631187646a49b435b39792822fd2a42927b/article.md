---
schema_version: "1.0.0"
document_id: "81a532b8c046531b7376cec6d5aa9631187646a49b435b39792822fd2a42927b"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://blog.predictleads.com/2026/07/22/company-discovery-engine-mcp"
published_at: "2026-07-22T12:39:04+00:00"
first_seen_at: "2026-07-25T19:43:43.337837+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:5ee5f1a889d409c1eb75cbc50e07bb0abd373c59f979cc088fc6205d62eb096a"
---

# How to Build a Company Discovery Engine with the PredictLeads MCP Server

You can build a complete **company discovery engine** by connecting an AI agent to the **PredictLeads Model Context Protocol (MCP)** server and chaining a handful of discovery endpoints, so the agent finds accounts by signal, qualifies them, and hands back a list you can work the same day. Discovery is the opposite of enrichment. Enrichment looks up a company you already know. Discovery finds every company that matches a signal you care about, whether that is a technology, a role being hired, a news trigger, or a fresh funding round.


A company discovery engine in action: the AI agent connects to the PredictLeads MCP server, discovers companies by signal, qualifies them with filters, and returns a matched account list.


For a GTM team, this engine replaces the manual list building that eats the first hour of every campaign. For a marketing agency, it is something you build once and point at a new client’s ideal customer profile each week, then run as a signal-based service.


This post is the map. Each post that follows takes a single discovery endpoint and shows how to combine it with the others through the MCP server. By the end you have an agent that runs the whole workflow from one prompt.


Table of Contents


Toggle


##


What can you discover, and at what scale?


Every signal below is a live query and not a static export. The counts are what the API returns right now.


Signal Discover endpoint Live scale today Leans toward


Technology use Technology Detections 37,312 companies on Snowflake, from 54,000 tracked technologies GTM, agencies


Hiring for a role Job Openings (O*NET) 263,776 open Data Scientist roles GTM, agencies


News triggers News Events 1,238,951 partnership events GTM, market intelligence


Funding rounds Financing Events 8,101 Series B rounds GTM, VC


Firmographics Companies 239,699 US companies at 51-200 employees GTM, agencies


##


What is a company discovery engine?


It is a repeatable workflow that turns a signal into a qualified account list without manual research. You start broad with one signal, then narrow with others until what remains is a list worth a rep’s time.


The old way is to buy a static list and let it decay. The engine approach keeps the list live, because each endpoint returns the current state of the data every time you call it. A company that adopted your target technology yesterday shows up today.


##


How does the PredictLeads MCP server fit in?


The MCP server exposes each PredictLeads dataset as a tool an AI agent can call directly. You do not write integration code or manage pagination by hand. You give the agent a plain-language goal, and it decides which endpoints to call and in what order.


That is what makes this a story about one engine rather than five separate lookups. The agent holds the list in context, passes it from one endpoint to the next, and applies each filter in turn. If you have used the[PredictLeads MCP integration guide](https://docs.predictleads.com/mcp_integration) , this is the same connection, put to work on discovery instead of single-company enrichment.


##


How do the endpoints combine into one workflow?


Here is the full chain, using Snowflake as the seed. Read it as the series in miniature.


- **Start with a technology.** Find companies that adopted Snowflake recently. That is the next post.
- **Add hiring intent.** Keep only the ones hiring data roles. The post after that.
- **Add timing.** Flag the ones with a recent news trigger.
- **Add budget.** Surface the ones that just raised a round.
- **Generalize.** Define the firmographic universe and replay every signal against it.


Run it yourself, as a single MCP prompt:


```text
Using the PredictLeads MCP server, find companies that adopted Snowflake
in the last 90 days, keep only those hiring Data Scientists, then flag any
that raised a Series B this quarter. Return company name, domain, and the
signals each one matched.
```


The agent resolves that into a sequence of endpoint calls:


```text
discover_technology_technology_detections(fuzzy_name: "Snowflake", first_seen_at_from: "2026-04-23")
then, for the returned companies: company_job_openings(onet_codes: "15-2051.00")
then: company_financing_events(...) filtered to series_b
```


Authentication uses your API key and token, set as environment variables, never pasted into the prompt.


##


How do GTM teams and agencies use this differently?


A GTM team points the engine at its own ideal customer profile and wires the output into the CRM, so reps open their queue to accounts that matched three signals instead of one.


An agency runs the same engine per client. The prompt changes, the workflow does not. One client wants users of a competitor’s technology, another wants Series B companies hiring in a region. Same engine, new inputs, a fresh deliverable each time. Because the data is live, the list you hand over is current on the day you send it, not the day you built it.


##


Frequently Asked Questions


**What is the difference between company discovery and enrichment?**


Enrichment adds detail to a company you already have. Discovery finds companies you do not yet know by matching a signal such as a technology, a role, a news event, or a funding round.


**Do I need to write code to use the PredictLeads MCP server?**


No. The MCP server exposes each dataset as a tool your AI agent calls directly. You describe the goal in plain language and the agent handles the endpoint calls.


**Which signals can I discover companies by?**


Technology use, hiring by role using O*NET codes, news events by category, funding rounds by type, and firmographics such as location and size. Each is a separate discovery endpoint you can combine with the others.


**Is the data current when I query it?**


Yes. Each call returns the present state of the dataset, so a company that matched your signal today appears today rather than in a stale export.


##


Related Guides


- [Why MCP Is Becoming the Standard for AI-Driven Company Enrichment](https://blog.predictleads.com/2026/07/10/why-mcp-is-becoming-the-standard-for-ai-driven-company-enrichment)
- [MCP for Company Enrichment: Connecting AI Agents to Real-Time Company Data](https://blog.predictleads.com/2026/07/13/mcp-for-company-enrichment-connecting-ai-agents-to-real-time-company-data)
- [Build a GTM Agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/09/gtm-agent-mcp-server)
- [Building an Enrichment Agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/17/building-an-enrichment-agent-on-the-predictleads-mcp-server)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=company-discovery-engine-mcp)
