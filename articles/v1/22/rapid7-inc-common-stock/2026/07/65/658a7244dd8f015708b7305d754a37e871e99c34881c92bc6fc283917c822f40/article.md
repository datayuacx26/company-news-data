---
schema_version: "1.0.0"
document_id: "658a7244dd8f015708b7305d754a37e871e99c34881c92bc6fc283917c822f40"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-critical-vmware-vcenter-vulnerabilities-allow-authentication-bypass-and-remote-code-execution-cve-2026-59309-cve-2026-59310"
published_at: "2026-07-30T10:35:21+00:00"
first_seen_at: "2026-07-30T11:04:34.228056+00:00"
fetched_at: "2026-07-30T11:04:36.316581+00:00"
content_hash: "sha256:9a310d3c15ea562f5e74ca6385f49bd4e7657e35b1f9f16a2d9b7651cf51b312"
---

# Critical VMware vCenter Vulnerabilities Allow Authentication Bypass and Remote Code Execution (CVE-2026-59309, CVE-2026-59310)

## Overview


On July 29, 2026, Broadcom published security advisory


[VMSA-2026-0006](https://support.broadcom.com/web/ecx/support-content-notification/-/external/content/SecurityAdvisories/0/38017) addressing multiple vulnerabilities in several VMWare products. Included in the advisory are two critical remotely exploitable vulnerabilities affecting VMware vCenter Server: CVE-2026-59309 and CVE-2026-59310. Both vulnerabilities carry CVSSv3.1 base scores of


[9.8](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H) and can be exploited by unauthenticated attackers with network access to a vulnerable vCenter Server.


**CVE**


**CVSSv3.1**


**Description Summary**


CVE-2026-59309


[9.8 (Critical)](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)


An authentication bypass vulnerability in the VMware Directory Service of vCenter that could allow a remote attacker to bypass authentication and gain unauthorized access to the vCenter management plane.


CVE-2026-59310


A directory traversal vulnerability in the vCenter Syslog server that could allow an attacker with network access to execute arbitrary code.


VMware vCenter Server provides centralized management for VMware vSphere environments, allowing administrators to manage ESXi hosts, virtual machines, resource allocation, availability, and other virtualization infrastructure from a central control plane. Compromise of vCenter can therefore provide an attacker with significant control over the virtualized environment and its associated workloads.


Both vulnerabilities are particularly significant because exploitation does not require prior authentication. However, an attacker must have network access to the affected vCenter services. Management interfaces such as vCenter are commonly restricted to internal or dedicated management networks, which can reduce exposure to internet-based attacks but does not mitigate the risk from an attacker who has already established access to an organization’s network.


At the time of publication, there is no known evidence of exploitation or scanning in the wild for either CVE-2026-59309 or CVE-2026-59310. There is also currently no known public proof-of-concept exploit code. However, vCenter Server has appeared on CISA’s KEV list ten times in the past for other vulnerabilities, so it is known that attackers target critical issues in this product. Customers running affected VMWare products are urged to patch on an urgent basis before exploitation in-the-wild occurs.


## Mitigation guidance


Organizations running VMware vCenter Server should prioritize applying the updates identified by Broadcom in


[VMSA-2026-0006](https://support.broadcom.com/web/ecx/support-content-notification/-/external/content/SecurityAdvisories/0/38017) on an urgent basis. Broadcom states that there are no workarounds for CVE-2026-59309 or CVE-2026-59310, making vendor-provided updates the primary remediation.


**VMware Product**


**Component**


**Version**


**Running On**


**Fixed Version**


VMware Cloud Foundation,


VMware vSphere Foundation


vCenter


9.1.x.x


Any


[9.1.0.0300](https://techdocs.broadcom.com/bin/gethidpage?ux-context-string=vcenter-9-1-0-3&appid=vcf-9-1&language=en&format=rendered)


VMware Cloud Foundation,


VMware vSphere Foundation


vCenter


9.0.x.x


Any


[9.0.2.0100](https://techdocs.broadcom.com/bin/gethidpage?ux-context-string=9-0-2-0-1&appid=vcf-9-0&language=en&format=rendered)


VMware vCenter


N/A


8.0


Any


[8.0 U3k](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/8-0/release-notes/vcenter-server-update-and-patch-release-notes/vsphere-vcenter-server-80u3k-release-notes.html)


VMware Cloud Foundation


vCenter


5.x


Any


Async patch to


VMware Telco Cloud Platform


vCenter


3.0, 4.x, 5.0.x, 5.1.x


Any


Refer to


[KB449886](https://knowledge.broadcom.com/external/article/449886)


VMware Telco Cloud Infrastructure


vCenter


3.0


Any


Refer to


[KB449886](https://knowledge.broadcom.com/external/article/449886)


For the latest mitigation guidance, please refer to the vendor


[advisory](https://support.broadcom.com/web/ecx/support-content-notification/-/external/content/SecurityAdvisories/0/38017) .


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-59309 and CVE-2026-59310 on


VMware vCenter Server, Cloud Foundation, and vSphere Foundation products


with unauthenticated vulnerability checks expected to be available in the July 30 content release.


## Updates


-


**July 30, 2026:**


Initial publication.
