---
schema_version: "1.0.0"
document_id: "d8e86f1cc8975374b64960b44ff5b3488db30956b8f61e2a055287edac8cd683"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-skills-blink-plugin"
published_at: "2026-06-06T00:25:30+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:32ad85741e69024b1a11c685bee2ccf71d67c6bef4d7d2ce9110ba838ed56a1f"
---

# What Are Cursor Skills? How to Add the Blink Plugin

## The Skills Way: 2 Commands


```text
npx   skills   add   blink-new/blink-plugin
```


This downloads 14 skills from the Blink plugin repository and configures the Blink MCP server automatically — no JSON file, no Cursor restart, no environment variables to copy.


```text
blink   login
```


This opens a browser tab, authenticates via GitHub or email, saves your API key, and activates the MCP connection. The whole process takes under 2 minutes.


The` npx skills add blink-new/blink-plugin` command handles all MCP configuration automatically — no JSON to write.


A developer at a terminal running the Blink skills install command with Cursor open in the background


Blink


## What 14 Blink Skills Give Your Agent


Once installed, your Cursor agent has full-stack infrastructure capabilities:


- **Database** : Provision a Postgres database without leaving Cursor
- **Auth** : Add user authentication — your agent writes and wires it
- **Hosting** : Deploy to a live Blink domain in one agent turn
- **Storage** : File storage and CDN for images, uploads, and assets
- **Backend** : Serverless functions with automatic scaling
- **Git** : Version-controlled deployments synced with your agent workflow
- **Env vars** : Secrets management without editing` .env` files by hand


The agent reads whichever skill is relevant to your prompt. Ask for a full-stack app, and it chains the database skill, auth skill, and deployment skill in sequence. You describe what you want. The agent figures out which skills it needs.


## Step-by-Step: Install and First Deploy


1


#### Install the Blink plugin


```text
npx   skills   add   blink-new/blink-plugin
```


Downloads all 14 skills and configures the MCP server. No JSON editing required.


2


#### Authenticate with Blink


```text
blink   login
```


Opens browser. Sign in with GitHub or email. API key saves automatically.


3


#### Verify the skills loaded


Open Cursor → **Settings → Features → MCP Tools** . Blink should appear in the list with a connected status indicator.


4


#### Run your first full-stack prompt


In Cursor's agent mode, type:


> "Build me a full-stack app with a database and host it on Blink."


Cursor can now provision a Blink Cloud database, user auth system, and full backend in a single agent turn.


## The Happy Path


```text
Step 1: npx skills add blink-new/blink-plugin
→ downloads 14 skills, auto-configures MCP


Step 2: blink login
→ browser opens, API key saved, MCP connected
→ NO manual mcp.json editing required


Step 3: Ask Cursor's agent:
"Build me a full-stack app with a database and host it on Blink."
→ agent provisions database, auth, backend, deploys


```


From zero to a live, publicly accessible full-stack app in one afternoon.


## Skills vs. Traditional Setup: What Actually Changes


Method Setup time JSON required Across all projects


Manual mcp.json 20+ steps Required Repeat per project


` npx skills add` 2 commands Not required Global install


Skills install to a global directory. Every Cursor project accesses them without repeating the setup.


Claude Code supports the same skill format — one install, available in both environments.


One honest tradeoff: skills work best in agent mode, where you're actively building and deploying. If your workflow is primarily inline autocomplete or quick edits, you won't invoke the infrastructure skills often. The value compounds the more you build full applications through the agent.


## Build Full-Stack Apps With Your Cursor Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app using Blink and host it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Rules are always-on instructions loaded into every prompt — they shape how the agent writes code regardless of the task. Skills are on-demand knowledge files invoked when the agent determines they're relevant. Rules set baseline behavior; skills give the agent specialized tools and context for specific workflows like provisioning infrastructure or running deployments.


No. The` npx skills add blink-new/blink-plugin` command handles all MCP configuration automatically. If you previously had a manual Blink entry in mcp.json, remove that duplicate to avoid conflicts. All your other manually-configured MCP servers remain untouched.


Yes. Skills use an open standard that works across AI coding tools including Cursor and[Claude Code](https://blink.new/blog/what-is-claude-code) . Install once, and both environments access the same 14 skills from the global skills directory.


It opens a browser tab to authenticate via GitHub or email. Once authenticated, Blink saves your API key locally and activates the MCP connection between Cursor and Blink Cloud. No manual key copying, no` .env` file editing — the CLI handles credential storage automatically.


Manual MCP setup gives you the tools but not the skills — the instructional context that tells the agent when and how to use each tool effectively. Skills install both layers simultaneously. Your agent knows what Blink can do AND how to chain those capabilities into coherent full-stack deployments.


After installation, your Cursor agent can provision Postgres databases, add user authentication, deploy apps to custom Blink domains, manage file storage, and handle environment variables — all from natural language prompts, without leaving Cursor. Read the[Cursor MCP setup guide](https://blink.new/blog/cursor-mcp-setup-blink) for a full breakdown of capabilities.


Learn more about[agentic coding](https://blink.new/blog/what-is-agentic-coding) to see where this workflow is heading.
