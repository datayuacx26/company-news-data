---
schema_version: "1.0.0"
document_id: "796842eb47922af8fe779461c8fef27309920c4aaff5e644258640a797484d21"
company_key: "yc-agency"
company: "Agency"
source_id: "yc-agency-news-import-0d31f4b059c0"
canonical_url: "https://blog.getagency.com/articles/soc-2-compliance-checklist"
published_at: "2026-02-24T00:00:00+00:00"
first_seen_at: "2026-07-21T05:05:48.945011+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:3a6616d725e35ed9c58a526612a66ea0ae3e6d56975dcd4fe1ca30efbcf15304"
---

# SOC 2 Compliance Checklist: Step-by-Step Preparation Guide

*Most SOC 2 projects fail not because the requirements are unclear, but because teams lose track of what needs to happen and in what order. We built this checklist from hundreds of engagements to give you a single working document that covers every phase from scoping through post-audit.*


This checklist covers every task required to prepare for and complete a SOC 2 audit — from initial scoping through post-audit activities. For a condensed, startup-focused version you can download and check off, grab our free[SOC 2 Readiness Checklist](https://getagency.com/lp/soc-2-readiness-checklist) . Each item includes enough context to understand why it matters and what a good outcome looks like. We recommend using this as a working document to track your compliance project from start to finish, assigning owners and deadlines to each task.


The checklist is organized into eight phases that follow the natural sequence of a SOC 2 engagement. In our experience, most first-time organizations complete all phases in three to six months for a Type I report. For foundational SOC 2 context, see our complete guide to SOC 2.


## Phase 1: Scoping and Planning


Scoping determines what is included in your audit and directly affects cost, timeline, and complexity.


-


**Select Trust Service Criteria** : Security (Common Criteria) is mandatory. We recommend evaluating whether your customers require Availability, Processing Integrity, Confidentiality, or Privacy. In our experience, starting with Security only is the right move unless buyers specifically request additional criteria. For criteria selection guidance, see our SOC 2 compliance requirements guide.


-


**Define system boundaries** : Identify which services, infrastructure, data stores, and processes are in scope. We advise documenting what is included and what is excluded. The system boundary determines what the auditor evaluates.


-


**Choose report type** : Type I evaluates control design at a point in time. Type II evaluates control design and operating effectiveness over an observation period (six to twelve months). We recommend that most organizations start with Type I for speed and transition to Type II. For comparison, see our Type I vs Type II guide.


-


**Identify the compliance lead** : Assign one person to own the SOC 2 project end-to-end. In our experience, this single-owner model is what separates projects that ship on time from those that drift.


-


**Establish project timeline** : Set target dates for each phase. We recommend working backward from your desired report delivery date.


-


**Secure executive sponsorship** : Obtain commitment from the CEO, CTO, or equivalent leader. You will need their time for policy approval, management assertion, and auditor interviews. We find this is one of the most underestimated dependencies in the entire process.


## Phase 2: Platform and Auditor Selection


Your GRC platform and auditor are the two most consequential vendor decisions in the SOC 2 process.


-


**Select a GRC platform** : Evaluate Vanta, Drata, Secureframe, Sprinto, and other platforms based on your tech stack coverage, framework needs, and budget. The platform automates evidence collection, policy management, and compliance monitoring. We help our clients evaluate these platforms and select the right fit for their environment.


-


**Complete platform onboarding** : Configure your workspace with company profile, framework selection, and Trust Service Criteria. We recommend setting your intended audit dates during initial setup.


-


**Connect cloud provider integrations** : AWS, Azure, and GCP integrations are the highest priority because they provide evidence for the largest number of controls.


-


**Connect identity provider** : Okta, Google Workspace, Microsoft Entra ID, or your identity platform — this provides user roster, MFA enforcement, and access management evidence.


-


**Connect code repositories** : GitHub, GitLab, or Bitbucket integration provides branch protection and code review evidence for change management controls.


-


**Connect HR platform** : BambooHR, Gusto, Rippling, or your HR system synchronizes employee data for personnel security controls.


-


**Connect remaining integrations** : Endpoint management (Jamf, Intune), monitoring (Datadog, PagerDuty), vulnerability scanning (Snyk, Qualys), and any other relevant tools.


-


**Deploy endpoint agent** : Install the GRC platform agent on all employee devices to verify disk encryption, screen lock, firewall, OS updates, and antivirus status.


-


**Engage an auditor** : Select a CPA firm experienced with SOC 2. Confirm fieldwork dates in writing. We cannot stress this enough — lock in dates early, because auditor availability is the most common timeline risk. For auditor discovery, AuditNex provides matching tools.


-


**Run initial gap assessment** : Use your GRC platform's compliance scan to identify gaps between your current state and SOC 2 requirements. We advise prioritizing findings by severity so you tackle the highest-risk items first.


## Phase 3: Policy Development


SOC 2 requires ten core policies that formalize your security program. Policies must be approved by management and acknowledged by all employees.


-


**Information Security Policy** : Overarching security governance including roles, responsibilities, program objectives, and organizational commitment to security.


-


**Access Control Policy** : User provisioning, authentication requirements (MFA), role-based access, access review procedures, and deprovisioning.


-


**Change Management Policy** : Change request, approval, testing, and deployment procedures for infrastructure and application changes.


-


**Incident Response Plan** : Detection, classification, escalation, response, recovery, and post-mortem procedures for security incidents.


-


**Risk Assessment Policy** : Risk identification methodology, assessment frequency, risk register management, and treatment plan procedures.


-


**Data Classification Policy** : Data categories (public, internal, confidential, restricted), handling requirements for each category, and labeling procedures.


-


**Acceptable Use Policy** : Employee guidelines for appropriate use of company systems, data, email, internet, and resources.


-


**Vendor Management Policy** : Vendor assessment criteria, security evaluation procedures, onboarding process, ongoing monitoring, and contractual requirements.


-


**Business Continuity and Disaster Recovery Plan** : Recovery procedures, RTO and RPO targets, testing schedules, and continuity strategies.


-


**Human Resources Security Policy** : Background check requirements, onboarding security procedures, security awareness training, and offboarding processes.


-


**Obtain management approval on all policies** : Route policies to CTO, CEO, or security leadership for formal sign-off. We recommend documenting approval dates explicitly — auditors will ask for them.


-


**Distribute policies to all employees** : Use your GRC platform's policy distribution workflow to send all policies to every employee.


-


**Collect employee policy acknowledgments** : Track acknowledgments until 100% of employees have acknowledged all policies. In our experience, this is one of the tasks that drags on if not actively managed.


## Phase 4: Control Implementation


Implement the technical and operational controls that your policies describe.


### Access Management


-


**Enforce MFA on all systems** : All employee accounts on all critical systems (cloud provider, identity provider, code repositories, production infrastructure) must require multi-factor authentication. We advise documenting any exceptions — auditors will scrutinize them.


-


**Implement role-based access control** : Assign access based on job function. We recommend documenting the access model showing which roles have access to which systems.


-


**Configure automated deprovisioning** : When an employee is terminated, access to all systems must be revoked within twenty-four hours. We strongly recommend testing the offboarding process before the audit.


-


**Conduct first quarterly access review** : Review all user access across critical systems. Document findings and remediate any inappropriate access.


### Change Management


-


**Enable branch protection on production repositories** : Require pull request reviews before merging to production branches. Block direct commits to main/production branches.


-


**Document deployment procedures** : Ensure your change management process includes change request, approval, testing, and deployment steps with audit trail.


### Monitoring and Logging


-


**Enable cloud audit logging** : Activate CloudTrail (AWS), Cloud Audit Logs (GCP), or Activity Log (Azure) across all accounts.


-


**Configure centralized log aggregation** : Aggregate security-relevant logs in a central location with alerting for critical events.


-


**Set up security alerting** : Configure alerts for failed login attempts, privilege escalations, configuration changes, and other security-relevant events.


### Endpoint Security


-


**Verify endpoint agent deployment at 100%** : Every employee device must have the GRC platform agent installed and reporting compliant status.


-


**Confirm disk encryption on all devices** : FileVault (macOS) or BitLocker (Windows) must be enabled on every employee device.


-


**Verify screen lock configuration** : We recommend configuring all devices to lock after five minutes of inactivity.


### Data Protection


-


**Encrypt data at rest** : All customer data must be encrypted at rest using AES-256 or equivalent.


-


**Encrypt data in transit** : All data transmission must use TLS 1.2 or higher.


-


**Implement data classification** : Apply your data classification policy to actual systems and data stores.


## Phase 5: Operational Requirements


Complete the recurring operational activities that auditors evaluate.


-


**Conduct formal risk assessment** : Identify risks, evaluate likelihood and impact, document in a risk register, and define treatment plans (accept, mitigate, transfer, avoid).


-


**Complete vendor inventory** : List all third-party vendors that handle customer data or provide critical services.


-


**Conduct vendor security assessments** : Evaluate security posture of critical vendors. We recommend collecting vendor SOC 2 reports or security certifications where available.


-


**Complete security awareness training for all employees** : Assign training covering phishing, data handling, password practices, incident reporting, and acceptable use. Track 100% completion.


-


**Verify background checks on file** : Confirm background checks are completed for all employees with access to production systems. We advise establishing a policy for future hires at this stage.


-


**Conduct incident response tabletop exercise** : Run a simulated security incident scenario with your team. Document the exercise, findings, and action items. In our experience, this is one of the most commonly forgotten items — schedule it early.


-


**Test backup and recovery procedures** : Verify that backups are running, test a recovery scenario, and document the results including recovery time.


-


**Conduct penetration testing** : Engage an independent firm to perform penetration testing. Document findings and track remediation.


-


**Document board/leadership security updates** : Record evidence that security is discussed at the leadership level — meeting minutes or presentation records satisfy this requirement.


## Phase 6: Readiness Assessment


Before engaging the auditor, we recommend verifying that your entire compliance program is ready.


-


**Run comprehensive GRC platform compliance check** : Review the compliance dashboard for any failing controls.


-


**Verify all control categories show passing status** : Walk through each Trust Service Criteria category and confirm evidence is flowing correctly.


-


**Remediate any remaining gaps** : Address all failing controls. If a control cannot be fully remediated, we advise documenting the exception and your remediation plan.


-


**Draft system description** : Write the system description that will appear in Section III of your SOC 2 report. Include services overview, infrastructure, boundaries, data flows, and control environment.


-


**Prepare management assertion letter** : Draft the management assertion stating that the system description is accurate and controls meet the Trust Service Criteria. This requires CEO or CTO sign-off.


-


**Verify evidence completeness** : For Type II, confirm that evidence has been collected continuously throughout the observation period with no significant gaps.


-


**Brief team members who will interact with the auditor** : Prepare engineering leads, HR, IT, and leadership for auditor interviews. We recommend explaining what to expect and how to respond — auditor interviews go much more smoothly when people are prepared.


## Phase 7: Audit Fieldwork


Support the auditor through the audit process.


-


**Grant auditor access to GRC platform** : Provide read-only access to your GRC platform so the auditor can review all evidence directly.


-


**Provide system description and management assertion** : Submit the system description and management assertion to the auditor.


-


**Support evidence review** : We recommend responding to auditor requests for additional evidence or clarification within twenty-four hours. Delays here directly extend your audit timeline.


-


**Facilitate auditor interviews** : Schedule and coordinate interviews with compliance lead, engineering leads, HR, IT, and executive leadership.


-


**Prepare supplementary documentation** : Architecture diagrams, network diagrams, data flow diagrams, and any other supporting materials the auditor requests.


-


**Track and resolve auditor findings** : If the auditor identifies issues during fieldwork, remediate promptly and provide updated evidence.


## Phase 8: Post-Audit Activities


After receiving the final report, we advise establishing the foundation for ongoing compliance.


-


**Review draft report for accuracy** : Check the system description, control descriptions, and any noted exceptions for factual accuracy. Provide corrections to the auditor.


-


**Receive and distribute final SOC 2 report** : Share the report with customers, prospects, and partners who request it. In our experience, most organizations share under NDA.


-


**Set up Trust Center page** : Create a public-facing compliance page where prospects can request access to your SOC 2 report.


-


**Document lessons learned** : Record what went well, what caused delays, and what to improve for the next audit cycle.


-


**Begin planning for Type II (if this was Type I)** : We recommend starting the Type II observation period immediately. Define the observation window and verify continuous evidence collection.


-


**Schedule recurring compliance activities** : Set calendar reminders for quarterly access reviews, annual risk assessment, annual training renewal, annual vendor assessments, and annual policy review.


-


**Monitor compliance continuously** : Use your GRC platform dashboard to track control status and address any issues as they arise rather than scrambling before the next audit.


*Want to self-assess where you stand? Download our free[SOC 2 Readiness Checklist](https://getagency.com/lp/soc-2-readiness-checklist) — 11 control categories and roughly 60 items you can check off before fieldwork begins.*


## Key Takeaways


- We recommend organizing your SOC 2 compliance effort across eight distinct phases: scoping, platform and auditor selection, policy development, control implementation, operational requirements, readiness assessment, audit fieldwork, and post-audit activities
- In our experience, platform and auditor selection should happen as early as possible — both decisions affect every subsequent phase
- Ten core policies must be drafted, approved by management, and acknowledged by all employees. We advise starting policy work in parallel with platform onboarding to save time
- Technical control implementation covers access management, change management, monitoring, endpoint security, and data protection. We find that access management controls require the most cross-functional coordination
- Operational requirements — including risk assessment, vendor management, employee training, incident response testing, and penetration testing — are where we see teams fall behind most often
- The readiness assessment phase is your safety net. We strongly recommend treating it as a formal gate before engaging the auditor
- Post-audit activities establish the foundation for ongoing compliance and the transition to Type II. We advise clients to begin Type II planning on the day they receive their Type I report


## Frequently Asked Questions


### How long does it take to complete this entire checklist?


What we tell clients is that a well-organized team with some existing security practices (SSO, MFA, code reviews) can complete all phases through a Type I audit in three to four months. For organizations starting with minimal security infrastructure, we typically budget five to six months. In our experience, the timeline is driven primarily by three factors: how quickly policies are approved, how quickly integrations are connected and gaps remediated, and auditor availability.


### Can I use this checklist for Type II as well?


Yes, and we walk clients through this transition regularly. The one critical addition for Type II is that Phase 5 operational requirements must be sustained throughout the entire observation period (six to twelve months), not just completed once. Quarterly access reviews must happen every quarter, continuous monitoring must run without interruption, and evidence must be collected throughout. What we tell clients is that the checklist items remain the same — the difference is sustained execution over time.


### What is the most commonly missed checklist item?


In our experience advising clients, vendor security assessments are the most commonly missed item because they require cooperation from third parties and often take longer than expected. We recommend starting vendor assessments early in Phase 5 and following up aggressively. The second most commonly missed item is the incident response tabletop exercise — what we see repeatedly is that organizations forget to schedule and document it before the audit window closes.


### Do I need to complete every item on this checklist?


What we tell clients is that every item on this checklist maps to a SOC 2 requirement or an audit expectation. Skipping items creates risk of audit exceptions or a qualified opinion. However, the depth of each item can vary — a five-person startup's vendor management program looks very different from a five-hundred-person company's program. We advise that every item is addressed at a level appropriate for your organization's size and complexity.


### Should I use a GRC platform or manage this checklist manually?


We strongly recommend using a GRC platform. In our experience, platforms automate evidence collection, policy management, control monitoring, and compliance tracking — reducing the manual burden by fifty to seventy percent. What we tell clients considering the manual route is that it may be feasible for very small organizations but becomes unsustainable as the company grows and the Type II observation period requires continuous evidence collection.
