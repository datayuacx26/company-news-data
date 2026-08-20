---
schema_version: "1.0.0"
document_id: "9d9f8048288ae74262fb0b55542c7d9036828154bd0ee40b5403cffb93d2612a"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/deploy-cursor-claude-code-app"
published_at: "2026-05-26T12:48:38+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:876329131666532e422c9d59a95d5f15df7d163dfd667482178b190782b46b10"
---

# How to Deploy What Cursor or Claude Code Builds: The Full-Stack Infrastructure Guide

## Option B: Blink Cloud


[Blink Cloud](https://blink.new/cloud) is the infrastructure layer built specifically for apps created with AI coding agents.


One command installs 14 skills into your agent. Your agent then provisions database, auth, storage, backend, and deploy without leaving the conversation.


The before/after:


What you need Old way With Blink


Database Supabase ($25/mo) Included


Auth Auth0 ($23/mo) Included


Storage S3 ($5+/mo) Included


Deploy Vercel ($20/mo) Included


Custom domain Vercel + DNS Included


Setup time 3–4 hours Under 2 minutes


Monthly cost $70–120 One bill


For the initial Cursor setup walkthrough, see[how to set up Blink Cloud in Cursor](https://blink.new/blog/cursor-mcp-blink-cloud-setup) . This guide covers deploying apps already built — whether in Cursor, Claude Code, or both.


## Setting Up Blink Cloud With Cursor


1


#### Install the Blink plugin


Run this in your terminal:


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads 14 skills and auto-configures MCP. No` mcp.json` editing required.


2


#### Log in


```text
blink   login
```


A browser window opens. Authenticate once. Your Cursor agent now has 62+ MCP tools available.


3


#### Ask your agent to deploy


In Cursor agent mode:


> "Take this app and deploy it to production using Blink — set up the database, auth, and hosting."


Blink handles the rest.


## Setting Up Blink Cloud With Claude Code


The same plugin works for Claude Code.


1


#### Install the plugin


```text
npx   skills   add   blink-new/blink-plugin
```


Claude Code picks up the MCP config automatically on next launch.


2


#### Authenticate


```text
blink   login
```


Same flow: browser opens, API key saves, MCP connects.


3


#### Deploy from your Claude Code session


In your Claude Code session:


> "Deploy this project to Blink — provision a Postgres database, set up auth, and give me a live URL."


Claude Code calls the Blink MCP tools directly. You get a production URL without opening a second terminal.


## What Blink Provisions Automatically


When you tell your agent to deploy to Blink, here's what gets created:


**Database:** A Postgres instance scoped to your project. Your agent creates the schema, runs migrations, and seeds initial data through natural language commands. No Supabase dashboard. No connection strings to copy-paste.


**Authentication:** Full auth with sign-up, sign-in, password reset, and JWT session management. Users can log in on your first deploy. No Auth0 tenant, no Clerk account, no authentication library to configure manually.


**File Storage:** Object storage for images, documents, and uploads. No IAM policies. No bucket configuration. Your agent writes the upload and retrieval logic; Blink handles where the files live.


**Backend Runtime:** Serverless API endpoints. Your agent writes and deploys functions as part of the same conversation that built your frontend. No separate Vercel project. No cold-start configuration.


**Deploy:** Your app ships to a` \[project\].blink.new` URL. Custom domains map to it with a single command.


**62+ MCP Tools:** The agent doesn't just provision — it can query the database, manage users, check deploy status, read logs, and update environment variables, all from inside the conversation.


Blink uses the[Model Context Protocol](https://modelcontextprotocol.io/) standard — the same open protocol that powers integrations across Claude Code, Cursor, and other major AI coding tools.


## From Claude Code Output to Live Production App


You've used Claude Code to build a SaaS dashboard for a client. Claude Code generates the full codebase — React frontend, API routes, database schema. The output runs locally at` localhost:3000` .


Here's how to take it live with Blink:


```text
"I have this Next.js app ready locally. Deploy it to Blink — provision a Postgres
database using the schema in /lib/db/schema.ts, set up auth for the user table,
and give me a production URL."


```


Claude Code executes four Blink MCP tool calls:


1. Creates a Blink project
2. Provisions the Postgres database from your schema file
3. Configures auth based on your user model
4. Deploys to a live URL


Total time: under five minutes. No DevOps work on your end.


## Build Production Infrastructure Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Deploy this project to production and host it on Blink — set up the database, auth, and backend."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Yes. If you have a Next.js, React, or Node.js app built with Cursor or Claude Code, you can deploy it to Blink Cloud. The MCP tools work with any codebase, not just apps started inside the Blink app builder. Tell your agent to "deploy this project to Blink" and it handles the infrastructure.


Blink Cloud supports Next.js, React, and Node.js-based backends. The deploy pipeline handles both frontend and backend in the same flow. Ask your agent which framework fits your project if you're starting fresh.


The blink.new app builder is a browser-based interface for building apps through conversation. Blink Cloud is the infrastructure layer — database, auth, storage, deploy — accessible via MCP from any coding agent including Cursor and Claude Code. They share the same backend; you choose which front end to use.


Blink Cloud covers the 80% case: Postgres, auth, file storage, backend functions, deploy, and custom domains. For specialized infrastructure — Redis, vector databases, third-party payment processors — you wire those in manually alongside Blink.


Yes. Your code lives in a repository you control. Your Postgres schema is standard SQL. Backend functions use Node.js-compatible syntax. If you outgrow Blink Cloud, migration to a manual stack is straightforward.


The DIY stack (Vercel + Supabase + Auth0 + S3) runs $70–120/month across four separate accounts. Blink Cloud includes all of that on a single bill. See[blink.new/cloud](https://blink.new/cloud) for current pricing.
