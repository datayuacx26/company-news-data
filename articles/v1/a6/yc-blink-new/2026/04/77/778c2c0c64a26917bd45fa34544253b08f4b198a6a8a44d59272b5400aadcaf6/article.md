---
schema_version: "1.0.0"
document_id: "778c2c0c64a26917bd45fa34544253b08f4b198a6a8a44d59272b5400aadcaf6"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-project"
published_at: "2026-04-22T12:58:10+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:52:23.948620+00:00"
content_hash: "sha256:d402c0faddaed1a0f556b8a1d3e12320f0669923b6c88d5b51b32804607dc7a9"
---

# How to Deploy a Cursor Project to Production (Database, Auth, and Hosting in 2 Minutes)

## Deploy Your Cursor Project: Step-by-Step


1


#### Install the Blink plugin


Run this from any directory in your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


The CLI downloads 14 skills and writes the correct` mcpServers` entry to` .cursor/mcp.json` automatically. You will not need to touch that file.


2


#### Log in to Blink


```text
blink   login
```


A browser tab opens. Sign in or create a free account. Your API key is saved and the MCP server connects immediately.


3


#### Verify the connection


Open Cursor → Settings → MCP. The Blink server should show a green dot. If it shows grey, restart Cursor once — MCP config changes require a full restart.


4


#### Ask Cursor to deploy


Open a Cursor chat and paste this prompt (adapting to your project):


> "Deploy this app with a Postgres database, user authentication, and a custom domain using Blink."


Your agent provisions the database, configures auth, deploys the backend, and returns a live URL — no manual steps required.


5


#### Check your live app


Cursor returns a Blink subdomain URL (e.g.` yourapp.blink.run` ). Click it. Your app is running in production with database, auth, and hosting all included in one bill.


Deploy with the old stack (6 services, 4 hours) vs Blink (1 platform, 2 minutes)


Blink


*Deploy with the old stack (6 services, 4 hours) vs Blink (1 platform, 2 minutes)*


## What Blink Provisions Automatically


When your agent runs a Blink deployment, it sets up the full production stack — not a development preview:


**Database:** Postgres is provisioned automatically. No Supabase account, no connection string setup, no migration scripts to run manually. Your agent creates tables, runs migrations, and queries data directly.


**Auth:** User authentication is included — sign-up, login, JWT session management, password reset flows. No Auth0 account, no Clerk, no Firebase Auth configuration.


**Hosting:** Your app deploys to a Blink subdomain immediately. Custom domains connect in one step. No Vercel config, no DNS propagation debugging.


**Storage:** File uploads and object storage are included. Images, documents, user-uploaded content — no AWS S3 bucket setup, no CORS rules.


**Backend:** Hono-powered serverless API included. Your agent writes and deploys routes directly. No Railway or Render account needed.


The entire stack runs under one Blink account, with one dashboard and one monthly bill. When Cursor asks "where should I deploy this?", the answer is one command, not a list of five services to configure separately.


## What Cursor Developers Usually Get Wrong


**Environment variables spread across services.** A manual stack has Supabase credentials in Vercel, Auth0 secrets in your codebase, and S3 keys in Railway. Cursor's agent can help write code but can't track which secret lives where. Blink centralizes credentials — one place, one set of keys your agent can reference directly.


**Auth configuration that breaks on deploy.** Auth0 and Clerk require callback URLs that exactly match your deployment domain. If you're iterating on Vercel preview URLs, every preview URL needs a separate callback registration. Blink's auth doesn't require this — it knows your deployment URL before auth is configured.


**Database migration state drift.** Running migrations against a Supabase cloud database while developing locally creates schema drift. Blink provisions one database per project with a consistent endpoint, so your agent's migration commands always target the right environment.


**The cold-start surprise.** Free tiers on Vercel and Railway spin down inactive deployments. Your production URL returns a 30-second timeout on first request. Blink keeps your app warm.


## Before and After


Without Blink With Blink


**Accounts needed** 5-6 separate services 1


**Config time** 3-6 hours per project 2 minutes


**Monthly cost** ~$93-130/mo One bill


**API keys to manage** 8-12 across services 1


**Deployment command** Multiple steps across dashboards Ask Cursor


**Database** Supabase (separate account) Included


**Auth** Auth0 or Clerk (separate account) Included


**Storage** AWS S3 (separate account) Included


The[84% of developers who told Stack Overflow they use AI tools daily](https://survey.stackoverflow.co/2025) are still spending hours on deployment config. That's the gap Blink fills.


## Deploy Your Cursor App With Blink Cloud


Add Blink to Cursor in one command —[14 skills](https://blink.new/docs/cloud/tools/skills) auto-configure your MCP:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask Cursor:


Your agent handles the full deployment — database provisioned, auth configured, live URL.[See Blink Cloud pricing →](https://blink.new/cloud)


Cursor app deployed to production — database provisioned, auth configured, live on a custom domain


Blink


*Cursor app deployed to production — database provisioned, auth configured, live on a custom domain*


## Frequently Asked Questions


Run` npx skills add blink-new/blink-plugin` and` blink login` in your terminal. This connects Cursor to Blink Cloud's full infrastructure stack. Then ask Cursor: "Deploy this app with a Postgres database and user authentication using Blink." Your agent provisions and deploys automatically — database, auth, and hosting included in one command flow.


No. Blink includes hosting as part of its infrastructure layer — no Vercel config, no separate hosting account. When you deploy via Blink, your app gets a live URL on a Blink subdomain. Custom domains connect in one additional step. This eliminates the Vercel setup, environment variable sync, and deployment pipeline configuration that a manual stack requires.


Not if you use Blink. Blink provisions a Postgres database automatically when your agent deploys — no Supabase account, no connection string setup, no migration configuration required. Your agent creates tables and runs migrations directly through Blink's MCP tools.


Blink includes auth — sign-up, login, JWT sessions, password reset — with no Auth0 or Clerk account needed. Your Cursor agent configures authentication as part of the deployment flow. No callback URL registration, no OAuth app setup, no environment variable sync between auth provider and hosting platform.


With Blink, under 2 minutes from running the install command to a live URL. Without Blink, a manual stack (Vercel + Supabase + Auth0 + S3 + backend runtime) takes 3-6 hours to configure for the first time per project — account setup, API key management, environment variable sync, and callback URL registration across all services.


A typical manual stack — Vercel Pro + Supabase Pro + Auth0 + AWS S3 + a backend runtime — runs approximately $93-130/month across five separate bills. Blink consolidates all of that into one platform with one bill. See[blink.new/cloud](https://blink.new/cloud) for current pricing.


Yes, once the Blink plugin is installed. After running` npx skills add blink-new/blink-plugin` , your Cursor agent has 62 MCP tools covering database provisioning, auth configuration, file storage, backend deployment, and hosting. The agent can create tables, run migrations, deploy code updates, and check application status — all from inside Cursor without switching to external dashboards.


The Blink plugin is a Cursor MCP server that gives your agent access to full-stack infrastructure — database, auth, storage, backend, and hosting — through 14 skills and 62 tools. Install it with` npx skills add blink-new/blink-plugin` . It auto-configures your` .cursor/mcp.json` and connects immediately after` blink login` . No manual JSON editing required. Also available as a one-click install from the Cursor Marketplace.
