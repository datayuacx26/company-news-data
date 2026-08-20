---
schema_version: "1.0.0"
document_id: "23350b96e4ae4369eca37a4a73fa08ef73b30f9675ad2e70482964571f70931b"
company_key: "yc-skypher"
company: "Skypher"
source_id: "yc-skypher-news-import-856f7f683911"
canonical_url: "https://blog.skypher.co/blog/business-risk-management-framework"
published_at: "2026-08-13T00:01:19.231+00:00"
first_seen_at: "2026-08-13T10:47:05.118155+00:00"
fetched_at: "2026-08-13T10:47:05.729837+00:00"
content_hash: "sha256:4ea38d0b7b1533d639a5f0dff2b04b428bb119de8b83a18281ec1945b1f0acdc"
---

# Business Risk Management Framework: A Practical Guide

A **business risk management framework** is the structured set of principles, processes, roles, and tools an organization uses to identify, assess, respond to, and monitor risks across its operations. The distinction that matters most before you pick one: enterprise-level frameworks like COSO ERM and ISO 31000 govern risk across the whole organization, while system-level frameworks like the NIST RMF govern risk within specific information systems, and cybersecurity frameworks like NIST CSF characterize the maturity of your security program. Choosing the wrong scope is the most common first mistake.


Your three immediate next steps, regardless of which framework you eventually adopt:


1. **Define your scope.** Are you managing enterprise-wide risk, IT system risk, or cybersecurity risk? That single decision narrows your framework shortlist from six to two or three.
2. **Name an accountable owner.** Assign a named executive (CRO, CISO, or equivalent) who has authority to accept risk on behalf of the organization. Without this, no framework survives contact with the first audit.
3. **Plan a time-boxed pilot.** Select one business unit or one system, run a 60–90 day pilot, and produce three artifacts: a risk register, a Plan of Action and Milestones (POAM), and one executive dashboard. These artifacts become the template for scale.


---


## Key Takeaways


A well-scoped, executive-sponsored risk management framework with a time-boxed pilot, centralized evidence, and continuous monitoring is the most reliable path from compliance checkbox to genuine risk decision support.


Point Details


Match framework to scope Enterprise risk needs COSO or ISO 31000; system authorization needs NIST RMF; cybersecurity maturity needs NIST CSF.


Name an owner before you start A named executive accountable for risk acceptance is the single factor most correlated with program survival.


Pilot before scaling A 60–90 day pilot producing a risk register, POAM, and one executive dashboard reduces resistance and validates your process.


Centralize your evidence Reusing control evidence across audits, questionnaires, and regulatory reviews is the highest-ROI efficiency change available at scale.


Skypher for evidence automation Skypher's questionnaire automation and Trust Center reduce manual evidence collection when your RMF requires repeated proof across customers and auditors.


---


## Table of Contents


- What is a business risk management framework, and how do the types differ?
- Core components every risk management framework should include
- Which framework fits your organization best?
- How to choose the right framework for your organization
- Step-by-step implementation playbook
- Practical risk assessment methods: qualitative, semi-quantitative, and quantitative
- Governance, roles, risk appetite, and reporting
- Continuous monitoring, metrics, and third-party risk integration
- Common implementation mistakes and how to avoid them
- An honest look at the tradeoffs practitioners face
- How Skypher helps you sustain your RMF without the manual overhead
- Authoritative resources to read next
- Sources


## What is a business risk management framework, and how do the types differ?


A risk management framework (RMF) gives an organization a repeatable, auditable method for making risk decisions. The core intent is not compliance; it is decision support. A well-designed framework tells a risk owner, at any point in time, which risks are open, how severe they are, what controls are in place, and whether those controls are working.


[ISO 31000:2018](https://www.iso.org/files/live/sites/isoorg/files/store/en/PUB100464_preview.pdf) defines risk as "the effect of uncertainty on objectives" and provides guidance to integrate risk management into governance, leadership, and culture across any organization. That definition is deliberately broad, which is why ISO 31000 works at the enterprise level. COSO ERM takes a similar enterprise posture, organizing risk management into five interrelated components that align risk appetite with strategy and performance.


System-level and cybersecurity frameworks operate at a narrower scope:


- **Enterprise RMF (COSO ERM, ISO 31000):** Covers strategic, operational, financial, compliance, and reputational risk across the whole organization. Best when the board or C-suite needs a unified risk view.
- **System-level RMF (NIST RMF):** Governs security and privacy risk for specific information systems through a defined authorization lifecycle. Required for U.S. federal agencies and contractors; widely adopted in regulated industries.
- **Cybersecurity framework (NIST CSF):** Characterizes the maturity of your cybersecurity program using Functions, Categories, and Tiers. Not a compliance mandate; a diagnostic and planning tool.


Choose an enterprise framework when your risk appetite needs to connect to business strategy. Choose a system-level framework when you need an authorization decision for a specific system. Use a cybersecurity framework when you need to benchmark and improve your security posture across the organization.


---


## Core components every risk management framework should include


Every credible framework, regardless of scope, shares five canonical processes. The names vary by standard, but the underlying logic does not.


- **Risk identification:** Systematically surface threats, vulnerabilities, and opportunities using workshops, interviews, threat intelligence, and process walkthroughs.
- **Risk assessment:** Evaluate likelihood and impact, using qualitative scoring, semi-quantitative matrices, or quantitative models like FAIR. Produce a prioritized risk register.
- **Risk response/treatment:** Select a treatment strategy (accept, avoid, transfer, or mitigate) and assign a control owner with a remediation deadline.
- **Monitoring and reporting:** Track control effectiveness, open POAMs, and risk trend over time. Report to the right audience at the right cadence.
- **Governance and culture:** Define who owns risk, who approves risk acceptance, and how risk awareness is embedded in day-to-day decisions.


A practical risk taxonomy helps teams categorize findings consistently. Common categories include: **strategic** , **operational** , **financial** , **compliance/regulatory** , **cybersecurity** , and **third-party/supply-chain** . Adding a seventh category, **reputational** , is worth considering for consumer-facing organizations.


**Pro Tip:** *Keep your processes simple enough that a new risk owner can execute them without a consultant in the room. If your risk register requires a 20-field form to add a new finding, people will stop adding findings. Start with five fields: risk description, category, likelihood, impact, and owner.*


---


## Which framework fits your organization best?


Leading practitioner summaries consistently list NIST RMF, NIST CSF, ISO 31000, COSO ERM, COBIT 2019, FAIR, and OCTAVE as the dominant frameworks, and note that organizations often combine elements to fit their scope and maturity. The table below maps each against the dimensions that matter most for selection.


Framework Best for / primary scope Strengths and typical use-cases Complexity / maturity required Regulatory / industry fit Core steps emphasized How success is measured


**NIST RMF** Federal agencies, contractors, regulated systems Structured authorization lifecycle; integrates privacy; maps to NIST SP 800-53 controls Moderate to high; requires dedicated security staff U.S. federal (FISMA); DoD; healthcare; finance Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor ATO status, open POAMs, control effectiveness rate


**NIST CSF 2.0** Any sector seeking cybersecurity maturity Sector-agnostic; Tiers and Profiles support gap analysis; maps to other frameworks Low to moderate; scales to any size Broadly applicable; referenced in SEC cyber rules Govern, Identify, Protect, Detect, Respond, Recover Tier progression, profile gap closure, incident metrics


**ISO 31000** Enterprise-wide risk governance Principles-based; flexible; integrates with any management system Low to moderate; leadership commitment required Global; no mandatory certification; widely recognized Principles, Framework, Process Risk register completeness, treatment closure rate


**COSO ERM** Enterprise strategy and performance alignment Links risk appetite to strategy; strong board-level narrative Moderate; requires cross-functional engagement Finance, public companies (SOX alignment) Governance & Culture; Strategy; Performance; Review; Information & Reporting Risk-adjusted performance metrics, appetite adherence


**COBIT 2019** IT governance and control Aligns IT risk with business goals; strong audit trail High; requires IT governance maturity Finance, healthcare, SOX, GDPR Governance objectives, management objectives, maturity levels Capability levels, audit findings, IT risk KPIs


**FAIR** Financial quantification of information risk Translates cyber risk into dollar exposure; supports investment decisions High; requires data and analytical skill Finance, insurance, any sector needing ROI justification Threat event frequency, vulnerability, loss magnitude Value at Risk (VaR), annualized loss expectancy (ALE)


**OCTAVE** Operational risk in mission-critical environments Asset-driven; developed at Carnegie Mellon's[SEI](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=13473) ; good for smaller teams Low to moderate; self-directed assessment Healthcare, education, government Asset identification, threat profiling, risk analysis Risk profile completeness, mitigation coverage


For risk management framework examples mapped to specific security team contexts, the Skypher blog covers how practitioners apply these in tech and finance environments.


---


## How to choose the right framework for your organization


The right framework is the one your team will actually use. That sounds obvious, but organizations routinely adopt NIST RMF because it looks authoritative, then abandon it six months later because they lack the staffing to sustain an authorization lifecycle. Use this checklist before committing.


**Decision checklist:**


1. **Scope:** Are you managing enterprise risk, system risk, or cybersecurity posture? Match the framework's primary scope to your need.
2. **Regulatory drivers:** Do you hold federal contracts (FISMA/FedRAMP)? Are you a public company (SOX)? Does your sector mandate a specific standard (HIPAA, PCI-DSS)? Regulatory requirements often decide the framework for you.
3. **Existing controls:** If you already have a control library (e.g., NIST SP 800-53, CIS Controls), choose a framework that maps cleanly to it rather than rebuilding from scratch.
4. **Maturity and resourcing:** A team of three cannot sustain a full NIST RMF authorization program. Be honest about your capacity before selecting a high-complexity framework.
5. **Executive sponsorship:** Is there a named executive who will champion the program, attend governance meetings, and accept risk on record? Without this, skip the enterprise frameworks and start smaller.
6. **Integration requirements:** Does the framework need to connect to your SDLC, procurement process, or finance planning cycle? COSO ERM and ISO 31000 integrate more naturally with business processes; NIST RMF integrates with system development.


**Red flags that suggest a framework will fail in your environment:**


- No named risk owner at the executive level
- Framework selected by IT without business unit buy-in
- Risk register lives in a spreadsheet with no assigned update cadence
- Controls are documented but never tested
- Risk appetite has never been formally approved by the board


**Pilot plan template (60–90 days):**


- **Objective:** Validate framework processes on one system or business unit before enterprise rollout.
- **Success criteria:** Completed risk register with at least 10 identified risks, a POAM with assigned owners and due dates, and one executive dashboard reviewed by a senior leader.
- **Artifacts to produce:** Risk register, POAM, governance charter (one page), and a lessons-learned memo.
- **Timeline:** Weeks 1–2 for scoping and stakeholder alignment; Weeks 3–6 for risk identification and assessment; Weeks 7–10 for treatment planning and control mapping; Weeks 11–12 for dashboard build and executive review.


For vendor selection criteria that map to these checklist dimensions, the 6 key insights for selecting risk management vendors guide covers the evaluation factors practitioners use most.


---


## Step-by-step implementation playbook


Implementation works best in four phases. The[NIST RMF](https://csrc.nist.gov/projects/risk-management) provides a disciplined 7-step process (Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor) that maps cleanly onto this phased approach, and[SP 800-37r2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-37r2.pdf) adds organization-wide preparation tasks that make the Prepare step far more substantive than most teams expect.


1. **Prepare (Months 1–2):** Assign roles, define scope, document risk appetite, select framework, and stand up a risk register template. Produce a governance charter.
2. **Pilot (Months 2–4):** Execute the framework on one system or unit. Run a risk assessment, populate the register, map controls, and produce a POAM. Hold a lessons-learned session.
3. **Scale (Months 4–7):** Extend the framework to additional systems or units. Standardize templates, automate evidence collection where possible, and integrate with SDLC and procurement gates.
4. **Operate and improve (Months 7–9 and ongoing):** Run continuous monitoring, report to governance committees, and conduct an annual framework review. Adjust risk appetite and control baselines as the threat environment changes.


**Roles and responsibilities:**


Role Owns Approves Operates


Board / Risk Committee Risk appetite statement Framework adoption Quarterly risk reports


CRO / CISO Risk program design Risk acceptance decisions Monthly risk reviews


Risk / Control Owners Control implementation Treatment plans Daily control execution


Internal Audit Assurance findings Audit reports Periodic control testing


IT / Security Teams Technical controls System authorization Continuous monitoring


**Sample POAM structure:** Each row in your POAM should capture: finding ID, system or process affected, control gap description, responsible owner, planned remediation action, target completion date, and current status. Keep it in a shared platform, not a local spreadsheet, so evidence can be attached directly to each finding.


**Pro Tip:** *When scaling from pilot to enterprise, the single highest-ROI change is centralizing control evidence. If every auditor, customer, and regulator pulls evidence from the same source of truth, your team stops rebuilding the same documentation package three times a year. Automation tools that support evidence reuse pay for themselves quickly at this stage.*


For a deeper look at[automation and smart controls](https://blog.skypher.co/blog/improve-risk-management-with-automation-and-smart-controls) in risk programs, the Skypher blog covers the specific integration patterns that work at scale.


---


## Practical risk assessment methods: qualitative, semi-quantitative, and quantitative


The method you choose for assessment should match your data availability and the decisions you need to support. No single approach is universally correct.


**Qualitative assessment** uses descriptive scales (High/Medium/Low or 1–5) for likelihood and impact. It is fast, requires no historical loss data, and works well for initial risk identification and prioritization. The limitation is that two assessors can score the same risk differently without a calibration session.


**Semi-quantitative assessment** assigns numeric weights to qualitative scales (e.g., likelihood 1–5 multiplied by impact 1–5 to produce a risk score of 1–25). This approach adds consistency without requiring actuarial data. A risk score of 20 or above typically triggers immediate treatment; scores of 10–19 go into the standard POAM queue.


**Quantitative assessment (FAIR)** translates risk into financial exposure using two primary variables: Loss Event Frequency (how often a threat event occurs and results in loss) and Loss Magnitude (the financial impact per event). The output is an Annualized Loss Expectancy (ALE), which lets you compare the cost of a control against the risk it reduces. FAIR is most useful when you need to justify security investment to a CFO or board, or when you are managing cyber risk in a financial institution.


**Sample risk register structure:**


- **Risk ID:** Unique identifier (e.g., RISK-2026-047)
- **Risk description:** Plain-language statement of the risk event
- **Category:** Strategic / Operational / Cyber / Compliance / Third-party
- **Likelihood:** 1 (rare) to 5 (almost certain)
- **Impact:** 1 (negligible) to 5 (catastrophic)
- **Risk score:** Likelihood × Impact
- **Current controls:** Brief description of existing mitigations
- **Treatment strategy:** Accept / Mitigate / Transfer / Avoid
- **Owner:** Named individual
- **Target date:** Remediation or review deadline
- **Status:** Open / In progress / Closed


**Scoring example:** A ransomware risk assessed at Likelihood 4 (likely) × Impact 5 (catastrophic) produces a score of 20, placing it in the immediate treatment tier. If a backup and recovery control reduces likelihood to 2, the residual score drops to 10, moving it to the standard queue. That shift in residual score is the metric your board should see.


---


## Governance, roles, risk appetite, and reporting


Governance is what keeps a framework alive between audits. Without a defined governance model, risk programs drift into annual compliance exercises that produce reports nobody reads.


**Governance layers:**


- **Board / Risk Committee:** Sets and approves the risk appetite statement; receives annual risk reports; challenges management on significant risk exposures.
- **CRO or CISO:** Owns the risk program; chairs the risk committee; approves risk acceptance decisions above the defined threshold; reports to the board.
- **Business unit risk owners:** Own the risks within their domain; implement controls; report monthly to the CRO.
- **Internal Audit:** Provides independent assurance that controls are operating effectively; reports findings to the Audit Committee.


COSO ERM explicitly links risk appetite to strategy and performance, which means your risk appetite statement should not be a standalone document. It should appear in your strategic planning process, your capital allocation decisions, and your M&A due diligence criteria.[Yale's ERM resources](https://ogc.yale.edu/erm/resources/enterprise-risk-management-framework) provide practical examples of how complex organizations operationalize this linkage at the institutional level.


CSF 2.0 adds a Govern function that formalizes cybersecurity risk governance as a first-class activity, not an afterthought. If your organization is aligning to CSF, the Govern function is where your risk appetite, roles, and reporting cadence all live.


**Reporting cadence:**


- **Monthly (operational):** Open POAMs by age, control effectiveness rate, new risks identified, risks closed.
- **Quarterly (executive):** Risk heat map, top 10 risks by score, treatment progress, budget vs. actual for risk remediation.
- **Annual (board):** Risk appetite review, framework maturity assessment, year-over-year risk trend, regulatory findings summary.


**Sample dashboard metrics:** Control uptime percentage, mean time to remediate (MTTR) open findings, percentage of risks with named owners, number of overdue POAMs, and risk score trend (rolling 12 months).


---


## Continuous monitoring, metrics, and third-party risk integration


Continuous monitoring is not a technology; it is a discipline. The technology supports it, but the discipline requires defined exception workflows, assigned reviewers, and a clear escalation path when a control fails.


**Continuous monitoring patterns:**


- **Control monitoring:** Automated checks that verify a control is operating (e.g., patch compliance rate, MFA enrollment, backup success rate). Failures trigger a POAM entry automatically.
- **Telemetry and alerting:** SIEM, EDR, and vulnerability scanner feeds that surface anomalies in near-real-time. Tie these to your risk register so a new critical vulnerability automatically updates the relevant risk score.
- **Exception workflows:** When a control fails or a new risk is identified, a defined workflow routes the finding to the right owner within 24–48 hours, not the next quarterly review.


**Metrics to track program health:**


- Control uptime rate (target: above 95%)
- Mean time to remediate critical findings (target: under 30 days)
- Percentage of risks reviewed on schedule
- Number of third-party vendors with completed risk assessments
- Risk score trend (are aggregate scores improving quarter over quarter?)


**Third-party and supply-chain risk integration** is now a regulatory expectation, not an option. Federal guidance increasingly requires organizations to map supply-chain and third-party risks into their system and organizational risk decisions. Practical steps:


- Maintain a vendor inventory with criticality ratings (critical, high, medium, low).
- Map each vendor to the systems and data they touch.
- Determine which controls can be inherited from the vendor's own certifications (SOC 2, ISO 27001) versus which require independent verification.
- Conduct periodic vendor risk reviews: annual for critical vendors, biennial for high, triennial for medium.
- Include supply-chain risk language in contracts, with the right to audit.


For a step-by-step approach to[vendor risk reviews](https://blog.skypher.co/blog/vendor-risk-review-steps-streamline-your-process) , including how to structure periodic assessments, the Skypher blog covers the full process.


---


## Common implementation mistakes and how to avoid them


Most RMF programs fail for the same handful of reasons, and none of them are technical.


**Do not do these:**


- Treat the framework as a compliance checklist. A risk register that exists to satisfy an auditor, not to inform decisions, is a liability, not an asset.
- Launch without a named executive owner. Risk programs without executive sponsorship stall at the first resource conflict.
- Over-customize at the start. Adapting a framework before you have run it once produces a bespoke process nobody else understands.
- Let the risk register go stale. A register last updated six months ago is worse than no register; it creates false confidence.
- Scope too broadly in the pilot. Trying to cover the entire organization in the first cycle guarantees an incomplete, low-quality output.


**Best-practice checklist:**


- Start with a small, well-scoped pilot and produce real artifacts before scaling using NIST AI RMF compliance automation to streamline your processes.
- Secure a named executive sponsor before the first stakeholder meeting.
- Integrate risk decisions into existing business processes (budget cycles, project gates, vendor onboarding) rather than running a parallel process.
- Reuse evidence across audits, customer questionnaires, and regulatory reviews. Single-source evidence is the most practical efficiency gain available.
- Review and update the risk appetite statement at least annually, and whenever the business strategy changes materially.
- Treat authorization as an ongoing risk decision, not a one-time gate. NIST emphasizes this explicitly: the authorization to operate (ATO) is a living decision, not a certificate.


**Pro Tip:** *Change management is where most RMF programs quietly die. Risk owners who feel the framework is being done to them, rather than with them, will comply minimally and disengage quickly. Run a 30-minute onboarding session for every new risk owner, explain what the framework asks of them specifically, and make it easy to report a risk without filling out a complex form. Adoption follows simplicity.*


---


## An honest look at the tradeoffs practitioners face


Most articles on risk management frameworks present the frameworks as if the hard part is choosing one. In practice, the hard part is sustaining one under real organizational pressure.


The tradeoffs practitioners actually face:


- **Speed vs. coverage:** A fast risk assessment that covers 80% of your risk surface is more useful than a perfect assessment that takes six months and is already outdated by the time it is approved. Calibrate your assessment depth to your decision timeline.
- **Automation vs. human review:** Automated control monitoring catches what it is configured to catch. Human review catches what nobody thought to configure. Both are necessary; neither replaces the other.
- **Centralized vs. federated ownership:** A centralized risk function produces consistency but creates bottlenecks. Federated ownership (risk owners in each business unit) produces speed but risks inconsistency. The answer is usually a hybrid: central standards, federated execution, and a shared platform for evidence and reporting.
- **Framework fidelity vs. pragmatism:** Implementing 100% of a framework's requirements from day one is almost never the right call. NIST acknowledges this explicitly: smaller organizations should scale tasks proportionally rather than discard the framework. Start with the 20% of the framework that addresses 80% of your risk exposure, and expand from there.


The organizations that sustain their risk programs longest are the ones that treat the framework as a living operating model, not a project with a completion date.


---


## How Skypher helps you sustain your RMF without the manual overhead


Once your framework is running, the bottleneck shifts from design to evidence. Every customer audit, regulatory review, and third-party questionnaire asks for the same controls documentation your team already maintains. The cost is not the first time you produce it; it is the fifth, tenth, and fifteenth time.


Skypher is built for exactly this stage. When your RMF requires repeated evidence collection across customers, auditors, and procurement teams, Skypher's AI-powered questionnaire automation answers security questionnaires in under a minute, drawing from a centralized, vectorized knowledge base of your controls documentation. The platform integrates with over 40 third-party risk management platforms, including OneTrust and ServiceNow, and connects directly to Confluence, Notion, Google Drive, OneDrive, and SharePoint, so your evidence stays in one place and your team stops rebuilding the same package for every reviewer.


For organizations managing multiple products or entities, Skypher's[Trust Center](https://skypher.co/trust-center) gives customers and auditors a single, always-current view of your security posture, reducing the back-and-forth that slows vendor onboarding and contract cycles. If you are ready to cut the manual overhead out of your evidence collection process,[request a demo at Skypher](https://skypher.co/) and see how quickly your team can reclaim that time.


---


## Authoritative resources to read next


The sources below are the primary references for building and validating a risk management framework in a U.S. organization.


- **[NIST RMF project page (CSRC)](https://csrc.nist.gov/projects/risk-management) :** The authoritative home for the 7-step RMF, supporting publications, and framework updates. Start here for federal and regulated-industry implementations.
- **[NIST SP 800-37r2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-37r2.pdf) :** The full guidance document for applying the RMF to information systems, including organization-wide tasks, continuous monitoring, and supply-chain integration.
- **[NIST CSF 2.0 (CSWP 29)](https://nvlpubs.nist.gov/nistpubs/cswp/nist.cswp.29.pdf) :** The current version of the Cybersecurity Framework, with the new Govern function and updated Tiers. Use this for cybersecurity program maturity assessment and gap analysis.
- **COSO ERM guidance:** The primary reference for enterprise risk management aligned to strategy and performance. Essential for public companies and finance-sector organizations.
- **[ISO 31000:2018](https://www.iso.org/files/live/sites/isoorg/files/store/en/PUB100464_preview.pdf) :** The international standard for risk management principles and process. Use this when you need a framework that applies across sectors and integrates with any management system.
- **[Yale ERM resources](https://ogc.yale.edu/erm/resources/enterprise-risk-management-framework) :** Practical institutional examples of ERM applied in a complex organization. Useful for governance design and reporting cadence.
- **[SEI / Carnegie Mellon resource library](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=13473) :** Original OCTAVE materials and operational risk assessment guides for practitioners who need an asset-driven, self-directed assessment method.
- **[Skypher risk management guide for tech and finance pros](https://blog.skypher.co/blog/risk-management-guide-for-tech-and-finance-pros) :** Practical implementation guidance and examples tailored to technology and finance teams navigating framework selection and rollout.


## Sources


- [NIST Risk Management Framework | CSRC](https://csrc.nist.gov/projects/risk-management)
- [Risk Management Framework for Information Systems and Organizations: A System Life Cycle Approach for Security and Privacy (NIST SP 800-37r2)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-37r2.pdf)
- [NIST CSWP 29 — NIST Cybersecurity Framework (CSF) 2.0](https://nvlpubs.nist.gov/nistpubs/cswp/nist.cswp.29.pdf)
- [ISO 31000:2018 — Risk management](https://www.iso.org/files/live/sites/isoorg/files/store/en/PUB100464_preview.pdf)


## Recommended


- [Risk management guide for tech and finance pros](https://blog.skypher.co/blog/risk-management-guide-for-tech-and-finance-pros)
- [Risk Management in Compliance: A Practitioner's Guide](https://blog.skypher.co/blog/risk-management-in-compliance)
- [What Is Risk Scoring? A Guide for Risk Managers](https://blog.skypher.co/blog/what-is-risk-scoring-a-guide-for-risk-managers)
