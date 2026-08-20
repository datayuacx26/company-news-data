---
schema_version: "1.0.0"
document_id: "d886ebf48dc4a074f441823a841c0225294bde34d601e2c901fa8d6fe11ab0e3"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-18577-n-able-n-central-authentication-bypass-exploited-in-the-wild"
published_at: "2026-08-04T11:11:54+00:00"
first_seen_at: "2026-08-04T12:43:36.240182+00:00"
fetched_at: "2026-08-04T12:49:29.257937+00:00"
content_hash: "sha256:2c37bb5644af50785ef560ba9d071be285bc354b73626c84d62c7934f09e16ee"
---

# CVE-2026-18577: N-able N-central Authentication Bypass Exploited in the Wild

## Overview


On August 2, 2026, N-able


[published](https://status.n-able.com/2026/08/02/n-central-2026-3-hotfix-1-mitigation-for-cve-2026-18577/) a security advisory for


[CVE-2026-18577](https://www.rapid7.com/db/vulnerabilities/cve-2026-18577/) , an authentication bypass vulnerability affecting N-central that was discovered being exploited in-the-wild after an incomplete fix for an earlier authentication bypass issue,


[CVE-2026-18556](https://www.rapid7.com/db/vulnerabilities/cve-2026-18556/) was disclosed. CVE-2026-18577 allows a remote unauthenticated attacker to bypass authentication and obtain administrative control of vulnerable N-central servers in affected deployments.


N-able


[N-central](https://www.n-able.com/products/n-central-rmm/network-and-device-management) is a widely deployed Remote Monitoring and Management (RMM) platform used by managed service providers (MSPs) and enterprise IT teams to centrally administer servers, workstations, network devices, and other managed assets. Because the platform operates with extensive administrative privileges across customer environments, successful compromise of an N-central server can provide attackers with an efficient path to compromise downstream managed systems.


According to N-able, exploitation of CVE-2026-18577 has been


[observed](https://www.n-able.com/blog/n-central-security-update-august-2-2026) in the wild since


[August 1, 2026](https://uptime.n-able.com/event/201454/) . Following successful exploitation, attackers leveraged the platform's Take Control functionality to remotely access managed endpoints, and deployed Cloudflare Tunnel (cloudflared) to establish persistent remote access. On August 3, 2026, CVE-2026-18577 was


[added](https://www.cisa.gov/news-events/alerts/2026/08/03/cisa-adds-one-known-exploited-vulnerability-catalog) to CISA’s Known Exploited Vulnerability (KEV) catalog.


## Mitigation guidance


Organizations operating vulnerable N-central deployments should prioritize remediation on an urgent basis, outside of normal patching schedules. Hosted N-central environments are upgraded automatically by the vendor, while on-premise deployments require manual remediation.


Affected versions:


-


All versions of N-able N-central up to and including version 2026.3.1, prior to Hotfix 1.


Fixed version:


-


N-able N-central 2026.3.1 Hotfix 1 (2026.3.1.7).


The vendor also recommends:


-


Upgrading N-central agents after applying the server hotfix.


-


Reviewing systems for indicators of compromise.


-


Contacting N-able Support immediately if evidence of compromise is discovered.


-


Engaging internal incident response teams if malicious activity is identified.


For further information, see the vendor


[advisory](https://status.n-able.com/2026/08/02/n-central-2026-3-hotfix-1-mitigation-for-cve-2026-18577/) .


## IOCs


N-able has


[published](https://status.n-able.com/2026/08/02/n-central-2026-3-hotfix-1-mitigation-for-cve-2026-18577/) several artifacts that administrators should investigate during incident response.


Endpoint Artifacts:


-


Presence of a Cloudflared service.


-


A suspicious svchost.exe located within the user's Documents folder.


Network Indicators:


-


Administrators should review historical network logs for inbound or outbound communication involving the malicious IP addresses identified by the vendor:


Organizations should also review:


-


Authentication logs


-


Administrative account creation or modification


-


Take Control session activity


-


Remote management logs


-


Windows service installation events


To assist affected organizations running N-central, the vendor has provided a


[detection template](https://developer.n-able.com/n-central/recipes/cve-2026-18577-detection) for CVE-2026-18577, which organizations can use to help identify potential compromise.


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-18577 with a vulnerability check expected to be available in the August 4 content release. Note that potential check type must be enabled in the scan template before scanning.


## Updates


-


**August 4, 2026:**


Initial publication.
