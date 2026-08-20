---
schema_version: "1.0.0"
document_id: "0f8843e174a836cfe20fbbda18dd8262493be1b0e95d95e1b5b879b732ce74a4"
company_key: "yc-rivia-ai"
company: "Rivia.AI"
source_id: "yc-rivia-ai-news-import-430ff3cc3a09"
canonical_url: "https://rivia.com/news/the-fda-wants-real-time-clinical-trials"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-27T06:24:14.659940+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:87f9992cebf65bf62856511c223346c5104627520ed67bf10748f4f88533b76b"
---

# The FDA Wants Real-Time Clinical Trials. Your Data Infrastructure Decides If You're Ready.

*The Real-Time Clinical Trials initiative raises the bar on what sponsors must demonstrate. The readiness gap is wider than most teams expect.*


‍


For decades, the rhythm of clinical development has been built around a familiar sequence: collect data, clean data, review data, submit data. The FDA reviewed evidence after a study was complete. Regulatory oversight and trial execution occupied different timelines, and sponsors had the space between them to reconstruct, reconcile, and explain.


‍


Through its **Real-Time Clinical Trials (RTCT) initiative** , the FDA is exploring a model in which safety signals, endpoints, and operational data are reviewed simultaneously by sponsors and the agency while oncology studies are still running. The April 2026 Request for Information on AI-enabled optimization of early-phase clinical trials makes the direction explicit: the FDA is asking how predefined signals and metadata can support near real-time analysis and shared decision-making between sponsors and regulators, during dose escalation, during safety reviews, and at the go/no-go decision points that determine whether a program advances.


#### Start With the Data Layer Underneath AI


The Request for Information asks, reasonably, how **AI-enabled technologies** can improve efficiency, speed, and quality of decision-making in early-phase trials. It is a legitimate question. AI has genuine near-term application in dose escalation optimization, safety signal detection, adaptive trial design, and biomarker-based patient stratification.


‍


There is a prior question that the focus on AI tends to obscure: what does the data layer underneath those AI models actually look like?


‍


In most early-phase trials today, the answer is fragmented. Clinical data is captured across systems that were designed independently of each other:


- EDC platforms
- Laboratory information systems
- Imaging archives
- Safety databases
- Concomitant medication logs


‍


A safety physician trying to assess whether a patient meets Hy's Law criteria needs to draw on at least three SDTM domains simultaneously. No single EDC form contains all the needed domains. No automated edit check fires across domain boundaries. The answer arrives via a cross-domain listing: a flat file, generated in SAS or Excel, refreshed weekly, emailed to the reviewing physician.


‍


That is the standard operating model today.


‍


AI models deployed into that environment inherit its constraints. Fragmented data produces fragmented signal detection, regardless of how sophisticated the model sitting on top of it is. The foundational work, getting data integrated, current, and queryable across domains at the point of review, is the prerequisite.


#### Three Infrastructure Requirements for Continuous Oversight


The Real-Time Clinical Trials initiative is an infrastructure question with a regulatory timeline attached to it.


‍


Real-time co-review between sponsors and the FDA requires that sponsors produce a continuous, auditable, cross-domain view of their trial data. When a regulator asks why a safety signal was reviewed the way it was, the answer should come from the system: timestamped, user-attributed, reproducible against the data snapshot that existed at the moment of review.


‍


ICH E6(R3), the current Good Clinical Practice guideline, expects evidence of quality oversight throughout the trial, with the emphasis on contemporaneous documentation rather than a clean final dataset. The Real-Time Clinical Trials initiative raises that bar further: sponsors will need to demonstrate that their oversight was continuous and that the evidence for it is already in the system.


‍


Three infrastructure requirements follow directly from this:


- **Cross-domain integration at the point of review.** Safety signal detection across multiple data domains depends on data from disparate systems being available in a unified, queryable form when a reviewer needs it. Weekly listing refreshes produce a series of snapshots with gaps between them. Those gaps are where signals accumulate undetected.
- **State and lifecycle tracking at the finding level.** A flat listing captures what the data shows at the moment of generation. It does not capture what a reviewer did with it, what the data showed when they reviewed it, or how a signal resolved when the underlying data was subsequently corrected. Continuous oversight requires each finding to carry a lifecycle — unreviewed, in review, closed — with recorded reasons for each state transition, so that the review record is as durable as the data record itself.
- **Audit infrastructure built into the system.** When a regulator asks who reviewed a safety signal, when, and what data they were looking at, that answer should come directly from the system. Reconstructing it from email threads, spreadsheet logs, and the memory of the clinical team is a different standard — and an increasingly inadequate one as regulatory co-review moves closer to trial execution.


‍


These are data infrastructure requirements. They are the precondition for AI to function as intended in an Real-Time Clinical Trials context.


#### Why the Current Model Reached Its Limits


The fragmentation of clinical trial data is the accumulated result of rational decisions made at each step of how trials have historically been built.


‍


EDC platforms were designed to capture site-entered data reliably. Laboratory systems were designed for laboratory workflows. Safety databases were designed for pharmacovigilance. Each was built to do its job well within its own domain. Real-time integration at the point of clinical review was simply outside the original design brief for most of them.


‍


Integration has therefore been handled at the output layer. Cross-domain listings are generated by SAS programmers who write code to join the relevant domains, apply the relevant logic, and produce a file the clinical reviewer can work with. That file is emailed. The reviewer triages it, often in a side spreadsheet. The following week, a new file arrives. Whether a finding from last week's listing is the same finding on this week's listing is answered by the reviewer's memory.


‍


The solution to meeting the Real-Time Clinical Trials standard is an architectural one. The programming layer that currently generates cross-domain listings needs to become a continuous evaluation layer: cross-domain rules defined once, applied automatically against every incoming data snapshot, producing findings with persistent state and full audit history. Refreshing the listings more frequently produces faster snapshots. It is still a flat file. It still carries no state, supports no concurrent reviewer workflow, and produces no audit trail a regulator can inspect directly.


‍


The architecture needed for real-time oversight is continuous by design, not periodic by convention.


#### Dose Escalation Is the Right Place to Start


The FDA's Real-Time Clinical Trials proof-of-concept work, including the TRAVERSE and STREAM-SCLC studies, has focused on oncology dose-escalation trials. That choice reflects where the regulatory stakes are highest: decisions made during dose escalation directly affect patient safety, and the data required to make them spans multiple sources that are rarely integrated at the point of review.


‍


Dose-escalation decisions depend on integrated signals: adverse event data, laboratory values, imaging, concomitant medications, prior medical history. A dose-limiting toxicity determination requires synthesizing information from multiple sources, often across multiple sites, within a decision window measured in days. A signal that enters the EDC on Monday may not reach the reviewing safety physician until the following listing refresh. If that signal is a serious adverse event in a cycle where no protocol action was recorded, the window for appropriate clinical response may have already closed.


‍


The same applies to any cross-domain safety standard that requires synthesizing data from laboratory values, medical history, and concomitant medications simultaneously. Those assessments depend on all relevant data being available and current at the moment of review. Under the Real-Time Clinical Trials model, the difference between continuous monitoring and weekly listing cycles will be visible to regulators in real time.


‍


Oncology dose escalation is also the right place to start because the lessons generalize. The integration requirements, the state tracking requirements, the auditability requirements — all of them apply, with varying degrees of complexity, across trial types and therapeutic areas. Demonstrating the model in the most demanding environment first produces findings with the broadest applicability.


#### Measuring What Actually Improved


The FDA's RFI asks how improvements from an Real-Time Clinical Trials pilot should be evaluated. The answer depends on what the pilot is designed to test.


‍


A pilot designed to evaluate AI model performance in isolation (accuracy rates, model drift, generalizability across patient populations) produces findings relevant to model selection. Those findings matter, but they are limited in their applicability to the sponsors running the majority of early-phase trials today, most of whom are operating on the standard model: fragmented systems, periodic listings, manual reconciliation.


‍


A pilot designed to measure the full value chain captures something more useful. Time from data entry to signal detection, not just model accuracy. Time from signal detection to clinical action, not just detection rates. Audit trail completeness across AI-supported and non-AI-supported trials. Whether the evidence produced under each model would satisfy a regulatory inspection on its own terms.


‍


These metrics map directly to the questions regulators are already asking under ICH E6(R3): was oversight continuous, was it documented contemporaneously, and can it be demonstrated at the level of the individual finding?


‍


Treating the Real-Time Clinical Trials pilot as a process improvement evaluation (measuring what improved, by how much, and why) produces findings that generalize across sponsor sizes and therapeutic areas. That framing also makes it possible to attribute improvements to specific interventions, which is the prerequisite for drawing conclusions that can inform subsequent waves of the pilot.


#### The Infrastructure Question Sponsors Need to Answer Now


The Real-Time Clinical Trials conversation that focuses on AI capability (better models, smarter algorithms, more sophisticated adaptive designs) is worth having. It will produce real improvements in clinical decision-making over time.


‍


The more immediate question for most Phase 1 sponsors is whether the data infrastructure supporting their trials can produce a continuous, cross-domain, auditable view of what is happening while the study is running.


‍


The FDA has been explicit about this direction. ICH E6(R3) has been explicit about this expectation. Sponsors building or evaluating their data infrastructure now have a meaningful window to close the gap before real-time regulatory co-review becomes the standard rather than the pilot.


‍


**Your trial data is continuously growing. Do you have real-time access to it?**


‍


***References***


1. [U.S. Food and Drug Administration. FDA Announces Major Steps to Implement Real-Time Clinical Trials. Press release, April 28, 2026](https://www.fda.gov/news-events/press-announcements/fda-announces-major-steps-implement-real-time-clinical-trials) .
2. [U.S. Food and Drug Administration. AI-Enabled Optimization of Early-Phase Clinical Trials Pilot Program; Request for Information. Federal Register, Vol. 91, April 29, 2026. Docket No. FDA-2026-N-4390](https://www.federalregister.gov/documents/2026/04/29/2026-08281/ai-enabled-optimization-of-early-phase-clinical-trials-pilot-program-request-for-information) .
3. [International Council for Harmonisation. ICH E6(R3) Guideline for Good Clinical Practice. Final version adopted January 6, 2025; came into effect July 23, 2025.](https://database.ich.org/sites/default/files/ICH_E6(R3)_Step4_FinalGuideline_2025_0106.pdf)


‍
