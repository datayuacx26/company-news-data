---
schema_version: "1.0.0"
document_id: "dab9ea6781d373fa2d1d0594052613735d9a839c6c6d9e8c54e98f206d027962"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/ai-coding-workflow"
published_at: "2026-05-17T12:49:08+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:15:16.761066+00:00"
content_hash: "sha256:402c338091dc33a5fb6ac7e8509dbf9251e405a87106d8259d3f44a5cd65422b"
---

# The AI Coding Workflow That Actually Ships: Spec → Plan → Execute → Verify

## The tools in this workflow


The four phases map to specific tool capabilities:


**Claude Code** is the executor. Plan mode (Shift+Tab) for Phase 2. Agentic mode for Phase 3.` CLAUDE.md` for the persistent spec layer that survives context resets. Claude Code now holds the highest satisfaction rating in the market — 91% CSAT in JetBrains' January 2026 survey of over 10,000 developers worldwide.


**Cursor** is the IDE layer.` .cursorrules` files handle the spec context for developers who prefer staying in the editor. Cursor doesn't have a dedicated plan mode, but you get the same effect by asking for a numbered plan explicitly before saying "now implement it."


**CLAUDE.md / AGENTS.md /` .cursorrules`** are where your spec lives in a form the AI reads on every session. GitHub's analysis of 2,500+ agent configuration files found the most effective ones use a three-tier boundary system:` ✅ Always do X` ,` ⚠️ Ask before Y` ,` 🚫 Never Z` . "Never commit secrets" was the single most common constraint across all 2,500 files. Simple, direct, unambiguous.


**Blink Cloud** is the deployment layer — once the code is verified and committed, Blink provisions the database, auth, backend, and hosting automatically. No separate Vercel config, no Supabase account setup.


## What breaks the workflow


These are specific failure modes — not warnings, but patterns that reliably produce bad output:


**Task-switching mid-session.** You're 20 minutes into a refactor and notice an unrelated bug. You ask the AI to fix it. The context now covers two tasks. The output reflects both, and not cleanly. The fix is one context window per task, always.


**Skipping the plan step.** You write a good spec and immediately say "go build it." The AI produces something that technically satisfies the spec but makes architectural choices you wouldn't have made. This happens because unspecified implementation decisions get filled in by the AI's heuristics, not your knowledge of the codebase.


**Too much scope per session.** "Build the auth system" is a project. "Create the user registration endpoint that validates email format and hashes the password with bcrypt" is a task. Larger scopes produce output that's harder to verify and accumulates more scope creep.


**Relying on AI for design decisions.** "What database should we use?" or "Should this be a microservice?" are architectural decisions that require your team's constraints. The AI will give you an answer. It won't be wrong exactly — but it won't be informed by what you actually need.


**No commit before starting.** If you skip committing before the next task, a bad task's rollback costs you two tasks' worth of work.


The AI coding workflow in action: small-scoped tasks executing sequentially with clean commits between each one, no mid-session pivots or scope creep


Blink


*The AI coding workflow in action: small-scoped tasks executing sequentially with clean commits between each one, no mid-session pivots or scope creep*


## Workflow by task type


The four phases apply to every task, but the emphasis shifts:


**Bug fix:** The spec is the repro case — exact input, expected output, actual output. Phase 2 is often one line: "fix the condition on line 47 that causes X." Execution is fast. Verification requires confirming the original repro no longer triggers and no regressions appear.


**New feature:** This is the full four-phase workout. Vague feature specs produce vague output. Write user-facing behavior first, then technical requirements. Review the plan carefully — new features have more architectural surface area than fixes, so the plan review matters most here.


**Refactor:** The spec needs to state what *shouldn't* change more clearly than what should. "Refactor the payment module to use the new API — external behavior must not change, all existing tests must still pass." The constraint boundary is as important as the goal.


**Architecture decision:** Don't use this workflow here. Architecture is design work, not execution. Use AI to generate options and enumerate tradeoffs. Make the decision yourself. Handing design decisions to the executor without your judgment in the loop is how you end up with "technically correct but strategically wrong" systems.


## Measuring if it's working


Three metrics that tell you whether the workflow is actually improving output:


**Revisions per task** — how many back-and-forths does a single task require before it passes verification? Consistently more than two means the spec or task scope needs work. Good specs with small scopes converge in one or two rounds.


**Context resets per day** — how often do you start a fresh session because the AI has lost the thread? At most once per major task. More than that means tasks are too large, or the persistent context layer (CLAUDE.md) isn't carrying enough project knowledge.


**PR review time** — compare time to review an AI-assisted PR versus a human-written one. Per Sonar's data, 38% of developers currently find AI code harder to review. If that's your experience, tighter scope and better specs — not more verification steps — is the fix.


## Build This Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up this project with CLAUDE.md conventions for spec-driven development and deploy it on Blink with database, auth, and hosting."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


---


A spec is per-task or per-feature — it describes what you're building right now: the problem, constraints, success criteria for this task.


CLAUDE.md is persistent project context. It contains what should be true for every task: your tech stack, coding conventions, what the AI should never do, how to run tests, branch naming.


You write a new spec for each task. You update CLAUDE.md once and it persists across every session, surviving context resets automatically.


Yes. Cursor's agent mode follows the same structure. Your spec goes into` .cursorrules` or a` SPEC.md` you reference explicitly at the start of each session.


Cursor doesn't have a dedicated plan mode the way Claude Code does (Shift+Tab), but you get the same outcome by asking for a numbered plan before approving execution. The phase separation is what matters — the tool doesn't enforce it, you do.


A task should be completable and verifiable in one session without a context reset.


In practice: one function, one endpoint, one component, or one database migration — not one feature. If you're writing a spec that covers 5 endpoints at once, split it into 5 tasks.


The heuristic: if you can't describe the success criteria in two sentences, the task is too large.


Use the three-tier system from GitHub's analysis of 2,500+ agent config files: ✅ Always, ⚠️ Ask first, 🚫 Never.


Concrete examples:


- ✅ Always run tests before committing
- ✅ Always follow the existing naming conventions in the codebase
- ⚠️ Ask before modifying database schema
- ⚠️ Ask before adding a new dependency
- 🚫 Never commit secrets or API keys
- 🚫 Never modify the CI config without explicit instruction


Start with "never commit secrets" — it was the single most common constraint across all 2,500 files analyzed. Add project-specific rules as you discover what the AI gets wrong in your context.
