---
schema_version: "1.0.0"
document_id: "4cb1ae04c91d896d07a31f20f67107f8a1f656466441aa1fab7405f5234361ca"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/add-blink-cloud-to-cursor"
published_at: "2026-06-13T00:21:20+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:777a711f6bd1f09d3a78126c26a45cf4512700596af0345ff99a8b69741e85b9"
---

# How to Add Blink Cloud to Cursor: Full-Stack Infrastructure in 2 Minutes

## What You Can Build After Setup


With Blink connected, your agent handles the entire build — not just the frontend code.


**A SaaS tool with subscription billing.** Ask your agent to create a project, set up Google auth, add a users table with a` plan` field, wire Stripe for payment collection, and deploy. Your agent provisions all of it in a single session. The backend runs on Cloudflare Workers; the database is ready immediately; auth is configured. You get a live URL under 5 minutes.


**A scheduling app.** User authentication, a bookings database, time slot availability logic, email confirmation on booking, public booking page — all in one prompt. The[how to build a scheduling app guide](https://blink.new/blog/how-to-build-a-scheduling-app) walks through the exact prompt sequence.


**A CRM for your team.** Contacts table, deals pipeline with Kanban view, activity log, team authentication with role-based access, reporting dashboard. Your agent builds and deploys the whole thing. The[how to build a CRM with AI guide](https://blink.new/blog/how-to-build-a-crm-with-ai) covers each step in detail.


The pattern is the same for any project: describe what you want, your agent provisions the infrastructure and builds the app, you get a production URL.


Three production apps built with Cursor and Blink Cloud — CRM, scheduling app, and SaaS tool all deployed and live


blink


*Three production apps built with Cursor and Blink Cloud — CRM, scheduling app, and SaaS tool all deployed and live*


## Before vs After Blink


Here's what building a production-ready app actually looks like with and without Blink Cloud:


Without Blink With Blink


Database Manual Supabase setup (30-60 min) Automatic


Auth Manual Clerk setup (20-40 min) Automatic


Deploy Manual Vercel config (15-30 min) Automatic


Custom domain DNS config in 3 dashboards (30+ min) One tool call


Time to production 3-4 hours Under 10 minutes


Monthly cost $70-130 (Supabase + Vercel + Clerk) Free to start ($24.95/month for Starter)


Number of accounts 6-8 separate services 1 account


A developer building their first project on the traditional stack loses a full working day to infrastructure setup — before writing a single line of product code. That cost compounds: every new project repeats the same 3-4 hour setup. Across 10 projects, that's weeks of infrastructure work that produces nothing users see.


## Build This With Your AI Agent


Add full-stack infrastructure to your Cursor agent in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then open Cursor and ask your agent:


> "Build me a full-stack app with database, auth, and a backend API — and deploy it to production on Blink."


Your agent uses the 62 MCP tools to create the project, provision the database, configure authentication, deploy the backend, and give you a live URL.


The[14 skills](https://blink.new/docs/cloud/tools/skills) are the difference between an agent that knows the API calls and one that knows the workflow — when to create the project first, how to handle schema migrations, when to deploy vs when to test locally. The skills encode production-proven patterns that took months of iteration to develop.


[Explore Blink Cloud →](https://blink.new/cloud)


Developer checks live app URL on phone after setting up Cursor with Blink Cloud infrastructure in 2 minutes


blink


*Developer checks live app URL on phone after setting up Cursor with Blink Cloud infrastructure in 2 minutes*


## Frequently Asked Questions


It downloads 14 skill files (teaching your agent each Blink Cloud capability), 4 subagent configurations (full-stack-builder, frontend-developer, backend-developer, verifier), persistent coding rules for Blink SDK usage, and the MCP server configuration pointing to Blink Cloud's 62 tools. Everything writes to your Cursor workspace automatically — no manual file editing needed.


No. The` npx skills add` command handles MCP configuration automatically. If Cursor can't find the command due to a PATH issue, add the MCP config to your global` ~/.cursor/mcp.json` instead of the project file — that resolves the issue on most setups.


Yes. The free tier includes one project with a database, user authentication, file storage, and production hosting — no credit card required. Paid plans start at $24.95/month (Starter) for custom domains and unlimited projects. Pro is $49/month and adds unlimited projects with a serverless backend.


Blink Cloud replaces Vercel (hosting), Supabase (database), Clerk or Auth0 (authentication), AWS S3 (file storage), QStash (background jobs and queues), Resend (email), Ably (real-time), and an AI gateway like OpenRouter — all from one account. The full cost comparison is on[blink.new/cloud](https://blink.new/cloud) .


Yes. Blink Cloud works with any MCP-compatible coding agent — Cursor, Claude Code, Codex, and others. The` npx skills add` command is specific to Cursor's skill system; for Claude Code, you add a` .mcp.json` file to your project. The same 62 tools are available in both environments.
