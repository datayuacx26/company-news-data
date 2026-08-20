---
schema_version: "1.0.0"
document_id: "4ab0deb5d0330e144eebb6d9ae42ac0ed34d03ca3884deaca7a930d1033106da"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-claude-code-app"
published_at: "2026-04-30T12:55:37+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:50.549866+00:00"
content_hash: "sha256:c4c70293c6365f26c7a087710b07f0747c485afa6b31fe835ed71eee58d0504e"
---

# How to Deploy What Claude Code Builds: The Complete Guide

## Route 2: The Blink path


Blink Cloud is infrastructure built specifically for apps built by AI agents. Instead of wiring together six services, you give Claude Code (or Cursor) a two-command setup and ask it to deploy.


**Step 1: Install the Blink plugin**


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


That's it for setup. The first command downloads[14 skills](https://blink.new/docs/cloud/tools/skills) into your agent and auto-configures the MCP connection. The second opens a browser for auth and saves your API key. No manual` mcp.json` editing required.


**Step 2: Ask your agent to deploy**


Open Claude Code (or Cursor, or whatever agent you're using) and type:


> "Deploy this app to production on Blink — provision a database, set up auth, and connect a custom domain."


The agent uses the Blink skills to:


1. Provision a managed Postgres database
2. Set up auth with social login support
3. Configure file storage
4. Deploy your backend runtime
5. Wire up your custom domain with SSL


**What Blink provisions automatically:**


- **Database** — Managed Postgres, connection pooling, automatic backups
- **Auth** — Email + password, social login (Google, GitHub, Discord), magic links — no separate Auth0 account
- **File storage** — S3-compatible object store, signed URLs, built-in CDN
- **Backend runtime** — Your server-side code runs in an isolated environment per workspace
- **Deploy** — CI/CD connected to your Git repo
- **Custom domain** — DNS configuration and SSL, done


The tradeoff is real: you have less low-level control than wiring Vercel + Supabase + Auth0 yourself. If you need enterprise-specific compliance configurations, advanced Postgres extensions, or platform-specific edge network rules, the manual route gives you more knobs to turn.


For most apps — the ones Claude Code builds in an afternoon — you don't need those knobs. You need something running in production today.


**Time to first deployment: ~10 minutes.**


## Manual Stack vs Blink Cloud


Manual Stack Blink Cloud


**Setup time** 3–8 hours ~10 minutes


**Services to configure** 5–6 (Vercel, Supabase, Auth0, S3, domain) 1


**Accounts required** 5–6 1


**Monthly cost** $70–130/month One bill, everything included


**Database** Supabase / PlanetScale / Neon Managed Postgres, included


**Auth** Auth0 / Clerk / Supabase Auth Built-in, no extra account


**File storage** AWS S3 / Cloudflare R2 Included


**Custom domain + SSL** Manual DNS config Included


**Agent-native setup** You configure it yourself 2 commands, agent does the rest


**Low-level control** Maximum Less (by design)


Manual infra setup vs Blink Cloud — before vs after for AI-built app deployment


Blink


## What to build next


Once your app is deployed, your agent can keep building on top of it. Here are specific prompts that work well with the Blink setup:


**Add a waitlist:**


> "Add an email waitlist to the landing page. Store signups in the database. Send a confirmation email with Resend."


**Add user dashboards:**


> "Create a user dashboard that shows their activity since signup. Pull data from the database."


**Add payments:**


> "Add a Stripe subscription flow for the Pro plan at $29/month. Gate the premium features behind the subscription check."


**Add an API:**


> "Create a public REST API so users can access their data programmatically. Add API key auth."


**Add an admin panel:**


> "Build an admin panel to see all users, their subscription status, and any support tickets they've filed."


Each of these prompts works inside the same Blink-connected agent session — the agent already knows about your database schema and auth setup from the deployment conversation.


## Deploy What Your Agent Builds in Two Commands


Add Blink Cloud as your deployment layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


Blink provisions Postgres, auth, file storage, backend runtime, and deploy — in the same conversation. No Vercel config, no Supabase account, no Auth0 setup.[Learn more about Blink Cloud →](https://blink.new/cloud)


From code to production in minutes — Blink Cloud handles the full infrastructure stack


Blink


## Frequently Asked Questions


Yes. Blink Cloud works with full-stack apps built by any AI agent — Claude Code, Cursor, Windsurf, Aider, or a plain terminal session. The Blink plugin adds MCP tools to your agent that handle provisioning. If your app runs on Node.js, Python, or any standard backend runtime, Blink can host it. For a deeper walkthrough of what Claude Code can build, see our[Claude Code tutorial](https://blink.new/blog/claude-code-tutorial-beginners) .


Vercel and Supabase are excellent individual services. The issue isn't the quality of either — it's the integration work. You're manually connecting auth tokens, environment variables, CORS policies, and DNS records across six separate dashboards. Blink Cloud collapses that into a single platform that your AI agent can configure in one conversation. The tradeoff is that Blink is less configurable at the individual service level. If you need specific Supabase extensions or Vercel edge network rules, the manual route gives you more control. For most apps, you don't need that level of control on day one.


No manual MCP configuration required. The` npx skills add blink-new/blink-plugin` command handles all of it — downloads the skills, writes the MCP config, connects to Blink's servers. You run` blink login` to authenticate, and you're done. If you're already set up with Cursor and want a more detailed walkthrough of the Cursor integration specifically, check out[how to set up Blink Cloud in Cursor](https://blink.new/blog/cursor-mcp-setup-blink-cloud) .


You can still use Blink Cloud as your deployment layer going forward. The Blink plugin doesn't require you to rip out your existing setup — you can deploy new projects to Blink while keeping existing ones on Vercel + Supabase. Many developers use Blink for new AI-built projects because the two-command setup is faster, while keeping legacy apps on their existing infrastructure.


Blink Cloud is infrastructure for any AI-built app — the deployment layer doesn't care which agent wrote the code. Claude Code, Cursor, and any other agentic coding tool can use the Blink plugin to deploy. The reason Claude Code comes up so much is that it's particularly good at writing complete full-stack apps quickly — which means the deployment gap is especially visible. For context on the broader shift, see[what is agentic coding](https://blink.new/blog/what-is-agentic-coding) .


The manual stack runs $70–130/month once you add up Vercel Pro ($20), Supabase Pro ($25), Auth0 Developer Pro ($23), and S3 storage costs — with separate billing for each service and usage-based overages that are hard to predict. Blink Cloud is a single bill with database, auth, storage, backend, and deploy included. Specific pricing is at[blink.new/cloud](https://blink.new/cloud) .
