---
schema_version: "1.0.0"
document_id: "e17461b79f7292e89ad9e0f39e9ff060f2bdb3a7e8a01da37eb4408ba2f79e33"
company_key: "yc-gpt"
company: "Nexus"
source_id: "yc-gpt-news-import-c235d041488d"
canonical_url: "https://agent.nexus/blog/nexus-raises-4-3m-seed-shipping-cue"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-22T06:05:29.374926+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:8f826351471fa84f63cdcf39c1cc81e5a4ee9ef8f6fe9e32d4f8e8cf96be035b"
---

# Nexus Raises $4.3M to Remove the Implementation Layer From Enterprise AI

Today we're announcing that Nexus has raised $4.3M in seed funding led by General Catalyst, with participation from Y Combinator (we're part of the F25 batch), Transpose Platform, Twenty Two Ventures, Phosphor Capital, and angels including Gokul Rajaram, Raphael Schaad, and Jake Mintz.


The headline is the easy part. The why is more interesting.


## The problem we kept seeing


Three years ago, before starting Nexus, I was a consultant at McKinsey. Every CIO and CMO I worked with had a version of the same problem: a backlog of AI initiatives that looked impressive in a slide deck and went nowhere in production. Pilots that wowed the steering committee. POCs that consumed quarters of effort. Engagements that produced thoughtful frameworks and zero shipped agents.


The pattern wasn't about the models. The models got better every quarter, faster than anyone could absorb. The pattern was that building, deploying, and governing an AI agent that does real end-to-end work inside a regulated enterprise — with the audit trails, role-based access, and data-residency controls a CIO can sign off on — required a team of senior engineers most companies didn't have, couldn't justify hiring, and certainly didn't want to hire one of per use case.


The industry's default answer was to wrap that engineering layer in a[consulting engagement](https://agent.nexus/blog/how-to-deploy-enterprise-ai-without-consultants) . Forward Deployed Engineers writing custom integration code on a customer's laptop. PRD ping-pong between business and IT. 12 weeks of bespoke implementation per agent, then another 12 for the next one. The economics of that model favoured slow, high-touch delivery, and the failure rate stayed where it had always been: somewhere between 70 and 80% of enterprise AI initiatives never made the leap from pilot to production, with the consulting-led category over-represented in the failure column.


Shady Al-Shoha and I started Nexus in 2024 to solve a different version of that problem.


## What we're shipping today: Cue


[Cue](https://agent.nexus/compare) is the[autonomous build loop](https://agent.nexus/blog/how-to-deploy-autonomous-ai-agents) you can watch above. It scopes the use case with the user, asks the right clarifying questions, connects to internal data with the correct permissions, builds the automation, tests it against realistic scenarios, and publishes it to the team — all in one to two hours, end to end, on its own.


That isn't a chatbot generating code snippets a developer compiles later. It's an agent that lands a production agent before the kickoff lunch is over, with the governance layer (audit trails, role-based access, data residency, compliance certifications) treated as the foundation rather than something bolted on after the fact.


Three principles we've stayed stubborn about as we've shipped.


**Outcome before output.** Every agent we deploy has a measurable business KPI tied to it from day one. If an agent isn't moving a number that matters — conversion, time saved, cost avoided — we rescope rather than ship something that looks productive but isn't.


**Governance is the foundation, not a feature.** If a CIO can't sign off on the data flow, the access model, or the audit trail, the agent doesn't ship. Several of our enterprise customers wouldn't have started a single pilot without that posture.


**The product does the implementation. Humans handle change management.** This last one is the substantial bet, and the reason for the org change below.


## Why we restructured delivery


We are restructuring our delivery team. Forward Deployed Engineers, which had become the standard delivery role across enterprise AI providers, are being[replaced inside Nexus](https://agent.nexus/blog/how-to-run-ai-transformation-without-consultants) by Forward Deployed Consultants.


The same people sit with our customers, but the job is different. They no longer write integration code or build agents on the customer's behalf — Cue does that, autonomously, in the time it takes to scope the use case. What they own is the part of enterprise AI rollouts that is actually hard and that no platform on its own can solve: identifying the workflows worth automating, designing the change management around adoption, defining the KPIs the agents will move, and running the political and operational work that turns a working agent into a meaningful business outcome.


This is counterintuitive even to us. The industry assumption is that[enterprise AI deployments require more engineering touch over time](https://agent.nexus/blog/how-to-deploy-ai-without-system-integrators) , not less. Our data says the opposite. The earlier the product can stand on its own, the faster customers compound on it. The second agent at a customer leans on the data the first one is generating. The third learns from both. By month six, the customer no longer has one agent — they have a small operating system, and our consultants are running the change-management work that lets that compounding actually land in the business.


## What 12 enterprises taught us


We have worked with 12 enterprises representing more than $70B in combined market cap, including[Orange Group](https://agent.nexus/telco) , Lambda.ai, and Proximus Global. To date, we have shipped 174 agents into production. Those agents have generated more than $13M in customer revenue and cost savings. Churn sits at 0%. Success rate, measured against the KPIs each agent was scoped against, is 100%.


The clearest illustration is Orange. Their team deployed an autonomous customer onboarding agent in four weeks. Conversion lifted by 50%. A single agent now drives more than $6M in annual LTV. Customer satisfaction climbed 10 points, and the consistency of conversations week-over-week is better than what a globally distributed support team can produce on its best day. At Lambda.ai, agents now run across sales and marketing, replacing work that previously took analysts hundreds to thousands of cumulative hours and unlocking the parts of the team's job that had never been able to scale.


The thing that has surprised us most isn't that agents work. We knew they would. What we didn't fully appreciate ahead of time was how fast they compound once the implementation layer is no longer the rate-limiting step.


## The investors backing this


We chose our partners deliberately. General Catalyst, with Yuri Sagalov leading the round, saw the same thesis we did: the platform matters, but obsessing over deployment is what makes the platform real. Y Combinator backed us at the earliest stage and has shown up in every round since. Transpose Platform, Twenty Two Ventures, and Phosphor Capital each brought specific operator wisdom we wanted on the cap table, and the angels — Gokul Rajaram, Raphael Schaad, Jake Mintz — are people who have shipped real things to real customers and don't pretend otherwise.


## What's next


Three priorities for the funding.


**Platform depth.** We are investing heavily in Cue itself: multi-agent orchestration so a single user request can spawn a coordinated team of agents, evaluation infrastructure so customers can measure quality continuously rather than at audit time, and the underlying systems that turn one agent into a small operating system inside a customer's stack.


**Integrations that go deeper.** We support more than 4,000 integrations today. The next phase is going meaningfully deeper on the ones our customers run their business on — SAP, Salesforce, Microsoft Dynamics, ServiceNow — so the agents can act on the systems of record rather than around them.


**Forward Deployed Consultants.** We are hiring across Brussels, San Francisco, and remote across Europe and North America. If your job today involves helping enterprises adopt new technology, and you have watched the implementation layer eat the substance of the relationship, come build the post-implementation playbook with us.


## Try Cue


If you're inside an enterprise and you've been stuck at the "we ran a pilot" stage for too long,[book a Cue session](https://calendly.com/d/crcb-qfd-592) — we'll scope your first agent on the call.


If you're a Forward Deployed Consultant who wants to do change management on top of a product that actually ships, we're hiring.


We're early. The next two years are the ones that matter.
