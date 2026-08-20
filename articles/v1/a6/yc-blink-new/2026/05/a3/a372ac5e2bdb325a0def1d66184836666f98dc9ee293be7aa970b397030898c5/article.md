---
schema_version: "1.0.0"
document_id: "a372ac5e2bdb325a0def1d66184836666f98dc9ee293be7aa970b397030898c5"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-app"
published_at: "2026-05-18T00:18:22+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:e502d4f170226cbf1127e3ff49228396fbc6edffd992a1acb74e0fac7aac0734"
---

# How to Deploy a Cursor Project to Production: Database, Auth, and Hosting in 2 Minutes

## Step-by-Step: Deploy Your Cursor App


1


#### Install the Blink plugin


Run from any directory in your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


The CLI installs 14 skills and auto-writes the correct MCP configuration to` .cursor/mcp.json` . No manual JSON editing. No digging through Cursor settings.


2


#### Authenticate with Blink


```text
blink   login
```


A browser tab opens. Sign in or create a free Blink account — no credit card required. Your API key is saved locally. The MCP server connects immediately.


3


#### Verify the MCP connection


Open Cursor → Settings → MCP. The Blink server should show a green dot. If it shows grey, restart Cursor — MCP config changes require a full restart to take effect.


4


#### Ask Cursor to deploy your app


Open a Cursor chat and describe what you need:


> "Deploy this app on Blink Cloud. Set up a Postgres database, add email/password authentication with Google sign-in, configure file storage for user uploads, and deploy to a live URL."


Your agent uses Blink's 62 MCP tools to execute each step. Watch the terminal — it creates database tables, runs migrations, configures auth, deploys the backend, and returns a Blink subdomain URL.


5


#### Connect a custom domain (optional)


Tell Cursor: "Connect my domain \[yourdomain.com\] to this Blink project." It configures DNS and SSL automatically. No manual certificate management.


Cursor app deployed on Blink in 2 minutes — database, auth, and hosting included in one platform


Blink


*Cursor app deployed on Blink in 2 minutes — database, auth, and hosting included in one platform*


## What Cursor Can Do With Blink Connected


Once the plugin is installed, your agent can provision and manage real production infrastructure through natural language. These all work:


**Database work:** "Create a users table with email, hashed password, and created_at. Add a posts table with a user_id foreign key." Cursor writes the migration, runs it against your Blink Postgres instance, and updates your schema.


**Auth setup:** "Add Google and GitHub OAuth to the existing email/password flow. Redirect to /dashboard after sign-in." Cursor configures the providers, sets up session handling, and adds callback routes.


**File uploads:** "Add a profile picture upload endpoint. Store files in Blink storage and save the URL in the users table." Cursor writes the handler, configures the storage bucket, and updates the database — no AWS account required.


**Deployment updates:** "Deploy the latest changes and run any pending migrations." One instruction, everything updates.


The difference from the old workflow: your agent isn't just writing code locally. It's using Blink's MCP tools to execute against real infrastructure in real time. No context-switching, no copy-pasting API keys between dashboards. You stay inside Cursor.


For a full walkthrough of the MCP setup, see[Cursor MCP Setup: Connect Your Agent to Real Infrastructure](https://blink.new/blog/cursor-mcp-setup) .


## Cost Comparison: Manual Stack vs Blink


Manual Stack Blink


**Database** Supabase Pro ($25/mo) ✅ Included


**Auth** Clerk ($25/mo) ✅ Included


**Hosting** Vercel Pro ($20/mo) ✅ Included


**File storage** AWS S3 ($5-15/mo) ✅ Included


**Backend runtime** Railway / Render ($6-20/mo) ✅ Included


**Monthly total** $81-105+/mo One bill


**Setup time** 3-6 hours Under 2 minutes


**Accounts to manage** 5-6 1


**MCP tools for Cursor** 0 62


The comparison isn't "Blink vs Vercel." It's "Blink vs the full list of services a production app actually needs." Manual stack pricing is conservative — add Stripe for payments and you cross $100/month easily before your first paying customer.


For more on choosing your AI coding tool, see[Cursor vs Claude Code: Which Should You Use?](https://blink.new/blog/cursor-vs-claude-code)


## Ask Cursor to Deploy Your Next App on Blink


Install[14 skills](https://blink.new/docs/cloud/tools/skills) :


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask Cursor:


> "Build me a full-stack \[app\] and host it on Blink Cloud."


Cursor provisions database, auth, backend, and hosting automatically.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


No. Blink provisions a Postgres database automatically when you deploy through the Cursor plugin. No Supabase account, no connection string setup, no separate database dashboard to manage. Your Cursor agent creates tables, runs migrations, and queries data directly through Blink's MCP tools — all from inside your IDE.


Blink Cloud includes a PostgreSQL database, user authentication (sign-up, login, sessions, password reset, OAuth), S3-compatible file storage, a Hono-powered backend runtime, global hosting with a Blink subdomain, and custom domain support with automatic SSL. It's a full production stack — not a sandbox or a preview environment that expires.


Yes. Blink works with React, Next.js, Vite, SvelteKit, and any other framework Cursor generates. The Blink plugin gives Cursor 62 MCP tools that provision real infrastructure regardless of your frontend framework. If your app includes a backend (Express, Hono, Fastify), Blink deploys that too — not just the static frontend.


Blink has a free tier with no credit card required — includes a Blink subdomain, database, and auth. Paid plans start at $20/month and cover everything under one bill. For comparison, a manual stack with Vercel Pro + Supabase Pro + Clerk typically runs $70-130/month across five separate services, plus 3-6 hours of setup time per project.


Yes. After deploying, tell Cursor: "Connect my custom domain \[yourdomain.com\] to this Blink project." Cursor uses Blink's MCP tools to configure DNS records and provision an SSL certificate automatically. Your app serves from your domain with HTTPS — no manual certificate management, no waiting for DNS to propagate while debugging cert errors.
