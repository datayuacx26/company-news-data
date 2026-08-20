---
schema_version: "1.0.0"
document_id: "c193a841ddde962d1723b3a60aa6e5a66e96c55b7f3e202ddc6fe3a96bb4a06c"
company_key: "yc-salarybox"
company: "SalaryBox"
source_id: "yc-salarybox-rss-27e69f0219eb"
canonical_url: "https://salarybox.in/blog/background-verification-for-it-and-bpo-companies-in-india-how-to-stop-data-breaches-moonlighting-and-credential-fraud-before-they-start/"
published_at: "2026-08-07T11:02:11+00:00"
first_seen_at: "2026-08-07T13:05:48.854059+00:00"
fetched_at: "2026-08-07T13:05:49.964939+00:00"
content_hash: "sha256:6361017ba764bfba9b750ee27c5990d0d31ccd298dc3a38f5faf905c89f3e660"
---

# Background Verification for IT and BPO Companies in India: How to Stop Data Breaches, Moonlighting, and Credential Fraud Before They Start

[Insights & Resources](https://salarybox.in/blog/category/insights-resources/)


# Background Verification for IT and BPO Companies in India: How to Stop Data Breaches, Moonlighting, and Credential Fraud Before They Start


August 7, 2026


[Shashank Yadav](https://salarybox.in/blog/author/shashank87cc9f270b/)


[No comments yet](https://salarybox.in/blog/background-verification-for-it-and-bpo-companies-in-india-how-to-stop-data-breaches-moonlighting-and-credential-fraud-before-they-start/#respond)


A software developer at a Bangalore IT company copies source code to a personal cloud drive on his last day. He joins a competitor the following Monday and uses that code to accelerate their product roadmap. The original company discovers the theft six months later when they see suspiciously similar features in a competitor’s release. By then, the developer has moved on again. The company checks his original application and discovers that the employment history on his resume was fabricated. He never worked at two of the four companies he listed. Nobody verified it before giving him access to proprietary codebases.


This is not a rare event. AuthBridge’s Workforce Fraud Files 2025 found a 9.46 percent discrepancy rate in the IT and ITES sector, with nearly one in five candidates misrepresenting their credentials through inflated job titles, exaggerated tenures, and unverifiable employers. In an industry that handles some of the most sensitive data on the planet, this level of credential fraud is alarming.


India’s technology sector directly employs approximately 6 million people as of FY 2026, with net additions of 135,000 employees in the year. The industry generates over $315 billion in revenue. IT and BPO companies handle client data spanning banking transactions, healthcare records, insurance claims, government databases, and personal information of hundreds of millions of people worldwide.


Yet the industry’s approach to background verification remains surprisingly inconsistent. Large companies like TCS, Infosys, and Wipro have established verification processes. But the thousands of mid size IT companies, BPO operations, and technology startups that employ the majority of the workforce often rely on informal checks, delayed verification, or no verification at all for certain employee categories.


This guide is for CTOs, CISOs, HR heads, and founders at IT and BPO companies of all sizes. We will cover the specific verification risks in the technology sector, what checks you need for different IT roles, the moonlighting challenge, and how to build a verification process that protects your clients’ data and your company’s reputation.


## Why IT and BPO Companies Face Unique Verification Risks


### Employees Access Client Data from Day One


In most IT companies, a new employee gets a laptop, VPN credentials, and access to client systems within their first week. A developer might be committing code to a banking client’s repository on day three. A BPO analyst might be processing insurance claims containing personal health information on their second shift.


This immediate, deep access to sensitive data makes IT hiring fundamentally different from most other industries. In manufacturing, a new worker starts on the factory floor under supervision. In retail, a new cashier works alongside an experienced colleague. In IT, a new developer might be working independently on a client’s codebase within days, sometimes from their home, with minimal direct oversight.


If that developer’s identity and credentials were never properly verified, you have given an unknown individual access to your client’s most sensitive systems. The potential for data theft, intellectual property misappropriation, or deliberate system compromise is enormous.


### The Moonlighting Problem


Moonlighting has emerged as a significant concern in the Indian IT sector. Randstad India reported a 25 to 30 percent increase in moonlighting activity across the IT sector over a three year period. Employees working simultaneously for multiple companies, sometimes competitors, create overlapping access risks.


While there is no central law in India that explicitly bans moonlighting for private sector employees, almost every IT employment contract includes an exclusivity clause requiring employees to devote their full time to the company’s business. The problem is detecting violations.


UAN verification through EPFO records can reveal whether an employee has active PF contributions from multiple employers simultaneously. This is one of the most effective tools for detecting moonlighting, yet most IT companies do not run this check either during hiring or periodically during employment.


### Contract and Freelance Developer Risk


IT companies frequently engage contract developers, freelancers, and staff augmentation resources from smaller vendors. These individuals often get the same system access as permanent employees but receive significantly less scrutiny during onboarding.


A contract developer brought in through a staffing agency for a three month engagement has access to the same codebase, the same databases, and the same client information as a full time employee who went through a thorough interview process. But the staffing agency’s verification of this person might amount to nothing more than confirming their resume looked reasonable.


### BPO Specific Data Sensitivity


BPO and KPO operations handle data that is extraordinarily sensitive. Health insurance claims processing involves personal health records protected under HIPAA (for US clients) and the DPDP Act. Financial services BPOs handle credit card numbers, bank account details, and transaction records. Legal process outsourcing teams access privileged attorney client communications.


An unverified BPO employee with access to these systems can cause damage that extends far beyond the BPO company itself. A data breach at an Indian BPO that handles US healthcare data can trigger HIPAA violation investigations, client contract terminations, and regulatory penalties in multiple jurisdictions.


### Intellectual Property Is the Product


Unlike manufacturing where the product is physical, in IT the product is code, algorithms, designs, and data. These assets can be copied in seconds, stored on a personal device, and transferred to a competitor without any physical trace.


When an employee with unverified credentials and unknown loyalties has access to proprietary source code, you are trusting that person with the core of your business value. Background verification does not guarantee loyalty, but it ensures that you at least know who you are trusting.


## What Background Checks IT and BPO Companies Should Run


### For All Employees (Permanent, Contract, and Freelance)


Every person who will access your systems, your code, or your client data needs identity verification before receiving access credentials.


**PAN Card Verification.** Confirms the person’s identity against the Income Tax Department database. Catches fake PAN documents immediately. Essential for linking the person to their tax identity.


**Voter ID Verification.** Validates identity and registered address through the Election Commission database. Provides address confirmation independent of what the employee claimed.


**UAN Verification.** Checks employment history through EPFO records. This is critical for IT hiring because it reveals actual employment dates and previous employers. A candidate who claims five years at a FAANG company but whose UAN shows PF contributions from a different company entirely is presenting fabricated experience.


ID Verify by SalaryBox (verify.salarybox.in) runs all three checks against government databases and returns results in minutes. The cross record confidence analysis automatically compares details across documents, catching discrepancies that would take manual review hours to identify.


### For Developers and Technical Staff


Beyond identity and employment verification, developers and technical staff with code access need additional scrutiny.


**Cross Record Confidence Analysis.** Verify multiple documents and let ID Verify automatically compare names, dates of birth, and other details across all documents. Inconsistencies between the PAN name, the Voter ID name, and the name on educational certificates could indicate borrowed credentials.


**Criminal Record Check.** Search court records for fraud, intellectual property theft, data theft, or any technology related offences. While criminal checks will not catch every bad actor, they filter out individuals with documented histories of data misuse.


### For BPO and Data Processing Staff


BPO employees handling client data need verification that addresses both identity and reliability.


**PAN and Voter ID Verification.** Identity baseline for every employee.


**UAN Employment History.** Verify employment tenure at previous BPO companies. Short stints at multiple BPOs could indicate performance terminations or disciplinary issues.


**Address Verification.** For employees handling highly sensitive data (healthcare, financial services), confirm their current residential address. This provides traceability and is sometimes a contractual requirement from BPO clients.


### For Team Leads and Managers


Employees in leadership positions control team access privileges, approve data access requests, and set the security culture for their teams.


**Comprehensive Verification.** Identity checks, criminal record searches, employment history through UAN, education verification, and reference checks from previous employers. A manager who fabricated their credentials may also be lax about verifying their team’s access requests.


### For Contract and Staff Augmentation Resources


Staff augmentation vendors should be contractually required to verify their resources before deployment. But you should run your own identity verification as an additional layer.


**PAN or Voter ID Verification.** At minimum, verify identity before granting system access. Do not rely on the staffing vendor’s word alone.


**Driving Licence Verification.** If the contractor uses company provided transportation or needs to travel to client sites.


## Detecting and Preventing Moonlighting


Moonlighting in the IT sector creates specific risks that background verification can help address.


### UAN Verification at Hiring


When you verify a candidate’s UAN, you see their complete PF contribution history. If a candidate who is supposedly unemployed or on notice period shows active PF contributions from another employer, they may be planning to moonlight.


This is not about policing employees’ personal time. It is about knowing whether the person you are hiring has an existing full time commitment that conflicts with the exclusivity clause in your employment contract.


### Periodic UAN Checks During Employment


Your verification policy should include periodic UAN checks for existing employees, particularly those in senior or data sensitive roles. If an employee’s UAN shows PF contributions from a second employer while they are employed with you, this warrants a conversation.


ID Verify by SalaryBox makes it easy to run periodic checks. The pay per check pricing means you can verify your entire team annually without a significant budget impact.


### The Moonlighting Conversation


When UAN verification reveals dual employment, approach it as a contract compliance issue rather than an accusation. Review the employment contract’s exclusivity clause. Discuss the finding with the employee. Determine whether the moonlighting creates a conflict of interest, particularly if the second employer is a competitor or client. Document the outcome and any agreed action.


## Data Protection and Client Compliance


### DPDP Act 2023 Obligations


Every IT and BPO company that processes personal data is a Data Fiduciary under the DPDP Act 2023. This creates specific obligations around who can access personal data and under what conditions.


Hiring unverified employees who access personal data is a compliance failure. You cannot demonstrate reasonable security safeguards if you have not verified the identity of the people handling the data.


### Client Contractual Requirements


Most IT services and BPO contracts include clauses requiring the service provider to conduct background verification of employees who will access client data. Some clients specify the types of checks required. Others require certification that all employees with data access have been verified.


A standardized verification process using a platform like ID Verify by SalaryBox makes it easy to demonstrate compliance with these contractual requirements. Generate verification reports, maintain timestamped records, and produce compliance documentation when clients audit your processes.


### SOC 2 and ISO 27001 Alignment


Companies pursuing SOC 2 Type II or ISO 27001 certification need documented employee verification processes. These frameworks require evidence that the organization verifies the identity and background of employees with access to information systems.


A systematic verification process that produces auditable records directly supports these certification requirements.


## Building an IT Specific Verification Process


### Step 1: No System Access Without Verification


Make identity verification a prerequisite for issuing system credentials. No laptop setup, no VPN access, no email account, and no client system access until PAN or Voter ID has been verified against government databases.


For permanent employees, this means completing Tier 1 verification (identity) before the joining date and Tier 2 verification (employment history, criminal records) within the first two weeks.


For contract resources, this means identity verification before the first day. The staffing vendor should provide verification records, and your team should run an independent identity check.


### Step 2: Verify Employment History for Every Technical Hire


The 9.46 percent discrepancy rate in IT hiring makes employment verification non negotiable. Run UAN checks for every technical hire. Compare the EPFO employment history with the resume. Flag gaps, missing employers, and tenure discrepancies.


A developer who fabricated two years of experience at a company they never worked for may lack the skills your project needs. Discovering this after they have been writing code for three months is much more expensive than discovering it before they start.


### Step 3: Implement Ongoing Verification


Background verification should not be a one time hiring exercise in IT companies. Implement annual identity re verification for all employees, quarterly UAN checks for employees in high sensitivity roles (this also detects moonlighting), re verification when an employee moves to a different client project, and event triggered verification when security incidents occur or when employees display concerning behavior patterns.


### Step 4: Extend Verification to Your Vendor Ecosystem


Add verification requirements to all staffing and subcontracting agreements. Require vendors to verify identity and employment history for every resource before deployment. Audit vendor compliance periodically.


### Step 5: Maintain Auditable Records


Keep verification records organized and accessible for client audits, SOC 2 assessments, and ISO certification reviews. ID Verify by SalaryBox generates timestamped PDF reports for every check, creating the documentation trail these audits require.


## Frequently Asked Questions


### What is the discrepancy rate in IT sector background verification?


AuthBridge’s data shows a 9.46 percent overall discrepancy rate in IT and ITES sector background checks. Nearly one in five IT candidates misrepresented their credentials in some way, including inflated job titles, exaggerated tenures, and unverifiable previous employers. This rate is significantly higher than many other industries.


### Can background verification detect moonlighting?


Yes. UAN verification through EPFO records shows active PF contributions from all employers simultaneously. If an employee’s UAN shows contributions from your company and another company during the same period, they are employed at both places. This is the most reliable method for detecting moonlighting.


### Should we verify contract and freelance developers?


Yes. Contract developers and freelancers get the same system access as permanent employees. They can access codebases, databases, and client information. Verify their identity at minimum (PAN or Voter ID) before granting system access. For longer engagements, verify employment history through UAN as well.


### How does verification support SOC 2 and ISO 27001 compliance?


Both frameworks require organizations to verify the identity and background of employees with access to information systems. A documented verification process that produces timestamped records directly satisfies these requirements. Auditors look for evidence that verification is systematic, consistent, and well documented.


### What verification do we need for BPO employees handling healthcare data?


BPO employees handling protected health information (PHI) need comprehensive verification: identity (PAN and Voter ID), employment history (UAN), criminal record checks, and address verification. Many US healthcare clients require this level of verification contractually, and HIPAA compliance audits will examine your employee screening processes.


### How quickly can we verify new IT hires?


Digital verification through ID Verify by SalaryBox returns results in minutes. A hiring coordinator can verify a batch of 20 new joiners in a single morning. For companies with regular batch joining dates, the entire cohort can be verified before their first day, ensuring nobody receives system access without confirmed identity.


## Your Code Is Only as Secure as Your People


Firewalls, encryption, access controls, and penetration testing protect your systems from external threats. But the biggest data breach risk in any IT company walks through the front door every morning with a laptop bag and an access badge.


ID Verify by SalaryBox (verify.salarybox.in) gives you the tools to verify every person who touches your systems, your code, and your client data. Run PAN, Voter ID, Driving Licence, and UAN checks in minutes. Detect moonlighting through employment history verification. Generate compliance documentation for client audits and certification reviews.


Because in IT, the weakest link is never the firewall. It is the person behind the keyboard.


*Visit*[verify.salarybox.in](https://verify.salarybox.in/) *to start verifying your IT workforce today.*


### *Related*


##### [Shashank Yadav](https://salarybox.in/blog/author/shashank87cc9f270b/)


### Leave a Reply[Cancel reply](https://salarybox.in/blog/background-verification-for-it-and-bpo-companies-in-india-how-to-stop-data-breaches-moonlighting-and-credential-fraud-before-they-start/#respond)
