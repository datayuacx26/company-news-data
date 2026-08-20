---
schema_version: "1.0.0"
document_id: "fb51f230055cbc36ec3af7718a7fd273e305a1a7cb3ada45973773cf02a0d38a"
company_key: "yc-agency"
company: "Agency"
source_id: "yc-agency-news-import-0d31f4b059c0"
canonical_url: "https://blog.getagency.com/articles/90-day-soc-2-type-i-preparation-plan"
published_at: "2026-02-24T00:00:00+00:00"
first_seen_at: "2026-07-21T05:05:48.945011+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:26322d2293a970becb4427634a2c650b93fe89584fba616a1d407f338c7f5b3f"
---

# 90-Day SOC 2 Type I Preparation Plan

We've guided dozens of companies through ninety-day SOC 2 Type I sprints, and the pattern is consistent: a SOC 2 Type I audit can be completed within ninety days from start to report delivery if you follow a structured preparation plan. This guide divides the ninety days into three phases — Foundation (Days 1-30), Implementation (Days 31-60), and Validation (Days 61-90) — with specific weekly milestones, deliverables, and ownership assignments for each phase. The plan assumes you are starting with some existing security practices (SSO, cloud infrastructure, code reviews) but have not yet formalized them into a SOC 2-compliant program.


This playbook is designed for compliance managers and engineering leads who have a firm audit date and need an immediately actionable week-by-week schedule. To gauge how much ground you need to cover before Day 1, run through our free[SOC 2 Readiness Checklist](https://getagency.com/lp/soc-2-readiness-checklist) . Every task has a defined timeline, a clear owner, and a specific deliverable.


## Before Day 1: Pre-Work


Before the ninety-day clock starts, we recommend completing these prerequisite decisions:


- **Confirm scope** : Security criterion only (we recommend this for first audits) or Security plus additional criteria
- **Select your GRC platform** : Vanta, Drata, Secureframe, Sprinto, or another platform. Platform selection cannot wait — it must be done before Day 1.
- **Engage your auditor** : Have at least a verbal commitment and preliminary scheduling for audit fieldwork starting around Day 70-75
- **Identify your compliance lead** : The person who will own this plan and drive it to completion


## Phase 1: Foundation (Days 1-30)


Phase 1 establishes your compliance infrastructure — the platform, integrations, policies, and team structure that everything else builds upon.


### Week 1 (Days 1-7): Platform Setup and Integration


Task Owner Deliverable


Complete GRC platform onboarding questionnaire Compliance Lead Platform configured with company profile


Connect cloud provider accounts (AWS, Azure, GCP) Engineering Cloud integrations active, initial scan complete


Connect identity provider (Okta, Google Workspace, Entra ID) IT / Engineering Identity integration active, user roster imported


Deploy endpoint agent to compliance lead and engineering team IT Agent running on pilot group devices


Run initial compliance scan Compliance Lead Gap report generated with prioritized findings


**Day 7 checkpoint:** GRC platform is active with cloud and identity integrations connected. You have a gap report showing what needs to be addressed.


### Week 2 (Days 8-14): Expand Integrations and Begin Policies


Task Owner Deliverable


Connect code repositories (GitHub, GitLab, Bitbucket) Engineering Branch protection and code review evidence flowing


Connect HR platform HR Employee roster synchronized


Connect endpoint management (MDM) IT Device compliance data flowing


Begin drafting core policies (Information Security, Access Control, Change Management) Compliance Lead First three policy drafts complete


Verify MFA is enforced on all connected systems IT / Engineering MFA compliance at 100% or exceptions documented


**Day 14 checkpoint:** All critical integrations are connected. First three policies are drafted. MFA enforcement is verified.


### Week 3 (Days 15-21): Complete Policy Suite


Task Owner Deliverable


Draft remaining policies (Incident Response, Risk Assessment, Data Classification, Acceptable Use, Vendor Management, BC/DR, HR Security) Compliance Lead All ten policies drafted


Route policies for management review Compliance Lead Policies distributed to CTO, CEO, department heads


Begin full endpoint agent deployment to all employees IT Deployment instructions sent to all employees


Start vendor inventory compilation Compliance Lead Initial vendor list with data handling categories


**Day 21 checkpoint:** All policies are drafted and in review. Endpoint agent deployment is underway.


### Week 4 (Days 22-30): Finalize Foundation


Task Owner Deliverable


Obtain management approval on all policies Compliance Lead All policies approved with documented sign-off


Trigger employee policy acknowledgment workflow HR / Compliance Lead Acknowledgment requests sent to all employees


Complete endpoint agent deployment (100% coverage) IT All employee devices enrolled and compliant


Finalize auditor engagement (signed agreement, fieldwork dates confirmed) Compliance Lead Signed engagement letter with audit dates


Connect any remaining integrations Engineering All applicable integrations active


**Day 30 checkpoint:** All policies approved. Endpoint deployment complete. Auditor engaged with confirmed dates. Foundation phase complete.


## Phase 2: Implementation (Days 31-60)


Phase 2 focuses on implementing the technical controls, completing operational requirements, and building the evidence base your auditor will evaluate.


### Week 5 (Days 31-37): Technical Control Implementation


Task Owner Deliverable


Enable cloud audit logging (CloudTrail, Activity Log, Cloud Audit Logs) Engineering Logging active across all cloud accounts


Configure centralized log aggregation and alerting Engineering Security alerts triggering for critical events


Enforce branch protection rules on all production repositories Engineering Direct commits to production branches blocked


Verify encryption at rest and in transit for all customer data Engineering Encryption configuration documented


### Week 6 (Days 38-44): Access Control and HR Security


Task Owner Deliverable


Conduct first quarterly access review Compliance Lead + Engineering Access review documented with findings and remediation


Verify background checks are on file for all employees HR Background check status documented per employee


Schedule and begin security awareness training HR / Compliance Lead Training assigned to all employees with deadline


Configure automated deprovisioning for terminated employees IT / Engineering Offboarding process verified with test


### Week 7 (Days 45-51): Risk Assessment and Vendor Management


Task Owner Deliverable


Conduct formal risk assessment Compliance Lead Risk register complete with likelihood, impact, and treatment plans


Complete vendor security assessments for critical vendors Compliance Lead Assessment records for top-tier vendors


Collect vendor SOC 2 reports or security certifications Compliance Lead Vendor compliance documentation on file


Verify data classification policy is applied to systems and data stores Compliance Lead + Engineering Data categories mapped to systems


### Week 8 (Days 52-60): Incident Response and Business Continuity


Task Owner Deliverable


Conduct tabletop exercise for incident response plan Security / Engineering Exercise report with findings and action items


Verify backup procedures and test recovery Engineering Backup verification records, recovery test documentation


Ensure all employees have completed security training HR / Compliance Lead 100% training completion with records


Ensure all employees have acknowledged all policies Compliance Lead 100% policy acknowledgment


Review GRC platform dashboard for any remaining failing controls Compliance Lead Updated gap list (should be minimal)


**Day 60 checkpoint:** All technical controls implemented. Risk assessment, access review, and incident response testing complete. Training and policy acknowledgments at 100%. Implementation phase complete.


## Phase 3: Validation (Days 61-90)


Phase 3 validates that everything is working, prepares your team for auditor interactions, and conducts the audit itself.


### Week 9 (Days 61-67): Internal Readiness Assessment


Task Owner Deliverable


Run comprehensive compliance check in GRC platform Compliance Lead Complete compliance status report


Review every control category for passing status Compliance Lead Control-by-control verification log


Identify and remediate any remaining gaps Various Gap resolution documentation


Draft system description for auditor Compliance Lead System description document


Prepare management assertion letter Compliance Lead + CEO/CTO Signed management assertion


### Week 10 (Days 68-74): Audit Preparation


Task Owner Deliverable


Grant auditor access to GRC platform Compliance Lead Auditor account with read-only access


Brief all personnel who will interact with auditor Compliance Lead Team briefing completed


Prepare supplementary documentation (architecture diagrams, network diagrams, data flow diagrams) Engineering Documentation package ready


Conduct final compliance scan and resolve any last-minute issues Compliance Lead Clean compliance dashboard


### Week 11-12 (Days 75-85): Audit Fieldwork


Task Owner Deliverable


Support auditor evidence review and requests Compliance Lead Responses provided within 24 hours


Facilitate auditor interviews with engineering, HR, IT, and leadership Compliance Lead Interviews completed as scheduled


Provide additional evidence or clarification as requested Various All auditor requests fulfilled


### Week 13 (Days 86-90): Report Review and Delivery


Task Owner Deliverable


Review draft report for factual accuracy Compliance Lead + CTO Review comments provided to auditor


Receive and distribute final SOC 2 Type I report Compliance Lead Report delivered and shared with stakeholders


Document lessons learned and begin Type II planning Compliance Lead Lessons learned document, Type II timeline draft


**Day 90: SOC 2 Type I report delivered.**


## Risk Factors and Contingency Planning


In our experience, several factors can threaten your ninety-day timeline. We advise clients to plan for these proactively:


Risk Impact Mitigation


Auditor scheduling conflict +2-4 weeks Engage auditor before Day 1; confirm dates in writing


Slow policy approval +1-2 weeks Set firm review deadlines; escalate to CEO if needed


Employee training non-completion +1 week Set hard deadlines; follow up individually


Endpoint agent deployment resistance +1 week Communicate early; deploy via MDM where possible


Missing vendor security documentation +1-2 weeks Start vendor assessments in Week 7; follow up aggressively


Auditor requests additional evidence +1 week Respond within 24 hours; have engineering support available


The single biggest risk we see to the ninety-day plan is auditor availability. If your auditor cannot begin fieldwork by Day 75, the entire plan shifts. Lock in your audit dates before Day 1.


## Resource Requirements


Role Time Commitment (90 days) Peak Weeks


Compliance Lead 15-25 hours/week Weeks 1-4 (foundation) and 11-12 (fieldwork)


Engineering Lead 8-12 hours/week Weeks 1-2 (integrations) and 5-8 (technical controls)


IT / Security 5-8 hours/week Weeks 1-4 (agent deployment, MFA, MDM)


HR 3-5 hours/week Weeks 3-6 (policies, training, background checks)


CEO / CTO 2-3 hours/week Weeks 4 (policy approval) and 12 (auditor interviews)


*Starting your own 90-day sprint? Download our free[SOC 2 Readiness Checklist](https://getagency.com/lp/soc-2-readiness-checklist) — 11 control categories and roughly 60 items to self-assess before the clock starts.*


## Key Takeaways


- We consistently see that a SOC 2 Type I can be completed in ninety days with disciplined execution across three phases: Foundation (days 1-30), Implementation (31-60), and Validation (61-90)
- GRC platform selection and auditor engagement must happen before the ninety-day clock starts — we cannot stress this enough
- Week 1 priorities are cloud and identity integrations — everything else builds on this foundation
- We recommend having all ten core policies drafted by Day 21, approved by Day 30, and acknowledged by all employees by Day 60
- Conduct your risk assessment, first access review, and incident response tabletop during the Implementation phase (days 31-60) — these are the evidence items auditors scrutinize most closely
- Reserve weeks 11-12 for audit fieldwork with all evidence prepared and team briefed by Day 74
- The biggest timeline risk is auditor availability — in every accelerated engagement we've managed, locking in audit dates before Day 1 is the single most important preparation step


## Frequently Asked Questions


### Is ninety days realistic for a first SOC 2 Type I?


What we tell clients is yes — for organizations that have basic security practices in place (SSO, MFA, code reviews, cloud infrastructure). We've successfully guided teams through this timeline many times. The ninety-day plan is aggressive but achievable if the compliance lead treats it as a primary responsibility and the engineering, HR, and IT teams are committed to meeting their milestones. Organizations starting without any security fundamentals may need one hundred twenty days or more.


### What if we cannot complete everything by Day 90?


Based on what we see across our client base, the most common solution is to shift the audit fieldwork dates by two to four weeks. We advise communicating with your auditor early if you anticipate a delay — most auditors can accommodate moderate schedule changes with advance notice. The Foundation phase (days 1-30) is the most critical to complete on time because everything else depends on it.


### Can we compress this to sixty days?


For well-prepared organizations — those that already have policies, MFA enforcement, endpoint management, and documented processes — we've seen sixty-day Type I completions work. The compression primarily comes from the Foundation phase, where existing controls reduce the setup and implementation work. Fieldwork and report delivery timelines are largely fixed and cannot be compressed significantly.


### Should we pursue Type I on this timeline or go straight to Type II?


The advice we give most often here depends on your deal pipeline. This ninety-day plan is specifically for Type I. Going directly to Type II adds the observation period (six to twelve months) before fieldwork can begin. If you need a SOC 2 report quickly — to close an enterprise deal, respond to a customer request, or demonstrate initial compliance maturity — Type I on a ninety-day timeline is the right approach. You can begin the Type II observation period immediately after Type I report delivery.


### What GRC platform features are most important for this ninety-day plan?


The three features we consider most critical for a compressed timeline are: automated evidence collection (reduces the manual burden during Implementation phase), policy templates (accelerates the Foundation phase policy drafting), and real-time compliance dashboard (enables the Validation phase readiness assessment). All major platforms — Vanta, Drata, Secureframe, Sprinto — provide these features.
