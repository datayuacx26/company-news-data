---
schema_version: "1.0.0"
document_id: "cfbce3a83c69ef43849b0051cbee55225fe4bdaa0d333fbbb34fb2582858aacd"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-memory-management"
published_at: "2026-05-26T01:04:51+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:fbab17847a12957eb622870ac800c85d290ddefa4c49377294d8d8b33dcc6a1f"
---

# Claude Code Memory Management: How to Stay Sharp in Long Coding Sessions

## CLAUDE.md: Your Agent's Persistent Memory


The most underused context management tool in Claude Code is CLAUDE.md. It's a file Claude reads at the start of **every session** — making it your only true persistent memory across context resets.


Whatever you put in CLAUDE.md doesn't need to be repeated, re-taught, or re-established after a` /clear` . It loads automatically.


**What belongs in CLAUDE.md:**


```text
# Project: YourApp


## Commands
-   `npm run dev`   — local dev server (port 3000)
-   `npm test`   — runs Jest, watch mode
-   `npm run lint`   — ESLint + TypeScript check
-   `npm run db:push`   — apply Prisma schema changes


## Architecture
-   Next.js 15 App Router — all pages in   `src/app/`
-   Prisma + PostgreSQL for the database
-   Clerk for authentication (already configured)
-   All API routes in   `src/app/api/`


## Code Style
-   TypeScript strict mode — no   `any`   types
-   Use   `const`   over   `let`   everywhere
-   Named exports only (no default exports)
-   Error handling: always   `console.error`   before returning in catch blocks


## Key Constraints
-   Never use   `npm run build`   — it destroys the dev server
-   Never commit with   `git add -A`   — stage files explicitly
-   Database migrations require a   `.sql`   file alongside schema changes


## Testing
-   Run   `npm test -- --testPathPattern=filename`   for single file
-   All new API routes need a test in   `src/lib/auth/api-security.test.ts`
```


**The CLAUDE.md anti-pattern to avoid:**


Bloated CLAUDE.md files cause Claude to ignore sections because they're buried in noise. The official Claude Code documentation is direct: "If Claude keeps doing something you don't want despite having a rule against it, the file is probably too long."


The rule: for every line in CLAUDE.md, ask "Would removing this cause Claude to make mistakes?" If not, delete it.


**CLAUDE.md can import other files:**


```text
# Main CLAUDE.md
See @README.md for project overview.


## Git Workflow
@docs/git-instructions.md


## API Conventions
@docs/api-conventions.md
```


This keeps the main file short while preserving depth. Claude loads the imports when relevant.


## Structuring Long Sessions for Context Efficiency


For codebases that span multiple sessions, the single most effective pattern is **session segmentation** : each session has one clear scope, and you document the handoff before ending it.


**The session size rule:** aim for sessions under 5,000 active tokens (roughly 15,000 words of conversation). When you exceed this, either` /compact` or end the session and start fresh with a handoff.


**Task-per-session pattern:**


Instead of one long session that covers planning + implementation + debugging + testing, break work into discrete sessions:


```text
Session 1: Explore + Plan → output: SPEC.md
Session 2: Implement auth → output: modified files list
Session 3: Implement dashboard → output: modified files list
Session 4: Write tests → output: test results
Session 5: Debug + fix → output: bug report + patches


```


Each session starts with a focused prompt. The AI never gets overloaded with unrelated earlier context.


**AGENTS.md for large codebases:**


For larger codebases, Claude Code supports multiple CLAUDE.md files — one at root and additional ones in subdirectories. Claude loads the most relevant one based on what files it's working with.


Structure:


```text
project/
├── CLAUDE.md           ← global rules (dev commands, style, constraints)
├── src/
│   ├── auth/
│   │   └── CLAUDE.md   ← auth-specific patterns and gotchas
│   ├── api/
│   │   └── CLAUDE.md   ← API conventions, route auth requirements
│   └── db/
│       └── CLAUDE.md   ← migration rules, schema safety


```


## The Handoff Protocol: Ending and Resuming Sessions Cleanly


A handoff prompt is a message you send at the END of a session to capture everything important for the next session. Copy the output into a` HANDOFF.md` file or into your CLAUDE.md.


**Handoff prompt template:**


```text
Before we end this session, write a handoff summary with:
1. What we implemented today (specific files changed and why)
2. What was left incomplete (with exact stopping point)
3. The next 3 specific tasks to do next session
4. Any decisions we made that aren't reflected in the code yet
5. Any bugs we found but didn't fix (with file and line number)
Format: markdown, concise, no fluff


```


**How to start the next session:**


```text
I'm resuming work on [project]. Here's the handoff from last session:
[paste HANDOFF.md contents]


Start by confirming you understand the current state, then ask me what to prioritize first.


```


This pattern means Claude Code starts every session at full performance — not gradually working out context from scratch.


**Named sessions:**


Use` /rename` to give sessions descriptive names:` oauth-migration` ,` dashboard-v2` ,` stripe-integration` . When you need to resume, run` claude --resume` and choose from the list. Each session is a focused workstream, not one endless conversation.


## Common Context Mistakes and How to Fix Them


**The kitchen sink session.** You start debugging a payment bug, get distracted, ask Claude to refactor something else, then go back to the original bug. The context is full of irrelevant information. Fix:` /clear` between unrelated tasks, always.


**The correction loop.** Claude does something wrong. You correct it. Still wrong. You correct again. The context is now polluted with failed approaches, and Claude is pattern-matching against its own mistakes. Fix: After two failed corrections,` /clear` and write a better prompt incorporating what you learned.


**The bloated CLAUDE.md.** Your CLAUDE.md is 400 lines. Claude ignores 80% of it because important rules are buried. Fix: Run an audit every two weeks. Delete every line that Claude already does correctly without instruction.


**The infinite exploration.** You ask Claude to "investigate" the codebase without scoping it. It reads 50 files, consuming your context. Fix: Scope investigations narrowly. Or use subagents: "Use a subagent to investigate how our authentication system handles token refresh." The subagent explores in a separate context window and reports back a summary — without consuming your main session.


**The file-read avalanche.** Claude reads a dozen large files to understand context before responding. Each read expands the context. Fix: Use` @file` references instead of having Claude read files it doesn't immediately need. Reference files explicitly: "Look at` src/auth/session.ts` and` src/api/middleware.ts` specifically."


The context management workflow for long Claude Code sessions — from bloated to focused


Blink


## Build This With Your AI Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent: "Build me a full-stack app with proper CLAUDE.md context management and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account. The Blink plugin installs Blink-specific skills into Claude Code so the agent knows exactly how to build and deploy to Blink's infrastructure without configuration steps.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


` /clear` wipes the entire conversation history — Claude starts with zero memory of your session.` /compact` summarizes the conversation intelligently, preserving key decisions and code context while condensing verbose output. Use` /clear` when switching to an unrelated task or after a session has gone badly wrong. Use` /compact` when you're mid-task and want to free up context without losing your progress.


Use` /compact` proactively — before you hit the context limit, not after you notice degradation. A good rule:` /compact` after every major feature completion, after any debugging session longer than 30 minutes, and before switching from planning to implementation. You can also set up automatic compaction by configuring instructions in your CLAUDE.md.


CLAUDE.md should contain: all dev commands (run, test, lint, deploy), architectural patterns specific to your project, hard constraints Claude must never violate (like "never run npm run build"), code style rules that differ from defaults, and environment variable names and their purpose. Don't include: API documentation (link to it), standard language conventions, self-evident practices, or anything Claude already does correctly without instruction.


Not automatically. Each new session starts fresh. But CLAUDE.md is loaded at the start of every session — making it persistent memory. Write your most important project context there. For session-to-session continuity, use the handoff prompt pattern: ask Claude to write a summary at the end of each session and paste it at the start of the next one.


Claude Code shows a context usage indicator. You'll also notice behavioral signals: Claude starts repeating suggestions you've already discussed, contradicts earlier architectural decisions, or gives generic answers instead of project-specific ones. These are signs of context saturation — time to` /compact` or` /clear` .


Use multiple CLAUDE.md files — one at the project root and additional ones in key subdirectories (` src/auth/` ,` src/api/` ,` src/db/` ). Use session segmentation: one session per feature or subsystem, with a handoff prompt at the end. Use subagents for investigation tasks so exploration doesn't consume your main session context. Name your sessions descriptively so you can resume the right one.


---


**Related reading:**


- [CLAUDE.md Best Practices: Writing Persistent AI Memory](https://blink.new/blog/claude-md-best-practices)
- [Claude Code Tutorial for Beginners](https://blink.new/blog/claude-code-tutorial-beginners)
- [Context Engineering for AI Coding](https://blink.new/blog/context-engineering-ai-coding)
- [Blink Cloud: Full-Stack Hosting for AI-Built Apps](https://blink.new/cloud)
- [Blink Skills for Claude Code](https://blink.new/docs/cloud/tools/skills)
