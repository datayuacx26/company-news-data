---
schema_version: "1.0.0"
document_id: "6c221aeb6ece56b64d251b84bc9bb377aba6381741ccda38b4c3cfcd86c723d4"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-tips-tricks"
published_at: "2026-06-03T13:27:18+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:c6f8648a2c275e8eb033d4b5ec94e19fa33fb42a53b4cedf48be98333e98fde9"
---

# Claude Code Tips and Tricks: 15 Power User Moves

## Tips 6–10: Workflow That Actually Scales


### 6. Shift+Tab into Plan Mode before any multi-file change


**The problem:** Claude jumps straight to coding. It opens five files you didn't intend, makes related changes that break adjacent functionality, and you spend 20 minutes untangling it.


**The fix:** Press` Shift+Tab` before running any prompt that touches multiple files. Plan Mode activates — Claude reads, analyzes, and proposes, but *nothing gets changed* . You see exactly what it intends to touch and why. Edit the plan with` Ctrl+G` to remove files you don't want changed, then approve.


**Before:** Run prompt → Claude edits 5 files → 2 of them shouldn't have been touched → cleanup takes longer than the original task.


**After:** Run prompt in Plan Mode → Claude lists exactly which files and what changes → remove the 2 you don't want → approve → zero surprises.


For any change touching more than 2 files, Plan Mode is faster overall, even accounting for the review step.


### 7. Run 3–5 parallel sessions in git worktrees


Sequential sessions are the biggest productivity ceiling in Claude Code. One instance, one task, one thread.


Run` claude --worktree` to spin up a session in an isolated git worktree — its own branch, its own directory, its own context. Multiple worktrees run simultaneously without polluting each other's state.


Three patterns that work well:


- **Writer/reviewer:** Session A implements the feature. Session B writes tests and reviews the diff.
- **A/B approach:** Two sessions try different implementations of the same problem. You pick the better one.
- **Parallel features:** Auth on one branch, dashboard on another. Both done in 35 minutes instead of 60.


For developers pushing toward fully autonomous multi-agent workflows, see[OpenClaw vs Claude Code](https://blink.new/blog/openclaw-vs-claude-code) for how managed agent hosting changes what parallel sessions can do.


### 8. --dangerously-skip-permissions — and when to actually use it


Constant approval prompts break flow. Most developers start click-through-approving without reading. That defeats the safety purpose entirely.


There's a spectrum — use the right level for each context:


- **Default mode:** every command prompts for approval
- **` /permissions` allowlist:** pre-approve safe commands (` Bash(npm run lint)` ,` Bash(npm test)` ) while leaving prompts on for destructive operations
- **` --dangerously-skip-permissions` :** full autonomy — all prompts removed — for repos you fully control with no production access


For most developers, the` /permissions` allowlist is the right default. Pre-approve the commands you run twenty times a day; keep prompts for anything that writes to a database or touches production.` --dangerously-skip-permissions` is for local, trusted, sandboxed repos where you want zero friction.


### 9. /effort levels: match thinking depth to task complexity


Default effort level misallocates in both directions — burning expensive tokens on variable renaming, and under-thinking gnarly architecture problems.


Three settings to internalize:


- ` /effort low` — renaming, reformatting, mechanical changes. Fast. Cheap. Right for 40% of tasks.
- Default — feature work, most bug fixes
- ` /effort max` — complex debugging, database schema design, performance investigation


For one-off deep reasoning without changing your global setting, use` ultrathink` inline:


```text
ultrathink — Design a migration strategy for moving this single-tenant user table to a multi-tenant schema without downtime.


```


**Before:** Claude gives a reasonable but shallow architecture suggestion for a problem that deserved ten minutes of deep thought.


**After:** The` ultrathink` prompt returns three distinct approaches with trade-off analysis — including the race condition in approach 2 that would have caused a production incident.


### 10. Use subagents to protect your main context window


**The problem:** Investigating an unfamiliar codebase eats context before you write a single line. After Claude opens 20 files researching how your auth token refresh works, you've burned 30K tokens on research, not implementation.


**The fix:** Delegate investigation to subagents. Prompt:` "Use a subagent to investigate how our token refresh logic handles concurrent requests and report back a summary."` The subagent reads dozens of files in its own isolated context window. Only the conclusion — the summary — enters your main session.


Good use cases for subagent delegation:


- Deep investigation of unfamiliar code areas
- Code review of a large diff you want a second perspective on
- Writing documentation by reading source
- Verifying that a fix didn't break adjacent code in files you didn't touch


Claude Code subagents handling separate investigation tasks — each working in its own context window so your main session stays clean


Blink


## Tips 11–15: Power User Territory


### 11. PostToolUse hooks: automation Claude can't ignore


**The problem:** AI-generated code skips formatting. Your CI fails on style checks. You fix it manually every time.


**The fix:** Add a PostToolUse hook in` .claude/settings.json` . It fires after every matching tool use — deterministic, unlike CLAUDE.md instructions, which are advisory and occasionally skipped when context is tight.


```text
{
"hooks"  : {
"PostToolUse"  : [{
"matcher"  :   "Edit|Write"  ,
"hooks"  : [{
"type"  :   "command"  ,
"command"  :   "prettier --write   \"  $CLAUDE_FILE_PATHS  \"  "
}]
}]
}
}
```


Every file edit gets formatted automatically. CI passes. You never touch formatting again.


The distinction matters: CLAUDE.md is advice. Hooks are enforcement. Use CLAUDE.md for preferences and style; use hooks for anything that must happen on every single file write.


### 12. /loop and /schedule: put recurring tasks on autopilot


**The problem:** PR review monitoring, CI check babysitting, stale branch cleanup — you keep interrupting real work to check on these manually.


**The fix:**` /loop 30m Check for new PR review comments and address them` — Claude handles it every 30 minutes while you focus on something else.


For tasks that need to run even when Claude Code is closed,` /schedule` accepts cron syntax:


```text
/schedule "0 * * * *" Run the migration health check script and report any failures to Slack


```


The Anthropic team's internal examples are instructive:` /loop 5m /babysit` to shepherd PRs,` /loop 1h /pr-pruner` to close stale ones. These run in the background. You get the output when the loop completes.


### 13. Custom slash commands for your team's shared workflows


**The problem:** Every developer on your team re-prompts the same multi-step workflows — code review, migration checks, test generation — from memory, with different wording, getting inconsistent results.


**The fix:** Add a` .md` file to` .claude/commands/` and it becomes a slash command. Commit the directory to git and every developer on the team shares the same commands.


Starting commands worth creating:


- ` .claude/commands/review.md` →` /review` runs your structured code review (security, performance, test coverage) every time
- ` .claude/commands/migrate.md` →` /migrate` verifies migration safety and confirms rollback plan before you run anything
- ` .claude/commands/test.md` →` /test \[component\]` generates a consistent test suite for whatever component you pass


The improvement over copy-paste prompts:` /review` runs the same thorough prompt every time, regardless of who's running it or how tired they are.


### 14. Multi-repo work with --add-dir


**The problem:** Real features often span multiple repositories. You copy-paste code between them, run parallel sessions without shared context, or lose the thread switching between windows.


**The fix:**` claude --add-dir ../shared-lib --add-dir ../api-service` gives one Claude session simultaneous read/write access to multiple repositories. All in scope, all editable, no copy-paste.


Add` additionalDirectories` to` settings.json` for directories that should always be included:


```text
{
"additionalDirectories"  : [  "../shared-lib"  ,   "../api-service"  ]
}
```


One session. Full context across your entire stack.


### 15. Enable /memory for persistent cross-session knowledge


The` /memory` command saves notes that survive` /clear` and session restarts. Unlike CLAUDE.md (which you manage),` /memory` is Claude's own working notes about your project.


Use it actively: after any decision, preference, or discovery you want Claude to remember long-term, run` /memory Add: \[what Claude should know\]` . Claude reads these notes at the start of every session before it reads anything else.


Pairs well with the CLAUDE.md update workflow from Tip 1. Think of them as two layers:


- **CLAUDE.md** — your project conventions and rules (developer-authored, explicit)
- **/memory** — Claude's own working knowledge (Claude-authored from corrections and context)


A month of consistent use leaves Claude operating with context that previously would have taken 20 minutes of onboarding at the start of every session.


## Build a Full-Stack App With Claude Code


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build a full-stack app and host it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Only include things that change Claude's default behavior for your specific project: preferred import style, test runner commands, branch naming, linting rules, and explicit prohibitions ("never run migrations without confirmation"). Don't include general coding best practices Claude already knows — you're filling context with noise. The r/ClaudeCode community's consistent advice is to keep it under 200 lines. Past that point, Claude starts deprioritizing the bottom content during heavy sessions.


Use` /compact` when you want to preserve progress on the *current task* but free up context mid-session. Pass a hint (` /compact Focus on the API changes` ) to control what gets preserved. Use` /clear` when you're completely done with one task and switching to something unrelated — it wipes the slate clean. If you've been in the same session across multiple different tasks for 90+ minutes,` /clear` is almost always the right call.


It removes all confirmation prompts, so Claude can run any command without asking. In a local repo you fully control with no production access, the practical risk is low — Claude won't randomly destroy things. In a shared repo, one with CI/CD hooks, or anything touching production data, use the` /permissions` allowlist instead: pre-approve safe commands like` npm test` while keeping prompts on for destructive operations. Reserve` --dangerously-skip-permissions` for sandboxed local environments.


Run` claude --worktree` to create each session in an isolated git worktree — its own branch and working directory. Sessions run simultaneously without sharing state. A common pattern: Session A implements, Session B reviews and writes tests. Use` /color` to color-code terminal tabs so you know which session is which at a glance. Three parallel sessions is a comfortable upper bound before coordination overhead starts eating the time savings.


Plan Mode (Shift+Tab) enforces a hard gate: Claude cannot modify any files while planning. It reads and proposes, and nothing changes until you explicitly approve. Asking Claude to "plan first" in a normal session is advisory — Claude might decide mid-plan that it needs to create a test file or update a config, and do it. For anything touching more than two files, Plan Mode is the reliable choice.


CLAUDE.md instructions are advisory. Claude reads them and follows them when context permits, but can reinterpret or skip them if the session is context-heavy or if a more specific instruction takes priority. Hooks in` .claude/settings.json` are deterministic — they fire after every matching tool use regardless of what Claude decided or how full the context is. Use CLAUDE.md for preferences, style, and conventions. Use hooks for things that must happen every single time without exception.
