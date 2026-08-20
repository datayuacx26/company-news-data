---
schema_version: "1.0.0"
document_id: "2f81fa2ce6f1e18eeaabddc8e75eca935e21531f42e9cef0a02ddede56631a53"
company_key: "yc-wolfia"
company: "Wolfia"
source_id: "yc-wolfia-news-import-63b7007a854b"
canonical_url: "https://wolfia.com/blog/dora-vendor-risk-questionnaire-requirements-for-2026"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-22T23:41:27.201440+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:15c5114223f894b6ea37ec83e56977de6125cdd3fc61bb398ea14d43c5df41c2"
---

# DORA vendor risk questionnaire requirements for 2026

## TL;DR


- DORA (Regulation (EU) 2022/2554) has been directly applicable to EU financial entities since 17 January 2025, and it requires them to push specific ICT third-party risk terms down into vendor contracts and questionnaires.
- Article 30 of DORA lists mandatory contract provisions, service levels, data location, audit rights, subcontracting disclosure, exit assistance, and buyers now ask vendors to confirm each one before signing.
- Financial entities also maintain a register of information on every ICT third-party provider, which drives a new block of questionnaire fields: legal entity identifiers, service criticality classification, and subcontracting chains.
- DORA questionnaires arrive alongside SOC 2 and ISO 27001 requests, not instead of them, so vendors need both evidence sets mapped and ready.
- Wolfia pre-loads DORA-mapped answers, including Article 30 contract terms and subcontractor disclosures, so SaaS vendors selling into EU financial services aren't drafting these answers from scratch on deal five.


## What is DORA and why does it change vendor questionnaires?


The Digital Operational Resilience Act is an EU regulation that requires banks, insurers, investment firms, and other financial entities to manage ICT risk, including the risk introduced by their vendors, under one harmonized framework. It became directly applicable across the EU on 17 January 2025, per[the official DORA regulation text on EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R2554) , and financial entities have spent 2025 and 2026 pushing its requirements into procurement.


That flow-down is why SaaS vendors are seeing new question categories on questionnaires from EU banks, insurers, and asset managers. DORA does not regulate a US or non-EU SaaS company directly. It regulates the financial entity, and that entity is contractually and supervisorily on the hook for its ICT providers' resilience, so it has to ask its vendors for proof.


## What DORA requires in vendor risk questionnaires


Most DORA-driven questionnaires cluster around three areas: contractual terms under Article 30, ICT incident and business continuity readiness, and register-of-information fields the buyer's compliance team must file internally. Expect questions like "Will you agree to unrestricted audit rights for our regulator," "What is your subcontracting notification period," and "What is your maximum tolerated downtime for this service."


On the questionnaires we process for financial-sector buyers, the recurring choke point isn't the security posture questions, most vendors already answer those cleanly from SOC 2 evidence. It's the contract-shaped questions: exit assistance periods, subcontractor chain disclosure, and data location commitments that a security team can't answer alone without pulling in legal.


## How does DORA's register of information affect ICT vendors?


Every DORA in-scope financial entity has to maintain a structured register listing each ICT third-party provider, the service supplied, and a criticality classification. That register requirement means vendors get asked for fields most other frameworks never touch: legal entity identifier (LEI), the specific function the service supports, and whether any part of the service is subcontracted outside the EU.


If your questionnaire includes a field asking for your LEI or a full list of subprocessors down to the fourth tier, that's the register of information talking, not a generic vendor security check. Buyers need those exact fields to stay compliant with their own filing obligation, so vague answers ("we use standard cloud providers") get bounced back for specifics.


## Article 30 contractual provisions SaaS vendors need to answer for


Article 30 sets the mandatory minimum contract terms for any agreement between a DORA-scoped financial entity and an ICT third-party provider. The provisions cover: a full description of the service and its locations, data protection and access terms, service level commitments tied to availability and performance, cooperation during ICT incidents, audit and inspection rights (including the right to conduct on-site inspections), termination rights, and exit assistance covering a defined transition period.


A questionnaire built off Article 30 will often mirror the article's structure almost line for line. Vendors who have already mapped answers to[SOC 2 compliance requirements](https://wolfia.com/blog/soc-2-compliance-requirements-complete-guide) and[ISO 27001 certification](https://wolfia.com/blog/iso-27001-certification-guide) still need a separate pass here, because Article 30 asks about contract commitments, not just control evidence. "Do you have an incident response plan" and "will you contractually commit to notifying us within a defined window of an ICT incident affecting our service" are different questions with different owners inside most SaaS companies.


## DORA vs SOC 2 and ISO 27001: what's different


SOC 2 and ISO 27001 evidence answers "do you have the controls." DORA questionnaires answer "will you contractually commit to specific resilience and cooperation terms, and can you name every subcontractor in your delivery chain." A vendor can pass SOC 2 Type II cleanly and still stall a DORA questionnaire because nobody on the security team owns exit-assistance language or subcontracting disclosure.


This is the same pattern we've seen with[the EU AI Act's vendor assessment requirements for SaaS](https://wolfia.com/blog/eu-ai-act-vendor-assessment-requirements-for-saas) : EU regulation increasingly asks vendors to answer at the contract and supply-chain level, not only the control level. Teams that treat these as one more security questionnaire category, instead of a distinct answer set with its own owners, end up re-drafting the same legal language deal after deal.


## Critical vs non-critical ICT third-party providers


The European Supervisory Authorities designate a subset of ICT third-party providers as "critical" based on systemic importance to the financial sector, and those providers face direct EU oversight rather than only contractual flow-down from customers. Most SaaS vendors selling questionnaire automation, CRM, HR, or productivity software are not on that critical list.


Being off that list does not reduce the due diligence a financial-entity buyer owes you under Articles 28 and 29, which require a risk assessment before onboarding any ICT third-party provider regardless of criticality. Non-critical status only spares you direct regulator inspection. The buyer's questionnaire stays just as long.


## How Wolfia handles DORA-driven questionnaires


Wolfia is built for security and GRC teams answering exactly this kind of cross-framework questionnaire. For DORA specifically:


- **Knowledge management dashboard** : store your Article 30 contract positions, subcontractor list, and exit-assistance terms as first-class knowledge base entries alongside your SOC 2 and ISO 27001 evidence, so answers stay consistent across every EU financial-sector deal.
- **Questionnaire automation** : Wolfia matches incoming DORA-shaped questions, register-of-information fields, subcontracting disclosures, LEI requests, to your stored answers automatically, with source citations on every answer so reviewers can verify against the underlying policy.
- **Answer auto-routing to legal** : contract-commitment questions (exit assistance windows, audit rights, notification periods) route automatically to your legal or compliance reviewer before the questionnaire ships, so security isn't guessing at terms only legal can commit to.
- **Trust Center** : publish your standard DORA contractual positions and subcontractor disclosures once, gated behind NDA, so buyers can self-serve the fields that don't change deal to deal.
- **Portal Agent** : for buyers who route DORA questionnaires through OneTrust or ServiceNow rather than email, Wolfia's Chrome extension fills the portal directly using the same DORA-mapped answer set.


We also cover[third-party risk management](https://wolfia.com/blog/third-party-risk-management-guide) broadly and the[vendor security assessment checklist procurement teams use](https://wolfia.com/blog/vendor-security-assessment-checklist-for-procurement-teams) , both useful starting points if you're building a DORA answer set from an existing vendor risk program rather than from scratch.


## Final Thoughts


DORA has been live for financial entities since January 2025, and 2026 is the year its Article 30 contract requirements and register-of-information fields show up consistently on SaaS vendor questionnaires, not just from the largest banks but from mid-size EU insurers and asset managers working through their own vendor inventories. The fastest way to stop re-answering the same subcontracting and exit-assistance questions from scratch is to map them once, get legal sign-off on the standard language, and store it where your questionnaire workflow can pull it automatically.
