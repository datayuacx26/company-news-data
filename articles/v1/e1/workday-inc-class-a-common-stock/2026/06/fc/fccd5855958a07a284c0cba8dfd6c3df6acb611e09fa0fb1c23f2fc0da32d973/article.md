---
schema_version: "1.0.0"
document_id: "fccd5855958a07a284c0cba8dfd6c3df6acb611e09fa0fb1c23f2fc0da32d973"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/by-zeke-miller-director-machine-learning-engineering-at-workday-b5a8cbd55c97"
published_at: "2026-06-16T22:19:23+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:bd88040fb4b9dd458d64d8668427c06ce7a31bfd2728cc08c6376732df8b1e9e"
---

# Designing Durable Agentic Systems: Thin Orchestration, Thick Data & Governance in Enterprise AI

[Workday Technology Writer](https://medium.com/@workday_technology?source=post_page---byline--b5a8cbd55c97---------------------------------------)


8 min read


·


Jun 16, 2026


--


Press enter or click to view image in full size


*By Zeke Miller, Director, Machine Learning Engineering at Workday*


Two years ago, my team spent six months building a conversational analytics agent. We wired up custom tool-calling loops, built our own orchestration code, and spent weeks tuning our evaluation harness. It was a legitimate engineering effort, the kind of work you’re proud of, and the evals we built there are what made the comparison meaningful later. A year later, someone on our team wired the same underlying analytics system into an off-the-shelf integration framework using a single, well-designed connector. When we ran our evals against that setup, it outperformed our custom stack with essentially zero additional work. Same data, same evals, different agent layer. The evals were the constant. **That moment changed how I think about durability in agentic systems** . And it’s probably a moment you’re having right now too.


### **The Core Problem: Speed ≠ Durability**


If you’re building agents in 2026, you feel this tension: models, tools, and patterns evolve faster than ever. Yet the domains we work in, HR, finance, compliance, are the ones with the least tolerance for breaking changes. The instinct is to build something robust that lasts. The reality is that robustness now means something very different than it did five years ago. I’ve spent the last several years at the intersection of coding tools, analytics, and enterprise platforms. The throughline isn’t about finding the perfect stack. It’s about **designing systems that can evolve without being rebuilt.**


### **Mental Model: Thin Agent Layer, Thick Foundation**


Here’s what guides most everything I do: **The agent layer should be thin. The foundation should be thick.** Most engineers think intelligence lives in the agent: complicated tool graphs, custom planners, bespoke memory systems. In enterprise contexts, that’s backwards. The thick part is your platform: evals, data models, security, governance, auditability. That’s durable. That’s where the hard constraints and a lot of the business intelligence live.


On top of that you have two fast-moving pieces:


- The **foundational model** : Claude, Gemini, etc., and
- A thin **agent layer** : prompts, tool wiring, orchestration logic


The agent layer matters for usability and performance, but it should be designed assuming you’ll rewire it often as models and paradigms evolve, or even bypass it entirely when users plug tools like Claude Desktop or OpenClaw straight into your APIs. In our analytics story, the intelligence was split: the BI model (LookML) carried deep semantic knowledge of the data, and the frontier model was increasingly capable of generating SQL and Python on its own. Once the LookML and APIs were solid, orchestration became nearly trivial. When a better framework emerged, we swapped it out without touching the foundation, and the newer models were simply good enough to replace a lot of the bespoke agent logic we’d built. Being able to ride both curves, the model frontier and the agent‑design frontier, without rebuilding your platform is the real differentiator. If your agent is monolithic and tightly coupled to a specific model or framework, the cost of adapting becomes prohibitive.


**Three Principles:**


1. **Invest heavily in the system of record and system of action:** APIs, security boundaries, access controls, workflows, make them explicit and composable. These don’t change when models get better, which means you can reap the benefits of the model improvements on day one.
2. **Make the agent layer declarative and interchangeable:** Clear tool contracts, minimal bespoke state, well-defined orchestration interfaces. If someone can swap your framework and improve performance, that’s a good sign.
3. **Expect to rewrite the agent layer frequently, and design so that’s okay.**


Don’t treat agent code as sacred. Expect it to be replaced. Durability means the parts that should change can do so without breaking the parts that must not.


### Code is Cheap. Assumptions Are Expensive.


Between code-generation models and new dev workflows, you can stand up a working demo, front-end, backend, voiceover, in an hour or two. On my team, PMs prototype experiences in agentic coding tools before we formally kick off engineering. This speed is incredible. It’s also a great way to bake in assumptions you’ll regret for two years. Simple rule: **Nail one use case (or two)** . Then scale the patterns. Instead of designing a unified platform up front, we focus on solving one problem really well, with real customers, in production. We learn what works and what doesn’t. Only then do we generalize. Real example: Evals and dashboards. Everyone wants a unified view of agent performance. But each agent is solving different tasks. The meaningful metrics aren’t generic:


- [Recruiting Agent](https://www.workday.com/en-us/products/talent-management/ai-recruiting.html) **:** Did this move a candidate forward and respect guardrails?
- [Travel Agent:](https://newsroom.workday.com/2026-05-21-Workday-Announces-Sana-for-IT-Service-Management-and-New-Travel-Agent) **** Did this plan a flight between the right cities on the right dates, and automatically manage expenses?
- [Legal Contract Intelligence Agent](https://www.workday.com/en-us/artificial-intelligence/ai-agents.html?tab=Agentic-Legal) **:** Did this identify key terms and legal risks accurately without missing critical clauses?


We could try to spend weeks collecting requirements from every agent team and still miss what actually matters in practice. Instead we:


- Provide a minimal, hackable starting point
- Let teams fork and customize for what they need *right now*
- Periodically pull everyone back together, standardize only what’s proven useful


That tradeoff, **shipping to learn the real requirements, then consolidating, beats trying to design the perfect cross‑agent dashboard in a vacuum.**


### **Evals as the Contract**


The thick foundation is the system of record, and the thin layer is the agent veneer. What keeps that thin layer honest? **Evaluations.** I think of evals as an *executable contract* between system and users. Those same evals are what let us see, very clearly, that the “one‑connector” implementation beat six months of custom agent work. The agent layer changed; the contract didn’t .A PRD says, “Users want to see time off remaining.” An eval makes that measurable:


- **Input:** A user, context, question
- **Expected behavior:** Right data, right format, right permissions
- **Pass/fail:** Assertion you can validate


The critical insight: this contract doesn’t care about implementation. Frontier models, hand-rolled loops or tomorrow’s framework, or MCP servers, if the eval passes, behavior is correct. This is powerful. It decouples user intent from implementation. You can swap models, update prompts, change tools, the contract holds. This turns improvement into optimization. With evals plus logging, you iterate toward better behavior, seeing immediately if you improved.


For one agent, we have three eval classes:


- **Correctness:** Can the agent generate valid schema changes?
- **Safety:** Does it respect access control?
- **Satisfaction:** Do outputs match what users actually needed? (Weekly feedback from real admins.)


We run correctness and safety on every commit. When something breaks, we have:


- Full traces and logs
- Ability to replay and reproduce
- A place to encode the fix as an eval


This creates a self-improving agent layer over time, not because the agent magically learns, but because we’ve built the teacher (evals and feedback loops) into the system.


### **Governance: Not Optional at Enterprise Scale**


Early agent experiments happen in low-risk environments. We’re different: extremely sensitive HR and financial data, workflows where mistakes have real consequences. My rule is simple: never rely on an LLM to “do the right thing” by intuition. Instead, design systems where even unexpected model behavior can’t become a problem. The approach is to apply to agents what you’d already do for humans: strong, explicit access controls, principle of least privilege, clear audit trails, and scoped permissions aligned with real roles. These are foundational security practices. Agents need the same treatment. One concrete way to operationalize this is to treat agents as first-class security entities with specific capabilities. Rather than hoping an agent stays within bounds, you define precisely: which data can it read, which workflows can it initiate, whose behalf can it act on, and under what conditions. In practice, that means sandboxing agent interactions: scoping what they can see and do so that any unexpected behavior is contained to a narrow blast radius. This is the same permission model you’d apply to any service or user in your system. This design approach lets you do three things:


- **Limit the blast radius** of unexpected behavior, if the model does something surprising, the security boundary catches it.
- **Reason about permissions consistently** , the same way you do for humans and services, using frameworks you already understand.
- **Plug agents into existing governance frameworks** instead of inventing parallel systems for AI. Your audit logs, compliance dashboards, and approval workflows all work the same way.


Projects like OpenClaw are great for showing the upper bound of what’s possible when you almost completely ignore security and governance, they’re a glimpse of the form factor we should be aiming for. Our job is to bring that level of usefulness into a world where people and money data live, and to re-introduce guardrails, auditability, and trust without losing the magic.


### **Moving Fast in an Enterprise**


Agent Factory is a startup from within. That’s less about aesthetics and more about who’s allowed to question assumptions. Two concrete examples:


- **Release Cadence:** Traditional enterprise: six-month cycles. In AI, that’s too long. If your design is frozen for six months while the frontier moves, you ship something outdated the day it lands. We take agents from concept to early access in roughly a quarter. Then iterate weekly with design partners, responding to model improvements, user feedback, new patterns. This speed doesn’t sacrifice rigor. Evals, observability, governance stay baked in. We just don’t wait six months between feedback cycles.
- **Platform Openness:** Historically Workday owned the UI. For good reasons, coherent experience, security, measured rollout. But agentic systems push toward channel-agnostic access: Workday as a governed system of record accessible from chat, third-party tools and custom apps.


We’re proving the model works through early access agents in Slack, showing value. Evidence makes the case to teams originally skeptical about opening the platform.


### **The Engineers I Want Next to Me**


If you’re an engineer considering where to focus on agentic systems, here’s what we care about:


- Hungry to **build and ship** things that real customers use.
- Comfortable with ambiguity, but opinionated about contract and boundaries.
- Understand that durability now means designing for change, not avoiding it.
- Loves scrappy demos for what they are, the first iteration, not the finish line, and cares just as much about the governance and consequences of what we eventually ship.


From where I sit, the upside is that agentic systems in enterprise contexts are both technically deep and directly tied to business value. When done right, we change how organizations manage work, pay, careers, compliance at scale. Code may have a six-month half-life. Architectures, tools, and best practices will continue to evolve. Our goal is to build agents that can move with that frontier while standing on foundations we’ll be proud of years from now.


Press enter or click to view image in full size
