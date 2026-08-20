---
schema_version: "1.0.0"
document_id: "9b56596f85e5be28b563bb4db5cd6a04f732a13993b7d58d58b9ed39e641b794"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/07/14/how-qualys-etm-identity-detects-responds-to-identity-based-attacks"
published_at: "2026-07-14T18:00:00+00:00"
first_seen_at: "2026-07-20T03:32:58.378885+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:4697571bd6c005be0a77cb5b8e4d4bd6eab23d239f4790d3ba627ce0a3bbee33"
---

# How Qualys ETM Identity Detects Identity-Based Attacks Faster

#### Table of Contents


- Why Identity-Based Attacks Are Growing
- What Are Identity-Based Attacks?
- Common Types of Identity-Based Attacks
- What Is Identity Security Posture Management (ISPM)?
- How an ISPM Differs from Traditional Identity and Access Management (IAM) Platforms
- Why ISPM Matters for Identity Threat Detection and Response (ITDR)
- How Qualys ETM Identity Detects Identity Risk
- How Qualys ETM Identity Helps Respond to Identity-Based Attacks
- From Identity Alerts to Measurable Risk Reduction
- How ETM Identity Fits into the Qualys EnterpriseTruRiskPlatform (ETM)
- Frequently Asked Questions (FAQs)


### Key Takeaways


- Identity-based attacks are among the fastest and most effective intrusion methods because valid credentials let attackers operate as trusted users.
- Techniques like Pass-the-Hash, Kerberoasting, Domain Controller Synchronization (DCSync), and Authentication Server Response Roasting (AS-REP Roasting) are common Active Directory (AD) attacks that exploit AD trust relationships.
- **Qualys ETM Identity** helps organizations discover risky identities, correlate identity and asset risk, and prioritize attack paths based on real business impact.
- Detection and response improve dramatically when identity posture, exploitability, and asset context are unified into a single risk model.
- Qualys ETM Identity operationalizes identity risk reduction through attack path analysis, domain trust mapping, Identity TruRisk™ scoring, and automated remediation workflows.


## Why Identity-Based Attacks Are Growing


Identity is a primary entry point because valid credentials are easier to exploit than hardened infrastructure. According to[Flashpoint’s 2026 Global Threat Intelligence Report](https://www.techradar.com/pro/security/in-2026-cybercrime-has-reached-a-point-of-total-convergence-new-research-claims-ai-attacks-are-taking-over-so-how-can-your-business-stay-safe) , more than 11.1 million machines were infected with infostealers in the last 12 months, resulting in 3.3 billion stolen credentials and cloud tokens. Cybercrime has shifted from “breaking in” to “logging in.”


This shift is being driven by:


- **Credential exposure:** Phishing and infostealers have made stolen credentials widely available.
- **Privilege sprawl:** Access accumulates as employees change roles, projects end, and admin rights are rarely revoked.
- **Inconsistent multi-factor authentication (MFA) coverage:** MFA is rarely enforced consistently across applications, directories, and legacy systems.
- **Directory misconfigurations:** AD and Microsoft Entra ID environments accumulate stale accounts, weak group policies, and overly permissive trust relationships over time.
- **Shadow admins:** Nested group memberships and indirect entitlements grant effective admin rights that traditional access reviews fail to identify.
- **Unmanaged service accounts and machine identities.** According to the[Cloud Security Alliance’s 2026 whitepaper, The Non-Human Identity Governance Vacuum](https://labs.cloudsecurityalliance.org/research/csa-whitepaper-nonhuman-identity-agentic-ai-governance-v1-cs/) , machine identities outnumber human ones by an average of 45:1 across enterprises and 144:1 in cloud-native environments, yet operate with elevated privileges and less oversight.


## What Are Identity-Based Attacks?


Identity-based attacks abuse legitimate human or machine identities instead of exploiting software vulnerabilities.


This includes:


- Compromising **user accounts** via phishing, password reuse, or session hijacking
- Abusing **admin accounts** with excessive or poorly governed privileges
- Exploiting **service accounts and Application Programming Interface (API) credentials** that are rarely rotated or reviewed
- Stealing **authentication tokens** to bypass login controls


## Common Types of Identity-Based Attacks


Security teams don’t need to understand every identity attack technique. They need visibility into the attack patterns and techniques most commonly used in breaches, such as:


### Credential Theft and Account Takeover


Credential theft is the most common entry point, via phishing, password reuse, infostealer malware, token theft, or session hijacking.


**Pass-the-Hash (PtH)** allows attackers to reuse stolen NT LAN Manager (NTLM) hashes to authenticate without cracking passwords. Qualys ETM Identity helps identify the conditions that increase PtH risk, such as legacy NTLM authentication, weak or misconfigured authentication settings, cached credentials, excessive administrative privileges, and exposed lateral movement paths, by correlating identity and access data to reveal how a compromised hash can be used to move across systems.


### Privilege Escalation


Privilege escalation turns a low-value account into a high-value one by exploiting excessive permissions, nested group memberships, or forgotten admin roles. Misconfigured group relationships can unintentionally grant Domain Admin–level access.


Attackers use **DCSync** after obtaining elevated privileges, replicating password hashes and Kerberos secrets directly from a domain controller. DCSync exploits AD replication permissions to extract credential data, bypassing traditional detection. Qualys ETM Identity maps DCSync exposure in its identity graphs, surfacing identities with replication rights so risky relationships can be remediated before they are exploited.


### Lateral Movement Through Identity Paths


Once an identity is compromised, attackers use trust relationships to pivot across resources and privileged accounts.


**Pass-the-Ticket (PtT)** is a common technique used for lateral movement. By stealing valid Kerberos tickets (Ticket Granting Tickets (TGTs) or Ticket Granting Service tickets (TGSs) from memory, attackers can impersonate users across trusted systems without reauthenticating. Qualys ETM Identity visualizes trust relationships and uses Identity TruRisk™ scoring to quantify risk based on an identity’s criticality and the severity of its associated misconfigurations.


### Misconfigured AD and Entra ID


AD and Microsoft Entra ID underpin enterprise identity and access management. Stale accounts, weak group policies, excessive standing permissions, and insecure identity configurations create access paths that go unnoticed until they are exploited.


**AS-REP Roasting** demonstrates how a minor AD misconfiguration can enable AD attacks and become a serious exposure. When Kerberos pre-authentication is disabled for an AD account, attackers can request encrypted authentication data and attempt offline credential cracking without needing valid authentication. Qualys ETM Identity identifies accounts with disabled Kerberos pre-authentication and other risky identity configurations, helping teams remediate exposures before attackers can exploit them.


---


#### Qualys Webinar


### Learn how AD, Entra ID, and Okta environments expose hidden privilege relationships and how to prioritize identity risk using attack path analysis and TruRisk™-based scoring.


[Watch Now](https://www.brighttalk.com/webcast/11673/665480)


---


## What Is Identity Security Posture Management (ISPM)?


ISPM continuously discovers identities, evaluates risk exposure, map attack paths, supports AD posture management, and prioritizes remediation based on business impact.


## How an ISPM Differs from Traditional Identity and Access Management (IAM) Platforms


IAM determines who gets access and under what conditions. ISPM evaluates risk from access, including excessive privileges, misconfigurations, posture drift, and exploitable attack paths.


**Traditional IAM** **ISPM**


**Primary Question** Who can log in, and to what? How exposed are we because of who can log in?


**Focus** Authentication, provisioning, access policies Continuous risk, posture, and exploitability


**Assessment Style** Point-in-time provisioning and periodic reviews Continuous discovery and real-time risk scoring


**Business Context** Limited to access policy Correlates identity risk with asset criticality and exploitability


**Outcome** Access is granted correctly Risk from that access is measured and reduced


## Why ISPM Matters for Identity Threat Detection and Response (ITDR)


ISPM provides the context needed for effective identity threat detection and response. An alert has limited value without understanding the user’s privileges, attack paths, and potential blast radius.


Existing tools (IAM, Identity Governance and Administration (IGA), Privileged Access Management (PAM), ITDR) manage access or detect anomalies, but lack risk correlation across identity exposure. ISPM closes that gap by assessing identity posture, including inventory, entitlements, misconfigurations, and posture drift.


## How Qualys ETM Identity Detects Identity Risk


Qualys ETM Identity detects identity-based attacks by combining continuous discovery, unified risk telemetry, and attack path analysis through these core capabilities:


- **Comprehensive identity discovery:** It discovers human identities and service accounts across AD, Entra ID, Okta, and other identity providers. It maps ownership, lifecycle state, and entitlements to uncover shadow admins and forgotten service accounts.
- **Unified identity risk telemetry:** It consolidates data from Qualys Cloud Agent, native integrations to identity platforms, and third-party tools into a single risk view.
- **Identity Insights that uncover toxic combinations of identity weaknesses:** It correlates multiple identity exposures on a single identity or asset, such as weak password policies, stale administrative privileges, Active Directory Certificate Services (AD CS) misconfigurations, and risky Open Authorization (OAuth) consents, to surface high-impact attack scenarios.
- **Attack path analysis and domain trust mapping:** It visualizes hidden privilege relationships, identifies toxic privilege combinations, and provides contextual visibility into potential attack paths.


- **Identity TruRisk™ scoring:** Each finding receives an Identity TruRisk™ score correlated with asset risk, which is aggregated into Asset and Org TruRisk™. This allows teams to measure exposure, communicate risk clearly, and prioritize targeted remediation.


## How Qualys ETM Identity Helps Respond to Identity-Based Attacks


Effective response prioritizes the most exploitable paths and routes findings into actions that actively reduce risk.


### Prioritize the Most Exploitable Identity Paths


Identity TruRisk™ helps teams prioritize identity paths that expose critical assets or create measurable business impact.


### Reduce Excessive Privileges


Qualys ETM Identity identifies entitlements that create exposure, so teams can remediate them quickly.


### Strengthen Entra ID and AD Security Posture


Qualys ETM Identity supports remediation through 200+ AD policy controls, complemented by out-of-the-box automation scripts and guided actions to operationalize fixes, such as hardening directory settings, removing stale accounts, reviewing privileged groups, and strengthening authentication policies.


### Connect Identity Risk to Risk Operations


It routes identity findings into the same Risk Operations Center (ROC) workflows used for vulnerability, cloud, and asset risk, including ServiceNow and Jira ticketing with evidence and approvals.


### Remediate Identity Risks with Guided Mitigation


ETM Identity provides actionable remediation guidance for identity findings, with mitigation recommendations to address excessive privileges, insecure configurations, and identity exposures. Organizations can validate remediation through rescans and track improvements in their identity risk posture.


## From Identity Alerts to Measurable Risk Reduction


Identity security programs mature by measuring whether identity exposure is decreasing, critical attack paths are closing, and risk reduction is measurable.


Qualys ETM Identity validates completed remediation, automatically updates Identity TruRisk™, and provides audit-ready reporting aligned with Defense Information Systems Agency Security Technical Implementation Guides (DISA STIG), Center for Internet Security (CIS), Health Insurance Portability and Accountability Act (HIPAA), Payment Card Industry Data Security Standard (PCI DSS), and General Data Protection Regulation (GDPR), while continuously tracking identity-related changes.


## How ETM Identity Fits into the Qualys Enterprise TruRisk™ Platform (ETM)


Qualys ETM Identity is a native capability within ETM, which unifies asset and identity risk into a single TruRisk™ score.


Vulnerable workloads, stale admin accounts, misconfigured domain trusts, and AD posture risks are evaluated using standardized risk metrics and business context. ETM Identity integrates with AD, Entra ID, Okta, and third-party identity tools, extending existing investments.


---


**Start your 30-day free trial and surface hidden attack paths, prioritize real exposure, and shut down identity-driven risk before it escalates.**


[Sign Up Now](https://www.qualys.com/forms/etm-identity)


---


## Frequently Asked Questions (FAQs)


### What Is ISPM?


ISPM discovers human and machine identities, evaluates risky access, detects identity exposures, and prioritizes attack paths based on business impact. Unlike IAM and IGA tools, which handle authentication and access provisioning, ISPM operates alongside them, focusing exclusively on measuring and reducing the risk created by existing access to prioritize what attackers can exploit.


### How Do Identity-Based Attacks Work?


Identity-based attacks exploit legitimate human or machine identities by leveraging stolen credentials, excessive privileges, weak authentication, or misconfigured access controls. Attackers exploit trusted identities instead of software vulnerabilities.


### Why Are Identity-Based Attacks Increasing?


Credential theft has become cheap and scalable; privileges accumulate over time, and machine identities such as service accounts and API keys now vastly outnumber human accounts with less oversight. Combined with inconsistent MFA coverage and directory misconfigurations, organizations have more identity-based entry points than ever before.


### What Is ISPM in Cybersecurity?


ISPM is the continuous practice of assessing and hardening the security posture of identities across hybrid environments. It evaluates inventory, entitlements, misconfigurations, and posture drift, so security teams understand exposure before an incident occurs.


### How Is ISPM Different from IAM?


IAM controls who gets access and under what conditions. ISPM continuously evaluates the risk posed by that access, including excessive privileges, misconfigurations, posture drift, and exploitable attack paths, even when IAM-granted access is correct.


### How Does Qualys ETM Identity Detect Identity Risk?


Qualys ETM Identity discovers identities across AD, Entra ID, and other identity providers, consolidates risk telemetry from Qualys Cloud Agent and third-party sources, and identifies hygiene gaps, toxic privilege chains, and misconfigurations. It then maps attack paths and domain trust relationships and scores each identity with Identity TruRisk™.


### How Does Qualys ETM Identity Help Respond to Identity-Based Attacks?


Qualys ETM Identity prioritizes the most exploitable identity paths, supports remediation through 200+ AD policy controls and automation, and routes actions through ServiceNow and Jira workflows.


### Why Is Identity Risk Correlation with Asset Risk Important?


An identity’s risk depends not only on its configuration, but also on what it can reach. A stale account on a low-value system may be low priority, while the same account with a path to a crown-jewel asset becomes critical. Correlating identity and asset risk helps prioritize what matters most.


### How Does Identity TruRisk™ Help Prioritize Identity Security Risks?


Identity TruRisk™ calculates each identity’s exposure as a quantified score that considers misconfigurations, excessive privileges, and asset exploitability. Because it rolls into Asset TruRisk™ and Org TruRisk™, security teams can prioritize identity remediation alongside vulnerability and cloud risk using a common metric.


### How Can Organizations Reduce Identity-Driven Attack Paths?


Start by discovering every identity, correlating that inventory with asset exposure, and prioritizing the paths that reach critical systems. Reduce excessive privileges, harden AD and Entra ID configurations, enforce consistent MFA, and use closed-loop remediation workflows that verify fixes worked.
