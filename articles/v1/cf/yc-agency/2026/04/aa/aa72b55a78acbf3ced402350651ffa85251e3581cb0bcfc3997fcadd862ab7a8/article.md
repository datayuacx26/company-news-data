---
schema_version: "1.0.0"
document_id: "aa72b55a78acbf3ced402350651ffa85251e3581cb0bcfc3997fcadd862ab7a8"
company_key: "yc-agency"
company: "Agency"
source_id: "yc-agency-news-import-0d31f4b059c0"
canonical_url: "https://blog.getagency.com/articles/cmmc-compliance"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-27T08:21:07.883449+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:3dcdd936e40ef208745b05d0979ebf1ea9f2dec4e530004b134adc335584d158"
---

# CMMC Compliance: Your Complete Guide to the Certification Journey

*After guiding defense contractors through years of DFARS self-attestation and now CMMC preparation, we can say with confidence that the organizations who succeed are the ones who treat compliance as a structured journey — not a last-minute scramble before a contract deadline.*


The Cybersecurity Maturity Model Certification (CMMC) represents the most significant change to cybersecurity requirements for the defense industrial base (DIB) since the introduction of DFARS clause 252.204-7012 in 2017. Unlike the previous self-attestation model, CMMC introduces verified assessments — and for many contractors, third-party assessments — to ensure that organizations actually implement the cybersecurity controls they claim.


This guide focuses on the compliance journey: the process of understanding where your organization stands today, determining what level you need, building a plan to get there, and navigating the assessment process. For a detailed breakdown of the specific[requirements at each level](https://blog.getagency.com/articles/cmmc-requirements-explained) , we have a companion guide. Here, we focus on the practical steps, decisions, and timeline that define the path from where you are to where you need to be.


## Understanding the Three CMMC 2.0 Levels


CMMC 2.0 streamlined the original five-level model into three levels. Each level corresponds to a type of information you handle and a baseline of security controls you must implement.


### Level 1: Foundational


**Who it applies to:** Organizations that handle[Federal Contract Information (FCI)](https://blog.getagency.com/articles/what-is-fci) but do not handle[Controlled Unclassified Information (CUI)](https://blog.getagency.com/articles/what-is-cui) .


**What it requires:** 17 basic safeguarding practices derived from FAR clause 52.204-21. These are fundamental cybersecurity hygiene measures — access control, identification and authentication, media protection, physical protection, system and communications protection, and system integrity.


**Assessment type:** Annual self-assessment only. The organization conducts its own evaluation, and a senior official affirms the results in the Supplier Performance Risk System (SPRS). No third-party involvement is required.


**Practical reality:** Level 1 is achievable for most organizations that have basic IT security practices in place. The 17 practices include things like limiting system access to authorized users, using antivirus software, and keeping systems patched. If your organization has a functioning IT department, you are likely close to Level 1 already.


### Level 2: Advanced


**Who it applies to:** Organizations that handle CUI on DoD contracts.


**What it requires:** All 110 security controls from NIST SP 800-171 Rev 2, organized across 14 control families. This is a substantial step up from Level 1 — moving from 17 basic practices to 110 detailed controls covering everything from access control and audit logging to incident response and risk assessment.


**Assessment type:** This is where CMMC 2.0 introduced an important bifurcation. Level 2 has two assessment paths:


- **Self-assessment** — For contracts involving CUI that the DoD determines is not critical to national security. The organization conducts its own assessment and a senior official affirms the results.
- **C3PAO assessment** — For contracts involving CUI that the DoD determines is critical. A Certified Third-Party Assessment Organization conducts an independent assessment against all 110 controls.


The distinction between self-assessment and C3PAO assessment at Level 2 is determined by the specific contract, not by the contractor. The DoD decides, based on the sensitivity and criticality of the CUI involved, which assessment type is required. This means the same organization might need a self-assessment for one contract and a C3PAO assessment for another.


**Practical reality:** Level 2 is where most defense contractors will land, and it is where the real compliance work happens. Implementing 110 controls requires significant investment in technology, processes, documentation, and training. For a detailed walkthrough, see our[CMMC Level 2 compliance guide](https://blog.getagency.com/articles/cmmc-level-2-compliance) .


### Level 3: Expert


**Who it applies to:** Organizations that handle the most sensitive CUI on the highest-priority DoD programs.


**What it requires:** All 110 NIST 800-171 controls plus a subset of enhanced security requirements from NIST SP 800-172. The specific NIST 800-172 controls required for Level 3 are defined in the CMMC final rule.


**Assessment type:** Government-led assessment conducted by the Defense Contract Management Agency (DCMA) Defense Industrial Base Cybersecurity Assessment Center (DIBCAC). This is neither a self-assessment nor a third-party assessment — the government itself evaluates your security posture.


**Practical reality:** Level 3 applies to a relatively small number of organizations working on the most sensitive defense programs. If you are unsure whether you need Level 3, you almost certainly do not. The contracts that require Level 3 will make it explicitly clear.


## Determining Your Required Level


The first practical step in your CMMC compliance journey is determining which level you need. This is not a choice you make — it is determined by the information you handle and the contracts you hold or pursue.


### Decision Framework


Use this decision tree to identify your starting point:


1.


**Do you have any DoD contracts (prime or sub)?** If no, CMMC does not currently apply to you, though it may in the future as requirements expand.


2.


**Do any of your contracts involve CUI?** Check for DFARS clause 252.204-7012 and any CUI markings on government-furnished information. If no CUI — only FCI — you need Level 1.


3.


**Does the specific contract require a C3PAO assessment?** The solicitation or contract will specify. If self-assessment is sufficient, prepare for Level 2 self-assessment. If C3PAO assessment is required, prepare for Level 2 with third-party assessment.


4.


**Does the contract explicitly require Level 3?** This will be stated in the contract. If yes, prepare for Level 3 with government-led assessment.


### Common Scenarios


Organization Type Typical Data Likely CMMC Level


IT services provider (general support, no CUI access) FCI only Level 1


Janitorial/facilities contractor on a DoD base FCI only Level 1


Machine shop manufacturing parts to DoD specifications CUI (technical drawings, specifications) Level 2


Software developer building systems that process CUI CUI (technical data, system documentation) Level 2


Defense electronics manufacturer CUI (ITAR data, test results, specifications) Level 2


Major weapons system prime contractor Critical CUI (weapons specifications, vulnerability data) Level 2 or Level 3


Cleared defense contractor on special access programs Critical CUI + classified Level 3 (for unclassified CUI environment)


## The Phased Rollout Timeline


CMMC is not appearing in all DoD contracts at once. The Department of Defense designed a phased implementation to give the defense industrial base time to prepare and to allow the assessment ecosystem (C3PAOs, assessors) to scale.


### Phase 1 (Starting 2025)


Phase 1 begins when both the 32 CFR Part 170 rule (the CMMC program rule) and the 48 CFR DFARS rule (the contract clause) are in effect. During Phase 1:


- **Level 1 self-assessments** can appear as contract requirements
- **Level 2 self-assessments** can appear as contract requirements
- The DoD may include CMMC requirements in new solicitations at its discretion


This phase establishes the foundation. Organizations that have not yet conducted a self-assessment should treat Phase 1 as their activation signal.


### Phase 2 (Approximately 12 months after Phase 1)


Phase 2 adds:


- **Level 2 C3PAO assessments** as contract requirements
- Contracts that previously required only self-assessment may now require third-party assessment for new awards


This is the phase that most contractors are planning for. Once C3PAO assessments become contract requirements, organizations that have not achieved certification will be unable to bid on affected contracts.


### Phase 3 (Approximately 24 months after Phase 1)


Phase 3 adds:


- **Level 3 government-led assessments** as contract requirements
- Broader application of Level 2 C3PAO assessment requirements


### Phase 4 (Full Implementation)


Phase 4 represents full implementation, where:


- All DoD contracts involving FCI or CUI include the appropriate CMMC requirements
- CMMC is a standard element of all applicable solicitations
- The option period of existing contracts may also include CMMC requirements


### Planning Against the Timeline


What we tell clients: do not wait for your specific contract to require CMMC before starting preparation. The assessment ecosystem — particularly C3PAO availability — will be constrained, especially in the early phases. Organizations that begin preparation early will have access to more assessment slots and more time to remediate any gaps.


A realistic timeline for CMMC Level 2 C3PAO assessment readiness:


Phase Duration Activities


Gap analysis 4-8 weeks Assess current state against 110 NIST 800-171 controls


Remediation planning 2-4 weeks Prioritize gaps, budget, and schedule remediation


Remediation execution 3-12 months Implement controls, deploy technology, develop documentation


Internal assessment 4-6 weeks Conduct thorough self-assessment to verify readiness


C3PAO engagement 2-4 weeks Select and contract with a C3PAO


C3PAO assessment 2-4 weeks On-site and remote assessment by certified assessors


POA&M remediation (if needed) Up to 180 days Address any findings documented in a POA&M


Total elapsed time from start to certification: 6-18 months, depending on your starting maturity and the scale of remediation needed.


## The Self-Assessment Process


For Level 1 and certain Level 2 contracts, self-assessment is the required assessment type. While simpler than a C3PAO assessment, self-assessment is not a rubber stamp. It requires genuine evaluation, documentation, and senior official accountability.


### How Self-Assessment Works


1.


**Scope your environment** — Identify all systems, networks, and processes that handle FCI (for Level 1) or CUI (for Level 2)


2.


**Evaluate each practice/control** — For Level 1, assess your implementation of all 17 practices. For Level 2, assess all 110 NIST 800-171 controls. For each control, determine whether it is fully implemented, partially implemented, or not implemented.


3.


**Calculate your SPRS score** — For Level 2, each unmet control reduces your score from a perfect 110. The scoring methodology assigns point values to each control. Your score reflects your current security posture.


4.


**Document your findings** — Create a System Security Plan (SSP) documenting how you implement each control, and a Plan of Action and Milestones (POA&M) for any controls that are not fully met.


5.


**Senior official affirmation** — A senior official at your organization (typically a C-suite executive or equivalent) must affirm the accuracy of the assessment results. This affirmation carries legal weight — false statements can trigger False Claims Act liability.


6.


**Submit to SPRS** — Enter your assessment results into the Supplier Performance Risk System, the DoD's repository for contractor security assessments.


### The Accountability Factor


The affirmation requirement is the enforcement mechanism that gives self-assessment teeth. Under the CMMC final rule, the senior official who affirms the assessment results is personally accountable for the accuracy of that affirmation. This is not a pro forma signature. False or inaccurate affirmations can result in:


- **False Claims Act liability** — Treble damages and per-claim penalties
- **Contract termination** — Loss of the specific contract
- **Suspension or debarment** — Potential exclusion from all government contracting


What we tell clients: treat self-assessment with the same rigor you would bring to a third-party audit. The penalties for inaccurate self-assessment are severe, and the DoD has signaled its intent to hold senior officials accountable.


## The C3PAO Assessment Process


For Level 2 contracts where the DoD requires third-party assessment, you will engage a Certified Third-Party Assessment Organization (C3PAO) accredited by the Cyber AB (formerly the CMMC Accreditation Body).


### Selecting a C3PAO


The Cyber AB maintains a marketplace of accredited C3PAOs. When selecting a C3PAO, consider:


- **Availability** — Assessment slots will be limited, especially in early CMMC phases. Book early.
- **Industry experience** — Some C3PAOs specialize in specific industries or organization sizes
- **Assessment team composition** — Ensure the team includes Certified CMMC Assessors (CCAs) with relevant experience
- **Conflict of interest rules** — A C3PAO cannot assess an organization it has also consulted for. If you used a consultant for remediation, ensure they are not affiliated with your C3PAO.
- **Cost** — C3PAO assessment fees vary based on organization size, scope complexity, and the number of assessor-days required


### What to Expect During Assessment


A typical C3PAO assessment follows this structure:


**Pre-assessment (2-4 weeks before):**


- The C3PAO reviews your SSP, POA&M, network diagrams, and other documentation
- They identify areas that need further investigation
- They develop an assessment plan and schedule


**Assessment execution (1-3 weeks on-site/remote):**


- Assessors interview key personnel (IT staff, security team, system administrators, end users)
- They review evidence of control implementation (screenshots, configurations, logs, policies)
- They observe processes in action (incident response procedures, access provisioning, etc.)
- They test controls where possible (vulnerability scans, access control verification)


**Post-assessment:**


- The C3PAO compiles findings and determines a score
- If you meet all 110 controls, you receive certification
- If you have a limited number of unmet controls that qualify for POA&M, you may receive conditional certification with a 180-day remediation window
- If you have too many unmet controls or fail critical controls, you do not receive certification


### Conditional Certification and POA&Ms


The CMMC final rule permits conditional certification under specific circumstances. If your assessment reveals unmet controls that:


- Are documented in a[Plan of Action and Milestones (POA&M)](https://blog.getagency.com/articles/cmmc-poam-guide)
- Do not exceed the POA&M limitations defined in the rule
- Are not among the controls designated as non-POA&M-eligible


Then you can receive conditional certification while you remediate those gaps. You have 180 days from the conditional certification date to close all POA&M items. A C3PAO must verify the closure.


This is an important safety valve, but it should not be your plan. Organizations that go into assessment expecting to rely heavily on POA&Ms are taking a significant risk. Not all controls are POA&M-eligible, and exceeding the allowed number of POA&M items means no certification at all.


## Building Your Compliance Roadmap


Based on our experience helping organizations through this process, here is the structured approach we recommend:


### Phase 1: Discovery and Scoping (Weeks 1-6)


**Objective:** Understand your current state, define your CUI environment, and identify gaps.


Key activities:


- Inventory all DoD contracts and identify FCI/CUI requirements
- Map data flows to determine where CUI is processed, stored, and transmitted
- Define the boundary of your CUI environment (the systems in scope for CMMC)
- Conduct a gap assessment against the applicable NIST 800-171 controls
- Calculate a preliminary SPRS score
- Identify quick wins and major remediation projects


**Deliverables:** CUI data flow diagrams, gap assessment report, preliminary SPRS score, remediation priority list


### Phase 2: Remediation Planning (Weeks 6-10)


**Objective:** Build a realistic remediation plan with budget, timeline, and resource allocation.


Key activities:


- Prioritize gaps based on risk, cost, and assessment impact
- Evaluate technology solutions (SIEM, EDR, MFA, encryption, etc.)
- Determine whether to build controls in-house or leverage managed services
- Consider architectural approaches to reduce CUI scope (enclaves, cloud solutions)
- Develop a remediation project plan with milestones and accountability


**Deliverables:** Remediation project plan, technology procurement list, budget estimate, milestone schedule


### Phase 3: Remediation Execution (Weeks 10-40+)


**Objective:** Implement the controls, deploy the technology, and build the documentation.


Key activities:


- Deploy technical controls (access management, encryption, monitoring, etc.)
- Develop and implement policies and procedures
- Build the System Security Plan (SSP) — the comprehensive document describing your security program
- Conduct security awareness training
- Implement incident response procedures and test them
- Configure audit logging and review processes
- Establish continuous monitoring practices


**Deliverables:** Fully implemented control environment, complete SSP, trained workforce, operational security processes


### Phase 4: Assessment Preparation (Weeks 40-46)


**Objective:** Verify readiness and prepare for the formal assessment.


Key activities:


- Conduct a thorough internal assessment (mock assessment)
- Verify that all 110 controls are implemented and documented
- Prepare evidence packages for each control
- Brief key personnel on assessment procedures and expectations
- Identify and remediate any remaining gaps
- Select and engage a C3PAO (if required)


**Deliverables:** Internal assessment report, evidence packages, assessment-ready SSP, C3PAO contract


### Phase 5: Formal Assessment (Weeks 46-50)


**Objective:** Successfully complete the CMMC assessment.


Key activities:


- Support the C3PAO assessment team with access, evidence, and personnel
- Respond to assessor questions and requests for additional evidence
- Address any real-time findings where possible
- Receive assessment results and certification determination


**Deliverables:** CMMC certification (or conditional certification with POA&M)


## DFARS Clauses and Contract Requirements


Understanding how CMMC requirements appear in contracts is essential for compliance planning. Two regulatory frameworks work together:


### 32 CFR Part 170 (The Program Rule)


Published as the[CMMC final rule](https://blog.getagency.com/articles/cmmc-final-rule) in October 2024, 32 CFR Part 170 establishes the CMMC program itself: the levels, assessment types, requirements, and processes. This rule defines what CMMC is, but it does not put CMMC requirements into contracts.


### 48 CFR (DFARS) (The Contract Rule)


The DFARS rulemaking under 48 CFR creates the actual contract clause — DFARS 252.204-7021 — that makes CMMC a contractual requirement. This clause specifies the CMMC level and assessment type required for a given contract. Both rules must be in effect before CMMC can appear in contracts.


### Key DFARS Clauses for CMMC


Clause Purpose


DFARS 252.204-7012 Safeguarding Covered Defense Information — the existing requirement to implement NIST 800-171


DFARS 252.204-7019 Notice of NIST SP 800-171 DoD Assessment Requirements — requires SPRS score submission


DFARS 252.204-7020 NIST SP 800-171 DoD Assessment Requirements — provides for government assessment


DFARS 252.204-7021 CMMC Requirements — the new clause specifying CMMC level and assessment type


For current contracts, 252.204-7012 remains the primary cybersecurity clause. As CMMC phases in, 252.204-7021 will appear alongside or replace these clauses in new solicitations.


## Cost Considerations


CMMC compliance costs vary widely based on organization size, current maturity, and target level. Here are the cost categories to plan for based on our experience:


### Level 1


Cost Category Notes


Gap assessment Scope depends on environment size


Remediation (technology and process) Varies by current security posture


Documentation Internal or outsourced policy development


Annual self-assessment effort Internal labor cost


Level 1 investments are generally modest for organizations with basic IT security already in place. Contact advisors for current pricing based on your scope.


### Level 2


Cost Category Notes


Gap assessment Scales with organization size and complexity


Remediation (technology and process) The most variable component — driven by current maturity


SSP development and documentation Internal or outsourced


C3PAO assessment fee Contact accredited C3PAOs for current pricing


Ongoing compliance (annual) Tools, personnel, and monitoring costs


The wide range for Level 2 reflects the enormous variation in starting maturity. An organization with mature IT security practices, existing SIEM, and documented policies will spend far less than one starting from minimal security infrastructure. For detailed cost breakdowns, see our[CMMC certification costs guide](https://blog.getagency.com/articles/cmmc-certification-costs) .


### Strategies to Reduce Cost


1.


**Minimize your CUI boundary** — The fewer systems in scope, the fewer controls to implement and maintain. Architectural decisions that isolate CUI into a defined enclave dramatically reduce cost.


2.


**Leverage cloud services** — FedRAMP-authorized cloud services can inherit many NIST 800-171 controls, reducing the number of controls you must implement yourself.


3.


**Use managed security services** — For small and mid-size contractors, managed SIEM, managed EDR, and managed IT services can be more cost-effective than building capabilities in-house.


4.


**Start early** — Rushing compliance increases cost. Organizations with 12-18 months of preparation time make better technology decisions and avoid premium pricing for expedited services.


## Maintaining Compliance


CMMC certification is not a one-time event. The program requires ongoing compliance maintenance:


- **Annual affirmation** — A senior official must annually affirm continued compliance
- **Continuous monitoring** — Security controls must be monitored and maintained continuously
- **Triennial reassessment** — C3PAO assessments are valid for three years, after which reassessment is required
- **Change management** — Significant changes to your environment (new systems, network changes, cloud migration) may impact your compliance posture and should trigger a reassessment of affected controls


What we tell clients: build compliance into your operations, not as a separate project. Organizations that treat CMMC as a periodic assessment event inevitably fall out of compliance between assessments. Those that integrate CMMC controls into daily IT operations and governance maintain compliance naturally.


## Where to Start Today


Regardless of where your organization is on the compliance journey, these are the actions you should take now:


1. **Determine your CMMC level** — Review your contracts and identify whether you handle FCI, CUI, or both
2. **Conduct a gap assessment** — Understand where you stand against the applicable controls
3. **Calculate your SPRS score** — If you handle CUI, you should already have a score in SPRS under current DFARS requirements
4. **Build a remediation plan** — Prioritize gaps and allocate resources
5. **Monitor the rollout** — Track the 48 CFR DFARS rulemaking and phased implementation timeline
6. **Engage early with C3PAOs** — If you anticipate needing a third-party assessment, start conversations now while availability is less constrained


The organizations that approach CMMC compliance methodically — with clear scoping, realistic timelines, and sustained investment — are the ones that achieve certification efficiently and maintain it over time. The[CMMC requirements](https://blog.getagency.com/articles/cmmc-requirements-explained) are demanding, but they are achievable with the right approach.
