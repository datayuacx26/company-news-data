---
schema_version: "1.0.0"
document_id: "175abed7810579aba573623d1596c9e145af56ecefbe88282da07eedeec8d67c"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/what-is-model-context-protocol"
published_at: "2026-04-29T00:20:32+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:55.411391+00:00"
content_hash: "sha256:24e94a12e65147db5abee0558718466cf8e5300ca9158decd13fb36cd3b5be98"
---

# What Is Model Context Protocol (MCP)? Plain-English Guide for Developers

## How MCP Works


The protocol is JSON-RPC based. An MCP server exposes a list of tools; an MCP client calls those tools and gets structured responses back.


There are two transport types:


- **stdio** — runs locally as a subprocess. Your IDE spawns the server process. Best for local tools that need direct system access.
- **HTTP/SSE** — runs remotely as a network service. Your IDE makes HTTP requests. Best for cloud services, shared team servers.


A tool definition looks like this:


```text
{
"name"  :   "query_database"  ,
"description"  :   "Run a SQL query against the production database"  ,
"inputSchema"  : {
"type"  :   "object"  ,
"properties"  : {
"query"  : {   "type"  :   "string"  ,   "description"  :   "SQL query to execute"   }
},
"required"  : [  "query"  ]
}
}
```


The AI client sees that tool definition, decides when to call it, passes the inputs, and gets back the query result. The agent never needs to know the underlying database credentials or implementation — the MCP server handles auth and execution.


When you ask your agent "what are the 10 most active users this month?", it calls` query_database` with the right SQL, gets the result, and answers you. Same session, no copy-paste, no tab-switching.


## Adding MCP Servers to Cursor


Cursor has native MCP support. Open **Settings → Features → MCP** to manage your server list.


The fastest path for most servers is the` npx` one-liner pattern. For example, to add a Postgres MCP server:


```text
npx   @modelcontextprotocol/server-postgres   postgresql://localhost/mydb
```


Cursor reads MCP configuration from` ~/.cursor/mcp.json` . A typical entry looks like:


```text
{
"mcpServers"  : {
"postgres"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-postgres"  ,   "postgresql://localhost/mydb"  ]
}
}
}
```


After saving the config, restart Cursor. The agent will see the new tools automatically. You can verify they're available in Settings → Features → MCP — the status indicator goes green when a server is connected.


For a full walkthrough, see the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup) .


## Adding MCP Servers to Claude Code


Claude Code uses the` claude mcp add` CLI command. The HTTP transport is recommended for cloud-based services:


```text
# Add GitHub MCP server (HTTP transport)
claude   mcp   add   --transport   http   github   https://api.githubcopilot.com/mcp/


# Add a local stdio server (Postgres example)
claude   mcp   add   --transport   stdio   postgres   --   npx   -y   @modelcontextprotocol/server-postgres   postgresql://localhost/mydb
```


After adding, run` /mcp` inside Claude Code to authenticate OAuth-based servers. Claude Code stores config at three scope levels:


- **Local** (` ~/.claude.json` ) — private, per-project. Use for experiments.
- **Project** (` .mcp.json` at repo root) — shared with your team via git.
- **User** (` ~/.claude.json` globally) — available across all projects.


Project-scoped` .mcp.json` is the right choice for shared tooling: your team commits it and everyone gets the same MCP servers automatically.


For the full Claude Code setup, see the[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-beginners) .


## 5 MCP Servers Every Developer Should Know


MCP: one standard protocol connects AI agents to any tool — database, GitHub, calendar, and more


Blink


**1. GitHub MCP** — repository operations, PR reviews, issue creation. One command:` claude mcp add --transport http github https://api.githubcopilot.com/mcp/` . Authentication via OAuth inside Claude Code.


**2. Postgres MCP** — direct SQL access. Your agent queries your actual schema, not a stale copy you pasted into a prompt. Works locally via stdio or against a cloud database via HTTP.


**3. Filesystem MCP** — structured file access beyond the working directory. Useful when your agent needs to read or write files outside the current project root.


**4. Playwright/Browser MCP** — browser automation. Your agent can open URLs, click through forms, extract structured data, and run end-to-end tests. Best for scraping, testing, and UI validation workflows.


**5. Blink MCP** — full-stack infrastructure in one plugin. Blink ships 14 dedicated skills and 62 total MCP tools covering database provisioning, auth, backend deployment, custom domains, and hosting. Instead of wiring Supabase + Clerk + Vercel separately, your agent provisions everything through a single MCP layer. More on this below.


For a broader picture of what agents can do with these tools, see the[agentic coding guide](https://blink.new/blog/agentic-coding-guide) .


## Connect Your AI Agent to Real Infrastructure With Blink


MCP gives your agent the ability to call external tools. Blink gives it the full-stack infrastructure to call.


Install the Blink plugin — 14 skills, 62 total tools — in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account, no separate billing dashboard.


[Learn more about Blink Cloud →](https://blink.new/cloud)


For the full list of available skills and what each one does, see the[Blink skills documentation](https://blink.new/docs/cloud/tools/skills) .


## Frequently Asked Questions


MCP (Model Context Protocol) is an open standard that lets AI agents connect to external tools and data sources — databases, GitHub, calendars, file systems — through a single consistent protocol. Think of it as a USB standard for AI: instead of every tool needing its own custom cable, one protocol works everywhere. Introduced by Anthropic in November 2024, it's now supported by OpenAI, Google, Cursor, Claude Code, and most major AI IDEs.


No. Anthropic introduced MCP, but the protocol is fully open. Cursor, VS Code, Windsurf, GitHub Copilot, OpenAI's agents, and Google's AI tools all support MCP. Any MCP server you build works across all compatible clients — that's the whole point of the standard.


A regular API requires the calling application to know exactly what endpoints exist and how to authenticate. MCP adds a discovery layer: an MCP server advertises its tools as structured definitions, so any MCP client can introspect what the server can do and call the right tool automatically. The AI model reads the tool definitions and decides when and how to call them — you don't write glue code for each integration.


Open Cursor's Settings → Features → MCP. Add server entries to` ~/.cursor/mcp.json` using the` mcpServers` key. Most community servers provide an` npx` one-liner you can paste directly into the config. After saving, restart Cursor and the tools appear in the agent's context automatically. The[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup) has a full walkthrough with real config examples.


MCP servers can expose any capability as a callable tool: SQL queries, file reads and writes, REST API calls, browser automation, code execution, secret management, and more. An MCP server is just a process that listens for tool calls and returns structured results. The Blink MCP server, for example, handles database provisioning, auth setup, backend deployment, and custom domains — 62 tools total — giving your AI agent full-stack infrastructure control from a single plugin.
