---
schema_version: "1.0.0"
document_id: "6bed9cd63a79a36797ef369b314ef422d1cd4499429764d3a95c571801fe093f"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-add-blink-to-cursor"
published_at: "2026-06-07T12:29:01+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:9fc5a94017c2e964703750e4158d691119975cc0c9ea0b2c7f1329cafdca9897"
---

# How to Add Blink to Cursor: Full-Stack Infrastructure in One Command

## Why Not Just Use Supabase + Vercel + Auth0?


The standard vibe coding stack:


Service Monthly cost Setup time Accounts


Supabase $25/mo 30 min 1


Vercel $20/mo 20 min 1


Auth0 $23/mo 45 min 1


Resend (email) $20/mo 15 min 1


**Total** **$88/mo** **110 min** **4**


The Blink alternative:


Service Monthly cost Setup time Accounts


**Blink** **Free to start** **2 min** **1**


Blink includes database, auth, backend, hosting, and email in one platform. One bill. One account. Two commands to connect your Cursor agent.


## What the 14 Skills Do


The Blink plugin installs 14 specialized skills across your full stack:


Category Skills What they do


Database 3 skills Provision DB, run migrations, query tables


Auth 2 skills Set up email/OAuth auth, manage users


Deploy 3 skills Deploy backend, set up domain, manage env vars


Storage 2 skills File uploads, CDN, presigned URLs


Backend 2 skills Create API endpoints, handle webhooks


Dev tools 2 skills Log viewer, health checks


Your Cursor agent has access to all 14. You do not need to know which skill handles which task — the agent selects the right one automatically.


## Verify the Setup


After` blink login` , test the connection inside Cursor:


Open a chat session and type:


> "Create a new Blink project called 'test-app' and provision a database."


If Blink is connected, Cursor responds with the project URL and database connection details. The whole thing takes under 30 seconds.


## Cursor Marketplace Alternative


If you prefer a one-click install, search for "Blink" in the Cursor Marketplace:


Cursor → Settings → Extensions → search "Blink"


Click Install. Then authenticate. The Marketplace install and the` npx skills add` command produce identical results — use whichever fits your workflow.


## The Agent Prompt That Builds a Production App


Here is a prompt that builds a complete app in one Cursor session after Blink is connected:


```text
Build a job board app with the following features:
- Users can post jobs (title, company, description, salary range, remote/hybrid/on-site)
- Anyone can browse and filter jobs by category, salary, and location type
- Job posters sign up with email and can manage their posts
- Use Blink for database, auth, and hosting
- Deploy it and give me the live URL


Use TypeScript and Tailwind CSS.


```


Cursor will build the full app, set up the database schema, configure auth, and deploy — without you doing any infrastructure work.


Yes. The npx skills add command configures both Claude Code and Cursor if both are installed. The 14 skills work identically in both environments. If you use both tools, run the setup command once and both agents get the Blink connection automatically.


Blink starts free with no credit card required. If you upgrade to a paid plan and later cancel, your apps continue running until the end of the billing period. Blink provides data export tools so you can migrate your database and code to any other platform. No lock-in.


The Blink plugin connects to Blink-provisioned databases. If you want to connect an existing PostgreSQL database (hosted on RDS, Supabase, or elsewhere), use the PostgreSQL MCP server directly. See the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup-guide) for how to configure that alongside Blink.


No manual mcp.json editing is required. The npx skills add command handles configuration automatically — it writes the necessary MCP config and the blink login command saves the authentication. If you want to inspect what was configured, check ~/.cursor/mcp.json after setup. The Blink entry will be there, but you should not need to edit it manually.
