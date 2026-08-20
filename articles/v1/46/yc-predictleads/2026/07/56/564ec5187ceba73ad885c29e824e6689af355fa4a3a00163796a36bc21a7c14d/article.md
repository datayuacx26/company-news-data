---
schema_version: "1.0.0"
document_id: "564ec5187ceba73ad885c29e824e6689af355fa4a3a00163796a36bc21a7c14d"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://blog.predictleads.com/2026/07/16/how-to-enrich-company-records-with-mcp-a-practical-guide"
published_at: "2026-07-16T01:29:00+00:00"
first_seen_at: "2026-07-25T19:43:43.337837+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:47998953b47565d3f659565ccffb64f21779699240d4064377270ffb016e7ca3"
---

# How to Enrich Company Records with MCP: A Practical Guide

**MCP (Model Context Protocol)** lets an AI agent call a live data source directly, mid-conversation, instead of you exporting a CSV and running it through a batch enrichment job. This guide walks through how that actually works, using the[PredictLeads MCP Server](https://docs.predictleads.com/mcp_integration) as the working example.


By the end, you’ll know how to authenticate, what a request/response cycle looks like, and how to wire enrichment into an agent workflow instead of a nightly script.


How to enrich company records with MCP: traditional batch enrichment compared with live, on-demand company record enrichment through the PredictLeads MCP Server.


Table of Contents


Toggle


##


What You Need


- A PredictLeads account with an API key and API token (available on your Subscription Plans page).
- An MCP-compatible client – this can be an AI agent framework (Claude, a custom LangChain/LlamaIndex agent, an n8n or Make workflow) or any HTTP client that supports MCP’s connection model.
- A company identifier to enrich – typically a domain or company name.


##


Step 1: Authenticate


The PredictLeads MCP Server sits at` https://mcp.predictleads.com/` and supports two authentication paths, depending on how automated your setup is.


###


Fastest path: HTTP headers


For direct access or quick testing, pass your credentials as request headers:


- ` X-Api-Key: {your_api_key}`
- ` X-Api-Token: {your_api_token}`


This is the simplest way to get a working connection while you’re prototyping an agent or workflow.


###


Production path: OAuth2


For server-to-server integrations, use the Client Credentials flow: set the Access Token URL to` https://oauth.predictleads.com/token` , and use your API key as the Client ID and your API token as the Client Secret. If your client supports Dynamic Client Registration, the Authorization Code flow handles setup automatically.


##


Step 2: Understand How Enrichment Works Through MCP


Each tool exposed by the PredictLeads MCP Server maps to a specific API endpoint – company technologies, job openings, news events, financing events, and similar companies, among others – with the same parameters as the underlying API. There’s also a tool that exposes the full OpenAPI schema, so an agent can work out which tool to call and how to call it for a given request, rather than you hardcoding that logic yourself.


In practice, an enrichment flow looks like this: the agent receives a company domain, calls the relevant PredictLeads tool (or lets the model pick the right one from the OpenAPI tool), and gets back structured data it can write into a CRM field, a lead score, or a summary for a rep.


##


Step 3: A Concrete Example


Say a sales rep drops a company domain into your agent and asks, “What should I know about this account before I call them?” A typical MCP-driven flow:


- Agent calls the technologies tool for the domain – returns the company’s current tech stack.
- Agent calls the job openings tool – returns open roles, which often signal what the company is investing in.
- Agent calls the news events tool – returns recent funding, product launches, or leadership changes.
- Agent combines all three into a short account brief, in the same conversation, with no batch job in between.


That last point is the actual difference from a traditional enrichment API integration: the data is fetched on demand, in response to a specific question, instead of pre-loaded in bulk and going stale between refreshes.


##


Request Limits


Each account has a monthly request limit tied to its subscription plan, visible on your Subscription Plans page. Once you hit the limit, further requests return a` 402` HTTP error until the next billing cycle. Worth checking your plan’s limit before wiring MCP enrichment into a high-volume workflow like enriching every inbound lead automatically.


##


FAQ


**Do I need to write custom API integration code to use MCP enrichment?**


No. The point of MCP is that your agent client handles the protocol; you authenticate once and the agent calls tools directly. You still need an MCP-compatible client, but you don’t need to hand-build request/response handling for each endpoint.


**Which authentication method should I use, HTTP headers or OAuth2?**


HTTP headers (X-Api-Key and X-Api-Token) are the fastest way to get started or prototype. OAuth2, especially the Client Credentials flow, is a better fit for production server-to-server integrations.


**What happens if I exceed my monthly request limit?**


Further requests return a 402 HTTP error until your usage resets on the next billing cycle. You can track current usage on your Subscription Plans page.


**Can MCP enrichment replace a batch enrichment pipeline entirely?**


For many workflows, yes; especially ones triggered by a specific event, like a new lead or an account review. High-volume, scheduled bulk enrichment may still be better served by direct API or flat file access, depending on your request limits.


##


Related Guides


- [PredictLeads MCP Integration Docs](https://docs.predictleads.com/mcp_integration)
- [PredictLeads MCP Integration Guide](https://blog.predictleads.com/2026/03/13/predictleads-mcp-integration-guide)
- [Building a GTM Agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/09/gtm-agent-mcp-server)
- [Best Technographic Data Providers in 2026](https://blog.predictleads.com/2026/03/06/technographic-data-providers)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=mcp-company-enrichment-guide)
