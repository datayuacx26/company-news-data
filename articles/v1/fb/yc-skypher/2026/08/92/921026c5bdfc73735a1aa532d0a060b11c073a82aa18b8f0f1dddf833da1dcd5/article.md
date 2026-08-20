---
schema_version: "1.0.0"
document_id: "921026c5bdfc73735a1aa532d0a060b11c073a82aa18b8f0f1dddf833da1dcd5"
company_key: "yc-skypher"
company: "Skypher"
source_id: "yc-skypher-news-import-856f7f683911"
canonical_url: "https://blog.skypher.co/blog/pci-compliance-roc"
published_at: "2026-08-10T00:18:19.800+00:00"
first_seen_at: "2026-08-10T14:34:32.901922+00:00"
fetched_at: "2026-08-10T14:34:34.133963+00:00"
content_hash: "sha256:cccd8424469a0c4761acd02c87aa16510eb9940baf1b4c3a647f443390568795"
---

# PCI Compliance ROC: A Practical Guide for Security Teams

A **Report on Compliance (ROC)** is a formal document prepared by a Qualified Security Assessor (QSA) that records the results of a full PCI DSS assessment, confirming whether an organization meets every applicable requirement. In the United States, Level 1 merchants and Level 1 service providers generally must produce one annually, and a data breach can trigger the requirement regardless of transaction volume. If your organization processes, stores, or transmits cardholder data at scale, this guide covers exactly what the PCI compliance ROC process demands and how to prepare for it.


## Key Takeaways


A PCI ROC is a QSA-authored, full-scope assessment that Level 1 merchants and Level 1 service providers in the United States must complete annually, with the AOC serving as the summary document submitted to acquirers.


Point Details


Who needs a ROC Level 1 merchants and Level 1 service providers, plus any organization post-breach, are generally required to complete a ROC annually.


Confirm with your acquirer Acquirers may apply stricter thresholds than card-brand minimums; get the requirement in writing before engaging a QSA.


Five response statuses Every sub-requirement receives one of: In Place, In Place with CCW, Not Applicable, Not Tested, or Not in Place.


Continuous evidence collection Maintaining a year-round, indexed evidence repository reduces QSA follow-up and overall engagement time.


Skypher for readiness Skypher automates evidence tagging and requirement mapping, reducing manual preparation work without replacing the QSA.


## Table of Contents


- What is a PCI Report on Compliance in practice?
- Who must complete a ROC in the United States?
- How does a ROC differ from an SAQ and an AOC?
- Who performs a ROC and what roles are involved?
- What does the ROC contain? Template sections and response statuses
- How does the ROC assessment process work, step by step?
- How to prepare for a ROC: a practical readiness checklist
- Common ROC pitfalls and how to avoid them
- What is the typical ROC timeline and what drives cost?
- How automation can accelerate your ROC readiness
- Authoritative resources and recommended next steps
- The part of a ROC that no checklist fully prepares you for
- Skypher reduces the manual work in your ROC readiness phase
- Sources


## What is a PCI Report on Compliance in practice?


The ROC is not a self-assessment form. It is a QSA-prepared report that documents the assessor's testing methodology, evidence reviewed, and conclusions for each applicable PCI DSS sub-requirement. Think of it as the formal record of an independent audit: every control tested, every sample reviewed, and every finding noted in a structured format that your acquiring bank and payment brands can rely on.


The report draws directly from **assessor work papers** , which are the raw evidence artifacts the QSA collects during the engagement. Work papers include configuration screenshots, interview notes, log samples, policy documents, and scan results. The ROC summarizes those findings; the work papers provide the traceability behind each conclusion. If a regulator or card brand ever questions a finding, the work papers are what the QSA defends.


The[PCI Security Standards Council](https://www.pcisecuritystandards.org/standards/) (PCI SSC) is the authoritative body that publishes both the PCI DSS standard and the official ROC Reporting Template. The v3.2.1 ROC Reporting Template defined the mandatory structure for v3.2.1 submissions, and the[PCI DSS v4.0 ROC Template](https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Reporting%20Template%20or%20Form/PCI-DSS-v4-0-ROC-Template-r1.pdf?agreement=true&time=1651869195480) updates that structure for v4.x assessments, clarifying reporting language and consolidating redundant sections without introducing new technical requirements. Teams moving from v3.2.1 to v4.x should review the updated template with their QSA before the engagement begins, since terminology and section organization have shifted.


Key facts about the ROC as a document:


- It covers every in-scope PCI DSS requirement, not just the ones an organization believes it meets.
- The QSA, not the assessed organization, authors and signs the final report.
- Work papers must be retained and are available for review by the payment brands if requested.
- The ROC feeds directly into the **Attestation of Compliance (AOC)** , which is the summary document submitted to acquirers and card brands.


## Who must complete a ROC in the United States?


The primary driver is transaction volume.[Visa's security and compliance guidance](https://corporate.visa.com/en/resources/security-compliance.html) establishes that Level 1 merchants, commonly defined as those processing more than 6 million Visa or Mastercard transactions annually, generally must complete an annual ROC. Level 1 service providers, typically those storing, processing, or transmitting cardholder data for more than 300,000 transactions per year (Visa's threshold), face the same requirement. Mastercard and other card brands use similar but not always identical thresholds, so confirming with each relevant brand matters.


Beyond the volume thresholds, a ROC can be required in these situations:


- **Post-breach mandate:** Any organization that has experienced a data breach involving cardholder data may be required to complete a ROC regardless of its normal merchant or service-provider level.
- **Card-brand or acquirer request:** A payment brand or acquiring bank can require a ROC at any time, even for a Level 2 or Level 3 merchant, based on risk assessment or contractual terms.
- **Self-elected validation:** Some organizations choose a ROC to provide stronger assurance to enterprise customers or partners, even when an SAQ would technically suffice.


**Pro Tip:** *Confirm your ROC requirement and scope directly with your acquiring bank before engaging a QSA. Acquirers sometimes apply stricter thresholds than the card-brand minimums, and getting that confirmation in writing protects you if the requirement is ever disputed.*


For e-commerce merchants looking to reduce their cardholder data environment (CDE) exposure before a ROC engagement, platform-level security controls are a practical starting point. Teams running payment flows on platforms like BigCommerce, for instance, can consult security plugin guidance for BigCommerce to harden their environment and potentially narrow scope.


## How does a ROC differ from an SAQ and an AOC?


These three terms describe different points in the PCI DSS validation process, and confusing them is one of the most common mistakes compliance teams make.


The **SAQ (Self-Assessment Questionnaire)** is a self-administered validation tool for merchants and service providers that qualify for a reduced-scope assessment. No independent QSA is required to author it, though a QSA can assist. The **ROC** replaces the SAQ for organizations that must undergo a full, independent assessment. The **AOC (Attestation of Compliance)** is a summary document produced after either a ROC or an SAQ is complete; it attests to the organization's compliance status and is what acquirers and card brands typically receive.


Validation document Who authors it Assessment type Typical use case


SAQ Merchant/service provider (self) Self-assessment Level 2–4 merchants, lower-risk service providers


ROC Qualified Security Assessor (QSA) Independent, full assessment Level 1 merchants, Level 1 service providers, breach-triggered


AOC QSA (after ROC) or merchant (after SAQ) Summary attestation Submitted to acquirer or card brand as proof of compliance


A few practical distinctions worth noting:


- An AOC without a ROC behind it (in a Level 1 context) is not sufficient. The ROC is the evidentiary foundation.
- SAQs come in multiple variants (SAQ A, SAQ B, SAQ D, and others) depending on how an organization handles card data. The ROC covers the full standard.
- When sharing compliance status with customers or partners, organizations typically share the AOC, not the full ROC, since the ROC contains sensitive infrastructure details.


## Who performs a ROC and what roles are involved?


The ROC must be authored by a **Qualified Security Assessor (QSA)** , a company accredited by the PCI SSC to perform PCI DSS assessments. Individual QSA employees hold QSA certifications and are the ones who conduct interviews, review configurations, test controls, and sign off on findings. The PCI SSC maintains an[official listing of accredited QSAs](https://listings.pcisecuritystandards.org/assessors_and_solutions/qualified_security_assessors) that organizations should use when selecting an assessor. Choosing a QSA not on that list is not a valid ROC.


An **Internal Security Assessor (ISA)** is a certified employee of the assessed organization who can assist with internal readiness work and gap assessments. ISAs cannot author or sign the final ROC; that authority belongs exclusively to the QSA company. ISAs are most valuable during the pre-engagement readiness phase, where they can mirror QSA testing methodology to surface gaps before the formal assessment begins.


On the client side, a successful ROC engagement typically requires three clear points of contact:


- **Evidence owner:** Responsible for collecting, organizing, and delivering evidence artifacts to the QSA on schedule.
- **CDE owner:** The technical lead who understands the cardholder data environment architecture, network segmentation, and data flows.
- **IT/Security lead:** Handles configuration reviews, system access for the QSA, and technical clarifications during testing.


Distributing these responsibilities clearly before the engagement starts prevents the bottlenecks that extend assessment timelines.


## What does the ROC contain? Template sections and response statuses


The PCI SSC ROC Reporting Template defines the required structure that every QSA must follow. The major sections of the ROC are:


1. **Contact information and report date** — Identifies the assessed entity, the QSA company, and the assessment period.
2. **Executive summary** — High-level overview of the assessment scope, methodology, and overall compliance status.
3. **Description of scope and approach** — Documents the CDE boundary, segmentation controls, and the QSA's scoping methodology.
4. **Description of the reviewed environment** — Network diagrams, system component inventory, and data flow documentation.
5. **Quarterly scan results** — ASV scan results for the assessment period, confirming no unresolved high-severity vulnerabilities.
6. **Findings and observations** — The core of the ROC: the QSA's conclusions for each applicable PCI DSS requirement and sub-requirement.
7. **Appendices** — Compensating controls worksheets (CCWs), sampling details, and any additional documentation the QSA includes.


For each sub-requirement in the findings section, the QSA assigns one of five response statuses defined in the template:


Response status Meaning


**In Place** The control exists and was verified as operating effectively.


**In Place with CCW** The control is met through a compensating control, documented in a Compensating Controls Worksheet.


**Not Applicable** The requirement does not apply to this environment, with documented justification.


**Not Tested** The requirement was not tested during this assessment (rare; requires justification).


**Not in Place** The control does not exist or was not operating effectively at the time of assessment.


A single "Not in Place" finding means the organization is not compliant for that assessment period. Remediation must occur and be re-tested before the QSA can issue a clean ROC.


**Pro Tip:** *Index your work papers by requirement number and sub-requirement before the QSA arrives. A well-organized evidence index, where each artifact is labeled with the specific sub-requirement it supports, can cut QSA review time significantly and reduces the back-and-forth that extends engagements.*


The ROC Reporting Instructions published by the PCI SSC provide additional guidance on how QSAs should document their testing procedures and conclusions within each section.


## How does the ROC assessment process work, step by step?


Understanding the assessment phases helps your team allocate time and resources correctly. The process generally follows this sequence:


1. **Pre-engagement scoping.** The QSA and your team define the CDE boundary, identify all in-scope system components, and confirm segmentation controls. This is where scope reduction opportunities, such as tokenization or point-to-point encryption (P2PE), are evaluated. Errors made here compound through every subsequent phase.
2. **Documentation review.** The QSA reviews policies, procedures, network diagrams, data flow diagrams, and prior assessment artifacts before any onsite or remote testing begins.
3. **Onsite (or remote) assessment.** The QSA conducts interviews with personnel, reviews system configurations, and tests controls against each applicable PCI DSS requirement. Sampling methodology applies here: the QSA selects a representative sample of systems, personnel, and time periods rather than testing every instance.
4. **Evidence collection and work-paper assembly.** Evidence artifacts are collected, labeled, and cross-referenced to specific sub-requirements. The QSA documents testing procedures and results in work papers.
5. **Draft findings and remediation window.** The QSA shares draft findings with your team. Organizations typically have a defined window to remediate "Not in Place" findings and provide evidence of remediation before the report is finalized.
6. **Final ROC and AOC.** Once all findings are resolved or documented, the QSA issues the final ROC and the accompanying AOC. The AOC is submitted to your acquirer and relevant card brands.


[Practitioner guides](https://ispectratechnologies.com/hub/pci-dss/report-on-compliance.html) consistently flag the draft-findings phase as the most stressful for compliance teams. Remediating a control gap under time pressure, while simultaneously managing the QSA relationship, is where preparation pays off most visibly.


## How to prepare for a ROC: a practical readiness checklist


Preparation is where most of the real compliance work happens. The QSA engagement itself should feel like a confirmation of what you already know, not a discovery exercise.


1. **Run a formal readiness assessment.** Mirror the QSA's testing methodology internally before the engagement. Organizations that complete a mock ROC find and fix issues earlier, reducing assessor follow-up and overall engagement time.
2. **Confirm and document your scope.** Define the CDE boundary in writing, including all system components, people, and processes that store, process, or transmit cardholder data. Validate that segmentation controls are functioning as designed.
3. **Complete required scans and penetration tests.** Collect all four quarters of ASV scan results for the assessment period. Complete your annual internal and external penetration test and confirm that all high-severity findings are remediated. Missing scan artifacts is one of the most common causes of assessment delays.
4. **Map evidence to sub-requirements.** For each PCI DSS sub-requirement in scope, identify the specific artifact that demonstrates compliance: a policy document, a configuration screenshot, a log sample, a change record, or a scan report.
5. **Organize work papers into an indexed repository.** Structure your evidence repository by requirement number. A folder structure like` Req_1.1 > \[artifact files\]` lets the QSA navigate directly to evidence without repeated requests.
6. **Validate compensating controls.** If any requirement is met through a compensating control, prepare the CCW documentation in advance and confirm the QSA agrees with the compensating control approach before the formal assessment.
7. **Brief your internal team.** Prepare the evidence owner, CDE owner, and IT/Security lead for the types of questions and access requests the QSA will make. Delays caused by unavailable personnel are avoidable.


**Pro Tip:** *Build your evidence index as a living spreadsheet: columns for requirement number, sub-requirement, artifact name, file location, last-updated date, and the team member responsible. Update it continuously, not just before the assessment. This document becomes your single source of truth during the engagement.*


For broader[security compliance best practices](https://blog.skypher.co/blog/security-compliance-best-practices-streamline-audits) that apply across audit types, including evidence organization and control testing cadences, Skypher's compliance blog covers operational strategies that translate directly to ROC readiness.


## Common ROC pitfalls and how to avoid them


Even experienced compliance teams run into the same set of problems. Knowing them in advance is most of the defense.


- **Under-scoping the CDE.** Teams often exclude systems they believe are "out of scope" without validating that segmentation actually isolates them. If the QSA finds a connection between an excluded system and the CDE, scope expands. Run a segmentation test before the engagement, not during it.
- **Disorganized evidence.** Submitting evidence in an unstructured format, such as a shared drive with files named "screenshot1.png," forces the QSA to spend time organizing rather than testing. That time gets billed. An indexed work-paper repository eliminates this entirely.
- **Segmentation failures discovered mid-assessment.** This is the costliest pitfall. Validate every segmentation control with actual testing, including firewall rule reviews and network traffic analysis, before the QSA arrives.
- **Late remediation.** Attempting to fix control gaps during the assessment window, rather than before it, puts the QSA in the position of re-testing controls under time pressure. Complete remediation before the engagement starts wherever possible.
- **Missing quarterly scans or penetration test results.** The ROC requires a full year of ASV scan results and a current penetration test. If a quarterly scan was missed or a finding was not remediated, the QSA cannot mark the relevant requirements as "In Place." Track scan schedules in a compliance calendar.
- **Inadequate documentation of compensating controls.** A compensating control without a completed CCW is not a valid compensating control in the ROC context. Prepare CCW documentation early and get QSA alignment before the formal assessment.


Running a mock ROC, where an ISA or internal team mirrors QSA testing procedures, is the single most effective way to surface hidden scope or documentation gaps before they become formal findings.


## What is the typical ROC timeline and what drives cost?


A full ROC engagement, from initial scoping through final report delivery, typically spans several months when readiness work is included. The phases that consume the most calendar time are the pre-engagement readiness period and the post-draft remediation window.


**Timeline phases:**


- **Readiness and gap remediation:** 4–12 weeks, depending on the maturity of existing controls and the volume of gaps identified.
- **QSA onsite or remote assessment:** 1–4 weeks for the active testing phase, depending on CDE size and scope complexity.
- **Draft findings and remediation:** 2–6 weeks, depending on the number and severity of findings.
- **Final report and AOC issuance:** 1–2 weeks after all findings are resolved.


**Primary cost drivers:**


- **QSA fees:** Vary by firm, scope complexity, and geographic location. Larger CDEs with more system components and personnel require more assessor hours.
- **Scope breadth:** Every additional system component in scope adds testing time. Scope reduction through tokenization, P2PE, or network segmentation is the most direct cost lever.
- **Remediation engineering:** Control gaps discovered during readiness or assessment require engineering effort to fix. The later a gap is found, the more expensive the remediation.
- **Penetration testing and ASV scanning:** External penetration tests and quarterly ASV scans carry their own fees, separate from QSA engagement costs.
- **Evidence preparation effort:** Disorganized evidence increases QSA hours and therefore cost. A well-indexed work-paper repository directly reduces billable assessor time.


**Cost-reduction levers:**


- Reduce scope through segmentation and tokenization before engaging a QSA.
- Complete a readiness assessment and remediate gaps before the formal engagement begins.
- Deliver organized, indexed evidence to minimize QSA time spent on evidence management.
- Maintain continuous compliance practices year-round so the assessment confirms existing controls rather than discovering gaps.


## How automation can accelerate your ROC readiness


Automation does not replace the QSA. The independence of the assessor is the entire point of the ROC, and no software platform changes that. What automation can do is reduce the manual labor involved in evidence collection, requirement mapping, and work-paper organization, which are the activities that consume the most internal team time during readiness.


Practical automation use cases for ROC readiness include:


- **Automated evidence tagging:** Platforms that connect to cloud storage, SIEMs, and ticketing systems can pull artifacts and tag them to specific PCI DSS sub-requirements automatically, replacing hours of manual mapping.
- **Answer templates mapped to PCI requirements:** Pre-built response templates for common PCI sub-requirements let compliance teams populate evidence mappings faster and with greater consistency.
- **Centralized work-paper versioning:** A centralized repository with version control ensures the QSA always reviews the most current artifact, and that prior versions are retained for traceability.
- **Integration with enterprise data sources:** Connections to tools like Confluence, Google Drive, OneDrive, SharePoint, and ServiceNow allow evidence to be pulled from where it already lives rather than manually assembled.


**Pro Tip:** *Use an automation platform to build your evidence index before the QSA engagement, not during it. The time savings come from having a clean, requirement-mapped repository ready on day one of the assessment, not from trying to organize evidence in real time.*


A compliance team preparing for a ROC can use an automation platform to map existing policy documents, configuration records, and scan artifacts to their corresponding PCI DSS sub-requirements in a fraction of the time manual mapping would take. The QSA still reviews and validates every artifact; the platform simply ensures the evidence is organized, labeled, and accessible when the assessor needs it.


For teams managing multiple compliance frameworks simultaneously, PCI and HIPAA compliance overlap is a common challenge where automation provides additional leverage, since many control artifacts satisfy requirements across both frameworks.


## Authoritative resources and recommended next steps


The PCI SSC is the only authoritative source for ROC templates, reporting instructions, and QSA listings. Use these official resources:


- [PCI DSS v4.0 ROC Reporting Template](https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Reporting%20Template%20or%20Form/PCI-DSS-v4-0-ROC-Template-r1.pdf?agreement=true&time=1651869195480) : The current template for v4.x assessments. Download and review with your QSA before the engagement.
- [PCI DSS v3.2.1 ROC Reporting Template](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v3_2_1-ROC-Reporting-Template.pdf) : The mandatory template for v3.2.1 submissions, still relevant for teams reviewing prior-cycle reports.
- [PCI SSC Standards page](https://www.pcisecuritystandards.org/standards/) : The central hub for all PCI DSS versions, supplemental guidance, and supporting documents.
- [PCI SSC QSA listing](https://listings.pcisecuritystandards.org/assessors_and_solutions/qualified_security_assessors) : The official directory of accredited QSA companies. Always verify your assessor appears here before signing an engagement contract.
- ROC Reporting Instructions: PCI SSC guidance on how QSAs should document testing procedures and conclusions.


Recommended immediate next steps:


- Confirm your ROC requirement and scope in writing with your acquiring bank.
- Schedule a readiness or gap assessment, ideally with an ISA or QSA-assisted review.
- Collect and verify all four quarters of ASV scan results and confirm your penetration test is current.
- Begin building your evidence index mapped to PCI DSS sub-requirements.


One important note on sharing: when providing a ROC or AOC to customers or partners, sensitive infrastructure details may be redacted per PCI SSC guidance. The AOC is the standard document for external sharing; the full ROC is typically retained internally and made available to card brands or acquirers only upon request.


## The part of a ROC that no checklist fully prepares you for


The readiness checklists, evidence indexes, and mock assessments all matter. But the part of a ROC engagement that consistently catches compliance teams off guard is not a missing document or a failed control. It is the day-of-assessment experience itself: the QSA is on-site (or on a video call), asking questions in real time, and the person who owns a particular system is unavailable, or the answer to a configuration question requires pulling a ticket from three months ago that nobody indexed.


The practical lesson from that experience is straightforward: maintain continuous evidence collection year-round. Teams that treat compliance as a point-in-time exercise, scrambling to collect evidence in the weeks before the QSA arrives, consistently face longer engagements and more follow-up requests than teams that log evidence as controls are implemented and updated. A living evidence repository, updated every time a configuration changes or a policy is reviewed, turns the QSA engagement into a confirmation exercise rather than an excavation.


## Skypher reduces the manual work in your ROC readiness phase


The evidence mapping and work-paper organization phases of a ROC readiness effort are where internal teams spend a disproportionate amount of time on tasks that are repetitive and error-prone when done manually. Skypher's AI-powered recommendation engine maps your existing documentation to PCI DSS sub-requirements automatically, and its[smart security knowledge base](https://skypher.co/feature/smart-security-knowledge-base) centralizes versioned evidence artifacts so your QSA can navigate directly to what they need.


Skypher integrates with the tools your evidence already lives in: Google Drive, OneDrive, SharePoint, Confluence, ServiceNow, and more, pulling artifacts into a structured, requirement-mapped repository without manual assembly. The platform does not replace your QSA or the independent judgment the ROC requires. What it replaces is the spreadsheet-and-folder approach to evidence management that costs compliance teams weeks of preparation time. If your team is heading into a ROC engagement and wants to evaluate whether automation can reduce that overhead, Skypher is worth a close look.


## Sources


Use only official PCI SSC documents for reporting format and QSA guidance:


- [PCI DSS v3.2.1 Template for Report on Compliance (PCI SSC)](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v3_2_1-ROC-Reporting-Template.pdf)
- [PCI DSS v4.0 ROC Template (PCI SSC, docs-prv)](https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Reporting%20Template%20or%20Form/PCI-DSS-v4-0-ROC-Template-r1.pdf?agreement=true&time=1651869195480)
- [PCI Security Standards Council — Standards](https://www.pcisecuritystandards.org/standards/)
- [PCI SSC — Qualified Security Assessors](https://listings.pcisecuritystandards.org/assessors_and_solutions/qualified_security_assessors)
- [Visa corporate security & compliance guidance](https://corporate.visa.com/en/resources/security-compliance.html)
- [PCI DSS Report on Compliance (RoC) Explained | ISpectra](https://ispectratechnologies.com/hub/pci-dss/report-on-compliance.html)


*This article provides general informational guidance about the PCI DSS ROC process and is not a substitute for advice from a Qualified Security Assessor or legal counsel. Confirm current requirements with the PCI SSC, your acquiring bank, and a qualified assessor before making compliance decisions.*


## Recommended


- [PCI Compliance Certifications: Impact on Cybersecurity Efficiency](https://blog.skypher.co/blog/pci-compliance-certifications-explained)
- [Why security compliance matters: efficiency and risk reduction](https://blog.skypher.co/blog/why-security-compliance-matters-efficiency-risk)
- [15 security compliance best practices to streamline audits](https://blog.skypher.co/blog/security-compliance-best-practices-streamline-audits)
- [Key compliance frameworks: Streamlining security for tech and finance](https://blog.skypher.co/blog/key-compliance-frameworks-security-tech-finance)
