---
schema_version: "1.0.0"
document_id: "a048e032ab774e46ea147ab1936c97d6057c98d4f96684ed012c7430dd17fea0"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-blink-cloud-setup"
published_at: "2026-05-23T00:31:48+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:8bed00b2726af819663a87e4730d6b0fc3384dd8a14acf0aeddd8862bf1ee62c"
---

# How to Add Blink to Cursor: Full-Stack Infrastructure in 2 Commands

## Your First Full-Stack Project With Cursor + Blink


Once connected, paste this into Cursor's Agent tab:


```text
Build a task management app with:
- User authentication (email + Google OAuth)
- PostgreSQL database with projects and tasks tables
- REST API for CRUD operations
- React frontend
Deploy everything to Blink Cloud.


```


What Cursor's agent does next:


1. Scaffolds the frontend and API routes
2. Calls Blink MCP → provisions a PostgreSQL database
3. Calls Blink MCP → configures email + Google OAuth authentication
4. Deploys the backend to Blink's serverless runtime
5. Deploys the frontend to Blink hosting
6. Returns a live URL


Steps 2–5 were manual before Blink. Now they're tool calls in the same conversation where your code was written.


Cursor agent provisioning full-stack infrastructure via Blink MCP tools


Blink


## What Blink Handles Automatically


### PostgreSQL Database


Provisioned in seconds. The agent creates tables, runs migrations, returns the connection string — no Supabase account required.


### Authentication


Email, Google, GitHub, Apple, Microsoft OAuth. Configured by your agent — no Clerk, no Auth0, no OAuth app registration.


### Backend Runtime


Serverless functions deployed to your Blink endpoint. Writes and deploys in one agent conversation.


### Hosting + Domains


Production deploy to a Blink subdomain or your custom domain. No Vercel config, no DNS wrangling.


The 14 skills give Cursor structured knowledge for each infrastructure workflow. Without skills, agents reason through every step from scratch — often missing parameters or sequencing operations incorrectly. Skills encode the right order.


## The Real Cost Comparison


5 Services Blink Cloud


Database $25/mo (Supabase) Included


Auth $25/mo (Clerk) Included


Backend $20/mo (Railway) Included


Hosting $20/mo (Vercel) Included


Storage $10/mo (S3) Included


Accounts to manage 5 1


Setup time (new project) 2–4 hours 2 commands


mcp.json work Manual, per service Auto-configured


Monthly total ~$100 From $0


No Vercel config. No Supabase account. No Auth0 or Clerk. No separate AWS S3. One platform. One bill.


## What the 14 Skills Do


Skills aren't documentation — they're context files your agent reads automatically before executing infrastructure tasks. When you ask for "Google auth," the agent reads the` blink-auth` skill and executes the correct pattern on the first try.


Key skills included:


- **blink-full-stack** — full project setup workflow
- **blink-database** — tables, migrations, queries
- **blink-auth** — all OAuth providers, protected routes
- **blink-backend** — serverless functions, API endpoints
- **blink-deploy** — preview and production deploys
- **blink-domains** — custom domains, DNS, SSL


You don't activate skills manually. Cursor reads them automatically when they're relevant.


All infrastructure tasks completed automatically by Blink MCP via Cursor


Blink


## Frequently Asked Questions


No. Running` npx skills add blink-new/blink-plugin` writes the complete MCP configuration to` ~/.cursor/mcp.json` automatically. You never open the file. After` blink login` , Cursor has everything it needs — all 62 Blink tools activate on the next restart.


The Marketplace installs the MCP server (62 tools) only.` npx skills add` installs the server plus all 14 skills — structured context that tells your agent exactly how to use each Blink feature. Use the CLI method for any serious development work. Skills eliminate agent guessing on multi-step workflows.


Yes.` npx skills add blink-new/blink-plugin` works with any MCP-compatible editor — Cursor, Claude Code, Windsurf, and Zed. Same two commands, same 14 skills, same 62 tools across all four editors.


PostgreSQL database, authentication (email, Google OAuth, GitHub OAuth), serverless backend runtime, and hosting on a Blink subdomain. No credit card required. Paid plans unlock custom domains, higher compute limits, and more usage.


Yes. Installing the Blink plugin doesn't affect existing infrastructure. Use Blink for new projects while keeping existing apps where they are. Blink's MCP tools only act when your agent explicitly calls them — they don't touch current deployments.


---


For MCP configuration details beyond Blink, see the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup-guide) . For deploying your first production app, see[how to deploy a Cursor project to production](https://blink.new/blog/deploy-cursor-project-production) .
