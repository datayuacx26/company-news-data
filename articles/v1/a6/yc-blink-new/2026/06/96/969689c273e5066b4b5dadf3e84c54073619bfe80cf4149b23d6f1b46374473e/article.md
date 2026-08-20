---
schema_version: "1.0.0"
document_id: "969689c273e5066b4b5dadf3e84c54073619bfe80cf4149b23d6f1b46374473e"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/blink-cloud-cursor-setup"
published_at: "2026-06-11T00:17:58+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:81a8d896c0b62ada00a6dcefd8ffdef8a54d3ffb4153b11c83d358b02a7164aa"
---

# How to Set Up Blink Cloud in Cursor: Add Full-Stack Infrastructure in 2 Minutes

## Set Up Blink Cloud in Cursor: 3 Steps


1


#### Install the Blink plugin


Open your terminal and run:


```text
npx   skills   add   blink-new/blink-plugin
```


This installs[14 skills](https://blink.new/docs/cloud/tools/skills) — specialized instruction sets that teach Cursor's agent how to use Blink's 62 MCP tools. The CLI automatically writes the Cursor MCP configuration. No` mcp.json` editing required.


2


#### Log in to Blink


Run:


```text
blink   login
```


Your browser opens. Log in or sign up for a free Blink account. Your API key saves to the MCP config automatically. The connection is live in under 30 seconds.


3


#### Ask your agent to build


Open Cursor and ask your agent:


> "Build me a full-stack app using Blink and host it on Blink."


Your agent now has 62 MCP tools at its disposal. It provisions the database, configures auth, deploys the backend, and returns a live URL — without you touching any config.


You can also skip the terminal entirely. Search for "Blink" in the Cursor Marketplace, click Install, and authenticate with OAuth. The same 62 tools become available in your agent immediately.


## What Blink Gives Your Agent: 14 Skills, 62 MCP Tools


MCP tools are functions your agent can call — like` create_database` ,` deploy_backend` , or` configure_auth` . Skills are the instruction sets that teach your agent *when* and *how* to use those tools correctly.


The 14 skills downloaded by` npx skills add blink-new/blink-plugin` cover every layer of a production stack:


**Database** — Postgres provisioning, schema inference from your app code, migrations, row-level security, index configuration, and environment variable injection. Your agent reads your existing code to generate the correct schema.


**Authentication** — User signup, login, social OAuth (Google, GitHub, Apple, Microsoft), magic links, JWT handling, email verification, and session management. One tool call per auth flow.


**File Storage** — Upload endpoints, access control (public vs. private), CDN delivery, signed URLs for private assets. Zero manual bucket configuration.


**Backend Deploy** — API server deployment to Blink's global edge, environment variable sync, custom domain wiring, auto-SSL, zero-downtime deploys.


**Hosting** — Frontend served from Blink's CDN with automatic SSL and custom domain support. Your agent deploys with one command.


**AI Gateway** — Access to 200+ AI models (GPT, Claude, Gemini, and more) through one OpenAI-compatible API. No separate API accounts or keys to manage.


**Realtime** — Presence channels, live cursors, pub/sub — provisioned from a single tool call.


Across all 14 skills, your agent can access 62 distinct tools — from` create_project` and` provision_database` to` set_secret` and` list_deployments` . That's the full coverage a production app needs, available from natural language prompts inside Cursor.


Blink Cloud setup in 3 steps: npx skills add, blink login, ask agent to build


Blink


*Blink Cloud setup in 3 steps: npx skills add, blink login, ask agent to build*


## Real Prompts That Work Immediately


Once Blink is configured in Cursor, these prompts work out of the box:


**Task manager with user accounts:**


> "Build a full-stack task management app. Users sign up, log in, create projects, and add tasks with due dates and priorities. Store everything in Blink's database and host it on Blink."


**CRM for a small team:**


> "Build a lightweight CRM. Our team tracks contacts, companies, and deals through a pipeline. Add user auth so each team member has their own login. Deploy on Blink with a custom domain."


**Booking system:**


> "Build a booking app where customers can schedule appointments with a service provider. Include calendar availability, email confirmations, and a dashboard for the provider. Host on Blink."


Each of these prompts triggers your agent to provision exactly the infrastructure the app needs. No manual steps. No back-and-forth between dashboards.


## Before vs After: 8 Services Replaced by 1 Platform


Most production Cursor apps need at least 8 services to run properly. Blink replaces all of them.


Service DIY Stack Blink Cloud


Hosting Vercel ($20/mo) Included


Database Supabase ($25/mo) Included


Auth Clerk or Auth0 ($35/mo) Included


File Storage AWS S3 ($10/mo) Included


Backend API Railway ($20/mo) Included


Email Resend ($20/mo) Included


Realtime Ably ($25/mo) Included


AI Gateway OpenRouter ($50+/mo) Included


**Total cost** **~$205/month** **Free to start**


**Setup time** **3–5 hours per project** **2 minutes**


**Accounts needed** **8** **1**


**Agent access** **None — manual only** **62 MCP tools**


The numbers above use current pricing from each provider's website (verified June 2026). The free tier comparison is honest: most DIY services have a free tier too. The difference is that Blink's free tier includes all 8 services pre-connected — you get a working full stack without any configuration.


## Build Your First Full-Stack App With Cursor and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack project management app with user auth and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account, no Auth0 setup.[Explore Blink Cloud →](https://blink.new/cloud)


Cursor + Blink Cloud: ask your agent to build a full-stack app and it handles database, auth, backend, and hosting


Blink


*Cursor + Blink Cloud: ask your agent to build a full-stack app and it handles database, auth, backend, and hosting*


## Frequently Asked Questions


No.` npx skills add blink-new/blink-plugin` writes the Cursor MCP configuration automatically. After` blink login` , your API key is saved to the config. You never touch` mcp.json` for the Blink integration. If you prefer a GUI, install Blink from the Cursor Marketplace — OAuth handles everything with zero command-line setup.


MCP tools are functions your agent calls directly —` create_database` ,` deploy_backend` ,` configure_auth` . Skills are instruction sets that teach your agent when and how to use those tools in context. Without skills, your agent knows a function exists. With skills, it knows to infer your database schema from your app code, set the right row-level security per table, and update your environment variables automatically.


Yes.` npx skills add blink-new/blink-plugin` works with Cursor, Claude Code, Codex, Windsurf, and any MCP-compatible coding agent. The CLI detects which agent you're using and writes the configuration to the correct location. One install covers all of them. See the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup) for a deeper walkthrough of the MCP setup flow.


Blink starts free. The free tier includes one project with database, auth, file storage, and production hosting — no credit card required. The Starter plan at $24.95/month adds custom domains and more projects. The Pro plan at $49/month adds unlimited projects and a serverless backend. See[blink.new/cloud](https://blink.new/cloud) for current pricing.


Yes. Tell your agent: "I have an existing Next.js app with these tables: \[describe your schema\]. Deploy it on Blink — create the database, configure auth, and give me a live URL." The agent reads your existing code and provisions infrastructure to match. You don't rewrite anything.


Blink supports React/Vite, Next.js (static export), Vue, Svelte, Astro, and Expo React Native for the frontend. The serverless backend runs on Hono — a fast, lightweight TypeScript framework deployed globally. Your agent writes` backend/index.ts` and deploys it with a single tool call.


No. Your frontend code is standard React, Vue, Svelte, or any other framework you chose — fully portable. The database is SQLite, the most portable database format. The backend is a standard Hono server you can run anywhere. You can export your entire project and deploy it elsewhere at any time.
