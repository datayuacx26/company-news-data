---
schema_version: "1.0.0"
document_id: "943a4f26435f9dbf6e681df73e73af97ea5b63c83524df977d0dbe54f6c0c3ef"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-blink-full-stack-deploy"
published_at: "2026-04-26T12:17:00+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:52:06.558365+00:00"
content_hash: "sha256:dce5439d315b03468a00758e53d39db2046270a4c440b4bf1803728297a7a627"
---

# Claude Code + Blink: Build the Full Stack, Not Just the Frontend

## Step-by-Step Setup


1


#### Install the Blink plugin


In your terminal (works in any project directory):


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


The first command downloads 14 skills and auto-configures MCP. The second opens a browser tab, you log in with your Blink account, and your API key is saved automatically. No manual` mcp.json` editing required.


2


#### Open Claude Code and build your app


Start a Claude Code session in your project directory. Now Claude Code knows about Blink and can use it:


```text
claude


```


Then give it a build prompt:


> "Build a full-stack task management app with user accounts, projects, tasks, and due dates. Use Blink for the database, auth, and hosting. Make it production-ready."


Claude Code generates the code AND provisions your Blink infrastructure simultaneously. You see it creating database tables via the Blink MCP tool, configuring auth rules, writing your app code, and deploying to production.


3


#### Get your production URL


When Claude Code finishes, ask it:


> "What's my production URL?"


Claude Code returns your live app URL from Blink's deployment. It's immediately accessible. Share it, embed it, test it.


4


#### Iterate from the same session


Keep working in the same Claude Code session:


> "Add a team collaboration feature. Multiple users should be able to share a project and see each other's tasks in real time."


Claude Code updates the database schema, adds the real-time logic, and re-deploys. Migrations run automatically. No downtime.


## The Before/After Infrastructure Comparison


DIY Stack Blink


**Database** Supabase (separate account, connection limits) Included, auto-provisioned


**Auth** Clerk or Auth0 (separate account, $25+/mo) Included, zero config


**File storage** S3 bucket (AWS account, IAM roles) Included


**Hosting** Vercel (separate account, cold starts on free tier) Included


**Setup time** 2-4 hours per project 2 commands


**Monthly cost (dev project)** $40-80+ Free to start


**Accounts to manage** 4+ 1


## What Claude Code Can Now Do With Blink


With the Blink plugin installed, Claude Code gains 14 skills and 62 MCP tools:


```text
blink_create_table        — provision a new database table
blink_query               — read from the database
blink_insert              — write to the database
blink_deploy_function     — deploy server-side logic
blink_get_auth_token      — retrieve auth session
blink_set_auth_rules      — configure access control
blink_upload_file         — store files
blink_get_public_url      — get file URLs
blink_list_tables         — inspect current schema
blink_run_migration       — apply schema changes safely
...and 52 more


```


Claude Code doesn't just generate code that *would* provision these things — it actually provisions them in your account during the session.


## Using Blink With Cursor Instead of Claude Code


The same plugin works with Cursor, Codex, and any MCP-compatible coding agent:


1. Open Cursor's MCP settings
2. The Blink plugin is already configured from` npx skills add`
3. Ask Cursor: "Build me \[app\] and deploy it on Blink"


All 62 MCP tools are available to Cursor. The workflow is identical.


For Cursor-specific setup, see[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup-guide) .


## Build This Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app with user auth, a database, and file uploads, and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Yes. The Blink plugin uses the MCP standard (Model Context Protocol), which Claude Code supports natively since its GA release. Any version of Claude Code that supports MCP works with the Blink plugin.


If you have an existing database (Supabase, Railway Postgres, PlanetScale), you can point Blink at it by configuring the connection string in Blink's settings. The auth and hosting are still Blink; only the database is external. This is useful for migrating from an existing stack incrementally.


Your app goes into a paused state but your data is preserved. You can export your entire database as a SQL dump at any time from the Blink dashboard. Your code is in your local repository — nothing is Blink-proprietary. Migrating to a self-hosted stack requires reconnecting the database and auth layers, but your application code is fully portable.


Yes. Blink's infrastructure is production-grade: 99.9% uptime SLA, automatic backups, TLS certificates, DDoS protection. Starter plans include everything needed for production apps with up to 10K monthly active users. Pro plans scale to 100K+ MAU.


Add your custom domain in the Blink dashboard. Add the provided CNAME record to your DNS. TLS certificate is provisioned automatically within 5 minutes. Your app serves from your domain immediately. No Cloudflare configuration or certificate management needed.
