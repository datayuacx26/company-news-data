---
schema_version: "1.0.0"
document_id: "e670e6265dcab7c7f50d0e30b9c3e19e983ad6dbac4f7d9ac18e9412dd4432d5"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/ai-coding-tools-more-capable-than-your-process/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:f9d82a3bcc7674860958509ec6108b0ff62d3d9261eecaa36f1d0f2989504590"
---

# Your AI Coding Tools are More Capable Than Your Process

# Your AI Coding Tools are More Capable Than Your Process


Most teams bolted AI onto their existing workflow. The teams seeing real results restructured the workflow around what agents can actually do.


Apr 3, 2026 — 9 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


*Part 5 of 5 · Previous:[The DIY Context Trap](https://www.driver.ai/blog/diy-context-trap-codebase-context-infrastructure)*


We were on a call recently with a VP of Engineering at a large enterprise. Three hundred and fifty engineers. They’d invested heavily in Cursor — massive adoption, lines of code through the roof, engineers hitting token limits and asking for more budget.


But velocity was flat. Lead times hadn’t moved. Nobody was taking on more commitments in sprint planning.


His words: “That was naive me, thinking everyone would just be so excited about this and they’d just go do it.” Instead, what he was seeing was engineers finishing work faster and then going on to something else. The process hadn’t changed. Same program increments, same quarterly commitments, same pod structure. They’d bolted a faster engine onto the same chassis and were surprised when the car didn’t go anywhere new.


We see this everywhere.


## Phase 1


Most teams right now are in what we’ve started calling Phase 1 — bolting AI onto the existing workflow. Same sprint planning. Same tickets. Same PRs. Same code review. The agent writes code faster. Everything else stays the same.


This produces real speed improvements on individual tasks. It also leaves most of the value on the table.


The progression is remarkably consistent across our customers. It starts with autocomplete using Copilot in the IDE. Code suggestions. Some engineers love it, some ignore it. Nobody’s workflow actually changes. Then agentic tools like Cursor, Claude Code. The agent doesn’t just suggest, it implements. Engineers start relying on it for real work. Then a watershed moment. For many teams, this coincided with Claude Code and Opus becoming available. The model gets capable enough to do genuine autonomous work on non-trivial tasks.


Then the wall. The agent works on small, well-defined tasks. On larger tasks that require cross-service changes, architectural work, unfamiliar areas of the codebase it struggles. Hallucinations. Missed dependencies. Locally correct changes that are globally wrong. Engineers develop workarounds. Markdown files checked into repos. Elaborate grounding prompts. Letting Claude Code rip for tens of minutes to hours just to build up context before starting the actual work.


This is where most serious engineering teams are right now. Powerful tools, manual workarounds, inconsistent results. The tools aren’t the bottleneck. The process is.


## What Changes When You Restructure


The teams that have pushed past Phase 1 have done something specific: they’ve restructured their development process around what agents can actually do.


We’ve been through this internally at Driver. We audited our own process and found that across a three-plan implementation, we had 32 steps — 11 of which were pure bookkeeping. Updating plan status, tracking progress, managing transitions between phases. Developers were spending more time managing the orchestration than making design decisions.


What we’ve landed on is a stage-based approach: research before code, planning before implementation, validation before shipping. At each stage, our agent is a collaborator, not just an implementation tool.


**Research first.** Before writing anything, the agent researches the problem. Reads existing implementations. Maps the relevant architecture. Identifies patterns. Surfaces prior decisions about why the current design exists, what constraints shaped it. Most teams skip this and go straight to implementation. Then they pay for it in hallucinations and rework.


**Planning with testable criteria.** The agent produces a specific, testable plan before writing code. Which files will change. What the expected behavior is. What the blast radius is. This is the highest-leverage review point in the entire process. Catching a wrong approach in the plan costs minutes. Catching it after implementation costs hours or days.


**Mechanical implementation.** Once research and planning are done well, implementation becomes the least interesting part. The agent knows what to build, where to build it, what patterns to follow. The quality is almost entirely determined by the quality of the previous steps.


**Architectural review.** The reviewer checks whether the implementation matches the plan, whether the plan was correct given the research, and whether the change fits the broader architecture. Higher-level review. Intent and architecture, not syntax and logic.


The result, in our experience: what used to require 15 or more manual prompts per plan — directing the agent, managing context, tracking state — comes down to 3 or 4 genuine decision points. The engineer shifts from process manager to decision-maker.


## What Happens Without the Process


A cautionary example. A German software company had engineers using AI agents extensively on a feature branch. The engineers were competent. The agents were capable. But without a structured process, the agents made architectural decisions in the first week that nobody questioned.


When the feature branch was finally reviewed for merge, the technical debt was so extensive it couldn’t be unwound. Significant work, built on compounding architectural mistakes that an agent made early and nobody caught because there was no research step, no plan review, no blast radius analysis.


This isn’t a story about bad agents or bad engineers. It’s a story about a missing process.


## What’s Coming


Everyone is in a transition right now. Teams are rapidly figuring out that the bottleneck is no longer code but everything around code. Defining the work. Validating the output. Maintaining the context. Coordinating across teams.


The next subway stop is an orchestration layer that makes the structured SDLC deterministic. The system knows the stages. It knows what context each stage requires. It knows what quality gates need to pass before progressing. The engineer defines the intent. The system executes the process. The engineer reviews at the checkpoints that matter.


But all of it depends on one thing: the agent having access to accurate, comprehensive codebase context at every stage. Without context infrastructure, every step in the process is operating on incomplete information. And that’s where the whole thing breaks down.


We keep asking ourselves what, in three months, we will wish we would have done today. Right now, I think the answer is: help more teams get past Phase 1. The tools are capable enough. The models are smart enough. The question is whether the process and the context infrastructure underneath it are ready for what the tools can actually do.
