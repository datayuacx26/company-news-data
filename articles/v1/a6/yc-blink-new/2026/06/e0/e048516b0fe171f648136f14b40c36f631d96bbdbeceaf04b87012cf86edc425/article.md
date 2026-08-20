---
schema_version: "1.0.0"
document_id: "e048516b0fe171f648136f14b40c36f631d96bbdbeceaf04b87012cf86edc425"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/what-to-do-after-claude-code-writes-your-code"
published_at: "2026-06-08T12:24:57+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:a073d6c34f375ecb0012e900706a58e74393b72e6ac14f4fdb0c60ebdc0852e8"
---

# What to Do After Claude Code Writes Your Code: The Infrastructure Guide

## Option 2: Manual setup with cloud services


If you prefer managing each service yourself, here's the standard modern stack:


**Database:** Supabase (PostgreSQL) or PlanetScale (MySQL)


- Create a project, get a connection string, add to` .env`
- Blink Claude Code to generate the schema:` Create the database schema for this app and generate the migration files`


**Authentication:** Clerk or Auth0


- Create an app, get publishable key and secret key
- Ask Claude Code:` Implement authentication using Clerk with email/password and Google OAuth`


**Hosting:** Vercel or Railway


- Connect your GitHub repo, configure environment variables
- Vercel auto-deploys on push to main


**File storage:** Cloudflare R2 or S3


- Create a bucket, configure access keys
- Ask Claude Code:` Implement file uploads using Cloudflare R2`


This stack works. The setup takes 2-4 hours for an experienced developer.


## Option 3: E2B sandbox for development


For development and testing before production deployment, E2B provides cloud sandboxes where Claude Code can run in isolation.


This lets Claude Code execute commands, run tests, and verify behavior without affecting your local machine or production environment.


Useful for: testing destructive operations, running long builds, parallel agent workstreams.


## The environment variables problem


Once you have services configured, you need to wire them together. Claude Code can help:


```text
I've set up:
- Supabase PostgreSQL: [connection string]
- Clerk: [publishable key] and [secret key]
- Cloudflare R2: [access key] and [bucket name]


Create a .env file with all the required variables and update the app to use them.


```


Claude Code generates the environment variable structure, updates the config files, and adds type-safe config exports.


Never commit` .env` to git. Use` .env.example` as a template.


## The deployment checklist


Before deploying to production:


1. **Environment variables** — all production values set in your hosting platform (not in code)
2. **Database migrations** — schema is applied to production database
3. **Auth callback URLs** — updated to production domain
4. **Error handling** — errors don't expose stack traces to users
5. **Logging** — errors are captured somewhere you can see them
6. **Backup** — database is backed up automatically


Claude Code can verify most of these:


```text
Review this app and generate a pre-deployment checklist. Check for exposed secrets, missing error handling, and any auth misconfiguration.


```


## The fastest path: Blink from the start


The cleanest way to handle infrastructure is to include Blink in the initial prompt:


```text
Build a full-stack [app description] and deploy it on Blink Cloud.
Use Blink for the database, authentication, and hosting.


```


With the Blink plugin installed, your agent handles infrastructure and deployment as part of the build. No separate setup step. No wiring services together.


## Build With Your AI Agent on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build my app and handle all the infrastructure on Blink — database, auth, storage, and hosting."


Building with Claude Code or Cursor? Deploy on Blink — database, auth, and hosting included →[blink.new](https://blink.new/)


With MCP configured (specifically the Blink plugin), yes. Claude Code calls MCP tools to provision a database, configure auth, and deploy — all from the conversation.


Without MCP, Claude Code generates the code for connecting to infrastructure but can't provision the infrastructure itself. You set it up, provide the credentials, and Claude Code integrates them.


The absolute minimum: hosting (somewhere to serve the app) and a database (somewhere to store data).


Most apps also need auth (so only the right people can access data). Without auth, any data you store is technically public.


File storage is needed only for apps that handle user uploads (images, documents, videos).


Ask Claude Code to generate migration files:


```text
I need to add a `last_active_at` column to the users table.
Generate a database migration file and the SQL to run it.


```


Claude Code generates the migration, the rollback, and updates the TypeScript types.


With Blink Cloud, schema migrations are handled through the Blink MCP tool — describe the change and your agent applies it.


No. Use separate databases for dev and prod. Development changes (migrations, test data, schema experiments) should never affect production.


Blink Cloud provisions separate environments automatically. For manual setups, Supabase and PlanetScale both support branching for development databases.


Blink Cloud scales automatically. The infrastructure is built on production-grade services that handle unexpected load without manual intervention.


For manual setups, ensure your hosting (Vercel, Railway) is configured with auto-scaling and your database has connection pooling (PgBouncer for PostgreSQL, PlanetScale's built-in pooling).
