---
schema_version: "1.0.0"
document_id: "f52a62dd6e1a03bac794e0cb3b1a1f3d2ccb682f65f1970323deae85ff907bff"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-mcp-setup-blink"
published_at: "2026-06-05T00:16:26+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:346bd71b4cc04682da07287073a1ff916b7c0f87c14773b55fd4c5cafa9c1ab1"
---

# Cursor MCP Setup + Blink Plugin: Full-Stack Infrastructure in 2 Commands

## Step 1: Install the Blink Plugin


1


#### Run the install command


From any terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads 14 skills and automatically configures the MCP connection. No manual` mcp.json` editing required.


2


#### Authenticate with Blink


```text
blink   login
```


Your browser opens. Log in to your Blink account (or create one). Your API key saves automatically and the MCP connection activates.


3


#### Restart Cursor and verify


Restart Cursor. Open Settings → Features → Model Context Protocol. You'll see Blink listed with 62 tools active.


Alternatively: search "Blink" in the Cursor Marketplace and click **Add to Cursor** for a one-click install with no commands.


## What You Get: 62 MCP Tools + 14 Skills


The Blink plugin gives Cursor access to:


**Infrastructure tools (62 MCP tools):**


- Database provisioning and querying
- Auth setup (email, Google, GitHub OAuth out of the box)
- Backend function deployment
- File storage and CDN
- Environment variable management
- Custom domain assignment
- Deployment logs and monitoring


**14 skills** — structured workflows your agent follows for common tasks:


- ` build-fullstack-app` — database + auth + backend + hosting in one prompt
- ` deploy-backend` — deploy a Hono/Express/FastAPI server to Blink Cloud
- ` setup-auth` — add email and OAuth providers to any app
- ` manage-database` — create, migrate, and query a SQLite or Postgres database
- And 10 more for specific infrastructure tasks


Your agent can read the skill files directly and know exactly how to execute each step — no improvising.


## Build With Your Agent: A Practical Example


Once Blink is connected, ask Cursor:


```text
Build me a task management app with user auth and a Postgres database. Host it on Blink.


```


Cursor's agent:


1. Generates the frontend and backend code
2. Calls Blink's` provision_database` tool to create a Postgres instance
3. Calls` setup_auth` to configure email + OAuth login
4. Calls` deploy_backend` to host the API on Blink Cloud
5. Calls` deploy_frontend` to publish the app with a Blink URL
6. Returns the live URL — no DevOps required


The whole process takes under 5 minutes. No Vercel account. No Supabase account. No Auth0 account.


## Manual MCP Setup (If You Need It)


If you prefer to configure MCP manually, create or edit` ~/.cursor/mcp.json` :


```text
{
"mcpServers"  : {
"blink"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@blinkdotnew/mcp-server"  ],
"env"  : {
"BLINK_API_KEY"  :   "your-api-key-here"
}
}
}
}
```


The` npx skills add blink-new/blink-plugin` command handles this automatically. Manual setup is only needed for enterprise environments with restricted CLI access.


**Config file locations:**


- ` ~/.cursor/mcp.json` — global (available in all projects)
- ` .cursor/mcp.json` inside a project folder — project-specific only


## Cursor Marketplace (One Click)


Cursor's Marketplace lets you install MCP plugins without touching the terminal:


1. Open Cursor
2. Press` Cmd+Shift+P` (Mac) or` Ctrl+Shift+P` (Windows/Linux)
3. Type **Cursor: Open Marketplace**
4. Search **Blink**
5. Click **Add to Cursor**


Authentication completes via OAuth in your browser. The MCP connection activates immediately.


The CLI method (` npx skills add` ) installs the 14 skills in addition to the MCP tools. The Marketplace installs only the MCP tools. Use the CLI method for the full experience.


## Troubleshooting MCP Connections


If Blink doesn't appear in Cursor's MCP panel after installation:


1. **Restart Cursor** — MCP servers load on startup
2. **Check MCP logs** — open the Output panel (` Cmd+Shift+U` ), select "MCP Logs"
3. **Re-run` blink login`** — your API key may have expired
4. **Check tool count** — if you see Blink but 0 tools, the API key isn't authenticated


For detailed Cursor MCP debugging, open Settings → Features → Model Context Protocol and check the toggle next to Blink.


## Set Up Blink Cloud in Cursor — Your Full Stack in 2 Commands


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up Blink Cloud in Cursor and build me a full-stack app with auth and a database."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


MCP (Model Context Protocol) is an open standard for connecting AI coding agents to external tools. Without MCP, Cursor can only write code — it can't deploy, query databases, or provision infrastructure. MCP gives Cursor "reach" beyond your local files into services like Blink Cloud, databases, and APIs.


No. Running` npx skills add blink-new/blink-plugin` handles all the MCP configuration automatically. You don't need to find, create, or edit any` mcp.json` file. The CLI writes the config and connects your API key in one step.


Blink Cloud is a full-stack infrastructure platform — database, auth, backend, hosting, and storage in one place. The Blink Cursor plugin gives your Cursor agent 62 MCP tools to provision and manage all of it. Ask Cursor to build and deploy an app, and Blink handles the infrastructure layer automatically.


Blink Cloud has a free tier for getting started. Paid plans start at affordable monthly rates and scale with usage. See[blink.new/cloud](https://blink.new/cloud) for current pricing.


Yes. The` npx skills add blink-new/blink-plugin` command works with Claude Code and other MCP-compatible agents. Claude Code supports MCP natively. See our guide on[what Claude Code is](https://blink.new/blog/what-is-claude-code) and our[Claude Code tutorial](https://blink.new/blog/claude-code-tutorial-beginners) for more context.
