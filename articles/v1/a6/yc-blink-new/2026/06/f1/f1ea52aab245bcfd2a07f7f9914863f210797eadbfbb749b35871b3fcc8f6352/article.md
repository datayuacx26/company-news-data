---
schema_version: "1.0.0"
document_id: "f1ea52aab245bcfd2a07f7f9914863f210797eadbfbb749b35871b3fcc8f6352"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-md-best-practices"
published_at: "2026-06-05T12:57:29+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:74a59db5e2a95f831424da75c3b9e8f6a82a8a451181978c9e9bf82f48d9b892"
---

# CLAUDE.md Best Practices: The 10-Section Template That Makes Claude Code 10× Better

## Section Deep Dives


### Section 1: Project Overview


Two to three sentences maximum. Claude Code reads this first — it orients the agent to what it's working in.


Bad: "This is a web app I'm building." Good: "Customer-facing SaaS for restaurant reservation management. Restaurants log in to manage tables; diners book via a public booking flow. About 50 restaurant accounts in production."


### Section 3: Rules (Never Violate)


This is the highest-value section. List the specific mistakes that would cause real problems.


The` CLAUDE.md` in the Blink engineering team's main repo (the repo you're reading this article from) includes:


- "Never run` npm run build` — destroys the dev server"
- "Never` git commit -A` — stage files explicitly"
- "Never push to main directly"


These aren't style preferences — they're things that, if violated, cause hours of recovery work. Document them explicitly.


### Section 7: Current Work Context


The most underused section. Update it at the start of each working session:


```text
## Current Work Context
Working on: Stripe subscription integration
Status: Webhook handling working, payment form UI in progress
Known blockers: Stripe test mode webhooks need ngrok tunnel
Next step: Complete the checkout flow, then test end-to-end with Stripe test card
```


This context means Claude Code understands the current goal without you explaining it in every conversation.


### Section 10: Off-Limits


Explicitly list files that should never be touched without discussion:


- Security-critical files (auth, session management)
- Generated files (compiled output, migration snapshots)
- Config files that require human review (env examples, CI config)


Without this list, the agent will helpfully modify these files when it thinks a change would help. Sometimes it's right. Sometimes it's catastrophic.


## CLAUDE.md vs AGENTS.md — When to Use Each


Both files are loaded at session start. They serve different purposes:


File Purpose Who reads it


` CLAUDE.md` Claude Code-specific config, rules, and workflow for THIS codebase Claude Code


` AGENTS.md` Broader architectural context, multi-agent instructions, team docs Any AI agent


Use` CLAUDE.md` for rules specific to how you work with Claude Code in this project. Use` AGENTS.md` for architectural docs and team context that any AI agent (Claude Code, Cursor, Codex) should know.


Many projects maintain both.` AGENTS.md` is the authoritative architecture reference.` CLAUDE.md` references` AGENTS.md` and adds Claude Code-specific configuration.


## Context Engineering: What Goes in vs Out


**Token budget awareness:** Claude Code's context window is 200,000 tokens. A typical CLAUDE.md file is 2,000-4,000 tokens — well within budget. But CLAUDE.md + codebase context + conversation history adds up. Keep your CLAUDE.md under 500 lines.


**What to put in CLAUDE.md:**


- Stable information (tech stack, architecture, coding rules)
- Session-specific current work context
- Non-negotiable rules


**What to keep out of CLAUDE.md:**


- Information that changes frequently (use session chat)
- API keys and secrets (use .env)
- Documentation that belongs in code comments


## Build This With Your AI Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up a CLAUDE.md for this project that covers the tech stack, rules, and workflow preferences, then configure Blink Cloud as the deployment infrastructure."


Your agent sets up the configuration file and wires the Blink plugin — database, auth, backend, and hosting provisioned automatically.[Learn more about Blink Cloud →](https://blink.new/cloud)


In the root of your project directory — the same folder as your` package.json` or` pyproject.toml` . Claude Code reads it automatically when you open that directory. You can also place CLAUDE.md files in subdirectories for project-specific context in monorepos.


Update the "Current Work Context" section at the start of each session. Update tech stack and architecture sections when they change significantly. Review the rules section monthly to remove outdated constraints. The file should reflect your project's current reality, not its history.


Cursor uses` .cursorrules` and` .mdc` files for similar functionality. Cursor reads` CLAUDE.md` as documentation but doesn't automatically use it as a system prompt the way Claude Code does. If you use both tools, maintain both — see the Cursor Rules guide for Cursor-specific configuration.


Writing rules as suggestions instead of constraints. "Try to keep functions under 30 lines" gets ignored. "Never let a function exceed 20 lines — decompose it" gets followed. Specificity and active language matter.


Yes. Put a top-level CLAUDE.md with global rules and architecture. Put package-level CLAUDE.md files in each service directory with service-specific context. Claude Code loads the most specific file it finds. For example, when working in` packages/api/` , it loads` packages/api/CLAUDE.md` with the top-level file as additional context.
