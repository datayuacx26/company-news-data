---
schema_version: "1.0.0"
document_id: "8db67355e045d63f3f7c1c18cdedd70f4327ab40609ee8ffb67958c1a2ce86de"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/conways-law-ai-factory/"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:979b7884647d161bb28c35004dbb9196395b19d66adf27f6ad00abb25e792e82"
---

# Conway's Law 2.0: The VP's Playbook

*This is Part 3 of the AI Software Factory series.*


In[Part 1](https://speedscale.com/blog/sdlc-shift-ai-agile/) , we examined how “elastic code” is breaking the Agile SDLC. In[Part 2](https://speedscale.com/blog/ai-software-factory-context-layer/) , we explored the technical architecture of the AI Software Factory: a system where a Unified Context Layer enables a Funnel of Increasing Trust, replacing fragmented tools and manual gates.


But software is fundamentally a human endeavor. If the machinery of the factory changes, the people operating it must change as well.


As a VP of Engineering, your hardest challenge in the coming years will not be selecting the right LLM or building the Context Layer. Your hardest challenge will be reorganizing your humans. We are entering the era of **Conway’s Law 2.0** .


### Your Org Chart is Your Architecture


[Conway’s Law](https://en.wikipedia.org/wiki/Conway%27s_law) famously states that organizations design systems that mirror their own communication structures. If you have a frontend team, a backend team, and a database team, you will inevitably build a three-tier architecture.


But what happens to Conway’s Law when 80% of your “team” consists of autonomous silicon agents?


If you maintain a traditional, siloed Agile org chart (where humans are grouped by functional coding syntax, e.g., “The React Team” or “The Go Team”), you will artificially constrain the AI. The agents will be forced to operate within the narrow bounds of those human silos, passing context back and forth through slow PR processes.


To build the Factory, you must flatten the org chart.


The division of labor shifts away from *syntax* (Frontend vs. Backend) and toward *domain intent* (User Onboarding vs. Checkout). Because agents can generate code across the entire stack simultaneously, human engineers no longer need to specialize in a single language. They need to specialize in the business domain.


```text
flowchart TB
subgraph Old["Conway's Law 1.0: Syntax Silos"]
direction LR
FE[Frontend Team<br/>React, CSS]
BE[Backend Team<br/>Go, Java]
DB[Database Team<br/>SQL, Schemas]
end
subgraph New["Conway's Law 2.0: Domain Pods"]
direction LR
Onb[Onboarding Pod<br/>humans + agents<br/>full stack]
Chk[Checkout Pod<br/>humans + agents<br/>full stack]
Bill[Billing Pod<br/>humans + agents<br/>full stack]
end
Old ==>|elastic code collapses<br/>functional silos| New


```


This is the inverse Conway maneuver: deliberately restructure the org to force the architecture you want, rather than letting your existing communication structure dictate the system you build. Amazon’s two-pizza teams in the early 2000s were an inverse Conway move; the team boundaries were drawn first, and the service boundaries followed. Conversely, plenty of companies have tried to ship microservices with a single, undifferentiated platform team and ended up with a “distributed monolith”: services in name only, all owned by everyone, deployable only together. The team shape leaked through.


In the AI era, the inverse Conway maneuver becomes more aggressive. You are no longer just shaping how humans communicate; you are shaping where context flows. A pod that owns Onboarding end-to-end can give its agents a tight, complete context window: the user model, the email service, the analytics events, the rollout flags. A frontend team that asks an agent to “fix login” without that context will get plausible-looking React code that breaks the second it touches the auth service. The agent inherits your org chart’s blind spots.


### The Developer as Orchestrator


The most common anxiety among engineers today is, “Will AI take my job?” The answer is no, but it will fundamentally change the definition of the job.


We are moving from a world of **Syntax Experts** to a world of **Systems Orchestrators** .


In the Agile era, an engineer’s primary value was translating a Jira ticket into highly specific, syntactically correct code. In the Factory era, that translation is commoditized. The new primary value of the human engineer is found at the edges of the Funnel of Trust:


1. **At the Top (Intent Articulation):** Humans must clearly define the architectural boundaries, business rules, and product intent that feed into the Context Layer. They are writing the prompts that run the factory.
2. **At the Bottom (The Carbon Gate):** When the code passes through the Silicon Critic and the Deterministic Gauntlet ([traffic replay](https://speedscale.com/proxymock/) , E2E tests), the human acts as the final arbiter. They review the system for high-order architectural drift, ethical alignment, and user experience nuance that models cannot yet grasp.


Engineers become editors, architects, and orchestrators. They manage a fleet of agents, continuously refining the factory’s validation engine to ensure the output remains true to the intent.


### Governance at the Speed of AI


In traditional SDLCs, security, compliance, and architectural review are often treated as post-development gates. A security team reviews the code right before deployment.


In the AI Software Factory, post-development governance is fatal. Because agents can generate and propose millions of lines of code a week, a human security review board will instantly become the organization’s largest bottleneck.


Governance must shift from a *human gate* to *embedded code* .


Security policies, compliance requirements (like SOC2 or GDPR), and architectural standards must be injected directly into the Unified Context Layer. The “Silicon Critic” (the adversarial LLM we discussed in Part 2) must be fine-tuned on your organization’s specific governance rules. When an agent proposes a change, it should be automatically rejected by the factory floor if it violates compliance, long before a human ever sees it.


A concrete example. SOC2 CC7.2 requires that systems detect and respond to security events. Translated for a Silicon Critic, that becomes a rule like: *Any handler that touches PII (user email, billing, address) must emit a structured audit event with the user ID, the action, the timestamp, and the requesting service. If the change adds, modifies, or removes such a handler, verify the audit event is still present and structurally valid. Otherwise, reject and explain.*


This is not novel as a rule; it has been encodable in tools like OPA, Semgrep, and CodeQL for years. What is novel is the encoding format. The Silicon Critic does not need a precise AST query or regex; it needs a clear specification in natural language plus a few worked examples. That dramatically lowers the barrier for compliance teams to author rules without becoming static-analysis experts. Your governance backlog stops being “find an engineer to encode this” and starts being “write the spec and worked examples; ship to the Critic.”


When rules conflict (the change satisfies one policy but violates another), the Critic escalates rather than rejecting silently. That escalation is where the human Carbon Gate earns its cost.


### Metrics That Matter: Retiring Velocity


If you are managing an AI Software Factory, you must throw out your current engineering metrics.


“Velocity” (story points burned per sprint) and “Lines of Code” are obsolete. When a machine can write 10,000 lines of code in a minute, measuring output volume is measuring the wrong thing.


The new North Star metric for VPs of Engineering is **Intent-to-Impact Latency** .


This is the measurement of how long it takes for a validated business idea (the intent) to navigate the Context Layer, pass through the Funnel of Trust, deploy to production, and return observable user behavior (the impact) back into the system.


Concretely, instrument these stage transitions:


- **t0:** Intent committed to the Context Layer (the moment a PRD, RFC, or product brief is approved and indexed)
- **t1:** First agent-generated proposal lands as a PR
- **t2:** Silicon Critic and Deterministic Gauntlet pass (Levels 2 and 3 of the Funnel)
- **t3:** Human Carbon Gate approves
- **t4:** Deploy to production
- **t5:** First observable user signal (a successful event, a metric movement, an error rate change)


t5 minus t0 is your Intent-to-Impact Latency. The DORA “lead time for changes” metric measures roughly t1 to t4; it skips the upstream (was the intent even clear?) and the downstream (did the change actually move anything?). For a factory, both ends matter more than the middle.


A worked example: a team adds a new email opt-in to checkout. Intent committed Monday 9am. First agent proposal at 9:08am. Silicon Critic and replay pass by 9:42am. The human reviewer is in three meetings and approves at 2:15pm. Deploys at 2:30pm. First successful opt-in event at 2:31pm. IIL = 5 hours, 31 minutes. 99% of that latency is the human approval step. That tells you exactly where to invest: not in faster agents, but in either tightening the Silicon Critic so the human trusts it more, or in restructuring the human’s interrupt pattern.


### What This Means for Junior Engineers


The most uncomfortable question in this transition is the one few VPs are answering directly: where do junior engineers come from in an AI Software Factory?


In the Agile era, juniors learned by writing the first draft. They sat next to a senior, wrote bad code, got it reviewed, and over five years built the pattern recognition that lets a senior glance at a diff and feel something is wrong. That apprenticeship loop assumed humans were the cheapest way to generate code. They are no longer the cheapest. The first draft is now an agent’s job.


The risk is a missing generation. If juniors never write the first draft, they never build the muscle memory needed to oversee an agent’s output. They will see “the code passes the tests, the Silicon Critic approved it” and have no instinct that says *this looks wrong* . The Carbon Gate degrades. Worse, the senior engineers retiring in fifteen years will be the last cohort who learned by suffering.


There is no painless answer, but the inversion is becoming clear: juniors should pair with agents the way they used to pair with seniors. The apprenticeship is no longer “watch a human write code”; it is “watch an agent write code, then explain back what it did wrong, why, and what the better version looks like.” The teaching material is the agent’s failures. Senior engineers shift from writing reference code to curating reference critiques, the same way a writing teacher does not write the student’s essay but teaches them to read.


Hiring will follow. The interview question stops being “implement a binary search” and becomes “review this agent-generated PR; what would you push back on, and why?” The thing being tested shifts from production capacity to taste.


### Conclusion: Leading the Transition


The shift from Agile to the AI Software Factory is not merely a tooling upgrade; it is an industrial revolution. It requires VPs of Engineering to stop managing the *assembly* of code and start managing the *machinery* that builds it.


You must champion the creation of the Unified Context Layer. You must implement the Funnel of Trust. And most importantly, you must give your engineers the permission and the training to stop typing syntax and start orchestrating systems.


The organizations that cling to two-week sprints and manual task management will be outpaced by those that learn to manage intent at the speed of silicon. The Factory floor is open. It’s time to start building.
