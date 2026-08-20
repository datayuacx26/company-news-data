---
schema_version: "1.0.0"
document_id: "2217d687aa51527e2f9e133c623fe24740d61dce1b619f04e4ada5a12eba9dcb"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/agentic-coding-best-practices"
published_at: "2026-06-09T12:20:08+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:18fd8ad810b7d407cbe79e8bd939ce43544244d757fd33caba5822898e82efda"
---

# Agentic Coding Best Practices: Spec-Driven Development, Human-in-the-Loop, and Context Control

## Best Practice 3: Use Plan Mode Before Agent Mode


Plan mode is the diff between "what I asked for" and "what the agent is about to do."


Claude Code's plan mode (and Cursor's equivalent) forces the agent to propose before executing. Read the plan. Trim scope if needed. Then approve execution.


Three files or more touched = plan mode required.


One file, clear scope = run it directly.


The rule is not about caution. It's about information. Plan mode surfaces what the agent is about to do before it does it. That's the moment to catch scope creep, wrong assumptions, and architectural choices you weren't part of.


Human-in-the-loop: review the agent's plan at key checkpoints, approve, then let it run — context control for consistent results


Blink


*Human-in-the-loop: review the agent's plan at key checkpoints, approve, then let it run — context control for consistent results*


Read the plan like a code review. What files are changing. What assumptions are baked in. What's explicitly out of scope. If the plan is right, approve it. If it isn't, redirect now — before the agent has touched anything.


The[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-beginners) covers plan mode setup in detail with specific workflow steps.


## Best Practice 4: Manage Context Aggressively


Context degradation is silent. The agent gets worse as the context fills — but it doesn't tell you.


[Anthropic's Claude Code documentation](https://code.claude.com/docs/en/best-practices) states this directly: LLM performance degrades as context fills. At 70-80% capacity, the model starts forgetting earlier instructions and making more frequent errors.


Two rules:


**` /clear` at task boundaries.** Finished one feature, starting another?` /clear` . Non-negotiable. Context from the previous session bleeds into the new one in ways that are subtle and hard to catch.


**` /compact` during long sessions.** When you're deep in one complex problem and don't want to clear entirely,` /compact` lets the agent summarize what matters before the window fills.


What belongs in` CLAUDE.md` :


- Stack decisions and conventions (tech choices, naming patterns)
- Areas explicitly out of scope ("Do not touch /legacy")
- Current sprint focus ("Sprint 4: payments only")
- Commands the agent would otherwise forget (test scripts, environment setup)


What stays inline in the prompt:


- Task-specific context that changes per session
- Anything narrower than project-wide conventions


The distinction matters. A CLAUDE.md that's too long gets ignored. Keep it short enough that every line gets read.


## Best Practice 5: Test as You Go, Not at the End


"I'll write tests when the feature's done" is how you end up with a broken feature and no easy way to debug it.


Write the test first. Give the agent a failing test and tell it to make the test pass without modifying the test file.


```text
// Write this first. Then hand it to the agent.
it  (  'should archive read emails older than 7 days'  ,   async   ()   =>   {
const   email   =   await   createEmail  ({ read:   true  , createdAt:   daysAgo  (  8  ) });
await   runArchiveJob  ();
const   updated   =   await   db.email.  findUnique  ({ where: { id: email.id } });
expect  (updated?.archived).  toBe  (  true  );
});
```


The test is the spec. When the test passes, the task is done. When the test doesn't exist, the agent decides when it's done — and that judgment is unreliable.


[SWE-bench](https://www.swebench.com/) benchmarks confirm this: agents perform significantly better given explicit acceptance criteria versus open-ended prompts. The test is acceptance criteria the agent can run itself.


Run tests every 3-5 changes during a session. Don't accumulate 20 changes and then discover three are broken.


## Best Practice 6: Give the Agent Escape Valves


The agent will get stuck. It will hit an ambiguous case it doesn't know how to resolve. The question is: what does it do?


Without explicit instruction, it guesses. And keeps going.


With explicit instruction, it stops and asks.


Tell it what "stuck" means and what to do about it. Put it in the spec or in the prompt:


```text
If you are uncertain about the right approach for any auth-related code,
stop and describe the two options before picking one.


Do not modify /src/billing under any circumstances without asking first.


```


These are escape valves. They create checkpoints at the moments of highest risk — when the agent hits something it can't confidently resolve.


The second rule ("do not modify /src/billing") is also an abort condition. Name the files and systems that require explicit human approval before the agent touches them. Write them down.


## Best Practice 7: Start Small, Then Expand


The worst sessions start with "build the whole thing."


The best sessions start with "build the core loop — just the thing that makes this feature work at all, nothing else."


Build the user invitation trigger. Get it working. Then add the email template. Then the 7-day expiry. Then the resend logic. Each expansion is a new, small task with a new spec and a clean context.


This isn't about safety. It's about quality.


Small vertical slices let you validate the architecture before committing 500 lines to it. They produce smaller diffs. They're easier to review, easier to roll back, easier to test.


The[Claude Code guide](https://blink.new/blog/what-is-claude-code) covers this commit discipline in depth — the sessions that produce clean output share one trait: each task was scoped small enough to finish in 15-20 minutes.


## Build Agentic Coding Best Practices Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app following spec-driven development — start by showing me the schema and API plan before writing any code, then stop at each review point. Host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Spec-driven development means writing what gets built, what doesn't, edge cases, and acceptance criteria before giving the agent any prompt. The agent builds to a written contract instead of making assumptions. A spec can be five bullet points in a` .md` file — what matters is that it exists before the first prompt runs. Without it, the agent infers scope from context, and that inference is wrong at the edges roughly every time.


Three controls matter most: a written spec before every session (builds to a contract, not assumptions), explicit review checkpoints in the task description (stop and show me at specific milestones), and` /clear` between unrelated tasks (prevents context bleed). The agent goes off-rails when it has ambiguous instructions, stale context, or no explicit stopping conditions. Address all three and runaway sessions mostly stop.


Plan mode for anything touching more than two files, any architectural decision, or anything with significant rollback cost — refactors, schema changes, new features, shared utilities. Single-file edits with clear scope can run without a plan. When in doubt, run plan mode. It costs 30 seconds and routinely saves hours.


Every time you make a decision the agent would otherwise guess at. New service added to the stack, a convention change, a library swap, an area marked out of scope — all belong in CLAUDE.md. In practice, update it once or twice a week during active development. A CLAUDE.md from three months ago is often worse than no CLAUDE.md because it contains actively wrong information the agent will follow confidently.


Short. Commit every 15-20 minutes of meaningful agent work. If you cannot review the diff in under 5 minutes, the session ran too long without a checkpoint. Long sessions produce entangled diffs where unrelated changes mix, making rollback painful. The pattern: small task → agent executes → you review → commit → next task.


Yes — and you should. The pattern that works: write a failing test first, give it to the agent, tell it to make the test pass without modifying the test file. Let the agent run tests autonomously during the session. The judgment you keep: what to do when a test fails. A failing test is a human checkpoint — either the implementation is wrong or the test is wrong, and that call belongs to you.


Yes, for the right tasks. Writing, refactoring, and test generation are significantly faster with an agent. Architectural decisions are not — the agent still needs human guidance there. The developers who get the most value treat the agent as a fast executor and reserve judgment calls for themselves. The spec-driven workflow described here is specifically designed to get the speed without sacrificing control.
