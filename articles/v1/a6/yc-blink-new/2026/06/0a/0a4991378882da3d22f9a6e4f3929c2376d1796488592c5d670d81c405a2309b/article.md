---
schema_version: "1.0.0"
document_id: "0a4991378882da3d22f9a6e4f3929c2376d1796488592c5d670d81c405a2309b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-getting-started"
published_at: "2026-06-03T00:17:32+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:22bf77c7f7dcb6c19a144aefeb1eee77845258730eef04369fa8daab55c94bcb"
---

# Claude Code Getting Started: Complete Setup Guide (2026)

## Step 2: Authenticate


Once installed, run` claude` to authenticate:


```text
claude
```


On first run, Claude Code opens a browser window for login. Complete the OAuth flow, return to your terminal — you're authenticated for all future sessions.


**Two authentication paths:**


Claude Code authentication: Pro subscription for individuals, API key for teams and CI/CD pipelines


Blink


*Choose subscription auth for one-bill simplicity, or API key for team visibility and CI/CD integration*


### Path 1: Claude.ai Subscription (Easiest for Individuals)


If you have Claude Pro, Max, Team, or Enterprise — Claude Code is already included. Log in with your Claude.ai account when prompted. No API key, no separate billing setup, no extra accounts. Your subscription quota covers usage.


### Path 2: Anthropic API Key (Better for Teams and CI/CD)


For teams, CI pipelines, or heavy automated use, an API key gives you usage visibility and per-project billing:


1. Go to[console.anthropic.com](https://console.anthropic.com/)
2. Create a project and generate an API key
3. Set the environment variable:


```text
export   ANTHROPIC_API_KEY  =  "sk-ant-..."
# Add to ~/.zshrc or ~/.bashrc to persist it
```


Then run` claude` — it picks up the key automatically.


**Which should you choose?** Pro subscription is right for individuals who want simplicity. API key is right for teams who want separate usage tracking per project, or for CI/CD workflows where OAuth isn't practical.


## Step 3: Your First Task


Navigate to any project directory and start a session:


```text
cd   ~/my-project
claude
```


Or give it a task directly from the command line:


```text
claude   "what is this project and what does it do?"
```


Claude reads your directory structure, package files, and key config files — then gives you a summary. That confirms codebase access is working.


A slightly more agentic first task:


```text
claude   "find all TODO comments in the codebase and list them with file and line number"
```


Claude searches every file, returns a structured list. No grep, no manual search, handles nested directories automatically.


## Step 4: Create Your CLAUDE.md


` CLAUDE.md` is the most important setup step after installation. It's a markdown file at your project root that Claude reads automatically at the start of every session — before doing anything else.


The fastest way to create one:


```text
claude   "/init"
```


The` /init` command analyzes your codebase — reads` package.json` , detects your tech stack, identifies test commands, inspects directory structure — and writes a` CLAUDE.md` draft. Review it and add your team's specific conventions.


A good` CLAUDE.md` includes:


```text
# Project Context


## Stack
-   Next.js 16, TypeScript, Tailwind CSS
-   PostgreSQL via Prisma ORM
-   Auth: NextAuth.js with JWT


## Dev Commands
-   `npm run dev`   — dev server on port 3000
-   `npm test`   — Jest with React Testing Library
-   `npm run lint`   — ESLint (must pass pre-commit)


## Conventions
-   Components: src/components/, one folder per component
-   API routes: src/app/api/
-   Database: always use Prisma, never raw SQL


## Important Rules
-   Never   `npm run build`   during development
-   All new API routes must require auth
-   Use generateId(8) helper for IDs — never auto-increment
```


Commit` CLAUDE.md` to git. Every developer on your team and every CI run starts each Claude session from the same project context.


## Key Commands Reference


Once you're using Claude Code daily, these are the commands you'll reach for:


Command What it does


` /init` Generate a CLAUDE.md from your codebase


` /compact` Summarize session history to free up context window


` /clear` Start a fresh session (clears all history)


` Shift+Tab` Toggle plan mode (read-only planning before execution)


` Esc` Interrupt the current running action


` Ctrl+C` Exit Claude Code session


**Plan mode** is particularly useful for complex tasks. Press` Shift+Tab` and Claude reads your codebase and describes exactly what it will do — without touching anything. You review the plan, approve or redirect, then switch to execution. Use it before any refactor touching more than 3–4 files.


## Common First-Time Issues


**"command not found: claude"** — The installer modified your PATH, but your current terminal session doesn't know about it yet. Close and reopen your terminal. If the issue persists, run` echo $PATH` and confirm the Claude install directory appears.


**Authentication loop / browser doesn't open** — Run` claude logout` then` claude` again to restart the auth flow.


**Session gets slow on long tasks** — The context window is filling up. Run` /compact` to summarize session history. Claude continues from the summary with a smaller context footprint.


**File edits seem wrong** — Start with plan mode (` Shift+Tab` ). Review Claude's plan before any edits are applied. On complex tasks, inspect the diff before confirming.


## Build Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up this project with CLAUDE.md and deploy the app on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Not if you use the native installer. The` curl -fsSL https://claude.ai/install.sh | bash` command installs a standalone binary with no Node.js dependency. If you prefer the npm package (` npm install -g @anthropic-ai/claude-code` ), then Node.js 18+ is required. For most users, the native installer is simpler and the recommended path.


The CLI is free to download and install. You pay for usage via an Anthropic subscription (Pro at $20/month is the entry point) or API credits. There is no free tier for Claude Code access. The Pro plan is the lowest-cost entry point — you can cancel monthly if it's not the right fit.


Native installer: Claude Code auto-updates in the background. Check your current version with` claude --version` . Homebrew: run` brew upgrade claude-code` . WinGet: run` winget upgrade Anthropic.ClaudeCode` . For Linux package manager installs, re-run the install script or use your package manager's update command.


CLAUDE.md is a plain markdown file in your project root that Claude reads at the start of every session. It tells Claude about your tech stack, test commands, coding conventions, and rules. Without it, Claude starts each session without project context. With it, every session starts informed. Run` /init` inside a Claude session to generate a first draft automatically from your codebase.


Yes. Install the Claude Code extension from the VS Code marketplace (` Cmd+Shift+X` → search "Claude Code"). The extension gives you inline diffs, @-file mentions, and conversation history inside the IDE panel. You can also run the CLI in VS Code's integrated terminal — it works inside any IDE including Cursor, JetBrains, and Zed.


Press` Shift+Tab` inside a running Claude session to toggle plan mode on or off. In plan mode, Claude reads your codebase and describes what it will do — but cannot edit files or run commands. When you're satisfied with the plan, press` Shift+Tab` again to switch to execution. You can also start a session directly in plan mode:` claude --plan` .
