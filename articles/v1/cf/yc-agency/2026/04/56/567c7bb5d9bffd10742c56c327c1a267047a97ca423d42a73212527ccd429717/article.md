---
schema_version: "1.0.0"
document_id: "567c7bb5d9bffd10742c56c327c1a267047a97ca423d42a73212527ccd429717"
company_key: "yc-agency"
company: "Agency"
source_id: "yc-agency-news-import-0d31f4b059c0"
canonical_url: "https://blog.getagency.com/articles/nist-800-171-rev-3-transition"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-27T08:21:07.883449+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:ce814934598c10794b27dba8ac328fa7539034b97fc4a1208a9c155a5b312f85"
---

# NIST 800-171 Rev 3 Transition: What Is Changing and How to Prepare

*The transition from NIST 800-171 Rev 2 to Rev 3 represents the most significant restructuring of CUI protection requirements since the standard was first published. Organizations in the defense industrial base cannot afford to treat this as a simple version update -- it changes how controls are organized, how they are assessed, and how much flexibility organizations have in implementation. Here is what is changing and what you need to do about it.*


NIST Special Publication 800-171 defines the security requirements for protecting Controlled Unclassified Information (CUI) in nonfederal systems and organizations. Every company that handles CUI as part of a Department of Defense contract is required to implement these controls. Rev 2 has been the operative version since February 2020, and its 14 control families and 110 security requirements have become deeply embedded in defense contractor compliance programs, DFARS self-assessments, and CMMC preparation efforts.


Rev 3, finalized in May 2024, restructures the standard in ways that affect every organization subject to it. This guide walks through the control families, the specific changes in Rev 3, and the practical steps organizations should take to prepare for the transition.


## The Rev 2 Baseline: 14 Control Families and 110 Requirements


Before examining what is changing, it is worth establishing the Rev 2 baseline that most organizations are currently operating under.


### Rev 2 Control Family Summary


Control Family ID Number of Requirements


Access Control AC 22


Awareness and Training AT 3


Audit and Accountability AU 9


Configuration Management CM 9


Identification and Authentication IA 11


Incident Response IR 3


Maintenance MA 6


Media Protection MP 9


Personnel Security PS 2


Physical Protection PE 6


Risk Assessment RA 3


Security Assessment CA 4


System and Communications Protection SC 16


System and Information Integrity SI 7


**Total** **110**


Rev 2 organizes these 110 requirements into "basic" and "derived" categories. Basic requirements come directly from FIPS 200, while derived requirements provide additional specificity drawn from NIST SP 800-53. This distinction has caused confusion in practice -- many organizations have treated basic requirements as more important than derived ones, which was never the intent.


For a deeper dive into the current requirements, see our[NIST 800-171 compliance guide](https://blog.getagency.com/articles/nist-800-171-compliance-guide) .


## What Is Changing in Rev 3


### Structural Reorganization


The most visible change in Rev 3 is the elimination of the basic/derived requirement distinction. All requirements are now treated equally, which resolves the prioritization confusion that plagued Rev 2 implementations. Requirements are restructured to align more directly with NIST SP 800-53 Rev 5 controls, creating a cleaner traceability path between the two standards.


The 14 control families are retained in name but the requirements within them have been reorganized, consolidated, and in some cases expanded. The total count of distinct requirements changes, though the exact number depends on how you count consolidated versus net-new requirements.


### Key Changes by Control Family


Control Family What Changed in Rev 3


Access Control (AC) Consolidated overlapping requirements; added explicit controls for system use notification and remote access management; clarified wireless access restrictions


Awareness and Training (AT) Expanded to include role-based training requirements and training content for specific threat scenarios


Audit and Accountability (AU) Enhanced requirements for audit record content, centralized log management, and audit review frequency


Configuration Management (CM) Added software allow-listing requirements; strengthened change management controls


Identification and Authentication (IA) Strengthened multi-factor authentication requirements; added device identification and authentication


Incident Response (IR) Expanded to include incident handling, monitoring, and reporting requirements; added incident response testing


Maintenance (MA) Consolidated maintenance controls; clarified requirements for remote and nonlocal maintenance


Media Protection (MP) Refined media sanitization and disposal requirements; updated for cloud and virtual media


Personnel Security (PS) Added personnel transfer and termination procedures; expanded screening requirements


Physical Protection (PE) Consolidated physical access controls; added visitor management requirements


Risk Assessment (RA) Added vulnerability scanning requirements; expanded risk assessment scope and frequency


Security Assessment (CA) Added continuous monitoring requirements; expanded plan of action and milestones management


System and Communications Protection (SC) Added boundary protection specificity; enhanced cryptographic requirements; added segmentation controls


System and Information Integrity (SI) Expanded malicious code protection; added security alerts and advisories; enhanced flaw remediation


### Organization-Defined Parameters (ODPs)


The single most significant functional change in Rev 3 is the introduction of Organization-Defined Parameters. ODPs are placeholders within control requirements where organizations must specify values appropriate to their environment and risk profile.


Examples of ODPs include:


- **Frequency of access reviews.** Rather than a fixed requirement, Rev 3 allows organizations to define the review frequency based on their risk assessment (for example, quarterly, semi-annually, or annually).
- **Audit event types.** Organizations define which events must be logged based on their specific threat landscape and data sensitivity.
- **Incident response timeframes.** Instead of a single mandated timeline, organizations specify their own incident notification and escalation windows.
- **Account inactivity thresholds.** Organizations define how long an account can remain inactive before automatic disablement.
- **Session lock and termination timeouts.** Organizations set their own thresholds for session inactivity.


What we tell clients is that ODPs are simultaneously the best and most challenging aspect of Rev 3. They provide the flexibility that Rev 2 lacked, but they also mean that organizations cannot simply check boxes anymore. You must justify your ODP selections based on your risk assessment, and assessors will evaluate whether your selections are reasonable.


### Alignment with NIST SP 800-53 Rev 5


Rev 3 strengthens the traceability between 800-171 requirements and 800-53 Rev 5 controls. Each Rev 3 requirement maps directly to one or more 800-53 controls, with a formal mapping table provided in the publication. This has several practical benefits:


- **Easier gap analysis** for organizations already implementing 800-53 controls in other contexts (such as FedRAMP)
- **Clearer assessment criteria** because assessors can reference the 800-53 control catalog for implementation guidance
- **Better alignment with CMMC** assessment objectives, which are being updated to reference Rev 3


### New and Enhanced Requirements


Several areas receive materially new or significantly enhanced requirements in Rev 3:


**Supply chain risk management.** Rev 3 adds requirements for identifying and managing supply chain risks related to CUI processing. This includes requirements for system component inventory management and provenance tracking.


**System and service acquisition.** New requirements address security considerations in the system development lifecycle and third-party service agreements.


**Planning.** Rev 3 adds a planning control family that requires organizations to develop, document, and maintain system security plans that address the full Rev 3 requirement set.


**Privacy.** Enhanced requirements address privacy considerations for CUI that contains personally identifiable information, creating alignment with federal privacy requirements.


## The DFARS and CMMC Transition Timeline


### Current State


As of early 2026, the defense industrial base operates under DFARS clause 252.204-7012, which references NIST SP 800-171 as the baseline for protecting CUI. Most organizations have self-assessed against Rev 2 and submitted scores to the Supplier Performance Risk System (SPRS).


CMMC 2.0 rulemaking has progressed with the final CMMC rule published in late 2024. CMMC Level 2 assessments are tied directly to NIST 800-171 requirements, and the transition to Rev 3 as the referenced version is expected to follow a phased approach.


### Expected Timeline


Milestone Expected Timing Impact


NIST 800-171 Rev 3 published (final) May 2024 (complete) Organizations can begin voluntary transition


NIST SP 800-171A Rev 3 (assessment procedures) Published alongside Rev 3 Defines how Rev 3 requirements will be assessed


DFARS clause update to reference Rev 3 Mid-to-late 2026 Contractual obligation shifts to Rev 3


CMMC assessment alignment with Rev 3 Late 2026 to early 2027 C3PAO assessments will evaluate against Rev 3


Rev 2 sunset Expected 12-18 months after DFARS update Rev 2 self-assessments no longer accepted


### Transition Period Considerations


The Department of Defense has historically provided transition periods when updating DFARS cybersecurity requirements. Based on past precedent and current rulemaking signals, we expect:


- A **grace period** of 12 to 18 months between the DFARS clause update and mandatory enforcement
- **Dual acceptance** of Rev 2 and Rev 3 self-assessments during the transition
- **Updated SPRS scoring** methodology to reflect the Rev 3 control structure
- **CMMC assessment flexibility** during the transition, with assessors accepting either version for a limited period


For details on how CMMC and NIST 800-171 interact, see our guide on[CMMC vs NIST 800-171](https://blog.getagency.com/articles/cmmc-vs-nist-800-171) .


## Practical Steps to Prepare for the Transition


### Step 1: Conduct a Rev 2 to Rev 3 Mapping


Start by mapping your current Rev 2 implementation to the Rev 3 requirement structure. NIST provides a formal mapping table in the Rev 3 publication. For each Rev 2 requirement, identify:


- Whether it maps directly to a Rev 3 requirement (most do)
- Whether it has been consolidated with other requirements
- Whether additional implementation is needed to satisfy the Rev 3 version
- What ODPs need to be defined


### Step 2: Define Your ODP Selections


This is the most labor-intensive aspect of the transition. For each requirement that includes ODPs, you must:


1. Review the ODP placeholder and understand what value is needed
2. Assess your current implementation to determine what value you are effectively operating at today
3. Evaluate whether your current value is reasonable given your risk profile
4. Document and justify each ODP selection in your System Security Plan


In our experience, organizations that have mature risk assessment programs find ODP definition relatively straightforward. Those without robust risk assessments will need to invest in that capability before they can credibly define ODPs.


### Step 3: Address Net-New Requirements


Identify the requirements in Rev 3 that have no direct equivalent in Rev 2. These are the areas where new implementation work is required. Common gaps include:


- **Supply chain risk management** processes and documentation
- **System development lifecycle** security requirements for custom-developed systems
- **Enhanced incident response** testing and coordination procedures
- **Vulnerability scanning** at defined frequencies (an ODP)
- **Software allow-listing** implementation


### Step 4: Update Your System Security Plan


Your SSP must be restructured to reflect the Rev 3 organization. This means:


- Reorganizing the plan to follow the Rev 3 requirement structure
- Documenting all ODP selections with justification
- Updating control implementation descriptions where requirements have changed
- Adding new sections for net-new requirement areas
- Ensuring the SSP accurately reflects your current operational state


### Step 5: Update Your SPRS Score


Once you have mapped and assessed against Rev 3, calculate your new SPRS score using the Rev 3 methodology. The scoring approach changes to reflect the restructured requirements, and your Plan of Action and Milestones (POA&M) items should be updated to reference Rev 3 requirement identifiers.


### Step 6: Prepare for CMMC Assessment Against Rev 3


If you are planning for a CMMC Level 2 assessment, work with your C3PAO to understand their timeline for transitioning to Rev 3 assessment objectives. Key preparation activities include:


- Reviewing the updated CMMC assessment guide when published
- Ensuring your evidence artifacts align with Rev 3 requirement descriptions
- Conducting a mock assessment against Rev 3 requirements
- Addressing any POA&M items identified during the mapping


For a full breakdown of CMMC requirements, see our[CMMC requirements explained](https://blog.getagency.com/articles/cmmc-requirements-explained) guide.


## Common Transition Challenges


### ODP Justification


The most common challenge we see is organizations struggling to justify their ODP selections. "We've always done it this way" is not a sufficient justification. Assessors will expect ODP values to be traceable to your risk assessment. If you define a 90-day vulnerability scanning frequency, you need a documented rationale for why 90 days is appropriate given your threat environment and data sensitivity.


### Documentation Debt


Organizations that have accumulated documentation debt under Rev 2 -- operating with outdated SSPs, incomplete POA&Ms, or missing policy documents -- will find the Rev 3 transition significantly more painful. The restructured requirements expose documentation gaps that may have been hidden under Rev 2's less precise language.


### Scope Creep


Rev 3's expanded requirement areas (supply chain, system development, enhanced planning) may pull additional systems and processes into scope that were previously not considered part of the CUI environment. Organizations should carefully evaluate whether their CUI boundary needs to be updated.


### Resource Constraints


Defense contractors, particularly small and mid-sized businesses, face the challenge of transitioning while maintaining current Rev 2 compliance and potentially preparing for CMMC assessments. We advise clients to treat the transition as an integrated program rather than three separate efforts.


## Rev 3 Transition Checklist


Use this checklist to track your transition progress:


- Obtain and review the final NIST SP 800-171 Rev 3 publication
- Obtain and review NIST SP 800-171A Rev 3 assessment procedures
- Complete Rev 2 to Rev 3 requirement mapping
- Identify all ODP placeholders across Rev 3 requirements
- Define and document ODP selections with risk-based justification
- Identify net-new requirements with no Rev 2 equivalent
- Perform gap assessment against net-new requirements
- Develop implementation plan for identified gaps
- Update System Security Plan to Rev 3 structure
- Update Plan of Action and Milestones to reference Rev 3
- Calculate updated SPRS score
- Conduct internal assessment against Rev 3 requirements
- Update evidence repository to align with Rev 3 assessment procedures
- Engage C3PAO or assessor to confirm Rev 3 readiness (if pursuing CMMC)


## Conclusion


The NIST 800-171 Rev 3 transition is not optional for organizations handling CUI. While the timeline provides some runway, the scope of changes -- particularly around ODPs and net-new requirements -- demands early action. Organizations that begin their mapping and gap assessment now will be positioned to transition smoothly when DFARS and CMMC formally adopt Rev 3. Those that wait risk a compressed timeline with limited assessor availability and potential gaps in their compliance posture during the transition window.


The Rev 3 changes ultimately make the standard more flexible and more aligned with how modern organizations actually implement security controls. The investment in transitioning pays dividends in a compliance program that is better tailored to your organization's actual risk profile.
