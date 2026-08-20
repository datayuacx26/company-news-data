---
schema_version: "1.0.0"
document_id: "87e9bf5054d9a8eb2f675d4ff92530755c2ac07a599754a30ce631f3c2d956f0"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-subagents"
published_at: "2026-06-04T01:21:23+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:1b4b006721fafd0ccac152f651629cfe48f7a3f760e647f1219cd1eb523fbeba"
---

# Claude Code Subagents: Run Parallel AI Workers Across Your Codebase

## Create Your First Custom Subagent


Run` /agents` in Claude Code to create a subagent interactively. Or write the file directly — it is a Markdown file with YAML frontmatter.


**Project-level** (shared with the team, check into version control):` .claude/agents/`


**User-level** (personal, works in any project):` ~/.claude/agents/`


Here is a minimal code-reviewer subagent:


```text
---
name: code-reviewer
description: Expert code reviewer. Proactively reviews code for quality, security, and maintainability. Use immediately after writing or modifying code.
tools: Read, Grep, Glob, Bash
model: sonnet
---


You are a senior code reviewer. When invoked, run   `git diff`   to see recent changes and provide a prioritized list of findings: Critical issues (must fix), Warnings (should fix), Suggestions (consider improving).
```


The` description` field is the most critical field in the entire file. It controls when Claude automatically delegates to this subagent. Write it as a directive — "Use when..." or "Invoke after..." — and be specific about the trigger.


Key frontmatter fields:


Field What it does


` name` Unique identifier — what you @-mention


` description` **Most important.** Controls automatic delegation


` tools` Allowlist — only these tools available. Omit = inherits all


` disallowedTools` Denylist — removes these from inherited tools


` model`` sonnet` ,` opus` ,` haiku` , or` inherit` (default)


` permissionMode`` default` ,` acceptEdits` ,` auto` ,` bypassPermissions` ,` plan`


` background`` true` — always runs as a background task


` isolation`` worktree` — runs in an isolated git worktree


See the[official subagents docs](https://code.claude.com/docs/en/sub-agents) for the full frontmatter reference.


You can also mandate subagent behavior in` CLAUDE.md` so it applies every session:


```text
## Code review standards
When asked to review code, ALWAYS use a subagent with READ-ONLY access (Glob, Grep, Read only).
```


For more on writing effective project instructions, the[agentic coding best practices](https://blink.new/blog/agentic-coding-best-practices) guide covers CLAUDE.md patterns in depth.


## The Three Ways to Invoke a Subagent


**1. Natural language** — "Use a subagent to explore how authentication works." Claude decides whether to delegate based on the` description` fields in your agents directory. Flexible, but not guaranteed to trigger delegation.


**2. @-mention** — Type` @"code-reviewer (agent)"` in your message. The subagent runs regardless of whether Claude would have delegated automatically. Use this for guaranteed invocation.


**3.` --agent` flag** —` claude --agent code-reviewer` starts the entire session using that subagent's prompt, tools, and model. Good for team configurations where every session uses a specific persona.


The @-mention is the most practical for daily use. It is explicit, immediate, and does not require remembering CLI flags.


## Four Patterns That Work


**Research before implementing.** Spawn the Explore subagent to map how authentication is wired across 15 files. Get a clean summary back in the main context. Build with full knowledge and zero noise.


**Parallel modifications.** Three files need the same logging pattern added. Run three subagents simultaneously — one per file. Three tasks complete in the time one would have taken sequentially.


**Independent code review.** A fresh subagent with read-only access has no memory of the implementation decisions that led to your current code. It finds issues the main context is too invested to see. Give it` Glob, Grep, Read` only — no edit access.


**Pipeline workflows.** A design subagent produces a spec file. An implementation subagent reads the spec and writes the code. A test-writing subagent reads the code and writes the tests. Each stage hands off through files — no direct coordination required.


For large-scale work, Claude Code's dynamic workflows support[tens to hundreds of parallel subagents](https://claude.com/blog/subagents-in-claude-code) in a single session. Jarred Sumner used this approach to port 750,000 lines of the Bun runtime from Zig to Rust in 11 days, with 99.8% of tests passing.


This is what[multi-agent workflows](https://blink.new/blog/what-is-agentic-coding) look like at scale — not one Claude doing everything, but many Claudes doing specific things at the same time. If you are new to Claude Code, the[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-for-beginners) covers the foundation before subagents make sense to add.


The[awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) community repository is also a solid reference for real-world subagent configurations.


The four Claude Code subagent patterns — research, parallel modifications, independent review, and pipeline handoffs


Blink


*The four Claude Code subagent patterns — research, parallel modifications, independent review, and pipeline handoffs*


## Build Multi-Agent Apps With Claude Code and Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build a multi-step application with agent workflows and host it on Blink"


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


A subagent runs in an isolated context window — it starts fresh with no knowledge of your main session history. It returns only a summary, so the 40 files it read or the intermediate steps it took never appear in your main context. Prompting Claude directly in the main conversation exposes every intermediate step and file read to the conversation, which bloats context and narrows what the main session can do next.


No. Subagents cannot spawn other subagents — there is no nesting. If you need to chain work across multiple specialized agents, use the main conversation to delegate to each subagent in sequence. For large-scale parallel work, Claude Code's dynamic workflows feature (available on Max plan) handles tens to hundreds of concurrent agents.


Write a specific` description` field in your subagent's frontmatter — use directives like "Use immediately after writing or modifying code" or "Invoke when the task requires reading 10+ files." You can also add instructions to` CLAUDE.md` : "When asked to review code, ALWAYS use a subagent with READ-ONLY access." CLAUDE.md instructions apply consistently across every session in that project.


Give it` Read, Grep, Glob, Bash` — and nothing that modifies files. Read-only access gives the reviewer a fresh, unbiased perspective with no ability to make changes the main session has not approved.` Bash` lets it run` git diff` to see recent changes without touching any files.


Ask Claude to run multiple subagents concurrently: "Run three subagents in parallel — one per file — and update the logging pattern in each." Claude starts them simultaneously. Use Ctrl+B to background any running subagent and continue working. Type` /tasks` to see all background agent status.


The built-in Explore subagent skips CLAUDE.md for speed. Custom subagents inherit CLAUDE.md by default unless you configure otherwise. You can use CLAUDE.md to mandate when Claude should always delegate to a subagent — this makes the behavior consistent across every session without needing to prompt for it each time.
