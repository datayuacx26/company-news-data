---
schema_version: "1.0.0"
document_id: "91df98932971b6136879d03bf49e80a79d7c53826f54c4da659052661fd12322"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/what-real-sovereignty-looks-like"
published_at: null
first_seen_at: "2026-07-24T03:44:24.727364+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:b0bede0860fee6d1a29db3d53b5089c0f460fd4f167947d0322ab1f5504fcb5a"
---

# What Real Sovereignty Looks Like in the Architecture: The Design Choices that Either Deliver It or Preclude It

Most sovereignty conversations focus on what vendors say about their products. The more useful conversation focuses on what vendors have built.


Architecture is harder to fake than marketing. The design decisions a vendor made years before sovereignty became a procurement requirement often determine whether sovereignty can be delivered in practice or only described in a contract.


Some design choices preserve customer control. Others give it away. Once a system is built around a particular set of choices, the resulting sovereignty posture cannot be retrofit with contractual addenda or jurisdictional shell games. The architecture is the answer.


## The Cryptographic Key Question Is the Cleanest Test


The single clearest architectural test of sovereignty is who holds the cryptographic keys. Not who is contractually committed not to use them or who has agreed not to disclose them under normal circumstances, but who possesses them.


Two architectures produce two materially different sovereignty postures.


**Vendor-held keys.** The vendor, as the operator of the service, holds the keys that protect customer data. The vendor commits, contractually, not to use those keys for purposes beyond service delivery. The customer's protection is the contract, the vendor's policies, and the vendor's operational integrity.


**Customer-held keys.** The customer holds the keys in a customer-controlled key management infrastructure. The vendor, as the operator of the service, has no technical capability to access the protected data, even if compelled by legal authority, even if internally compromised, even if the contract is terminated.


In the first model, the customer's data confidentiality is conditional on the vendor's continued cooperation, integrity, and resistance to compulsion. In the second, it is conditional on the customer's own key management.


This matters most under adversarial conditions: legal compulsion, vendor compromise, contract dispute, or geopolitical disruption. In normal operations, the two architectures look identical. Under stress, they diverge sharply. A vendor holding the keys can be compelled to disclose data by a court order in their jurisdiction, by a government compulsion regime, or by a sufficiently sophisticated breach of their key management infrastructure. A vendor who has never held the keys cannot disclose what they cannot access.


For sovereignty-sensitive customers, this single architectural choice does more for sovereignty than most contractual language combined. And it is a choice that must be made early in the system's design. Bolting customer-held keys onto a vendor-held-keys architecture is not a refactor but a rewrite.


## Deployment Topology Determines Operational Sovereignty


The second architectural test is whether the system can run independently of the vendor's cloud infrastructure.


A SaaS-only architecture, where the customer subscribes to a vendor-operated cloud service, gives the customer convenience and gives the vendor permanent operational authority. The customer cannot operate the system. Only the vendor can. The customer's continuity depends on the vendor's continuity. The customer's compliance depends on the vendor's compliance posture. The customer's geographic data placement depends on what the vendor offers as configuration options within their cloud. The customer is structurally a tenant and operational sovereignty has been transferred to the vendor.


A deployment topology that includes on-premises, customer-operated configurations, where the customer can run the system in their own infrastructure, with their own operations team, in their own jurisdiction, on their own hardware, produces a structurally different sovereignty posture. The customer can isolate the system from the public internet entirely. The customer can run the system through periods of connectivity disruption. The customer can subject the system to their own audit, their own change control, and their own governance. The vendor delivers the software. The customer operates it.


The most sovereignty-preserving deployments go further still. Air-gapped configurations, where the system runs in a customer environment with no connection to vendor infrastructure for any operational purpose, represent the maximum operational control a vendor can offer. The customer is operationally independent. Updates flow through controlled channels the customer manages. Telemetry, if any, flows through customer-controlled paths. The vendor's continued existence is not a precondition for the customer's continued operation.


This range of deployment topology, from pure SaaS at one end to fully air-gapped at the other, is the operational sovereignty axis. Most real sovereignty conversations are about where on this axis a particular customer needs to sit and whether the vendor's architecture supports that position.


A vendor whose architecture only supports SaaS, with no on-premises deployment option, has


fundamentally limited the customer's operational sovereignty regardless of how rigorous their cloud is. A vendor whose architecture supports the full range, SaaS for customers who want convenience, on-premises for customers who need operational control, and air-gapped for customers operating in classified or critical environments, gives the customer the choice of sovereignty posture. The architectural difference is decades of investment, not a configuration toggle.


## The Operating-Entity Question Separates Marketing from Substance


Beyond the technical architecture, there is an organizational architectural choice that is often more decisive: who is the actual operating entity for sovereignty-sensitive customers and how much operational substance does that entity carry?


Before this question matters, a different question must be answered: does the vendor's parent-company jurisdiction clear the customer's legal floor? Sovereignty assessment is conducted by some authority, a procurement office, a regulator, or a board, whose jurisdiction defines what counts as foreign legal exposure. The operating-entity test is most useful for vendors whose parent jurisdiction is acceptable to the customer's framework. Where the parent's jurisdiction creates legal exposure that is foreign to the customer (US-headquartered vendors selling into EU sovereignty procurement is the most discussed example, but the same logic operates symmetrically the other way for any cross-border procurement), the operating-entity question is downstream of a floor that has already been failed. Operational substance does not change foreign-jurisdiction legal exposure that the parent retains by virtue of incorporation.


The operating entity often reveals whether sovereignty exists in practice or only on paper. A common pattern is for a vendor headquartered in one jurisdiction to establish a subsidiary in a sovereignty-sensitive jurisdiction (typically the EU, increasingly other regions). The subsidiary is presented as the local sovereign anchor. Customers contract with the subsidiary. The subsidiary holds the local certifications. The subsidiary is named in marketing materials.


The question that distinguishes substance from facade is: what does the subsidiary actually do?


Substance-thin subsidiaries exist primarily for contracting and compliance. They employ a small staff, mostly in commercial and account-management roles. Engineering happens at the parent. Build infrastructure runs in the parent jurisdiction. Software updates originate from parent-operated pipelines. The subsidiary signs the contract. The parent operates the service. If the parent were unavailable, the subsidiary could not continue to operate. The subsidiary is a juridical wrapper, not an operating entity.


Substance-rich subsidiaries carry meaningful operational weight. They employ engineering staff who contribute to the codebase that ships to local customers. They operate build infrastructure in the local jurisdiction, with local controls. They control the deployment and update channels for local-variant deployments. They have the technical and contractual standing to continue operating the service for local customers if the parent were unavailable. The subsidiary is not just contracting. It is operating.


The distinction is rarely visible in marketing. Both kinds of subsidiaries describe themselves the same way. The distinction is visible in operational questions: where do the engineers sit, where does the build pipeline run, what is the source of truth for updates that ship to government customers, and what would happen if the parent were no longer available.


For customers serious about sovereignty, these are the questions that distinguish a vendor with a real sovereign operating entity from a vendor with a corporate-structure facade.


And it cannot be built overnight. Building a substance-rich subsidiary takes years of investment in engineering capacity, build infrastructure, certification effort, and operational autonomy. A vendor that has not made that investment by the time sovereignty becomes a procurement criterion cannot manufacture it on contract terms.


## Certification Breadth as Evidence of Operational Integrity


Certifications are often discussed in marketing terms as a list of logos that imply legitimacy.


The important question is not how many certifications a vendor has, but what those certifications actually validate.


Operations-validating certifications are the more interesting category for sovereignty assessment. They include the following.


**Certifications that validate operational integrity in classified deployment.** When a national security authority accredits a system for classified use, the accreditation extends to the operational substance, not just the architectural design. It covers the system as deployed, with its operating procedures, its update model, its access controls, and its operational supply chain. NATO Restricted accreditation, NSA Commercial Solutions for Classified guidance, BSI federal-level certifications, and equivalent allied accreditations carry this weight.


**Certifications that validate continuous operations.** Continuous monitoring frameworks like FedRAMP Class D (High) require ongoing evidence of operational security, not point-in-time evaluation. SOC 2 Type 2 attests that controls operate effectively over an evaluation period, not just that they exist on paper. ISO 22301 specifically certifies business continuity operations.


**Certifications that validate cryptographic and platform security.** Common Criteria evaluation at higher Evaluation Assurance Levels validates the cryptographic and platform-security claims of a product through independent third-party laboratory testing.


The volume of certifications a vendor holds is less interesting than the question of which certifications they hold and what each one validates. A vendor with a long list of architecture-only certifications is structurally different from a vendor with a smaller list of operations-validating certifications across multiple jurisdictions.


The most sovereignty-relevant pattern is multi-jurisdictional operational certification: certifications from multiple allied governments, validating operations in classified or sensitive deployments, held simultaneously across an integrated platform. This pattern is rare and difficult to achieve. It requires ongoing operational engagement with multiple national security authorities, each with their own evaluation processes, threat models, and operational standards. A vendor that holds NATO-level operational accreditation, US classified-grade certification, and allied-government federal certifications across the same product platform has demonstrated operational integrity in a way that no single certification could.


This kind of certification portfolio rarely appears by accident. It reflects years of engagement with national security customers, repeated evaluation cycles, and an architecture stable enough to satisfy multiple authorities simultaneously.


## Supply Chain Transparency as a Structural Property


The most heavily weighted sovereignty objective in modern frameworks is supply chain. The reason is that supply chain is the most durable test of sovereignty. Legal status can change with a treaty. Operational footprint can be relocated. Marketing claims can be updated. Where software is architected, built, and shipped from is harder to refactor.


The architectural pattern that supports supply chain sovereignty is structured transparency. Specifically:


**Build infrastructure in customer-relevant jurisdiction.** For deployments to a sovereignty-sensitive customer, the build pipeline that produces the software running in that customer's environment should run in jurisdiction-appropriate infrastructure. This is a separate question from headquarters jurisdiction, which is a legal-floor input addressed elsewhere in any rigorous assessment. The build-pipeline question is about where the build artifacts are produced and signed, who has access to that pipeline, and what jurisdictional protection applies to the artifacts themselves. Both questions are required. A vendor that satisfies one without the other has a partial answer.


**Hardware sovereignty for cryptographic anchors.** The HSMs (hardware security modules) that anchor the cryptographic infrastructure of sovereignty-sensitive deployments should be sourced from suppliers whose own jurisdiction supports the customer's sovereignty requirements. EU-jurisdiction HSMs (Thales France, Utimaco Germany, and similar) are structurally different from non-EU-jurisdiction HSMs even when the technical specifications are equivalent.


**Source code escrow and audit access.** Customers in classified-grade deployments often require the ability to inspect or escrow source code as a condition of trust. A vendor whose architecture supports source code escrow under government certified-customer programs gives the customer the option of independent verification. A vendor whose architecture does not support this, typically because the codebase is too tightly integrated with proprietary cloud infrastructure to be meaningfully escrowed, has constrained customer options.


**OpenChain or equivalent supply chain compliance.** OpenChain (ISO/IEC 5230) compliance is a structured framework for software supply chain transparency. Vendors with OpenChain compliance have demonstrated through external audit that their supply chain practices meet a defined standard. This is structural evidence of supply chain discipline.


These patterns are architectural, not contractual. A vendor either has built their supply chain to be inspectable or they have not. If they have, sovereignty-sensitive customers can engage. If they have not, no contract can produce supply chain transparency that does not exist in the architecture.


## The Vendor Exit Question that Few Address


Sovereignty discussions usually focus on the entry point of a vendor relationship: how the vendor is selected, how their sovereignty posture is evaluated, and what protections are in place at signing. The exit point is rarely addressed with the same rigor, but it is structurally just as important to the customer's long-term sovereignty.


The architectural questions that determine exit cost are the following.


**Are protocols open or proprietary?** A vendor using open, standards-based protocols has structurally enabled the customer to migrate to another vendor that supports the same standards. A vendor using proprietary protocols has structurally locked in the customer to whatever extent the proprietary protocols are integrated into the customer's operations.


**Are data formats portable?** Customer data stored in vendor-proprietary formats, with no documented export path or standard equivalent, is


inaccessible in practice. Customer data stored in open formats, or with documented export paths, is portable.


**Are integrations replaceable?** A vendor whose value to the customer depends on tightly coupled integrations with the vendor's other products has structurally created a switching cost that increases over time as more integrations are added. A vendor whose product is interoperable with third-party alternatives has not.


**Is the architecture extractable?** In the most sovereignty-conscious deployments, customers may need the ability to extract the entire system, software, data, configurations, and operational procedures and continue operating it independently. Vendors whose architecture supports this (through licensing arrangements, source code provisions, or open architectures) give the customer real exit sovereignty. Vendors whose architecture cannot support this have structurally retained authority over the customer's continued operation.


A vendor optimized for customer sovereignty designs for exit as a first-class case. A vendor whose business model depends on lock-in does not. The customer's long-term sovereignty depends on which kind of vendor they have engaged.


## The Structural Pattern, in Summary


The customers who have been buying sovereignty-grade systems for the longest time —national security agencies, defense ministries, and classified communications operators —have, over time, settled on a structural pattern that defines what real sovereignty looks like in the architecture. The pattern starts with a jurisdictional floor and then builds out from there. The legal-floor question asks whether the vendor's parent jurisdiction creates legal compulsion exposure foreign to the customer's own jurisdiction. If it does, the architectural strengths below are downstream of a floor that has already been failed for that customer's specific assessment. Within that floor, the pattern includes:


-


Customer-held cryptographic keys by architecture, not by promise


-


Multi-modal deployment topology including on-premises and air-gapped options


-


Operating entities in customer-relevant jurisdictions that carry real engineering and operational substance


-


Multi-jurisdictional operational certifications validating the system in deployment


-


Inspectable supply chains with build-pipeline transparency, hardware sovereignty for cryptographic anchors, source code provisions, and supply chain compliance frameworks


-


Open protocols and portable data formats that preserve customer exit sovereignty


Vendors that meet the customer's legal floor and have built their products around this pattern can deliver sovereignty as an operational reality. Vendors that fail the legal floor cannot deliver sovereignty for that customer regardless of architectural strength on the other dimensions because the floor sits above the architectural layer. Vendors that clear the legal floor but have built their products around alternative patterns, vendor-held keys, SaaS-only deployment, substance-thin local subsidiaries, single-jurisdiction certifications, opaque supply chains, and proprietary lock-in also cannot deliver sovereignty as an operational reality regardless of how much sovereignty marketing they apply on top.


The architectural choices were made years ago. The sovereignty conversation that is now arriving in procurement is, in effect, surfacing both the jurisdictional positions of vendors and the architectural decisions they made long before sovereignty became the procurement vocabulary. Customers serious about sovereignty are increasingly looking past the marketing and asking the structural questions in sequence: what is the parent jurisdiction relative to ours, who holds the keys, where can the system run, who actually operates the local entity, what does each certification validate, where does the build pipeline live, and can we leave.


Customers serious about sovereignty are increasingly asking a different set of questions: who holds the keys, where can the system run, who actually operates it, what do the certifications validate, where does the build pipeline live, and can we leave? The answers reveal far more than any sovereignty claim on a website.


**Related:**
**[Sovereignty Gets a Score: What SEAL Is and Why It Matters Beyond Cloud](https://www.blackberry.com/en/secure-communications/insights/blog/sovereignty-effective-assurance-levels-explainer)**
**[Sovereignty Stops Being Optional: How Governments and Enterprises Should Think About Control](https://www.blackberry.com/en/secure-communications/insights/blog/global-sovereignty-frameworks-guide)**
