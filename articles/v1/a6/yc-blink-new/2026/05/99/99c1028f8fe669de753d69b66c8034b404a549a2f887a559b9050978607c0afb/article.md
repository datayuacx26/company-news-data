---
schema_version: "1.0.0"
document_id: "99c1028f8fe669de753d69b66c8034b404a549a2f887a559b9050978607c0afb"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/what-is-mcp-model-context-protocol"
published_at: "2026-05-31T00:45:28+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:1e41da36361fe10a0d78c6ee76136b8cfae61ddfebe98b3a73e39ab1bbb5cccd"
---

# What Is MCP (Model Context Protocol)? Plain-English Guide for Developers

## How MCP Works (Simplified)


MCP has three components. You don't need to understand the spec to use it — but knowing the three parts helps you reason about what's possible.


**1. The Host (your AI client)** This is Claude Code, Cursor, Claude.ai, or any other AI application you use. The host is the entity that wants to use external tools. It speaks MCP and knows how to send requests to servers.


**2. The Server (your tools and data)** An MCP server exposes capabilities to the host. A Postgres MCP server exposes` query_database` . A GitHub MCP server exposes` list_repos` ,` create_branch` ,` push_commit` . A filesystem MCP server exposes` read_file` ,` write_file` ,` list_directory` . Each capability is described in a standard schema the host reads on connect.


**3. The Protocol (the handshake)** MCP runs on JSON-RPC 2.0. When you start a session, the host asks the server: "What can you do?" The server responds with a list of named functions and their input schemas. The host passes that context to the LLM. When the LLM decides to use a tool, the host sends the call to the server, gets the result, and feeds it back into the model's context window.


How MCP works: the host, protocol, and server architecture in plain English


Blink


## Real-World MCP Examples


Here's what an MCP-connected agent can do that a plain LLM cannot:


**File system access.** The agent reads your` package.json` , understands the project structure, edits the right file, and runs` npm install` — without you copying and pasting anything. This is the core of how Claude Code and Cursor work today.


**Database queries.** Ask "which users signed up last week and haven't completed onboarding?" The agent writes the SQL, runs it against your real database, reads the results, and proposes a re-engagement sequence — all in one turn.


**Web browsing.** The agent opens a URL, reads the content, fills in a form, or extracts structured data. Useful for market research, scraping competitor pricing, or automating browser-based workflows.


**API calls.** The agent creates a GitHub issue, sends a Slack message, updates a Notion page, or charges a Stripe customer — by calling the MCP servers that wrap those APIs. No copy-pasting from the chat window.


**Code execution.** The agent writes a script, runs it in a sandboxed environment, reads the output, and fixes bugs in the same loop. This is why[Claude Code](https://blink.new/blog/what-is-claude-code) can ship working software in an afternoon.


67% of enterprise AI teams are using or actively evaluating MCP as their primary agent-tool integration layer, according to the[2026 MCP adoption report](https://agentmarketcap.ai/blog/2026/04/23/mcp-17-month-anniversary-10k-servers-97m-downloads-category-standard) .


## Best MCP Servers for Developers


The MCP ecosystem has grown to 10,000+ public servers. These eight cover the most common developer use cases:


**1. Filesystem MCP (official — Anthropic)** Read, write, list, and search local files and directories. The foundation of most local agent workflows. Ships with Claude Code by default. →[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)


**2. GitHub MCP (official — GitHub)** Manage repos, branches, PRs, issues, and code search. The agent can open a PR, review CI results, and merge — without leaving the chat. →[github.com/github/github-mcp-server](https://github.com/github/github-mcp-server)


**3. Postgres MCP** Run read/write queries against a Postgres database. Essential for any agent that needs to reason about real data. Works with any Postgres-compatible DB (Neon, Supabase, Railway). →[github.com/modelcontextprotocol/servers/tree/main/src/postgres](https://github.com/modelcontextprotocol/servers)


**4. Brave Search MCP** Web search with real-time results. Useful for research agents that need current information beyond the model's training cutoff. →[github.com/modelcontextprotocol/servers/tree/main/src/brave-search](https://github.com/modelcontextprotocol/servers)


**5. Puppeteer MCP** Full browser automation via headless Chromium. The agent can navigate URLs, click buttons, fill forms, and extract structured data from web pages. →[github.com/modelcontextprotocol/servers/tree/main/src/puppeteer](https://github.com/modelcontextprotocol/servers)


**6. Slack MCP (official — Slack)** Send messages, read channels, look up users, and manage notifications. Useful for agents that need to report back or escalate to humans. →[github.com/modelcontextprotocol/servers/tree/main/src/slack](https://github.com/modelcontextprotocol/servers)


**7. Memory MCP** Persistent knowledge graph for agents that need to remember facts across sessions. The agent stores and retrieves structured information without relying on context window size alone. →[github.com/modelcontextprotocol/servers/tree/main/src/memory](https://github.com/modelcontextprotocol/servers)


**8. Blink MCP Plugin (62 tools + 14 skills)** Full-stack infrastructure for app-building agents. Provisions database, auth, backend, storage, and hosting directly from Claude Code or Cursor. The fastest way to go from "build me an app" to a live production URL. →` npx skills add blink-new/blink-plugin` (see below)


## How to Set Up MCP With Claude Code or Cursor


### Claude Code


Claude Code supports MCP natively from the command line:


```text
# Add an MCP server (example: GitHub)
claude   mcp   add   github   npx   @github/mcp-server


# List installed servers
claude   mcp   list


# Remove a server
claude   mcp   remove   github
```


Servers persist across sessions. Once added, Claude Code knows every tool that server exposes and uses them automatically when relevant.


Alternatively, edit` ~/.claude/config.json` directly to add servers with custom configuration:


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


### Cursor


In Cursor, go to **Settings > Cursor Settings > MCP** and click **Add new MCP server** . Or edit` .cursor/mcp.json` in your project root:


```text
{
"mcpServers"  : {
"filesystem"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-filesystem"  ,   "/path/to/project"  ]
},
"github"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@github/mcp-server"  ],
"env"  : {
"GITHUB_PERSONAL_ACCESS_TOKEN"  :   "your-token-here"
}
}
}
}
```


Cursor also supports a marketplace install for popular servers — look for the MCP tab in Extensions. For a detailed setup guide, see the[Cursor vs Claude Code comparison](https://blink.new/blog/claude-code-vs-cursor-comparison) .


## Build MCP-Connected Apps With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app with MCP integration and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Installing the Blink MCP plugin adds full-stack infrastructure to Claude Code or Cursor in one command


Blink


## Frequently Asked Questions


MCP is a standard protocol that lets AI agents connect to external tools — databases, file systems, APIs, browsers — and use them directly. Without MCP, an AI can only tell you how to use a tool. With MCP, it uses the tool itself. Think of it as a USB port standard for AI: any compatible AI client connects to any compatible tool without custom integration code.


No. The MCP ecosystem already has 10,000+ public servers covering most common developer tools: GitHub, Postgres, Slack, Stripe, file systems, web browsers, and more. For most use cases, you install an existing server in a few commands. You only need to build your own if you have a proprietary internal system or a tool not yet covered by the ecosystem.


MCP servers run locally by default — they do not send your data to a third-party service. A filesystem MCP server reads your files and passes the content to the AI client; it does not store or transmit files anywhere else. For database servers, the agent makes queries and receives results; it does not have persistent storage of your database contents. That said, you control which servers you install, and you should review what each server exposes before connecting it to sensitive systems.


Claude (including Claude Code), ChatGPT, Cursor, GitHub Copilot, Windsurf, and VS Code all have native MCP support as of 2026. Anthropic, OpenAI, Google, and Microsoft are all members of the Agentic AI Foundation (AAIF) governance body that maintains the standard. If you use any of these tools, you can start adding MCP servers today.


Function calling is OpenAI's approach to letting models invoke tools defined inline in the API request. MCP is a protocol layer above that — it standardizes how tools are discovered, described, and invoked across any AI client and any tool. A model still uses function calling (or equivalent) under the hood; MCP defines the contract that makes those calls portable across clients and servers. The practical difference: MCP tools work in Cursor, Claude Code, and ChatGPT without rewriting the integration for each one.


Claude Code and Cursor use MCP to give the AI agent access to your development environment — file system, terminal, Git, package managers. Without MCP, an AI coding assistant can only suggest code. With MCP, it edits files, runs tests, fixes errors, and commits changes autonomously. MCP is what makes the difference between a code autocomplete tool and a coding agent. For a comparison of how Claude Code and Cursor use MCP differently, see our[Claude Code vs Cursor guide](https://blink.new/blog/claude-code-vs-cursor-comparison) .
