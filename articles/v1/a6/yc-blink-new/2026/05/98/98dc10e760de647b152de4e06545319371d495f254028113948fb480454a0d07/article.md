---
schema_version: "1.0.0"
document_id: "98dc10e760de647b152de4e06545319371d495f254028113948fb480454a0d07"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-plan-mode-guide"
published_at: "2026-05-03T12:15:38+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:56:02.721351+00:00"
content_hash: "sha256:8b52faf9f411e935923356bc82f08f22ce487db3a767e2da5e0ec3777d592fe2"
---

# Claude Code Plan Mode: The One Workflow That Prevents 90% of Mistakes

## What a plan looks like


Here's a real example. Task: "add rate limiting to the login endpoint."


Without plan mode, Claude Code starts writing immediately. With plan mode, it outputs:


```text
Plan for: add rate limiting to the login endpoint


1. Read src/app/api/auth/login/route.ts — understand current auth logic
2. Read src/lib/rateLimit.ts (if exists) — check for existing rate limit utilities
3. Install or verify: redis client in package.json (rate limiting needs persistent storage)
4. Create src/lib/rateLimit.ts — IP-based rate limiter using Redis
- 5 attempts per 15 minutes per IP
- Return 429 with Retry-After header on limit
5. Modify src/app/api/auth/login/route.ts — add rate limit check before auth logic
6. Write src/middleware.ts — apply rate limiter to /api/auth/login route
7. Run: npm test src/app/api/auth/ — verify no regressions


Approve this plan? [y/n/r(efine)]


```


That plan surfaces a dependency decision you might not have anticipated (Redis). You can catch it here, redirect the agent to use an in-memory store instead, and avoid an unexpected npm install mid-execution.


## Why developers who skip plan mode regret it


The most common Claude Code frustration on r/ClaudeAI and developer Twitter is a version of: "It went off in the wrong direction and now I have a mess to fix."


Trace it back and the pattern is always the same: complex multi-file task, no plan mode, execution started before the agent's interpretation was confirmed.


Claude Code is excellent at execution. It can misread scope — write a solution that technically fulfills the literal prompt while missing the intent. Plan mode surfaces that misread *before* three files have been changed.


The fix is one keypress. The regret is avoidable.


## Plan mode vs default mode vs auto mode


Mode When agent acts Your role Best use case


**Default** After your approval per action Approve each file write and shell command Routine edits in a known codebase


**Plan** After you approve the full sequence Review the plan, then approve or refine Complex tasks, unfamiliar code, high-stakes changes


**Auto** Immediately, no prompts Monitor and interrupt if needed Trusted, fast iteration on known tasks


Plan mode is the highest-oversight option. Auto mode is the lowest. Default mode sits in between.


Power users often run plan mode for the first pass on any new task, then switch to auto once the plan is confirmed.


## When plan mode is most valuable


**Unfamiliar codebases.** When you've just joined a project or are working in a module you don't know well, plan mode tells you what the agent thinks the right approach is — before it starts refactoring code you didn't expect it to touch.


**Large multi-file changes.** Refactors that touch 10+ files have more surface area for misinterpretation. Plan mode lets you verify scope before execution.


**Any change near auth, payments, or data access.** These are the highest-stakes parts of any application. Seeing the full plan before any edits run gives you a final review gate that the per-action approval in default mode doesn't offer (because by the time you see action #7, you've already approved actions 1-6).


**After a context reset.** If you've run` /clear` or started a fresh session, Claude Code is rebuilding its understanding of your project. Plan mode confirms it got the context right before acting.


## The refinement loop


Approving a plan isn't the only option. You can refine it before execution:


```text
Approve this plan? [y/n/r(efine)]
r


> Don't use Redis for rate limiting — we're not running Redis in this project.
Use an in-memory store with a 15-minute sliding window instead.


[Claude Code updates the plan]


Updated plan:
1. Read src/app/api/auth/login/route.ts
2. Create src/lib/rateLimit.ts — in-memory rate limiter, Map-based, 15-minute TTL
...


Approve this plan? [y/n/r(efine)]
y


```


The plan mode loop is: see the plan → check it against your intent → refine if needed → approve → execute. That loop takes 60 seconds on most tasks. Rework takes 30 minutes.


## Build the plan mode habit


Most experienced Claude Code users converge on the same workflow: plan mode for anything non-trivial, auto mode for small iterative changes where the scope is obvious.


The trigger for switching to plan mode:


- The task involves more than 2 files
- The task touches auth, payments, or database access
- You haven't worked in this part of the codebase recently
- The task has external dependencies (new npm packages, environment variables)


For developers building with Claude Code and deploying to Blink, plan mode pairs well with Blink's deployment preview: plan the code change, approve, execute, see the preview URL, deploy to production.


## Build What You Build With Claude Code on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build a full-stack app using Claude Code plan mode and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Plan mode is a permission setting in Claude Code that makes the agent read your files, reason through the task, and output a full action plan before making any changes. You review the plan, approve or refine it, and only then does execution begin. It's the highest-oversight mode and catches the most common class of Claude Code mistakes: misinterpreted scope.


Three ways: launch with` claude --permission-mode plan` , press` Shift+Tab` mid-session to cycle to plan mode, or prefix any single task with` /plan` . To make it the default for a project, add` {"permissions": {"defaultMode": "plan"}}` to` .claude/settings.json` in your project root.


Default mode asks for your approval before each individual action — each file write, each shell command. Plan mode shows you the entire sequence of planned actions before any of them run. Plan mode is better for complex tasks because you see the full scope upfront; default mode is fine for simple, targeted edits.


Yes. When Claude Code presents a plan, you can type` r` (or just describe the change) to refine it. Claude Code revises the plan based on your feedback and presents the updated version. You keep refining until it matches your intent, then approve. This loop typically takes under a minute.


For complex tasks (3+ files, auth or payments, unfamiliar code), yes — always. For simple one-file edits where the scope is obvious, auto mode or default mode are faster. Many developers use plan mode for every new task type, then switch to auto once they've seen the agent handle similar tasks correctly a few times.
