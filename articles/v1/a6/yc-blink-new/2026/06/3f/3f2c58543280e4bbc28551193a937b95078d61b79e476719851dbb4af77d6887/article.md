---
schema_version: "1.0.0"
document_id: "3f2c58543280e4bbc28551193a937b95078d61b79e476719851dbb4af77d6887"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-mcp-blink-cloud-setup"
published_at: "2026-06-02T00:28:11+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:20be24ccd7d1f256b752b0ef88dd4fd80d216e08ecb119f3f614da1ef22b1a80"
---

# Cursor MCP Setup: Add Full-Stack Infrastructure in 2 Minutes

## What Blink Cloud Gives You


Blink provides 62 MCP tools and 14 skills that cover the full stack: database provisioning, authentication setup, file storage, backend deployment, and hosting — all accessible to your Cursor agent without writing a line of config.


No manual` mcp.json` editing required.


The 14 skills are specialized instruction sets that teach your agent exactly how to use each tool. Your agent doesn't just have access to an API — it understands how to provision a production database schema, set up auth flows, and deploy a complete app in a single conversation.


The difference: instead of your agent knowing it can call a` create_table` function, it knows to create a table *and* set the right indexes for your use case *and* configure row-level security *and* update your app's environment variables to point at the new database.


No separate Supabase account. No Vercel dashboard. No Auth0 configuration.


## Setting Up in 2 Minutes


This is the actual flow:


**Step 1: Install the Blink plugin**


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads all 14 skills and automatically configures MCP in Cursor. The CLI writes the` mcp.json` entry for you. No JSON editing.


**Step 2: Authenticate**


```text
blink   login
```


Your browser opens. You log in or create a free Blink account. The API key is saved automatically. Your MCP connection is live.


**Step 3: Ask your agent to build**


Open Cursor and type:


> "Build me a full-stack app using Blink and host it on Blink."


Your agent now has 62 tools to work with. It provisions the database, sets up auth, deploys the backend, and returns a live URL.


Deploying with Blink — 14 skills installed, full-stack infrastructure ready in one command


Blink


## What Cursor Can Build Once MCP Is Connected


With Blink's 62 MCP tools available, your Cursor agent can:


- Create and manage a Postgres database — schema, migrations, queries, row-level security
- Configure user authentication — signup, login, password reset, social auth providers
- Deploy a backend API — write the routes, deploy to Blink's infrastructure
- Handle file storage — uploads, access control, CDN delivery for public assets
- Set custom domains — your app ships to a real URL, not` localhost:3000`


Before Blink: your agent writes a Next.js app with a` TODO: add database` comment. After Blink: the agent provisions the database, configures auth, deploys the backend, and hands you a URL.


The before/after is concrete. "8 services, 3 hours" becomes "1 platform, 2 commands, one bill."


## Alternative: One Click via the Cursor Marketplace


If you'd rather skip the terminal entirely, search for "Blink" in the Cursor Marketplace. Click install. Authenticate with OAuth. The same 62 tools are available in your agent.


Both paths end up in the same place. The` npx skills add` route is faster if you're already in a terminal and want to build immediately.


Cursor agent provisioning database, auth, and hosting via Blink's 62 MCP tools


Blink


## Set Up Blink Cloud in Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up Blink Cloud in Cursor and build a full-stack app."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


No.` npx skills add blink-new/blink-plugin` handles the Cursor config automatically. The CLI writes the MCP entry for you. If you install via the Cursor Marketplace, OAuth handles everything. You should never need to touch` mcp.json` for the Blink integration.


MCP tools are functions your agent can call — like` create_database` or` deploy_backend` . Skills are instruction sets that teach your agent how to use those tools correctly in context. Blink ships 62 tools and 14 skills together, so your agent understands not just that it can call a tool, but when and how to use it for your specific use case.


Yes. The same` npx skills add blink-new/blink-plugin` command works with Claude Code. See our[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-beginners) for the full walkthrough with Claude Code specifically.


A red dot means Cursor can't start the server. Run` blink login` again to confirm your auth is valid. Open the MCP Logs panel (Cmd+Shift+U → select "MCP Logs") for the specific error. If the issue persists, remove and re-add the server via the Cursor Marketplace to reset the config.


Yes. You can install the plugin and start building on a free Blink account. See[blink.new/cloud](https://blink.new/cloud) for tier details and what's included at each plan.
