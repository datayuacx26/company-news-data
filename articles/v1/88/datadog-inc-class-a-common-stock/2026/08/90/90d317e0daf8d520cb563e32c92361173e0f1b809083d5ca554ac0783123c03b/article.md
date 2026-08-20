---
schema_version: "1.0.0"
document_id: "90d317e0daf8d520cb563e32c92361173e0f1b809083d5ca554ac0783123c03b"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/cisa-bod-26-04-vulnerability-prioritization/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T16:24:41.406781+00:00"
fetched_at: "2026-08-19T16:24:44.379908+00:00"
content_hash: "sha256:625c727867711ae33e9cc466f14068012994b7724c4dae18837f14fc5f8d736f"
---

# How CISA’s BOD 26-04 changes vulnerability prioritization

Christina DePinto


Senior Product Marketing Manager


Sophie Wang


Senior Product Marketing Manager


AI-accelerated attacks are redefining the threat landscape, but many of them still rely on one of the oldest tactics in the book: exploiting known vulnerabilities. The difference today is speed. Vulnerabilities that once took skilled hackers months or weeks to exploit can now be weaponized in hours or minutes. This acceleration is forcing organizations to rethink how they identify and remediate risk.


To help contend with the high volume and velocity of attacks, the[Cybersecurity and Infrastructure Security Agency (CISA)](https://www.cisa.gov/) released[Binding Operational Directive (BOD) 26-04: Prioritizing Security Updates Based on Risk](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) . This mandate changes how federal agencies must prioritize vulnerability remediation. In this post, we’ll explore:


-


The requirements of BOD 26-04


-


The challenges of achieving compliance


-


How the Datadog Runtime Prioritization Engine can help customers meet their compliance obligations


## What is BOD 26-04?


BOD 26-04 codifies risk-based prioritization and patching timelines for federal agencies based on four key variables:


1.


**Asset exposure** : Is the vulnerable asset publicly exposed?


2.


**Known exploited vulnerability (KEV) status** : Is the vulnerability, as identified by a Common Vulnerabilities and Exposures identifier (CVE ID), in CISA’s[Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) ?


3.


**Exploit automation** : Is an adversary able to automate all the steps necessary to exploit the vulnerability?


4.


**Technical impact** : Does an adversary gain partial control or total control of the vulnerable asset after exploitation of the vulnerability?


BOD 26-04 establishes remediation timelines that are dependent on the answers to these questions. The most critical vulnerabilities require remediation within **3 days** and include requirements for forensic triage and investigation. Conversely, vulnerabilities that do not meet any of the directive’s risk criteria are deprioritized and can be remediated during the next system upgrade. The following table shows the remediation timelines:


Publicly exposed? In the KEV Catalog? Automatable by adversary? Technical impact Agency timeline (calendar days) for remediation


Yes Yes Yes Total control 3 days and forensic triage


Yes Yes Yes Partial control 3 days


Yes Yes No Total control 3 days and forensic triage


Yes Yes No Partial control 14 days


Yes No Yes Total control 3 days


Yes No Yes Partial control 14 days


Yes No No Total control 14 days


Yes No No Partial control 60 days


No Yes Yes Total control 3 days and forensic triage


No Yes Yes Partial control 14 days


No Yes No Total control 14 days


No Yes No Partial control 14 days


No No Yes Total control 60 days


No No Yes Partial control 60 days


No No No Total control Fix on system upgrade


No No No Partial control Fix on system upgrade


BOD 26-04 focuses patching efforts on the areas of highest risk rather than treating all vulnerabilities and systems equally. It also revokes[BOD 19-02](https://www.cisa.gov/news-events/directives/bod-19-02-vulnerability-remediation-requirements-internet-accessible-systems-revoked) , eliminating the requirement to use the[Common Vulnerability Scoring System (CVSS)](https://www.first.org/cvss/) as the primary mechanism for vulnerability prioritization.


While BOD 26-04 applies specifically to federal agencies, its underlying message is relevant to every security team: Business-aligned prioritization is critical as attack volumes increase. Organizations need to focus on the vulnerabilities that are most likely to be exploited and most likely to affect critical business operations.


## Challenges of BOD 26-04


BOD 26-04 assumes organizations can answer questions such as the following:


-


Is the vulnerable asset actually exposed?


-


Is the vulnerable code running in production?


-


Is the vulnerability likely to be exploited?


-


Would exploitation impact a critical business function?


-


Who is responsible for remediating the vulnerability?


These questions seem straightforward, but answering them at scale across modern cloud environments can be difficult. Many organizations have vulnerability data but lack the contextual depth needed to determine which findings represent meaningful risk. Instead, they rely on manually maintained asset metadata, point-in-time scans, and disconnected tooling.


Even when organizations successfully identify high-priority vulnerabilities, remediation often stalls because security and engineering teams operate in different tools, with different workflows and competing priorities. To understand what issues are critical and how to take action, teams need runtime context in a shared platform.


## How the Datadog Runtime Prioritization Engine can help


The[Datadog Runtime Prioritization Engine](https://www.datadoghq.com/blog/runtime-prioritization-engine/) combines runtime behavior with exploitability, exposure, and business context from Datadog observability and security telemetry data to identify the small percentage of findings that pose real, exploitable risk. As part of[Datadog Cloud Security](https://docs.datadoghq.com/security/cloud_security_management/) , the Runtime Prioritization Engine makes prioritization transparent and explainable by evaluating findings across five dimensions:


-


**Reachability** : Is the vulnerable component actually running in production?


-


**Exposure** : Can attackers realistically reach the affected resource?


-


**Exploitability** : Is there evidence that the vulnerability is likely to be exploited, such as public exploit code, high[Exploit Prediction Scoring System (EPSS)](https://www.first.org/epss/) scores, or inclusion in CISA’s Known Exploited Vulnerabilities Catalog?


-


**Business criticality** : Would a successful compromise impact a critical business service, sensitive data, or a high-value asset?


-


**Actionability** : Is ownership known, and is a fix available so that remediation can happen quickly?


For example, the Runtime Prioritization Engine automatically infers business-critical assets, known as[crown jewels](https://docs.datadoghq.com/security/cloud_security_management/crown_jewels/) , from observability signals such as service dependencies,[APM traces](https://docs.datadoghq.com/tracing/glossary/#trace) , traffic patterns, service level objectives (SLOs), and incident history. This information enables teams to understand not just whether a vulnerability exists, but whether exploitation would impact a critical business function.


The Runtime Prioritization Engine also infers ownership by using operational metadata such as service ownership, deployment information, on-call configurations, source control integrations, and service catalog data. This metadata is combined with runtime package usage, exploit intelligence, and network exposure analysis to give security teams a continuously updated picture of which findings deserve immediate attention, who is responsible for addressing them, and how urgently they should be addressed.


By identifying ownership automatically and integrating directly with engineering workflows, the Runtime Prioritization Engine helps ensure that prioritized findings don’t stop at triage. Teams can route findings and coordinate remediation by using their existing collaboration tools, giving responders the context they need to understand the risk and take action.


The[Datadog Security MCP toolset](https://docs.datadoghq.com/security/mcp_server/) extends the Runtime Prioritization Engine’s capabilities by enabling AI agents to securely access Datadog security context and remediation processes. Teams can use their preferred AI agent to analyze, triage, investigate, and correlate signals and findings while using Datadog as the system of record.


## Accelerate prioritization and remediation for BOD 26-04 with Datadog


BOD 26-04 changes how federal agencies must prioritize and fix cyber vulnerabilities, but the directive’s sentiment is valuable to all security teams: Identify and prioritize the highest risks, and remediate them quickly. The Datadog Runtime Prioritization Engine helps teams achieve these goals with a combination of runtime prioritization, ownership intelligence, workflow integration, and AI-assisted operations that aligns security efforts with business risk. Instead of triaging thousands of findings based primarily on CVSS scores, teams can focus on remediating the vulnerabilities that are running, reachable, exploitable, impactful, and actionable.


For more information,[read the Runtime Prioritization Engine documentation](https://docs.datadoghq.com/security/cloud_security_management/triage_and_prioritize/runtime_prioritization_engine/) . To stay updated about the latest features,[join the Runtime Prioritization Engine Preview program](https://www.datadoghq.com/product-preview/runtime-prioritization-engine/) .


If you’re new to Datadog, you cansign up for a 14-day free trial to start identifying and prioritizing your security risks.


*This post is for informational purposes only. Nothing here constitutes legal advice, a compliance assessment, or a warranty of any kind. While Datadog offers powerful tools to assist customers in achieving their own compliance with government and industry standards, customers are responsible for their own compliance obligations.*


-
-
-
