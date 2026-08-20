---
schema_version: "1.0.0"
document_id: "782d5109f3699d7a4d32b8f9af2df709dab4df13546cf88cff15b630dfdb486c"
company_key: "yc-wolfia"
company: "Wolfia"
source_id: "yc-wolfia-news-import-63b7007a854b"
canonical_url: "https://wolfia.com/blog/when-a-security-questionnaire-asks-for-evidence-you-lack"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-22T23:41:27.201440+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:a81e7a91655ba2df0e68ff63c3393110c575e3e525b4d074377d8a92b648df96"
---

# When a security questionnaire asks for evidence you lack

## TL;DR


- Buyers routinely ask for artifacts you have not built yet: pen test reports, architecture diagrams, DPAs, incident response plans.
- Leaving the field blank or writing "N/A" reads as evasion and slows the deal down more than an honest gap admission does.
- The fastest fix is a short, specific statement of current state plus a compensating control or a realistic build timeline.
- Track which evidence requests recur across questionnaires so you build the highest-frequency artifacts before the next deal asks for them, not after.
- Wolfia's knowledge hub flags evidence gaps against incoming questionnaires proactively, so GRC teams know what to build before a deal stalls on a missing artifact.


## Why buyers ask for evidence you do not have yet


Enterprise buyers standardize their vendor risk questionnaires around a checklist that assumes a mature vendor: a completed penetration test, a current architecture diagram, a signed data processing agreement, a documented incident response plan. Early and mid-stage vendors rarely have all four on day one, and that gap is not a red flag by itself. What buyers are actually screening for is whether the vendor knows its own gaps and has a plan.


The mismatch shows up constantly in[SIG questionnaires answered without a formal GRC team](https://wolfia.com/blog/how-to-answer-a-sig-questionnaire-without-a-grc-team) , where a single reviewer is filling in fields written for a company with a dedicated security function. The questionnaire template does not know your company stage. Your answer has to communicate it.


## What "evidence" usually means in a security questionnaire


Most evidence requests fall into a short list of recurring categories, and it helps to know which one you are dealing with before you decide how to answer:


**Penetration test reports.** A third-party assessment following a methodology like the one described in[NIST's Technical Guide to Information Security Testing and Assessment](https://csrc.nist.gov/pubs/sp/800/115/final) , SP 800-115. Buyers want the executive summary and remediation status, not the raw findings.


**Architecture diagrams.** A visual of data flow, network segmentation, and trust boundaries. This is the artifact most vendors underestimate. It takes an afternoon to produce and buyers ask for it in nearly every enterprise cycle.


**Data processing agreements.** Required by contract, not just best practice, once you process EU personal data on a customer's behalf under[GDPR Article 28](https://gdpr-info.eu/art-28-gdpr/) . If you sell into Europe and do not have a DPA template ready, this field will stall a deal on its own.


**SOC 2 reports.** An attestation against the AICPA's trust services criteria. Vendors without one can still pass most questionnaires by documenting the underlying controls individually.


**Incident response plans.** A written runbook for detection, containment, and notification. Buyers check whether it exists and whether it has been tested, not whether it is exhaustive.


## What happens if you leave the field blank


A blank field or an unexplained "N/A" is the single most common cause of a second round of buyer follow-up. Reviewers cannot tell the difference between "we do not have this and have not thought about it" and "we do not have this yet but know exactly why." Silence reads as the first one every time.


We see this pattern constantly in the questionnaires our team processes: a vendor with a genuinely solid security posture loses a week of deal velocity because one evidence field went unanswered instead of getting a two-sentence explanation. The follow-up email costs more time than writing the honest answer would have.


## How do you answer a questionnaire without the proof?


State the current status directly, then name the compensating control or the concrete timeline. A pattern that works across most evidence categories: "We have not completed a third-party penetration test as of \[date\]. Internal code review and automated dependency scanning run on every release, and a third-party assessment is scheduled for \[quarter\]." That is three sentences, no hedging, and it gives the reviewer something concrete to file.


Avoid two failure modes. The first is silence, covered above. The second is overselling, claiming a control exists in some partial or aspirational form. Reviewers cross-reference answers against later stages of due diligence, and a claimed control that does not hold up under a follow-up call does more damage than the original gap would have.[Inaccurate vendor security questionnaire answers](https://wolfia.com/blog/inaccurate-vendor-security-questionnaire-answers) are one of the fastest ways to lose buyer trust mid-cycle.


## How do you decide which artifacts to build first?


Build the artifact that shows up most often across your last several questionnaires, not the one the current deal happens to be asking for. Most vendors discover, once they look, that the same three or four gaps recur: a DPA, an architecture diagram, a written incident response plan, and a pen test summary. Building those once, ahead of the next request, removes the gap from every future cycle instead of one.


This is where a lot of teams lose time they do not need to lose. Without a system tracking which fields keep getting flagged, the same gap gets rediscovered and re-explained deal after deal.[Building a questionnaire knowledge base that maintains itself](https://wolfia.com/blog/build-a-questionnaire-knowledge-base-that-maintains-itself) is the underlying fix: every answer, including the honest gap admissions, becomes reusable instead of reinvented.


## Building the artifact vs explaining a compensating control


Not every gap needs to become a build project immediately. An architecture diagram is cheap to produce and worth doing on the first request. A SOC 2 report is a multi-month, multi-thousand-dollar undertaking that most early-stage vendors should not rush into just to satisfy one questionnaire field. The decision comes down to frequency and cost.


If an artifact shows up in the majority of questionnaires you receive and costs a day or two to produce, build it now. If it shows up occasionally and costs months, a well-written compensating-control answer is the right move until the volume of deals justifies the investment.[The anatomy of a perfect security questionnaire answer](https://wolfia.com/blog/the-anatomy-of-a-perfect-security-questionnaire-answer) covers what makes a compensating-control answer specific enough to satisfy a reviewer instead of triggering escalation.


## How Wolfia flags evidence gaps before they stall a deal


Wolfia's knowledge hub sits underneath every questionnaire a team answers and tracks which fields keep coming back unanswered or answered with a gap admission. Instead of discovering a missing DPA mid-negotiation, GRC teams see the pattern surface after two or three questionnaires flag the same request, with time to build the artifact before the next enterprise deal depends on it.


A few specific pieces of how this works in practice:


The knowledge management dashboard shows which source documents are stale or missing entirely, not just which questions were answered. Source citations on every generated answer mean a reviewer can trace a compensating-control claim back to the actual policy document instead of taking the sentence at face value. When an evidence gap does need a human sign-off, answers auto-route to the right reviewer, legal or compliance, before the questionnaire goes back to the buyer, so a gap admission gets checked rather than shipped on autopilot. Wolfia is built for security and GRC teams handling this exact workflow: recurring evidence requests across a growing questionnaire volume, without a dedicated analyst to track which gaps are closing and which keep resurfacing.


## Final Thoughts


A missing pen test report or DPA is not usually the thing that kills a deal. An unexplained blank field is. The fix is not building every artifact a buyer could theoretically ask for before you send your first questionnaire back. It is answering honestly about what exists today, naming the compensating control or the timeline, and tracking which gaps recur so you close the highest-frequency ones before the next enterprise buyer finds them first.
