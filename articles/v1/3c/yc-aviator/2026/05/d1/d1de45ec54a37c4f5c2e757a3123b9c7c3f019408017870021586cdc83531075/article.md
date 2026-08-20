---
schema_version: "1.0.0"
document_id: "d1de45ec54a37c4f5c2e757a3123b9c7c3f019408017870021586cdc83531075"
company_key: "yc-aviator"
company: "Aviator"
source_id: "yc-aviator-rss-35144b27026a"
canonical_url: "https://www.aviator.co/blog/do-we-even-need-a-better-github/"
published_at: "2026-05-06T19:52:06+00:00"
first_seen_at: "2026-07-20T23:23:35.507164+00:00"
fetched_at: "2026-07-28T22:12:45.343034+00:00"
content_hash: "sha256:09fddd72fd7100e745906919795e66c269a671ae570eca4d87496935280c24e4"
---

# Do we even need a better GitHub?

GitHub is having a tough time. Outages are frequent, PRs are missing, and their merge queue recently shipped a bug that silently deleted commits from mainline branches.


GitHub’s reliability issues are not surprising if you look at what’s happening to PR volumes. Agentic coding tools don’t just help engineers write code faster; they generate more discrete units of work. **More PRs, more CI runs, more merge conflicts, more webhook events, more of everything.**


The conversation online has moved from “GitHub needs to fix this” to “Who is building the next GitHub?”


I think that’s the wrong question. Maybe the better question is whether anyone needs to. GitHub was built for a world where humans write code. That world is ending.


## The workflow was designed for humans


Think about what the standard workflow looks like: a developer writes code, opens a PR, a peer reviews it, CI runs, the PR gets approved and merged, it deploys. Every step in that chain was designed for a world where humans wrote all the code. The PR as a unit of work exists because it maps to a human’s mental model of a coherent change.


When AI generates the code, these assumptions shift. **The review becomes a human trying to understand machine-generated logic** , a fundamentally different cognitive task than reviewing a colleague’s work. CI is still valuable, but the failure modes of AI-generated code are different from the failure modes of human-written code. The PR might represent a task that an agent decomposed into subtasks on its own, with no human reasoning trail attached.


## What happens when you 5x the throughput


**A team that was merging 5 PRs per developer per week is now merging 20 or 25.** Some devs are approaching hundreds of code changes a day. At that volume, the whole delivery pipeline degrades.


## The industrialization of software


If you were designing a code delivery system from scratch for a world where AI generates most of the code and humans provide judgment, direction, and quality oversight, you probably wouldn’t arrive at what we have today.


We recently had a discussion in[The Hangar community](https://dx.community/) about ‘the industrialization of software.’ You probably keep hearing more about building your software factory.


Before industrialization, manufacturing was done by hand, and quality was a function of the craftsperson’s skill. Post-industrial manufacturing is done by machines, and quality is a function of the systems you build around those machines: inspection, testing, feedback loops, and continuous improvement.


Software is going through a similar transition. Code used to be a craft product. It’s becoming a manufactured product. And the quality systems need to change accordingly.


Andy Grove’s *High Output Management* describes quality control in chip manufacturing through sampling: inspect a subset, increase your sampling rate when you find errors, reduce it when things stabilize. Build trust incrementally. With AI-generated code we cannot sample results, but can still build better quality control.


Instead of reviewing every line, you review strategically. You identify the categories of errors AI consistently makes in your codebase and build automated checks for those. Over time, as your guardrails improve, the need for manual review decreases because you’ve systematically addressed the failure modes. You cannot just drop code reviews. Build trust incrementally. And in layers. The (in)famous[Swiss cheese model.](https://www.latent.space/p/reviews-dead)


This is how continuous deployment matured as a practice, too. We moved from long massive deployments to continuous deployment by building better guardrails, not by putting more humans on an assembly line. If things break, we improve the guardrails, so it doesn’t happen again.[If it hurts, do it more often!](https://continuousdelivery.com/)


## We need to rethink the verification layer


The ecosystem around GitHub is so deep that people almost can’t imagine doing software engineering without it. But only scaling Git or source control won’t get us in a better place. The layer above Git, the collaboration and[verification layer](https://verify.aviator.co/) , is where the real innovation needs to happen. Rather than a “better GitHub,” I think what teams need is a rethinking of the verification layer. A few concrete things:


**Intent-first workflows.** As I’ve[argued before](https://www.latent.space/p/reviews-dead) , aligning on intent and behavior would be the most impactful intervention[before the code is generated](https://www.aviator.co/blog/what-if-code-review-happened-before-the-code-was-written/) , not reviewing the code after. If the[acceptance criteria](https://docs.aviator.co/verify/concepts/verification-layers) are clear and agreed upon, verification becomes a matter of checking output against criteria rather than trying to reverse-engineer intent from implementation.


**Behavioral verification at the merge layer.** Instead of CI that runs unit tests and linters, a merge system that can verify acceptance criteria against the actual code changes. This is what BDD and TDD were always aiming for , but the overhead made them impractical when humans were writing everything. AI makes them practical because AI can generate the acceptance criteria, humans review them, and AI can then generate the code and assist in the verification.


**Smarter batching.** Instead of merging individual PRs one at a time, batch related changes together, run comprehensive verification on the batch, and ship the batch as a unit. This is what merge queues should evolve into: not just a queue for ordering merges, but a staging area for grouped verification.


**Adaptive review depth.** Not every change needs the same level of scrutiny. A system that can triage changes by risk, route high-risk changes to human review, and let low-risk changes flow through automated verification would let teams focus human attention where it matters.


**Feedback loops from production.** When something breaks in production, trace it back to the change that caused it, identify what category of error it was, and feed that back into both the prompting layer (to prevent it) and the verification layer (to catch it from happening again). This is the slop register concept applied continuously.


## Start with your bottlenecks, not your platform


The honest answer is that nobody has fully figured this out yet. We’re in a transitional period where the tools for generating code have leaped ahead of the tools for verifying and delivering it. The organizational structures haven’t caught up either.


What I’d encourage teams to focus on is less “Which platform should we move to” and more **“Which parts of our delivery pipeline are bottlenecks now that weren’t bottlenecks a year ago?”** For most teams, the answer is review, verification, and the feedback loop between production issues and prevention.


GitHub may or may not solve its infrastructure problems. That matters, but it matters less than whether your team has a coherent approach to verifying AI-generated code before it reaches production.
