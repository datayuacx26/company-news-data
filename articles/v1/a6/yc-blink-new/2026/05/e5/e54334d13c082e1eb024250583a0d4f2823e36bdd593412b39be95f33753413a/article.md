---
schema_version: "1.0.0"
document_id: "e54334d13c082e1eb024250583a0d4f2823e36bdd593412b39be95f33753413a"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-context-management"
published_at: "2026-05-15T01:14:50+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:55:53.408599+00:00"
content_hash: "sha256:d9eecf56eaa99111fcccec59c3025a205c1bb103279edc84c00ead972debccfe"
---

# Claude Code Context Management: /clear, /compact, and Session Handoff

## The Decision Guide


Claude Code session handoff pattern — how to continue work across context window limits


Blink


Situation Reach for Why


Same task, context still relevant Continue Everything in the window is load-bearing


Claude went down the wrong path` /rewind` Keep file reads, drop failed attempt, re-prompt


Session bloated with stale debug noise` /compact <hint>` Low effort; Claude decides what mattered


Starting a genuinely new task` /clear` Zero rot; you control exactly what carries forward


Next step produces tons of output you won't need Subagent Intermediate noise stays in child's context


## Session Handoff: Continuing Work Across Sessions


Long projects outlive single sessions. Here's the pattern that works.


Developer managing multiple coding sessions in a terminal environment


Blink


1


#### End the current session with a handoff message


Before closing, ask Claude: "Write me a handoff brief — what we built, what's left, what constraints apply, and what files are relevant." This becomes your starting context for the next session.


2


#### Save the brief somewhere persistent


The handoff brief lives in` CLAUDE.md` (project-level memory), a scratch file in the repo, or just your clipboard. Claude's context doesn't persist between sessions — your brief does.


3


#### Open a new session with the brief as your first message


Paste the brief, then state the next task clearly. Claude has all the context from the handoff without the noise of the previous session.


4


#### Use /rewind if the new session starts on the wrong foot


If Claude misunderstands the brief and goes in a wrong direction, rewind early rather than accumulating corrective messages.


## CLAUDE.md as Persistent Memory


` CLAUDE.md` is the most underused context management tool in Claude Code. It's a file in your project root that Claude reads at the start of every session — automatically.


Use it to store things Claude should always know:


- Architecture decisions and their reasons
- Constraints and anti-patterns ("never use X approach because Y")
- Key file paths and what they do
- Coding style rules specific to this project
- What's been built and what's planned


A good` CLAUDE.md` eliminates the need to re-explain project context in every new session. Claude starts each session already knowing the project — you just give it the task.


> "CLAUDE.md changed my workflow completely. Every project has one now. Architecture decisions, constraints, what NOT to do — Claude reads it fresh every session. No more explaining the same context ten times." — r/vibecoding, Reddit


Treat` CLAUDE.md` like a README for Claude. Update it at the end of every major session with what changed and what it means for future work.


## 5 Power-User Patterns


**1. Proactive /compact before the limit.** Don't wait for auto-compact. At ~600K tokens, run` /compact focus on \[current task\]` yourself. You get a cleaner summary at a point when context rot is lower.


**2. Subagents for isolated work.** Ask Claude to "spin up a subagent to verify this implementation against the spec" or "spin off a subagent to read the other codebase and summarize the auth pattern." The subagent's intermediate noise (file reads, attempts) stays in its own context. Only the result comes back.


**3. Task files as context anchors.** For complex tasks, write a` .task` or` .plan` file before the session. It describes the task, constraints, and acceptance criteria. Reference it at the start of each session: "follow the plan in auth-refactor.task." Claude reads it fresh each time.


**4. Rewind instead of correcting.** When Claude makes a mistake in a long session, resist the urge to append "that's wrong, do X instead." Rewind to before the mistake, re-prompt with what you learned. The correction message approach accumulates noise; rewind keeps the context clean.


**5.` /usage` to track your token spend.** Use the` /usage` slash command to see how much context you've consumed. If you're at 70%+ of the window and the task isn't done, decide now: proactive` /compact` or` /clear` . Don't let auto-compact catch you at the worst moment.


## Build Context-Managed Apps With Claude Code + Blink


Add[Blink](https://blink.new/) as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app and host it on Blink — use plan mode to design the architecture first."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Claude Code runs on a 1 million token context window with Claude Sonnet as the default model. That's enough for large codebases and long sessions — but context rot means performance still degrades as you approach the limit. Proactive context management (using` /compact` before you hit the ceiling) consistently outperforms letting the window fill.


Use` /compact` when you want to keep the session going but trim the noise — mid-task, when debugging output is bloating the window but the thread is still relevant. Use` /clear` when starting a genuinely new task, or when the session is so tangled that starting fresh with a clean brief is faster than summarizing it.` /clear` gives you control;` /compact` automates the cleanup.


Context rot is the degradation in model performance as context fills. More tokens = attention spread thinner = older details matter less. Prevention: use proactive` /compact` before the limit, use` /rewind` instead of accumulating corrective messages, and start new sessions with a clean brief rather than letting sessions run indefinitely across multiple distinct tasks.


Not automatically. Each new session starts from scratch.` CLAUDE.md` is how you give Claude persistent project memory — it's read at the start of every session automatically. Write your architecture decisions, constraints, and project context there. For task-specific handoffs, save a brief at the end of each session and paste it into the next.


` /rewind` (double-tap Esc) jumps back to a previous message and drops everything after it. Use it when Claude goes down the wrong path but the earlier context (file reads, setup) is still useful. It's cleaner than appending correction messages — those accumulate noise. Rewind to a known good state and re-prompt with what you learned.


Subagents are separate Claude instances with their own fresh context windows. When Claude spawns a subagent via the Agent tool, that subagent does its work (potentially generating lots of intermediate output) and only the final result comes back to the parent session. This keeps the parent context clean. Use subagents for isolated tasks: verifying implementations, reading external codebases, generating documentation.


More tips on building efficiently with AI agents at the[Blink blog](https://blink.new/blog) .


*Disclosure: Blink is our product. We believe it's the best option for full-stack app development, but the guides above reflect genuine Claude Code best practices.*
