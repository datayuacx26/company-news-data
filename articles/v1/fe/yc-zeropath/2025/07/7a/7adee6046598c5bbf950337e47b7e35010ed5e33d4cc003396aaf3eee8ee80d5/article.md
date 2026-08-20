---
schema_version: "1.0.0"
document_id: "7adee6046598c5bbf950337e47b7e35010ed5e33d4cc003396aaf3eee8ee80d5"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-25a2b11929a5"
canonical_url: "https://zeropath.com/blog/what-is-pci-dss-12-requirements-to-be-pci-dss-compliant"
published_at: "2025-07-16T00:00:00+00:00"
first_seen_at: "2026-07-24T06:49:27.067531+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:de897fff725f05a7cd99c81afea806e65bfdc7aab921e9a05657f12f35861490"
---

# What is PCI DSS? 12 Requirements to be PCI DSS Compliant

PCI DSS stands for Payment Card Industry Data Security Standard. It’s a set of security requirements designed to ensure that all companies that process, store, or transmit credit card information maintain a secure environment.


PCI-DSS is one of the most famous standards under the "PCI Compliance" umbrella term. If you are curious and unsure which PCI standard applies to you, we have provided a detailed breakdown of all types of PCI compliance in our["What is PCI Compliance?"](https://zeropath.com/blog/what-is-pci-compliance-does-your-business-need-pci-compliance) blog.


## Who needs PCI-DSS Compliance?


Before we discuss the specifics of PCI-DSS, it's beneficial to understand how different players fit into this landscape and who bears the most compliance responsibilities.


So, a PCI-DSS is required by any organization (no matter the size) that stores, processes, or transmits payment-card account data.


This captures two broad populations.


-


**First are merchants** : the coffee shop with a single countertop terminal ([different PCI standards for day-to-day payment mediums like terminal machines](https://zeropath.com/blog/what-is-pci-compliance-does-your-business-need-pci-compliance) ), the local e-commerce start-up accepting card payments through Shopify, and retailers such as Walmart or Amazon that operate an enormous volume of card data spanning online and brick-and-mortar sales. Visa, Mastercard, AMEX, Discover, and JCB classify all these merchants into four categories based on their transaction-volume “levels,” so even a sole proprietor who handles a handful of card payments a day is technically in scope, while a firm like Target, which processes millions of transactions a year faces heavier validation and annual on-site assessments.


-


**Second are service providers** : products that store, process, or transmit cardholder data on behalf of others. Payment gateways, such as Stripe, Adyen, or Square, are examples of this category, which handle card data for thousands of merchants.


Also, note that the data center and managed-hosting companies, such as Equinix, call-center outsourcers that record card numbers for reservations (think Marriott’s reservation hotline run by a BPO), and even tokenization or fraud-scoring SaaS vendors are likewise considered service providers and must be PCI-DSS compliant if they ever touch the primary account number or related authentication data.


Therefore, this way both categories get compliant.


During the Home Depot’s self-managed point-of-sale network breach in 2014, the compromise exposed approximately fifty million card numbers; investigators later linked the initial intrusion to lax segmentation and password reuse, areas that the PCI-DSS clearly addresses in its core requirements.


In 2020, the cloud service provider Jelly Bean received fines after investigators discovered that merchants using its web-checkout widget were unintentionally collecting raw magnetic-stripe data, placing every one of those merchants out of compliance, even though they had never seen the card numbers directly.


## Breakdown of PCI-DSS Compliance


### Current Version: PCI DSS 4.0.1


- Released: March 31, 2022 (4.0), Updated June 2024 (4.0.1)
- Mandatory Since: April 1, 2024
- PCI DSS 3.2.1 retired: March 31, 2024
- Future requirements become mandatory: March 31, 2025 (51 new requirements)


### The 12 Core Requirements


To be PCI-DSS compliant, an organization must meet these 12 foundational requirements, which primarily ensure that data is collected, transmitted, and stored securely with proper implementation of software development practices.


#### 1. Build and Maintain a Secure Network


Firewalls create the first line of defense between trusted internal networks and the open internet, as well as within an organization, between the cardholder data environment (CDE) and the rest of the corporate network.


If you are looking to be PCI-DSS compliant, start by drawing a simple diagram of every place your company’s systems touch the public internet and every internal segment that will ever see cardholder data. Anything inside the “cardholder-data environment” (CDE) must be protected by a firewall or cloud security group that blocks all traffic except that explicitly required by a payment function.


#### 2. Eliminate default credentials and settings


Every factory password, SNMP string, demo certificate, and open service must be changed or disabled before a system goes live. Walk through each router, switch, virtual machine image, SaaS admin panel, payment terminal, and development stack. Change or disable factory passwords, demo accounts, “admin/admin” logins, public SNMP strings, and self-signed test certificates before the device ever sees production traffic. If you maintain golden images in a CI/CD pipeline, bake these hardening steps into the pipeline so new servers launch in a secure state by default.


#### 3. Encrypt or truncate stored card data


Any primary account number (PAN), expiration date, or cardholder name that resides on disk, in database tables, or backups must be rendered unreadable through strong cryptography or irreversible hashing, with cryptographic keys stored and rotated under strict management. Once the authorization is complete, orgs should not retain any of the additional sensitive authentication data (full magnetic-stripe contents, CVC2/CVV2, PIN blocks). By ensuring that a compromised storage platform reveals nothing useful, this requirement significantly limits the financial value of a breach and the incentive to attack.


#### 4. Encrypt card data in transit


Whether data travels between a point-of-sale terminal and a payment processor, or between cloud microservices, the traffic must use up-to-date secure protocols (TLS 1.2+ or authenticated VPN tunnels), strong cipher suites, and properly validated certificates. Encryption in transit closes a common interception path: network sniffing, rogue access points, and man-in-the-middle attacks become ineffective because the attacker sees only ciphertext.


#### 5. Remove malware on every vulnerable system


PCI-DSS requires centrally managed, signature-based, and behavior-based anti-malware solutions on every workstation and server commonly targeted by malicious code, with continuous updates, alerting, and tamper protection.


Deploy an endpoint-detection-and-response (EDR) agent to every workstation and server that can reach the CDE. Turn on real-time scanning, automatic signature updates, and tamper protection; forward alerts to a shared security inbox or SIEM. If you build container images, run malware scans in the CI pipeline, and block the build when high-severity detections appear.


#### 6. Deploy and maintain secure systems and applications.[Zeropath is helping companies be more secure for PCI Compliance.](https://zeropath.com/blog/how-to-meet-security-requirements-for-pci-dss-compliance)


As a company handling user-sensitive data, you must ensure that the software is secure from all types of vulnerabilities, including recent and previously disclosed ones. Track vendor security advisories, patch critical vulnerabilities within one month, follow secure coding practices, and perform code reviews either manually or scale it better with SAST tools like ZeroPath in the pipeline.


We understand if this sounds like a lot of things to keep track of, and it is. Therefore, AI Native SAST tools like ZeroPath make security checks for PCI compliance easier for larger teams. ZeroPath runs periodic scans over the internal codebase and identifies vulnerabilities, such as authentication flaws and business logic issues, that other SAST tools still can't detect.


If you're interested in learning how ZeroPath works, you can find a[detailed behind-the-scenes here](https://zeropath.com/blog/how-zeropath-works) .


#### 7. Access control policies:


These policies must map every account, role, and privilege to a documented business justification, adhering to the principle of least privilege. The standard requires role-based access control, separation of duties, and formal authorization workflows. Limiting who can read, write, or query card data reduces the leakage significantly.


Create role-based access control (RBAC) groups that reflect specific job duties, such as “Support-L1,” “DBA-Read,” and “Dev-No-Prod.” Nobody outside the Payment Operations team should be able to view full PANs, and no single person should be responsible for both approving and deploying code to production. Review group membership every quarter; use an identity-governance tool that automatically deactivates accounts after sixty days of inactivity.


#### 8. Tie every action to a unique, MFA-protected identity


Each person or automated process must use a unique ID, with multi-factor authentication for administrative and remote access. Strong password policies, account-lockout thresholds, and periodic credential rotation are mandatory. These unique identifiers help security teams reconstruct a breach timeline by knowing exactly which identity performed each action. MFA also helps prevent stolen-credential attacks that often target remote access services.


#### 9. Restrict physical access to cardholder data


Servers, point-of-sale devices, paper storage, and networking closets that contain the CDE must be located in monitored, access-controlled facilities with visitor logging and secure media handling procedures. Physical controls complement logical controls. If someone can walk off with an unencrypted backup tape or plug a hardware keylogger into a cash register, software defenses are not really of much use.


#### 10. Collect and store tamper-proof logs


Configure syslog or agent-based forwarding from every firewall, server, application, and database into a centralized SIEM. Retain logs for twelve months, keeping the most recent three months “hot” for immediate search.


All this data will enable real-time alerting and post-incident analysis.


#### 11. Regularly test security systems and processes


For most companies, depending on their transaction volume, this could involve a scheduled quarterly to yearly external and internal vulnerability scan using an ASV-approved scanner. If you are actively looking for such scanners, here is a comprehensive list.


Along with ASV scans, most companies also conduct a full annual pentest or whenever they launch a significant new payment flow. You can get a[full pentest from ZeroPath](https://zeropath.com/products/penetration-tests) itself, which is going to be faster, efficient, and much more cost-effective than other solutions out there. ZeroPath's average turnaround time for a pentest has consistently been less than a week.


#### 12. Security in policy and governance


Last but not least, a crucial point in this compliance is how we make decisions, how transparent they are, and how we share them within the team.


Companies should have an information-security policy that clearly states who owns each control, outlines the process for reporting violations, and specifies how to handle incidents. Have senior leadership sign it, publish it on the company wiki, and train every employee on the parts that apply to their role.


Maintain a third-party inventory listing of every vendor that handles card data, require them to prove their own PCI compliance annually, and include a right-to-audit clause in new contracts. Finally, companies should reach out to a Qualified Security Assessor (QSA) to perform a formal gap assessment and guide them toward the correct Self-Assessment Questionnaire (SAQ) or Report on Compliance (ROC) required for their merchant or service-provider level.


## Conclusion & Next Steps


Many people and organizations read PCI DSS as a to-do list, but at its core, it's more about an engineering design discipline.


Firewalls, encryption, and RBAC give card data a hardened perimeter, yet the system is only as strong as the code your team ships every day. Requirement 6, which is deploying and maintaining secure systems and applications, is one of the most crucial requirements in this process. Because software changes with every commit, this requirement isn't a one-time config but more like a continuous check, which in many teams needs to happen multiple times a day.


That is precisely where a security platform, rather than a compliance cheatsheet, is required. ZeroPath’s platform addresses this by starting with its core[best-in-class, AI-native SAST engine](https://zeropath.com/products/sast) . Following that, you also get to secure your software supply chain with[Software Composition Analysis (SCA)](https://zeropath.com/products/sca) , prevent insecure configurations with[Infrastructure as Code (IaC) scanning](https://zeropath.com/products/iac) , and even[detect and validate leaked secrets](https://zeropath.com/products/secrets) hidden deep within your codebase.


That's one complete AI-native AppSec solution that meets and exceeds compliance needs.


This continuous process begins on the first run, where ZeroPath indexes and scans your entire application. This could also be the first for your team to shift left. From that point on, security is embedded directly into the development lifecycle through automated[PR Reviews](https://zeropath.com/products/pr-reviews) , ensuring developers can catch vulnerabilities before they are ever merged. For many common issues, ZeroPath can even[automatically fix security vulnerabilities](https://zeropath.com/products/sast-autofix) with AI-powered code remediation.


While these might seem like building a lot of tooling infra, it really comes down to embedding these controls directly into the developer workflow through[CI/CD and tool integrations](https://zeropath.com/products/integrations) and automatically syncing findings with[AppSec Risk Management](https://zeropath.com/products/risk) tools like Jira, Linear, Asana, GitHub, GitLab, Azure DevOps and many more.


ZeroPath makes Requirement 6 from an annual audit concern into a background service that takes care of itself autonomously.


If PCI-DSS is on your roadmap and you need help meeting security requirements, the ZeroPath team is more than happy to help. Moreover, we also have a[detailed walkthrough of how you can achieve the security requirement for PCI-DSS with ZeroPath](https://zeropath.com/blog/how-to-meet-security-requirements-for-pci-dss-compliance) .
