---
schema_version: "1.0.0"
document_id: "1c8314ede7b4a9ad0d484da478d2e0daf021358cb23c7a07f1d745256a7e2a44"
company_key: "yc-clearly-ai"
company: "Clearly AI"
source_id: "yc-clearly-ai-news-import-066e8c806377"
canonical_url: "https://clearly-ai.com/blog/how-ai-is-rewiring-our-product-engineering-workflow"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-23T05:49:00.156409+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:76d64833f80c903fc3a738c3d989058dad5e98375998ef47af6f88dcdcbfe0ce"
---

# How AI Is rewiring our product engineering workflow

We expected AI to help engineers write code faster. It did.


But after months of experimenting with coding assistants, agents, and internal automation, we noticed something unexpected: the highest-leverage workflows were not in implementation. They were in the gaps around it, like triage, on-call incident investigation, release prep, QA evidence collection, PR review, sprint assessment, and runbook maintenance.


The bottleneck was rarely writing code itself but instead about reconstructing context quickly enough for humans to make good decisions: What changed? Does it matter? Which systems are affected? Where is the risk boundary? How do we verify the result?


At Clearly AI, our product helps enterprise security and privacy teams turn scattered context from code, docs, tickets, and policies into structured, review-ready assessments. Internally, our engineering workflows started to look surprisingly similar. The context was scattered. The output needed to be structured enough for a human to trust.


The early assumption was:


> Agents helps engineers implement faster.


That turned out to be true, but incomplete. The more valuable pattern was this:


> Agents work best when embedded into bounded engineering workflows with durable context, observable outputs, and explicit human review.


The biggest gains came from compressing the operational loops around software delivery. Not autonomous coding. Not replacing engineers. Workflow compression.


## Engineering work moved up the stack


The clearest illustration of this came while building our Risk Registry feature.


We spent weeks iterating on the PRFAQ, RFCs, technical architecture reviews, schema design, and rollout strategy. There were multiple rounds of discussion around data model ownership, service boundaries, and where the event-driven orchestration pattern should reside.


Once those specifications were finalized, implementation and deployment to our beta environment happened within hours.


That ratio of weeks spent on design and hours spent on implementation fundamentally changes how engineering work feels.


The expensive part is no longer translating decisions into code. The expensive part is making decisions precise enough that they can safely become code.


The upfront work kept paying off later. When a new ticket asked to link or deduplicate risks across platform and app reviews, the Clearly bot traced it back to the Risk Registry RFC and in-flight MVP, recognized that the schema already supported the relationship, and flagged the request as part of an open deduplication discussion rather than a standalone feature.


An RFC was no longer just a planning artifact. It became durable context: something agents could use later to triage tickets, connect related work, and keep future implementation aligned.


Design reviews carry more weight because their output can now turn into shipped code almost immediately. Vague RFCs become operationally expensive in ways they were not before, because the cost of a misaligned spec is no longer absorbed by weeks of implementation friction. Instead, it surfaces as merged code that later has to be unwound.


Architecture, schema design, and rollout planning have become the load-bearing parts of the process.


## Code creation is escaping the IDE


Another notable shift was where implementation work starts.


Code no longer originates only from an engineer opening an IDE. Increasingly, it starts from Linear tickets, Slack threads, on-call incidents, deployment workflows and other operational touchpoints.


A tenant release ticket can now trigger the Clearly bot to gather context, update configuration files and deployment manifests, open a PR and tag the right reviewers. A developer still reviews the diff, and the normal deployment pipeline still carries the safety guarantees.


We see the same pattern in Slack. A team discusses an issue, tags the on-call agent, and receives either a well-scoped Linear ticket or a review-ready PR.


The IDE has not disappeared. But it is no longer the only interface for creating code.


That changed the reliability question. If code can originate from Slack, tickets, incidents, and deployment workflows, the system needs clear boundaries around when agents can act, what they can touch, and where they have to stop.


## Reliability came from workflow design


At first, we assumed better results would come from more capable agents.


In practice, reliability came less from model capability and more from workflow design.


The workflows that proved most reliable were usually the simplest ones. They shared the same shape: a clear trigger, a bounded tool surface, an inspectable output and an explicit stopping condition.


Our on-call workflow follows this pattern. It starts with a Slack escalation in` #clearly-eng-on-call` integrated with PagerDuty. The agent gathers CloudWatch logs, deployment status from the tenant manifest, alerts, runbooks and incident history, then builds a timeline artifact with evidence attached to each claim.


The output is not a recommendation floating in chat. It is an inspectable artifact: a timeline with source links, confidence notes and a clear handoff point.


Engineers still own diagnosis and resolution. But the time spent reconstructing operational context dropped significantly.


When confidence is high, the agent can also open a PR for review or draft customer-facing incident notes for the on-call engineer to share.


The key lesson was not that agents should do more. It was that they should do bounded work with clear inputs, inspectable outputs, and explicit stopping points. Inspectable output mattered more than autonomy.


## Human gates turned out to be critical


The workflows engineers trusted most had explicit human checkpoints: PR reviews, approval flows, escalation paths, read-only planning steps, and clear boundaries around production changes.


Agents were useful, but they could turn ambiguity into confident output too easily. Human gates kept uncertain reasoning from becoming operational state.


That mattered most in high-risk workflows: production incidents, release automation, and systems that could modify other workflows.


The lesson was simple: the goal was not maximum autonomy. The goal was reliable automation.


## What did not work


Some patterns consistently failed.


-


**Vague Tasks** :Agents generate output too easily when the task itself is unclear. Poorly-scoped work created noisy artifacts, shallow reasoning, and review fatigue.


-


**Unbounded Permissions** : Any workflow capable of mutating large amounts of state without constraints became operationally uncomfortable very quickly.


-


**Replacing Deterministic Systems** : Agentic QA helped significantly with exploratory validation, evidence collection, workflow coverage, and visual verification. But deterministic systems still carried the invariant load: unit tests, integration tests, lint, type checks, CI enforcement.


Agents worked best as a layer on top of deterministic systems, not as a replacement for them.


## What remains unsolved


Several challenges remain unresolved.


-


**Review Fatigue** : As virtual teammates scale, they can produce artifacts continuously and operate around the clock. Because human gates still need to review their outputs, the volume of review work can increase quickly. An engineer might wake up in the morning and have to review the 2 AM PR opened by a virtual colleague in response to an issue detected overnight


-


**Measurement** : Counting generated artifacts is easy. Measuring whether the workflow improved engineering outcomes is harder. The next maturity step is moving from activity metrics. PRs opened, tickets created, summaries generated to outcome metrics: acceptance rate, rollback rate, review latency, MTTR impact, override frequency, and workflow reliability


## Closing thoughts


The most useful agentic systems we built were the ones embedded into clear operational loops: bounded workflows, observable artifacts and explicit human review.


AI did help us write code faster, but the larger impact showed up around the work surrounding code: triage, planning, review, QA evidence, incident investigation and release readiness.


As implementation gets faster, the quality of the surrounding workflow matters more. Strong context, clear ownership, and well-defined approval paths become the difference between useful automation and noisy output.


In Part 2, we'll go deep on the workflows behind it. By the way - if this is the kind of engineering you want to do,[we're hiring](https://www.workatastartup.com/companies/clearly-ai) .
