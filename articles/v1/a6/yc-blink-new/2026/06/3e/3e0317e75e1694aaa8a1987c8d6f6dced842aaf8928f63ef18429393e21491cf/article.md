---
schema_version: "1.0.0"
document_id: "3e0317e75e1694aaa8a1987c8d6f6dced842aaf8928f63ef18429393e21491cf"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-backend-deploy"
published_at: "2026-06-11T12:32:10+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:891360c934f9ec2e2792a9a867bc3ef925f3fb35368284684e772a99dc9f036f"
---

# How to Deploy What Claude Code Builds: Database, Auth, and Hosting Included

## A Different Path: Deploy in 2 Commands


[Blink](https://blink.new/cloud) is a full-stack cloud platform built specifically for AI-coded apps. It provides database, auth, hosting, file storage, and serverless backend functions — all included, all configured automatically, on one bill.


Once you connect Blink to your agent, the agent can provision and manage your entire infrastructure through natural-language commands. You describe what you want to build; the agent builds and deploys it.


Here's how to connect it:


**Step 1 — Install the Blink plugin for Claude Code or Cursor:**


```text
npx   skills   add   blink-new/blink-plugin
```


**Step 2 — Authenticate:**


```text
blink   login
```


That's it. Your coding agent now has access to 62 Blink tools covering every infrastructure primitive:` create_database` ,` setup_auth` ,` deploy_app` ,` upload_file` ,` create_api_route` , and more.


Alternatively, if you use Cursor, install the **Blink** extension directly from the[Cursor Marketplace](https://blink.new/cloud) — one click, no terminal required.


After setup, you can ask your agent directly:


```text
"Build me a job board and host it on Blink"
"Add user authentication and a database to this app"
"Deploy what we just built with a custom domain"


```


The agent handles the provisioning. You get a live URL.


## What Blink Actually Provisions


When your agent uses Blink to deploy, here's what gets created automatically:


### Database


Every Blink app gets a dedicated Postgres database — no Supabase account required. Schema management, migrations, and connection pooling are handled automatically. Your agent can create tables, run queries, and update schemas through the Blink tool suite.


### Authentication


Built-in auth supports email/password, magic links, Google OAuth, GitHub OAuth, and Apple Sign In. No Clerk account, no Auth0 dashboard, no JWT secret configuration. Users and sessions are managed inside the same platform.


### Hosting and CDN


Blink deploys your app to a global CDN automatically. Every deployment gets a` *.blink.app` URL instantly, with custom domain support available. Zero Vercel configuration, zero Netlify config files.


### Backend Functions


Serverless functions run on Blink's backend infrastructure. Your agent can create API routes, background jobs, and scheduled tasks — all within the same platform. No Railway project, no separate Fly.io app.


### File Storage


Uploads, media, and document storage are built in. Your agent can create storage buckets, generate signed URLs, and manage file access policies through the same tool interface.


## Step-by-Step: From Claude Code Project to Live in Production


This is the exact workflow for deploying an app built with Claude Code or Cursor:


**1. Install the plugin** (one-time setup):


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


**2. Open Claude Code or Cursor** and start a new session in your project directory.


**3. Ask your agent to deploy:**


```text
"I have a Next.js app that needs a database with users and posts tables,
email authentication, and hosting. Deploy everything to Blink."


```


**4. The agent runs the Blink tools:**


```text
→ create_database("my-app-db")
→ create_table("users", { id, email, created_at })
→ create_table("posts", { id, user_id, title, content })
→ setup_auth({ providers: ["email", "google"] })
→ deploy_app({ framework: "nextjs", domain: "my-app" })


```


**5. Your app is live** at` my-app.blink.app` — with a real database, real auth, and a real CDN-hosted frontend. Time elapsed: under 5 minutes.


For a complete walkthrough of MCP setup for Cursor specifically, see[Cursor MCP Setup Guide: Connect Your Agent to Real Infrastructure](https://blink.new/blog/cursor-mcp-setup) .


Blink provisions your entire infrastructure stack automatically — database, auth, hosting, and CDN in under 5 minutes


Blink


## Before vs After: The Real Comparison


Traditional stack Blink


**Services required** 5–6 (Vercel + Supabase + Clerk + S3 + Railway + Resend) 1


**Accounts to create** 5–6 1


**Setup time** 3–6 hours Under 5 minutes


**Monthly cost** $100–$115/mo $29–$99/mo


**API keys to manage** 10–20 across services 1


**Dashboards to monitor** 5–6 1


**Agent integration** Manual config per service 62 tools, zero config


**Custom domain** Vercel config + DNS setup Built-in


The cost difference alone — roughly $70–$86/month in savings — covers a Blink Pro plan with room to spare. The time savings are more significant: 3–6 hours of infrastructure setup, repeated every time you start a new project, compounded across every app you build.


For developers who build with AI coding tools regularly, that adds up to days of recovered time each month.


For a deeper look at the full deployment workflow including CI/CD and production best practices, see[Deploy Your Cursor App to Production: The Complete Infrastructure Guide](https://blink.new/blog/deploy-cursor-app-production) .


Ship production apps — not prototype demos — by combining Claude Code with Blink's full-stack infrastructure


Blink


## FAQ


Yes. The Blink plugin installs as an MCP server that any MCP-compatible agent can use. Claude Code, Cursor, and other tools that support the Model Context Protocol all work. Run` npx skills add blink-new/blink-plugin` once and the 62 Blink tools are available across every agent you use.


Nothing — Blink doesn't touch your existing services. If you're migrating an existing app, you can move incrementally: deploy the frontend to Blink first, then migrate the database, then cut over auth. If you're starting a new project from scratch, you skip all that setup entirely.


No. The Blink tools your agent calls handle all configuration automatically. No` vercel.json` , no` supabase/migrations/` , no Auth0 tenant setup, no S3 bucket policies. Your agent describes what it needs in natural language; the tools provision the resources.


Blink plans start at $29/month and scale to $99/month for the Pro plan. The traditional stack — Vercel Pro ($20) + Supabase Pro ($25) + Clerk Pro ($25) + S3 ($10) + Railway ($10) + Resend ($20) — runs $110+/month before usage spikes. Most Blink users save $50–$80/month from day one, with the additional benefit of one support team and one billing relationship.


Blink exports standard Postgres schemas, standard S3-compatible storage, and standard OAuth-compatible auth. If you ever migrate out, you're working with open standards, not proprietary lock-in. That said, Blink's architecture is designed for production scale — the same infrastructure runs thousands of deployed apps today.
