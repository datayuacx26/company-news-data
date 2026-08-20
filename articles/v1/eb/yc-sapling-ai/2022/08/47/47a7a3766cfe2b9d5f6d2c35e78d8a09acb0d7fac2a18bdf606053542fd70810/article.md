---
schema_version: "1.0.0"
document_id: "47a7a3766cfe2b9d5f6d2c35e78d8a09acb0d7fac2a18bdf606053542fd70810"
company_key: "yc-sapling-ai"
company: "Sapling.ai"
source_id: "yc-sapling-ai-rss-3361843b7812"
canonical_url: "https://blog.sapling.ai/an-overview-of-hipaa-compliance/"
published_at: "2022-08-29T07:41:41+00:00"
first_seen_at: "2026-07-25T22:15:25.567776+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:916a2aabbb8783c447ecc04860800fa09200cf0bc8f76c9a7a2d2767872c91eb"
---

# An Overview of HIPAA Compliance

**Disclaimer:** This article is not legal advice. Make sure to do your own research or consult with an expert on HIPAA regulatory standards. HIPAA is managed by the US Department of Health and Human Services (HHS). The official documentation for HIPAA can be found here:[https://www.hhs.gov/hipaa/index.html](https://www.hhs.gov/hipaa/index.html)


Contents


Toggle


##


**What are the Rules and Regulations for HIPAA compliance?**


The Health Insurance Portability and Accountability Act (HIPAA) was passed in 1996 and is a US regulation to protect the privacy and security of personal health information. Compliance requirements were slowly phased in between 2005 and 2006. Companies that touch health information often have SaaS companies as vendors. These companies provide a variety of basic business services such as email, or medical services such as electronic health record (EHR) storage and management, or medical dictionaries and medical spell checking for doctors’ notes. These companies (which include Sapling) need to be HIPAA compliant as health information may flow through them. This document provides an overview of what HIPAA compliance entails and some of the high-level steps a SaaS company can take towards HIPAA compliance.


##


HIPAA Concepts and Acronyms


**Protected Health Information (PHI) and Electronic Protected Health Information (ePHI)**


The HIPAA framework refers to health information for individuals as “protected”. This information could be related to an individual’s physical or mental health, demographic information or information about any treatment provided. This also covers what is considered Personally Identifiable Information (PII), such as names, emails, phone numbers, addresses, etc. The rest of this overview will focus primarily on electronic information. De-identified health information or information that is not tied to an individual is not under HIPAA restrictions.


**Business Associate Agreements (BAA)**


A business associate is a person or group that provides services around ePHI for a a covered entity that is required to be HIPAA compliant. The Business Associate Agreement (BAA, or sometimes referred to as the Business Associate Contract) is an agreement on a set of rules around how ePHI is handled. As an example, Sapling processes text for companies in the healthcare space. We enter into BAAs with our customers as the business associate because this text may contain PHI. Other companies that often enter into BAAs offer include billing services and data processing/analytics platforms.


**What is a Covered Entity?**


A Covered Entity in the context of HIPAA is a person or organization directly associated with providing healthcare services, and is required to be compliant with HIPAA.


##


**HIPAA “Privacy Rule”**


[https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html](https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html)


The HIPAA Privacy Rule details what information is protected and who is required to follow these rules. Covered entities need to protect PHI and ePHI and ensure that the information is not shared with anyone outside of their privacy policies. As a downstream consequence, SaaS companies (like Sapling) that want to provide services for these healthcare related companies also need to comply with the same standards.


Covered entities need to inform individuals of their privacy practices, including how their information is used, and provide the right to file violation complaints against the entity as well as the HHS. Individuals also have the right to access their own ePHI. Outside of that, ePHI also can be disclosed to third-parties for “Public Interest and Benefit Activities”, an umbrella term for cases where sharing information is required by law, such as when part of an investigation, public health activities, domestic violence, organ donation, etc. Other uses of ePHI need written authorization. Organizations need workforce training around general HIPAA compliance as well as their specific privacy policies. Organizational policies should be documented for 6 years past both creation and last effective date.


The HIPAA privacy rule (Standards for Privacy of Individually Identifiable Health Information) sets up a framework around how medical information for patients and users can be transmitted and stored. HIPAA compliance *does not require* third-party auditing or certification, though there are a lot of companies that provide services around this. Compliance is enforced through financial penalties: violations are subject to (inflation-adjusted) fines that are roughly between $100 and $50,000 per instance of violation. There have been cases of companies paying millions of dollars in fines when ePHI is exposed.


##


**HIPAA Compliance Penalties**


HIPAA penalties are assigned per instance of violation, and fines are capped by the number of years a particular violation has been occurring.


##


**HIPAA “Security Rule”**


[https://www.hhs.gov/hipaa/for-professionals/security/index.html](https://www.hhs.gov/hipaa/for-professionals/security/index.html)


The HIPAA Security Rule covers how Covered Entities should secure ePHI:


1. Ensure confidentiality, integrity, and availability of ePHI.
2. Identify and protect against reasonably anticipated threats to the security or integrity of the information.
3. Protect against reasonably anticipated and impermissible use or disclosure.
4. Ensure workforce compliance.


For a SaaS company, securing ePHI involves a combination of proper organizational and product security considerations. We detail some of our recommendations below. Anticipating and protecting against threats typically involves some sort of documented risk analysis.


Workforce compliance involves security and HIPAA training for employees as part of onboarding, as well as annual retraining to ensure that everyone is up-to-date on HIPAA rules and best practices.


Risk analysis involves identifying potential risks that might impact the security of availability of ePHI, evaluating potential impact and likelihood of occurrence, and then any action items or tasks that could be implemented to reduce either the impact or likelihood of occurrence of the risk. Examples of categories of vulnerabilities are shown below.


- Vendors (subprocessers that have access to ePHI being compromised)
- Device Security (lost or stolen devices)
- Facility Security (trespassing)
- Black hat hackers
- Human error (accidental data deletion, inaccurate data entry)
- Leaked information or trapdoors in the codebase
- Natural / environmental catastrophes (e.g., floods, earthquakes, and power outages)


An example of a risk analysis of a vulnerability is shown below:


**Risk:**


Environmental factors can result in an AWS region being offline.


**Impact:**


Affects ePHI availability, most likely will not result in a data breach.


**Likelihood**


Datacenters being knocked offline is a rare event. This event is not very likely.


**Action Items:**


Host data on multiple availability zones if possible.


##


**HIPAA Breach Notification Rule**


[https://www.hhs.gov/hipaa/for-professionals/breach-notification/index.html](https://www.hhs.gov/hipaa/for-professionals/breach-notification/index.html)


The HIPAA Breach Notification Rule specifies how if covered entities and business associates discover a breach in their ePHI, they need to notify individuals, the public, and the HHS. Breaches, as defined by the HHS, typically involve unauthorized access of ePHI information from outside of the organization. A breach involves unsecured ePHI that has not been rendered unusable through something like encryption. Notifications must be provided within 60 days of the discovery of a breach.


##


**Organizational** **Security Considerations**


SaaS companies require the same physical organizational security considerations as non-software businesses that deal with ePHI. There are also additional security considerations for the software developed in-house as well as third-party software libraries used.


- Facility security: limit physical access to facilities


- Keys, key codes, keycards, and ID cards are useful in controlling ingress to offices and buildings


- Device security:


- Any electronic services used should require passwords with difficulty requirements. NIST publishes password recommendations, such as having a minimum length of 8.
- Computers should have encrypted disks


- Account Security:


- Only authorized users can access ePHI
- Multi-factor authentication MFA is a very good choice here \[1\], though SMS is not the best choice.[https://www.cnet.com/news/privacy/do-you-use-sms-for-two-factor-authentication-heres-why-you-shouldnt/](https://www.cnet.com/news/privacy/do-you-use-sms-for-two-factor-authentication-heres-why-you-shouldnt/)


- Codebase security, availability and correctness


- Human-based code review processes, unit testing, integration testing, linting and static analysis


- A process of managing provisioning and de-provisioning of access to facilities, devices or accounts. Onboarding, off-boarding processes and documents with responsible parties explicitly listed.


- Single Sign-on (SSO) helps in ensuring employee identity and access management is centralized
- Employees should use password managers for accounts that are not authenticated through SSO


- Make sure ePHI is transmitted securely and properly through all mediums, whether it be internet, fax, phone, mail.
- Data processor / vendor review


- Make a comprehensive list of all data processors and note what they are used for, whether or not that processor handles ePHI or PII. Work to ensure that each vendor either has a BAA in place or does not handle ePHI.


Entity Name Use ePHI Other PII Notes


Amazon Web Services Cloud Hosting Yes Yes BAA in place


Google Workspace Email, internal documents No Yes


<fill in> Source code management


<fill in> Internal messaging


<fill in> Bug tracking


<fill in> Error reporting


<fill in> Video conferencing


<fill in> Product analytics


<fill in> Infrastructure monitoring


<fill in> Email marketing


<fill in> Payment processing


Sample Data Processor Review


##


**Product** **Considerations**


The following product security considerations target ePHI, but for a SaaS company, it probably makes sense to treat all user data with the same measures as ePHI. Rigorous security and availability policies can help with delivering a good user product experience and brand reputation


- ePHI Storage


- Encryption of ePHI in databases and file storage.


- ePHI transmission


- Encryption of ePHI during transmission, for example over HTTPS secured by up-to-date TLS protocols. Making sure certificates are up to date, doing SSL server and port scanner testing.


- ePHI Audit Logs


- Recording administrator access, user and activity logging around access of environments with ePHI, or downloading and modification of data.


- ePHI Access and Integrity


- Making sure ePHI is not improperly read or written (or deleted). This means consistently securing application-level access to make sure users cannot access others information. Other methods of data access through networks, servers and ports also need to be secured or hardened. Vulnerabilities in the operating system, library, and application logic should be patched. Third-parties can be engaged for penetration testing or bug-bounties to find vulnerabilities.


- ePHI Availability


- Making sure data is accessible involves incorporating engineering best practices to maximize certain attributes. **Infrastructure** **Scalability** is important for handling request spikes or high traffic loads. ****There are physical limits to how much products can scale “vertically” with more powerful devices. Eventually products and the services behind them all need to be scaled “horizontally” across multiple devices. **Compute redundancy** can help with uptime and handling power or server outages. **Product development processes** like code-reviews, automated unit and integration testing, quality assurance testing and gradual roll-outs can minimize the impact of breaking changes. **Application Performance Monitoring (APM)** tools can help notify engineers around degraded performance or outages. **Error and Crash reporting tools** can capture stack traces or information around when an application behavior that should be fixed. **Database** **Backups** where stored data is periodically backed up, and the restore process is exercised to make sure that no data is lost


##


**Observability Tools and Compliance Software**


There are security standards that require third-party certification or auditing, such as SOC2 or PCI DSS. Typically a software platform that can help with automating security monitoring and observability or gathering data and evidence for compliance auditors. Here’s an example of a set of companies that have products that provide such services: Vanta, Drata, Secureframe, and Tugboat Logic.


For security standards that are self-certifying, like HIPAA, the software platform is arguably less useful, as the benefit of automated compliance evidence gathering for third-party auditors is no longer there. Companies will need to decide if it’s worth the cost to have a platform to help in monitoring compliance.


##


**Security Software**


Software services can also involve the monitoring of software stacks a company usages. They can also help with monitoring for cyber attacks and prevention and help with uptime, or just general security hardening. These services can help with some of the following tasks:


- Vulnerability tracking for software libraries, reported to places like CVE or OSV.
- Monitoring and automatic updating of package dependancies and updating them, such as Depandabot or Snyk.
- Monitoring of source code to ensure service secrets are not get committed.
- Static code analysis tools such as Sourcegraph or CodeQL.
- Realtime vulnerability scanners such as Netsparker or Nessus.
- DDOS attack mitigation services from CDNs such as Cloudflare or Akamai .
- Larger companies have Security Information and Event Management (SIEM) requirements, and can use services like IBM QRadar or Alien-vault for log management.
- Host Intrusion Prevention System or Network Intrusion Prevention Systems, like Snort or Suricata.


Security services and products can come in paid and open-source variants, and may come with tradeoffs between price, ease of use, support and configurability. They aren’t necessarily required for HIPAA compliance, but are part of the available toolkit software companies can use to secure their services.


##


Is there Certification for HIPAA?


The HSS does not require an official certification or third-party audit for HIPAA compliance. There are companies out there that offer optional services in terms of auditing and helping with compliance. However, as a company that handles ePHI, it is important to protect private data, both in terms of product and public trust, but also to avoid hefty fines and other severe consequences. Compliance requires an understanding of the regulations set forth by HHS and also a continuous implementation of security and availability controls. Demonstrating security and compliance is an important signal to customers as well as business partners. Contact us if you have any questions or concerns about integrating Sapling into your workflow in a HIPAA compliant manner.
