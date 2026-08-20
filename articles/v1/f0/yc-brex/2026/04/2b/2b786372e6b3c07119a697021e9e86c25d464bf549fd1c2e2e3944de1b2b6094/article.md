---
schema_version: "1.0.0"
document_id: "2b786372e6b3c07119a697021e9e86c25d464bf549fd1c2e2e3944de1b2b6094"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/building-autonomous-agents-for-technical-tasks"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:2f2707f9f3cd26b82fc6d6fb8175b149d53bf243f25898baa5d790959c0d3cfb"
---

# Building autonomous agents for technical tasks: 5 lessons learned

## **Designing a system to tackle the busywork**


You send a Slack message describing a bug or a refactor. The system spins up a dedicated environment, assigns an agent, and gives it the full toolchain. The agent reads its own CI failures and applies suggestions by code review bots to ensure the PR is in pristine condition. Your next interaction with it is a human review.


We ran dozens of gRPC migrations across our monorepo, and the process got better with each one. Minimal engineer-hours spent relaying CI output. No one copying error logs into chat windows. No afternoon check-ins.


That’s not a demo; engineers at Brex are using this right now. This is how we got there.


## **Where engineering teams hit bottlenecks**


AI coding agents today typically live in isolated environments: give them a repo, a task, and a sandbox. They perform impressively. Then you put them on real production work, such as a live monorepo, real CI, and real review bots, and a specific failure mode emerges: the agent finishes its changes, hits a wall of automated feedback it can't read, and stops. Or worse, it keeps going without that feedback and produces output that looks correct but isn't.


The issue is connectivity:


- Your CI system knows what failed and why.
- Your review bots flag style violations and security issues.
- Your test runner produces exact stack traces.


All of that information exists. It was built for engineers. But agents can't reach it, so it never gets back to them.


The standard workaround is a human in the loop, not to make decisions, but to relay messages. **That's an expensive solution to a plumbing problem.**


## **The job that made it real**


We needed to migrate gRPC client factories across 400+ services in our monorepo. Teams had built their own implementations over the years. The result? Duplicated logic, inconsistent configs, dependency injection conflicts that surfaced at deploy time. The fix was clear: centralize the client libraries, replace every factory. The scope was large enough that doing it manually wasn't realistic.


We started pairing with AI agents to do the refactors. For simpler services it worked well — 100 to 200 lines of changes, one-shot, about 30 minutes each. The work was repetitive enough that you could describe the pattern once and let the agent run.


For a while, that felt like enough.


## **The broken toolchain**


For simpler migrations the agents could do the work, but we were still running the process. For complex services with multiple client factories, they couldn't handle it at all.


Every migration followed the same loop:


- Spin up a remote developer environment
- Kick off the agent, wait for it to finish
- CI runs, wait for it to finish
- Check if review bots left comments
- Copy those comments back to the agent, ask it to fix things
- Wait again
- Repeat until green
- Finally review and merge


Every morning we'd kick off a handful of migrations. Every afternoon we'd come back and manually relay whatever automated systems had said back to the agents. We turned engineers into messengers.


All the information the agents needed already existed. Bots comment on PRs. CI returns detailed logs. Test runners tell you exactly what broke and where. We'd built that infrastructure for our human engineers. The agents just couldn't access it. So we stood in the middle, passing notes.


## **Closing the loop**


We started simple with three Python scripts: one to forward Slack task requests to remote developer environments, one to feed PR bot comments back to agents, and one to handle CI failures. They ran continuously in the background.


The first time a migration ran start to finish without either of us touching it was a confirmation: would closing the feedback loop be enough? It was. Getting each piece to hand off cleanly to the next just worked.


The time unlock was magic.


You'd send a message in the morning and find a green PR waiting by the end of the day. No afternoon check-ins, no copying error logs into chat windows. The agents were doing what agents are supposed to do: running until they're done.


## **Beyond migrations: delegating engineering work to agents**


What we built is a general-purpose template for delegating engineering work to agents: a trigger, a dedicated environment, a full toolchain, and closed feedback loops with every automated system that would normally surface a problem to a human engineer. Give agents that setup and they can own a wide class of well-scoped tasks without anyone managing them.


So we turned it into a platform:


Platform for delegating engineering work to agents


Tasks come in from Slack, Linear, GitHub, or a cron job. The orchestrator routes them to the agent pool. Each task gets its own RDE. The agent works, hits failures, reads feedback, iterates, and puts up a PR. We come back to something reviewable.


We're continuing to extend what agents can access: internal tool verification, test workload deployment, browser-based checks for frontend changes. The scope of what's delegable keeps expanding, and the pattern scales cleanly because each agent runs in its own environment and manages its own feedback loop.


## What this taught us about running agents on real work


The gap between "AI agents can do this" and "AI agents are doing this reliably" is mostly an infrastructure problem. A few things became clear:


**Feedback loop closure is the core problem.** Agents stall when they need information that's sitting in systems they can't reach. The integration work is less exciting than the model capability question, and it matters more.


**Environment quality determines output quality.** The hypothesis is that an agent without validation tooling is guessing. We gave agents the same toolchain our engineers have, and early PRs are bearing that out.


**Parallelization is a systems design problem.** Running one agent on one task is easy. Running 50 in parallel, each iterating independently, requires thinking carefully about orchestration and environment isolation.


**Scope the work tightly.** This is for tasks that are tedious, well-defined, and common enough that nobody wants to own them manually. That category is larger than you'd expect, and it grows as the platform does.


**Governance is critical.** Agents should only have access to the information they need to perform the task at hand. When agent work is put in front of a human to review it needs to be attributed back to an actual human and not just a bot.


We ran dozens of migrations. None of them required an engineer to spend an afternoon relaying CI output. The platform that emerged from that work is what we're building on now.


*Contributors: Graham Fuller, Jacky Chung, Sofhia de Souza Gonçalves*
