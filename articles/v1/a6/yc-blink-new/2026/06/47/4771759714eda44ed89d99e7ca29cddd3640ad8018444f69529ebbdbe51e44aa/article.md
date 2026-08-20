---
schema_version: "1.0.0"
document_id: "4771759714eda44ed89d99e7ca29cddd3640ad8018444f69529ebbdbe51e44aa"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-getting-started"
published_at: "2026-06-04T01:57:38+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:4bd504fa40945ba8d660b6a06a17c230767063dc6185cd7d28833f7279a5aac5"
---

# Cursor Getting Started Guide: From Install to Your First AI-Built App

## Configure Your AI Model


Open Settings → Models. Cursor ships pre-configured with Claude Sonnet 4 and GPT-4o. Both are solid starting points for general coding work.


For larger tasks — refactoring a whole module, building a new feature from scratch — switch to Claude Opus 4 or GPT-4.1 in the model dropdown inside the Composer panel. Tab completion runs on a faster, lighter model automatically. You don't need to manage that separately.


If you have your own API key from Anthropic or OpenAI, add it under Settings → API Keys. This routes requests through your own account instead of Cursor's credits.


One setting worth enabling early: **Include project structure** in the context options. Cursor will read your folder layout before every prompt, which makes architectural suggestions significantly more accurate.


## Cursor's Three Modes — And When to Use Each


Cursor's three interaction modes — Tab autocomplete, Chat, and Agent — each built for a different kind of task


Blink


Cursor has three ways to work with AI. Most beginners use only one of them. That's the main reason early sessions feel underwhelming.


**Tab completion** is always on. As you type, Cursor predicts the next line or block. Press Tab to accept. No prompt needed. This is the fastest loop — you feel it within the first ten minutes of use.


**Chat (Cmd+L)** opens a conversation panel beside your code. Ask questions about the open file, request explanations, or ask for a targeted rewrite. Add files to the context with` @filename` . Chat is best for precise, scoped requests.


**Agent mode (Cmd+I)** is different in kind, not just degree. You describe a goal and the agent runs multiple steps — reading files, writing code, executing terminal commands — until the task is complete.[Developers using AI coding agents report completing tasks 55% faster](https://arxiv.org/html/2605.25438v1) than without them. That number comes from Agent mode.


Use Tab for everyday editing. Use Chat for questions and single-file changes. Use Agent for anything that touches more than one file or requires a sequence of steps.


## Your First Cursor Project


Open a new folder. Press Cmd+I to open Agent mode. Start with something concrete:


> "Create a React app with TypeScript. Include a home page, a dashboard page, and a login form. Use Tailwind for styles."


Cursor writes the components, installs dependencies, and runs the dev server. Watch the terminal at the bottom — the agent runs the commands itself.


When the output lands, open Chat and ask questions: "What does this component do?" or "How do I add a new page?" Cursor explains its own output in plain language. That's the fastest way to learn what it wrote.


After two or three sessions, the back-and-forth becomes fluent. The first session is always the slowest.


That said — a React scaffold with a login form is still just a frontend. It has no backend. No real auth. Nowhere to store data. That gap is where most beginners stall out.


## Add MCP Tools to Extend Cursor's Reach


[MCP (Model Context Protocol)](https://blink.new/blog/cursor-mcp-setup-guide) is how Cursor connects to external systems. When an MCP server is configured, Cursor's agent can call tools directly — querying databases, triggering deploys, reading live data — without you switching apps.


With the Blink plugin installed, the agent has 62 MCP tools available. Creating a database table, running a migration, deploying a backend, setting up auth — all of those happen inside the agent's task loop.


There's a full guide to[the best MCP servers for developers](https://blink.new/blog/best-mcp-servers-developers) if you want to extend further. But for most new projects, the Blink plugin covers everything you need: database, auth, backend, deploy, and custom domain from one connected layer.


## Build a Full-Stack App With Cursor and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app and host it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Deploying a full-stack Cursor app to Blink Cloud — database, auth, and hosting provisioned automatically by the agent


Blink


## Frequently Asked Questions


No. Cursor's Agent mode understands plain English. You can describe what you want to build without knowing specific syntax, and the agent writes the code. Most beginners get a working prototype in their first session. Being able to read the output — not write it — is the only skill worth developing early.


Chat (Cmd+L) is conversational — ask questions, get explanations, request a specific rewrite in the open file. Agent mode (Cmd+I) is task-driven — you describe a goal and the agent runs multiple steps across multiple files to complete it, including terminal commands and package installs. Use Chat for targeted edits; use Agent for anything larger than a single function.


Without extra tooling, you'd set up Supabase or another service separately, wire it in manually, configure your deploy — about 2-3 hours minimum. With the Blink plugin installed via` npx skills add blink-new/blink-plugin` , Cursor's agent can provision a Postgres database, backend API, and auth layer on Blink Cloud directly from the editor. Tell the agent what you need and it handles the setup.


MCP (Model Context Protocol) is a standard that lets AI agents connect to external tools and services. When an MCP server is configured, Cursor's agent can call those tools directly — reading databases, triggering deploys, calling APIs — as part of its task loop. Cursor supports MCP natively. The Blink plugin is one MCP server that gives Cursor's agent full-stack capabilities including 62 tools for database, auth, backend, and hosting.


Yes. Open the folder and Cursor reads the entire project tree as context. Reference any file in your prompts with` @filename` . The agent understands multi-file context and can make coordinated changes across the project. Most developers start by asking Cursor to explain a specific module — then work outward from there.


Cursor has a free tier with limited AI usage per month. The Pro plan is $20/month and includes unlimited Claude and GPT-4o interactions. The Blink plugin is free to install; Blink Cloud has a free tier and paid plans starting at $20/month for production-ready infrastructure — database, auth, backend, and hosting included with no per-service bills.
