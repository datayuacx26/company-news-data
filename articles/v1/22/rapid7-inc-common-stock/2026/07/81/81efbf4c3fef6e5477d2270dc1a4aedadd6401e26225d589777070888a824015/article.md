---
schema_version: "1.0.0"
document_id: "81efbf4c3fef6e5477d2270dc1a4aedadd6401e26225d589777070888a824015"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/ve-cve-2026-55040-microsoft-sharepoint-jwt-token-authentication-bypass-fixed"
published_at: "2026-07-14T13:00:00+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:bccc6ae00fbe923766629c94ae367cd3b244e22144ca57d56e050dd4e52a73a7"
---

# CVE-2026-55040: Microsoft SharePoint JWT Token Authentication Bypass (FIXED)

## Overview


Rapid7 Labs conducted a zero-day research project against Microsoft SharePoint, resulting in the discovery of two new vulnerabilities that, when chained together, achieve unauthenticated remote code execution (RCE) against a vulnerable SharePoint server. Today, both Rapid7 and Microsoft are disclosing the first vulnerability in this chain, the authentication bypass vulnerability CVE-2026-55040. The RCE component of the exploit chain is expected to be patched by Microsoft in the next update cycle for August 2026. The exploit chain was developed as an entry for the recent


[Pwn2Own Berlin](https://www.zerodayinitiative.com/blog/2026/5/15/pwn2own-berlin-2026-day-two-results) hacking competition – part of Rapid7 Labs' continued effort to


[raise the bar](https://www.rapid7.com/blog/post/ve-rapid7-labs-at-pwn2own-vuln-intel) in Vulnerability Intelligence and our commitment to the preemptive protection of our customers through original vulnerability research.


A remote unauthenticated attacker can leverage CVE-2026-55040 to bypass authentication on a vulnerable SharePoint server and perform operations as a SharePoint site user or administrator. The vulnerability is due to several issues in the JWT token validation pipeline.


CVE-2026-55040 has a CVSSv3.1 score of


[9.1 (Critical)](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N/E:U/RL:O/RC:C) , and a Common Weakness Enumeration (CWE) of


[CWE-1390: Weak Authentication](https://cwe.mitre.org/data/definitions/1390.html) .


## Product description


Microsoft


[SharePoint](https://www.microsoft.com/en-ie/microsoft-365/sharepoint/collaboration) is a ubiquitous, web-based collaboration and document management platform deeply integrated into the Microsoft 365 ecosystem. Serving as the central hub for corporate intranets, internal file sharing, and workflow automation, it is trusted by enterprises worldwide to store and manage vast repositories of sensitive business data. Because SharePoint acts as a critical bridge between internal users, active directories, and cloud infrastructure, vulnerabilities within its architecture present a high-risk attack surface.


## Impact


By leveraging CVE-2026-55040, a remote unauthenticated attacker can assume the identity of any SharePoint site user; the prerequisite is the attacker must know in advance the user they wish to identify as. This can be achieved in a number of ways, including via a user’s Active Directory (AD) Security ID (SID), or via a user’s AD User Principal Name (UPN). A UPN is the primary logon name for a user in either Windows AD or Microsoft Entra ID, and is formatted similar to that of an email address, e.g.


[\[email protected\]](https://www.rapid7.com/cdn-cgi/l/email-protection)


.


In the example screenshot below, with identifying information redacted, a Rapid7 Labs proof-of-concept script discovers potential SharePoint users via SID enumeration and then leverages CVE-2026-55040 to bypass authentication on the target SharePoint site to assume the identity of that user — ultimately identifying the SharePoint site administrator user account.


*Figure 1: The Rapid7 Labs PoC for CVE-2026-55040.*


⠀


An attacker who successfully exploits CVE-2026-55040 can perform operations against the target SharePoint site as the user they identify as. Furthermore, this authentication bypass can be chained to additional vulnerabilities within the authenticated attack surface of the target site.


Rapid7 Labs has chained the authentication bypass CVE-2026-55040 with a separate RCE vulnerability for unauthenticated RCE. Patching CVE-2026-55040 will successfully break this exploit chain; this underscores the importance of patching vulnerabilities such as authentication bypasses, which can break complex and high-impact exploit chains. The RCE component has been disclosed to Microsoft and is expected to be patched in the scheduled August patch cycle.


## Leveraging AI


To develop our SharePoint exploit chain, Rapid7 Labs undertook a research project divided into two main sprints, the first in January and the second in March, 2026. While both sprints did encompass more traditional vulnerability research such as manual code review and reverse engineering, a significant amount of the work was undertaken through an agent. Over 24 active days of agentic work, we leveraged 96 sessions, issued 256 prompts, and generated approximately 80,000 agentic tool calls.


The initial January sprint was unsuccessful, resulting in no findings that could be leveraged for an exploit chain. We used this sprint to experiment with several different publicly available models, along with different workflows to navigate and reason across a massive and complex codebase. However, our second sprint in March was successful and yielded, through a heavily prompted agent, a two-vulnerability exploit chain that achieved unauthenticated RCE.


The improvement in quality between January and March in terms of agentic work, along with our improved workflows, was noticeable. This highlights the speed at which this field is evolving, how publicly available models are improving, and how as research teams develop their workflows, the results begin to compound.


## Credit


This vulnerability was discovered by Stephen Fewer, Senior Principal Security Researcher at Rapid7 and is being disclosed in accordance with


[Rapid7's vulnerability disclosure policy](https://www.rapid7.com/security/disclosure) .


## Vendor statement


The following statement has been provided by Microsoft:


*“We would like to thank Rapid7 for responsibly reporting this issue through coordinated vulnerability disclosure.”*


## Technical analysis


Rapid7 will be publishing full technical details for CVE-2026-55040 within 30 days of this disclosure.


## Remediation


Customers are advised to apply the latest available


[updates](https://learn.microsoft.com/en-us/officeupdates/sharepoint-updates) for the impacted product to ensure they are protected.


## Rapid7 customers


Exposure Command, InsightVM and Nexpose customers will be able to assess their exposure to CVE-2026-55040 with Authenticated vulnerability checks available in the July 15 content release


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


**June 30, 2026:**


Rapid7 requests supporting information for the upcoming disclosure.


-


**June 30, 2026:**


Microsoft provides supporting information to Rapid7.


-


**July 14, 2026:**


This disclosure for CVE-2026-55040.


- **July 14, 2026**


: The blog is updated to reflect that Microsoft’s published advisory now scores CVE-2026-55040 as critical severity, rather than medium severity.


- **July 15, 2026:**


Updated to reflect the availability of vulnerability checks in the July 15 content release.
