---
schema_version: "1.0.0"
document_id: "ed126093e275e80089cf4ed32db6a2a37198ed2b284cf370230bd4ef4bfdd4c8"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/sovereignty-effective-assurance-levels-explainer"
published_at: null
first_seen_at: "2026-07-27T20:26:23.529236+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:fb0b09144bc92eec49476e38458ef6d24611272e72e94ef6b3edbd61e3098f89"
---

# Sovereignty Gets a Score: What SEAL Is and Why It Matters Beyond Cloud

Several sovereignty frameworks have emerged over the past 18 months, but the EU Cloud Sovereignty Framework SEAL warrants the closest attention. It is the most structurally rigorous attempt yet to operationalize sovereignty assessment. Its design shows how a sophisticated procurement authority breaks sovereignty into measurable components. To study SEAL closely is to study the structural template other jurisdictions are likely to adopt.


In October 2025, the European Commission published[v1.2.1 of the Cloud Sovereignty Framework](https://ec.europa.eu/newsroom/dae/redirection/document/113103) . In April 2026, the framework moved from policy document to operational reality[when the Commission used it to award €180 million in sovereign cloud contracts](https://digital-strategy.ec.europa.eu/en/news/commission-advances-cloud-sovereignty-through-strategic-procurement) . Vendors had to clear a defined sovereignty threshold on every dimension before their bids would even be scored. The framework performed as designed: most winners landed at SEAL-3 (Digital Resilience), and the rejection criteria did most of the sorting before anyone consulted the weighted score column.


That two-stage structure is what most observers missed when the framework first appeared. SEAL is not a single number but a procurement floor combined with a weighted award score, and the floor does the heavy lifting.


## What Is SEAL?


SEAL stands for Sovereignty Effective Assurance Levels. It is a 0-to-4 scoring methodology embedded within a broader framework that asks eight structural questions about a vendor's service:


-


**SOV-1 Strategic.** Where do the bodies with decisive authority over the service sit and how stable is that arrangement?


-


**SOV-2 Legal.** Can a foreign government compel the vendor to disclose data?


-


**SOV-3 Data and AI.** Who holds the cryptographic keys? Where do AI inference paths run?


-


**SOV-4 Operational.** Can EU customers run, support, and evolve the service independently of non-EU resources?


-


**SOV-5 Supply Chain.** Where is the software architected, programmed, packaged, and distributed from?


-


**SOV-6 Technology.** Are interfaces open and auditable, or does proprietary lock-in apply?


-


**SOV-7 Security.** Are EU-recognized certifications held, and do they validate operations as well as architecture?


-


**SOV-8 Environmental.** Is sustainability performance measurable and achieved or merely aspirational?


Each objective is scored from SEAL-0 (no meaningful sovereignty) to SEAL-4 (full digital sovereignty, no critical non-EU dependencies). The contracting authority then sets two thresholds: a minimum SEAL level on each objective (the floor) and a minimum on the weighted Sovereignty Score (the award threshold).


## Why the Floor Matters More than the Score


The two-stage evaluation is the framework's most consequential design choice. A vendor with strong security certifications, mature operations, and a deep partner ecosystem can still face outright elimination if a single Sovereignty Objective registers SEAL-0 — a hard rejection.


The treatment of legal sovereignty (SOV-2) carries the greatest operational weight. SEAL is a framework for *EU* sovereignty assessment, which means SOV-2 asks whether the vendor is exposed to legal compulsion by authorities foreign to the EU. Vendors subject to extraterritorial compulsion regimes that reach into EU operations from outside the EU, most prominently the U.S. CLOUD Act and FISA Section 702, score SEAL-0 on legal sovereignty regardless of how much EU data residency, encryption, or operational footprint they layer on top. The framework treats foreign-to-the-EU legal authority as the governing fact: if a non-EU jurisdiction can compel disclosure, the contractual or technical barriers above that authority do not change the underlying sovereignty assessment for EU procurement purposes.


This assessment is jurisdiction-relative. A U.S. federal procurement officer reading SEAL would recognize the same structural logic operating in their own frameworks. FedRAMP, DoD impact levels, and U.S. classified-grade certifications all ask whether the vendor is exposed to compulsion by authorities foreign to the *United States* . Each procurement authority defines its sovereignty floor relative to its own jurisdiction. The mathematics is symmetric. The conclusions in any specific assessment depend on whose sovereignty is being assessed and which jurisdictions sit above the vendor.


In the April 2026 awards, several major providers were ruled out before scoring began because their legal exposure fell outside SEAL-2 requirements despite strong technology and compliance credentials.


## What the Weighting Tells You about EU Priorities


Among vendors that clear the floor, the framework computes a weighted Sovereignty Score using these weights:


-


SOV-5 Supply chain: 20 percent


-


SOV-1 Strategic, SOV-4 Operational, SOV-6 Technology: 15 percent each


-


SOV-2 Legal, SOV-3 Data and AI, SOV-7 Security: 10 percent each


-


SOV-8 Environmental: 5 percent


Two things stand out. First, supply chain is the single most heavily weighted dimension. The Commission's framing, read through the contributing factors, makes clear that the EU regards supply chain sovereignty as the most durable kind: legal status can change with a treaty and operational substance can be relocated, but where software is architected, built, and shipped from is far harder to refactor on demand.


Second, legal sovereignty carries only 10 percent in the score, yet it applies with full force at the floor. A vendor scoring SEAL-4 across every other dimension cannot compensate for SEAL-0 on legal sovereignty. This is a deliberate inversion of how vendor evaluation is sometimes treated, where strength in one area offsets weakness in another. Under SEAL, structural exposure to foreign legal compulsion is disqualifying, not balanceable.


## How the Framework Is Extending


SEAL was authored for cloud services and the April 2026 awards demonstrated it operating as designed in cloud procurement. But the framework has not stayed contained to cloud.


The European Commission has stated publicly that it is applying the same sovereignty criteria across other digital services it provides to EU institutions; the upcoming Cloud and AI Development Act is expected to translate the framework's principles into binding EU law. Industry analysts have described SEAL as the emerging reference for sovereignty assessment in regulated procurement more broadly. Reporting outside the EU has noted it as a cited reference point in markets including Australia.


The structural reason for this extension is that the eight Sovereignty Objectives are domain-agnostic. The questions they ask (who holds authority, who runs operations, who controls keys, where the supply chain originates) have nothing cloud-specific about them. They translate cleanly to any digital service category: secure communications, mobile device management, critical event management, productivity software, data analytics, and AI services. Wherever a vendor provides a service that touches sensitive data or sensitive operations, the framework's structural questions apply.


## What This Means for Organizations Evaluating Vendors


For organizations whose procurement is bound by EU rules, SEAL is now an active evaluation framework. Vendors are being scored against it and procurement teams should expect the SEAL-2 floor to do significant filtering before the award conversation begins.


For organizations not bound by EU procurement, the framework remains valuable as a structural lens. The eight Sovereignty Objectives offer a more rigorous decomposition of the sovereignty conversation than most internal procurement criteria. Asking the same questions in a U.S. federal, allied government, regulated enterprise, or critical infrastructure context produces a more defensible vendor evaluation than relying on certification count alone or on marketing claims about sovereignty. Headquarters jurisdiction is one critical input, assessed *relative to the customer's own jurisdiction* , but the SEAL decomposition makes clear that sovereignty assessment also requires answering questions about operations, keys, supply chain, exit cost, and demonstrated security. Each procurement authority sets its own legal floor relative to its own jurisdictional position and the operational questions then operate within whatever floor is set.


The core insight SEAL formalizes is that sovereignty is not a single attribute. It is a system of structural facts about who controls what, distributed across legal, operational, supply chain, technology, security, and environmental dimensions. Some of those dimensions (legal in particular) are floor conditions: failure on any one disqualifies the vendor regardless of strength elsewhere. Others contribute to a weighted assessment among qualifying vendors. Treating sovereignty as if it were a single score, or as if you could negotiate it by adding compensating contractual language, misunderstands how the framework works.


## What to Do Next


If your organization procures digital services where sovereignty matters, and increasingly that describes most regulated procurement, three steps are worth taking now:


1.


**Read the framework.** The framework is a public document. The contributing factors under each Sovereignty Objective hold the operative detail and they are more concrete than the headline scoring categories suggest.


2.


**Assess current vendors against the eight SOVs.** Not formally, and not as a procurement action, but as an internal exercise. Where do current vendors score? Where would they score SEAL-0, and what does that imply about future procurements?


3.


**Treat the floor as the strategic question.** A vendor that scores SEAL-0 on any single dimension will face increasingly difficult procurement conversations. The award score conversation matters less than the floor conversation and the floor conversation usually turns on legal exposure, supply chain origin, or operational autonomy, the factors that are hardest to retrofit.


SEAL is the most structurally honest sovereignty framework currently in production. Its value lies in the discipline it enforces about which vendor relationships can sustain sovereignty and which cannot. That discipline is portable and the questions it formalizes will outlive any particular weighting or threshold the EU eventually settles on.
