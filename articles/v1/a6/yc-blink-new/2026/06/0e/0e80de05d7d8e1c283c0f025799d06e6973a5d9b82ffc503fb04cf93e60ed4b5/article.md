---
schema_version: "1.0.0"
document_id: "0e80de05d7d8e1c283c0f025799d06e6973a5d9b82ffc503fb04cf93e60ed4b5"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-tutorial-beginners"
published_at: "2026-06-12T13:54:53+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:60e0982ec32420feb0ba3ace616e296faa3b065e3adc65af17e6f7166e065d72"
---

# Claude Code Tutorial for Beginners: From Install to Your First Autonomous Task

## Install Claude Code


1


#### Install the CLI


Run this in your terminal:


```text
npm   install   -g   @anthropic-ai/claude-code
```


This installs the` claude` command globally. It takes about 30 seconds.


Alternatively, install from the VS Code extension marketplace — search "Claude Code" and click Install.


2


#### Authenticate


```text
claude   auth   login
```


This opens a browser window. Log in with your Anthropic account. Your session is saved locally — you won't need to do this again.


3


#### Verify the install


```text
claude   --version
```


You should see a version number. If you see "command not found," check that npm's global bin folder is in your PATH.


You can also run Claude Code from the Claude Desktop app. Download it from[claude.ai](https://claude.ai/) — it includes a built-in terminal and file browser. Good option if you prefer a GUI over a bare terminal.


## Your first session


Open a terminal. Navigate to any project folder you already have.


```text
cd   ~/projects/my-app
claude
```


Claude opens an interactive session. It reads your folder structure. It knows what you're working with.


Now type a task in plain English:


```text
Add input validation to the login form and show an error message if the email is invalid.


```


Claude reads the relevant files, writes the code, and shows you what it changed. You can approve each change, or let it run automatically.


That's the core loop. Read → think → edit → show you → repeat.


Your first few tasks should be small and in a project you know well. That way you can check Claude's work and build trust before handing it something bigger.


## Plan mode: the most important habit


Before you give Claude any task that changes code, type` /plan` first.


```text
/plan Refactor the user service to use async/await instead of callbacks


```


Claude shows you exactly what it intends to do — which files it will open, what changes it will make, and in what order. You read the plan. You approve it or adjust it. Then it executes.


This is the most important habit for beginners.


Without` /plan` , Claude just starts making changes. Fine for small tasks. For anything that touches multiple files, you want to see the plan first.


The example above? Without` /plan` , you might not realize Claude is about to restructure 6 files and rename a function used in 23 places. With` /plan` , you see that in 10 seconds and can say "actually, leave the function name alone."


Auto mode lets Claude make changes without asking for confirmation on each step. Enable it only after you've reviewed the plan and trust the task scope. For beginners, leave it off.


## Create your CLAUDE.md


` CLAUDE.md` is the single most valuable thing you can add to any project.


It's a markdown file at the root of your project. Claude reads it at the start of every session. It tells Claude how your project works — conventions, constraints, what to avoid.


Without it, Claude guesses. With it, Claude follows your rules from line one.


Here's a real example for a Next.js app:


```text
# Project Context


This is a Next.js 14 app with TypeScript and Tailwind CSS.


## Tech stack
-   Frontend: Next.js App Router (not Pages Router)
-   Database: Postgres via Prisma ORM
-   Auth: NextAuth.js with Google OAuth
-   Styling: Tailwind only — never add inline styles


## Conventions
-   Components go in /components/[  feature-name  ]/
-   API routes go in /app/api/ following REST conventions
-   All database access goes through the Prisma client in /lib/db.ts
-   Use async/await, never .then() chains


## Important constraints
-   Never touch /lib/auth.ts — it has production OAuth logic
-   Never modify migration files in /prisma/migrations/
-   Always run   `npm test`   before saying a task is done


## What "done" means
A task is done when: the code is written, linted, and all tests pass.
```


CLAUDE.md as Claude Code's project briefing — what sections to include and how Claude uses it to follow your rules from line one


Blink


Start with 10 lines. Add more as you notice Claude making assumptions you don't want. Your CLAUDE.md grows with your project.


Add a "What 'done' means" section. It's the most skipped — and the most valuable. Claude defaults to stopping when the code looks right. If you need tests to pass, lint to be clean, and a specific file to be updated, write it down.


## Your first autonomous task


Here's a real first task that works well:


```text
Fix all TypeScript errors in src/lib/api.ts and run the tests.
Tell me if any tests fail and why.


```


What Claude does with this:


1. Opens` src/lib/api.ts`
2. Reads the TypeScript errors using` tsc --noEmit`
3. Fixes each error one by one
4. Runs the test suite
5. Reports pass/fail with details


The output looks like this:


```text
Fixed 4 TypeScript errors:
- Line 23: Added missing `userId` to UserRequest type
- Line 47: Changed return type from `string` to `string | null`
- Line 89: Added null check before accessing `.email`
- Line 112: Removed unused import


Running tests... ✓ 14 tests passed
✗ 1 test failed: "should return 404 for missing user"
Expected: 404, Received: 500
The getUserById function throws instead of returning null.
The null check added on line 89 fixes this.
Re-running... ✓ All 15 tests pass.


```


That's the full loop. One instruction. Claude ran a multi-step debugging session and closed every open issue.


## What's new in 2026


Two major additions landed this year.


**Dynamic workflows** went generally available on June 10, 2026. For complex tasks — like "refactor the entire payment module" — Claude now writes an orchestration script first. That script breaks the work into parallel subtasks and runs multiple sub-agents simultaneously. Large tasks that used to take 30+ minutes now finish in 5–10.


**The ultracode setting** (available on Max and Team plans) increases how aggressively Claude parallelizes work and how long it runs before pausing for confirmation. On large codebases, it's a meaningful speed increase.


Both features are on by default in the CLI. You don't need to enable anything.


Dynamic workflows are most visible on tasks with 5+ files or multiple independent subtasks. On small single-file edits, Claude handles those the same way it always has.


## Context management: /clear and /compact


Two commands every Claude Code user needs to know.


` /clear` resets the conversation context completely. Use it when you're switching to a different task or a different area of the codebase. Fresh context means faster, more accurate responses.


` /compact` compresses the current conversation into a shorter summary without losing the key context. Use it when you're deep into a long session and responses start slowing down. Claude summarizes what it knows and continues from there.


Think of` /clear` as opening a new tab.` /compact` is cleaning up the current one.


## Build Your Next App With Claude Code and Deploy on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack todo app with auth and host it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


From Claude Code install to deployed full-stack app — the path from first session to production with Blink Cloud


Blink


## FAQ


Claude Code requires a paid Claude plan. It's included with Claude Pro ($20/mo), Max ($100/mo), Team, and Enterprise. There's no separate CLI subscription fee. Pro gives you a generous monthly credit allowance. Max removes most rate limits and adds the ultracode setting for faster parallel execution.


By default, yes — Claude shows you each change and waits for your confirmation. Auto mode turns this off and lets Claude run without pausing. You control which mode you're in at any time. For beginners, leave auto mode off until you've verified Claude's judgment on a few tasks.


All major languages — TypeScript, JavaScript, Python, Go, Rust, Ruby, Java, PHP, C#, and more. Claude reads any text-based file, which means config files, SQL migrations, markdown docs, and CI/CD YAML are all fair game. The only hard limit is binary files — those it cannot read or edit.


Yes. Claude Code runs in your terminal with full access to your git setup. You can ask it to commit, push, and draft pull request descriptions. It uses your existing git credentials — no separate authentication required.


Start with three things: your tech stack (framework, database, auth), your file structure conventions (where components live, where API routes go), and a short list of things Claude should never touch. Ten lines is enough. Add more as you catch Claude making assumptions you don't want repeated.


Copilot completes lines as you type — it's inline autocomplete inside your editor. Claude Code takes full tasks from a terminal prompt and handles multi-file, multi-step work autonomously. You don't need to be in an editor at all. Many developers use both: Copilot for line-level suggestions while writing, Claude Code for larger autonomous tasks.


Press` Ctrl+C` at any time. Claude stops immediately. It won't automatically roll back changes already made — use` git diff` to see what changed, and` git checkout .` to revert if needed. This is another reason to use` /plan` before large tasks: you catch the scope before Claude touches anything.
