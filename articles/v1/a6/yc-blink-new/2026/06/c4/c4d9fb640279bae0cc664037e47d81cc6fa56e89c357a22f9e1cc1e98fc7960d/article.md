---
schema_version: "1.0.0"
document_id: "c4d9fb640279bae0cc664037e47d81cc6fa56e89c357a22f9e1cc1e98fc7960d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-mcp-setup-blink-cloud"
published_at: "2026-06-01T12:31:53+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:1f5068a61a17a980b1b0289854fcb0feced706645007443f27e0e0bd7cfc6ab9"
---

# How to Set Up Blink Cloud in Cursor: 2-Minute Guide

## What You Get


After installation, your Cursor agent has access to everything needed to ship production apps:


Service What Blink provides What you'd otherwise use


Database PostgreSQL, auto-provisioned Supabase, PlanetScale, Railway


Auth Email/OAuth built in Auth0, Clerk, Firebase Auth


File storage S3-compatible buckets AWS S3, Cloudflare R2


Backend Hono server on Cloudflare Workers Vercel, Railway, Render


Hosting Live subdomain on` blink.new` Vercel, Netlify


No separate accounts. No separate bills. One` blink login` covers all five.


8 services and 3 hours of setup vs Blink's two-command install — the infrastructure gap is real


Blink


*8 services and 3 hours of setup vs Blink's two-command install — the infrastructure gap is real*


## What the 14 Blink Skills Do


Skills are natural-language instructions loaded into your agent alongside the 62 MCP tools. They tell your agent *how* to use Blink, not just *that* it can. The 14 skills cover:


- **Database** — schema creation, migrations, queries, seeding test data
- **Auth** — user creation, session management, OAuth provider setup
- **Storage** — bucket creation, file upload/download, signed URL generation
- **Backend** — API route creation, middleware, serverless function deployment
- **Deploy** — production builds, subdomain assignment, environment variables
- **Project management** — workspace creation, token rotation, billing inspection


Your agent reads these automatically. You don't configure them. Ask your agent to "create a users table and add email/password auth" — it knows exactly which tools to call and in what order.


## Your First App With Blink in Cursor


After setup, open a Cursor chat and try this prompt:


> "Build a task management app with user accounts, a PostgreSQL database for tasks, and deploy it live on Blink."


Here's what your agent does:


1. Creates a Blink project workspace
2. Provisions a PostgreSQL database
3. Sets up email auth with session management
4. Creates the frontend (React or plain HTML, your choice)
5. Deploys the backend on Cloudflare Workers
6. Returns a live URL:` your-app.blink.new`


Total time from prompt to live URL: under 5 minutes. Without Blink, configuring the same stack manually takes 2–3 hours.


According to Anthropic's[MCP protocol spec](https://modelcontextprotocol.io/introduction) , MCP enables AI models to use tools with structured, reliable inputs. Blink's 62 tools are designed specifically for this — each one handles a real infrastructure operation your agent would otherwise have to script from scratch.


For a deeper look at how MCP configuration works in Cursor, read[Cursor MCP JSON: Add Any Tool to Your AI Agent](https://blink.new/blog/%5BREDACTED%5D/blog/cursor-mcp-json-guide) . For a full deployment walkthrough once your app is ready, see[How to Deploy a Cursor App to Production](https://blink.new/blog/%5BREDACTED%5D/blog/deploy-cursor-app-production) .


## Build Full-Stack Apps With Your Cursor Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/blog/%5BREDACTED%5D/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   [REDACTED]/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app using Blink and host it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/blog/%5BREDACTED%5D/cloud)


No.` npx skills add \[REDACTED\]/blink-plugin` writes the MCP config automatically. You never touch` ~/.cursor/mcp.json` or any project-level config file. Blink handles the server registration, tool listing, and auth token injection during install.


Cursor 2.6+ (March 2026) is recommended. It ships improved MCP reliability and the new MCP Apps feature for interactive tool UIs. Blink works on earlier versions but tool-call stability improves on 2.6+. Check your version in Cursor → About Cursor.


Yes. Blink's MCP tools work with any MCP-compatible agent. The` npx skills add` command also supports Claude Code, Codex, and Windsurf. See[How to use Blink with different AI coding agents](https://blink.new/blog/%5BREDACTED%5D/blog/cursor-vs-claude-code) for a comparison of setup across tools.


Blink has a free tier for exploration. Paid plans start at $15/month and include database, auth, storage, backend, and hosting in one bill. This replaces Supabase ($25/mo) + Vercel ($20/mo) + Auth0 ($23/mo) — typically saving $50+ per month on the same stack. See[blink.new/cloud](https://blink.new/blog/%5BREDACTED%5D/cloud) for current pricing.


Blink adds 62 MCP tools and 14 skills to your Cursor agent. The tools cover database operations, auth management, file storage, backend deployment, and project administration. The 14 skills tell your agent how to sequence these tools to build complete features — not just call individual operations.
