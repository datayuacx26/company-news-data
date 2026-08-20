---
schema_version: "1.0.0"
document_id: "bab3a53f642caf1672e517df9c718c3bb2734ca884b03c724ccd68a73df2bb56"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/stop-trying-to-review-ais-code-faster-bet-on-rollbacks-instead"
published_at: "2026-05-05T15:57:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:d7fc7c3c1b18233e6559c1a30fc1ccae518e7dc74a7744e9a341ac49226947be"
---

# Stop trying to review AI's code faster: bet on rollback instead

A few months ago at Rootly, an AI agent helped us migrate from our old feature flag system to a fancier third-party provider. The work was the kind of tedious, repetitive configuration agents handle well: mapping flags between systems, running the cutover, double-checking the wiring. It did all of it.


It also flipped the wrong flag in the wrong system.


A feature silently broken for hours in staging before we caught it. The agent wasn't bad at config, it actually did a great job most of the time. It just didn't know which provider we were actually using at that moment in the migration, one single time. And that was enough to cause a near miss.


No code review would have caught this. The code the agent wrote was correct. The diff was clean. The tests passed. The failure was that all of the correct code was running against the wrong system, in a window where the system's state was intentionally inconsistent. That's a different shape of bug from the ones code review was built to catch… and it's the shape AI is producing in volume now.


I recently sat a panel moderated by[Ian Sinnott](https://www.linkedin.com/in/iansinnott/) , MTS at Anthropic, alongside[Zhen Lu](https://www.linkedin.com/in/zeen/) , co-founder & CEO at Runpod, and[Eran Kampf](https://www.linkedin.com/in/erankampf/) , VP of Engineering at Twingate (formerly, co-founder and CTO at[monday.com](http://monday.com/) ). We discussed how AI is shifting engineering challenges and how each of us is dealing with them.


## Most AI incidents are context bugs, not code bugs


Look at the named agent failures of the last quarter. PocketOS losing its Railway database to a Cursor agent in nine seconds, after a credential mismatch in staging. Meta's March SEV1, where an agent passed every IAM check but performed an action its delegating human never authorized. Amazon's Kiro deleting AWS production after inheriting an engineer's permissions. McKinsey's Lilli walking through 22 unauthenticated endpoints to exfiltrate 46.5 million chat messages.


These get framed as permissions failures. They're not, at least not primarily. They're targeting failures: the agent did the right action against the wrong artifact, often in a window where the system's state was meant to be inconsistent. Migrations are the worst case because the agent's notion of "context" is wrong by definition.


Zhen Lu, CEO of Runpod, put the underlying question bluntly during our panel. His version was customer-facing: "How bad of a day is your customer going to have if you push some crappy code to production and it brings down their stuff?" The answer, increasingly, is that the bad day doesn't come from the code being crappy. It comes from correct code running against an environment the agent didn't fully understand. Permissions controls don't catch that. Linting doesn't catch it. Code review barely gets a glimpse of it.


## Reviewing faster doesn't catch context bugs


The constraint at Rootly has moved squarely to review. Over 80% of our pull requests have AI as the primary author. We layer AI-assisted review on top of that. Review is still the bottleneck. A human trying to read AI-written code at AI's writing speed isn't running a process: it's running a queue with a person at the front of it apologizing.


Eran put the broader version of this cleanly: before AI, the engineering team was throttled by how many engineers we have and how much code we can write, test, and deploy. That throttle is gone. Code output is some multiple of what it was. The constraint has moved from how fast you can write code to how confidently you can ship it.


The dominant industry response is to throw more AI at the review problem. CodeRabbit, Cursor's Bugbot, GitHub's Copilot Review, Greptile, Cognition's Devin Review, Sourcery, Qodo, PR-Agent. There's an active tool race for "review faster." I think it's the wrong abstraction.


CodeRabbit's own *State of AI vs Human Code Generation* report finds AI PRs ship roughly 1.7x more issues per PR and about 2x more error-handling gaps than human-authored ones. That's the case for *more* review. But review at the speed of AI authorship doesn't catch deep issues, it catches surface ones.


The deep issues, the ones that break customers, mostly look fine on the diff. They're failures of context, not failures of code. You don't catch them by reading faster.


## What does work: production-side defenses


Intercom's team has been running an interesting version of this experiment publicly. 93% of their PRs are agent-driven, 19% auto-approved with no human reviewer, and downtime from breaking changes dropped 35% as deployment volume doubled.


They framed it as AI review. The more honest framing is what they actually built: a deployment posture that catches regressions in production, fast, instead of trying to catch them at the diff.


That's the bet we're making at Rootly. Tight CI/CD with real smoke tests against staging that mirrors prod. Anomaly detection on the SLIs that correlate with customer pain. Automated rollback triggered by error-budget burn, not by a human's judgment call. Progressive delivery on anything customer-facing, with kill switches at every stage.


The thesis is that the marginal engineering hour is better spent on those primitives than on a faster review queue.


You don't catch the regression at the diff. You catch it within minutes of deploy and revert before customers feel it. It's early. I won't claim it works yet. But the math on reading every PR thoroughly stopped working a year ago, and the longer we pretend otherwise, the more review becomes ritual.


## Your tests have the same blind spot


Zhen flagged that AI-written tests are one of the most underhyped use cases right now. Given decent guidance, an LLM is a strong first-pass test author, and you can run a much wider automated test surface than you'd ever staff humans to write. Net positive. I agree.


But there's a problem in that picture I underestimated for too long. If the agent writing the implementation also writes the tests, the tests pass when they shouldn't, because the agent and the tests share the same broken mental model. The targeting bug that broke production won't be flagged by a test the same agent wrote, because both the test and the implementation believe the same wrong thing about the world.


CodeRabbit's report sees this indirectly: agents write tests around the new (broken) shape, and CI goes green. Russinovich and Hanselman's April 2026 piece in *Communications of the ACM* names it more directly. Agents "implement special-case hacks that pass tests but fail in production," because the test author and the implementation author are the same agent with the same context.


The structural fix is to ensure they don't share context. Use a separate agent with a deliberately different prompt: one explicitly looking for what could break the implementation, not what would prove it works.


Adversarial test generation, not confirmatory test generation. Any CI suite where the tests share a brain with the code is, at best, marking its own homework.


## Even with rollback, you need to know which agent did it


Eran's frame is Zero Trust. "You don't trust the actor, you verify the actions." That gets harder, not easier, when actions arrive at machine speed. Sandboxing, just-in-time access, third-party policy enforcement at the gateway: these stop being security hygiene and start being a precondition for letting agents touch anything in production.


There's a related problem he surfaced that's been bothering me since.


Agents currently act as the human running them. There is no per-agent identity. When an agent does something stupid, the audit log says the human did it. NIST's AI Agent Standards Initiative launched in February with agent identity as a core pillar.


The IETF has a draft on agent audit-trail formats. KubeCon EU 2026's platform-engineering day spent serious airtime on SPIFFE/SPIRE-for-agents, kagent, and[Solo.io](http://solo.io/) 's agentgateway. OWASP published an MCP Top 10. The substrate is moving.


Fast rollback gets you out of the incident. It does not tell you which agent, or which engineer's agent, caused it. Attribution breaks. Paging logic breaks. Audit trails on incident channels degrade. Gravitee's 2026 survey reports only 21.9% of organizations treat agents as identity-bearing entities. Kiteworks reports 60% can't terminate a misbehaving agent and 33% lack any audit trail. Those numbers are bad in their own right. They're catastrophic the first time you try to write an[incident retrospective](https://rootly.com/incident-postmortems) and discover you can't reconstruct what happened.


## The model isn't the leverage, the harness is


Almost every AI failure I've seen this year, mine included, has the same shape underneath.


The model did what it was told, against a world it had an incomplete picture of. The defenses that catch that aren't faster reviews or smarter prompts. They're production-side: rollback paths, anomaly detection, adversarial tests, identity boundaries, kill switches at the gateway.


That harness is platform-engineering work. CI hooks. Sandboxes. JIT scopes. Per-agent attribution. Feature-flag systems with sane defaults. Observability on the SLIs that matter. None of it is glamorous. All of it is the difference between an agent that ships value and an agent that lands a team on Hacker News for the wrong reason.


There's a corollary worth saying out loud. When you can ship anything, choosing what to ship becomes the constraint. Eran put it well: "I can load the product with tons of features, but it won't help customers or grow the organization."


The same logic applies one layer down. When an agent can do anything in your infrastructure, choosing what to let it do is the constraint. Both versions of that question used to be answered by implementation cost. Now they're answered by judgment about what should exist, and by the wrapping that keeps the answer honest.


## Stop fearing mistakes, build to absorb them


That's the era of engineering we're in now. The bug class we used to ship (wrong syntax, hallucinated APIs, off-by-one errors) was a writing problem, and writing is what code review was good at catching.


The bug class we ship now is a context problem, and context is what production-like smoke tests catch, robust tooling around production to instantly get back on your feet. The teams that figure this out fastest won't be the ones with the latest models. They'll be the ones who stopped trying to prevent every possible mistake at the diff and started building the infrastructure to absorb if it slipped anywhere else.


‍
