---
schema_version: "1.0.0"
document_id: "c5aa809042a5aad4f627cd58b99afdb23004d9f6f01ff6a957bd1a021321f63a"
company_key: "yc-cubic"
company: "cubic"
source_id: "yc-cubic-news-import-612274238019"
canonical_url: "https://www.cubic.dev/blog/skills-mcp-launch-week-03-day-4"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-23T06:53:38.364034+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:68006831e7a79a9e5ce0916977d42ccaa8828f757e5718288e912d37e618c685"
---

# cubic blog: Skills & MCP | Launch Week 03, Day 4

cubic has always reviewed your code after you pushed it, in Github.


Today, we bring its power into your IDE.


## How it works


With the new MCP and skills, your local coding agent can directly interact with cubic.


Pull architectural context from your cubic wiki before you plan. Surface your team's coding standards while you write. Run a local review before you push. Fetch and address GitHub review comments without leaving your editor.


One install, works across all coding agents.


` npx @cubic-plugin/cubic-plugin install`


## 9 MCP tools


Your coding agent can now call cubic directly:


-


**Reviews Comments** — pull review comments without leaving your editor


-


**Wiki** — query AI-generated documentation about your codebase architecture


-


**Review Learnings** — surface your team's coding patterns and conventions


-


**Codebase Scans** — check scan results and drill into findings


These are not summaries. They are the same tools cubic uses internally, exposed as MCP endpoints your agent calls on demand.


## 5 skills that give your agent superpowers


Skills trigger based on what you are doing. You do not need to remember commands or configure triggers.


**Skill**


**Triggers when**


**What it does**


**check-pr-comments**


You mention PR comments or review feedback


Fetches unresolved cubic comments, fixes the real issues, commits, pushes, and resolves threads


**run-review**


You say "review my code" or want a pre-commit check


Runs a local cubic review via CLI and surfaces issues by priority


**cubic-loop**


You say "loop until clean" or want to polish before merge


Reviews, fixes, re-reviews — repeats until clean or max iterations


**codebase-context**


You ask how something works


Queries cubic's AI-generated wiki for architectural context


**review-patterns**


You are writing or reviewing code


Pulls your team's learned conventions so your code matches on the first try


Install cubic MCP and skills now with:


` npx @cubic-plugin/cubic-plugin install`


Available today on all cubic plans.[docs.cubic.dev/skills](https://docs.cubic.dev/skills)
