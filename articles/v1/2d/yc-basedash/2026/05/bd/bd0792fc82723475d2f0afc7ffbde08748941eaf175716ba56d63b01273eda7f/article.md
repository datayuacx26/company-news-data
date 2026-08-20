---
schema_version: "1.0.0"
document_id: "bd0792fc82723475d2f0afc7ffbde08748941eaf175716ba56d63b01273eda7f"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-the-basedash-mcp-server/"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:12:44.576485+00:00"
content_hash: "sha256:f3b0b11e873bb473d566ae4fda5e53360507c88bb283ab47d96e9167b0d4e4e1"
---

# Introducing the Basedash MCP server

Today we’re launching the **Basedash MCP server** — a single remote endpoint that turns Basedash into a tool any AI client can call.


Connect Claude Code, Cursor, ChatGPT, Windsurf, or any other MCP-compatible client to one URL, run through an OAuth flow, and Basedash shows up as a tool the moment you’re done. Ask data questions in plain English and get real answers — numbers, charts, reasoning — without leaving the chat or editor you already live in.


Your data analyst, inside every AI tool you ship in.


## Why we built it


The shape of how people use data has changed. A year ago the question was “how do I get to an answer faster inside my BI tool?” Today the question is “how do I get the answer inside the agent I’m already talking to?”


Engineers debugging in Cursor want feature-usage numbers without context-switching to a dashboard. Product managers in Claude want to compare cohort retention while drafting a spec. Operators in ChatGPT want last week’s revenue alongside the rest of their workflow. Every one of those moments is an analytics question — and forcing a tab switch to answer it breaks the loop.


The Basedash MCP server closes that loop. Instead of pulling people back into a BI app, it pushes the BI capability out to where they already are. The agent gets a real data analyst on call; the team gets to keep the flow they’re in.


## How the MCP server works


One URL. One OAuth flow. Two tools that go deep.


1. **Add the server** — paste` https://charts.basedash.com/api/public/mcp` into your client as a remote streamable-HTTP MCP server.
2. **Authenticate** — the first call opens an OAuth window in your browser. Sign in with your Basedash account, the client gets a scoped token, and you’re done.
3. **Ask anything** — the client can now call Basedash from inside the chat or editor. Behind the scenes, every call hits the same AI data analyst that powers Basedash chat.


```text
claude   mcp   add   basedash   --transport   http   https://charts.basedash.com/api/public/mcp
```


No API keys. No copy-pasting tokens. No proxy to maintain.


## Two tools, full coverage


We kept the surface deliberately small — and made each call go deep.


**` ask_question`** — a back-and-forth with your data analyst. Pose a question in plain English (“trial-to-paid conversion by signup source, last 12 weeks, top 3 sources”) and get a real answer. Quick lookups, deeper trends, comparisons, strategic analysis — same engine that runs Basedash chat. Generates and validates SQL behind the scenes, returns numbers, charts, and reasoning, and continues the same chat across follow-ups.


**` get_data_sources`** — what’s available before you ask. Lists every connected database, warehouse, and SaaS source in your workspace so the client knows where to point a question. Direct databases like Postgres and MySQL, warehouses like BigQuery and Snowflake, and 750+ SaaS apps via Fivetran or MCP connectors.


Small surface, full reach.


## Same permissions, every client


The most important design decision: whatever an account can see inside Basedash is exactly what flows through MCP — nothing more.


- **Workspace permissions, enforced.** Every tool call respects the same access controls as your Basedash workspace.
- **OAuth-authenticated.** Each client gets a scoped, per-user token. There are no API keys to share, store, or rotate.
- **Source-level visibility.** If your account can’t see a data source in Basedash, neither can the AI client connected to it.


That means you can roll MCP out to engineering, product, and ops without rebuilding governance from scratch. The model your team already trusts in Basedash is the same model agents inherit.


## What teams are using it for


We’ve been running the MCP server internally and with a handful of customers for the last few weeks. A few patterns keep showing up:


- **Engineers in Cursor.** Check feature adoption or error rates against the production database before shipping a change — without leaving the editor.
- **PMs in Claude.** Compare retention cohorts and pull MRR snapshots inline while drafting a spec or a roadmap doc.
- **Operators in ChatGPT.** Ask “what happened in the business this week?” and get a real, sourced answer back in the same conversation they use for everything else.
- **Founders in Claude Code.** Run a one-shot “is anything unusual today?” before standup, in the same surface they already prompt against the codebase.
- **Anyone in Windsurf or a custom client.** Same URL, same answers, same governance.


The common thread: analytics is no longer a place you go. It’s a capability that travels with whichever agent you’re already talking to.


## Getting started


The Basedash MCP server is available today for all Basedash workspaces.


1. [Sign up for Basedash](https://charts.basedash.com/signup) (or log in)
2. Connect your data sources
3. Add` https://charts.basedash.com/api/public/mcp` as a remote MCP server in your client of choice
4. Complete the OAuth flow and start asking questions


For client-specific install steps — Claude Code, Cursor, ChatGPT, Windsurf, and others — see the[MCP server feature page](https://www.basedash.com/features/mcp-server) and the[docs](https://www.basedash.com/docs/features/mcp-server) .


## What’s next


The MCP server is the second half of how we think about Basedash and the agent ecosystem.[MCP connectors](https://www.basedash.com/features/mcp-connectors) let external apps act through Basedash’s agent. The MCP server lets external agents read through Basedash’s analyst. Together, your data and the actions around it are reachable from whichever client your team prefers.


Combined with[AI chat](https://www.basedash.com/features/ai-data-analyst) inside Basedash,[Insights](https://www.basedash.com/features/insights) for proactive findings,[Automations](https://www.basedash.com/features/automations) for scheduled workflows, and the[Dashboard Agent](https://www.basedash.com/features/dashboards) for end-to-end dashboard generation, Basedash is now a complete AI-native BI platform — inside our app, or inside any AI tool you bring.


**Try the Basedash MCP server today and put your data analyst in every chat your team already uses.**
