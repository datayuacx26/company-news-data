---
schema_version: "1.0.0"
document_id: "e56b1fc2a622fad1958d64ee49652a4db1482e6c89664f03fecb071cc386ac49"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-deploy-cursor-app-production"
published_at: "2026-06-03T13:04:22+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:d581506d15a65857733c3097e2e975b2c24209b841ce697d1b31499fdb7674e4"
---

# How to Deploy a Cursor App to Production

## Step-by-Step: From Cursor Code to a Live URL


1


#### Install the Blink plugin


Run` npx skills add blink-new/blink-plugin` in any terminal. The CLI downloads 14 skills and auto-configures` .cursor/mcp.json` . No JSON editing required. Restart Cursor after it completes.


2


#### Log in to Blink


Run` blink login` . A browser tab opens. Sign in or create a free account — no credit card required. Your API key saves automatically and the MCP connection activates.


3


#### Tell your agent what you built


Open a new Cursor chat. Describe your app specifically: "I have a React frontend with user auth, a Postgres schema with users and posts tables, and file upload routes. Set this up on Blink and give me a live URL."


4


#### Agent provisions infrastructure


Your agent calls the Blink MCP tools in sequence. It creates the database schema, configures auth flows with your callback URLs, deploys the backend routes, provisions file storage, and registers a live hosted URL — all from that one prompt.


5


#### Get your live URL


Within 2 minutes, your agent returns a live HTTPS URL. Custom domain? Add it with one more line: "Set up` myapp.com` as the domain for this project." The agent registers it and updates the DNS configuration.


## The Real Cost: DIY Stack vs. Blink


The $295/month figure is the full production-grade stack — the one you'd need if you were building something real and needed to cover every use case. Here's every line item:


DIY (12 Services) DIY (Minimal 4) Blink Cloud


**Frontend hosting** Vercel Pro $20/mo Vercel Pro $20/mo Included


**Database** Supabase Pro $25/mo Supabase Pro $25/mo Included


**Auth** Clerk Pro $35/mo Auth0 $35/mo Included


**File storage** AWS S3 + CDN $10/mo AWS S3 $10/mo Included


**Backend API** Railway $20/mo — Included


**Queue + email + realtime** $75/mo — Included


**Monitoring + analytics** $50/mo — Included


**Total monthly** **~$295/mo** **~$90/mo** **Free / $24.95 paid**


**Accounts to manage** 12 4–5 1


**Setup time** 6+ hours 3 hours 2 minutes


The minimal 4-service path costs ~$90/month. That's still 4 accounts, 3 hours of config, and all four failure modes above waiting to find you.


The full 12-service path exists for production apps with specific requirements — compliance mandates for a particular AWS region, a team that already manages Kubernetes and wants container-level control, or an organization with an existing Supabase enterprise contract. For those situations, the DIY path makes sense.


For the 90% of Cursor-built apps that don't have those requirements, Blink is faster, cheaper, and doesn't require a DevOps background.


The full DIY stack at $295/mo across 12 services vs. Blink Cloud's one-account, free-to-start model


Blink


## Blink vs. Northflank: Two Deployment Philosophies


[Northflank's deployment guide for Cursor apps](https://northflank.com/blog/how-to-deploy-vibe-coded-cursor-apps-to-production) is well-written. They understand the vibe-coding deployment problem. Their platform handles CI/CD, preview environments, and container orchestration. Teams with existing Docker expertise and PR-heavy workflows get real value from their approach.


But their path starts with Docker.


You write a Dockerfile. You connect a GitHub repository. You configure environment variables in the Northflank dashboard. You add Supabase for the database. You add Clerk for auth. You now have three accounts, one Dockerfile to maintain, and a GitHub Actions workflow to debug.


Here's the direct comparison:


Northflank Blink Cloud


**Deploy method** Dockerfile → GitHub → Northflank dashboard` npx skills add blink-new/blink-plugin` + describe to agent


**Database included** No (add Supabase or PlanetScale separately) Yes — provisioned automatically


**Auth included** No (add Clerk or Auth0 separately) Yes — included


**File storage** MinIO addon Yes — included


**Agent-native (MCP tools)** No Yes — 62 tools


**Setup time** 20–40 minutes 2 minutes


**Free tier** 2 services, 1 database 1 full project (database + auth + storage + hosting)


Northflank wins on Docker-native flexibility, custom container runtimes, and BYOC (bring your own cloud) for enterprise deployments. If your team writes Dockerfiles and wants fine-grained control over the container lifecycle, their platform is a legitimate choice.


Blink wins when you want the agent to handle everything — no Dockerfile, no GitHub Actions, no dashboard configuration. The 62 MCP tools let your Cursor agent provision a complete stack from a plain-language description.


From Cursor code to live production URL — the 2-minute path with Blink Cloud


Blink


## Deploy Your Cursor App Into Production With Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Deploy my Cursor app to production — create the database, configure auth, and give me a live URL."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Run` npx skills add blink-new/blink-plugin` and` blink login` in your terminal. Restart Cursor, then ask your agent to deploy your app to production. It provisions database, auth, backend, and hosting automatically. You get a live HTTPS URL in under 2 minutes — no separate Vercel, Supabase, or Auth0 account required.


No. Cursor writes code — it doesn't provision infrastructure. You still need somewhere to run a database, authenticate users, and host the app. Blink Cloud fills that gap with 62 MCP tools your Cursor agent calls directly, handling the full stack from a single natural-language prompt.


No. Vercel handles frontend hosting, but you'd still need Supabase for the database and Clerk for auth — three accounts, three dashboards, and the four failure modes covered above. Blink Cloud includes hosting, database, and auth in one platform with one bill.


Blink Cloud includes a SQL database that your agent provisions automatically from your schema description. No Supabase account needed. The agent handles schema creation and migrations. For apps that don't need a specific Postgres version or region, Blink's built-in database is the fastest path.


With Blink, ask your agent: "Set up auth with email/password and Google OAuth for this project." It configures the auth flows, registers callback URLs against your live domain, and connects everything to your database — no Auth0 dashboard, no Clerk configuration, no manual OAuth application setup.


The minimal DIY stack (Vercel + Supabase + Clerk + S3) runs about $90/month across 4 accounts. The full production stack with queuing, email, and monitoring runs $295/month across 12 accounts. Blink Cloud is free to start — no credit card required. Paid plans start at $24.95/month and include database, auth, hosting, storage, and backend in one bill.


Yes, with the right MCP tools installed. Run` npx skills add blink-new/blink-plugin` to give your Cursor agent 62 infrastructure tools. It can then provision and deploy your entire stack from a single natural-language prompt — no manual configuration between steps. Works with Claude Code, Codex, and Windsurf too.


Blink Cloud deploys a Hono serverless backend automatically. Your agent writes the API routes, Blink handles the hosting and routing. No Railway service setup, no Docker, no manual deploy pipeline. The backend URL is ready and live within the same 2-minute deployment flow.


Yes. Blink Cloud gives you database, auth, and file storage in one platform — the three things most teams use Supabase for. Your Cursor agent provisions all three automatically from a prompt description. See the[step-by-step Cursor deployment guide](https://blink.new/blog/deploy-cursor-project) for a detailed walkthrough of the full provisioning flow.


With Blink Cloud, 2 minutes from prompt to live URL with SSL. The DIY path — Vercel + Supabase + Clerk + AWS S3 — takes 3–6 hours the first time you do it, including the four failure modes (env vars, auth callbacks, schema drift, cold starts) that typically require a second debugging session. Read[what to do after Cursor writes your code](https://blink.new/blog/deploy-cursor-app-production) for the full deployment checklist.
