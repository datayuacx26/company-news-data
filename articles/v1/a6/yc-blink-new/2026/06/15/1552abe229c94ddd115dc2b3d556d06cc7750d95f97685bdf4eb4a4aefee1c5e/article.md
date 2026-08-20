---
schema_version: "1.0.0"
document_id: "1552abe229c94ddd115dc2b3d556d06cc7750d95f97685bdf4eb4a4aefee1c5e"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-app-to-production"
published_at: "2026-06-05T00:15:29+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:90febde6246e3a75d03b1b64c082fecf4d67d28433e2f90dae60a5be7915958c"
---

# How to Deploy Your Cursor App to Production

## Setting Up Cursor With Blink


1


#### Install the Blink plugin for Cursor


Run this command in your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads 14 skills and auto-configures the Blink MCP server for Cursor. No manual` mcp.json` editing required.


The plugin is also available directly from the Cursor Marketplace — one click, no commands needed.


2


#### Log in to Blink


```text
blink   login
```


A browser window opens. After authentication, your API key saves automatically and the MCP connection activates. Your Cursor agent now has access to 62 MCP tools across Blink's full infrastructure stack.


3


#### Ask Cursor to build and deploy your app


Open Cursor and type:


> "Build me a full-stack app and host it on Blink — database, auth, and deploy included."


Your agent provisions database, auth, backend, and hosting automatically. You get a live URL at the end.


For an existing project, you can ask:


> "Deploy this app to Blink Cloud. Set up the database and auth."


## What the Blink Plugin Gives Your Agent


Installing the Blink plugin gives Cursor 62 MCP tools and 14 skills. These cover the full production stack:


**Infrastructure tools** cover database provisioning, schema management, auth configuration, file storage setup, and environment variable management.


**Deployment tools** handle domain setup, SSL certificates, build configuration, and live URL generation.


**Developer experience tools** provide real-time logs, error monitoring, and deployment history — all queryable from Cursor chat.


This is why the ask-and-deploy flow works: your agent has the actual tools to take action, not just generate config files for you to paste elsewhere.


Blink Cloud is free to start. You only pay when you deploy. No credit card required for the first project.


For more context on Cursor's position among coding tools, see[Cursor alternatives](https://blink.new/blog/cursor-alternatives) and[Cursor vs Claude Code](https://blink.new/blog/cursor-vs-claude-code) .


## Common Deployment Questions Cursor Users Hit


**Environment variables** : Blink manages these automatically. Ask your agent to "add an env variable called X with value Y" and it handles the rest.


**Custom domains** : Ask Cursor to "connect myapp.com to this deployment" and the agent provisions the DNS records and SSL certificate.


**Database migrations** : Blink's database tools let your agent run migrations directly from chat. No separate migration runner.


**Staging environments** : Ask Cursor to "create a staging environment" and Blink provisions an isolated copy of your production stack.


## Build Your Full-Stack App With Cursor and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask Cursor:


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


Add full-stack infrastructure to your coding agent in one command:` npx skills add blink-new/blink-plugin` →[blink.new/cloud](https://blink.new/cloud)


## Frequently Asked Questions


No. Running` npx skills add blink-new/blink-plugin` auto-configures the Blink MCP server for Cursor. The plugin handles all MCP setup — you don't touch` mcp.json` . If you prefer the Cursor Marketplace, the one-click install there also handles configuration automatically.


Blink Cloud supports full-stack web apps with database, auth, file storage, and custom backends. It works with any app your Cursor agent can build — Next.js, React + API routes, Node.js backends, and more. It's not for mobile app binaries or static HTML sites.


Yes. The Blink plugin works with any coding agent that supports MCP, including Claude Code. For more on the Cursor vs Claude Code decision, see[Cursor vs Claude Code](https://blink.new/blog/cursor-vs-claude-code) . Both agents use the same 62 MCP tools from Blink.


Blink keeps database backups separate from app deployments. Deleting an app deployment does not automatically delete the database. You manage database lifecycle separately from the app. Ask your agent to "delete the database for project X" if you want to remove it explicitly.


Vercel and Supabase are both excellent individual services. The difference is integration depth and agent control. With Blink, your Cursor agent has 62 tools that can operate the full stack — not just deploy frontend code. There's also one bill, one dashboard, and no manual connection between services. See[best vibe coding tools](https://blink.new/blog/best-vibe-coding-tools) for a broader comparison.
