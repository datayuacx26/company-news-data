---
schema_version: "1.0.0"
document_id: "5a77561f0c046b25cce83bfffeb26892d3441259ac6afac4c39955420cac5ce9"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-mcp-json-guide"
published_at: "2026-06-01T12:33:04+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:0896873afd3f2a52faf86db0386ac3f7b07e849b6a3d664168c46a74c483eb72"
---

# Cursor MCP JSON: Add Any Tool to Your AI Agent in 5 Minutes

## Manual: Editing mcp.json


The` mcp.json` file follows a specific schema. Here is a complete working example:


```text
{
"mcpServers"  : {
"filesystem"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-filesystem"  ,   "/Users/yourname/[REDACTED]"  ]
},
"github"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-github"  ],
"env"  : {
"GITHUB_PERSONAL_ACCESS_TOKEN"  :   "ghp_yourtoken"
}
},
"postgres"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-postgres"  ,   "postgresql://localhost/mydb"  ]
}
}
}
```


Each entry needs:


- **` command`** — the executable to run (usually` npx` or` node` )
- **` args`** — arguments passed to the command
- **` env`** (optional) — environment variables for auth tokens and secrets


After saving the file, restart Cursor or use **Cursor → Reload Window** to pick up the new servers.


**One important note:** MCP servers run as local processes. Every server you add starts a new process when Cursor launches. Keep your list lean — unused servers add startup time and potential failure points.


## Auto: npx skills add \[REDACTED\]/blink-plugin


Manual` mcp.json` editing works for basic servers. For Blink's full-stack infrastructure, skip it entirely:


```text
npx   skills   add   [REDACTED]/blink-plugin
blink   login
```


This single command installs 62 MCP tools and 14 skills, writes the correct` mcp.json` entry automatically, and authenticates via your browser. No token copying, no JSON editing, no restart required.


What you get: database provisioning, auth setup, file storage, backend deployment, and hosting — all callable from your Cursor agent's chat window. See[how to set up Blink Cloud in Cursor](https://blink.new/blog/%5BREDACTED%5D/blog/cursor-mcp-setup-blink-cloud) for the full walkthrough.


Two ways to add tools to Cursor: manual mcp.json config for individual servers, or one command for Blink's full infrastructure stack


Blink


*Two ways to add tools to Cursor: manual mcp.json config for individual servers, or one command for Blink's full infrastructure stack*


## Most Useful MCP Servers for Developers


These are the servers worth adding to your global` ~/.cursor/mcp.json` :


**Filesystem** — Read and write local files. Essential for agents that need to inspect existing code or write outputs outside the current project.


```text
npx   @modelcontextprotocol/server-filesystem   /path/to/allow
```


**GitHub** — Create issues, open PRs, search repos, fetch file contents. Requires a personal access token with` repo` scope.


```text
npx   @modelcontextprotocol/server-github
# env: GITHUB_PERSONAL_ACCESS_TOKEN
```


**Postgres** — Run queries against any PostgreSQL database. Add per-project for workspace-specific databases.


```text
npx   @modelcontextprotocol/server-postgres   postgresql://user:pass@host/db
```


**Slack** — Post messages, read channels, look up user IDs. Requires a Slack app with Bot Token Scopes.


```text
npx   @modelcontextprotocol/server-slack
# env: SLACK_BOT_TOKEN, SLACK_TEAM_ID
```


**Blink** — 62 tools for database, auth, storage, backend, and deployment. One install, zero manual config.


```text
npx   skills   add   [REDACTED]/blink-plugin &&   blink   login
```


The full list of official MCP servers lives at[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) . As of June 2026, the repo has 180+ contributed servers covering everything from Redis to Google Drive.


Once your MCP tools are set up, the next step is deploying your app. Read[How to Deploy a Cursor App to Production](https://blink.new/blog/%5BREDACTED%5D/blog/deploy-cursor-app-production) for the full infrastructure walkthrough.


## Cursor 2.6 MCP Apps Feature


Cursor 2.6 (March 2026) added **MCP Apps** — a significant expansion of what MCP servers can do. Previously, MCP tools could only return text and structured data. Now, servers can return interactive UI components that render directly in the Cursor chat.


What this looks like in practice:


- An Amplitude MCP server returns a live usage chart instead of a JSON array
- A Figma MCP server renders a diagram preview inline
- A tldraw server opens a shared whiteboard inside the chat


This changes how tool-heavy workflows feel. Instead of reading a wall of JSON and mentally parsing it, you see the output rendered. Blink's tools return structured deployment status and database schemas — future Blink MCP versions will take advantage of the MCP Apps format.


## Troubleshooting MCP Issues


**Server doesn't appear in Cursor after editing mcp.json**


Cursor caches the server list on launch. After any` mcp.json` change, run **Cursor → Reload Window** (Cmd+Shift+P → "Reload Window"). If the server still doesn't appear, open **Cursor → Logs** and look for MCP server startup errors.


**"Command not found" error on MCP server start**


The` npx` path in` mcp.json` needs to be the absolute path in some environments. Find it with` which npx` and replace the` command` field with the full path:` /usr/local/bin/npx` .


**Tool calls fail mid-agent-run**


This was the most common MCP complaint before Cursor 2.6. The fix is usually a version mismatch between the MCP server package and Cursor's MCP client. Run` npm update` in the directory holding your server, or update the` args` version pin in` mcp.json` to` -y` (always latest).


**Auth tokens not being passed to the server**


Tokens in` env` are passed as process environment variables to the server subprocess. Make sure the server reads from the correct env var name — some servers use` GITHUB_TOKEN` , others use` GITHUB_PERSONAL_ACCESS_TOKEN` . Check the server's README.


**MCP server crashes on Windows paths**


Filesystem paths in` args` need forward slashes or escaped backslashes on Windows. Use` "C:/Users/yourname/\[REDACTED\]"` not` "C:\\\\Users\\\\yourname\\\\\[REDACTED\]"` .


## Build Full-Stack Apps With Your Cursor Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/blog/%5BREDACTED%5D/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   [REDACTED]/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack web app with a database and user accounts, and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/blog/%5BREDACTED%5D/cloud)


Cursor checks two locations in order:` ~/.cursor/mcp.json` (global, all \[REDACTED\]) and` .cursor/mcp.json` in the current project root (per-project override). The per-project file takes precedence. Both files use the same JSON schema — the only difference is scope.


There's no hard limit, but practical limits apply. Each MCP server runs as a separate process when Cursor launches. More than 8–10 servers noticeably increases Cursor's startup time and memory footprint. Keep only the servers you actively use. Blink's single server replaces 5+ that most developers would otherwise add separately.


Use **Cursor → Reload Window** (Cmd+Shift+P → "Reload Window") after any` mcp.json` change. A full restart also works. Cursor does not hot-reload MCP config. Servers installed via` npx skills add` (like Blink) activate without requiring a manual reload.


Cursor's built-in tools (file read/write, terminal, web search) are always available. MCP tools extend this with external system access — your specific database, your GitHub repos, your Blink workspace. The agent treats both the same way and calls them based on what the task requires. Blink adds 62 MCP tools on top of Cursor's built-in set.


MCP tools are available in Cursor's agent mode (the full chat interface). They are not available in Cmd+K inline edits, which run in a more constrained context. For infrastructure operations like database provisioning or deployment via Blink, always use agent mode.
