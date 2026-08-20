---
schema_version: "1.0.0"
document_id: "12e2b65e39499e9c0cd2453ed4f34c921c879800b4c20426712881b1105b72c9"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-backend-blink"
published_at: "2026-06-12T13:56:08+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:c04fdf0bee83f1eeb408d91b8d28b179c4f61e929a23cb298c36fd3509bbd25e"
---

# Claude Code + Blink: Build the Full Stack, Not Just the Frontend

## The stack, side by side


Infrastructure layer DIY (multiple services) Blink Cloud (one platform)


**Database** Supabase account + manual schema setup Blink provisions Postgres automatically


**Authentication** Clerk or Auth0 — separate account, separate billing Blink auth, wired by Claude Code


**File storage** S3 bucket + IAM role configuration Blink storage, included in plan


**Deploy + hosting** Vercel project + build config + domain wiring Blink deploy, triggered by one command


**Monthly cost** $80–$140 across services at modest scale Single Blink plan


Before Blink: 8 accounts, 3 hours of config. After Blink: 1 platform, 2 commands, production running


Blink


## Give Claude Code Full-Stack Deploy Capabilities in 2 Commands


Install the[Blink plugin](https://blink.new/docs/cloud/tools/skills) — 14 skills that let Claude Code provision and deploy your complete infrastructure:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


The first command downloads 14 skills and configures your MCP server automatically. No manual` mcp.json` editing required. The second opens a browser tab, saves your API key, and connects Claude Code to Blink Cloud.


Then tell Claude Code:


> "Deploy this app to production on Blink — set up the database, user auth, and hosting on a custom domain."


Claude provisions a Postgres database, user auth system, backend API, and production hosting automatically. No Supabase account. No Vercel project. No separate auth service.


[See how Blink Cloud works →](https://blink.new/cloud)


## Step 2: Ask Claude Code to deploy


The Blink plugin works with any project type. Match the prompt to what your app actually needs.


1


#### Full-stack web app


Ask Claude Code:` "Deploy this app to Blink. Provision a Postgres database, add Blink auth for user login, and host it on my custom domain."`


Claude reads your codebase, figures out what your stack requires, and provisions the matching infrastructure without guessing.


2


#### API-first backend


Ask Claude Code:` "Set up a Blink backend for this project. Create the database schema, build the REST endpoints, and deploy with Blink Cloud."`


Blink's 14 skills guide Claude through schema creation, endpoint wiring, and the deploy sequence — in the right order.


3


#### SaaS with user accounts


Ask Claude Code:` "This is a multi-tenant SaaS. Set up Blink auth with email login, provision a Postgres database, and deploy to Blink with a production domain."`


Claude handles auth configuration, user table setup, and environment variable wiring without you touching a single dashboard.


You don't need to enumerate every infrastructure detail upfront. Tell Claude what users should be able to do — "sign up, log in, and upload a file" — and Claude Code figures out what to provision.


## Step 3: Manage environment variables


Production apps need secrets: database connection strings, API keys, webhook tokens. Managing them manually across four dashboards is where configuration drift starts.


Blink Cloud uses managed environment vaults. Claude Code writes and reads your secrets through the MCP at deploy time. Nothing gets pasted into chat history. Nothing drifts between environments.


As of June 2026, Claude Managed Agents vaults are available on all Blink plans. You can rotate keys, add new variables, and audit what's in use — all by asking Claude directly.


Database connection strings and auth tokens stay inside Blink's vault. Claude accesses them through the MCP without writing them to local files or exposing them in conversation output.


## What Claude Code can do once Blink is connected


After` npx skills add blink-new/blink-plugin` runs, these are single-prompt operations:


- **Provision a Postgres database** — schema created, migrations run, connection string wired automatically
- **Add user authentication** — email/password and OAuth, with session logic already in your codebase
- **Deploy a new version** — Claude pushes the build and verifies the production deployment succeeded
- **Set a custom domain** — DNS wired through Blink, SSL certificate provisioned
- **Rotate API keys** — Claude updates the vault without modifying your source code
- **Add a storage bucket** — file uploads ready in one conversation turn


Every one of these used to be a tab-switching, form-filling, documentation-hunting task. Now each takes one sentence.


That's the shift. Claude Code was always good at writing the code. Blink Cloud makes it good at running it.


Deploy success with Blink Cloud — database provisioned, auth live, custom domain connected, all from two terminal commands


Blink


Yes. The Blink plugin connects to any existing codebase. Claude Code reads your project files and provisions the infrastructure that matches your stack. You don't need to start a new project or migrate to a Blink template — just run` npx skills add blink-new/blink-plugin` in the project root.


You can migrate incrementally. New projects go on Blink Cloud immediately. For existing projects, Claude Code can help you export data from Supabase and move your deploy config. Most developers complete a migration in a single conversation.


Yes.` npx skills add blink-new/blink-plugin` works in any MCP-compatible coding environment, including Cursor. Cursor also offers a one-click Blink install from the Cursor Marketplace if you prefer not to use the terminal command.


Tools handle specific operations — create a database table, set an environment variable, trigger a deploy. Skills are higher-level sequences Claude uses for multi-step tasks like provisioning a full auth system or running a database migration. The 62 tools give Claude Code precise control over each operation; the 14 skills give it the judgment to execute them in the right order.


Blink has a free plan. You can connect Claude Code, provision a database, and deploy a project before paying anything. Visit[blink.new/cloud](https://blink.new/cloud) for current plan details.


Blink handles DNS provisioning for Blink subdomains automatically. For custom domains, Claude Code walks through the DNS record setup in your registrar — it takes under five minutes.
