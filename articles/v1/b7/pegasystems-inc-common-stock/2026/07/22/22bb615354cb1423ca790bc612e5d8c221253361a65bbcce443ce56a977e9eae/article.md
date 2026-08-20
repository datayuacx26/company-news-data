---
schema_version: "1.0.0"
document_id: "22bb615354cb1423ca790bc612e5d8c221253361a65bbcce443ce56a977e9eae"
company_key: "pegasystems-inc-common-stock"
company: "Pegasystems Inc."
source_id: "pegasystems-inc-common-stock-news-import-897a88525e2a"
canonical_url: "https://www.pega.com/insights/articles/moving-fast-without-losing-control-what-governing-agentic-ai-actually-looks"
published_at: "2026-07-08T12:00:00+00:00"
first_seen_at: "2026-07-22T20:29:28.980825+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:f02f8184ce37234ddaa23b31c31efb559bda03deefc5d455c9866f1321e4a33a"
---

# Moving fast without losing control: What governing agentic AI actually looks like

Most enterprises have stopped debating whether[agentic AI](https://www.pega.com/agentic-ai) will arrive. The debate has shifted to[governance](https://www.pega.com/ai-governance) . Specifically, how to evolve from models built for deterministic workflows to ones built for systems that set goals, make decisions, and take actions at runtime.


At[PegaWorld 2026](https://www.pega.com/events/pegaworld) , I moderated a conversation with three leaders governing agentic AI in production across financial services, insurance, and healthcare. I’m Paul Barnes, Senior Director of Business Excellence for Intelligent Automation and Customer Service at Pega, responsible for adoption strategy for Pega’s agentic AI capabilities and the Lighthouse program, which is our co-innovation program helping clients and partners get to market with enterprise AI faster and with less risk.


Here’s what they shared.


## The moment everything changed


Every governance problem starts with a wake-up call. For Mamadou Seck, VP of AI Engineering at Navy Federal Credit Union, the world’s largest credit union, it came with scale:


**Mamadou Seck:** *"Every team moved at a different speed, doing very different things with it. Suddenly you have hundreds of agents and you don’t know what they can do, what identity, access, or entitlements they hold. We let creativity take over, and we lost control."*


Non-human entities accumulated entitlements no one could see, audit, or reason about. Shadow IT, reimagined as shadow AI.


Peter la Croix, Manager of Platforms and Integration at Achmea, one of the Netherlands’ largest insurance groups, saw the same tension from the governance side:


**Peter la Croix:** *"The business started experimenting with AI outside the pace and structure of centralized IT governance. We saw an explosion of ideas – proofs of concept, pilots – moving far faster than our traditional model could handle. If governance stays centralized and delivery stays slow, the business will route around it."*


From the contact center floor at Blue Shield of California, Shawna Maitia watched frontline work buckle under constantly changing regulations:


**Shawna Maitia:** *“During COVID, policies changed rapidly, and agents were overwhelmed trying to find the right information, apply it correctly, and document everything afterward. The human cost was high… frustration, burnout, inconsistent experiences for members.”*


The common thread: Governance problems don’t surface at design time. They emerge in execution.


## Why traditional controls break


The standard governance playbook of static approvals, deterministic testing, periodic compliance reports was built for predictable processes. Agentic AI isn’t predictable.


**Mamadou Seck:** *“Most of the governance we inherited was designed for deterministic processes – well-defined steps, predictable outcomes, neatly defined checkpoints where a named person makes the call. With agentic systems you’re no longer in that tidy sequence. The process itself is stochastic.”*


Peter’s team hit this on the compliance side. Achmea manages 34 platforms, each requiring monthly paper proof of compliance delivered to auditors. They built an agent to replace it: a continuous traffic-light dashboard showing green, orange, or red status across their entire landscape in real time. That is objectively better, but the auditors still wanted the printouts.


**Peter la Croix:** *“Traditional governance breaks because it’s optimized for reporting, not reality. Agentic systems force a mindset shift: governance moves from periodic proof to always-on observability.”*


Mamadou sharpened the structural challenge:


**Mamadou Seck:** *“Governance that used to happen at design time now has to move to runtime. It has to be implemented as code through observability, detecting signals, and maybe even implementing kill switches so you can stop agents from engaging in certain behaviors if you sense it could lead to a failure of compliance.”*


The problem isn’t that organizations lack governance. It’s that their governance is aimed at the wrong layer, and built for the wrong moment.


## What you can actually govern


If you can’t pre-approve every decision, what’s left to control?


Peter’s answer: govern the platform, not individual agent actions.


**Peter la Croix:** *“You don’t govern individual agent decisions. You govern the platform and context they operate in. Define non-negotiable guardrails: security boundaries, data access rules, clear constraints on what agents are not allowed to do. Enforce them at platform level. Once that foundation is in place, teams can move very fast, but there’s no way to bypass the guardrails.”*


Shawna’s team governs through human review and correction:


**Shawna Maitia** : *"When an AI summarizes a call, agents can correct it if something’s wrong. We retain the original output, the edited version, and a full audit trail. If we see patterns – dates being wrong, certain details consistently needing edits – we use that data to improve the agent.”*


Mamadou brings a three-question framework, competence, authority, and responsibility, to every agent before production.


**Mamadou Seck:** *“Is the agent actually competent at this task? What is it explicitly authorized to do? And who is the named person on the business side who takes the fall when it’s wrong? Based on that, the decision lands in one of three buckets: automated, spot-check, or hold.”*


The value of the framework is that it makes automation a socio-technical decision, not just a technical one – getting the right people around the table to decide whether something should be automated, not just whether it can be. Accountability cannot live with the AI team alone.


One often-underestimated dimension: security constraints extend to model selection itself. For Navy Federal, data residency requirements govern which models are permissible. A cloud LLM that can’t guarantee continental U.S. data residency isn’t an option, regardless of capability. In regulated industries, your governance model has to reach that far.


## Trust is earned at runtime


Moving from assistive agents to[autonomous ones](https://www.pega.com/autonomous-agents) isn’t a design decision, it’s an earned transition.


**Shawna Maitia:** *“Summarization has been a powerful gateway use case. It reduces cognitive load without introducing risk. Agents don’t feel replaced; they feel supported. We start assistive, measure outcomes, monitor quality, and only then consider expanding autonomy.”*


**Peter la Croix:** *“Start small, see how it behaves in practice, and only scale once you understand what it actually does. And if an agent starts doing something unexpected, adjust how it’s used. Don’t shut everything down.”*


The signals to watch: first-call resolution, after-call work time, consistency of output, and whether frontline staff feel supported or stressed. Shawna was direct about what change fatigue actually looks like on the floor:


**Shawna Maitia:** *“We get agents so much information, and it’s all by email. We’re expecting them to read those emails and still take their call and focus on the customer’s need. Whereas if you’re just surfacing information to them, they’re not going to be exhausted from all of that.”*


Surfacing information in context rather than pushing it at people isn’t just a UX improvement. It’s a precondition for trust.


## Who owns it when it fails?


When an[agentic workflow](https://www.pega.com/agentic-workflows) produces a bad outcome, the answer to “who gets the call?” has to be clear before deployment, not after.


**Peter:** *“It’s not about who owns a specific task, but who is accountable for the outcome. You don’t hand off responsibility. You share it and you stay responsible.”*


**Shawna:** *“Agents don’t make decisions, they inform them. Humans are accountable, and governance exists to support that responsibility, not override it.”*


**Mamadou:** *“Know every agent in your environment – who created it, what it can do, what data it can read, where that data lives. Shadow AI is already happening.”*


## Three layers of governance, and the one most organizations miss


An audience question at the end of the session surfaced a clean structural frame. Mamadou confirmed Navy Federal operates across all three layers:


**Approval governance:** Before a use case reaches technology teams, a member strategy office defines the target business outcome and what success looks like.


**Runtime governance:** Continuous observability of agent behavior, monitoring for patterns, anomalies, and guardrail breaches in real time.


**Outcome governance:** A formal review six months post-deployment: Did the agent actually move the needle on the metric it was built to affect?


Most organizations have the first layer. Some are building the second. Almost none have closed the third loop, and that’s where the accountability gap lives.


## What to do in the next 90 days


**Peter** : Build governance into the platform, not the approval process. Observability and guardrails first, then let teams move fast.


**Shawna:** Start with assistive use cases that make people better at their jobs. Earn trust through outcomes, not promises. Leaders have to go first. If they don’t understand the tools, their teams won’t buy in.


**Mamadou:** Get identity and access management for non-human agents right before anything else. Then invest in enterprise-wide education. Navy Federal calls it “augmented intelligence” deliberately to make clear the goal is humans getting stronger, not humans getting replaced. That framing matters, and leaders need to carry it.


The platform matters. Architecture matters. But the operating model is where trusted agentic AI is won or lost – the ownership structures, the runtime guardrails, the cultural willingness to share accountability across functions that historically haven’t had to.


The organizations moving fastest aren’t the ones that removed controls. They’re the ones that moved them closer to execution.


**Watch the full session replay —[Moving Fast Without Losing Control: New Rules for Governing Agentic AI](https://www.pega.com/insights/resources/pegaworld-2026-moving-fast-without-losing-control-new-rules-governing-agentic)**
