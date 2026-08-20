---
schema_version: "1.0.0"
document_id: "7391801c98bdf3079dfe442519f348e516897b22504ee4add3e10eb137123718"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/what-is-claude-code"
published_at: "2026-06-12T13:07:38+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:1d9ac57047392c158260f1547937c9bc5f6791c389063c6e28baad70f9510cdd"
---

# What Is Claude Code? A Plain-English Guide for Developers

## What's new in 2026


Anthropic announced several major features at Code with Claude Tokyo on June 9–10, 2026.


### Dynamic workflows (GA June 10, 2026)


Dynamic workflows let Claude write its own orchestration script for complex tasks. When a task is too large for one context window, Claude writes a JavaScript harness that spawns parallel subagents — each with a clean context and one focused job.


A repo-wide security audit that once required multiple manual sessions now runs as one workflow. Each subagent checks a different subsystem. Results are verified and synthesized into one output.


Dynamic workflows are on by default for Max, Team, and Enterprise plans. Pro plan users enable them in` /config` .


### Ultracode


Ultracode combines maximum reasoning effort with automatic workflow creation.


Enable it with` /effort ultracode` . Claude then evaluates every task in the session and decides when to spawn a workflow — you stop manually deciding when a task needs multi-agent orchestration. Claude decides.


One request can trigger multiple workflows in sequence: one to understand the code, one to make the change, one to verify it. Ultracode resets at the end of each session.


### Routines (June 9, 2026)


Routines let you save a Claude Code configuration and run it on a schedule.


Create a routine from the web at` claude.ai/code/routines` or inside the CLI with` /schedule` . Set it to run every morning, on an API call, or when a GitHub PR opens. Anthropic runs the routine on managed cloud infrastructure — your laptop doesn't need to be open.


Common uses: nightly PR review summaries, scheduled dependency audits, CI-triggered code quality checks.


### Agent view


Agent view gives you a single pane to manage multiple active Claude Code sessions.


Monitor progress across parallel sessions, review what each agent is doing, and track token usage from one CLI view. It's useful when Claude Code is running on multiple tasks across different repositories at the same time.


### Managed Agents (June 2026)


Managed Agents adds scheduled deployments and environment variable vaults to the Claude API.


Scheduled deployments run an agent on a cron schedule — useful for nightly database audits or weekly reports. Environment variable vaults let agents use API keys without the model seeing the actual key. Both are in public beta, accessible with the` managed-agents-2026-04-01` API header.


## How to get started


1


#### Install Claude Code


Run this in your terminal:


```text
npm   install   -g   @anthropic-ai/claude-code
```


Claude Code requires Node.js 18+. Installation takes under a minute.


2


#### Start a session


Navigate to your project directory and type` claude` :


```text
cd   your-project
claude
```


On first use, a browser window opens for authentication. You need a Claude Pro, Max, Team, or Enterprise plan.


3


#### Create a CLAUDE.md file


Add a` CLAUDE.md` to your project root with your tech stack, coding conventions, and test commands.


This file gives Claude the context it needs to work on your codebase without re-explaining things every session.


4


#### Run your first task


Type what you want in the Claude Code prompt:


```text
Add input validation to the signup form.
Run the form tests to confirm they pass.


```


Claude reads the relevant files, shows you its plan, and asks for approval before making any changes.


5


#### Review and accept


Claude Code shows every file change before making it. Review the diff, approve each action, or enable "Accept all" once you trust the direction for that session.


Your job shifts from writing code to reviewing and directing.


Use plan mode before any complex task. Press` Shift+Tab` in a session or start with` --permission-mode plan` . Claude maps out the entire solution before touching a single file. You see exactly what it intends to do.


## When to use Claude Code


Claude Code earns its place on large, multi-file work.


**Repo-wide refactors** — renaming a type across 50 files, migrating from one ORM to another, updating an API interface used throughout the codebase. Claude holds the whole project in context.


**Autonomous test-fix loops** — ask Claude to run the test suite, fix failures, and rerun until everything passes. Claude iterates through the cycle without you watching each step.


**Codebase exploration** — onboarding to a new project. Ask Claude to explain the architecture, trace a request through the stack, and identify where specific behavior lives.


**Background automation via Routines** — nightly code quality reports, automated PR descriptions, scheduled dependency audits. Claude runs the task on a schedule; you review the output.


**Deep debugging** — stack traces spanning multiple files. Claude follows the call chain across the full repository and identifies root causes that file-by-file tools miss.


For a deeper comparison:[Claude Code vs. Cursor →](https://blink.new/blog/cursor-vs-claude-code)


If you're evaluating options:[Claude Code alternatives →](https://blink.new/blog/claude-code-alternatives)


## Build This With Claude Code and Deploy on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app using Claude Code and host it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Deploying a Claude Code project on Blink Cloud — from one command to a running full-stack app


Blink


*Deploying a Claude Code project on Blink Cloud — from one command to a running full-stack app*


Claude Code is included with Claude Pro ($20/month), Max ($100 or $200/month), Team, and Enterprise plans. You don't pay separately — it's part of the subscription. API access is billed at standard Claude token rates.


Yes. Claude Code has an official VS Code extension that gives you the same agentic capabilities inside the editor. It also runs in the terminal (alongside any editor), a desktop app, the browser, and JetBrains IDEs. Your CLAUDE.md files and settings work across all surfaces.


Claude is the AI model — it powers conversations at Claude.ai. Claude Code is an agentic tool built on top of Claude with real developer tools: file editing, terminal commands, git operations, and MCP integrations. Claude answers questions. Claude Code executes tasks in your codebase.


Claude Code reads files from your local filesystem using its 1M-token context window. It doesn't need you to manually paste files into a prompt — it reads what it needs as it works. Use` CLAUDE.md` to give Claude persistent context about architecture, conventions, and constraints that apply to every session.


Dynamic workflows let Claude write an orchestration script for large tasks. Claude spawns parallel subagents — each with a clean context window and a focused piece of the work. Results are verified and synthesized before they surface to you. Dynamic workflows went GA on June 10, 2026, and are on by default for Max, Team, and Enterprise plans.


Claude Code asks for permission before modifying any file or running any command. You approve each action or enable "Accept all" for a specific session. Use plan mode (` Shift+Tab` ) to review Claude's complete plan before it touches a single file. You decide what gets committed.


Yes, with Routines. Save your Claude Code prompt and repositories as a routine, then set a schedule — hourly, daily, on a cron expression, or triggered by a GitHub event. Anthropic runs it on managed cloud infrastructure. Routines launched on June 9, 2026, and are available on Pro, Max, Team, and Enterprise plans.
