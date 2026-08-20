---
schema_version: "1.0.0"
document_id: "b5d3388fb9277ba792a7e0657a3b196df970b13ed3215b9215dc2a4d4f95485f"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-set-up-blink-cloud-cursor"
published_at: "2026-05-12T00:20:00+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:07.653203+00:00"
content_hash: "sha256:c43b977298e8251421d54dc3753562ce6d3640ef468f3786882e7e19aea7b87d"
---

# How to Set Up Blink Cloud in Cursor: 2-Minute Guide

## Install Blink Cloud in Cursor


1


#### Run the install command


Open your terminal and run:


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads all[14 skills](https://blink.new/docs/cloud/tools/skills) and writes the correct MCP server configuration to` ~/.cursor/mcp.json` automatically. The entire install takes under 30 seconds. You don't need to open or edit any config file yourself.


2


#### Authenticate with Blink


Run the login command:


```text
blink   login
```


Your browser opens to Blink's auth page. Sign in or create a free account. Your API key saves to your local config automatically — no copy-pasting credentials, no environment variables to set by hand.


3


#### Restart Cursor


Close and reopen Cursor completely. The MCP connection loads at startup. To verify it's working, open the Agent panel and click the tools icon — you should see Blink tools listed there.


4


#### Ask your agent to build


Switch to Cursor's Agent tab and try this prompt:


> "Build me a full-stack app with user auth and a Postgres database, and host it on Blink."


Cursor provisions a real database, configures real auth, scaffolds the backend, and deploys to a live URL — all from inside the chat window.


You can also install Blink directly from the Cursor Marketplace. Open Cursor → Settings → MCP → Browse Marketplace → search "Blink" → Add to Cursor. The` npx skills add` path installs the full 14-skill library on top, giving your agent richer task-level workflows.


## What Happens When Your Agent Builds


Once the Blink MCP connection is live, Cursor's agent gains access to every layer of your infrastructure. A typical "build me a full-stack app" request triggers this sequence automatically:


1. Agent calls Blink's` create_database` tool — Postgres instance provisioned
2. Agent runs migrations through` run_migration` — schema created
3. Agent calls` setup_auth` — email + social login configured
4. Agent deploys the frontend via` deploy_frontend` — live URL returned
5. Agent wires environment variables — connection strings injected automatically


You see each tool call in the chat window as it happens. No switching between browser tabs. No pasting API keys. No debugging connection strings.


Cursor agent connecting to database auth and hosting through Blink MCP tools


Blink


## The Full Toolset: 62 MCP Tools Across 5 Categories


Category What Your Agent Can Do


**Database** Provision Postgres, run migrations, create tables, query data


**Auth** Set up email + social login, manage users and sessions, configure API keys


**File storage** Upload, download, manage files, link to user records


**Backend** Deploy Node/Python/Ruby servers, scale instances, manage environment variables


**Hosting** Deploy frontends, configure custom domains, manage HTTPS


You never call these tools directly. Describe what you want, and the agent picks the right tools and chains them together.


## Before vs. After: Infrastructure Setup Without Blink


Here's the real difference in setup time and complexity:


Without Blink With Blink


**Accounts needed** Vercel + Supabase + Auth0 + S3 + Redis + billing = 6+ 1 (Blink)


**Setup time** 2–4 hours of configuration 2 minutes


**MCP tools available** 0 (none configured by default) 62 (automatic)


**Skills available** 0 14


**Agent can deploy** No — writes code only Yes — full stack


**Monthly cost** $50–130+ across 6 services One platform


The infrastructure gap is the reason most Cursor projects stall at the "it works locally" stage. Blink closes that gap in two commands.


For a detailed look at how Cursor compares to other AI coding tools, see[best AI coding agents in 2026](https://blink.new/blog/best-ai-coding-agents-2026) . If you want to go deeper on MCP configuration, the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup-guide) covers the full manual path and other popular servers.


## Also Available: One-Click from Cursor Marketplace


If you prefer not to use the terminal, Blink is available in the Cursor Marketplace with one click.


Open Cursor → Settings → Features → MCP → Browse → search "Blink" → Add to Cursor. The Marketplace install configures the MCP connection automatically. Run` npx skills add blink-new/blink-plugin` afterward to add the 14-skill library. Both paths end at` blink login` .


Getting started with Blink Cloud in Cursor — database auth and hosting in two commands


Blink


## Get Started: Add Full-Stack Infrastructure to Cursor


Install the Blink plugin in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Your browser opens, you sign in, and Cursor gains access to 62 MCP tools and 14 skills — database, auth, storage, backend, and deployment all in one platform.


Then ask your agent: *"Build me a full-stack app using Blink and host it on Blink."*


Add full-stack infrastructure to your coding agent in one command:` npx skills add blink-new/blink-plugin` →[blink.new/cloud](https://blink.new/cloud)


Try Blink free — ship your first app today


Describe what you want to build. Get a working app with database, auth, and hosting in minutes.


[Start free](https://blink.new/)


No. The` npx skills add blink-new/blink-plugin` command writes the correct MCP configuration to` ~/.cursor/mcp.json` automatically. You never need to open or edit the file yourself. The plugin handles server registration, API key storage, and tool activation in a single install step.


Skills are high-level task workflows — "set up user auth," "provision a Postgres database," "deploy a backend to Blink." MCP tools are the lower-level primitives those skills use: create a table, run a migration, add an environment variable, check deployment status. You interact with skills by describing what you want to build. The agent uses MCP tools to carry out each step.


Yes. The Blink plugin works with any MCP-compatible coding agent — Cursor, Claude Code, and Windsurf all support the same protocol. The install command is identical:` npx skills add blink-new/blink-plugin` . See[how to deploy what Claude Code builds](https://blink.new/blog/how-to-deploy-what-claude-code-builds) for Claude Code-specific guidance.


Blink opens a browser window for authentication. After you sign in, your API key saves to your local config automatically. Restart Cursor and the Blink tools appear in the Agent panel's tool list within seconds. No manual key copying, no environment file editing.


Yes. Blink Cloud has a free tier — no credit card required to get started. You can run the full setup described in this guide and have a connected development environment at $0. Paid plans unlock higher usage limits, production-scale infrastructure, and custom domains.
