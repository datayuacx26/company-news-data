---
schema_version: "1.0.0"
document_id: "5f06f83529aaca218b800a38cb5b365e165bbabad36d8bf40309c4e5745db039"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-project-production"
published_at: "2026-06-10T12:37:43+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:b5459dc3d0d90b87dc47b1a4e8e91fca5d1bcb1d461f2b63350df59037acaca5"
---

# How to Deploy a Cursor Project to Production (Database, Auth, and Hosting in 2 Minutes)

## What Blink Sets Up Automatically


No Supabase account needed. No Vercel configuration. Blink handles both.


When you ask your Cursor agent to deploy using Blink, here's what gets provisioned:


**PostgreSQL database** — configured for your schema. Blink reads the data models in your code and creates the tables. Connection strings are injected automatically into your environment.


**User authentication** — email + social sign-in, out of the box. Authentication is built into Blink, so your users can sign up and log in from day one. No Auth0, no Clerk, no callback URL configuration.


**Backend API endpoints** — your server-side routes deployed as managed functions. No Docker, no server configuration.


**HTTPS hosting with CDN** — your app gets a production URL immediately. Custom domain setup takes one more command.


**Environment variables** — auto-injected. The connection strings, API keys, and secrets your app needs are wired in automatically.


## Step-by-Step: From Cursor Code to Live App


1


#### Install the Blink plugin in Cursor


Run this in your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


The CLI downloads 14 skills and configures the MCP connection automatically. No` mcp.json` editing. No manual configuration.


Alternatively: install "Blink" directly from the Cursor Marketplace with one click.


2


#### Authenticate with Blink


```text
blink   login
```


Your browser opens. Sign in with GitHub or email. The API key is stored automatically. Close the browser tab — you're connected.


3


#### Ask Cursor to deploy your app


In a Cursor chat, describe what you need:


> "Deploy this app to production using Blink — set up the database, user authentication, and hosting. Here's what the app does: \[brief description of your app's data model and features\]."


The more specific you are about your data model, the better Blink configures the database schema.


4


#### Verify your app is live


Cursor will return a Blink URL — something like` yourapp.blink.new` . Open it in your browser. Your app is live.


If you want a custom domain, tell Cursor: "Connect the custom domain` yourdomain.com` to this Blink app." Done in under a minute.


## After Deployment: What to Do Next


Your app is live. Three things to do next:


**Custom domain** — Blink supports custom domains. Ask Cursor: "Add my custom domain to this Blink deployment." The agent configures DNS settings and SSL automatically.


**Environment variable management** — any environment-specific config (API keys for third-party services, feature flags) goes through Blink's env management. Tell Cursor what you need to add.


**Monitor your app** — Blink provides logs and usage dashboards. No separate monitoring tool required for most apps in early stages.


## Deploy Your Cursor-Built App in 2 Minutes


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask Cursor:


> "Deploy this app to production using Blink — set up the database, user authentication, and hosting."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Blink provisions database, authentication, and hosting in one agent workflow — everything checked off automatically


Blink


## Frequently Asked Questions


If you have an existing PostgreSQL database, you can point your Blink deployment at it by providing the connection string. For new projects, Blink provisions a fresh database automatically — this is the faster path for most Cursor-built apps starting from scratch.


You can continue using Vercel for existing projects. Blink is the better choice when you want database, auth, and hosting to be provisioned together in one agent workflow — which is exactly how Cursor-built apps work best. Migrating an existing Vercel project to Blink is also possible; ask Cursor to handle the migration.


Traditional production stack — Supabase ($25/mo) + Vercel ($20/mo) + Auth0 ($23+/mo) + storage — runs $68+ per month before any usage overages. Blink's pricing consolidates everything. Check[blink.new/pricing](https://blink.new/pricing) for current tiers. Most early-stage apps run on Blink's free tier.


Blink supports Next.js, Node.js, and most JavaScript/TypeScript frameworks natively. For Python backends, ask Cursor to scaffold the app using a supported runtime — Blink's backend layer handles the deployment. Check[blink.new/docs](https://blink.new/docs) for the current list of supported frameworks and runtimes.


The Blink MCP gives Cursor 14 skills covering database provisioning, user management, file storage, API deployment, custom domain setup, and environment variable management. Instead of Cursor writing generic configuration code for external services, it can directly call Blink to provision real infrastructure. The result: one agent workflow instead of six separate tool setups. Learn more at[blink.new/docs/cloud/tools/skills](https://blink.new/docs/cloud/tools/skills) .
