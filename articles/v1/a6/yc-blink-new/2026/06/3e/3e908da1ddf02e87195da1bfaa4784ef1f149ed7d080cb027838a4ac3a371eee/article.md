---
schema_version: "1.0.0"
document_id: "3e908da1ddf02e87195da1bfaa4784ef1f149ed7d080cb027838a4ac3a371eee"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/blink-plugin-cursor-setup"
published_at: "2026-06-10T12:29:11+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:7b56da73b07e7f026c585eb25f3064936d510cbdec849c91d521ae50dad71beb"
---

# How to Add Blink to Cursor: Full-Stack Infrastructure in One Command

## The 2-Command Setup


### Command 1: Install the Blink Plugin


```text
npx   skills   add   blink-new/blink-plugin
```


This command does four things in one shot:


1. Downloads **14 Blink skills** to your agent's skills directory
2. Writes the MCP server configuration to` ~/.cursor/mcp.json` automatically
3. Installs the` blink` CLI globally
4. Makes **62 MCP tools** available inside Cursor


You never open or edit` mcp.json` . The CLI handles the entire configuration.


### Command 2: Authenticate


```text
blink   login
```


Your browser opens to the Blink login page. Sign in or create a free account. Your API key is saved locally and written into the MCP config.


Return to the terminal. Done.


### Verify the Connection


Open Cursor Settings → Features → MCP. Look for **blink** with a green **Connected** status.


Or ask Cursor directly:


```text
What Blink skills do you have?


```


Cursor lists all 14 skills with descriptions. If you see the list, MCP is connected and working.


Blink Cloud: from install command to production URL in under 2 minutes


Blink


## What You Can Ask Cursor to Do Now


Once connected, these are real prompts that work:


**Provision a database:**


```text
Create a PostgreSQL database for my app. Set up a users table with id,
email, created_at, and role. Add an index on email.


```


**Enable authentication:**


```text
Set up user authentication with Google OAuth and email/password sign-in.
Add a login page with a redirect after sign-in.


```


**Deploy to production:**


```text
Deploy this Next.js app to Blink Cloud. Give me the production URL.


```


**Create a backend API:**


```text
Create a POST /api/save-feedback endpoint that writes to the database.
Return the saved record with a 201 status.


```


**Full end-to-end:**


```text
Build a full-stack feedback collection app with a PostgreSQL database,
user auth, and a clean UI. Deploy it to Blink. Give me the URL when live.


```


Your agent calls Blink's MCP tools in sequence — create project, provision database, enable auth, write code, deploy — and returns a production URL. No Vercel config. No Supabase. No Auth0.


## Alternative: Cursor Marketplace


If you prefer clicking over typing:


1. Open Cursor
2. Go to the **Marketplace** tab
3. Search for **Blink**
4. Click **Install**


One click. OAuth authentication in the browser. Same 62 tools and 14 skills — no terminal required.


## What the 14 Skills Include


Each skill is a structured workflow your agent follows. They prevent your agent from hallucinating non-existent tool names or skipping infrastructure steps.


Skill area What your agent can do


Project management Create, manage, and configure Blink projects


Database Provision PostgreSQL, run migrations, query data


Authentication Enable OAuth providers, add sign-in pages


Frontend deploy Ship React, Next.js, Vue, Svelte, Astro


Backend Create serverless API routes (Hono framework)


Storage Configure file upload with global CDN


Real-time Add presence, channels, live updates


Domains Connect custom domains with auto-SSL


Secrets Manage environment variables


AI Gateway Configure 200+ model access inside your app


Background jobs Queue tasks, run scheduled workflows


OAuth connectors 38+ pre-built OAuth integrations


Database migrations Schema version control, safe upgrades


Monitoring Error tracking, performance metrics


The agent reads these skills and knows exactly how to use each Blink capability — in the right order, with the right parameters.


## What You're Replacing


A standard production setup using separate services:


Service Monthly cost


Vercel Pro (hosting) $20


Supabase Pro (database) $25


Clerk Pro (auth) $35


AWS S3 (storage) $10


Task queue $25


Email service $20


**Total** **$135+/month**


That's 6 accounts, 6 dashboards, 6 sets of API keys to wire together — manually, for every project.


Blink Cloud starts free. One account, one dashboard, everything included. No setup between services. Paid plans from $24.95/month for custom domains and unlimited projects.


Blink Cloud pricing: one platform vs 6+ service accounts and $135+ per month of separate subscriptions


Blink


## Build Full-Stack Infrastructure Into Your Cursor Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app with a database and user authentication, hosted on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


No. Running` npx skills add blink-new/blink-plugin` writes the MCP configuration to` ~/.cursor/mcp.json` automatically. You never open or edit the file. If you need to troubleshoot, run` blink login` to refresh authentication — that's the only command you'll ever need after the initial install.


Node.js 18 or higher. Run` node --version` to check. If you need to update, download the current LTS from nodejs.org. The` npx` command that runs the installer requires Node.js 18+ to work correctly.


Yes. Run the commands in PowerShell or Windows Terminal. The Blink CLI creates the config at` %USERPROFILE%\\.cursor\\mcp.json` on Windows automatically. The setup process is identical to macOS and Linux.


The result is identical — same 62 tools, same 14 skills, same infrastructure access. The Marketplace method handles everything in-browser with OAuth. The terminal method is faster if you're already in a workflow. Both auto-configure MCP without manual JSON editing.


Yes. Run the same two commands —` npx skills add blink-new/blink-plugin` +` blink login` . The CLI detects Claude Code and writes skills to` ~/.claude/skills/` . Same 62 MCP tools, same 14 skills, same database, auth, and hosting access.


Yes. Blink Cloud's free tier includes one project with a PostgreSQL database, user authentication, file storage, and production hosting — no credit card required. This is enough to build and deploy a full working app. Paid plans from $24.95/month add custom domains and unlimited projects.


Every project gets a PostgreSQL database provisioned automatically. Your agent creates tables, runs migrations, and queries data through MCP tool calls. No Supabase account, no RDS configuration, no connection string to manage manually.


Vercel + Supabase are two separate platforms you wire together yourself. Blink Cloud is one platform your Cursor agent controls directly via MCP — the database, auth, hosting, storage, and backend are all provisioned by tool calls, not by you logging into dashboards. No configuration between services. No API keys to copy-paste. One bill instead of six.
