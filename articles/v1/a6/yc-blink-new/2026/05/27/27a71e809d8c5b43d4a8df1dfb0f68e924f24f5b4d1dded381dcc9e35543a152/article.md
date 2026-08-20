---
schema_version: "1.0.0"
document_id: "27a71e809d8c5b43d4a8df1dfb0f68e924f24f5b4d1dded381dcc9e35543a152"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/what-is-mcp"
published_at: "2026-05-14T00:48:02+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:55.365924+00:00"
content_hash: "sha256:369875c66aa1e21a760e13d28439564c1c480329acf21158e4b58530dea8ed0d"
---

# What Is MCP (Model Context Protocol)? A Plain-English Guide for Developers

## MCP Hosts, Clients, and Servers


Three roles. Each one distinct.


**MCP Host** — the application the user runs. Claude Desktop, Cursor, VS Code with the Continue extension, and Claude Code are all MCP hosts. The host decides which servers are trusted, configured, and running.


**MCP Client** — the component inside the host that speaks the MCP protocol. It maintains a one-to-one connection with each configured server, routes tool calls, and surfaces results back to the model. Most developers never interact with the client layer directly.


**MCP Server** — a lightweight process that exposes tools to any connected client. Servers can run locally as subprocesses or remotely over HTTP with Server-Sent Events. Anthropic maintains SDKs in 13+ programming languages: Python, TypeScript, Go, Java, Kotlin, C#, Ruby, Rust, Swift, and more.


The composability is the point. A GitHub MCP server written today works in Claude Desktop, Cursor, any custom agent you build tomorrow, and any agent built by a third party using a completely different model. The ecosystem compounds.


## MCP in Practice: Claude Code and Cursor


Claude Code ships with native MCP support. Add a server entry to` ~/.claude/claude_desktop_config.json` and the agent sees its tools in every project immediately. The[MCP tutorial for Claude Code](https://blink.new/blog/mcp-tutorial-claude-code) covers the full configuration and first tool call.


Cursor has shipped deep MCP integration across recent releases. On March 3, 2026,[Cursor 2.6](https://cursor.com/changelog/2-6) introduced MCP Apps — interactive UIs rendered directly inside agent chat. An Amplitude MCP server renders analytics charts. A Figma server displays design diagrams. A tldraw server opens a live whiteboard — all inside the conversation window, without switching tabs.


Cursor 2.6 changelog showing MCP Apps and Team Marketplaces — evidence of MCP becoming industry standard


Blink


Cursor 2.6 also shipped Team Marketplaces for Plugins. Teams and Enterprise admins can create private plugin collections, share them internally, and enforce access controls centrally. MCP has crossed the threshold from developer experiment to enterprise distribution mechanism.


The broader adoption story is equally significant. OpenAI officially adopted MCP in March 2025. Google DeepMind followed in April 2025. In December 2025, Anthropic donated MCP governance to the Linux Foundation's Agentic AI Foundation (AAIF) — co-founded with Block and OpenAI. The AAIF's MCP Dev Summit in New York in April 2026 drew approximately 1,200 attendees.


MCP is now a shared industry standard. It belongs to the ecosystem, not to Anthropic.


## Most Popular MCP Servers in 2026


The MCP ecosystem has grown to thousands of published servers. These are the ones developers reach for first.


**GitHub** — Read and write issues, pull requests, files, and commits. Anthropic ships a reference implementation. The most-used server in software development workflows by a wide margin.


**Postgres / SQLite** — Query databases in plain language. Your agent composes the SQL, the server executes it, and structured results come back. Combine with a filesystem server for full local development context.


**Filesystem** — Read and write local files with explicit directory scoping. Essential for any agent that edits code or configuration files outside the IDE's built-in editor.


**Puppeteer / Browser** — Control a headless Chromium instance. Navigate pages, extract content, fill forms, take screenshots. Useful for testing, scraping, and automation workflows.


**Slack** — Send messages, read channels, and search message history. Combine with GitHub MCP for automations like "post to #deploys when this PR merges."


**Google Drive / Docs** — Read files, search Drive, and access Sheets data. Common in enterprise workflows where documents live in Google Workspace.


**Fetch / Web Search** — Make HTTP requests or search the web in real time. Gives agents current information without hallucination from stale training data.


**Memory / Knowledge Graph** — Persist facts across conversations. Agents remember user preferences, project context, and prior decisions between sessions.


The[MCP Registry](https://modelcontextprotocol.io/registry) at modelcontextprotocol.io surfaces community-built servers organized by category. If a tool exists, an MCP server for it probably exists too.


## How to Add an MCP Server to Your AI Agent


The setup takes under five minutes for any local server.


**For Claude Code:**


1. Find or install the server package (example:` @modelcontextprotocol/server-github` )
2. Open` ~/.claude/claude_desktop_config.json`
3. Add the server under the` mcpServers` key:


```text
{
"mcpServers"  : {
"github"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-github"  ],
"env"  : {
"GITHUB_PERSONAL_ACCESS_TOKEN"  :   "ghp_your_token_here"
}
}
}
}
```


1. Restart Claude Code. The tools appear in the agent's tool list automatically.


**For Cursor:**


1. Open Settings → MCP
2. Click "Add Server"
3. Enter the server command and any required environment variables
4. Cursor installs the server and connects it on the next agent run


For remote MCP servers using HTTP transport, point the client at the server URL instead of a local command. The config shape stays the same — only the transport layer changes.


The[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup) covers connecting multiple servers simultaneously, including environment variable management and debugging common connection failures.


MCP makes AI agents composable — each tool becomes a plug-in server that any agent can use


Blink


## Build Apps With MCP-Connected Agents


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app using Blink and give it MCP tools to connect to external services."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


No. Function calling is model-specific — you define functions in the format one provider expects and they only work with that provider's API. MCP is provider-agnostic. Build one MCP server and any MCP-compatible agent uses it, regardless of which model or vendor powers it. The[Cursor vs Claude Code comparison](https://blink.new/blog/cursor-vs-claude-code) shows both tools using the same MCP servers despite running different models.


To use existing MCP servers, no. Tools like Cursor and Claude Desktop let you add servers through a config file or settings UI. To build a custom MCP server, you need basic programming skills — but Anthropic's Python and TypeScript SDKs make a working server achievable in under 100 lines of code.


MCP defines transport and protocol, not security policy. Security researchers identified prompt injection and tool-poisoning risks in April 2025. Best practices: only run servers from trusted sources, scope filesystem access to specific directories, use access tokens with minimal permissions, and review tool descriptions before trusting them in production agents.


REST APIs describe how one specific service communicates with callers. MCP is a universal protocol for AI tool use — like HTTP but purpose-built for agents. MCP standardizes how agents discover tools, call them, and handle results. A REST API requires custom integration code per caller. An MCP server works with every MCP-compatible agent immediately.


Yes. OpenAI adopted MCP in March 2025 — it works with GPT-4o and o3. Google DeepMind followed in April 2025. Cursor uses the same MCP servers across Claude, GPT-4o, and other models. MCP is model-agnostic by design. The same GitHub or Postgres server works regardless of which model your agent runs.


MCP Apps (formalized as SEP-1865 in early 2026) extends the base protocol to support interactive UIs — React dashboards, charts, forms — delivered from an MCP server directly into the host application. Before MCP Apps, servers could only return text and structured data. Now an Amplitude server can render an analytics chart inside Cursor. Cursor 2.6 (March 3, 2026) was the first major IDE to ship MCP Apps support.
