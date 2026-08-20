---
schema_version: "1.0.0"
document_id: "0e18170ec8338069687d144b041cfe9ae42781f98cae54bf5674c863b198ba07"
company_key: "yc-layerup"
company: "Layerup"
source_id: "yc-layerup-news-import-60fa3a01cf26"
canonical_url: "https://uselayerup.com/blog/deploying-ai-agents-inside-policy-admin-and-claims-cores"
published_at: "2026-01-16T00:00:00+00:00"
first_seen_at: "2026-07-24T02:01:56.334076+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:00e0a0eba805728f97fbe9981c3dd720d6cb28eb3ea4bfbbfc3c74ded597ac86"
---

# Deploying AI agents inside modern policy admin and claims cores without ripping them out

[Back to blog](https://www.uselayerup.com/blog) Platform & engineering · CIO / CTO


# Deploying AI agents inside modern policy admin and claims cores without ripping them out


A practical architecture and governance brief for CIOs and CTOs on running agents against core PAS and claims systems in production.


Layerup


•


January 16, 2026


•


11 min read


Core replacement required


No


Every CIO conversation about AI agents in insurance starts in the same place: are we going to have to rip out the core. The answer is no. The deeper answer is that the core is the asset the agent makes more valuable, because the core is where the system of record lives and the agent's job is to write back into it cleanly and auditably.


This piece is the architecture and governance brief we wish more CIOs had on day one of the conversation. It is biased toward how we deploy with Layerup, but the patterns generalize.


## The architecture pattern in one paragraph


Agents run as a discrete service layer alongside the core. They read from the core through stable APIs, integration tables, or document handoffs. They write back through the same channels the carrier already trusts for system updates. They never have a write path that bypasses the carrier's existing change management, security, or audit controls. That is the entire architectural claim. Everything else is implementation detail.


Core change required


None


Net-new write paths


Zero


Audit trail surface


Existing + agent log


Identity model


Service principal


## Modern cores and the integration patterns that survive production


Modern policy admin and claims cores — across the major vendor families and across mature in-house builds — expose enough integration surface to support agents in production. The implementation details vary; the structural patterns are the same.


- Cloud-native cores expose REST APIs, integration gateways, and document management hooks. Activities, notes, and structured fields are the typical write-back surfaces.
- Platforms with integration data hubs expose service APIs as the natural seam; the agent writes events that follow the same downstream paths as any other integrated event.
- Multi-line platforms (P&C, L&AH, group) expose standardized integration surfaces with document and policy lifecycle events as first-class write targets.
- Mature in-house cores typically expose internal integration buses or messaging surfaces; the agent participates as another producer or consumer, with no special access path.


Regardless of vendor, the deployment looks the same from a control perspective. The agent has a service-principal identity, scoped permissions, and the same logging surface as any other integrated system. The carrier's security and IT operations teams treat the agent the way they treat other integrated systems, because that is what it is.


## The write-back contract is the most important design artifact


If we had to recommend a single discipline early in a deployment, it would be writing down the write-back contract before the first end-to-end use case ships. The contract specifies what the agent is allowed to write, on which entities, under which conditions, with what audit metadata.


1. Entity coverage. Which entities the agent writes to — claim, activity, document, note, exposure, reserve recommendation, policy transaction.
2. Field coverage. Which fields on each entity the agent can populate. Some fields are agent-eligible. Some are human-only.
3. Condition coverage. When the agent is allowed to write — auto on confidence threshold, human-approved, never auto.
4. Audit coverage. What metadata accompanies every write — agent identity, input citations, confidence, human approver if any.


## Sandbox to production path


The deployment path that consistently works is a four-stage path. The temptation to skip stages always exists. The carriers that skip stages always pay for it later.


1. Read-only shadow. The agent reads production data and produces outputs, but writes nothing. Outputs are compared against human outputs on the same files. This is where evals get tuned.
2. Sandbox write. The agent writes to a non-production environment that mirrors production. Internal reviewers exercise the workflow end-to-end. Issues are integration, not capability.
3. Production with human approval. The agent runs in production, but every write requires human approval. Throughput is real. Risk is bounded because nothing reaches the system of record without sign-off.
4. Production with conditional auto-write. Specific outputs — those that have demonstrated high confidence and low-risk profile across the prior stage — are allowed to write without per-instance approval, with sampled QA.


Stage 4 is not the goal for every output. Some outputs stay at stage 3 forever, and that is correct. The deployment is mature when the carrier knows which outputs sit at which stage and why.


## Evals and ongoing quality


The eval harness is the second most important design artifact, after the write-back contract. Carriers ask whether the agent is right enough to deploy. The harness is what answers that, on a recurring schedule, in a way the model risk team accepts.


- Golden datasets: held-out files with known correct outputs, refreshed as the business evolves.
- Production sampling: a defined percentage of production outputs reviewed against human judgment.
- Drift detection: model output distribution monitored over time, with alerts on shifts.
- Adversarial review: deliberate testing of edge cases and known failure modes on every release.
- Reviewer disagreement capture: every override is a signal; the disagreements are mined for tuning and for surfacing inconsistency in the rules themselves.


## Governance and audit


The audit trail for an agent has to answer four questions about any output, instantly, for any reviewer: what did the agent do, why did it do it, what was the input evidence, and who approved the result. If those four questions can be answered, the audit posture is solid. If any one cannot be answered, the deployment is not ready for production.


1. Every output carries a reasoning trace: the inputs, the prompt or rule path, the model version, the confidence score.
2. Every citation is anchored to a source document or system field, by ID and by location.
3. Every approval is logged with the approver's identity and decision (confirm, override, escalate), and the timestamp.
4. Every override is captured with the override rationale and fed back into the eval set.
5. Every release of the agent — prompts, rules, models — is versioned and tied to the outputs it produced.


Build this once and the entire downstream model risk and compliance conversation gets simpler. Skip it and every conversation becomes a fresh argument.


## Security, data handling, and tenancy


The carriers we work with are uncompromising on data handling, as they should be. The defensible architecture has a few non-negotiable properties.


- Carrier data does not leave the carrier's logical boundary except through paths the carrier has explicitly approved.
- No training of foundation models or shared models on carrier data without contractual authorization.
- Tenant isolation by default. No cross-tenant data flow under any circumstances.
- Encryption in transit and at rest as table stakes; key management aligned with the carrier's existing practice.
- Logging available to the carrier's SIEM and security operations on standard formats.


## What to ask vendors before signing


1. Show me the write-back contract pattern from a deployment in my core system.
2. Show me the eval harness output for a comparable workflow, including drift detection and golden-set performance over the last quarter.
3. Show me how an override gets back into your training or rule set, and on what cadence.
4. Show me the audit trail for an output, end to end, including model version and approver identity.
5. Show me the deployment timeline for a comparable workflow at a comparable carrier, including the time spent in each of the four stages above.
6. Show me how you handle model risk management documentation and what artifacts you produce on a recurring schedule.


## What this pattern buys you


An architecture that runs agents alongside the existing core, writes back through audited paths, and produces validation artifacts on a recurring schedule survives the model risk review, the security architecture review, the audit, and the eventual regulator inquiry. It also lets the carrier ship the business outcome — cycle time, loss ratio, throughput — without taking on a core replacement program in parallel.


The carriers that get this right treat the deployment as an integration program with strong governance, not as a model program with light integration. The two framings produce very different two-year results.


> “We were ready to fight about the core. We did not need to. The integration pattern was familiar. The governance pattern was new and that is where we spent the time.”


— CIO of a top-50 P&C carrier, on the first six months


## Summary


Agents do not require a core replacement. They require a clean write-back contract, a four-stage deployment path, an eval harness that produces validation artifacts on a schedule, an audit trail that answers four questions about any output, and a security posture that meets the carrier's existing bar. None of this is novel. All of it is necessary. The carriers that build it first ship the business outcomes first.


Tags


Integration


Architecture


Governance


PAS


Claims systems


MRM


Authored by


Layerup


The agentic AI operating system for insurance. We deploy AI agents inside the systems carriers, MGAs, MGUs, TPAs, and health plans already run.


[Book a demo](https://www.uselayerup.com/contact) •


[Explore the platform](https://www.uselayerup.com/platform)


—


Related


## Keep reading.


More pieces from the same category, or the same audience.


[Platform & engineering Purpose-built, not general-purpose: why Layerup ships a different agent for every underwriting and claims line General-purpose AI demos well on any line and underperforms in production on every line. This is the technical anatomy of Layerup's purpose-built agents — per line, per function — and why specialization wins on the metrics that matter. June 12, 2026 13 min read](https://www.uselayerup.com/blog/purpose-built-agents-for-underwriting-and-claims-lines)


[Claims Agents that compound: how Layerup's AI improves the more your enterprise uses it The first agent you deploy is the worst agent you will ever run. This is the engineering behind why Layerup's agents get measurably better on your data — and what that looks like on core claims metrics. June 4, 2026 11 min read](https://www.uselayerup.com/blog/how-ai-agents-improve-as-enterprises-use-them)


Get started


## Move from reading to deploying.


Pick one workflow inside one line of business. Talk to us about where the highest-leverage starting point is in your operation.


[Book a demo](https://www.uselayerup.com/contact)[All posts](https://www.uselayerup.com/blog)
