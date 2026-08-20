---
schema_version: "1.0.0"
document_id: "ef47672228627acd3a449d5ba7d0476c81116f8c78775100a441aa0ae1e199d9"
company_key: "yc-agency"
company: "Agency"
source_id: "yc-agency-news-import-0d31f4b059c0"
canonical_url: "https://blog.getagency.com/articles/soc-2-in-due-diligence-what-investors-check"
published_at: "2026-07-26T00:00:00+00:00"
first_seen_at: "2026-07-27T08:21:07.883449+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:1a7b1b0a8b485dacb3794efe586877b813e108249bb3d83183990aae376f9e3a"
---

# SOC 2 in Due Diligence: What Reviewers Check

*Almost every founder treats SOC 2 as a binary: you have the report or you don't. Reviewers do not read it that way. They open the PDF, and within about ten minutes they have formed a view on whether your security program is real or performed — from the report type, the dates, the opinion paragraph, and above all the exceptions table. This article is about what happens in those ten minutes, and how to make sure your report survives them.*


The short version: reviewers check six things in a predictable order, and each one has a specific failure mode you can fix before anyone looks. A current Type II report with a clean opinion, no scope gaps around your core product, and exceptions that each carry a documented remediation will clear diligence. A report with an expired observation window, a core product excluded from scope, or the same exception appearing two years running will not — not because it kills the deal, but because it changes the terms.


## The two settings, and why they behave differently


SOC 2 gets scrutinized in two situations that feel similar and are not.


**Enterprise procurement.** A security or vendor-risk team reviews you as a prospective vendor. They are gate-keeping: the question is whether you can be approved, and the consequence of failure is a stalled or lost deal. Timelines are short, the reviewer is often junior, and the review is heavily checklist-driven — which means a missing artifact can block you even when your actual security is fine.


**Investor and acquirer diligence.** A fund's diligence team, or an acquirer's technical diligence workstream, reviews you as an asset. They are pricing risk rather than gate-keeping. The consequence of a weak finding is rarely a dead deal — it is a lower number, an escrow holdback, or a remediation covenant. Reviewers here are more senior and more willing to look past a checklist item if the underlying story is coherent.


The practical implication: procurement rewards *completeness of artifacts* , and investor diligence rewards *coherence of the program* . You need both, but if you are optimizing under time pressure, know which room you are in.


## What gets requested, by stage


Stage Typically requested What is usually accepted instead


Pre-seed / Seed Security questionnaire, policy set, pen test summary Documented policies and a stated roadmap; SOC 2 rarely required


Series A SOC 2 Type I or Type II, pen test report, policy set, subprocessor list Type I plus a dated Type II plan


Series B+ SOC 2 Type II, pen test, incident history, BCDR test evidence, vendor-risk program Type II with a bridge letter if the window has lapsed


Enterprise procurement SOC 2 Type II, completed DDQ or SIG/CAIQ, pen test attestation, insurance certificates, DPA Type I only in narrow, low-data-sensitivity cases


M&A technical diligence All of the above, plus multi-year report history and exception trend Nothing — this is the least flexible reviewer


The pattern worth noticing: the request list grows monotonically, and it grows fastest in the artifacts that take longest to produce. An observation period cannot be compressed. That is the entire argument for starting earlier than feels necessary, which we cover in[SOC 2 readiness for fundraising startups](https://blog.getagency.com/articles/soc-2-readiness-for-fundraising-saas-startups) .


## The six things reviewers check, in order


### 1. Report type and scope


**What they look at:** The cover page and the system description boundary — Type I or Type II, which Trust Services Criteria are in scope, and which systems and products are covered.


**How it fails:** Security-only scope when the buyer cares about Availability or Confidentiality. Or a scope boundary that covers your primary application but excludes a newer product the buyer is actually purchasing.


**Fix it first:** Align scope to what you sell, not to what was cheapest to audit. If you have a legitimate exclusion, name it proactively and explain it. For the Type I versus Type II decision, see our[comparison](https://blog.getagency.com/articles/soc-2-type-1-vs-type-2) .


### 2. Observation period freshness


**What they look at:** The period covered, and the gap between its end date and today.


**How it fails:** The window closed months ago and there is no[bridge letter](https://blog.getagency.com/articles/soc-2-bridge-letter-explained) covering the interval. This is the most common finding in the entire category, and the cheapest to prevent.


**Fix it first:** Keep a current bridge letter on hand as a standing artifact. Do not wait to be asked.


### 3. The auditor's opinion


**What they look at:** The opinion paragraph — unqualified, qualified, adverse, or a disclaimer.


**How it fails:** A qualified opinion with no accompanying explanation or remediation plan.


**Fix it first:** A qualified opinion is survivable when it arrives with context. Prepare a one-page explanation of what was qualified, why, and what has changed since.


### 4. Exceptions in the testing results


**What they look at:** The tables at the back that most sellers hope nobody reads. Reviewers read them.


**How it fails:** Exceptions with no management response. Worse, the *same* exception across consecutive reports — the clearest available signal that a control was documented rather than fixed.


**Fix it first:** Every exception gets a management response, a remediation owner, and evidence of closure. Track exception trend across years yourself, before an acquirer does it for you.


### 5. System description alignment


**What they look at:** Whether the narrative describes the company they are buying from. Reviewers cross-check it against your architecture docs, your marketing, and in M&A, engineering interviews.


**How it fails:** A description written eighteen months ago that no longer matches the product. Newly added infrastructure or a migrated data store that the narrative does not mention.


**Fix it first:** Treat the system description as a living document reviewed each cycle, not boilerplate carried forward.


### 6. Subservice organization monitoring


**What they look at:** How you manage your own vendors — the ones your controls depend on. Increasingly the most scrutinized item on this list.


**How it fails:** No vendor inventory, no evidence you review your critical subprocessors' own SOC 2 reports, and carve-outs that shift material risk to a subservice organization you cannot demonstrate oversight of.


**Fix it first:** Maintain a real vendor inventory with risk tiering and evidence of periodic review. Our[vendor risk management guide](https://blog.getagency.com/articles/vendor-risk-management) covers the mechanics.


## Sample due diligence questionnaire items


This is the part founders most want to see in advance. Below is representative DDQ language, grouped as reviewers group it. The phrasing varies by buyer, but the substance is remarkably stable — and a current Type II report answers a large share of it by reference, which is most of the commercial value of holding one.


**Governance and program**


1. Describe your information security program, including governance structure and the individual accountable for security.
2. Do you have a current SOC 2 Type II report? Provide the report and the period covered.
3. How frequently are security policies reviewed and approved, and by whom?
4. Describe your risk assessment process and provide the date of your most recent assessment.


**Access control**


1. Describe your authentication requirements, including MFA coverage across production systems and administrative interfaces.
2. How is privileged access granted, reviewed, and revoked? Provide evidence of your most recent access review.
3. What is your offboarding process, and what is the maximum time between termination and access revocation?


**Data protection**


1. Is customer data encrypted in transit and at rest? Specify algorithms and key management.
2. Describe data segregation between tenants in your multi-tenant environment.
3. What is your data retention and secure deletion policy, and how is deletion verified?


**Secure development**


1. Describe your SDLC, including code review requirements and pre-production security testing.
2. When was your most recent third-party penetration test? Provide the report or an attestation letter.
3. How are dependencies and container images scanned for known vulnerabilities, and what are your remediation SLAs by severity?


**Incident response and resilience**


1. Describe your incident response plan and the last date it was tested.
2. Have you experienced a security incident or breach in the last 24 months? If so, describe it and the remediation.
3. What are your RTO and RPO commitments, and when did you last test recovery?


**Third parties**


1. Provide a list of subprocessors with access to customer data and the purpose of each.
2. How do you assess the security posture of critical vendors, and how often?


**Compliance and insurance**


1. Which regulatory frameworks apply to your processing, and how do you demonstrate compliance?
2. Provide evidence of cyber liability coverage, including limits.


Standardized formats exist precisely because answering this repeatedly is expensive — the SIG and CAIQ are the two you will meet most often, and our[guide to security questionnaires](https://blog.getagency.com/articles/security-questionnaires-caiq-sig-vsa) covers how they differ and how to build a reusable answer library. If questionnaire volume is already slowing your deals,[fast-passing security questionnaires](https://blog.getagency.com/articles/score-those-deals-fast-pass-security-questionnaires) is the operational fix.


## What reprices a deal rather than killing it


In investor and acquirer diligence, weak security posture usually gets *priced* rather than used as a reason to walk. The four findings that most reliably move terms:


Finding Why it moves the number


Expired observation period, no bridge letter Reads as an unmanaged program, not just a stale document


Qualified opinion with no remediation plan Signals a known problem nobody owns


Core product excluded from scope The thing being valued was never actually audited


Same exception across consecutive reports Strongest available evidence that controls are documented, not operating


Each of these is preventable at close to zero cost with a few months of lead time. That asymmetry — trivial to fix in advance, expensive to explain under diligence — is the whole reason to treat this as standing work rather than a fire drill.


## Build the standing folder before you need it


Companies that clear diligence quickly maintain a security folder the same way they maintain a financial data room: continuously, not on request. It contains the current SOC 2 report, a current bridge letter, the most recent penetration test report and attestation letter, the approved policy set with review dates, the vendor and subprocessor inventory, the most recent access review evidence, incident history, BCDR test results, and insurance certificates.


The reason this works is not organization for its own sake. It is that a reviewer who receives a complete package in one pass forms a different impression than one who extracts it over three weeks — and that impression is itself a finding.


## If diligence has already started and you do not have SOC 2


This is more common than the fundraising advice ecosystem admits, and it is workable. The sequence that holds up:


**Disclose immediately, with a date.** Discovered gaps are punished far more heavily than declared ones. A dated commitment is credible; a discovered absence is not.


**Scope and start the observation period now.** The clock is the only part you cannot accelerate, so start it before anything else.


**Assemble interim evidence.** Approved policies, a completed penetration test, and a control implementation status report demonstrate that the program exists even though the attestation does not yet. This is frequently enough for a buyer to proceed under a contractual commitment.


**Consider Type I as a milestone, not a substitute.** It shows controls are designed appropriately and gives the reviewer something dated to point to. Present it as a step toward Type II, with the Type II date named.


For what this looks like executed under real deal pressure, see our[B2B SaaS enterprise deal case study](https://blog.getagency.com/articles/b2b-saas-soc-2-enterprise-deals-case-study) , and for the regulated end of the spectrum,[what enterprise banks expect](https://blog.getagency.com/articles/what-enterprise-banks-expect-fintech-soc-2) .


## Key Takeaways


- **Reviewers check six things in order:** report type and scope, observation-period freshness, the opinion, exceptions, system-description alignment, and subservice-organization monitoring. Every one has a preventable failure mode.
- **The exceptions table is the part that matters.** Reviewers read it even when sellers assume they won't. Exceptions are fine; exceptions without a management response are not, and the same exception two years running is the worst signal in the document.
- **An expired observation period with no bridge letter is the most common finding** and among the cheapest to prevent. Keep a current one as a standing artifact.
- **Procurement rewards complete artifacts; investor diligence rewards a coherent program.** Know which room you are in before you optimize.
- **Weak posture usually gets priced, not walked away from** — through a lower number, a holdback, or a remediation covenant. That makes lead time enormously valuable.
- **A current Type II report answers much of a standard DDQ by reference,** which is most of its commercial value. Build the answer library once and reuse it.
- **If you don't have SOC 2 and diligence has started, disclose it with a date.** Declared gaps are survivable; discovered gaps reset trust.


For a deeper treatment of how acquisition teams and enterprise buyers scrutinize reports, and how to prepare the documentation set, see Agency's guide to[SOC 2 in due diligence](https://getagency.com/soc-2-in-due-diligence) . If you are under active fundraising or enterprise-deal pressure and need the program built rather than advised on, the[startup compliance program](https://getagency.com/startups) is designed for exactly that timeline, and the[SOC 2 readiness checklist](https://getagency.com/lp/soc-2-readiness-checklist) is a reasonable place to start a self-assessment.
