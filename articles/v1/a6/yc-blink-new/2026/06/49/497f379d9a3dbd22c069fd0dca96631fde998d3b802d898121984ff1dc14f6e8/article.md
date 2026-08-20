---
schema_version: "1.0.0"
document_id: "497f379d9a3dbd22c069fd0dca96631fde998d3b802d898121984ff1dc14f6e8"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-getting-started-guide"
published_at: "2026-06-13T12:59:59+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:a32090d5ee018b8f06fdbcc347965630f81f2e2f65c75818df7032ea45226c80"
---

# Claude Code Getting Started: Complete Setup Guide (Mac, Windows, Linux)

## The 3 Commands That Prove It's Working


Run these after any install. They confirm the binary is reachable, authenticated, and ready:


```text
claude   --version        # shows version number — confirms PATH is set
claude   doctor           # runs diagnostics — catches common config issues
claude                  # starts interactive mode — confirms login works
```


If` claude --version` works but` claude` hangs on startup, run` claude doctor` first — it identifies the exact cause.


On first run, Claude Code opens a browser window for OAuth login. Complete authentication there. Your credentials are stored and you won't need to log in again.


Successful Claude Code installation — version confirmed in terminal


Blink


---


## Setting Up CLAUDE.md


Once Claude Code is installed, create a` CLAUDE.md` file in your project root. This file tells Claude Code things it should always know about your project — without you re-explaining them every session.


A minimal` CLAUDE.md` :


```text
# Project Context


This is a Next.js 14 app with TypeScript and Prisma.


## Commands
-   `npm run dev`   — starts the development server
-   `npm run test`   — runs Jest tests
-   `npm run lint`   — runs ESLint


## Conventions
-   Use named exports, never default exports
-   All API routes must validate auth before processing
-   Error messages go in /src/constants/errors.ts


## Do Not
-   Modify prisma/schema.prisma without adding a migration
-   Add dependencies without checking package.json for conflicts
```


Claude Code reads` CLAUDE.md` at the start of every session. Good entries: coding conventions, commands the agent should know, things it should never do, and the project's current state. Bad entries: lengthy context that belongs in comments, things that change per-task, or generic best practices the model already knows.


You can also place` CLAUDE.md` files in subdirectories — Claude Code reads the one closest to the file it's working in.


---


## Common Errors and Exact Fixes


**Error:` command not found: claude` after install**


The binary installed but your shell can't find it. The native installer places` claude` at` ~/.local/bin/claude` on macOS/Linux. Add this directory to your PATH:


```text
# For zsh (default on macOS):
echo   'export PATH="$HOME/.local/bin:$PATH"'   >>   ~/.zshrc   &&   source   ~/.zshrc


# For bash (default on Linux):
echo   'export PATH="$HOME/.local/bin:$PATH"'   >>   ~/.bashrc   &&   source   ~/.bashrc
```


Then verify:` claude --version`


---


**Error:` OAuth error: Invalid code`**


The login code expired before you pasted it. Press Enter to retry, then complete authentication quickly. If the browser doesn't open automatically (common in WSL or SSH sessions), type` c` at the login prompt to copy the OAuth URL and paste it into your local browser.


---


**Error: Unexpected API charges / wrong account billed**


If you have` ANTHROPIC_API_KEY` set as an environment variable, Claude Code uses that API key instead of your subscription. This is the most common source of surprise charges.


Check it:


```text
echo   $ANTHROPIC_API_KEY
```


If it prints a key you didn't intend to use, unset it:


```text
unset   ANTHROPIC_API_KEY
```


Remove it permanently by deleting the` export ANTHROPIC_API_KEY=...` line from` ~/.zshrc` or` ~/.bashrc` . Then restart your terminal and log in with` /login` inside Claude Code.


---


**Error:` Killed` during install on Linux VPS**


Your server has less than 4 GB available RAM. The Linux OOM killer terminated the installer. Add swap space (see the Linux section above), then retry.


---


**Error:` dyld: cannot load` on macOS**


You're running macOS 12 or earlier. Claude Code requires macOS 13.0+. Update macOS from System Settings → Software Update.


---


## Plan Mode vs Agentic Mode


Claude Code runs in two primary modes. Knowing when to use each saves significant time.


**Agentic mode** (default) — Claude acts immediately. It reads files, edits code, runs commands, and iterates until the task is complete. Use this for:


- Clear, well-scoped tasks ("add input validation to the signup form")
- Bug fixes where the cause is known
- Routine changes you've made before


**Plan mode** — Claude thinks before acting. It produces a plan, waits for your approval, then executes. Activate it by starting your prompt with` /plan` or pressing` Shift+Tab` to cycle modes. Use this for:


- Large refactors touching many files
- Tasks where wrong assumptions are expensive to undo
- Anything involving database schema changes
- Features you haven't built in this codebase before


The default for new users is often too much agentic mode on tasks that warrant planning. When Claude starts making changes you didn't expect, stop the session, switch to plan mode, and get the plan confirmed before proceeding.


Claude Code plan mode — review before the agent acts


Blink


---


## Build This Into Your App With Claude Code or Cursor


If you're building an app and want Claude Code or Cursor to handle deployment, auth, and databases — not just local code — add the Blink plugin:


```text
npx   skills   add   https://github.com/blink-new/blink-plugin
```


```text
blink   login
```


Then give your agent a prompt like this:


```text
Build a user authentication system with email/password sign-in,
protected routes, and a user profile page. Deploy it to Blink Cloud
with a Postgres database. Use the Blink plugin for hosting and auth.


```


Blink Cloud handles database provisioning, auth infrastructure, and hosting — you ship a running app instead of configuring services. See[blink.new/cloud](https://blink.new/cloud) for the full platform.


---


## Frequently Asked Questions


Not with the native installer, which is the recommended path. The native installer downloads a self-contained binary — no Node.js dependency. Node.js 18+ is only required if you install via` npm install -g @anthropic-ai/claude-code` , which is the fallback method.


No. Claude Code requires a paid Claude plan: Pro, Max, Team, Enterprise, or Console (API access). The free Claude.ai plan does not include Claude Code access. If you're on a free plan, upgrade at claude.ai/settings before attempting install.


The native installer auto-updates in the background — you always run the latest version without manual action. Homebrew does not auto-update; you need to run` brew upgrade claude-code` yourself. For most individuals, the native installer is simpler. For teams managing software through Homebrew as policy, the Homebrew cask is cleaner.


Use native Windows if your project and tooling are Windows-native. Use WSL2 if your project uses Linux shell scripts, Docker containers, or Bash-based build tools. The Claude Code binary installs in both environments — choose based on where your code actually runs, not where it's more convenient to install.


Native install: run` claude update` , or it updates automatically in the background. Homebrew:` brew upgrade claude-code` WinGet:` winget upgrade Anthropic.ClaudeCode` apt:` sudo apt update && sudo apt upgrade claude-code` npm:` npm install -g @anthropic-ai/claude-code@latest`


Claude Code reads environment variables in your shell, including any keys you've exported. It does not access system keychain secrets or files outside your project directory by default. Review what environment variables your shell session exposes before working on sensitive projects. Run` claude doctor` to see what credentials Claude Code has detected.


When` ANTHROPIC_API_KEY` is present, Claude Code uses that API key for every request instead of your subscription credentials. API calls cost per token and are billed through Anthropic Console. If you have a Pro subscription and an old API key set in your profile, you'll be paying twice — subscription unused, API balance depleting. Unset the variable if you want subscription-based access.
