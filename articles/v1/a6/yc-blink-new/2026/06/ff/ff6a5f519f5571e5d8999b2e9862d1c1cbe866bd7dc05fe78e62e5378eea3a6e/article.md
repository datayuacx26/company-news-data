---
schema_version: "1.0.0"
document_id: "ff6a5f519f5571e5d8999b2e9862d1c1cbe866bd7dc05fe78e62e5378eea3a6e"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-getting-started-setup"
published_at: "2026-06-07T12:27:35+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:7897e332ff4068062778ccd7cddb280b530409a07a4dac60ca814e5126a5149e"
---

# Claude Code Getting Started: The Complete Setup Guide (Mac, Windows, Linux)

## Windows Setup (WSL2 Required)


Claude Code requires a Unix environment on Windows. WSL2 (Windows Subsystem for Linux) provides this.


**Enable WSL2:**


Open PowerShell as Administrator:


```text
wsl   --  install
```


Restart your computer when prompted.


**Install Ubuntu from the Microsoft Store** , then open it and install Node.js:


```text
curl   -fsSL   https://deb.nodesource.com/setup_20.x   |   sudo   -E   bash   -
sudo   apt-get   install   -y   nodejs
node   --version    # should be 20.x.x
```


**Install Claude Code inside WSL:**


```text
npm   install   -g   @anthropic-ai/claude-code
```


**Navigate to your Windows projects from WSL:**


```text
cd   /mnt/c/Users/YourName/projects/my-app
claude
```


Windows paths under WSL use` /mnt/c/` instead of` C:\\` . Your files stay where they are — WSL reads them directly.


## Linux Setup


**Ubuntu/Debian:**


```text
curl   -fsSL   https://deb.nodesource.com/setup_20.x   |   sudo   -E   bash   -
sudo   apt-get   install   -y   nodejs
npm   install   -g   @anthropic-ai/claude-code
```


**Fedora/RHEL:**


```text
sudo   dnf   install   nodejs   npm
npm   install   -g   @anthropic-ai/claude-code
```


**Arch Linux:**


```text
sudo   pacman   -S   nodejs   npm
npm   install   -g   @anthropic-ai/claude-code
```


## Authenticate Claude Code


Run Claude Code for the first time:


```text
claude
```


First run opens a browser window for authentication. Log in with your Anthropic account.


After login, Claude Code saves the credential locally. You do not re-authenticate on subsequent runs.


**If you prefer API key authentication directly:**


```text
export   ANTHROPIC_API_KEY  =  sk-ant-...
claude
```


Add this export to your` .bashrc` or` .zshrc` so it persists between terminal sessions.


## Set Up CLAUDE.md


CLAUDE.md gives Claude Code persistent project context. Create it at` .claude/CLAUDE.md` in your project root:


```text
# Project Context


[Your stack: e.g. "Next.js 14, TypeScript, Prisma, PostgreSQL"]


## Key Commands
-   npm run dev — start development server
-   npm test — run test suite
-   npm run lint — ESLint check


## Code Style
[Your style rules]


## What NOT to Touch
[Any files or directories Claude Code should avoid]
```


With CLAUDE.md in place, every Claude Code session starts with full context. You stop repeating yourself.


## Your First Session


Start a session in your project directory:


```text
cd   ~/projects/your-app
claude
```


Type your first task. Be specific:


**Too vague:**


> "Improve the code"


**Specific and effective:**


> "Add input validation to the POST /api/users endpoint. Email must be a valid email format. Name must be 2-50 characters. Return a 400 error with field-specific error messages if validation fails. Add Jest tests covering the validation logic."


The more specific the task, the better Claude Code performs. Treat it like briefing a capable developer — they need context and scope, not vague directions.


## Managing Costs


Claude Code costs money per token used. Three practices keep costs predictable:


1. **Set a budget cap** in the Anthropic console (Settings → Usage Limits)
2. **Use` /compact`** when sessions get long — it summarizes context and reduces token use
3. **Use plan mode** for exploratory tasks so you see scope before execution costs accumulate


Most developers spend $15–40/month on regular use. Complex daily sessions (refactoring, major feature builds) cost more.


## Build This With Your AI Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up this project with Claude Code and deploy it to Blink Cloud with a database and auth."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


**Building with Claude Code or Cursor? Deploy on Blink — database, auth, and hosting included →[blink.new](https://blink.new/)**


Yes. They do not conflict. Copilot operates inside your IDE (VS Code, JetBrains) as inline autocomplete. Claude Code operates in the terminal as an autonomous agent. Many developers use Copilot for real-time suggestions while coding and Claude Code for larger tasks. Run them in parallel — there's no resource conflict.


No. Claude Code sends your code to Anthropic's API for processing. It requires an internet connection for every task. If privacy is a concern, Anthropic offers enterprise agreements with data processing commitments. The Community plan sends code to Anthropic's servers under their standard privacy policy.


Claude Code updates through npm:` npm update -g @anthropic-ai/claude-code` . Run this whenever Anthropic announces a new version. Check the current version with` claude --version` . Breaking changes are rare — the update is usually safe without testing in a dev environment first.


Claude.ai is the web chat interface — you type questions and have a conversation. Claude Code is a terminal agent — it executes tasks in your actual codebase, reads files, runs commands, and makes changes. They use the same underlying Claude models but Claude Code has tools (file system access, terminal execution) that Claude.ai does not. Use Claude.ai for questions and reasoning; use Claude Code for actual coding work.
