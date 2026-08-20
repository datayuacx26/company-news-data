---
schema_version: "1.0.0"
document_id: "a88851b3fa5d2c3022875c4e4a428fc6245071cd9f28ebcc5d3c2b22a11cd739"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/global-sovereignty-frameworks-guide"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-24T03:44:24.727364+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:8321779bb65529a10abf3311553aa6a635bcde9a97da331d597818ffdc3c9029"
---

# Sovereignty Is Not Optional: How Governments and Enterprises Should Think About Control

# Sovereignty Is Not Optional: How Governments and Enterprises Should Think About Control


Governments and enterprises need disciplined sovereignty assessments to manage legal, operational, cryptographic, and supply chain control.


Jul 2, 2026


·


Blog


·


Jay Goodman


Sovereignty has become one of the most overused words in enterprise technology. It is invoked to mean data residency, local entity ownership, encryption at rest, local citizenship of executives, or compliance with one regulation or another.


The term often means whatever the speaker needs it to mean. One organization uses it to describe data residency, another uses it to describe ownership, and a vendor may use it to describe something else entirely. As a result, sovereignty discussions frequently collapse into debates over definitions rather than substance .


That flexibility is becoming increasingly difficult to maintain. Across multiple jurisdictions in 2025 and 2026, governments moved sovereignty out of the marketing conversation and into procurement frameworks, regulatory requirements, and formal assessment programs. Governments, sector regulators, and procurement authorities are publishing structured frameworks, mandatory assessment regimes, and procurement floors that bind vendors and buyers alike. The frameworks differ in detail, but they converge on the same core questions about who controls what.


For organizations handling sensitive data, critical operations, or regulated workloads, sovereignty is no longer optional. The challenge now is understanding which frameworks apply and building the internal capability to keep pace as new ones emerge.


## The Frameworks that Have Arrived


Here's a snapshot of the major frameworks already in force or approaching implementation.


### **EU Cloud Sovereignty Framework (SEAL)**


[The European Commission published v1.2.1 in October 2025](https://commission.europa.eu/document/download/09579818-64a6-4dd5-9577-446ab6219113_en)


and


[applied it operationally in April 2026 to award €180 million in sovereign cloud contracts](https://www.reuters.com/business/media-telecom/eu-commission-awards-180-million-euro-cloud-contract-four-european-providers-2026-04-17/)


. The framework scores vendors on eight Sovereignty Objectives across five Sovereignty Effectiveness Assurance Levels (SEAL-0 through SEAL-4). SEAL-2 functions as a procurement floor, and a weighted score determines award priority among qualifying vendors. The Commission has stated that it is applying the same criteria across additional digital services it provides to EU institutions and the upcoming Cloud and AI Development Act is expected to translate the framework's principles into binding EU law.


### **UK Cyber Assessment Framework and GovAssure**


The UK National Cyber Security Centre's Cyber Assessment Framework (CAF) provides a structured, outcomes-based assessment of cyber resilience across critical national infrastructure and government systems. The


[Government Cyber Action Plan, published in January 2026](https://www.gov.uk/government/publications/government-cyber-action-plan)


, reinforces CAF as the standard assessment mechanism and uses GovAssure to apply it to government systems. From the 2026–27 cycle, third-party audits replace self-assessment and auditors require documented technical evidence rather than policy statements. The UK Cyber Security and Resilience Bill is expected to extend related obligations to private-sector operators of essential services.


### **DORA (Digital Operational Resilience Act)**


A direct EU regulation in force since January 2025, DORA applies to roughly 22,000 financial entities across the EU plus their critical ICT third-party providers. It requires ICT third-party registers, contractual provisions on audit rights and termination, and major-incident reporting within four hours of classification. For financial entities,


[DORA is the operative framework on ICT third-party risk with the European Supervisory Authorities](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en)


exercising direct oversight over critical providers.


### **NIS2 Directive**


Replacing the original NIS Directive,


[NIS2 expands scope from roughly 20,000 to roughly 160,000 organizations](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive)


across essential and important sectors, including energy, transport, healthcare, water, digital infrastructure, manufacturing, and public administration. Member states had until October 2024 to transpose NIS2 into national law (some have not yet completed transposition) with full compliance required by October 2026. NIS2 imposes cybersecurity risk management obligations, supply chain security requirements, and incident reporting timelines that mirror DORA in concept.


### **Australian Hosting Certification Framework**


Operational since 2022 and now undergoing reform announced in November 2025, the


[HCF requires that all sensitive Australian Government data, whole-of-government systems, and PROTECTED-classified systems be hosted with HCF-certified providers](https://www.hostingcertification.gov.au/)


. Certification requires demonstrated ownership and control structures aligned with Commonwealth interests, supply chain resilience, and data sovereignty controls.


### **National Sovereign Cloud Initiatives**


[France's Cloud de Confiance](https://www.idc-a.org/news/France-is-building-its-own-Cloud-de-Confiance-for-government-agencies-and-critical-infrastructure-pl)


,


[Germany's Souveräner Cloud](https://www.heise.de/en/news/Google-Cloud-Sovereign-Cloud-in-Germany-by-end-of-2026-11338389.html)


, and similar national programs across European member states create jurisdiction-specific requirements that operate alongside EU-level frameworks. A structurally distinct but often-conflated category is the licensed-technology trustee model: arrangements such as Bleu (Orange and Capgemini operating Microsoft technology in France) and Delos (SAP operating Microsoft technology in Germany) where European entities operate licensed US technology under contractual sovereignty constructs. Whether such trustee structures clear the legal floor of a given framework depends on whether the underlying licensor's jurisdiction creates extraterritorial exposure that flows through the licensing relationship. That analysis is in active discussion across European procurement authorities and remains one of the consequential open questions in current sovereignty policy. Beyond Europe, similar requirements have been published or proposed in Japan, Malaysia, South Korea (CSAP), South Africa, Nigeria, Pakistan, Bolivia, Colombia, Panama, and others. The 2026 US National Trade Estimate Report on Foreign Trade Barriers references cloud sovereignty requirements in roughly fifty percent more countries than the 2025 report. Whether one views these requirements as sound public policy or as trade barriers is beside the point for procurement purposes: they exist, they apply, and they must be navigated.


### **Sector-Specific Assurance Regimes**


Financial regulator guidance from the FCA, PRA, OCC, and equivalent authorities. Healthcare resilience requirements under HIPAA and equivalent national regimes. And energy and critical infrastructure resilience requirements under CER and its equivalents. Each brings its own sovereignty-adjacent obligations on data residency, third-party risk management, and operational resilience.


This list is not exhaustive, and it will likely grow. What matters is the direction of travel: sovereignty requirements are appearing across jurisdictions, sectors, and procurement regimes. Sovereignty is now a multi-framework discipline, and organizations that approach it as a single-framework compliance exercise will be perpetually behind.


## What the Frameworks Have in Common


Put these frameworks side by side and a common pattern emerges. Across SEAL, CAF, DORA, NIS2, HCF, and the national programs, the same core questions appear in different vocabulary.


Who has the legal power to compel disclosure? Where does the data physically reside and under what jurisdiction? Who runs operations and could the customer continue if the vendor were unavailable? What does the supply chain look like and where are its dependencies? What evidence exists that the system behaves as claimed? How are incidents reported and to whom? Can the customer exit and on what terms?


The frameworks differ in weighting, procurement thresholds, and reporting obligations, but they are ultimately trying to answer the same foundational questions. An organization that has built internal discipline around the questions can navigate any specific framework with reasonable confidence. An organization that has built compliance around one framework alone will keep retooling as new frameworks arrive.


This is why organizations should build sovereignty capabilities rather than optimize for a single regulation. Frameworks will change. The governing questions are far more durable.


## Sovereignty Is Not a Property of a Vendor


With the landscape established, one conceptual move makes the rest of the conversation tractable: sovereignty is not a property of a vendor.


Vendors do not have sovereignty. Customers have sovereignty and a vendor relationship either preserves it or erodes it.


It may seem like a subtle distinction, but it changes the entire assessment. Once sovereignty is viewed from the customer's perspective, the discussion shifts from vendor claims to actual control.


When a customer signs a contract with a vendor, some degree of control over data, infrastructure, or operations is transferred. Sovereignty assessment asks how much control moved, what type of control moved, and who ultimately exercises it.


Consider a vendor headquartered in a jurisdiction with extraterritorial legal compulsion, providing a SaaS service from local data centers with locally resident support staff and locally held encryption keys. That vendor has built a partial operational façade without transferring sovereignty to the customer. The authority remains with the parent company, the parent company's jurisdiction, and the parent company's compulsion regime. The customer's sovereignty is a function of that reality, not the visible facade.


Sovereignty ultimately comes down to control: who has it, where it sits, and what authority governs it. Recent frameworks approach the problem differently, but they increasingly converge on that same principle.


## Five Dimensions Every Sovereignty Assessment Must Address


Strip away the labels and most sovereignty assessments come down to five questions. Different frameworks slice them slightly differently but the foundational questions are the same.


### **1. Legal Authority**


Who has the legal power to compel the vendor to disclose, modify, or hand over data and operations, and is that authority foreign to the customer's jurisdiction? "Foreign" is always relative to the organization conducting the assessment, whether that is a procurement office, regulator, or governing body. A vendor's home jurisdiction is not inherently problematic; the issue is whether that jurisdiction can exercise authority in ways that conflict with the customer's sovereignty requirements. EU procurement asks whether the vendor is exposed to non-EU compulsion. US federal procurement asks whether it is exposed to non-US compulsion. Allied governments ask whether it is exposed to compulsion by jurisdictions outside the alliance. Each framework defines its own legal floor in those terms. This is also why the parent company's jurisdiction matters more than the operating subsidiary's: the parent retains the legal authority to compel the subsidiary regardless of where the subsidiary is incorporated. Extraterritorial compulsion regimes, laws that authorize a state to compel its companies to produce data regardless of where that data physically sits, are the central concern and they are the central concern relative to the assessing authority. The 2025 sworn testimony before the French Senate by a major hyperscaler's local subsidiary, confirming that local data could not be guaranteed safe from parent-jurisdiction compulsion, is the canonical public-record example for EU sovereignty assessment specifically. The same testimony would land differently in a US federal procurement context, where compliance with US law is constitutive of acceptable sovereignty, not a violation of it.


The principle is consistent; the outcome depends on the jurisdiction conducting the assessment.


### **2. Operational Authority**


Who runs the system day to day and could the customer continue to operate it if the vendor were unavailable? This is sometimes framed as "operational autonomy" or "exit cost," but at its core it asks whether the customer has technical and contractual access to the operating substance of the service: source code, build pipelines, deployment infrastructure, and support escalation paths. A SaaS service with no on-premises option offers structurally less operational sovereignty than a service the customer can deploy and run within their own infrastructure, regardless of how robust the vendor's operations are. The International Criminal Court's 2025 transition away from a major hyperscaler to open-source alternatives, prompted by US sanctions affecting ICC personnel, is a real-world example of operational sovereignty becoming an immediate concern.


### **3. Cryptographic and AI Authority**


Who holds the keys and where do AI inference paths run? Encryption keys held by the vendor, even when the vendor is contractually committed not to use them, are structurally different from keys held by the customer. A vendor that holds the keys can be compelled legally or breached technically in ways the customer cannot prevent. The same logic applies to AI features: if customer data flows through a model hosted in non-customer-controlled infrastructure, the customer has lost control over that data flow, even if the vendor commits not to retain or train on it. As AI capabilities are woven into every category of digital service, this dimension is becoming the fastest-growing source of unintended sovereignty erosion.


### **4. Supply Chain Authority**


Where is the software built and through what chain of suppliers does it reach the customer? This is the question with the longest causal chain and often the least visibility. Software is rarely built end to end in one place. It is composed of dependencies, libraries, build tools, and operational infrastructure that may be sourced globally. Sovereignty here is a question about the resilience of that supply chain to disruption, sanction, or compromise, not just the headquarters location of the named vendor. The EU's Cloud Sovereignty Framework weigh


t


s supply chain at twenty percent, the highest of any single dimension, reflecting a policy judgment that supply chain origin is the most durable and least retrofittable of the sovereignty dimensions.


### **5. Demonstrated Security and Operational Integrity**


What evidence exists that the system does what it claims? Sovereignty in the abstract is meaningless if the system has been repeatedly compromised. Certifications matter here, but as evidence of operational maturity, not as marketing accolades. The relevant certifications are the ones that validate the system in deployment, such as NATO Restricted accreditation, NSA Commercial Solutions for Classified, BSI federal certifications, and CAF assessments at appropriate profiles, rather than those that validate the vendor's processes in the abstract.


These five dimensions are not exhaustive. Sustainability, technology openness, and strategic stability also matter, and rigorous frameworks include them. But these five are the core sovereignty conversation and any vendor evaluation that does not address all five is producing a partial answer.


## The Floor Problem


One of the easiest mistakes in sovereignty assessment is treating every dimension as a tradeoff. A weighted average of the five dimensions produces a score, but the score is misleading if any single dimension sits at zero.


A vendor with strong cryptographic architecture, mature operations, a transparent supply chain, and extensive security certifications but with exposure to foreign legal compulsion has a sovereignty hole the other four dimensions cannot fill. The hole is not in the visible part of the architecture but the underlying legal authority that sits above the architecture. Cryptography does not protect against keys handed over under legal order. Operational maturity does not protect against compelled access. Supply chain transparency does not change parent-company jurisdiction.


This is why sophisticated frameworks treat certain dimensions (with legal authority as the canonical example) as floor conditions: minimum thresholds that must be met independently rather than averaged with other dimensions. The EU Cloud Sovereignty Framework's two-stage evaluation, a procurement floor on every dimension followed by a weighted score among qualifying vendors, is one expression of this discipline. CAF's outcomes-based principles, assessed against profile-specific thresholds, is another. The pattern recurs because it reflects how sovereignty works under stress.


For organizations building their own assessment criteria, identifying the floor conditions is the most important early decision. The question to ask is which dimensions, if they failed, would make the entire vendor relationship untenable regardless of compensating strength. In most regulated procurement, legal authority is one. Cryptographic authority is often another as no amount of operational excellence compensates for the vendor holding your keys if your threat model includes vendor compromise or vendor compulsion. Supply chain integrity can be a third for systems where supply chain attacks are a primary concern.


The award conversation, the weighted score among vendors that clear the floor, is secondary. It matters, but it is a different kind of question. It concerns preference among acceptable options.


Before organizations compare vendors, they first need to determine which vendors are acceptable candidates at all.


## Why Operational Substance Matters More than Corporate Structure


A pattern recurs across sovereignty discussions: the use of corporate structure to claim sovereignty without changing operational substance. A vendor headquartered in one jurisdiction establishes a subsidiary in a sovereignty-sensitive jurisdiction, points to it as the local sovereign anchor, and claims sovereignty. On inspection, the substance is that the subsidiary is a contracting layer over a parent-controlled operation. Engineering happens at the parent. Build infrastructure runs in the parent jurisdiction. Update channels flow through parent control. The sovereignty claim is thin in substance.


Operational substance is the test that separates real sovereign operations from corporate-structure facade. But it is a test that operates within the legal floor, not around it. A substance-rich operating entity in a sovereignty-sensitive jurisdiction does not change the legal authority that sits above the entire structure. If the parent company is incorporated in a jurisdiction whose legal regime allows compulsion by an authority foreign to the customer's jurisdiction, the parent retains that compulsion exposure regardless of how much operational control the subsidiary carries. Operational substance can move SOV-4, SOV-5, SOV-6, and SOV-7 toward sovereignty. It cannot move SOV-2 (Legal) past the floor that the parent's jurisdictional posture imposes relative to the customer. Customers and regulators evaluating sovereignty claims should treat functional control and legal jurisdiction as separate questions, answered separately, with the legal floor decided first and decided relative to the customer's own jurisdiction.


With that constraint stated clearly, the operational-substance test is what separates real sovereign operations from corporate facade among vendors whose parent-company jurisdiction allows them to clear the legal floor in the first place. Among those vendors, the corollary pattern, where corporate structure follows operational control, produces durable sovereignty. An operating entity that runs the local-variant deployments, contributes engineering to the local-specific codebase, runs build infrastructure in local jurisdiction, and controls update channels for local customers is delivering sovereignty in operational control. The corporate structure reflects the operational reality rather than wrapping around the parent.


Distinguishing real operational substance from facade requires looking past the headline and applying the test at the right level of granularity. A vendor with global operations rarely fits cleanly into one pattern or the other across all of its products and customer segments. The same vendor may have a substance-rich operating subsidiary for one specific product configuration, typically the deployment variant designed for sovereignty-sensitive customers in a particular jurisdiction, while running other product lines through more conventional parent-led operations. The substance-rich versus substance-thin test is therefore most useful at the level of the specific deployment configuration being procured, not at the level of the vendor as a global brand. Asking "Is this vendor sovereign?" produces less useful answers than asking two narrower questions in sequence: "Is the parent company's jurisdiction one that allows this configuration to clear the legal floor?" and only then, "Is the specific deployment configuration we are buying, with the specific operating entity that will deliver it, structured for substance-rich operation in our jurisdiction?"


Start with jurisdiction. Then evaluate operations.


-


What is the parent company's jurisdiction and does that jurisdiction expose the parent to legal compulsion by an authority foreign to the customer's own jurisdiction?


-


Who employs the engineers writing the code that ships in this specific deployment configuration?


-


Where does the build pipeline for this configuration run and who has access to it?


-


Where do update channels for this configuration originate and who controls release decisions?


-


If the parent company were unavailable, could the local operating entity continue to operate the service for customers in this configuration?


When the parent's jurisdictional exposure is foreign to the customer, the legal floor fails regardless of how the operational questions are answered. The substance built on top of a non-sovereign legal foundation does not reach sovereignty for that customer —just a more credible facade. When the parent's jurisdiction is acceptable to the customer's framework and the operational answers point to the local operating entity as the operative substance, sovereignty is real for that configuration. When the parent's jurisdiction is acceptable but the operational answers point back to the parent, the sovereignty claim is a marketing layer regardless of how the contracting structure is presented.


**Related Reading:** **[Sovereignty Gets a Score: What SEAL Is and Why It Matters Beyond Cloud](https://www.blackberry.com/en/secure-communications/insights/blog/sovereignty-effective-assurance-levels-explainer)**


Get updates about the latest in-depth knowledge for secure communications.
