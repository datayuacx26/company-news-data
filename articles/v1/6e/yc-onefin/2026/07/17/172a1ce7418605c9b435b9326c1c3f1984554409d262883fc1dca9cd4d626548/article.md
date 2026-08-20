---
schema_version: "1.0.0"
document_id: "172a1ce7418605c9b435b9326c1c3f1984554409d262883fc1dca9cd4d626548"
company_key: "yc-onefin"
company: "OneFin"
source_id: "yc-onefin-rss-7ca07195f9bf"
canonical_url: "https://www.onefin.in/post/rbi-s-data-governance-draft-the-policy-is-the-easy-part"
published_at: "2026-07-29T08:42:48+00:00"
first_seen_at: "2026-07-29T08:49:45.190051+00:00"
fetched_at: "2026-07-29T08:49:45.980108+00:00"
content_hash: "sha256:9162e5984dcf4e8e1f1fac51bc12fd9ce4b1c449700362985233739b917ca2e5"
---

# RBI's Data Governance Draft: The Policy Is the Easy Part

The Reserve Bank of India released its draft


[Guidance on Regulatory Expectations for Data Governance](https://rbidocs.rbi.org.in/rdocs/PressRelease/PDFs/PR677DE9FC605996F417FB7FA3BC5C45A3F9D.PDF) on July 15. Comments from regulated entities are open until August 17. On paper, the ask sounds familiar: a Board-approved governance framework, defined roles, and documented policies. Lending executives who went through RBI's IT Governance Directions in 2023 might expect a similar exercise this time. That expectation would be wrong in one important way. This draft requires clear answers on the data: who owns it, where it lives, and whether every system that touches it can trace back to one authoritative source. Writing that policy is straightforward. Building the systems that make it true is not.


##


**What the Draft Actually Requires**


The Guidance applies to a wide set of regulated entities. Notably, it covers NBFCs across all four layers: Base, Middle, Upper, and Top. That is new ground. RBI's 2023 IT Governance Directions excluded Base Layer NBFCs entirely. This draft does not.


The requirements are direct:


-


**Board-level Data Governance Committee.** A new committee, or an existing committee formally assigned the role.


-


**Executive Data Governance Committee.** Cross-functional, responsible for implementation.


-


**Data Function.** Headed by an officer at Chief General Manager rank or equivalent.


-


**Four defined data roles.** Data Owner, Data Steward, Data Custodian, and the Data Function itself, each with distinct accountability across the data lifecycle, from origination to disposal.


Two provisions carry the most operational weight: the single source of truth requirement, and the rules governing data shared with third parties.


##


**Single Source of Truth: Where it Breaks Down**


Paragraph 43 sets the SSOT requirement: one designated authoritative source per data element, no parallel or competing sources, and every downstream system, model, and process derived from it.


Paragraph 44 allows any architecture, centralized, federated, or hybrid, as long as it delivers a clear authoritative source, consistent data across functions, and traceability of aggregated reports.


Paragraph 45 requires the SSOT designation, and any change to it, to be approved by the executive Data Governance Committee and documented.


Paragraph 46 is the one worth pausing on. It requires a reconciliation mechanism to identify and resolve inconsistencies between the SSOT and downstream data. The requirement is not that every system update in lockstep. It is that divergence gets caught, documented, and resolved through a governed process, rather than left to accumulate or get patched informally.


Inside most lending institutions, this is where the gap actually sits.


-


**Multiple systems derive different metrics from the same root data.** The Loan Management System tracks DPD. The Risk Engine derives an SMA classification from that DPD. Finance derives a provisioning figure from the same underlying status.


-


**The violation is the absence of a working reconciliation mechanism.** Paragraph 46 requires one. Most lenders may not have a formal, documented process that systematically catches when a derived figure has drifted from what the SSOT would produce.


-


**Manual overrides are the sharpest version of the problem.** A manual GL adjustment or a risk overlay applied outside the system, without flowing back through reconciliation or Data Owner approval, is exactly the kind of undocumented divergence paragraph 46 exists to prevent. This is a governance gap, not a timing problem.


The diagram below traces how this is supposed to work, and where it typically breaks in practice.


##


**Third-Party Data Doesn't Get a Pass**


Chapter VI of the draft extends the same logic outward. Paragraph 63 requires that data shared with third parties remain traceable to the RE's designated SSOT, with metadata and lineage capturing the extent of the sharing. Access must be need-to-know, agreements must include non-disclosure clauses, and third-party systems face periodic audits, including through CERT-IN empaneled auditors.


Co-lending arrangements are the clearest test case. Under typical co-lending or Loan Service Provider (LSP) structures, the fintech partner maintains its own transactional records for applications, collections status, and repayment schedules, while the regulated entity ingests updates through periodic API syncs or batch files.


-


**A sync interval is not, by itself, the problem.** Nothing in the draft requires instant synchronization with a partner's systems. It requires the lag, and the data crossing it, to be captured in metadata and lineage.


-


**The actual risk is untracked divergence.** If a repayment or address update sits in the partner's system without the RE's lineage recording that a change is pending, traceability breaks. That is the paragraph 63(i) requirement.


-


**The RE carries the accountability regardless.** Paragraph 60 makes this explicit: the RE is responsible for governance of data shared with third parties, including group entities. A partner's system design does not transfer that responsibility.


For lenders running FLDG-backed co-lending books at scale, this is where traceability claims either hold up under audit or do not.


##


**The Strategic Question**


Both provisions point to the same conclusion. RBI is not asking regulated entities to document good intentions. It is asking them to prove that a single, traceable version of the truth exists across systems that were built at different times, by different teams, often without governance in mind.


Base Layer NBFCs entering this regime for the first time do not get to treat it as a paperwork exercise: the standard is proportional to size, but the underlying architecture question, whether every system touching a data element can point to the same authoritative source, does not get easier just because the institution is smaller. The strategic question this draft raises is not whether NBFCs can write a compliant Data Governance Framework. Nearly all of them can. It is whether their loan origination, servicing, and risk systems were built to make that framework true.


##


**Where Platform Architecture Meets This Standard**


The gap between a documented Data Governance Framework and a defensible one usually sits in the plumbing. It comes down to whether origination, servicing, and collections data resolve to one source. And whether that source stays traceable through every downstream system and third-party integration.


-


**Unified data architecture** across origination, servicing, and collections limits how many systems can independently claim to be authoritative for the same data element.


-


**Built-in reconciliation and audit trails** capture the relationship between derived figures and their source at the point of creation. This supports the reconciliation mechanism the Data Function and Data Governance Committee are required to maintain.


-


**API-native integration** with co-lending and LSP partners captures sync timing and data flow in metadata. Traceability holds between sync cycles, not just at the moment of the last update.


OneFin's platform does not replace the governance decisions a Board and Data Governance Committee must make under this draft. It gives lenders infrastructure where those decisions, once made, can actually be enforced.


##


**Conclusion**


The comment window on this draft closes August 17, and the language may shift before it is finalized. What is unlikely to change is the underlying expectation. RBI has moved from asking regulated entities to secure their data, to asking them to govern it, with named owners, traceable lineage, and accountability that survives contact with third parties. For NBFCs, the practical test will not be the policy document submitted to the Board. It will be whether a DPD figure pulled from three different systems tells the same story. And whether it still does after the data crosses into a partner's hands.


To know more about OneFin


,


[schedule a Demo](https://www.onefin.in/request-demo) .
