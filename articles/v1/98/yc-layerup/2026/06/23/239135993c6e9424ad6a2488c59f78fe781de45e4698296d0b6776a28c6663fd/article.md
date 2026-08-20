---
schema_version: "1.0.0"
document_id: "239135993c6e9424ad6a2488c59f78fe781de45e4698296d0b6776a28c6663fd"
company_key: "yc-layerup"
company: "Layerup"
source_id: "yc-layerup-news-import-60fa3a01cf26"
canonical_url: "https://uselayerup.com/blog/purpose-built-agents-for-underwriting-and-claims-lines"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-24T02:01:56.334076+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:84861415f365945a05a36b6f7efec7aae4d3c349e11a2f738ce0c82eecb37d01"
---

# Purpose-built, not general-purpose: why Layerup ships a different agent for every underwriting and claims line

[Back to blog](https://www.uselayerup.com/blog) Platform & engineering · CIO / CTO


# Purpose-built, not general-purpose: why Layerup ships a different agent for every underwriting and claims line


An agent that underwrites IDI is not an agent that underwrites life, and neither is a general-purpose model with a long prompt. The performance gap is architectural, and it shows up in every line-specific detail.


Layerup


•


June 12, 2026


•


13 min read


Decision accuracy vs. general-purpose AI


Architectural, not incremental


There is a pattern in enterprise AI evaluations that every insurance CIO has now seen at least once. A general-purpose model is pointed at a stack of underwriting files or claims documents, it produces fluent, plausible summaries and recommendations in the demo, and the pilot kicks off with high expectations. Three months later the same model is misreading occupation classes on disability applications, hallucinating endorsement forms that do not exist, and proposing benefit amounts that fail arithmetic. The demo was real. So is the failure.


The explanation is not that the model is bad. It is that underwriting and claims are not one task. Individual disability income underwriting, life underwriting, commercial property underwriting, IDI claims adjudication, life claims adjudication, and auto physical-damage claims are different tasks with different documents, different vocabularies, different decision rules, and different failure modes. Layerup ships a purpose-built agent for each — and the difference between that and a general-purpose model with a long prompt is architectural, not cosmetic.


## Why general-purpose AI underperforms on specialized lines


A general-purpose model carries broad distributional knowledge and no line-specific structure. The failures that follow are predictable and worth naming precisely, because each one maps to a specific architectural countermeasure.


- Vocabulary collision. The same words mean different things across lines. 'Total disability' under an own-occupation IDI policy is a different legal test than under an any-occupation group LTD policy; 'replacement' means one thing in life insurance compliance and another in property claims. A general model resolves these ambiguities by frequency in its training data, which is precisely wrong for the less common line.
- Document-layout blindness. An attending physician statement, a paramedical exam report, an ACORD form, a loss run, and an estimate-of-record each have a structure that carries meaning — which box a value appears in changes what the value means. Generic extraction treats them as undifferentiated text and silently swaps fields that look alike.
- Numeric reasoning under policy constraints. Issue and participation limits in IDI, benefit-to-income ratios, table ratings in life, coinsurance penalties in property — these are deterministic computations with line-specific rules. Language models approximate arithmetic; they do not enforce it. A plausible-looking number that is wrong by one rating table is worse than no number.
- Uncalibrated confidence. A general model is equally fluent when it is right and when it is guessing. Without per-task calibration there is no defensible basis for deciding which files can flow through without human review — so either everything gets reviewed (no leverage) or errors flow through silently (unacceptable).
- No encoding of appetite and guidelines. Your underwriting manual, reinsurance treaty obligations, and claims-handling guidelines are not in any foundation model's training data. Prompting summarizes them; it does not enforce them.


## The anatomy of a purpose-built agent


Every Layerup agent is assembled from the same architectural components, but each component is specialized to the line and function it serves. This is what 'purpose-built' means concretely.


1. A line-specific ontology. Each agent reasons over typed entities for its line — for IDI underwriting that includes occupation class, earned versus unearned income, elimination period, benefit period, and rider structure; for life claims it includes contestability status, beneficiary designation chain, and cause-of-death classification. Extraction targets these types, not free text, so a value cannot land in the wrong slot without failing a type check.
2. Line-specific document models. Extraction is tuned per document family — APS narratives, paramed exams, Rx histories and MIB codes for life and IDI; loss runs, ACORD applications, and SOVs for commercial; estimates, EOBs, and itemized bills for claims. The model knows which page regions, tables, and code systems (ICD-10, CPT, NAICS) carry signal in each.
3. Deterministic computation via tools. Benefit calculations, issue and participation limits, ratings, offsets, and premium math are executed by registered tools with typed contracts — never left to the language model. The agent decides which computation applies; the tool computes it; the result is exact and auditable.
4. Compiled guidelines and appetite. Underwriting manuals and claims-handling guidelines are encoded as evaluable rules that gate the agent's outputs. An IDI underwriting agent cannot propose an issue amount that violates participation limits, because the rule fires before the recommendation is staged — structurally, not statistically.
5. Per-line evaluation sets and calibration. Each agent ships against a golden set built from that line's real decisions, and its confidence scores are calibrated per workflow. Touchless thresholds are set where calibrated precision supports them — separately for IDI claims and life claims, because the error costs differ.
6. Line-specific feedback loops. Corrections from your underwriters and examiners feed back into the agent for that line. An adjuster's correction on a residual-disability earnings calculation improves the IDI claims agent; it never pollutes the life claims agent with irrelevant signal.


## What this looks like in underwriting: IDI versus life


Consider the two lines that look most similar from a distance — individual disability income and life underwriting. Both involve medical evidence, financial evidence, and a risk classification decision. Underneath, almost every detail diverges, and each divergence is something the purpose-built agent encodes and the general model misses.


The IDI underwriting agent is built around occupation and income. It maps stated occupations to the carrier's occupation-class table — distinguishing a surgeon from a general practitioner from a medical administrator, because class drives both price and the own-occupation definition the applicant can buy. It performs financial underwriting: parsing tax returns and W-2s to separate earned from unearned income, computing benefit-to-income ratios, and enforcing issue and participation limits across in-force coverage from every carrier disclosed. It reads APS records for musculoskeletal and mental-nervous history specifically, because those drive IDI morbidity, and it knows that the same back-pain mention carries different weight for a roofer than for an accountant.


The life underwriting agent is built around mortality. It reads labs against age-and-sex-banded thresholds, applies build charts, reconciles Rx histories and MIB codes against the application's disclosures, and flags the discrepancies that matter for mortality — not morbidity. It computes preferred-class eligibility against the carrier's published criteria and proposes table ratings with the actuarial basis attached. Family history, tobacco evidence, and aviation or avocation questionnaires route through dedicated evidence paths that an IDI agent simply does not have, because they are not IDI questions.


IDI UW agent optimizes for


Morbidity & occupation


Life UW agent optimizes for


Mortality & build


Shared architecture


Same harness


Shared weights & rules


None


A general-purpose model asked to underwrite both lines collapses these distinctions into one undifferentiated notion of 'risk.' It will read the same APS the same way for both applications, miss that participation limits are an IDI concept entirely, and produce recommendations that sound like underwriting without being underwriting.


## What this looks like in claims: IDI versus life


The same divergence holds on the claims side, where the cost of a wrong decision is paid in leakage, litigation, and regulatory findings.


The IDI claims agent adjudicates an ongoing relationship, not an event. It evaluates the claimed disability against the policy's specific definition — own-occupation, modified own-occ, or any-occupation, including transition points where the definition changes after a defined period. It distinguishes total from residual disability, which requires computing pre-disability earnings from tax documents and comparing them to current earnings month over month. It applies elimination periods, COLA riders, and offsets for social insurance benefits where the policy integrates with them, and it schedules ongoing proof-of-loss reviews at intervals appropriate to the diagnosis and occupation. Every one of those steps is a typed computation with policy-form-specific rules.


The life claims agent adjudicates an event with legal finality. It verifies the death certificate against the insured's identity, checks whether the claim falls inside the contestability window and stages the material-misrepresentation review if it does, walks the beneficiary designation chain — including lapsed designations, divorce-revocation statutes that vary by state, and potential interpleader situations when claimants conflict — and validates policy status against lapse and reinstatement history before payment is staged. None of this resembles IDI claims work, and an agent tuned for one will be structurally wrong on the other.


IDI claims agent reasons over


Definitions & earnings


Life claims agent reasons over


Contestability & beneficiaries


Decision cadence


Monthly vs. once


Error cost


Leakage vs. litigation


## Where the measured performance gap comes from


The performance advantage of purpose-built agents is not a single large effect. It is the compounding of several specific ones, each attributable to an architectural choice.


- Error decomposition stops compounding. In a general pipeline, an extraction error becomes a reasoning error becomes a decision error. Typed extraction against a line ontology catches the first error at the type boundary — a benefit period extracted into an elimination-period field fails validation instead of flowing downstream.
- Arithmetic is exact by construction. Because benefit math, limits, ratings, and offsets run through deterministic tools, the entire class of numeric hallucination is removed rather than reduced. On lines like IDI where the monthly benefit calculation is the claim, this single choice dominates.
- Calibration enables touchless throughput. Per-workflow calibrated confidence means the agent knows which files it can clear and which it must route. Illustratively, a purpose-built agent operating at a calibrated high-precision threshold can clear a substantial share of routine files touchlessly, where an uncalibrated general model must either route everything to review or accept silent error rates no claims organization would sign off on.
- Evaluation sets keep improvement honest. Every change to a line agent is scored against that line's golden set before promotion. A general-purpose deployment has no equivalent harness — changes to the prompt change behavior everywhere at once, unmeasured.
- Feedback stays in-domain. Specialist corrections sharpen the specialist agent. The IDI examiner's overrides tune residual-earnings handling; the life examiner's overrides tune contestability review. Signal is never averaged across domains where it does not transfer.


## One platform, many specialists


Purpose-built does not mean fragmented. Every line agent runs on the same orchestration substrate — the same ingestion, ontology framework, tool registry, model gateway, approval queues, and audit lineage. What changes per agent is the content: the ontology types, the document models, the compiled guidelines, the tools, the evaluation sets, and the learned weights. The governance is uniform; the expertise is specialized.


This is also why the platform compounds. The harness that makes the IDI underwriting agent improve from your underwriters' corrections is the same harness that makes the life claims agent improve from your examiners' corrections. Adding a line means instantiating the architecture with new domain content — not building a new system, and not stretching a general-purpose model across one more domain it was never built for.


## The headline


Underwriting and claims are not one task, so they should not be one model. Layerup ships a purpose-built agent for each line and each function — IDI underwriting, life underwriting, IDI claims, life claims, and onward across commercial, property, auto, and specialty — each with its own ontology, document models, deterministic tools, compiled guidelines, calibrated confidence, and evaluation set, all governed by one orchestration layer. General-purpose AI optimizes for sounding right across every domain. Purpose-built agents optimize for being right in yours.


Tags


AI agents


Underwriting


Claims


IDI


Life


Architecture


Authored by


Layerup


The agentic AI operating system for insurance. We deploy AI agents inside the systems carriers, MGAs, MGUs, TPAs, and health plans already run.


[Book a demo](https://www.uselayerup.com/contact) •


[Explore the platform](https://www.uselayerup.com/platform)


—


Related


## Keep reading.


More pieces from the same category, or the same audience.


[Platform & engineering Deploying AI agents inside modern policy admin and claims cores without ripping them out Agents do not need a core replacement. They need a write-back contract, a sandbox path, an evals harness, and an audit trail. Here is the architecture pattern that survives a model risk review. January 16, 2026 11 min read](https://www.uselayerup.com/blog/deploying-ai-agents-inside-policy-admin-and-claims-cores)


[Claims Agents that compound: how Layerup's AI improves the more your enterprise uses it The first agent you deploy is the worst agent you will ever run. This is the engineering behind why Layerup's agents get measurably better on your data — and what that looks like on core claims metrics. June 4, 2026 11 min read](https://www.uselayerup.com/blog/how-ai-agents-improve-as-enterprises-use-them)


Get started


## Move from reading to deploying.


Pick one workflow inside one line of business. Talk to us about where the highest-leverage starting point is in your operation.


[Book a demo](https://www.uselayerup.com/contact)[All posts](https://www.uselayerup.com/blog)
