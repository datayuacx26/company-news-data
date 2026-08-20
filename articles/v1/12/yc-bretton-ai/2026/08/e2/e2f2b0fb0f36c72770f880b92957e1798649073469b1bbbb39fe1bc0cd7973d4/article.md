---
schema_version: "1.0.0"
document_id: "e2f2b0fb0f36c72770f880b92957e1798649073469b1bbbb39fe1bc0cd7973d4"
company_key: "yc-bretton-ai"
company: "Bretton AI"
source_id: "yc-bretton-ai-news-import-8e4d323a7594"
canonical_url: "https://www.bretton.com/blog/building-the-agent-was-never-the-hard-part-the-build-vs-buy-conundrum"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T02:02:46.014770+00:00"
fetched_at: "2026-08-19T02:02:48.152059+00:00"
content_hash: "sha256:252265690dc3a8b30d3cd1af3a3cd8713abe57caa49e7860f59b27aa5669875e"
---

# Building the agent was never the hard part: the build-vs-buy conundrum

*Prologue piece to a series on what it takes to run AI agents in production inside a financial institution. Later pieces go one layer deeper on the parts explored here: the evaluation layer, our agent builder, the agent harness, integrations, and the work of moving a legacy process onto agents and keeping it there.*


### The real build-versus-buy decision for AI agents in banking


A modern agent can research an entity, reason across conflicting evidence, use tools, and carry a multi-step investigation from intake to disposition. In a back office that has always run on analyst hours, that is a real change in what is possible. It is also the part of the problem that is already solved.


The foundation models work great and are getting better every week. Most internal agent projects at banks that stall do so somewhere else: in the distance between a demo that impresses a room and a system an institution can rely on, at production volume, inside its own controls. That distance is where the build-versus-buy conundrum is actually decided, and it is almost always underestimated.


### A demo is not a system


Handling a curated case in a sandbox is not the same as running an agent across real volume, inside real systems, under real policy. The demo shows capability. Production demands reliability, integration, auditability, and the ability to hold all three as the work and the tools change underneath you.


First Internet Bank shows the difference in practice. Bretton AI's initial workflow was running within the first week, followed by a 30-day calibration period that embedded the system into the bank's operating process. Across four compliance workflows, review time fell from 8 to 10 hours to under one hour, saving more than 24,000 hours annually. The result was not a faster model response. It was a repeatable, auditable operating process spanning diligence, loan origination, fraud, and periodic reviews.


Reaching that point is where most of the work lives, and where most projects underestimate what they have taken on.


### Why agent projects stall before production


The failure modes are consistent.


**Encoding the institution is harder than it looks.** An agent performs only to the standard it is given, and in a bank that standard does not live in one clean document. It is spread across policies, SOPs, escalation rules, QA criteria, risk appetite, and years of prior decisions. An SOP that says "check the corporate registry, then cross-check the website" leaves open which registry, what counts as a match, what to extract, and what to do when sources disagree. The analyst fills that in from context; the agent has none. Encoding the work is really reconstructing the operating logic behind it. That translation is not theoretical. At Meso, Bretton AI configured alert reviews around the company's existing SOPs and customer risk-rating methodology. The system assembled evidence and maintained source logs, dispositioned clear false positives, and escalated cases that required risk-based judgment to human reviewers. Meso reports that setup took under two weeks and required no engineering resources.


**Integration is the critical path.** An agent creates value when it completes the workflow, not when it can reason about it. That means reaching case management, transaction data, KYC and KYB providers, screening platforms, registries, and internal systems, several of which have no usable API. The agent can be finished long before it can touch the systems where the work happens. When Sling needed its screening service connected, Bretton AI reached it in about two weeks. An agent that cannot reach the work has not reduced it.


**Workflows compound failure.** A single investigation may chain identity resolution, counterparty research, screening, ownership analysis, document review, and disposition. Each step can look strong on its own while the end-to-end case stays unreliable: nine independent steps at 95 percent each succeed together only about 63 percent of the time. Good architecture narrows that gap; it does not close it. Task accuracy is not case accuracy, and the case is the only measure that matters in production.


**Production needs a second system.** Getting an agent right once proves little. Models, tools, providers, and policies all change, and every change is a chance to regress. Knowing whether the agent still performs means building an evaluation layer around it: ground truth, rubrics, regression tests, traces, and monitoring. Without it, every material change becomes another manual validation project.


How multi-step workflows compound failure


### Banks raise the bar


These problems exist in every industry. Banks make them more challenging because the cost of a wrong output is different in kind. An agent here can touch onboarding, investigations, risk decisions, and money movement, inside a control environment of compliance, model risk, internal audit, and privacy. A bad answer becomes a missed risk, an incorrect disposition, an audit finding, or a remediation program. The data is harder too: names cross alphabets, entities share identities, ownership sprawls across jurisdictions that disagree, and a missing result is not a clean one. A general agent framework gives you models, tool calling, and orchestration. It does not give you the financial-services intelligence to resolve those identities or measure the work against a real standard. That still has to be built.


### The question is not "can we build," but "what do we own after"


For many institutions, the answer to "can our team build an agent" is yes. The better question is what you have signed up to own once it is built. The model API spend is the least concerning line in the budget. An internal build means owning the harness, the financial-services skills, the integrations, the evaluation corpus, and the release process, and none of it ends at launch. Models change, providers change, policies change, integrations break, and every new case surfaces a new way to fail. A build is not a project with a finish line. It is a standing obligation, and it has a failure mode that rarely makes the business case, because it is a human problem.


### What happens when the people leave


One of our senior leaders lived this before joining Bretton AI. At a global bank, a strong team built a sophisticated set of internal tools to automate operational work. The tools were good. They shipped, they ran, and for a while they did exactly what they were built to do.


Then the team changed. People moved to other groups, others left, and the specific knowledge of how the tools were wired, why a threshold sat where it sat, what to do when a data source shifted, left with them. Nothing failed loudly. The tools degraded quietly, the way undocumented systems do, until they were producing work no one fully trusted and no one still on staff knew how to fix. The automation that was meant to remove operational load became operational load, and the work fell back onto people.


The lesson is not that the team was careless. They were absolutely capable, and the tools worked the day they shipped. The lesson is that an agent is not a deliverable you hand off and walk away from. It needs continuous care: supervision, retraining as the work drifts, and someone who understands it well enough to fix it when the ground moves. The hardest part of production is not the launch. It is the years after, and those years depend on people staying, staying current, and staying interested. That is a fragile thing to build a control function on.


### When building is the right call


There is a real version of build. If a team decides the back office itself is strategic ground it wants to own, automate, and drive cost out of as a durable advantage, building can make sense. So can a genuine constraint, where security, architecture, or governance rules out any outside platform. In each case the institution is choosing to permanently staff the problem. A strong engineering group can carry that. The question is whether rebuilding and maintaining that infrastructure is the highest-value thing that group could do, and whether the institution will keep staffing it long after the launch that justified it.


### Buying isn’t end all, be all


Buying is not free of trade-offs, and pretending otherwise is the inauthentic argument that this series is seeking to avoid. When you buy, you take on a dependency: your production workflow rides on another company's roadmap, release cadence, and continuity. You fit your process to a platform's model of the work rather than the reverse. For work that is genuinely proprietary, that can matter.


For many institutions the honest answer is neither pure build nor pure buy, but a hybrid: buy the undifferentiated production infrastructure (the harness, evaluation, integration patterns) and build on top only where the institution has a real edge. The point is not that building is always wrong. It is that building the entire production system underneath a model is rarely where an institution's advantage actually lives.


### Why buying wins for most


For most institutions, the workflow is strategic and the infrastructure under it is not. Two things decide it, and both are observable rather than asserted.


Time to value is measurable, and worth being precise about. In Bretton AI deployments, initial workflows have reached production on materially shorter timelines than an internal build: Meso reported setup in under two weeks without engineering resources, and First Internet Bank reported a one-month time to value that included a structured calibration period. Initial deployment, calibration, and full rollout are different milestones, and honest time-to-value names which one it means.


Quality compounds across workflows. A purpose-built platform brings reusable back-office capability rather than a blank architecture. HitPay deployed Bretton AI across KYB, EDD, periodic reviews, and sanctions, PEP, and adverse-media screening, completing 3,000 EDD cases and enriching more than 1,000 businesses in its first year. Breadth like that is the thing an internal build has to accumulate one incident at a time.


What you actually buy is the production layer, with a real mechanism behind each part:


- **Builder** compiles a customer's policies and SOPs into a layered agent configuration and freezes it before runtime, so the version that was reviewed is the version that runs.
- **The harness** enforces what should never depend on a model remembering it: material figures are computed deterministically and the narrative binds to those values, a citation counts only when the system knows the agent actually retrieved the source, and required checks are carried as items that must resolve or stay visibly failed.
- **Integrations** reach the systems where the work lives, including those with no clean API, and return results to the system of record.
- **Trust Infrastructure** reviews completed work against the customer's standard, ties every finding back to the source or trace step behind it, and is calibrated against human reviewers so the quality gate can be trusted.


Each of these gets its own piece later in the series. And the auditability is not only internal: at First Internet Bank, Bretton AI's diligence work was described as one factor contributing to a clean regulatory examination.


The strategic capabilities you must own if you choose to build vs. buy Bretton


### What this prologue does not prove


None of this shows that Bretton AI is the right platform. It shows that production is a system, and that most institutions should not build that system themselves. Whether to buy ours is a narrower question, and the only honest way to answer it is with evidence: does the platform actually hold up in production, at volume, inside a bank's controls?


That is what the rest of this series is for. We start where a skeptic should, with the numbers, in totality, with absolutely no cherry-picking.
