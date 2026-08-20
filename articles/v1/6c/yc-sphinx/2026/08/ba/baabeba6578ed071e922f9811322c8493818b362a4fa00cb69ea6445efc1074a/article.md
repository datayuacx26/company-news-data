---
schema_version: "1.0.0"
document_id: "baabeba6578ed071e922f9811322c8493818b362a4fa00cb69ea6445efc1074a"
company_key: "yc-sphinx"
company: "Sphinx"
source_id: "yc-sphinx-news-import-f18a1b608f6d"
canonical_url: "https://sphinxhq.com/blog-posts/aml-model-validation-requirements"
published_at: "2026-08-18T10:00:02.148+00:00"
first_seen_at: "2026-08-18T17:34:08.644986+00:00"
fetched_at: "2026-08-18T17:34:10.060072+00:00"
content_hash: "sha256:c43c97f4d0c81a908d5002b72c0f045e12f550f889b09b199b19c7130b152228"
---

# AML Model Validation Requirements: What Examiners Expect

**TL;DR:** AML model validation is the independent assessment of whether a transaction monitoring or screening system detects what it was designed to detect, with thresholds tuned on evidence rather than assertion. On April 17, 2026, the Federal Reserve, OCC, and FDIC replaced SR 11-7 with SR 26-2, folding BSA/AML systems into a single risk-based framework while the FFIEC independent testing obligation continues on its own track. The stakes are set by cases like TD Bank, where the Justice Department found 92% of total transaction volume — roughly $18.3 trillion — went unmonitored between January 2018 and April 2024.


## What Counts as a Model, and What Doesn't


Under the SR 26-2 definition, statistical detection engines are models while purely deterministic rules engines generally are not — though both still fall under FFIEC independent testing.


AML model validation is the independent assessment of whether an automated detection system — transaction monitoring, sanctions screening, customer risk rating, alert scoring — performs as designed and can be shown to do so under examination. Validation performed by the team that built or configured the system is self-review with a cover page.


Not every AML system is a model. SR 26-2 defines a model as a complex quantitative method that applies statistical, economic, or financial theories to process input data into quantitative estimates, and it explicitly excludes deterministic rule-based processes with no such theory underpinning their design. A pure if-then rules engine that flags cash deposits above a fixed amount within a rolling window may sit outside that definition. A risk-scoring engine weighting behavioral variables into a numeric output almost certainly sits inside it. The guidance also carves out generative and agentic AI models entirely, describing them as novel and rapidly evolving.


That distinction changes which framework applies, not whether the system gets tested. An institution that classifies its monitoring platform as a non-model still owes the FFIEC independent testing obligation, and examiners will still ask whether the filtering criteria are reasonable and whether the programming behind them has been independently verified. Teams that understand[how transaction monitoring systems generate alerts](https://sphinxhq.com/blog-posts/how-does-aml-transaction-monitoring-work) — which components apply statistical inference and which simply execute rules — get the classification right.


## SR 11-7 Is Gone. The Work Isn't.


On April 17, 2026, the Federal Reserve, OCC, and FDIC jointly issued revised model risk management guidance as SR 26-2, OCC Bulletin 2026-13, and FDIC FIL-15-2026.[OCC Bulletin 2026-13](https://www.occ.gov/news-issuances/bulletins/2026/bulletin-2026-13.html) rescinds SR 11-7 and OCC Bulletin 2011-12, along with the 2021 Interagency Statement on Model Risk Management for Bank Systems Supporting BSA/AML Compliance. There is no longer a BSA/AML-specific model risk statement to point to.


What replaced it is narrower and far less prescriptive.[The revised guidance](https://www.federalreserve.gov/supervisionreg/srletters/SR2602a1.pdf) states that it does not set forth enforceable standards or prescriptive requirements, and that non-compliance will not by itself result in supervisory criticism — though supervisory action may still follow from unsafe or unsound practices. It is expected to be most relevant to banking organizations with over $30 billion in total assets. The OCC had already signaled this:[OCC Bulletin 2025-26](https://www.occ.gov/news-issuances/bulletins/2025/bulletin-2025-26.html) told community banks that the model risk guidance does not, and should not be interpreted to, require them to perform annual model validation.


Reading any of that as a reduction in workload would be a mistake. Validation frequency is now explicitly risk-based, so each institution has to justify its own cadence rather than point to industry convention. A 12-month cycle used to be the safe answer to an examiner's question. The safe answer now is a documented rationale tied to model materiality, change velocity, and data limitations.


The independent testing pillar never moved. It sits in the BSA program requirements themselves rather than in supervisory guidance, which makes it mandatory regardless of how any system is classified. Two obligations now run in parallel, and institutions[preparing for a BSA examination](https://sphinxhq.com/blog-posts/community-bank-bsa-exam-preparation) answer to both at once. The most common gap is having done one thoroughly and treated the other as covered by it.


## Above the Line, Below the Line


Threshold tuning is the part of AML model validation with no clean analogue in credit or market risk, and above-the-line/below-the-line testing is how it gets evidenced. Production thresholds draw a line through a population of transactions, and a validator's job is to establish what sits on each side of it.


Below-the-line testing lowers a threshold beneath the production setting, replays historical transactions against the looser configuration, and reviews a sample of the alerts that would have fired in the gap. It answers the under-detection question: is the current setting suppressing activity the bank should be reporting? Above-the-line testing does the reverse, tightening the parameter and sampling the alerts that would have been lost to confirm none of them were productive. One test defends against missed risk. The other defends against wasted analyst capacity. Run together, they convert a threshold change from a judgment call into an evidenced decision.


Neither term appears in SR 26-2, in SR 11-7 before it, or in the FFIEC manual. Both are practitioner conventions, and both are precisely what examiners ask to see when a bank moves a threshold. The[OCC's 2024 cease and desist order against Bank of America](https://www.occ.gov/static/enforcement-actions/eaAA-ENF-2024-56.pdf) sets the standard plainly: an appropriately documented methodology for establishing and adjusting rules, thresholds, and filters, plus ongoing periodic testing of those settings for appropriateness to the bank's customer base, products, and geographies.


Two failure modes recur in below-the-line work. The first is a sample too small to support its own conclusion: pulling thirty alerts from a band that would have generated four thousand, finding nothing productive, and declaring the threshold sound. The second is testing that never reaches the segments that matter, concentrating the sample where volume is highest while the real risk sits in a smaller population. Tuning that chases[false positive reduction](https://sphinxhq.com/blog-posts/aml-solutions-that-reduce-false-positives-what-actually-works) without a paired below-the-line test is not tuning. It is turning down the sensitivity and documenting the savings.


## What a Validation Report Has to Prove


Four things, and coverage is the one institutions most often skip. SR 26-2 organizes validation around conceptual soundness, outcomes analysis, and ongoing monitoring. AML monitoring adds a fourth dimension that framework assumes rather than tests: whether the data the model needs actually arrives.


Conceptual soundness asks whether the design holds up under scrutiny. Do the deployed scenarios map to the typologies named in the institution's own risk assessment? Was each parameter set with a documented rationale, or inherited from a vendor default and never revisited? Are the stated limitations real constraints with compensating controls attached, or boilerplate from the last report?


Data integrity and coverage asks a blunter question: does everything that should reach the model actually reach it? This is where the largest AML failures live. According to the[Justice Department](https://www.justice.gov/criminal/case/united-states-america-v-td-bank-us-holding-company) , TD Bank intentionally excluded all domestic ACH transactions, most check activity, and numerous other transaction types from its automated monitoring system, leaving 92% of total transaction volume — approximately $18.3 trillion — unmonitored from January 2018 through April 2024, and added no new monitoring scenarios from at least 2014 through late 2022. No amount of threshold tuning repairs a feed that was never connected. TD Bank pleaded guilty and paid $1.8 billion in criminal penalties, with FinCEN separately assessing a record $1.3 billion civil penalty.


Outcomes analysis compares model output against real-world results, and it is harder in AML than in credit risk because the ground truth is incomplete. A filed SAR is not a confirmed crime. An unfiled one is not confirmed innocence. The workable proxies are alert-to-case and case-to-SAR conversion measured by scenario rather than in aggregate, trended across periods instead of captured once. Ongoing monitoring decays first, because a validation is a snapshot and portfolios move underneath it. New products, new geographies, and acquisition-driven growth each shift the population the thresholds were calibrated against.


Validation Component Evidence That Satisfies an Examiner


Conceptual soundness Scenario-to-typology mapping tied to the BSA/AML risk assessment, with a documented rationale for every parameter setting


Data integrity and coverage Reconciliation of source system volumes against volumes reaching the model, plus a justified list of every excluded transaction type


Threshold tuning Paired above-the-line and below-the-line testing showing sample sizes, segment breakdowns, and the disposition of each sampled alert


Outcomes analysis Alert-to-case and case-to-SAR conversion by scenario and segment, trended across multiple periods


Ongoing monitoring Defined performance thresholds, escalation triggers, and evidence that every breach was investigated and closed


Vendor models The institution's own understanding of the vendor methodology, plus separate validation of how the product was configured and tuned locally


Vendor models deserve particular attention. SR 26-2 is explicit that model risk management principles still apply when the underlying code, data, or methodology is proprietary. Accountability does not transfer with the license. Institutions[evaluating transaction monitoring platforms](https://sphinxhq.com/blog-posts/best-transaction-monitoring-software-2026) should treat a vendor's willingness to expose scenario logic and tuning artifacts as a validation requirement, not a procurement preference. A system that cannot be explained cannot be validated.


## Where Validation Programs Actually Break


The technical work is rarely the problem. Remediation tracking without closure evidence causes most repeat findings: a validation identifies eleven issues, management agrees with all eleven, and eighteen months later six remain open with twice-revised target dates. Examiners read the pattern rather than the plan. Close behind are findings that never reach the tuning cycle, where a validator recommends segment-level thresholds, the vendor configuration does not support segmentation, and the recommendation converts into an accepted limitation nobody revisits.


Documentation written for the auditor rather than for a successor does the most damage over time. A validation report should let a new head of financial crime reconstruct why every threshold sits where it does. Most cannot. When volumes climb and nobody can explain the original rationale, teams tune by backlog pressure rather than by evidence — and those are exactly the changes an examiner will ask about.


## Where Sphinx Fits


Sphinx operates downstream of the model, at the investigation and disposition layer. Its agents review the alerts a monitoring system produces, gather supporting evidence, and record the reasoning behind every recommendation in a structured, reviewable form. That carries a direct validation benefit: outcomes analysis depends on consistent disposition data, and inconsistent analyst narratives are among the more common reasons it cannot be performed at all. Every Sphinx recommendation is logged, source-linked, and subject to analyst override, producing[an audit trail](https://sphinxhq.com/blog-posts/how-sphinx-makes-every-decision-auditable---the-interpretable-agentic-framework) a validator can sample from directly rather than reconstruct from free-text notes.


## Frequently Asked Questions


### Is AML model validation still required now that SR 11-7 has been rescinded?


Yes, though the framework changed. SR 26-2 replaced SR 11-7 on April 17, 2026, applying a risk-based approach rather than prescriptive requirements, and it is expected to be most relevant to banking organizations with over $30 billion in total assets. The FFIEC independent testing obligation sits in the BSA program requirements and remains mandatory for every bank regardless of size.


### How often should an AML model be validated?


There is no longer a prescribed interval. Under SR 26-2 the timing and scope of validation vary with model purpose, methodology, frequency of model changes, and data limitations, so each institution must document and defend its own cadence. New products, new geographies, and acquisition-driven growth remain trigger events that warrant fresh validation regardless of the calendar.


### What is the difference between model validation and BSA/AML independent testing?


Model validation assesses whether a specific quantitative system performs as designed, covering conceptual soundness, outcomes analysis, and ongoing monitoring. BSA/AML independent testing assesses the adequacy of the entire compliance program against regulatory requirements. They overlap and can inform each other, but neither substitutes for the other.


### Does a rules-based transaction monitoring system count as a model?


Possibly not, under the narrowed SR 26-2 definition, which excludes deterministic rule-based processes with no statistical, economic, or financial theory underpinning their design. Most production platforms contain both rule-based and statistical components, so the classification is rarely all-or-nothing. Either way, the decision needs written justification, and independent testing still applies.


### Who can perform an AML model validation?


Anyone with sufficient technical expertise, organizational standing, and independence from the team that developed or configured the model. SR 26-2 frames this as effective challenge and stresses that validation quality depends on the rigor of the review rather than on where reviewers sit in the org chart. Internal model risk teams, internal audit, and outside consultants all qualify.
