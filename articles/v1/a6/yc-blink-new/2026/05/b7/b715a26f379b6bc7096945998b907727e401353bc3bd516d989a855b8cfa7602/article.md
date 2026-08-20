---
schema_version: "1.0.0"
document_id: "b715a26f379b6bc7096945998b907727e401353bc3bd516d989a855b8cfa7602"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/spec-driven-development-with-ai"
published_at: "2026-05-14T12:23:57+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:55.365924+00:00"
content_hash: "sha256:bf757f6c1e67565f40eb34a136ead6dc792f85903596f27f14293e817ad39248"
---

# Spec-Driven Development: Write the Spec First, Let AI Build It

## Writing the Spec: Step-by-Step


1


#### Generate a draft spec, don't code yet


Describe what you want to build to your agent and ask it to write a spec — not implement the feature. In Claude Code, start in Plan Mode (Shift+Tab) where the agent reads your codebase and drafts a plan without writing any code. Review and correct its assumptions before touching a line of implementation.


2


#### Fill in constraints aggressively


Every time you've been surprised by an AI decision in the past, add that situation as a constraint. "Do not add new dependencies." "Do not modify existing tests." "Do not change the database schema." Think about what the agent might helpfully do that you'd immediately revert.


3


#### Write verifiable acceptance criteria


For each requirement, write a command you can actually run:` make test` ,` curl /api/endpoint` ,` npx eslint src/` . If you can't write a concrete acceptance criterion, the requirement is too vague — sharpen it before handing it to the agent.


4


#### Open a fresh session for execution


Don't ask the same agent instance to plan and then execute in the same session. Planning and execution are different cognitive modes. A fresh session with the spec as context means the agent executes against it cleanly, without carrying forward any planning-mode assumptions.


5


#### Work task by task, commit each one


Break the spec's work into discrete tasks and implement them one at a time. Review, commit, then move to the next. This keeps the blast radius of any mistake small and gives you clean git history to roll back if needed.


## The Spec in Practice: Before vs After


Before and after spec-driven development — vague prompts create code chaos, structured specs produce clean organized code


Blink


Same feature request. Two very different outcomes.


**Before (vague prompt):**


```text
"Add pagination to the users API"


```


Agent output: Implements offset-based pagination (not what you wanted), adds a new` page` query param, changes the response schema, creates a new test file that conflicts with existing ones. Three correction rounds. 45 minutes.


**After (spec-first):**


```text
## Goal: cursor-based pagination on GET /api/users
## Requirements: keyed on created_at, default 50, max 200, backward-compatible
## Constraints: no schema changes, no new dependencies, preserve existing response format
## Acceptance criteria: make test passes, curl examples return expected shape
```


Agent output: Implements exactly cursor-based pagination, backward-compatible, no schema changes, tests pass. One round. 15 minutes.


The research from amux.io backs this up quantitatively. They tracked 10-task batches with and without specs. Without specs, each task averaged 2–3 hours of back-and-forth. With specs, the same 10 tasks took roughly 4 hours total — including spec-writing time. **That's a 5–7x throughput difference, and it widens with every additional agent you run in parallel.**


MIT Economics measured a 26% productivity gain across 4,867 developers using AI tools in field conditions. The developers who captured the most of that gain were the ones giving agents structured, constrained tasks — not open-ended prompts.


## When NOT to Write a Spec


Spec-driven development is not the answer to every task. Writing a spec has overhead, and that overhead only pays off above a certain complexity threshold.


**Skip the spec when:**


- You're fixing a typo or a one-line bug — just do it
- You're prototyping something you'll throw away in 30 minutes
- The task takes less than 15 minutes of agent work
- You're exploring whether an approach even makes sense


**Write the spec when:**


- The feature touches more than 2–3 files
- You've been surprised by agent decisions on similar tasks before
- The work will span multiple sessions
- You're running the agent unattended or overnight
- Multiple agents will implement different parts in parallel


Owain Lewis, who writes extensively about AI agent workflows, puts the threshold cleanly: *"If you're working on something large that might split into many tasks or run over multiple sessions — write a spec. If you're fixing a simple bug, just do it."*


The right answer isn't always spec-first. The right answer is to match the level of specification to the complexity of the task.


## Build Your App Spec-First With Claude Code and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Write a spec for a task management app with user authentication, a REST API, and a PostgreSQL database. Once I approve the spec, build it and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## FAQ


Aim for 10–30 lines. Under 10 lines usually means you left out constraints. Over 50 lines usually means you're over-specifying implementation details — you're writing the code yourself with extra steps. The sweet spot is 5–10 minutes of writing.


Both work, but write it yourself when you can. The act of writing a spec forces you to make decisions you'd otherwise delegate to the agent. Ask the agent to draft a spec only as a starting point, then review and revise aggressively — the agent will make assumptions that reveal where your requirements are still unclear.


A CLAUDE.md file (or similar system prompt) sets global rules for how your agent behaves across all tasks: code style, test framework, git conventions, boundaries. A spec is task-specific: what to build, what to constrain, how to verify it. You need both. The spec inherits the global constraints from CLAUDE.md and adds task-specific ones on top.


Two common causes: the spec had an ambiguous requirement the agent resolved with a guess (add that constraint explicitly for next time), or the agent's context window got overloaded mid-implementation and it started losing track of later constraints. The fix for the second is to work task by task with smaller sessions, not one massive implementation run.


Yes — the approach is tool-agnostic. A spec is just a markdown file. You can paste it into Cursor's composer, reference it with @SPEC.md, or use it as the starting prompt in any agentic coding tool. The key workflow principle — write first, execute second — works regardless of which agent runtime you're using.
