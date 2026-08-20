---
schema_version: "1.0.0"
document_id: "30ecdb1d56337331c7a02b3f181b0fec75d8ebe961f7cf298d31f0c0cbff428e"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-deploy-what-claude-code-builds"
published_at: "2026-05-02T00:26:47+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:46.128682+00:00"
content_hash: "sha256:0916aeef654874862bd0430f153b5c0ae6ed0756b872b69828993ddc9ff21f2f"
---

# How to Deploy What Claude Code Builds: The Infrastructure Guide

## Step-by-Step: The DIY Deployment Path


If you want to understand exactly what's involved before choosing a path, here's every step.


1


#### Deploy the frontend (Vercel)


Push your repo to GitHub. Connect it to Vercel. Set environment variables under Project Settings → Environment Variables. Each variable must be entered manually — no` .env` import.


```text
npx   vercel   deploy   --prod
```


Set` NEXT_PUBLIC_API_URL` to point to your backend. You'll figure out that URL in step 2.


2


#### Host the backend separately (Railway or Render)


Vercel serverless functions have a 10-second execution limit. Anything longer — file processing, webhooks, background jobs — needs a persistent backend. Deploy Express or FastAPI to Railway or Render.


```text
railway   up
```


Copy the generated URL. Go back to Vercel. Update` NEXT_PUBLIC_API_URL` . Redeploy.


3


#### Set up the database (Supabase or Neon)


Create a Supabase project. Navigate to Settings → Database → Connection string. Copy the` DATABASE_URL` .


Run your migrations:


```text
npx   prisma   migrate   deploy
```


Add` DATABASE_URL` to both Railway (backend) and Vercel (frontend, if you query DB from API routes). Setting up just the database layer for a Claude Code project[took a real developer 45 minutes](https://nielsberglund.com/post/2024-01-07-interesting-stuff---week-1-2024/) in a documented walkthrough.


4


#### Wire up auth (Supabase Auth or Clerk)


Create a Clerk account. Get your` CLERK_PUBLISHABLE_KEY` and` CLERK_SECRET_KEY` . Add them to Vercel. Add them to Railway.


Configure JWT templates to match what your backend expects. Update CORS settings to allow your Vercel domain.


```text
CLERK_SECRET_KEY  =  sk_live_...
CLERK_PUBLISHABLE_KEY  =  pk_live_...
ALLOWED_ORIGIN  =  https://your-app.vercel.app
```


5


#### Manage environment variables across platforms


You now have three places where environment variables live: Vercel, Railway, and locally. They must stay in sync manually. Any mismatch causes a production error that's painful to debug.


Some teams add Doppler or Infisical to manage this. That's a seventh account.


6


#### Connect a custom domain


Buy a domain. Point DNS A records at Vercel. Add a CNAME for your Railway backend service. Wait for SSL provisioning. Update CORS settings again with the new domain.


7


#### Set up monitoring (Sentry)


Create a Sentry account. Install the SDK in both frontend and backend:


```text
npm   install   @sentry/nextjs   @sentry/node
npx   @sentry/wizard@latest   -i   nextjs
```


Configure source maps. Set spending limits before you have users — an error storm can cost hundreds overnight.


JetBrains'[2026 developer ecosystem survey](https://www.jetbrains.com/lp/devecosystem-2025/) found the pattern holds across companies of every size: "Talented engineers leave organizations where they spend more time fighting infrastructure than building products."


## The Five Most Common Deployment Mistakes


These burn hours even for experienced developers.


**Committing` .env` files to GitHub.** This happens constantly. Rotate your secrets immediately — GitHub scanners surface them to attackers within minutes.


**Missing CORS config.** Your frontend is on` your-app.vercel.app` . Your backend is on` your-api.railway.app` . They are different origins. The browser blocks every request until you configure` Access-Control-Allow-Origin` correctly.


**Assuming Vercel handles long-running jobs.** The 10-second limit is real. File uploads, PDF generation, AI inference calls, and webhook processing all need a persistent backend or a task queue.


**No spending limits on AI API accounts.** A prompt injection attack or runaway retry loop can generate a $10,000 bill before you notice. Set hard limits on every AI provider account before you have users.


**No monitoring before first users.** You will not know your app broke at 3am unless something tells you. Set up Sentry before launch — not after the first complaint.


## What Reddit Developers Actually Experience


This is not a theoretical concern. From r/ClaudeAI:


> "Claude built my app in 20 minutes. I've spent 3 weeks trying to deploy it."


From r/vibecoding:


> "Building an App with Claude & Stuck with Backend"


Chalom Ellezam, writing on dev.to in April 2026, described the moment directly:


> "When you ask Claude Code 'how do I deploy this?', you get one of three answers: a list of commands that assume you already know what Docker is; a reference to 'your hosting provider' — implying you have one; a polite suggestion to 'consult your DevOps team.'"


The gap is real. Both paths are real. The difference is how many hours you want to spend on infrastructure versus the product itself.


## Deploy Your Claude Code App on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Take the app you built and deploy it on Blink — include database, auth, and hosting."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Blink Cloud](https://blink.new/cloud) replaces all six services in the DIY stack with one platform and one bill. The 62 MCP tools and 14 skills handle infrastructure provisioning through your coding agent — whether that's Claude Code, Cursor, or any MCP-compatible tool.


Before/after:


DIY Stack Blink Cloud


Accounts 6+ 1


Setup time 3+ hours ~5 minutes


Monthly cost $121–216/mo One bill


Auth Separate (Clerk/Auth0) Included


Database Separate (Supabase/Neon) Included


Monitoring Separate (Sentry) Included


[Learn more about Blink Cloud →](https://blink.new/cloud)


Deploying Claude Code app on Blink in 2 commands


Blink


## Frequently Asked Questions


Claude Code does not deploy apps on its own — it's a coding tool, not an infrastructure platform. To deploy automatically, install the Blink plugin with` npx skills add blink-new/blink-plugin` , then ask your agent to "deploy this on Blink." The agent uses Blink's 62 MCP tools to provision and deploy without manual configuration.


For the DIY path (Vercel + Supabase + Clerk + Railway), you'll need to configure DNS, CORS, JWT settings, and environment variables across multiple platforms. That typically requires 2-4 hours and prior experience with each service. Blink Cloud eliminates most of this — your agent handles provisioning through the[Cursor MCP setup](https://blink.new/blog/cursor-mcp-setup-guide) , and there's no manual service wiring.


For frontend-only projects with no backend: Vercel's free tier works well. For full-stack apps with database and auth, the cheapest DIY stack runs $70–121/mo at any meaningful usage tier. Blink Cloud has a free tier for early projects, making it cheaper than the combined DIY stack for apps below a certain traffic threshold.


Vercel is the right choice for frontend-only projects — pure React, Next.js, or static sites with no backend. If your Claude Code project has a backend, auth requirements, or a database, Vercel is one piece of a larger stack you'll have to assemble. Blink Cloud is the full stack — database, auth, backend, hosting, and custom domains in one platform. Use Vercel if you're deploying a UI; use Blink if you're deploying an app. For more on this topic, see our comparison of[Cursor vs Claude Code](https://blink.new/blog/cursor-vs-claude-code) .


With the DIY stack, you run migrations manually against your Supabase or Neon connection string before each deploy. With Prisma:` npx prisma migrate deploy` . With Drizzle:` npx drizzle-kit push` . The bigger challenge is keeping` DATABASE_URL` synced across Railway and Vercel. On Blink Cloud, the database is provisioned and managed as part of your app — migrations run in the same agent-driven flow as your deploy. To understand what makes this different, see[what is agentic coding](https://blink.new/blog/what-is-agentic-coding) .
