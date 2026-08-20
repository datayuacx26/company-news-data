---
schema_version: "1.0.0"
document_id: "9d1357cf6409f599320cfb92d4bc92d80a8151ff7df7460089d5fe5bd6b1c267"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-app-blink-cloud"
published_at: "2026-05-03T00:36:39+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:e392d01d8e7ba2dcb8a2d6dfbadbd7bbf72ca1f8ac7160ffa060ca8d5901c402"
---

# How to Deploy a Cursor Project to Production (Database, Auth, and Hosting in 2 Minutes)

## The Blink Path: 1 Platform, 2 Commands, 2 Minutes


[Blink Cloud](https://blink.new/cloud) is built for exactly this moment. It's full-stack infrastructure designed for AI-coded apps — the platform that takes you from "Cursor wrote the code" to "it's running in production" without touching Vercel, Supabase, Auth0, or S3.


The setup is two commands:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


That's it. Here's what each command actually does:


` npx skills add blink-new/blink-plugin` downloads[14 skills](https://blink.new/docs/cloud/tools/skills) into your project and auto-configures the MCP server. Your Cursor agent gets 62 MCP tools covering database, auth, backend, storage, and hosting. No manual` mcp.json` editing — the CLI handles it.


` blink login` opens your browser, completes auth, and saves your API key. Your agent is now connected to[Blink Cloud](https://blink.new/cloud) .


One bill. One dashboard. Everything included.


Blink Cloud is also available directly in the Cursor Marketplace — search for "Blink" and install with one click. No commands needed if you prefer the UI route.


## What Blink Cloud Actually Provisions


When your Cursor agent deploys on Blink, it's not pushing to a static host and hoping the rest figures itself out. It provisions your full stack:


Service What Blink does What you'd need otherwise


Database PostgreSQL, provisioned and migrated automatically Supabase or Railway Postgres


Auth User management, social login, session handling Auth0 or Clerk


Backend Serverless functions, API routes, webhooks Railway or Fly.io


File storage S3-compatible uploads and CDN delivery AWS S3 + CloudFront


Hosting Deploy and serve your Next.js or React app Vercel


Custom domains DNS management, SSL certificates Cloudflare + Vercel


Every one of those rows is a separate account on the traditional path. On Blink, they're one platform.


The 62 MCP tools your agent gets from Blink span all of these surfaces. Provisioning a database, running a migration, creating an auth user, uploading a file — all from the same conversation your agent is already in.


## Step-by-Step: Setting Up Blink With Cursor


### Install the Blink plugin


Open your terminal in your project directory and run:


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads 14 agent skills and configures the MCP server. You'll see it confirm that your` mcp.json` has been updated — no manual editing required.


### Connect your account


```text
blink   login
```


Your browser opens. Sign in or create a Blink account (free to start). Your API key is saved automatically. Your Cursor agent now has access to all 62 Blink MCP tools.


### Fully restart Cursor


Quit Cursor completely (` Cmd+Q` on macOS) and reopen it. A window reload isn't enough — MCP servers start at launch. After restarting, go to **Settings → MCP** and confirm Blink shows a green dot.


### Ask your agent to deploy


Switch to Agent mode in Cursor and type:


```text
Deploy this app to Blink with a PostgreSQL database, user auth, and production hosting.


```


Your agent reads the codebase, provisions what it needs, runs migrations, and deploys. Watch the progress in the chat.


### Verify in the Blink dashboard


Log in at[blink.new/cloud](https://blink.new/cloud) to see your running app, database tables, and auth users. Your live URL is ready.


## What Happens When You Ask Cursor to Deploy


Here's exactly what a deploy conversation looks like after the plugin is installed:


You type in Cursor's agent chat:


```text
Deploy this app to Blink. It needs a PostgreSQL database (users, posts tables),
Google OAuth auth, and the backend deployed at api.myapp.com.


```


Your agent responds with a plan, then executes:


1. Calls` blink_provision_database` → Blink creates a PostgreSQL instance and returns the connection string
2. Calls` blink_run_migration` → runs your schema migrations against the new database
3. Calls` blink_configure_auth` → sets up user auth with Google OAuth
4. Calls` blink_deploy_backend` → deploys your API routes as serverless functions
5. Calls` blink_configure_domain` → points` api.myapp.com` at your deployed backend
6. Calls` blink_deploy_frontend` → deploys your Next.js frontend and returns the live URL


The entire sequence completes in about 2 minutes. You don't write a single YAML file.


The 14 skills the Blink plugin installed tell your agent how to do all of this correctly — the order to provision things, how to handle environment variables, how to wire up auth to your database. The agent doesn't have to guess.


Asking Cursor's AI agent to deploy on Blink — the agent handles all infrastructure automatically


Blink


*Asking Cursor's AI agent to deploy on Blink — the agent handles all infrastructure automatically*


## What the Agent Does vs. What You Do


Before Blink Cloud, the deployment split looked like this:


- **Cursor's job:** write the code
- **Your job:** provision 5 services, configure environment variables, debug CORS, manage API keys, read documentation for each platform


After:


- **Cursor's job:** write the code, then deploy it
- **Your job:** run two commands once, then ask the agent


That's the before/after that matters. The agent can now own the full loop — not just the code, but the infrastructure it runs on.


Reddit's r/nocode captured this gap well in early 2026: "The deployment problem is the biggest unsolved pain point I see. People get 70-80% of the way with AI tools and then get completely stuck when it comes time to actually ship." Blink Cloud addresses exactly that gap.


According to[DigitalOcean's Claude Code deployment guide](https://digitalocean.com/community/tutorials/app-lifecycle-development-app-platform-claude) , the recommended process for deploying AI-generated apps involves separate database, auth, and hosting services — a workflow that Blink eliminates by consolidating everything into one platform. The[State of Vibe Coding 2026](https://northflank.com/blog/best-deployment-platforms-for-vibe-coders) from Northflank found that infrastructure setup is the #1 friction point for developers shipping AI-built apps.


## Build Your App's Infrastructure Into Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app with database, auth, and production hosting on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Cursor can write the code for your app, but it can't deploy it by default — it doesn't have access to infrastructure. Once you install the Blink plugin (` npx skills add blink-new/blink-plugin` ), your Cursor agent gets 62 MCP tools that cover database provisioning, auth setup, backend deployment, and hosting. At that point, you can ask Cursor directly to deploy your app and it will handle the full infrastructure stack through Blink Cloud.


The total cost of the typical DIY stack — Vercel + Supabase + Auth0 + S3 + Railway — runs around $130/month for a production app. Blink Cloud consolidates all of those services into a single platform with one bill. For most apps, that means significant savings and zero time spent managing five separate accounts. Blink also offers a free tier to get started.


After installing the Blink plugin (` npx skills add blink-new/blink-plugin` +` blink login` ), ask your Cursor agent: "Provision a PostgreSQL database for this project and run the schema migrations." The agent calls Blink's MCP tools, creates the database, returns the connection string, and runs your migrations — all in one conversation. No Supabase dashboard, no Railway setup, no connection string copy-pasting.


No. Running` npx skills add blink-new/blink-plugin` auto-configures the MCP server and writes the correct entry to your` mcp.json` . The CLI handles everything. After running the two commands (` npx skills add blink-new/blink-plugin` and` blink login` ), fully quit and reopen Cursor — MCP servers start at launch, not on window reload.


Blink Cloud replaces: Vercel (hosting), Supabase (database + basic auth), Auth0 (auth), AWS S3 (file storage), and Railway (backend services). Instead of creating five accounts and managing five dashboards, you run two commands and your Cursor agent handles provisioning through a single platform. See[how Cursor compares to Claude Code](https://blink.new/blog/cursor-vs-claude-code) for more on which coding tool to pair with Blink, and[the best AI app builders](https://blink.new/blog/best-ai-app-builders) for a broader look at the ecosystem.


Yes. The same` npx skills add blink-new/blink-plugin` command works for Claude Code. After running` blink login` , your Claude Code agent also gets 62 MCP tools and 14 skills for full-stack deployment. The workflow is identical: ask your agent to build and deploy, and Blink provisions the infrastructure automatically.
