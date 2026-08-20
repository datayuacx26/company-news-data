---
schema_version: "1.0.0"
document_id: "8fec9e376a8dce3a17213ca8c814e96333144a9267c3ee96c4632e5d607fde8e"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/agent-mcp"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:581415a0b98eac0f49cdce33fa4b5827df100ade76784cbf27a3c07b33ca28ba"
---

# The Airbyte MCP: One Connection, Your Entire Business Context

This is the era of MCPs. Many of us have ten or more hooked up to Claude, ChatGPT, Cursor, or whatever AI client we spend our days with. These are very convenient for everyday tasks, but they don’t always cut it when deploying agents in production, especially for data-intensive tasks.


That’s because each MCP queries a business system in isolation, leaving the agent to stitch together scattered bits of context to solve tasks. They work well for single-tool tasks, but when agents have to reason across your business data, they break.


The deeper issue: Raw APIs (and most MCPs) assume you already know what you need: specific endpoints, object IDs, exact fields.


Agents often start one step earlier, at discovery. They need to search and identify the relevant data sources, parameters, and entities before they can even start gathering the right data. And this becomes increasingly complex as a business grows and integrates more systems for the agent to query.


The Airbyte Agent MCP was built to fix all of this. One connection now gives your LLM your entire business context, unified and ready to be queried.


## **One MCP instead of fifteen**


The Airbyte Agent MCP is a[single connection](https://airbyte.com/mcp-gateway) that gives your agent access to all your business systems through something we call the[Context Store](https://airbyte.com/agentic-data/context-store) : a data index, optimized for agentic search, that unifies data from across your systems.


The MCP also offers direct read and write actions to APIs, so your agents always have a direct path to the source and can act on what they find.


The setup is simple. You connect the Agent MCP once in your client of choice, authenticate with your Airbyte account, then connect each data source through OAuth or an API key directly from the Airbyte UI.


From there, you can just start asking questions:


- "Show me all enterprise deals closing this month with open support tickets."
- "Find every customer who churned last quarter and had more than three escalations."
- "List the 10 most recent Gong calls with companies in our renewal pipeline."


These aren't single-system lookups. Each one spans your CRM, support desk, billing, and revenue intelligence at once. Without the Context Store, every question forces the agent to make several API calls and attempt to match records across different schemas. With it, the agent queries a single layer and gets the full picture in seconds.


Then you can tell it to take action. A growing number of Airbyte connectors support write operations, so your agent can update records, create tickets, and post messages back to the systems you connected.


In short, you can build AI agents in your client of choice, with the Agent MCP as your unified context layer.


## **Up to 80% lower token consumption… and why it matters**


There's a cost to the way most MCPs work that goes beyond latency and rate limits. It comes down to what happens inside the model when you flood its context window with raw API responses. In early testing, using the Context Store enabled 40% fewer tool calls and up to 80% lower token consumption compared to querying individual vendor MCPs directly.


We also compared our Agent MCP against a few vendor MCPs across five connectors: Gong, Linear, Salesforce, Slack and Zendesk. We tested retrieving, listing and searching scenarios on each.


Here's the token savings our MCP delivered across the board:


- Gong: up to 80% fewer tokens
- Zendesk: up to 90%
- Linear: up to 75%
- Salesforce: up to 16%


We’re planning to release our benchmark harness very soon and make it publicly available so you can run the tests yourself (stay tuned!).


You may be wondering why these efficiency savings matter for you. Let’s look at a practical example.


Say you ask an agent to find something in Slack. A common request, but more complicated than you would think. The Slack API technically supports search, but getting it to work depends on your permissions, token setup, and workspace configuration.


The agent has to list every channel, scroll through message histories, and pull individual threads just to find what's relevant.


But when you have Airbyte Agents set up, the Context Store already has your Slack data indexed. This means the agent can search it directly instead of reading through thousands of messages to find the relevant one.


This keeps the context window narrow. The model gets clean, structured data and has room to actually reason. So you're saving on costs, yes, but you're also getting better answers.


## **Enabling a consistent MCP experience for agents**


Every[vendor MCP](https://airbyte.com/agentic-data/mcp-servers) returns information differently and without consistent standards. They have their own limits and truncation rules, and your agent has to figure them all out on the fly.


When an agent is only querying a single tool, this is manageable. But the moment your agent needs to pull from three or four systems for one task, it's dealing with all of these inconsistencies at once. Every extra call adds tokens, adds latency, and introduces another chance for the agent to lose track of what it already retrieved.


That’s the biggest edge you get with the Airbyte Agent MCP. Everything flows from the Context Store, and your agent gets consistent, well-structured context for every task. Routing every connection through a single[MCP Gateway](https://airbyte.com/blog/mcp-gateway) is what removes the per-vendor inconsistencies that otherwise force your agent to relearn each tool's limits and truncation rules on the fly.


**Who this is for**


The Agent MCP is for anyone already using Claude, ChatGPT, or Cursor who wants to build agents that actually understand their business.


This is a product not just for the technical, but the tech-savvy. Marketers, people teams, ops, anyone with an idea and a laptop can build agents with Airbyte.


The Airbyte Agent MCP works today in Claude Desktop, Claude Code, ChatGPT (with Developer Mode), Cursor, VS Code Copilot, Codex, and any client that supports MCP. Setup takes a few minutes regardless of which client you use.


## **Where we are today**


The product is early. We're launching with 50 production-ready connectors covering the systems most central to how companies operate: Salesforce, HubSpot, Zendesk, Jira, Slack, GitHub, Stripe, Gong, Linear, and more. New connectors are shipping every week.


The Context Store is shipping as a unified layer for your business context, but there is still a ton to build, so keep an eye out for new features as they drop.


What you see today works. It's already delivering real results for the teams building on it. But there's more to build, and we want to do it in the open, alongside the community.


The Agent MCP is available today with a generous free plan. Paid plans are available as you scale your usage.


For setup instructions and the full list of supported clients, check out the[MCP documentation](https://docs.airbyte.com/ai-agents/interfaces/mcp/) .


One MCP. Your entire business context.[Try it out.](http://app.airbyte.ai/)
