---
schema_version: "1.0.0"
document_id: "344aa79faf586444f2c2e37d7f8c7bc512d15b7608de651485378e872a6df3d9"
company_key: "yc-cloudchipr"
company: "Cloudchipr"
source_id: "yc-cloudchipr-news-import-6f8352fb5c78"
canonical_url: "https://cloudchipr.com/blog/cloudchipr-mcp-server-announcement"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-25T00:15:01.414496+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:21edcc0fb38213e514f184545b43cb8a741f23ef6cc01cfda7d8c4c6ba9dc502"
---

# Cloudchipr Now Works With Your AI Assistant: Query Your Cloud Costs in Plain English

We're launching the[Cloudchipr MCP server](https://cloudchipr.com/mcp) , a Model Context Protocol integration that connects your AI assistant directly to Cloudchipr's cloud cost intelligence. In Claude, Claude Code, Codex, Cursor, or any MCP-compatible assistant, you can now ask about cloud spend, waste, rightsizing, and Kubernetes idle costs in plain English. No dashboards, no pivot tables, no waiting on a report.


## **The Problem: Cost Investigations Are Too Slow**


The bill lands 22% over forecast. Two engineers lose three days digging through Cost Explorer and spreadsheets, only to find a forgotten GPU instance that idled for seven weeks. Over $6,000 for nothing.


The workflow is the problem: cost data lives in dashboards, but engineers now work in AI environments like Claude Code, Codex, and Cursor. MCP connects the two. The real question is what you put behind the endpoint.


## **Any AI Can Read a Bill. Cloudchipr Understands It.**


MCP is just the pipe. What matters is what's on the other end of it.


Cloudchipr isn't a thin wrapper over billing APIs. It's a platform that has already normalized cost data across AWS, GCP, Azure, Kubernetes, DigitalOcean, Datadog, OpenAI, Anthropic, and more, so your assistant queries one unified model instead of a dozen inconsistent schemas. Through the MCP server, you get:


- **A multi-cloud billing explorer** with validated dimensions and exact filter values, so queries are accurate, not guessed
- **Live resource intelligence** pairing cost with real CPU, memory, and network metrics
- **A savings opportunity engine** with rightsizing, termination, and off-hours recommendations, each rated by implementation effort
- **Kubernetes cost breakdown** into total, idle, and reserved cost per cluster
- **Team-scoped views and custom sources** for any billing data you bring in


That's the difference between an MCP that gives you numbers and one that gives you answers.


## **What You Can Ask**


Here's what real questions look like in practice, the same whether you're typing them into Claude, Codex, Cursor, or an automated agent.


**1. Org-Wide Savings Summary**


*"What's our total optimization opportunity right now?"*


Your assistant returns current spend on flagged resources, projected spend after applying recommendations, and the delta. It's the headline number for any planning conversation.


**2. Savings Opportunities by Type, Account, or Tag**


*"Show me all underutilized VMs in our Azure subscriptions with more than $100 per month in potential savings."*


Filter opportunities by provider, region, service, instance type, tag, savings threshold, or effort. Each comes with a specific recommendation: terminate, rightsize, or schedule off-hours. Example: a VM at under 20% CPU for 7 days, $402 per month in savings, low effort.


**3. Billing Cost Breakdown**


*"Break down our AWS spend by service for the last 30 days."*


Your assistant queries the normalized billing explorer using validated dimensions and exact service names, so queries are never guessed. Covers AWS, GCP, Azure, Kubernetes, Datadog, OpenAI, Anthropic, GitHub, and more.


**4. Connected Accounts Overview**


*"Which accounts are connected and what's our utilization picture?"*


See every connected account with its underutilized and saved costs, so the biggest waste contributor is obvious without opening a dashboard.


**5. Kubernetes Cost and Utilization**


*"What's the idle cost on our production cluster?"*


Per-cluster cost split into total, idle, and reserved, plus CPU and memory utilization. One real cluster: $2,197 per month total, $1,357 idle. 62% of spend on unused capacity.


**6. Live Resource Inspection**


*"Is that RDS instance in us-east-1 actually being used?"*


Look up any running resource with its real-time metrics. The link between a cost anomaly and what the resource is actually doing.


**7. Tag-Based Cost Attribution**


*"What's the total optimization opportunity for everything tagged Env=Prod?"*


Filter opportunities and resources by tag. A team owns their tags, so they own their savings view.


## **Before and After**


**Before:** billing alert, Slack thread, CSV exports, a JIRA ticket, and a fix that lands days later, if ever.


**After:** an engineer already in their coding assistant types *"What drove the cost increase this week, and what's actionable with low effort?"* In one conversation, they get the spike's source plus three underutilized VMs and an orphaned load balancer, $437/month in savings, with console links to act on immediately.


The same tools work programmatically, so the agents you build for cost triage, weekly savings reports, or rightsizing pipelines use the exact same integration. No separate API, no vendor lock-in.


## **Get Started**


You'll need a Cloudchipr account with a connected cloud provider (https://app.cloudchipr.com/) and any MCP-compatible assistant. Connect in one of two ways:


- **OAuth** for interactive use: add the Cloudchipr MCP server in your assistant's settings and sign in. No keys, revocable anytime.
- **API token** for automation, CI, and config-file setups: generate a token in Cloudchipr settings and add it to your MCP configuration.


MCP Server:[https://mcp.cloudchipr.com/mcp](https://mcp.cloudchipr.com/mcp)
Full docs and per-assistant setup guides:[https://docs.cloudchipr.com/docs/cloudchipr-mcp-server](https://docs.cloudchipr.com/docs/cloudchipr-mcp-server)
Want to see it in action first?[Book a demo](https://cloudchipr.com/book-a-demo)
