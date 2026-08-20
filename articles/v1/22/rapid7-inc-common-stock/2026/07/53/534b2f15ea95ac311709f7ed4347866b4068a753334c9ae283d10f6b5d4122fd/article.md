---
schema_version: "1.0.0"
document_id: "534b2f15ea95ac311709f7ed4347866b4068a753334c9ae283d10f6b5d4122fd"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-58644-microsoft-sharepoint-server-unauthenticated-remote-code-execution-vulnerability-exploited-in-the-wild"
published_at: "2026-07-17T18:18:53+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:76c897bdbf3cc1fc2fa00449bbe77978f49d03a5dc27039c41b1de68ed935a33"
---

# CVE-2026-58644: Microsoft SharePoint Server Unauthenticated Remote Code Execution Vulnerability Exploited in the Wild

## Overview


On July 14, 2026, Microsoft


[published](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58644) a security advisory addressing


[CVE-2026-58644](https://nvd.nist.gov/vuln/detail/CVE-2026-58644) , a critical remote code execution (RCE) vulnerability affecting on-premises Microsoft SharePoint Server deployments. The vulnerability, which carries a CVSS v3.1 score of 9.8 (Critical), results from the deserialization of untrusted data (


[CWE-502](https://cwe.mitre.org/data/definitions/502.html) ) and allows an unauthenticated attacker to execute arbitrary code.


Microsoft confirmed active exploitation of CVE-2026-58644, and the vulnerability was subsequently added to CISA’s Known Exploited Vulnerabilities (


[KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) ) catalog on July 16, 2026. In parallel, CISA


[published](https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations) guidance recommending organizations immediately apply Microsoft’s security updates and leverage Microsoft Defender and AMSI detections to identify exploitation attempts.


Affected products:


-


Microsoft SharePoint Enterprise Server 2016


-


Microsoft SharePoint Server 2019


-


Microsoft SharePoint Server Subscription Edition


*Update:*


On July 22, 2026, a separate SharePoint vulnerability, CVE-2026-50522, was


[added](https://www.cisa.gov/news-events/alerts/2026/07/22/cisa-adds-two-known-exploited-vulnerabilities-catalog) to CISA’s KEV catalog. CVE-2026-50522 is a deserialization of untrusted data vulnerability also affecting Microsoft SharePoint, and allows a remote attacker to achieve unauthenticated RCE on a vulnerable system. This separate RCE vulnerability was


[disclosed](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50522) and patched by Microsoft as part of the same July 14


[Patch Tuesday](https://www.rapid7.com/blog/post/em-patch-tuesday-july-2026/) release as CVE-2026-58644. Customers who have applied all of the SharePoint security updates from the July 14 updates will be protected against both exploited in-the-wild vulnerabilities.


## Mitigation guidance


Organizations operating affected on-premises Microsoft SharePoint Server should prioritize remediation on an emergency basis.


Microsoft’s recommendations:


-


Apply the July 14, 2026 security updates for all affected SharePoint versions.


-


Verify that security updates completed successfully across all SharePoint servers.


-


Ensure Antimalware Scan Interface (AMSI) integration is enabled for every SharePoint web application.


-


Monitor Microsoft Defender and AMSI detections for indicators of attempted exploitation.


-


Initiate incident response procedures if exploitation artifacts are detected.


Microsoft and CISA


[recommend](https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations) monitoring for the following security detections associated with observed SharePoint exploitation activity.


AMSI / Microsoft Defender detections:


-


Exploit:Script/SuspSignoutReqBody.A


-


Request body scanning


-


SharePoint Server Subscription Edition


-


Microsoft reports observed exploitation attempts are blocked by this signature.


-


Exploit:Script/ToolPaneAuthBypass.A


-


Request header scanning


-


Applies to SharePoint Server 2016, SharePoint Server 2019, and Subscription Edition.


-


Exploit:Script/ToolPaneAuthBypass


At the time of publication, no public IP addresses, domains, URLs, or additional network-based indicators of compromise have been widely disclosed.


Administrators should consult Microsoft’s


[advisory](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58644) for the most current remediation guidance and update availability.


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-58644 with an authenticated vulnerability check available since the July 14 content release.


## Updates


- **July 17, 2026**


: Initial publication.


- **July 23, 2026:**


Added a description of CVE-2026-50522 to the overview.
