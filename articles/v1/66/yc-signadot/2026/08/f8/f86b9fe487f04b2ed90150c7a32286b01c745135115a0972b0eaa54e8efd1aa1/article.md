---
schema_version: "1.0.0"
document_id: "f86b9fe487f04b2ed90150c7a32286b01c745135115a0972b0eaa54e8efd1aa1"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/openspec-signadot-integration-tests-inside-the-agent-loop/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-19T01:00:38.618032+00:00"
fetched_at: "2026-08-19T01:00:42.431351+00:00"
content_hash: "sha256:27739a04dfead11c848222841773cd48beeed2ac6781d6d48a51e41626a46a5b"
---

# OpenSpec + Signadot: Integration Tests Inside the Agent Loop

Austin Xu, a former senior engineering manager at eBay, built an agent workflow that could ship a real refactor in three hours. Then it hit a wall. His coding agents had no way to test against real services, so anything involving more than one component waited on a 20-minute CI cycle that reported back long after the agent had moved on.


His answer was to give every change its own[Signadot sandbox](https://www.signadot.com/product/platform/) , where the modified service runs against the rest of the stack and integration assertions come back in seconds. He documented the whole thing in the latest installment of his OpenSpec + Superpowers series: what he wired up, what he tested, and what the eval numbers looked like once real integration feedback replaced unit test approximations.


This article outlines his experience at a high-level. The full writeup, including his eval logs, is[on his blog](https://austinxyz.github.io/blogs/blog/openspec-sandbox) .


## The series: building an agentic development stack in public


The series starts with[Stacking OpenSpec and Superpowers](https://austinxyz.github.io/blogs/blog/openspec-superpowers-combined) , where Austin combines OpenSpec’s spec-driven development workflow with the Superpowers skill library and ships a real refactor in three hours with 86 new tests.


From there, each post extends the system based on what broke in practice. He cataloged the friction points that surfaced across multiple projects and packaged the fixes into a reusable plugin. He added an evaluation harness that scores every implementation with an independent, fresh-context subagent. Then he scaled the whole thing to a team, with a branch model, parallel conflict detection, and a four-layer achieve gate that decides when work is actually done.


That team-scale post is where the problem showed up. Three of his four gate layers ran fine, but integration testing had nowhere good to live. Local environments could not reproduce the full service stack, so integration tests waited for CI. And CI’s feedback tempo is built for humans: a 20-minute loop is fine for a developer who retains context, but a coding agent submits a change in seconds and moves on. By the time CI reports back, the context that would have made the fix cheap is gone.


The four-layer achieve gate from Austin's team-scale post. Integration tests lived in the CI gate, outside the agent's loop. The four-layer achieve gate from Austin's team-scale post. Integration tests lived in the CI gate, outside the agent's loop.


Austin identified Signadot as a possible solution in that same post, after reading a New Stack piece on[CI for coding agents](https://thenewstack.io/ci-for-coding-agents/) by our CTO, Anirudh Ramanathan. Anirudh reached out, and a few weeks later Austin had integrated Signadot into his architecture and written a follow-up detailing the results.


## How he implemented it


The integration is built on Signadot’s core model: each change gets a lightweight fork of the cluster where only the modified service runs the new image, everything else stays shared, and routing keys steer test traffic through the forked service.


His agents drive the workflow through two Signadot skills they invoke directly. The first, signadot-plan, defines a sandbox test specification with explicit assertions for what passing means. The second, signadot-validate, executes that plan against a live sandbox and returns structured pass or fail results that his evaluation harness can score. If you want to see this pattern in action, our docs walk through the same workflow in the[plan-based validation guide](https://www.signadot.com/docs/tutorials/quickstart/plan-based-validation) .


He tested it on HotROD, the ride-sharing microservices demo from our[quickstart](https://www.signadot.com/docs/tutorials/quickstart/first-sandbox) , with a driver pickup confirmation feature that touches Redis, a notification service, and frontend polling.


The implementation was split into two groups. The Redis storage group was single-component, so unit tests sufficed and it skipped the sandbox. The arrival handler and notification group was genuinely multi-service, so it ran five sandbox assertions covering dispatch acceptance, idempotency, notification delivery, and 404 handling.


## The results


All five assertions passed in seconds against live services, and the groups scored 98 and 99 out of 100 in his harness.


5/5


integration assertions passed


Seconds


feedback vs a 20-minute CI loop


99/100


evaluation harness score


0


rework iterations


Results from Austin Xu's pickup confirmation case study, validated against live services in a Signadot sandbox.


The more meaningful shift was structural. Runtime evaluation is 40 percent of his scoring, and before the integration it leaned on unit tests as an approximation of multi-component behavior. After implementing Signadot, that score reflects validated service interactions.


With stronger planning up front, he reports zero rework iterations, and integration verification moved from after merge to during implementation, while the agent still has warm context and can correct immediately.


For the full writeup, including the skill definitions, the sandbox specs, and the eval logs behind those scores, read Austin’s post:[OpenSpec + Sandbox: Your CI Pipeline Is the Wrong Tool for AI Coding Agents](https://austinxyz.github.io/blogs/blog/openspec-sandbox) .
