---
schema_version: "1.0.0"
document_id: "733a2c29ff2b6def8e81e35afb902d1c3aca77e175bd6fc6e1d5e28564af3a69"
company_key: "yc-skypher"
company: "Skypher"
source_id: "yc-skypher-news-import-856f7f683911"
canonical_url: "https://blog.skypher.co/blog/gdpr-summary"
published_at: "2026-08-07T00:41:15.193+00:00"
first_seen_at: "2026-08-07T12:30:24.750646+00:00"
fetched_at: "2026-08-07T12:30:26.983338+00:00"
content_hash: "sha256:e2f7cd88cf5103739af9d22667704dbe72f3c6bbaedcbec87a852cc97d7ec677"
---

# GDPR Summary: What US Organizations Need to Know

The GDPR is the EU law that sets mandatory, enforceable rules for how personal data about EU individuals is collected, stored, and used — and it applies to US organizations the moment they offer goods or services to, or monitor the behavior of people in the EU.


> **If your product has EU users, your company is likely in scope.** The practical consequence: you must give EU individuals meaningful rights over their data, document every lawful basis for processing it and face heavy fines if you violate the rules.


Here is what to do first:


- **Check extraterritorial scope:** Does your website, app, or service target EU residents or track their behavior? If yes, the GDPR applies regardless of where your servers sit.
- **Map your data flows:** Identify every category of personal data you collect, where it goes, and who processes it on your behalf.
- **Update your privacy notice:** It must be clear, specific, and written in plain language that an ordinary person can understand.
- **Set up a DSAR process:** EU individuals can request access to their data within one month. You need a documented intake and response workflow before that request arrives.
- **Assess international transfers:** If EU personal data moves to US servers or vendors, you need Standard Contractual Clauses (SCCs) or another approved transfer mechanism in place.


---


## Key Takeaways


GDPR compliance for US organizations requires three things above all: correctly scoping which processing activities are in scope, documenting a lawful basis and transfer mechanism for each, and maintaining records that can be produced to a supervisory authority on request.


Point Details


Extraterritorial scope Any US organization offering services to or monitoring EU individuals is subject to the GDPR, regardless of server location.


Lawful basis documentation Every processing activity needs a documented lawful basis; legitimate interests requires a written balancing test.


72-hour breach notification Controllers must notify the relevant supervisory authority within 72 hours of becoming aware of a personal data breach.


Transfer mechanisms EU personal data sent to US systems requires SCCs plus a Transfer Impact Assessment, or DPF certification.


Maximum fines Serious violations can reach up to 4% of global annual turnover, whichever is higher.


---


## Table of Contents


- What is the GDPR and why does it exist?
- Who does the GDPR actually cover?
- What are the seven core GDPR data protection principles?
- What are the lawful bases for processing personal data?
- What rights do EU individuals have and how do you handle DSARs?
- Special categories and children's data carry higher risk
- Accountability in practice: DPOs, DPIAs, and records of processing
- Breach notification: what the 72-hour rule actually means
- How do international transfers work after Schrems II?
- Enforcement, fines, and who actually investigates
- A prioritized compliance checklist for US organizations
- Evidence management: documenting compliance and answering questionnaires
- What privacy teams actually do first in practice
- What we think about GDPR compliance in practice
- How Skypher helps with GDPR compliance evidence
- Sources


## What is the GDPR and why does it exist?


The General Data Protection Regulation, formally cited as[Regulation (EU) 2016/679](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?from=EN&uri=CELEX%3A02016R0679-20160504) , is the EU's primary law governing the processing of personal data. It entered into force on May 24, 2016, and became directly applicable across all EU member states on May 25, 2018. That second date is the one that matters operationally: it is when enforcement began and when organizations worldwide had to be ready.


The GDPR replaced Directive 95/46/EC, a 1995 framework that predated smartphones, cloud computing, and behavioral advertising. The Directive was implemented differently in each member state, creating a patchwork of national rules that made cross-border compliance inconsistent and enforcement slow. As a regulation rather than a directive, the GDPR is directly binding law in all 27 EU member states without requiring national transposition, which was a deliberate design choice to harmonize the rules.


The legal foundation sits in Article 8 of the EU Charter of Fundamental Rights, which recognizes data protection as a fundamental right. The[European Commission](https://commission.europa.eu/law/law-topic/data-protection/legal-framework-eu-data-protection_en) oversees the regulation alongside the European Data Protection Board (EDPB), which issues binding decisions and guidance to ensure consistent application across member states. For US teams, the EDPB's guidelines on topics like consent, legitimate interest, and international transfers are the most practical reference points beyond the regulation text itself.


---


## Who does the GDPR actually cover?


The GDPR's territorial scope is broader than most US organizations initially expect. EU Commission guidance identifies two distinct tests that bring a non-EU organization into scope:


**Test 1 — Establishment:** You have an office, branch, subsidiary, or any stable arrangement in the EU through which you process personal data, even if the actual processing happens on US servers.


**Test 2 — Targeting or monitoring:** You are outside the EU but either (a) offer goods or services to individuals in the EU (free or paid), or (b) monitor the behavior of individuals in the EU.


Run these questions against your product or service to determine scope:


- Does your website accept registrations or purchases from EU residents without actively blocking them?
- Do you display prices in euros, offer EU-language content, or run ads targeting EU countries?
- Do you use cookies, tracking pixels, or analytics tools that profile EU users' browsing behavior?
- Do you employ remote workers based in the EU whose work-related data you process?
- Do you have a subsidiary, reseller, or representative office in any EU member state?


A single "yes" is enough to trigger the GDPR. A US SaaS company with no EU office but with paying customers in Germany, France, or the Netherlands is in scope under Test 2. A US firm running retargeted ads to EU users through Google Ads or Meta is monitoring behavior and therefore in scope.


One narrow exception: purely personal or household activities fall outside the regulation. An individual who keeps a personal address book is not a controller under the GDPR. That exception does not extend to any commercial or professional activity, however small.


---


## What are the seven core GDPR data protection principles?


The European Commission's principles guidance names seven principles that govern every processing activity. Think of them as the design constraints your data practices must satisfy before you collect a single record.


**Lawfulness, fairness, and transparency** means processing must have a legal basis, must not deceive or harm individuals, and must be disclosed in a privacy notice they can actually understand. In practice: your privacy notice needs to name the lawful basis for each processing purpose, not just say "we use your data to improve our services."


**Purpose limitation** requires that data collected for one stated purpose is not repurposed for something incompatible. If you collect email addresses to send order confirmations, you cannot quietly start using them for behavioral profiling without a separate lawful basis.


**Data minimization** means collecting only what you genuinely need. If your checkout flow asks for a date of birth but age verification is not actually required, that field should not exist.


**Accuracy** obliges you to keep personal data correct and up to date, and to delete or correct inaccurate records promptly. HR systems that retain stale employee addresses or outdated health records are a common failure point.


**Storage limitation** prohibits keeping personal data longer than necessary for the stated purpose. You need a documented retention schedule that specifies how long each data category is kept and what triggers deletion or anonymization.


**Integrity and confidentiality** requires appropriate technical and organizational security measures. The GDPR is technology-neutral: it applies whether processing is automated or manual, and pseudonymized or encrypted data remains in scope if re-identification is possible. Encryption at rest and in transit, access controls, and audit logging are the baseline.


**Accountability** is the principle that ties the others together. You must not only comply but be able to demonstrate compliance through records, policies, and documented decisions. Auditors and supervisory authorities do not take your word for it.


---


## What are the lawful bases for processing personal data?


Every processing activity needs one of six lawful bases. Choosing the wrong one, or failing to document the choice, is one of the most common compliance gaps we see in US organizations.


Lawful Basis When it applies Key consideration


**Consent** Individual has given clear, specific, informed, unambiguous agreement Must be freely given; withdrawable at any time; not bundled with terms


**Contract** Processing is necessary to perform a contract with the individual Applies only to the data strictly needed for that contract


**Legal obligation** Processing is required by EU or member-state law The law must be identifiable and specific


**Vital interests** Necessary to protect someone's life Narrow; applies when the person cannot consent


**Public task** Necessary for a task in the public interest or official authority Primarily for government bodies


**Legitimate interests** Controller's or third party's interests, balanced against individual rights Requires a documented balancing test; cannot override fundamental rights


For most US tech and SaaS companies, the practical choice is between **consent** , **contract** , and **legitimate interests** . Consent is appropriate for marketing emails and non-essential cookies, but it is the most fragile basis because individuals can withdraw it at any time and you must stop processing immediately. Contract works cleanly for data you need to deliver the service the user signed up for. Legitimate interests is flexible but requires a written balancing test showing your interests do not override the individual's rights and freedoms.


Document your chosen basis for each processing activity in your records of processing. If you later need to change the basis, that change must be communicated to data subjects.


---


## What rights do EU individuals have and how do you handle DSARs?


The[Consilium's GDPR summary](https://www.consilium.europa.eu/en/policies/data-protection-regulation/) lists the full set of data subject rights: the right to be informed, right of access, right to rectification, right to erasure ("right to be forgotten"), right to restriction of processing, right to data portability, right to object, and rights related to automated decision-making and profiling.


The most operationally demanding of these is the **data subject access request (DSAR)** . Here is a practical handling checklist:


1. **Intake:** Receive the request in writing (email, web form, or postal mail). Log the date received — the clock starts immediately.
2. **Identity verification:** Confirm the requester is who they claim to be. Ask for reasonable verification without creating an excessive barrier.
3. **Scope the request:** Clarify what data the individual is asking for if the request is ambiguous. This pause does not stop the clock.
4. **Search:** Query all systems where that individual's data may reside: CRM, marketing platform, support tickets, analytics, backups.
5. **Redact third-party data:** Remove any information about other individuals before sending the response.
6. **Respond:** Provide the data in a portable, commonly used format. The standard deadline is one calendar month from receipt, extendable by two additional months for complex or numerous requests (with notice to the individual).
7. **Document:** Record the request, your response, any extensions claimed, and the outcome.
8. **Handle appeals:** If you refuse a request, explain why in writing and inform the individual of their right to complain to a supervisory authority.


**Pro Tip:** *Build a DSAR intake form that captures the requester's name, email, and a description of the data they want. Pair it with a documented search protocol that lists every system your team must query. That combination cuts response time and creates an audit trail that demonstrates good faith if a supervisory authority ever reviews your process.*


Erasure requests carry exceptions: you can refuse if processing is necessary for legal claims, compliance with a legal obligation, or freedom of expression. Document the exception you are relying on.


---


## Special categories and children's data carry higher risk


Article 9 of the GDPR identifies categories of personal data that require stricter handling because of their sensitivity and the potential for serious harm if misused. These include:


- Racial or ethnic origin
- Political opinions
- Religious or philosophical beliefs
- Trade union membership
- Genetic data
- Biometric data used for unique identification (fingerprints, facial recognition)
- Health data
- Sex life or sexual orientation


Processing any of these categories is prohibited by default unless one of the Article 9 exceptions applies, such as explicit consent, employment law obligations, vital interests, or substantial public interest. The practical implication: if your product collects health data, biometric identifiers, or any other Article 9 category, you need both a lawful basis under Article 6 and a specific exception under Article 9, plus a DPIA in most cases.


Children's data adds another layer. For information society services (apps, websites, online platforms) directed at children, the GDPR sets a default age of digital consent at 16, though member states can lower it to 13. If your service is used by minors, you need a verified parental consent mechanism or a robust age-gating process. Simply adding a checkbox that asks users to confirm they are over 16 is not sufficient verification.


Common high-risk scenarios to flag for legal review: employee health monitoring, facial recognition for access control, behavioral profiling of minors, and any processing of genetic data for research purposes.


---


## Accountability in practice: DPOs, DPIAs, and records of processing


Accountability is not a soft principle. It requires documented processes, assigned ownership, and records that can be produced on request. The European Commission's legal framework guidance describes the specific governance structures the GDPR mandates.


### When do you need a Data Protection Officer?


A DPO is mandatory if your organization is a public authority, carries out large-scale systematic monitoring of individuals, or processes special category data on a large scale. Many US tech companies with EU operations fall into the monitoring category. The DPO must have expert knowledge of data protection law, operate independently, and report directly to senior management. You can appoint an external DPO if an internal hire is not feasible.


### When is a DPIA required?


Higher-risk processing activities require a Data Protection Impact Assessment before processing begins. Triggers include:


- Systematic and extensive profiling with significant effects on individuals
- Large-scale processing of special category data
- Systematic monitoring of publicly accessible areas (CCTV, location tracking)
- New technologies where the privacy impact is unclear


A DPIA checklist covers: description of the processing and its purposes; assessment of necessity and proportionality; identification of risks to individuals; measures to address those risks; consultation with the DPO; and, if residual risk remains high, prior consultation with the supervisory authority.


### Records of processing activities


Every controller must maintain a record of processing activities (RoPA). For each processing activity, document:


- The name and contact details of the controller (and DPO if applicable)
- The purposes of processing
- Categories of data subjects and personal data
- Categories of recipients, including third-country transfers
- Retention periods
- A general description of technical and organizational security measures


This record does not need to be filed with any authority proactively, but it must be available on request. Keeping it in a spreadsheet is fine; keeping it current is what matters.


---


## Breach notification: what the 72-hour rule actually means


A personal data breach is any security incident that leads to accidental or unlawful destruction, loss, alteration, unauthorized disclosure of, or access to personal data. That definition covers ransomware attacks, accidental email sends to the wrong recipient, lost laptops, and misconfigured cloud storage buckets equally.


When a breach occurs, the controller must notify the relevant supervisory authority **within 72 hours of becoming aware** , where feasible. "Where feasible" is not a wide escape hatch. Supervisory authorities expect notification within that window unless you can document a specific reason for delay. The notification must include: the nature of the breach; categories and approximate number of individuals and records affected; likely consequences; and measures taken or proposed to address it.


Notification to affected individuals is required when the breach is likely to result in a high risk to their rights and freedoms. If the data was encrypted and the key was not compromised, notification to individuals may not be required, though you still notify the authority.


A practical incident response sequence:


1. **Contain:** Isolate affected systems, revoke compromised credentials, stop ongoing exfiltration.
2. **Assess:** Determine what data was affected, how many individuals, and the likely impact.
3. **Decide:** Apply the notification test. Is there a risk to individuals? Is it high risk requiring individual notification?
4. **Notify:** File with the supervisory authority within 72 hours. Notify individuals if the high-risk threshold is met.
5. **Remediate:** Fix the root cause and implement controls to prevent recurrence.
6. **Document:** Record the breach, your assessment, and every action taken, even if you decide not to notify.


Processor organizations must notify their controller "without undue delay" after discovering a breach, giving the controller time to meet the 72-hour window.


---


## How do international transfers work after Schrems II?


Transferring EU personal data to the United States requires a legal transfer mechanism. The Consilium's GDPR overview confirms that transfers to non-EU countries need either an adequacy decision from the European Commission or appropriate safeguards such as Standard Contractual Clauses (SCCs) or Binding Corporate Rules (BCRs).


The EU-US Privacy Shield was invalidated by the Court of Justice of the EU in the Schrems II ruling (July 2020). Its replacement, the EU-US Data Privacy Framework (DPF), was adopted by the European Commission in July 2023 and provides an adequacy decision for certified US organizations. If your organization is DPF-certified, EU personal data can flow to you without additional transfer mechanisms. Certification requires self-certification with the US Department of Commerce and public commitment to the DPF principles.


For organizations not pursuing DPF certification, the primary mechanism is the updated SCCs issued by the European Commission in June 2021. These are modular clauses that cover controller-to-controller, controller-to-processor, processor-to-processor, and processor-to-controller transfer scenarios. Critically, SCCs alone are no longer sufficient after Schrems II. You must also conduct a **Transfer Impact Assessment (TIA)** to evaluate whether US law (particularly surveillance laws like FISA 702) would undermine the protections the SCCs provide.


Practical mitigation steps for US organizations:


- Execute updated 2021 SCCs with all EU-based data sources and processors.
- Complete a TIA for each transfer, documenting the legal landscape and any supplementary technical measures (encryption, pseudonymization, data minimization).
- Consider DPF certification if your organization meets the eligibility criteria.
- Review vendor contracts to confirm your EU-based processors or sub-processors have transfer mechanisms in place for any onward transfers to the US.
- Document all transfer decisions in your records of processing.


---


## Enforcement, fines, and who actually investigates


Each EU member state has a national supervisory authority (SA) responsible for enforcement within its territory. The European Commission's legal framework describes the **one-stop-shop mechanism** : when a controller operates in multiple member states, the SA in the country of the controller's main establishment leads the investigation, with other concerned SAs involved as co-decision-makers. The EDPB resolves disputes between national SAs and issues binding decisions.


The GDPR establishes two tiers of fines: lower-tier fines for certain violations, and upper-tier fines for more serious infringements. The maximum fines can reach tens of millions of euros or a significant percentage of global annual turnover, depending on the severity and nature of the violation.


> **The maximum fine is not a ceiling that regulators rarely reach.** Supervisory authorities have issued very large fines against major technology companies for violations of consent rules and transparency obligations. The factors that influence severity include the nature and duration of the violation, the number of individuals affected, whether the violation was intentional or negligent, and the degree of cooperation with the investigation.


Non-financial enforcement actions include warnings, reprimands, temporary or permanent bans on processing, and orders to bring processing into compliance. For US organizations, a processing ban affecting EU operations can be more damaging than a fine.


Individuals can lodge complaints directly with any supervisory authority and seek judicial remedies and compensation for material or non-material damage caused by a GDPR violation.


---


## A prioritized compliance checklist for US organizations


Practical compliance for US organizations centers on three priorities: correctly scoping extraterritorial reach, documenting lawful bases, and implementing transfer safeguards that can withstand scrutiny. Here is how to sequence the work:


**Immediate (this week):**


- Apply the extraterritorial scope tests to every product and service line.
- Conduct a data mapping exercise: list every category of personal data, its source, its purpose, where it is stored, and who has access.
- Update your external privacy notice to meet GDPR transparency requirements (lawful basis, retention periods, data subject rights, DPO contact if applicable).
- Draft or update your DSAR intake and response procedure with assigned owners and documented timelines.
- Review your cookie consent mechanism: pre-ticked boxes and implied consent do not meet the GDPR standard.


**Near-term (within 90 days):**


- Complete a DPIA for any high-risk processing activities identified during data mapping.
- Determine whether a DPO is required and appoint one if so.
- Execute updated SCCs with all vendors and partners that receive EU personal data.
- Conduct a TIA for each US-bound transfer of EU data.
- Build or update your records of processing activities (RoPA).


**Ongoing:**


- Implement a GDPR training program for all staff who handle personal data (see the next section).
- Establish a breach detection and notification procedure with a documented 72-hour response protocol.
- Schedule annual reviews of your RoPA, privacy notices, and transfer mechanisms.
- Monitor EDPB guidance and national SA decisions for updates that affect your processing activities.


**Vendor and contract checklist:** Every data processing agreement (DPA) with a vendor that processes EU personal data on your behalf should include: a description of the processing; instructions limiting the processor to your documented purposes; confidentiality obligations; security requirements; sub-processor approval rights; breach notification obligations (without undue delay); assistance with DSARs and DPIAs; deletion or return of data on termination; and audit rights.


---


## Evidence management: documenting compliance and answering questionnaires


Documentation is the accountability principle made tangible. Without it, you cannot defend a regulatory investigation, pass a customer security review, or demonstrate good faith to a supervisory authority. The[EDUCAUSE GDPR library](https://library.educause.edu/topics/policy-and-law/eu-general-data-protection-regulation-gdpr) notes that institutional compliance programs depend on mapping GDPR concepts to operational processes and giving staff clear workflows for DSARs and DPIAs.


An evidence inventory for each processing activity should capture:


Evidence Item What to document


Processing activity name Clear label (e.g., "Customer account management")


Legal basis Which Article 6 basis; balancing test if legitimate interests


Data categories Specific fields collected, not just "contact information"


Retention period Specific duration and deletion trigger


Transfer mechanism SCC module, DPF certification, or adequacy decision


DPIA status Required / completed / not required (with rationale)


DPA in place Vendor name, agreement date, sub-processor list


Last reviewed Date of most recent review


When a customer, partner, or auditor sends a security questionnaire that touches GDPR compliance, the response workflow matters as much as the underlying controls. A practical sequence:


- **Intake:** Log the questionnaire, identify the deadline, and assign an owner.
- **Assemble evidence:** Pull from your evidence inventory rather than drafting answers from scratch.
- **Review and approve:** Legal or privacy counsel reviews answers that involve legal claims or transfer mechanisms.
- **Respond:** Deliver the completed questionnaire with supporting documentation attached.
- **Archive:** Store the completed response and evidence package for future reference.


On tooling: manual folders work for organizations with a small number of processing activities and infrequent questionnaires. A centralized evidence registry (a shared document or GRC platform) scales better and reduces the risk of outdated answers. For organizations that receive frequent security questionnaires from customers or partners,[automated questionnaire tooling](https://skypher.co/security-questionnaires-automation) can surface pre-approved answers from a vetted knowledge base, cutting response time from days to minutes. Skypher's platform, for example, connects to over 40 third-party risk management portals and can process complex questionnaires in under a minute, which is particularly useful when GDPR compliance evidence needs to be shared repeatedly across customer security reviews.


For teams managing chat or messaging data exports as part of DSAR responses, GDPR-compliant data export practices are worth reviewing to understand format and portability requirements.


---


## What privacy teams actually do first in practice


Most US privacy teams approaching GDPR compliance for the first time underestimate two things: how long data mapping takes, and how much organizational friction slows down the lawful-basis documentation exercise.


The realistic sequence is **discover, prioritize, remediate, document, monitor.** Discovery means finding every system that touches EU personal data, which in a mid-size SaaS company can involve 30 or more tools across marketing, sales, product, and support. Prioritization means ranking processing activities by risk, not by ease of fixing. Remediation means making the actual changes: updating privacy notices, renegotiating vendor contracts, implementing consent mechanisms. Documentation means capturing all of it in a RoPA and evidence inventory. Monitoring means treating compliance as a continuous process, not a one-time project.


Where US teams typically get stuck: the legitimate interests balancing test. Many US privacy professionals are accustomed to a notice-and-choice model, where disclosure plus opt-out is sufficient. The GDPR's legitimate interests basis requires a documented three-part test (purpose, necessity, balancing), and supervisory authorities have scrutinized this basis closely in enforcement actions. Getting legal counsel familiar with EU data protection law involved early in that analysis avoids costly rework.


Realistic timeline for a mid-size organization starting from scratch: data mapping and gap analysis takes four to eight weeks; remediation of high-priority gaps (privacy notice, DSAR process, SCCs) takes another six to twelve weeks; full program maturity (DPIAs, training, ongoing monitoring) takes six to twelve months. These are working estimates, not guarantees, and they assume dedicated internal ownership.


One practical note on GDPR versus US privacy laws: the GDPR is rights-based and applies by default to all EU individuals regardless of residency or citizenship. US laws like the California Consumer Privacy Act (CCPA) are opt-out models that apply to California residents and trigger at specific revenue or data volume thresholds. The GDPR's consent standard is stricter, its rights are broader, and its fines are larger. Organizations subject to both should build to the GDPR standard and confirm where CCPA requires additional steps (such as a "Do Not Sell" mechanism). Virginia's CDPA, Colorado's CPA, and other state laws follow a similar opt-out model to the CCPA, meaning GDPR compliance generally exceeds their requirements on most dimensions.


GDPR training for employees should cover: what counts as personal data; how to recognize a DSAR and where to route it; how to identify and report a potential breach; and the basic rules for sharing data with vendors or colleagues in other countries. Annual refreshers and role-specific training for staff who handle special category data or manage vendor contracts are the minimum standard most supervisory authorities expect.


---


## What we think about GDPR compliance in practice


The most common mistake US organizations make is treating GDPR compliance as a legal checkbox rather than an operational capability. A privacy notice drafted by outside counsel and filed away does almost nothing to reduce risk if the engineering team is still collecting data fields that were never disclosed, or if the support team has no idea what to do when a DSAR arrives in their inbox.


The accountability principle is, in our view, the most underrated part of the regulation. It shifts the burden of proof onto the organization. You do not just need to comply; you need to be able to show that you comply, on demand, to a regulator who may not give you advance notice. That means documentation, training, and assigned ownership are not optional extras. They are the compliance program.


We also think the Schrems II implications for US organizations are still not fully internalized. Many companies executed the updated SCCs in 2021 and considered the transfer problem solved. A Transfer Impact Assessment is not a one-time exercise. If your processing activities change, if new US surveillance laws are enacted, or if the EDPB issues updated guidance, your TIA needs to be revisited. The EU-US Data Privacy Framework provides a cleaner path for eligible organizations, but DPF certification requires ongoing compliance with the framework's principles and annual re-certification.


The gap between GDPR and US privacy law is real and consequential. US teams accustomed to opt-out models and broad implied consent will find the GDPR's opt-in consent standard and legitimate interests balancing test genuinely more demanding. Building to the GDPR standard first, then layering US state law requirements on top, is the most efficient approach for organizations operating in both markets.


---


## How Skypher helps with GDPR compliance evidence


When GDPR compliance evidence needs to move quickly, whether for a customer security review, a vendor questionnaire, or an internal audit, the bottleneck is almost always the same: finding the right documentation and getting it approved fast enough to meet the deadline.


Skypher's[Trust Center](https://skypher.co/trust-center) gives your team a centralized, always-current view of your compliance posture, including GDPR-related controls, that you can share with customers and partners on demand. The AI-powered recommendation engine surfaces pre-approved answers from your knowledge base, so your team spends time on judgment calls, not on rewriting the same GDPR compliance answers for every questionnaire that comes in. With integrations across Slack, Microsoft Teams, Confluence, Google Drive, and over 40 TPRM platforms, Skypher fits into the workflow your team already uses.


If you are building or maturing a GDPR compliance program and want to reduce the time your team spends on repetitive evidence requests, we would be glad to walk you through how Skypher handles it. Please feel free to reach out.


---


## Sources


The sources below are the primary references for every claim in this article. Each one is worth bookmarking for your compliance program.


- [Consolidated TEXT: 32016R0679 — EN — 04.05.2016](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?from=EN&uri=CELEX%3A02016R0679-20160504)
- [The general data protection regulation - Consilium](https://www.consilium.europa.eu/en/policies/data-protection-regulation/)
- [Legal framework of EU data protection - European Commission](https://commission.europa.eu/law/law-topic/data-protection/legal-framework-eu-data-protection_en)
- [EU General Data Protection Regulation (GDPR) - EDUCAUSE Library](https://library.educause.edu/topics/policy-and-law/eu-general-data-protection-regulation-gdpr)


## Recommended


- [GDPR Compliance Software: Automating Data Privacy Management](https://blog.skypher.co/blog/gdpr-compliance-software-basics)
- [GDPR Compliance Software: Automating Data Privacy Management](https://skypher.co/post/gdpr-compliance-software-basics-en)
- [GDPR Software Compliance: Key Actions for SaaS Success](https://blog.skypher.co/blog/gdpr-software-compliance-saas)
- [GDPR Compliant Software: Securing Data and Trust](https://blog.skypher.co/blog/gdpr-compliant-software-solutions)
