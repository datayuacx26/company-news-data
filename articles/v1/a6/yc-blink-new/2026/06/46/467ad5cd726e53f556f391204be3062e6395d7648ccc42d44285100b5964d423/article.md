---
schema_version: "1.0.0"
document_id: "467ad5cd726e53f556f391204be3062e6395d7648ccc42d44285100b5964d423"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/context-engineering-ai-developers"
published_at: "2026-06-12T00:57:49+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:c1ef20d6dd817a49eee0cec19e25d4ef4b876f6f2f116f17cada3c31b6e3deef"
---

# Context Engineering: The Most Important Skill for AI-Era Developers

## The 10-Section CLAUDE.md Framework


` CLAUDE.md` is the most direct lever you control. It runs before every conversation and shapes everything that follows. Here is the 10-section framework that covers every type of context an agent needs:


1. **Purpose + Overview** — what this project is, who it's for, what it does
2. **Tech Stack and Versions** — Node 22, TypeScript 5.4, Postgres 16 — exact versions matter
3. **Architecture Rules** — where code belongs, what talks to what, the boundaries that cannot be crossed
4. **Coding Standards** — naming, file structure, how functions are organized
5. **Testing Requirements** — what needs tests, which framework, coverage expectations
6. **Build and Run Commands** —` npm run dev` ,` npm test` ,` npm run db:migrate` — no guessing
7. **External Services** — which APIs, where credentials are stored, how services are configured
8. **What NOT to Do** — the most underrated section; explicit constraints prevent the most common mistakes
9. **Context Shortcuts** — abbreviations and domain terms the agent should know without explanation
10. **Ongoing Decisions Log** — architectural decisions that have been made and why


Here is an example of the structure in practice:


```text
# Project Context


## Overview
B2B SaaS for engineering teams. Node.js 22 + TypeScript + Postgres.


## Architecture Rules
-   All API routes in /src/api — never in /src/components
-   Database access only through /src/db — never direct queries in routes
-   Every route must be authenticated via middleware


## What NOT to Do
-   Never use   `any`   in TypeScript
-   Never commit secrets to .env files
-   Never bypass the auth middleware
```


The "What NOT to Do" section alone eliminates 40% of the revision cycles in typical AI-assisted development. Agents follow explicit constraints far more reliably than they infer them.


The CLAUDE.md file expanding to reveal 10 structured sections — the foundation of persistent context engineering


Blink


Write your CLAUDE.md as if you were onboarding a highly capable engineer who has never seen your codebase. Every assumption you leave unstated is a mistake waiting to happen.


## Context Decay — The Hidden Problem


Context quality degrades over long sessions. An agent that follows your rules perfectly in message 5 will start drifting by message 40. This is not a model defect — it's a fundamental property of how context windows work.


Three habits that prevent decay:


**Use /compact at 50% context, not 100%.** Compaction at the limit means your most recent rules and constraints get summarized away first. Compact earlier and the agent retains critical architectural decisions through the session.


**Start fresh sessions for new features.** A session that has been debugging a payment bug for two hours has built up context pollution. Start a new session when switching to a different domain — the clean baseline makes the agent more precise.


**Git commits as context checkpoints.** Commit after completing each discrete unit of work. The commit history becomes a structured record of decisions — one your agent can read in future sessions to understand what has been done and why.


Context decay is why skilled developers using AI ship faster than novice developers who generate the same volume of prompts. They know when to reset.


## MCP as Infrastructure Context


Model Context Protocol changes the fundamental nature of what an agent knows at runtime.


Without MCP, an agent reasons from training data plus the files you show it. It infers your database schema from your query code. It guesses your API structure from your route files. Every inference is a risk.


With MCP, the agent reads your live database schema directly. It sees your actual GitHub issues and PRs. It knows your deployed configuration, your queue state, your feature flags. Every inference becomes a lookup.


The quality difference is not incremental — it's architectural. An agent with live system access makes decisions your training data cannot replicate. It knows your schema has a` created_by_agent_id` column. It knows the PR it should reference is #847, not a made-up number. It knows your production database has 4.2 million user records and designs the migration accordingly.


MCP gives your AI agent live connections to real system data — database schema, GitHub, and external services


Blink


MCP is the difference between an agent that sounds knowledgeable and one that actually is.


## Build This Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack application using proper context engineering conventions and deploy it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Context engineering is the practice of structuring the persistent environment around an AI agent so every session starts from a high-quality baseline. It includes project structure, persistent instruction files like CLAUDE.md, runtime context (which files the agent sees), and tool connections via MCP. Unlike prompt engineering — which optimizes individual messages — context engineering shapes the entire operating environment the agent works within.


Prompt engineering improves individual messages in a single conversation. Context engineering improves the persistent environment that exists before any message is sent. A well-engineered context means an ordinary prompt produces excellent results. Without it, even an exceptional prompt must compensate for missing baseline knowledge. Developers who invest in context engineering see compounding returns — every session starts better than the one before.


The 10 most important sections are: project purpose and overview, tech stack with exact versions, architecture rules (where code belongs), coding standards, testing requirements, build and run commands, external service configurations, explicit "What NOT to Do" constraints, project-specific terminology shortcuts, and an ongoing architectural decisions log. Start with the "What NOT to Do" section — it eliminates the most common revision cycles and has the fastest payoff.


MCP (Model Context Protocol) gives your agent live access to your actual systems instead of forcing it to infer from static files. With MCP connections, your agent reads your real database schema, sees live GitHub issues and PRs, and knows your actual deployed state. This removes an entire category of errors caused by agents reasoning from outdated or incomplete information. For Claude Code and Cursor users, Blink's MCP plugin provides 62 tools covering database, auth, backend, and hosting — install with` npx skills add blink-new/blink-plugin` .
