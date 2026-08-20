---
schema_version: "1.0.0"
document_id: "671eecfc95fc716860f3dd26e372b9ebe030331cda2ffb8371116a1a9697b9ad"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/context-engineering-ai-coding"
published_at: "2026-05-17T12:48:24+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:4ad1bf4873cfaa768ed1c16f96b2a8289618767e5e1c915fd2646724cc7397bf"
---

# Context Engineering for AI Coding: The Skill That Separates Good From Great

## The 5 Principles of Context Engineering


5 context engineering principles for AI coding: fresh sessions, front-load architecture, compact, scope, verify


Blink


### 1. Start Fresh for New Tasks


Every context window accumulates from session start. Work you did in the morning — architectural discussions, debug sessions, half-finished features — stays in context unless you clear it.


That accumulated context helps for related work. For unrelated tasks, it hurts.


**The rule:** When you switch to a meaningfully different task, start a new session. In Claude Code,` /clear` wipes the conversation history. In Cursor, open a new chat window.


The hesitation is "I'll lose context." What you actually lose is *stale* context. A new session for the afternoon task — loaded with only what that task needs — produces better output than a session still carrying morning's half-finished migrations.


### 2. Front-Load Architecture Context


The highest-leverage technique in context engineering is a project context file: a CLAUDE.md or AGENTS.md at your repository root that the agent reads at session start.


This file is your onboarding document for the AI. Include:


- Tech stack and versions
- Naming conventions ("components are PascalCase, utilities are camelCase")
- Test frameworks and where tests live
- What NOT to do ("never use` any` in TypeScript, no direct DOM manipulation")
- Architectural decisions already made — and why you made them


Claude Code reads CLAUDE.md automatically when you run` claude` from a project directory. Cursor reads` .cursorrules` . The file format matters less than having one.


A well-maintained CLAUDE.md eliminates the most persistent failure mode: the agent confidently suggesting an approach you already evaluated and rejected. See our[CLAUDE.md best practices guide](https://blink.new/blog/claude-md-best-practices) for a full 10-section template.


### 3. Compact Long Sessions


Long sessions are unavoidable. A complex feature might take two hours of back-and-forth. But a context window that's 90% full means the agent's attention is stretched across hundreds of decisions, responses, and tool outputs — most of them no longer relevant to what you're doing right now.


Compaction solves this. In Claude Code,` /compact` triggers the model to summarize the conversation, preserving architectural decisions, open bugs, and current implementation state while discarding redundant tool outputs and old responses.


The result is a compressed context that maintains continuity without the noise. Anthropic's engineering team describes compaction as "the first lever in context engineering to drive better long-term coherence."


Use it: every 60-90 minutes in a continuous session, or when the agent starts repeating mistakes it had already corrected.


### 4. Scope Per-Task


"Here's everything about our codebase" is worse context than "here's exactly what you need for this task."


Agents don't benefit from knowing about your payment service while they're fixing your auth layer. Extra context doesn't just waste tokens — it creates noise the model has to filter through. Because transformer attention scales at n² pairwise relationships, every new token adds to a computation budget that gets stretched thin at scale.


For each task, load the minimum viable context:


- The specific files this task touches
- The conventions relevant to this area of the codebase
- The explicit scope boundary ("do NOT modify X")


This sounds tedious. In practice, it's the opposite. Tasks completed in 3 minutes with scoped context routinely take 30 minutes when the agent has to navigate a sprawling codebase to figure out what it's allowed to touch.


### 5. Verify Context Accuracy Before Executing


Before the agent runs anything consequential — a migration, an auth refactor, a bulk rename — ask it to summarize its understanding of the task and constraints first.


```text
Before you start: describe the changes you plan to make and the files you'll modify.


```


This takes 10 seconds. It catches three categories of problem:


1. The agent misunderstood the scope
2. The agent is working with stale context about a file that's changed
3. The agent has a dependency wrong that will cause cascading failures


Fixing a misunderstanding before execution costs nothing. Fixing it after costs 45 minutes of reverting, re-explaining, and re-running.


## Context Killers


These patterns predictably destroy context quality, even when everything else is right.


**Pasting error logs without explanation.** A 200-line stack trace dumps into context without telling the agent which line is the actual failure or which layer introduced it. The agent reads everything and focuses on the wrong thing. Give it the error, the location, and your hypothesis.


**Referencing files by name without opening them.** "The bug is in` auth.ts` " means the agent has to navigate to that file, read it, and form its own understanding — pulling in everything around the specific issue. Load the relevant section explicitly.


**Task-switching in the same session.** You start asking about the caching layer, solve it, then pivot to "also fix the pagination bug." The agent now carries both contexts. They contaminate each other. New session, new task.


**Over-loading CLAUDE.md.** A 500-line context file is not better than a 100-line one. More instructions compete with each other for the model's attention. Keep it tight. Every rule should earn its place.


## Tools That Fit Into a Context Engineering Workflow


Tool What it does


` CLAUDE.md` Front-loaded architecture context — read at every Claude Code session start


` AGENTS.md` Same purpose for multi-agent frameworks and other tools


` .cursorrules` Cursor's equivalent of CLAUDE.md


` /compact` (Claude Code) Compresses long sessions while preserving architectural decisions


` /clear` (Claude Code) Wipes session context entirely — use when switching to unrelated tasks


These aren't advanced features. They're the baseline for not wasting half your session on context confusion.


## The Context Engineering Daily Workflow


Context engineering workflow in action: clean sessions, scoped tasks, precise context — agents performing at their best


Blink


**Start of a new task:**


1. New session (or` /clear` if the previous task's context isn't relevant)
2. CLAUDE.md loads automatically — update it if you made an architectural decision this week
3. Load only the files this task needs. Name them explicitly in your first message.


**Mid-task:** 4. Every 60-90 minutes: run` /compact` to clean up accumulated noise 5. For each sub-task, specify scope explicitly ("modify only these files, do not touch X")


**Before any high-stakes change:** 6. Ask the agent to summarize its plan and the files it'll touch. Review first. Then execute.


**End of task:** 7. New task = new session. Carry only what the next task actually needs.


The overhead is about 30 seconds per task start. The payoff is sessions where the agent produces consistent, convention-following output — instead of output that looks right but quietly breaks your codebase's patterns.


## Build Context Engineering Into Your Workflow With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up this project with CLAUDE.md conventions and deploy it on Blink — database, auth, and hosting included."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Context engineering means deliberately deciding what information the AI has before and during a task — which files it reads, which constraints it knows, when to start fresh, and when to compress what's accumulated. It treats the context window as a finite resource to manage intentionally, rather than letting it fill up with whatever happened during the session.


Prompt engineering focuses on how you phrase a single request. Context engineering focuses on what information the agent has before and during execution. For agentic coding tasks that run autonomously across dozens of steps, context engineering matters more — you can't re-phrase every turn, but you can control what the agent knows going in. The two skills complement each other; a well-phrased prompt inside a well-engineered context is better than either alone.


Update it when you make a significant architectural decision, add a new library or framework, establish a new naming convention, or find the agent repeatedly making the same mistake. A CLAUDE.md that hasn't been updated in three months is likely missing decisions made in the last quarter. Treat it like an onboarding document that needs to stay current — stale context instructions are as harmful as no instructions.


Yes. The same principles apply to Cursor (with` .cursorrules` ), GitHub Copilot's agent mode (with` .copilot/instructions.md` ), and any other agentic coding tool. The specific file names and commands differ, but the practice is identical: front-load architecture context, scope each task explicitly, manage session length, and start fresh when switching to unrelated work.
