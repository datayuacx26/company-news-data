---
schema_version: "1.0.0"
document_id: "d7e150f8012ffeaedb65732cd9b0251e477513bbce7e6dcaffc09ddcb7a01b73"
company_key: "yc-medcrypt"
company: "MedCrypt"
source_id: "yc-medcrypt-news-import-56918f8bbce9"
canonical_url: "https://www.medcrypt.com/blog/enisas-updated-procurement-guidelines-are-about-to-make-medical-device-security-a-contract-requirement-not-a-nice-to-have"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T00:17:35.729282+00:00"
fetched_at: "2026-07-30T00:17:37.698507+00:00"
content_hash: "sha256:e59a8ed936cdf6dd5c4c9204fe5e6ad4d6e9e6df860da3d7678640d6b6f907a7"
---

# ENISA's Updated Procurement Guidelines Are About to Make Medical Device Security a Contract Requirement, Not a Nice-to-Have

ENISA, the EU Cybersecurity Agency, released updated procurement guidelines for the cybersecurity of hospitals and healthcare providers in July 2026 — a follow-up to its 2020 guidance. The timing isn't coincidental. It's the direct implementation piece of the Commission's January 2025 action plan on hospital cybersecurity, and it's designed to give hospitals a concrete, checklist-driven way to satisfy their NIS2 supply chain obligations at the exact point where those obligations are hardest to enforce: procurement.


It's guidance, not law — ENISA is explicit it doesn't override national regulations. But it enables hospitals and healthcare providers to implement laws and regulations, such as NIS2 obligations for risk management and incident reporting, that extend into how they buy. This document is ENISA operationalizing that legal reality into something a procurement team can build into an RFP.


‍


For medical device manufacturers and the hospitals that buy from them, this document turns abstract NIS2 risk-management language into specific contract requirements that purchasing departments can now incorporate in their tenders..


1. Why procurement is the pressure point


‍


The health sector absorbed 8% of all ransomware incidents tracked in ENISA's 2024 Threat Landscape report, and earlier ENISA data found only 27% of healthcare organizations have a dedicated ransomware defense program. ENISA's diagnosis is that legacy devices, thin security budgets, and sprawling multi-vendor supply chains are the structural reasons the sector keeps getting hit. NIS2 (Directive (EU) 2022/2555) already requires essential and important health entities to manage supply chain risk, but the directive itself doesn't tell hospitals how to realize these requirements in RFPs or contracts. That's the gap this guidance fills — and it does so by walking through the full procurement life cycle (plan, source, manage) with device-specific, auditable checklist items.


‍


2. What changes for medical devices specifically


‍


Medical devices/IVDs get their own dedicated use case in the guidance (a networked radiology fleet — CT and MRI machines — being procured by a large hospital), and it's the most fleshed-out of the three scenarios. Some of the concrete asks hospitals are now expected to write into device procurement:


‍


- Request the MDS2 (Medical Device Security Disclosure) form from every device supplier during procurement, and confirm bids show evidence of MDR alignment and standards like ISO 27001 or IEC 62304.
- Verify a UDI-DI (unique device identifier) is assigned and documented, with intended purpose and full variant/accessory descriptions.
- Confirm that devices are correctly classified under MDR Annex VIII (Class I/IIa/IIb/III) with justification, since that classification drives how much scrutiny the device gets.
- Require remote maintenance access to be VPN + MFA, session-limited, logged, and disabled by default when not in active use — no standing/permanent accounts.
- Require hardware sourced only from authorized manufacturers or certified distributors, with digital signatures or device certificates proving firmware integrity at first boot.
- Require software validated to IEC 62304, with a patch management process that includes testing on non-production samples and documented compensating controls for devices that can't be patched (a real issue for older imaging or life-support equipment).
- Build in supplier obligations for vulnerability disclosure and incident notification aligned to NIS2's 24-hour early-warning / 72-hour reporting clock.
- Plan for secure decommissioning — certified data sanitization and asset tracking so a retired device can't leak patient data or get quietly reused.


‍


None of these are new concepts to anyone who has followed MDR post-market surveillance or IEC 62443/62304 work. What's new is that ENISA is packaging them as procurement checklist items with explicit identifiers (e.g., requirements SOURCE-04.02, MANAGE-05.03) that a hospital's legal and IT teams can drop straight into tender documents and supplier scorecards.


‍


3. Why hospitals will actually adopt this


‍


Three things push adoption beyond "here's some good advice":


‍


1. Legal traceability: NIS2 requires essential/important entities to demonstrate risk-based supply chain management, and management bodies can be held accountable under Article 20 for failing to oversee it. A codified checklist gives hospital boards and CISOs something they can point to during an audit or after an incident: "we followed the ENISA-endorsed procurement measures relevant to this device class."
2. It's the default answer to an unresolved question: Most hospitals don't have in-house expertise to translate NIS2's principles-based language into device-specific contract clauses. ENISA has effectively done that translation for them, which lowers the cost of compliance to "adopt the checklist" rather than "build one from scratch."
3. It reallocates the burden to suppliers: Because the guidance tells hospitals exactly what to demand — MDS2 forms, UDI-DI documentation, IEC 62304 validation evidence, patch SLAs, incident notification clauses — device manufacturers that can't produce this documentation on request will simply lose bids. That market pressure is likely to move faster than regulatory enforcement alone.


‍


4. The practical takeaway for device manufacturers and hospitals


‍


If your device doesn't already ship with an MDS2 form, documented UDI-DI, IEC 62304 validation evidence, a real patch management commitment, and a defined incident-notification SLA, expect EU hospital tenders to start asking for exactly that — this document is likely to become the reference checklist procurement teams cite. The guidance also treats AI-enabled device functionality as a distinct disclosure category (model provenance, data flows, human oversight), so devices with embedded AI/ML features should expect additional documentation requests beyond the baseline MDS2-style disclosures.


‍


For hospitals, the message is similarly direct: NIS2 compliance for medical technology isn't going to be demonstrated primarily through firewalls and SOC dashboards — it's going to be demonstrated through what you required, and got, from suppliers before you signed the contract.


‍


5. Conclusion


‍


Taken together, these guidelines mark a shift from cybersecurity-as-afterthought to cybersecurity-as-contract-term in EU hospital procurement. ENISA hasn't created new legal obligations — NIS2, MDR, and GDPR already impose them — but it now has given hospitals a practical way to act and implement.


‍


With a ready-made checklist, explicit device-class use cases, and identifiers that map straight to tender language, adoption becomes the path of choice for procurement teams that are under audit pressure, and a competitive necessity for device manufacturers who want to keep winning EU hospital contracts. Most of this maps to practices manufacturers already invest in for MDR/IVDR and SDLC hygiene; where it goes further is making signed software delivery and end-of-life planning explicit, checkable procurement criteria tied to winning the deal.


‍


Expect this document to become the de facto reference for tenders within upcoming procurement cycles, with the manufacturers who can readily produce MDS2 forms, UDI-DI documentation, and IEC 62304 evidence gaining an edge over those who can't.


‍


---


Source: ENISA, "Procurement guidelines for the cybersecurity of hospitals and healthcare providers," July 2026


[https://www.enisa.europa.eu/sites/default/files/2026-07/Procurement%20guidelines%20for%20the%20cybersecurity%20of%20hospitals%20and%20healthcare%20providers.pdf](https://www.enisa.europa.eu/sites/default/files/2026-07/Procurement%20guidelines%20for%20the%20cybersecurity%20of%20hospitals%20and%20healthcare%20providers.pdf)


‍
