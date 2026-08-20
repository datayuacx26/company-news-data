---
schema_version: "1.0.0"
document_id: "5e250edb00ded9e7651ba9ffe11b677e03909ad155c7722b363737324c851f33"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/from-weeks-to-hours-inside-wix-s-autonomous-bug-fixing-system"
published_at: "2026-08-19T10:17:19+00:00"
first_seen_at: "2026-08-19T11:27:55.577378+00:00"
fetched_at: "2026-08-19T11:27:56.893258+00:00"
content_hash: "sha256:a8b272f7f5a02f3b8eb083092f9be41e4a330ed4a1a01cbb93b531ad20f82074"
---

# From Weeks to Hours: Inside Wix’s Autonomous Bug-Fixing System

A user opens a support ticket. Their subscription cancellation stopped working. Somewhere inside a system with thousands of services and millions of users, something broke.


Before we built


[Wix Orchestrator](https://www.wix.engineering/post/how-we-built-the-brain-behind-our-self-healing-system-context-retrieval-at-org-scale) , here's what happened next: the ticket joined a queue. Days later, a support engineer picked it up, tried to reproduce the issue, contacted the customer to gather more context, and opened a Jira ticket routed to R&D. We triaged it, prioritized it against everything else on our plate, investigated, and if things went smoothly shipped a fix.


Average time from user report to production:


**weeks** . In worse cases: months.


We set out to bring that under 24 hours. This is the architecture that got us close.


##


**The Problem: A Pipeline Built for Humans, at Human Speed**


The issue isn't that anyone in this pipeline is slow. It's that every single step requires a human to context-switch, gather information manually, make a judgment call, and hand off to the next person. The system moves at the speed of its slowest handoff.


When we noticed that our support queue was growing faster than we could drain it, the task was simple in principle: make bugs fix themselves. In practice, it meant rethinking the entire flow from the moment a user files a report to the moment a fix lands in production.


We kept one deliberate constraint: a human engineer reviews and approves every fix before it ships. Not because we don't trust the system, but because the decision to push something to production carries real risk, and that judgment belongs with a person. Everything up to that moment, though investigation, context gathering, fix implementation, code review, we wanted to automate.


##


**The Insight That Changed Everything**


Before we wrote a single agent, we asked a simpler question: why do bugs take so long to fix in the first place?


Almost never because the fix itself is complicated. Almost always because the engineer working the ticket doesn't have the right information. Which service is responsible? What did the logs show at the time? Has this pattern appeared before? Is this even a bug, or is it the system behaving exactly as designed?


This led us to what became the central principle of everything we built:


**it's not about the model, and it's not about the architecture, it's about context.** Not dumping all available data into a prompt. Precise, relevant context, assembled the same way you'd onboard a new engineer: here's the relevant repo, here's what the logs showed, here's what the database looked like at the time, here's the documentation that describes expected behavior.


That onboarding analogy became our design blueprint.


##


**The Architecture: Four Agents, One Orchestrator**


Wix Orchestrator


is four specialized agents coordinated by a central orchestrator. Each agent is stateless, isolated, scoped to a single responsibility, and deliberately separated from the others.


###


**Agent 1: The Detector**


The Detector pulls tickets from our support system, where users report issues and classifies them before anything else happens.


Not every ticket is a fixable code bug. Some are permission issues. Some are configuration problems. Some are users describing behavior that is, in fact, working exactly as intended. The Detector applies a configurable set of strategies to decide which tickets to pursue: high-volume clusters of similar reports, tickets that have been sitting unresolved for too long, or targeted queries from specific teams looking at specific areas.


Classification, it turned out, is one of the hardest problems in the system. Identifying a 500 error in the logs is straightforward. Recognizing that a premium user lost access to features because a billing event silently failed to fire with no obvious error anywhere requires a different kind of inference.


###


**Agent 2: The Enricher (Wix Octocode Research)**


This is the heart of the system.


[Wix Octocode Research](https://www.wix.engineering/post/how-we-built-the-brain-behind-our-self-healing-system-context-retrieval-at-org-scale) ’s job is to aggregate context from across our entire tooling landscape and produce a


*plan* ‘A structured’, agent-readable description of where the bug likely lives, what the relevant code paths look like, what the live state was at the time of the failure, and what a reasonable fix approach might be.


To do this, it uses a suite of specialized tools in parallel:


**OctoCode** for smart static code analysis and search across GitHub,


**Trino** for normalized query access across all Wix databases, and


**Grafana** for live server logs and runtime errors. It also cross-references our internal and public


**documentation** to determine whether observed behavior is a bug or by design synthesizing all of it into a structured context object.


The key insight behind Wix Octocode Research is that context has two sides: the


*static* side (the codebase, how it's structured, what it's supposed to do) and the


*live* side (what actually happened, the logs, the database state, the runtime errors). You need both to diagnose anything non-trivial. Connecting them, across every service in a large organization, is where most of the engineering complexity lives.


###


**Agent 3: The Coder**


Given Wix Octocode Research’s plan, the Coder spins up an isolated Docker container with a coding agent CLI and a Git CLI, clones the relevant repository, implements the fix, runs it, and opens a PR.


The model choice here matters less than you'd expect. What drives output quality is the quality of the incoming plan. A precise, well-structured plan specifying exactly where to look, what the expected behavior should be, and what changed gives even a modest coding agent enough to work with.


###


**Agent 4: The Reviewer**


The Reviewer is deliberately isolated from everything that came before. It receives only the original ticket and the resulting PR, no memory of the enrichment process, no knowledge of the coding decisions made along the way.


Its job is to evaluate the PR as an independent observer: does this fix actually address the stated problem? Does it introduce unintended side effects? What's the risk level of the affected area? A fix touching billing logic might be technically correct but carry enough risk that it needs explicit human review before going anywhere near production.


Why not just give the Coder the Reviewer's instructions upfront and skip the separate agent? Because attention is finite. An engineer who just wrote a fix is the worst possible reviewer of that fix. The same principle applies to agents, separation of concerns produces better outcomes than consolidation.


##


**What Connects It All**


A few implementation details that made a significant difference:


**Schema validation and retries everywhere.** Agents are non-deterministic. They will occasionally return malformed output. We validate the schema at every handoff, and when something breaks, we send the error back to the same agent along with the original context: here's what you returned, here's what was wrong, try again. This dropped our inter-agent error rate from roughly 50% to under 1%.


**Idempotency throughout.** If a ticket has already been resolved and a PR is already in flight, the system recognizes it and doesn't start again. This matters more than it sounds when you're running automated pipelines at any volume.


##


**The Problem We Didn't See Coming**


When we started bringing


Wix Orchestrator


-generated PRs to engineering teams, the feedback revealed a gap we hadn't anticipated.


One team told us they were mid-migration to V2 and didn't want any changes in V1. Another was aware of ongoing failures caused by a third-party vendor and waiting for that vendor to fix their side. Our PR was well-intentioned but irrelevant. A third pointed out that our fix touched an architectural decision made years ago for reasons that weren't written down anywhere.


None of this information was findable. It lived in Slack threads, in people's memories, in design documents that existed once and were never updated. We were missing what we now call


**Organizational Context,** the institutional knowledge that every long-running engineering team accumulates and that slowly evaporates as people leave, services evolve, and decisions go undocumented.


This is the second open problem in the system, alongside classification.


Our current approach: start documenting everything from here forward. Every ticket the system resolves, every decision it makes, every architectural constraint it discovers stored in a queryable form. We're also exploring an agent-to-agent protocol where each team maintains their own knowledge agent, and


Wix Orchestrator


can query those agents directly during enrichment, without forcing teams to store information in any particular format.


##


**How We Developed the System Itself: Specification Driven Development**


Building


Wix Orchestrator


changed not just how we fix bugs, but how we write the code that fixes them.


We used what we call


**Specification Driven Development (SDD)** . Before any code gets generated, we produce a Markdown spec file that describes the implementation plan in detail what we're building, why, and how. That spec gets reviewed by an independent agent before the Coder ever touches the repository. The Coder receives the spec as its primary input, not a vague task description.


One thing we found particularly useful: prompting the Coder to think of itself as the engineer who will also maintain this in production. That framing leads it to add proper logging, handle errors explicitly, and think about edge cases it might otherwise skip. An agent that knows it will be responsible for debugging its own output writes better code.


##


**What We Learned - and What Other Teams Should Know**


1.


**Start with context, not with agents.** The temptation is to jump straight to building the coding step. Resist it. Everything downstream depends entirely on the quality of what Wix Octocode Research produces. Invest in enrichment before you think about automation.


2.


**Separate agents by responsibility.** It's possible to put everything in one agent. It's not a good idea. Attention is finite, and each agent does better work when it has a single job. A reviewer that also wrote the fix is less likely to catch the fix's problems.


3.


**Schema validation and retries are not optional.** Build them into every handoff from the start. When something breaks, return the error to the same agent with the original context. This is cheap to implement and dramatically improves reliability.


4.


**Human review at the end is a feature.** We made a deliberate choice to keep an engineer in the loop before anything ships to production. This isn't a limitation, it's what makes the system trustworthy enough to run on real production code.


5.


**Document institutional knowledge as you go.** Every decision your system makes is an opportunity to capture something that would otherwise disappear. Don't wait for a perfect knowledge management strategy. Start storing what you learn now.


Wix Orchestrator is running in production. It takes support tickets, investigates them autonomously, and delivers PRs to engineering teams in hours rather than weeks. We're not done classification and organizational context are still open problems but the core architecture works.


The thing we keep coming back to is this: if you get the context right, almost any bug becomes solvable. The hard part isn't the model, and it isn't the code generation. It's building the infrastructure that assembles the right information, from the right places, at the right level of precision every time. That's the work. Everything else follows from it.


If you've found a reliable approach to classification or organizational knowledge capture, we'd genuinely love to compare notes.


*This post is base on* ***Israel Zablianov's session "*** *Self-Healing Software: From User Complaint to Pull Request, Autonomously" - watch the full talk here (Hebrew)*


This post was written by


**Israel Zablianov**


**More of Wix Engineering's updates and insights:**


-


**Follow us on:**[Twitter](https://twitter.com/WixEng) **|**[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) **** **|** ****[Instagram](https://www.instagram.com/wix_eng/) **** **|** ****[Facebook](https://www.facebook.com/WixEngineering/)


-


**Join our**[Telegram channel](https://t.me/wixeng) ****


-


**Visit us on**[GitHub](https://github.com/wix) ****


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/) ****


-


**Subscribe to our**[YouTube channel](https://www.youtube.com/WixTechTalks) ****


-


[Follow our Medium publication](https://medium.com/wix-engineering) ****
