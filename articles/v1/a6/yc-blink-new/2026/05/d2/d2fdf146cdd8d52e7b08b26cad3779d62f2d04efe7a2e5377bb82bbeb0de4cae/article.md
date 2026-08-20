---
schema_version: "1.0.0"
document_id: "d2fdf146cdd8d52e7b08b26cad3779d62f2d04efe7a2e5377bb82bbeb0de4cae"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-project-to-production"
published_at: "2026-05-29T12:15:43+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:84c3006ab97163b3c1f883b77aa15f35150f9334004c615a3d6d23a0f5d7bc23"
---

# How to Deploy a Cursor Project to Production (Database, Auth, and Hosting in 2 Minutes)

## What Blink Cloud Gives You


Blink Cloud is a full-stack infrastructure platform designed specifically for AI-built apps. Everything your Cursor app needs ships as a single platform, not a patchwork of services.


Here's what you get out of the box:


- **Postgres database** — automatic schema migrations, your data, exportable any time
- **Auth system** — email/password, social login, and magic links without Auth0
- **Object storage** — file uploads, images, and binary assets without S3
- **Backend runtime** — API routes, cron jobs, and background tasks without Railway
- **Auto-HTTPS** — TLS certificate provisioned automatically, no certbot
- **Custom domain** — connect from the Blink dashboard in one step
- **62 MCP tools** — your agent calls them directly from the Cursor chat panel


Compare to the alternative: Vercel ($20/mo) + Supabase ($25/mo) + Auth0 ($30/mo) + S3 ($20/mo) = $95/mo minimum, and that's before you've factored in the 3 hours of setup, the four separate dashboards you'll context-switch between, and the integration failures that happen when they don't talk to each other cleanly.


Blink Cloud's free tier covers hobby apps.[Paid plans start at $20/mo](https://blink.new/cloud) — one bill, one platform.


## Step-by-Step: First Cursor-to-Blink Deploy


1


#### Install the Blink plugin in Cursor


Open your terminal inside Cursor. Run` npx skills add blink-new/blink-plugin` followed by` blink login` . A browser window opens — authenticate once, and your API key is saved automatically. No` mcp.json` editing required.


2


#### Tell Cursor to deploy to Blink


In the Cursor agent chat, type: "Deploy this app to Blink Cloud. Set up a Postgres database with the schema we've been building, add user authentication, and deploy to production." Your agent has 62 tools available to execute this end-to-end.


3


#### Your app is live


Blink provisions the database, runs migrations, configures auth, builds the app, and deploys it — all in one agent session. You get a live HTTPS URL in under 2 minutes. Connect a custom domain from the Blink dashboard when you're ready.


For more detail on connecting Cursor to Blink, see the[Cursor backend setup guide](https://blink.new/blog/cursor-backend-setup) .


## What You Don't Need to Set Up


This is the list of accounts and services you can skip entirely when you deploy a Cursor app to Blink Cloud:


- **Vercel account** — hosting is included in Blink Cloud
- **Supabase account** — Postgres database is included
- **Auth0 or Clerk account** — auth system is included (email, social, magic links)
- **AWS S3 account** — object storage is included
- **Railway account** — backend runtime is included
- **Netlify account** — hosting and edge functions are included


You also skip the configuration overhead: no environment variable exports, no webhook URL setup between services, no CORS whitelisting across separate backends.


For a deeper look at what gets wired up automatically, see[what to do after Cursor writes your code](https://blink.new/blog/what-to-do-after-cursor-writes-your-code) .


## Build Your Full-Stack App With Cursor and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app with user auth, a dashboard, and a Postgres database — then deploy it to production on Blink."


Your agent provisions database, auth, backend, and hosting automatically. No separate Vercel. No Supabase. No Auth0.


[Learn more about Blink Cloud →](https://blink.new/cloud)


Ask Cursor to build and deploy — the Blink Cloud MCP handles the full infrastructure stack without leaving the editor


Blink


## Frequently Asked Questions


Yes — any JavaScript or TypeScript app Cursor builds can be deployed to Blink Cloud. The Blink plugin installs 14 skills and 62 MCP tools that your Cursor agent uses to provision infrastructure automatically. Python support is in beta.


No. The` npx skills add blink-new/blink-plugin` command handles MCP configuration automatically. You never touch` mcp.json` , copy API tokens, or restart Cursor manually.


Blink Cloud provisions a managed Postgres database. It runs schema migrations automatically when your agent deploys, and you can export your data at any time — it's your schema, your data.


Blink Cloud has a free tier for hobby apps. Paid plans start at $20/month — which covers database, auth, and hosting in one bill. Compare that to $95+/month for Vercel + Supabase + Auth0 + S3 separately, plus 3 hours of initial setup.


Yes. Connect a custom domain from the Blink dashboard and HTTPS is provisioned automatically. No certbot, no DNS certificate juggling.


Vercel handles only hosting and serverless functions — you still need Supabase for a database, Auth0 for auth, and S3 for file storage. Blink Cloud is a single platform that includes all four. Your Cursor agent can configure and deploy the entire stack in one conversation, without switching between four separate dashboards.
