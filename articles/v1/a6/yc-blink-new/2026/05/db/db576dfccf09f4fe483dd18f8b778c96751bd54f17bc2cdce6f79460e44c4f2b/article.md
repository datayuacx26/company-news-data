---
schema_version: "1.0.0"
document_id: "db576dfccf09f4fe483dd18f8b778c96751bd54f17bc2cdce6f79460e44c4f2b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/vibe-coding-cloud-deploy-cursor-claude-code"
published_at: "2026-05-04T12:19:22+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:46.128682+00:00"
content_hash: "sha256:e6718a343d78a22c873d8e58e9b711c5edeb6de19777b45bf2517566d11c7624"
---

# How to Deploy a Cursor Project to Production (Database, Auth, and Hosting in 2 Minutes)

## The Old Way: 6 Accounts, 3 Hours, $130/Month


The traditional deployment stack for an AI-built app looks like this:


Service What it covers Monthly cost


Vercel Pro Frontend hosting $20


Supabase Pro Postgres database $25 + overages


Auth0 or Clerk Authentication $25–35


AWS S3 File storage $5–20


Railway or Render Backend hosting $10–20


Redis Cloud Caching and sessions $10–20


**Total** **$95–140/mo**


Each service requires its own account, API keys, environment variables, and configuration. Connecting them together — getting your Supabase Postgres URL into Vercel's environment variables, configuring Auth0 callback URLs for your Vercel domain, wiring S3 credentials into your Railway backend — is where most developers spend 3–4 hours before writing a single line of product logic.


[State of vibe coding in 2026](https://afterbuildlabs.com/resources/state-of-vibe-coding-2026) reports that 61% of developers who build with AI coding tools abandon their project at the deployment stage. Not because the code is bad. Because the infrastructure is overwhelming.


## The Blink Way: 2 Commands, Everything Included


[Blink Cloud](https://blink.new/cloud) replaces the six-service stack with one platform. Database, auth, file storage, backend hosting, and frontend hosting — one provider, one bill, one API key.


More importantly: your AI agent can set it all up for you.


When you install the Blink plugin, Cursor and Claude Code gain 62 MCP tools and[14 skills](https://blink.new/docs/cloud/tools/skills) that let them interact with Blink Cloud directly. The agent doesn't just write code that references a database — it provisions the actual database, runs the migrations, and deploys the app.


Here's the comparison:


Old way Blink way


**Services needed** 6 1


**Accounts to create** 6 1


**Setup time** 3–4 hours 2 minutes


**Monthly cost** $95–140 One platform


**Agent can deploy** No Yes


Cursor writes the code, Blink deploys it — the complete vibe coding production pipeline


Blink


*The vibe coding pipeline: AI writes the code, Blink Cloud deploys it — no DevOps in between*


## Deploy Your AI-Built App: Step-by-Step


### Step 1: Install the Blink plugin


In your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


This installs[14 skills](https://blink.new/docs/cloud/tools/skills) and configures 62 MCP tools in Cursor (or Claude Code). No` mcp.json` editing required.


### Step 2: Authenticate


```text
blink   login
```


A browser window opens. Sign in. Your API key saves automatically. The MCP server connects.


### Step 3: Ask your agent to deploy


In Cursor's Agent tab or Claude Code, give your agent the deployment command:


```text
Deploy this app on Blink — provision the database, set up auth with email and Google login,
and host the frontend. Use the existing schema in /prisma/schema.prisma.
```


The agent uses Blink's MCP tools to:


- Provision a Postgres database and run your migrations
- Set up auth with your specified login providers
- Configure environment variables in Blink's secrets manager
- Deploy your backend to Blink Cloud
- Deploy your frontend with HTTPS and a Blink subdomain


### Step 4: Add a custom domain (optional)


Once deployed, ask your agent:


```text
Add my custom domain app.myproject.com to this Blink deployment.
```


The agent configures DNS instructions and Blink handles TLS certificate provisioning automatically.


## What Gets Provisioned Automatically


When your agent runs a Blink deployment, here's what gets created without any manual configuration:


**Database layer**


- Postgres database with your schema applied
- Connection string added to environment secrets
- Automatic daily backups


**Auth layer**


- User table with session management
- Email/password auth with secure hashing
- OAuth providers (Google, GitHub, etc.) based on your agent's instructions
- Password reset and email verification flows


**Storage layer**


- File storage bucket for user uploads
- Signed URL generation for private files
- Storage permissions linked to your user table


**Hosting layer**


- Backend server running on Blink Cloud infrastructure
- Frontend served from CDN with HTTPS
- Environment variables injected securely at runtime


## For Cursor vs. Claude Code Users


Blink Cloud works identically with both tools. The MCP protocol is the same. The skills are the same. The install command is the same.


If you're choosing between Cursor and Claude Code, see[Cursor vs Claude Code: Which Should You Pick in 2026?](https://blink.new/blog/cursor-vs-claude-code) for a detailed comparison. Either way, Blink connects as your deployment layer.


For Cursor users already familiar with MCP setup, see the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup-guide) for the full MCP configuration reference.


For Claude Code users: the same` npx skills add blink-new/blink-plugin` command works. Claude Code supports MCP natively. Run it from your project root and Claude Code will detect the Blink tools automatically.


For broader context on why this deployment gap exists, see[what is agentic coding](https://blink.new/blog/what-is-agentic-coding) — the shift from copilot tools to autonomous agents is why infrastructure integration matters more than ever.


## Deploy Your AI-Built App on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Deploy this app on Blink — provision the database, auth, and hosting."


Your agent handles everything automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Ship your AI-built app to production with database, auth, and hosting on Blink


Blink


*Your AI-built app goes from localhost to production — database, auth, and hosting in two commands*


Every production app needs five layers: a persistent database (SQLite won't survive serverless deploys), authentication with session management, file storage for user uploads, backend hosting that stays online, and frontend hosting with HTTPS. The traditional approach requires separate providers for each layer — Vercel, Supabase, Auth0, S3, and Railway — plus 3–4 hours of configuration. Blink Cloud bundles all five layers in one platform.


Vercel and Supabase are separate services that require separate accounts, separate API keys, and manual integration. The Vercel + Supabase production stack costs $45–145/month depending on usage, and Supabase's free tier pauses your database after 7 days of inactivity. Blink Cloud is a single platform that includes database, auth, storage, backend, and hosting — your agent provisions everything from one provider without any manual wiring.


Yes. The Blink plugin uses the Model Context Protocol (MCP), which both Cursor and Claude Code support natively. The install command (` npx skills add blink-new/blink-plugin` ) and authentication flow (` blink login` ) are identical for both tools. The 62 MCP tools and 14 skills work the same way regardless of which AI coding agent you use.


SQLite doesn't work on serverless hosting platforms — the filesystem is ephemeral and data disappears on every deploy. When you deploy to Blink, your agent detects the SQLite usage and migrates to Blink's Postgres database automatically. It generates the migration script, applies the schema, and updates your connection string in the environment config.


The traditional 6-service stack (Vercel Pro + Supabase Pro + Auth0 + S3 + Railway + Redis) runs $95–140/month at production scale. Blink Cloud's pricing covers the full stack in one plan. See[blink.new/cloud](https://blink.new/cloud) for current pricing. There's also a free tier for getting started and building your first project.
