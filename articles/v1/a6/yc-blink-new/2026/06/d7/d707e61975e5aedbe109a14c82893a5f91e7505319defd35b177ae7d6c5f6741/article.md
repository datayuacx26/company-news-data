---
schema_version: "1.0.0"
document_id: "d707e61975e5aedbe109a14c82893a5f91e7505319defd35b177ae7d6c5f6741"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-mcp-setup"
published_at: "2026-06-11T00:16:26+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:08be732a277c7feacd3ef473d6e076c9b88cb36d2addf7713e5c7d96b2e6765d"
---

# Cursor MCP Setup Guide: Connect Your Agent to Real Infrastructure

## Two Ways to Add MCP to Cursor


Getting MCP running in Cursor takes two very different paths.


Manual mcp.json editing vs npx skills add blink-new/blink-plugin — one command vs 30 minutes of config


Blink


**The manual path:** Open` ~/.cursor/mcp.json` , create the` mcpServers` block, add a server entry with the right` command` ,` args` , and` env` values, save, and fully restart Cursor. Most developers spend 20–30 minutes debugging JSON formatting and environment variable issues on their first attempt.


Here is what a typical manual entry looks like:


```text
{
"mcpServers"  : {
"my-server"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "some-mcp-server"  ],
"env"  : {
"API_KEY"  :   "${env:MY_API_KEY}"
}
}
}
}
```


**The one-command path:** Run` npx skills add blink-new/blink-plugin` . The CLI writes the correct JSON to` mcp.json` , downloads the server, handles credentials, and configures Cursor — no manual file editing at any step.[Blink Cloud](https://blink.new/cloud) connects in under two minutes.


Use the manual path for custom or self-hosted servers. Use the one-command path for everything else.


## Install the Blink Plugin in Cursor


This installs 62 tools and 14 skills in three steps.


1


#### Install the plugin


Run this in your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


The installer downloads[14 skills](https://blink.new/docs/cloud/tools/skills) from the Blink plugin registry and writes the correct MCP server entry to` ~/.cursor/mcp.json` . No manual file editing required at any step.


2


#### Authenticate


```text
blink   login
```


A browser window opens. Sign in with your Blink account — or create a free one. Your API key saves automatically to your local config. The MCP connection goes live immediately.


3


#### Ask your agent to build


Open a new Agent chat in Cursor and type:


> "Build me a full-stack app using Blink and host it on Blink."


Cursor now has 62 tools available — database provisioning, auth setup, backend functions, and deploy. It executes them in sequence and returns a live URL.


**Alternative:** Search "Blink" in the[Cursor Marketplace](https://www.cursor.com/en/marketplace) for a one-click install. Same 62 tools, no terminal required.


## What You Get: 62 Tools, 14 Skills


The Blink plugin gives Cursor access to a full production stack. No separate Vercel account. No Supabase account. No Auth0.


The 62 MCP tools cover five categories:


- **Database** — provision Postgres tables, run queries, execute migrations, seed test data
- **Auth** — user management, OAuth flows, session tokens, protected routes
- **Backend** — serverless functions, API endpoints, webhook handlers
- **Deploy** — push to production, manage custom subdomains, roll back releases
- **Storage** — file uploads, asset management, CDN delivery


The 14 skills go further. They're pre-built agentic workflows that chain these tools together for complete scenarios — "set up auth", "create a database schema", "deploy with a custom domain". One prompt triggers the entire multi-tool sequence. Cursor executes it without step-by-step guidance from you.


The plugin works identically in Claude Code, Windsurf, and Zed. Install once, use everywhere.


## Before MCP vs After MCP


The practical difference between manually provisioning a full-stack app and using Blink's MCP is significant.


**Before Blink MCP:**


- 8+ services to configure — Vercel, Supabase, Auth0, S3, and more
- 3+ hours of setup spread across multiple dashboards and documentation sites
- 5+ accounts to manage and billing to track separately
- Manual credential wiring between every service integration


**After Blink MCP:**


- 1 platform, 2 commands (` npx skills add` +` blink login` )
- Agent provisions the full stack inside the Agent chat window
- One bill, one account, zero cross-service credential management
- From zero to production URL in a single conversation


No separate Vercel. No Supabase. No Auth0. The agent handles the infrastructure the same way it handles the code.


## Common MCP Setup Mistakes


A few errors appear repeatedly in Cursor's community forums and each one costs developers an hour to diagnose.


**Forgetting` -y` with npx.** If you use` npx` in a manual server config without the` -y` flag, the process hangs indefinitely. It waits for package install confirmation — but there is no terminal to respond. Always write` npx -y package-name` in` mcp.json` .


**Hardcoding API keys.** Put credentials directly in` mcp.json` and they end up in source control. Use` ${env:VARIABLE_NAME}` interpolation instead — Cursor resolves environment variables at startup from your shell profile.


**Missing the` mcpServers` root key.** If you omit` mcpServers` from the JSON structure, Cursor silently ignores the entire file. No error. No warning. Zero tools loaded. The file must start with` { "mcpServers": { ... } }` .


**Not restarting Cursor fully.** MCP configuration only loads at startup. Changes to` mcp.json` require closing Cursor entirely and reopening it — toggling a window is not enough.


**Debug shortcut:** Open the Output panel (` Cmd/Ctrl + Shift + U` ), select "MCP Logs" from the dropdown. Connection errors, auth failures, and server crashes all appear here.


Blink MCP: ask Cursor to build a full-stack app and it provisions database, auth, backend, and hosting automatically


Blink


## Set Up Blink Cloud in Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up Blink Cloud in Cursor and build a full-stack app with authentication."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Model Context Protocol (MCP) is an open standard that lets Cursor's AI agent connect to external tools — databases, deployment platforms, auth services, GitHub, and more. When an MCP server is configured, Cursor calls its tools directly from the Agent panel, turning code generation into real infrastructure actions. Anthropic released MCP in November 2024; it became a vendor-neutral standard under the Linux Foundation in December 2025. Configure servers in` ~/.cursor/mcp.json` for global access or` .cursor/mcp.json` for project-specific setup.


Not for officially packaged plugins. Running` npx skills add blink-new/blink-plugin` writes the correct JSON to` ~/.cursor/mcp.json` automatically — no manual editing, no formatting errors. The Cursor Marketplace handles configuration for any plugin listed there via one-click install. Manual` mcp.json` editing is only necessary for custom or self-hosted MCP servers not available through those channels. When you do edit manually, use` ${env:VARIABLE_NAME}` syntax for secrets rather than hardcoding API key values in the file.


The Blink plugin adds 62 MCP tools across five categories: database (provision Postgres, run queries, execute migrations), auth (user management, OAuth flows, session tokens), backend (serverless functions, API endpoints, webhooks), deploy (push to production, custom subdomains, rollbacks), and storage (file uploads, asset management). It also installs 14 agentic skills — pre-built workflows that chain tools together for full-stack scenarios. The plugin works identically in Cursor, Claude Code, Windsurf, and any MCP-compatible editor.


An MCP tool is a single callable function — "create a database table" or "deploy to production". A skill is a higher-level agentic workflow that chains multiple tools together to complete a task. The Blink plugin installs both: 62 MCP tools (individual actions) and 14 skills (pre-built multi-step workflows). Skills run inside the Cursor Agent loop — describe the outcome and the skill executes the required tool sequence automatically without step-by-step prompting.


Yes. All servers listed in your` mcpServers` object activate simultaneously. Cursor loads all tool definitions on startup. There is no hard server limit, but each server's tools consume context window space — tool selection quality degrades above roughly 40 active tools. A practical stack: Blink (infrastructure, 62 tools) plus GitHub (source control) covers most full-stack development workflows within the efficient range. Disable unused tools individually via Settings → Features → Model Context Protocol.


Three most common causes: the server failed to start (check MCP Logs in the Output panel), Cursor was not fully restarted after changing` mcp.json` (close and reopen entirely), or the` mcpServers` root key is missing from the JSON file (Cursor silently ignores malformed config). For` npx` -based servers, run the exact command from` mcp.json` in your terminal to surface any errors Cursor is swallowing. Run` blink login` again if you see Blink authentication errors in MCP Logs.
