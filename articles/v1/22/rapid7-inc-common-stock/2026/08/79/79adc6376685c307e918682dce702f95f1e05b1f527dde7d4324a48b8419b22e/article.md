---
schema_version: "1.0.0"
document_id: "79adc6376685c307e918682dce702f95f1e05b1f527dde7d4324a48b8419b22e"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-63520-microsoft-sharepoint-remote-code-execution-fixed"
published_at: "2026-08-11T13:00:00+00:00"
first_seen_at: "2026-08-11T14:41:45.349327+00:00"
fetched_at: "2026-08-11T14:41:47.071904+00:00"
content_hash: "sha256:83e4f0d36402d74d2af02349dfea6bf4ccbe7f9944de05774eaafbd035a6255a"
---

# CVE-2026-63520: Microsoft SharePoint Remote Code Execution (FIXED)

## Overview


Rapid7 Labs conducted a zero-day research project against Microsoft SharePoint, resulting in the discovery of two new vulnerabilities that, when chained together, achieve unauthenticated remote code execution (RCE) against a vulnerable SharePoint server. Today, both Rapid7 and Microsoft are disclosing the second vulnerability in this chain, the RCE vulnerability CVE-2026-63520. The first vulnerability in the chain, CVE-2026-55040, was


[disclosed](https://www.rapid7.com/blog/post/ve-cve-2026-55040-microsoft-sharepoint-jwt-token-authentication-bypass-fixed/) by Rapid7 and Microsoft last month.


Our full disclosure timeline for the exploit chain can be seen below in Figure 1.


*Figure 1: The road to disclosure.*


⠀


CVE-2026-63520 affects all supported versions of Microsoft SharePoint, and certain versions of Microsoft Project Server and Microsoft Office Web Apps Server. For the purpose of our research, we focused solely on SharePoint. An attacker can leverage CVE-2026-63520 to execute arbitrary code on a vulnerable SharePoint server with the privileges of the SharePoint Site’s service account. The vulnerability is due to an unsafe .NET type instantiation issue within the


[Business Connectivity Services](https://learn.microsoft.com/en-us/sharepoint/administration/business-connectivity-services-overview) .


CVE-2026-63520 has a CVSSv3.1 score of


[8.1 (High)](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H/E:U/RL:O/RC:C) , and a Common Weakness Enumeration (CWE) of


[CWE-20: Improper Input Validation](https://cwe.mitre.org/data/definitions/20.html) . While the severity of the RCE is described as high, chained together with CVE-2026-55040 it becomes part of a critical unauthenticated RCE exploit chain against SharePoint.


The exploit chain was developed as an entry for this year's


[Pwn2Own Berlin](https://www.zerodayinitiative.com/blog/2026/5/15/pwn2own-berlin-2026-day-two-results) hacking competition; while our entry was unsuccessful on the day of the competition, this research highlights Rapid7 Labs' continued effort to


[raise the bar](https://www.rapid7.com/blog/post/ve-rapid7-labs-at-pwn2own-vuln-intel) in Vulnerability Intelligence and our commitment to the preemptive protection of our customers through original vulnerability research. Our research methodology focused on understanding how publicly available AI models can assist in the discovery of significant vulnerabilities against proprietary enterprise targets. Our results established that the rate of model advancement is significantly accelerating vulnerability research, model guidance from subject matter experts is a force multiplier, and complex proprietary targets are easily handled through agentic workflows.


Rapid7 is hosting a webinar on Thursday August 13, 2026 to discuss the research and findings for CVE-2026-55040 and CVE-2026-63520. Please


[join](https://www.brighttalk.com/webcast/10457/673829?utm_source=blog&utm_medium=website&utm_content=microsoft-sharepoint-webinar&utm_campaign=na-vrm-q3-2026-global-webinar-prospect-eng-etos-25) Douglas McKee and Stephen Fewer to learn more about this body of work.


## Workflow


For this research project we wanted to understand the capabilities and limits of publicly available LLMs circa January through to March of this year. We wanted to answer the question if an AI workflow could find and develop an unauthenticated RCE exploit against a hard target such as SharePoint. This research project concluded with the successful discovery and development of such a chain. To that end, the publicly available models at the beginning of this year were indeed capable. This is notable as the rate of model improvement from Q1 of 2026 through to today has been significant. Our team's later testing of the most recent frontier models confirms the significant increase in capabilities from that of the beginning of this year. Our primary conclusion from the SharePoint research project in Q1 is that an agent guided by a subject matter expert (SME) was crucial to keep moving the model and its work towards the end goal. Given our current experience of frontier model capabilities, the need for an SME to verify and guide a model is lessened, but the compounding impact an SME can bring remains.


Our first sprint in January did not result in any significant findings, rather, this sprint helped us establish the workflow and tooling that proved most useful, scope out the extremely large attack surface, and integrate prior work into our process. We augmented the agentic work with manual source code review and reverse engineering to provide additional context and steering to the model. Our early results quickly indicated how a fully automated and agentic approach would not suffice, the model would too often produce findings that were questionable or simply inaccurate. Steering the agent as it worked helped both the agent hone in on interesting and ultimately fruitful findings but also by constantly reviewing the agent's results, helped us refute inaccurate and unhelpful findings, along with several cases where the agent overstepped its guidance, effectively cheating to succeed in its goal - such as unexpectedly replaying admin credentials, enabling debug flags, or reading secrets, all of which were never within our original threat model.


By March, we had moved to a newer release of our chosen model, that combined with a solid attack surface, extensive prior work in place, and a broad architectural layout mapped out we began to quickly see success. An authentication bypass, now known as CVE-2026-55040, was discovered and verified in early March, followed two weeks later by the RCE, now known as CVE-2026-63520. By the time we had produced a working exploit chain, the agent had accrued 120 hours of run time spread over 24 days, leveraged 96 sessions, generated approximately 80,000 agentic tool calls and we had issued 256 prompts.


## Product descriptions


The RCE vulnerability, CVE-2026-63520, affects SharePoint, Project Server, and Office Web Apps Server, while the authentication bypass vulnerability, CVE-2026-55040, affects only SharePoint.


### SharePoint


Microsoft


[SharePoint](https://www.microsoft.com/en-ie/microsoft-365/sharepoint/collaboration) is a ubiquitous, web-based collaboration and document management platform deeply integrated into the Microsoft 365 ecosystem. Serving as the central hub for corporate intranets, internal file sharing, and workflow automation, it is trusted by enterprises worldwide to store and manage vast repositories of sensitive business data. Because SharePoint acts as a critical bridge between internal users, active directories, and cloud infrastructure, vulnerabilities within its architecture present a high-risk attack surface.


### Project Server


Microsoft


[Project Server](https://en.wikipedia.org/wiki/Microsoft_Project_Server) is an enterprise project portfolio management (PPM) platform built natively on top of the SharePoint architecture. Serving as the central hub for corporate scheduling, resource allocation, and capacity planning, it enables organizations to coordinate complex business initiatives.


### Office Web Apps Server


Microsoft


[Office Web Apps Server](https://learn.microsoft.com/en-us/webappsserver/office-web-apps-server-overview) is a dedicated companion service that provides browser-based viewing and editing of Microsoft Office documents. Functioning as the primary rendering engine for SharePoint and Exchange, it enables seamless file interaction without requiring local desktop installations.


## Impact


CVE-2026-63520 allows an attacker to execute arbitrary code on an affected server. By crafting a custom .NET gadget chain, an attacker can perform arbitrary operations such as executing an attacker-controlled OS command. The attacker's arbitrary code is executed with the permission of the Windows service account running the SharePoint Site instance. As CVE-2026-63520 can be chained to the authentication bypass vulnerability, CVE-2026-55040, the resulting exploit chain allows for unauthenticated RCE against a vulnerable server.


## Credit


This vulnerability was discovered by Stephen Fewer, Senior Principal Security Researcher at Rapid7 and is being disclosed in accordance with


[Rapid7's vulnerability disclosure policy](https://www.rapid7.com/security/disclosure/) .


## Vendor statement


The following statement has been provided by Microsoft:


*“We would like to thank Rapid7 for responsibly reporting this issue through coordinated vulnerability disclosure.”*


## Technical analysis


Rapid7 will be publishing full technical details for the RCE vulnerability, CVE-2026-63520, within 30 days of this disclosure.


The technical details for the authentication bypass vulnerability, CVE-2026-55040, have been published


[here](https://www.rapid7.com/blog/post/ra-microsoft-sharepoint-jwt-token-authentication-bypass-cve-2026-55040/) .


## Remediation


The following products are impacted by CVE-2026-63520:


-


Microsoft SharePoint Server Subscription Edition


-


SharePoint Server Subscription Edition Language Pack


-


Microsoft SharePoint Server 2019


-


Microsoft SharePoint Enterprise Server 2016


-


Microsoft Project Server 2013 Service Pack 1 (64-bit edition)


-


Microsoft Office Web Apps 2013 Service Pack 1


Customers are advised to apply the latest available


[updates](https://learn.microsoft.com/en-us/officeupdates/sharepoint-updates) for the impacted product to ensure they are protected.


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM and Nexpose customers will be able to assess their exposure to the RCE vulnerability, CVE-2026-63520, with authenticated vulnerability checks available in the August 12 content release. Customers can assess their exposure to the authentication bypass vulnerability, CVE-2026-55040, with authenticated vulnerability checks available in the July 15 content release.


## Upcoming webinar


Interested in the AI tooling leveraged throughout the research process? Join Rapid7's Stephen Fewer and Douglas McKee on Thursday, August 13 to walk through the full exploit chain, actionable next steps and more.[Register here](https://www.brighttalk.com/webcast/10457/673829?utm_source=Rapid7&utm_medium=brighttalk&utm_medium=organic-social&utm_campaign=673829%3Futm_source%3Dlinkedin&utm_campaign=global-mdr-q3-2026-global-webinar-prospect-eng-etos-25&utm_content=microsoft-sharepoint-webinar&utm_term=touch1) .


## Disclosure timeline


-


**May 18, 2026:**


Rapid7 discloses an unauthenticated RCE exploit chain to Microsoft. Microsoft acknowledges receipt of the disclosure the same day.


-


**May 20, 2026:**


Microsoft confirms the findings and indicates that the exploit chain will be patched across two scheduled update cycles - the authentication bypass component in July, and the RCE component in August.


-


**May 21, 2026:**


Rapid7 acknowledges the disclosure schedule and requests supporting information. Microsoft requests a 30 day stay on disclosure of technical details and publication of PoC.


-


**May 29, 2026:**


Rapid7 agrees to a 30 day stay on technical details with a proviso to publish earlier should either exploitation in-the-wild or third-party publication of details occur within the 30 days. Microsoft confirms the disclosure plan the same day.


-


**July 21, 2026:**


Rapid7 requests supporting information for the upcoming disclosure.


-


**July 31, 2026:**


Microsoft provides supporting information to Rapid7.


-


**August 11, 2026:**


This disclosure for CVE-2026-63520.
