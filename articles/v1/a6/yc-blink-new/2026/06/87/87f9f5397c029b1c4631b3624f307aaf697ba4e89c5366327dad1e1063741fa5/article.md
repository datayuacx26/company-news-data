---
schema_version: "1.0.0"
document_id: "87f9f5397c029b1c4631b3624f307aaf697ba4e89c5366327dad1e1063741fa5"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/add-blink-to-cursor"
published_at: "2026-06-12T13:55:40+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:9bbf458fcc4bef76e48c98a3e02b716cdb1e0ada495eb0837d8c80746056bb92"
---

# How to Add Blink Cloud to Cursor: Full-Stack Infrastructure in 2 Commands

## Option 2: Install from the Cursor Marketplace


If you prefer not to open a terminal, install Blink directly from inside Cursor.


Open Cursor and go to **Settings → Extensions → Marketplace** . Search for **Blink** . Click **Install** .


The marketplace install configures the same MCP connection as the CLI method. Same 14 skills, same 62 tools — no command line, no JSON editing.


Both install methods result in an identical setup. Use the CLI if you're already in a terminal. Use the marketplace if you're inside Cursor and want a single click.


## What you can now ask Cursor to do


Once the plugin is installed, these prompts work immediately — Cursor uses Blink's MCP tools to execute each one, not just suggest steps.


**Provision a database for a new project:**


> "Create a Postgres database for this project and generate the schema for a user accounts table."


**Add authentication to an existing app:**


> "Add email and password authentication to this app with sign-up, login, and protected routes."


**Deploy to a live URL:**


> "Deploy this app to Blink Cloud and give me the production URL."


**Add file upload support:**


> "Add the ability to upload profile photos using Blink storage. Store the URL in the user record."


**Full-stack app from a single prompt:**


> "Build a task management app with user accounts, a Postgres backend, file attachments, and deploy it live on Blink Cloud."


The agent doesn't hand you a to-do list. It runs the commands, provisions the resources, and returns a working URL.


## Before vs. after: the same project without and with Blink


This is what the infrastructure side of a typical full-stack app looks like before and after installing the Blink plugin.


Component Manual setup (before Blink) With Blink plugin


Database Configure Supabase (~$25/mo) ✅ Auto-provisioned


Authentication Set up Clerk (~$25/mo) ✅ Built-in


File storage Create S3 bucket + IAM roles ✅ Included


Deploy Configure Vercel project ✅ One command


Custom domain Vercel + DNS configuration ✅ Managed


Setup time 3–6 hours 2 minutes


Monthly cost $70–130+ From $20/mo


The code your agent writes doesn't change. What changes is everything underneath it: one platform instead of four, one monthly bill instead of four, two commands instead of six hours of setup.


## Start Using Blink Cloud in Cursor Right Now


Install the Blink plugin in one command —[14 skills](https://blink.new/docs/cloud/tools/skills) that give Cursor full-stack capabilities:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


No manual mcp.json editing. Then ask Cursor:


> "Build a full-stack app with user auth, a Postgres database, and deploy it on Blink Cloud."


Cursor provisions database, auth, backend, and custom domain automatically.


[See all 62 Blink MCP tools →](https://blink.new/cloud)


Deploying a full-stack app with Blink in Cursor — from prompt to production in minutes


Blink


## Frequently asked questions


Install first, then run` blink login` to create your account. It takes about 30 seconds — no credit card required for the free tier. The plugin installs and configures MCP before you log in.


No.` npx skills add blink-new/blink-plugin` appends Blink to your existing MCP config. It does not overwrite or remove any other MCP connections you have set up.


Both methods install the same 14 skills and configure the same MCP connection. The CLI is faster if you're already in a terminal. The marketplace install is a single click inside Cursor. End result is identical.


For most projects, yes. Blink Cloud includes Postgres database, user authentication, object storage, a serverless backend, deploy, and custom domain in one platform. If you have existing infrastructure you want to keep, Blink does not force migration — you can use it alongside what you already have.


Blink Pro starts at $20/month. That's less than Supabase alone — and it includes database, auth, storage, hosting, and serverless backend combined. Current pricing is at[blink.new/cloud](https://blink.new/cloud) .


Yes.` npx skills add blink-new/blink-plugin` works with any MCP-compatible agent — Cursor, Claude Code, and Codex all support MCP. Install once and the plugin is available across all your MCP-enabled tools.
