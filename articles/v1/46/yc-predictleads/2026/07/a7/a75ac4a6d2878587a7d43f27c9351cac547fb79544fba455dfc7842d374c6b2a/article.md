---
schema_version: "1.0.0"
document_id: "a75ac4a6d2878587a7d43f27c9351cac547fb79544fba455dfc7842d374c6b2a"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://blog.predictleads.com/2026/07/17/building-an-enrichment-agent-on-the-predictleads-mcp-server"
published_at: "2026-07-17T13:20:00+00:00"
first_seen_at: "2026-07-25T19:43:43.337837+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:6b6c832d027b4c96d44fa128d8e5e145260628ec36c5c54044e6b2b7bf2254b2"
---

# Building an Enrichment Agent on the PredictLeads MCP Server

This is a hands-on walkthrough for building a specific, narrow agent: one whose only job is to take a company (by domain or name) and return a structured enrichment summary – technology stack, open roles, recent news, and funding activity – pulled live from the[PredictLeads MCP Server](https://docs.predictleads.com/mcp_integration) .


If you’ve already read our guide on[building a GTM agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/09/gtm-agent-mcp-server) , this is the narrower, enrichment-specific version of that same idea.


Table of Contents


Toggle


##


What We’re Building


An agent that, given a company domain, returns a short brief covering:


- Current technology stack and recent technology changes.
- Open job postings, as a proxy for where the company is investing.
- Recent company news – launches, partnerships, leadership changes.
- Financing events, if any, and how recent they are.


The point isn’t a dashboard – it’s a single, on-demand answer a rep, analyst, or downstream workflow can request whenever they need it, instead of pulling it from a field that was last refreshed weeks ago.


Building an Enrichment Agent on the PredictLeads MCP Server: combine live technology, job openings, company news, and funding data into one structured company brief.


##


Step 1: Connect the MCP Server


Add the PredictLeads MCP server to your agent client, pointing at` https://mcp.predictleads.com/` . For a quick setup, authenticate with HTTP headers using your API key and token, available from your Subscription Plans page:


- ` X-Api-Key: {your_api_key}`
- ` X-Api-Token: {your_api_token}`


For a production deployment, use the OAuth2 Client Credentials flow instead – Access Token URL` https://oauth.predictleads.com/token` , with your API key as the Client ID and your API token as the Client Secret. Exactly how you register the server depends on your agent framework; see the[MCP integration docs](https://docs.predictleads.com/mcp_integration) for client-specific setup notes.


##


Step 2: Give the Agent a Narrow Job


Rather than exposing every PredictLeads tool with a generic “answer anything about this company” prompt, scope the agent’s instructions specifically: given a company domain, call the technology, job openings, news events, and financing events tools for that domain, and return a short structured summary combining all four. A narrow, well-scoped prompt produces more consistent output than an open-ended one, and it makes the agent’s behavior predictable enough to plug into a larger workflow.


##


Step 3: Example Run


Given a prompt like “Enrich acme-example.com,” the agent should:


- Call the technologies tool for the domain and summarize the current stack.
- Call the job openings tool and note roles that signal investment areas – engineering growth, new go-to-market hires, and so on.
- Call the news events tool and surface anything from the last 90 days.
- Call the financing events tool and note the most recent round, if any.
- Combine all four into a short brief, rather than four separate answers.


Because every call happens live, the brief reflects the company as it looks right now, not as it looked whenever a batch job last ran.


##


Step 4: Wiring It Into a Larger Workflow


Once the core enrichment agent works reliably, it’s straightforward to trigger it from other systems – a new-lead webhook, a Slack command a rep runs before a call, or a scheduled review of accounts that haven’t been looked at in a while. The agent itself doesn’t change; only what triggers it does.


##


A Note on Request Limits


Each account has a monthly request limit tied to its subscription plan. If this agent gets triggered automatically at volume – for example, on every inbound lead – check your plan’s limit first. Exceeding it returns a` 402` HTTP error until the next billing cycle.


##


FAQ


**What agent frameworks work with the PredictLeads MCP Server?**


Any MCP-compatible client, including Claude and other agent frameworks that support HTTP-based MCP connections authenticated via OAuth2 or HTTP headers.


**How is this different from the GTM agent guide?**


The GTM agent guide covers a broader outreach and account-research workflow. This tutorial builds a narrower agent focused specifically on producing a structured enrichment summary for a single company on demand.


**Should the enrichment agent call every available PredictLeads tool?**


Not necessarily. Scoping the agent to a specific, defined set of tools for a specific job produces more predictable, consistent output than giving it access to everything and an open-ended instruction.


**What happens if the enrichment agent is triggered more often than my plan allows?**


Requests beyond your monthly limit return a 402 HTTP error until the next billing cycle. Check your plan’s request limit before triggering the agent automatically at high volume.


##


Related Guides


- [PredictLeads MCP Integration Docs](https://docs.predictleads.com/mcp_integration)
- [Building a GTM Agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/09/gtm-agent-mcp-server)
- [PredictLeads MCP Integration Guide](https://blog.predictleads.com/2026/03/13/predictleads-mcp-integration-guide)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=build-enrichment-agent-predictleads-mcp)
