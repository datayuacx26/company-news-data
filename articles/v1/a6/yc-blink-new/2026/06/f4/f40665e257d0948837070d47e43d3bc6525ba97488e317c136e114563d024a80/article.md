---
schema_version: "1.0.0"
document_id: "f40665e257d0948837070d47e43d3bc6525ba97488e317c136e114563d024a80"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/blink-cloud-cursor"
published_at: "2026-06-04T00:39:42+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:481dd6db98636ab62d9c5e1544d3e3c800d3e5cf025bb473bf9b728b321b1c20"
---

# How to Set Up Blink Cloud in Cursor: 2-Minute Setup

## The 1-Click Marketplace Setup


Prefer the GUI? Open Cursor, navigate to the Marketplace, search **Blink** , and click Install.


OAuth handles authentication automatically — no terminal, no commands. You get the identical 62 tools and 14 skills as the terminal path. Both methods produce the same connected agent.


If you already installed the plugin via the Marketplace, skip` npx skills add` . Both paths lead to the same setup — pick whichever fits your workflow.


## Before and After: The Infrastructure Gap


Before Blink, full-stack infrastructure in Cursor meant wiring together independent services:


Service Setup time Monthly cost


Database (Supabase) 15 min $25/mo


Auth (Auth0 or Clerk) 30 min $23/mo


Hosting (Vercel) 20 min $20/mo


Storage (S3 + CloudFront) 25 min $15/mo


Domains (Route53) 15 min $12/mo


Queue (Upstash) 10 min $10/mo


Notifications (Resend) 15 min $10/mo


Monitoring (Sentry) 20 min $26/mo


**Total** **~3 hours** **~$130/mo**


After` npx skills add blink-new/blink-plugin` +` blink login` :


- 1 platform
- 2 commands
- 1 bill
- Agent does the rest


Blink is not a wrapper around those services. It runs its own Postgres database, auth system, backend runtime, storage, and hosting. You get one dashboard and one bill.


## What Your Agent Gets: 62 Tools, 14 Skills


This is not a single MCP integration. The Blink plugin ships a full inventory across 18 categories.


**62 MCP tools** span: database, auth, backend, hosting, storage, domains, queue, AI gateway, realtime, RAG, connectors, notifications, web search, agents, and phone.


**14 skills** cover every major capability:` blink-full-stack` ,` blink-database` ,` blink-auth` ,` blink-backend` ,` blink-storage` ,` blink-queue` ,` blink-deploy` ,` blink-domains` ,` blink-ai` ,` blink-realtime` ,` blink-rag` ,` blink-notifications` ,` blink-connectors` ,` blink-agents` .


**4 specialized subagents** handle distinct build modes: full-stack-builder, frontend-developer, backend-developer, and verifier.


Each skill tells your agent which tools to use, in what order, and how to verify success. That is what separates a skill from a raw tool call — the agent knows the full workflow, not just that the tool exists.


## Your First Prompts After Setup


Once Blink Cloud is connected, try these in Cursor's agent panel:


1. ` "Create a new Blink project called my-app with Google auth, a users table, and deploy it"`
2. ` "Build me a full-stack app using Blink and host it on Blink Cloud"`
3. ` "Set up a Postgres database with a posts table and auth for my Next.js app"`


Your agent reads the Blink skills, selects the right tools, and provisions infrastructure step-by-step. The terminal shows each action as it runs.


For a deeper walkthrough of what you can build next, see[Ask Cursor to Build a Full-Stack App Using Blink](https://blink.new/blog/cursor-full-stack-app-blink) .


Cursor agent executing Blink provisioning across database, auth, and hosting simultaneously — no manual setup required


Blink


## Build a Full-Stack App in Cursor With Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up Blink Cloud in Cursor and build a full-stack app."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


No. The` blink login` command auto-configures MCP — no JSON files to create or edit. If you ever need to troubleshoot a connection, the Blink docs at blink.new/docs/cloud/editors/cursor cover the manual fallback, but most users never reach it.


MCP tools let your agent call an API. Skills are SKILL.md instruction files that teach your agent *when and how* to use those tools — which tool to pick for a task, what to verify after, how to handle failures. The Blink plugin ships both: 62 MCP tools plus 14 skills that wire them into complete workflows. A raw MCP tool gives your agent a hammer; a skill tells it what to build.


The Blink plugin works with both Cursor and Claude Code. The` npx skills add` flow is Cursor-specific; for Claude Code, follow the[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-beginners) — the same 62 tools and 14 skills apply through a different install path.


Replacing them. Blink Cloud runs its own Postgres database, auth system, backend runtime, storage, and hosting. You get one platform, one dashboard, and one bill — not a wrapper that routes traffic through three other services.


Blink has a free tier that includes database, auth, and deploy to a Blink subdomain — no credit card required. Paid plans start when your project needs more than the free tier limits. See[blink.new/cloud](https://blink.new/cloud) for current pricing.


An empty project list is correct for a new account — it confirms Blink Cloud is connected and waiting for your first project. If you see an error instead, run` blink login` again to refresh the API key. The connection should resolve immediately.
