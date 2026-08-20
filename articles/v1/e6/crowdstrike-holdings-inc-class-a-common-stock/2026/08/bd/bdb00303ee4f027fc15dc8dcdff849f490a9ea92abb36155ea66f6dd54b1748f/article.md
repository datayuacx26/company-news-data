---
schema_version: "1.0.0"
document_id: "bdb00303ee4f027fc15dc8dcdff849f490a9ea92abb36155ea66f6dd54b1748f"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/patch-tuesday-analysis-august-2026/"
published_at: null
first_seen_at: "2026-08-12T06:21:09.129655+00:00"
fetched_at: "2026-08-12T06:21:11.696674+00:00"
content_hash: "sha256:4018f1b3401dd94733420de6a0e6240ec1f37dcb50b4e80c61c99fdec3b8d5a6"
---

# August 2026 Patch Tuesday: One Exploited Zero-Day and 62 Critical Vulnerabilities Among 415 CVEs

Microsoft has addressed 415 vulnerabilities in its August 2026 security update release. This month's patches include fixes for one exploited zero-day vulnerability, three disclosed zero-day vulnerabilities, and 62 **Critical** vulnerabilities, along with 349 additional vulnerabilities of varying severity levels.


## August 2026 Risk Analysis


This month's leading risk types by exploitation technique are[elevation of privilege](https://www.crowdstrike.com/cybersecurity-101/privilege-escalation/) with 174 patches (42%),[remote code execution](https://www.crowdstrike.com/cybersecurity-101/remote-code-execution-rce/) (RCE) with 109 patches (26%), and information disclosure with 85 (20%).


Figure 1. Breakdown of August 2026 Patch Tuesday exploitation techniques


Microsoft Windows received the most patches this month with 233, followed by Extended Security Updates (ESU) with 192 and Microsoft Office with 125.


Figure 2. Breakdown of product families affected by August 2026 Patch Tuesday


## Exploited Zero-Day Vulnerability in Windows Ancillary Function Driver for WinSock


[CVE-2026-68820](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68820) is an **Important** elevation of privilege vulnerability affecting the Windows Ancillary Function Driver for WinSock and has a **CVSS** score of **7.0** . A use-after-free flaw (CWE-416) allows a low-privileged local attacker to elevate privileges with no user interaction. Exploitation requires winning a race condition when a locally authenticated attacker runs a specially crafted application to trigger the flaw. Successful exploitation could allow the attacker to gain SYSTEM privileges.


Microsoft reports that this vulnerability has been exploited in the wild.


Table 1. Exploited zero-day vulnerability in Windows Ancillary Function Driver for WinSock **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 7.0 CVE-2026-68820 Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability Yes


## Publicly Disclosed Vulnerability in Windows User Profile Service


[CVE-2026-62832](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62832) is an **Important** elevation of privilege vulnerability affecting the Windows User Profile Service and has a **CVSS** score of **7.8** . A link following flaw (CWE-59) allows a low-privileged local attacker to elevate privileges with no user interaction and low attack complexity. An authenticated attacker with credentials for another local account could run a specially crafted application to load another user's registry hive, potentially gaining access to or modifying that user's data and elevating to administrator privileges.


While not confirmed at this time, CrowdStrike assesses this is likely the patch for the LegacyHive exploit released by the Nightmare-Eclipse persona in July 2026.


Table 2. Publicly disclosed vulnerability in Windows User Profile Service **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 7.8 CVE-2026-62832 Windows User Profile Service Elevation of Privilege Vulnerability Yes


## Publicly Disclosed Vulnerability in Windows Kernel


[CVE-2026-62737](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62737) is an **Important** elevation of privilege vulnerability affecting the Windows kernel and has a **CVSS** score of **7.8** . An untrusted pointer dereference flaw (CWE-822) allows a low-privileged local attacker to elevate privileges with no user interaction and low attack complexity. Successful exploitation could allow an attacker to gain SYSTEM privileges.


While not officially recognized by Microsoft as publicly disclosed, a Chinese-language blog was published on August 9, 2026, describing a proof-of-concept exploit that can cause a system crash.


Table 3. Publicly disclosed vulnerability in Windows kernel **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 7.8 CVE-2026-62737 Windows Kernel Elevation of Privilege Vulnerability Yes


## Publicly Disclosed Vulnerability in Windows Container Isolation FS Filter Driver


[CVE-2026-72971](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-72971) is an **Important** tampering vulnerability affecting the Windows Container Isolation FS Filter Driver (unionfs.sys) and has a **CVSS** score of **5.5** . Windows Container Isolation FS Filter Driver is a component of the Windows container infrastructure that provides filesystem isolation for containerized workloads. It acts as a file system filter driver that intercepts and manages file system operations, ensuring containerized processes are restricted to their designated scope and isolated from the host operating system's file system and other containers.


A link following flaw (CWE-59) allows a low-privileged local attacker to tamper with system integrity with no user interaction and low attack complexity. Successful exploitation impacts only integrity, with no effect on confidentiality or availability.


This vulnerability was publicly disclosed, though there is no evidence of exploitation in the wild. Microsoft assesses exploitation as unlikely.


Table 4. Publicly disclosed vulnerability in Windows Container Isolation FS Filter Driver **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 5.5 CVE-2026-72971 Windows Container Isolation FS Filter Driver (unionfs.sys) Tampering Vulnerability Yes


## Critical Vulnerability in Microsoft QUIC


[CVE-2026-62815](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62815) is a **Critical** RCE vulnerability affecting Microsoft QUIC and has a **CVSS** score of **9.8** . Microsoft QUIC (MsQuic) is Microsoft's open-source implementation of the QUIC transport protocol, which underpins HTTP/3 and is used across multiple Microsoft products and services for high-performance, encrypted network communication.


A use-after-free flaw (CWE-416) allows an unauthenticated remote attacker to execute code with no user interaction and low attack complexity. An attacker could exploit this vulnerability by sending a specially crafted packet to an affected service over the network.


Table 5. Critical vulnerability in Microsoft QUIC **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-62815 Microsoft QUIC Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Deployment Services


[CVE-2026-62893](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62893) is a **Critical** RCE vulnerability affecting Windows Deployment Services and has a **CVSS** score of **9.8** . A use-after-free flaw (CWE-416) allows an unauthenticated attacker to execute code over a network with low attack complexity. An attacker could exploit this by sending a specially crafted packet to the TFTP Server component of an affected service, with no authentication or user interaction required. Successful exploitation could allow the attacker to execute code on the target system.


Table 6. Critical vulnerability in Windows Deployment Services **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-62893 Windows Deployment Services TFTP Server Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows DNS Server


[CVE-2026-62878](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62878) ,[CVE-2026-62817](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62817) ,[CVE-2026-62820](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62820) , and[CVE-2026-65789](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65789) are **Critical** RCE vulnerabilities affecting Windows DNS Server and have **CVSS** scores of **9.8** , **8.8** , **8.1** , and **8.1** , respectively. All four can be exploited by sending specially crafted packets to an affected DNS server with no user interaction required.


CVE-2026-62878 stems from a stack-based buffer overflow flaw (CWE-121) and allows an unauthenticated remote attacker to execute code with low attack complexity. CVE-2026-62817 stems from an out-of-bounds write flaw (CWE-787) and is limited to adjacent network attackers, exploitable by calling arbitrary endpoints from within the network. CVE-2026-62820 stems from a race condition flaw (CWE-362) and requires an attacker to win a race condition. CVE-2026-65789 stems from a use-after-free flaw (CWE-416) and requires specific network configurations and timing conditions to exploit, meaning an attacker cannot reliably exploit the issue across all environments. It has only been observed in limited scenarios.


Table 7. Critical vulnerabilities in Windows DNS Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-62878 Windows DNS Server Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-62817 Windows DNS Server Remote Code Execution Vulnerability Yes


**Critical** 8.1 CVE-2026-62820 Windows DNS Server Remote Code Execution Vulnerability Yes


**Critical** 8.1 CVE-2026-65789 Windows DNS Server Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows iSCSI Target Service


[CVE-2026-65791](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65791) is a **Critical** RCE vulnerability affecting Windows iSCSI Target Service and has a **CVSS** score of **9.8** . A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated attacker to execute code over a network with low attack complexity. An attacker could exploit this by sending a specially crafted packet to an affected service, with no authentication or user interaction required, to execute code on the target system.


Table 8. Critical vulnerability in Windows iSCSI Target Service **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-65791 Windows iSCSI Target Service Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Reliable Multicast Transport Driver


[CVE-2026-62816](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62816) is a **Critical** RCE vulnerability affecting the Windows Reliable Multicast Transport Driver (RMCAST) and has a **CVSS** score of **8.8** . RMCAST is the Windows kernel driver that implements PGM (Pragmatic General Multicast), a reliable multicast protocol used for one-to-many data delivery across a network.


A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated attacker to execute code with no user interaction and low attack complexity, though exploitation is limited to adjacent network attackers. An attacker could exploit this vulnerability by sending a specially crafted packet to an affected service.


Table 9. Critical vulnerability in Windows Reliable Multicast Transport Driver (RMCAST) **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62816 Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Active Directory Certificate Services


[CVE-2026-62818](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62818) is a **Critical** RCE vulnerability affecting Windows Active Directory Certificate Services (AD CS) and has a **CVSS** score of **8.8** . A use-after-free flaw (CWE-416) allows a low-privileged remote attacker to execute code with no user interaction and low attack complexity. An attacker could exploit this vulnerability by sending a specially crafted request to an affected AD CS service over the network.


Table 10. Critical vulnerability in Windows Active Directory Certificate Services **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62818 Windows Active Directory Certificate Services (AD CS) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows DHCP Server


[CVE-2026-62823](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62823) is a **Critical** RCE vulnerability affecting Windows DHCP Server and has a **CVSS** score of **8.8** . A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated attacker to execute code over an adjacent network with low attack complexity. An attacker could exploit this by sending a specially crafted packet to an affected DHCP service, with no authentication or user interaction required, to execute code on the target system.


Table 11. Critical vulnerability in Windows DHCP Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62823 Windows DHCP Server Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows GDI+


[CVE-2026-62822](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62822) and[CVE-2026-62890](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62890) are **Critical** vulnerabilities affecting Windows GDI+ and have **CVSS** scores of **8.8** and **7.8** , respectively. CVE-2026-62822 stems from an integer overflow or wraparound flaw (CWE-190) and allows an unauthorized attacker to execute code over a network. It requires user interaction: An attacker would need to convince a user to open a specially crafted file to trigger remote code execution. CVE-2026-62890 stems from a heap-based buffer overflow flaw (CWE-122) and allows an authenticated attacker to elevate privileges locally with no user interaction required. Successful exploitation of CVE-2026-62890 could grant an attacker SYSTEM privileges.


Although both vulnerabilities affect the same component, they differ in threat type, attack vector, and privilege requirements, with one enabling network-based code execution through a malicious file and the other enabling local privilege escalation.


Table 12. Critical vulnerabilities in Windows GDI+ **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62822 Windows GDI+ Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-62890 Windows GDI+ Elevation of Privilege Vulnerability Yes


## Critical Vulnerabilities in Microsoft SharePoint Server


[CVE-2026-62827](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62827) ,[CVE-2026-64921](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64921) , and[CVE-2026-65665](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65665) are **Critical** vulnerabilities affecting Microsoft SharePoint Server, all carrying a **CVSS** score of **8.8** . CVE-2026-62827 stems from an improper authentication flaw (CWE-287), while CVE-2026-64921 stems from a missing authentication for critical function flaw (CWE-306). Both allow an authenticated attacker to elevate privileges over a network with low attack complexity. In each case, an authenticated attacker with access to the domain could perform RCE on the SharePoint server to elevate themselves to SharePoint admin.


CVE-2026-65665 stems from a deserialization of untrusted data flaw (CWE-502) and allows an authenticated attacker to execute code over a network with low attack complexity. In this case, an attacker authenticated as at least a site owner could write and inject arbitrary code to execute remotely on the SharePoint server. All three vulnerabilities require authenticated access to exploit, underscoring the importance of restricting and monitoring SharePoint permissions in addition to patching.


Table 13. Critical vulnerabilities in Microsoft SharePoint Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62827 Microsoft SharePoint Server Elevation of Privilege Vulnerability Yes


**Critical** 8.8 CVE-2026-64921 Microsoft SharePoint Server Elevation of Privilege Vulnerability Yes


**Critical** 8.8 CVE-2026-65665 Microsoft SharePoint Server Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Remote Desktop Client


[CVE-2026-62824](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62824) is a **Critical** RCE vulnerability affecting Remote Desktop Client and has a **CVSS** score of **8.8** . A stack-based buffer overflow flaw (CWE-121) allows an unauthenticated attacker to execute code over a network. It requires user interaction. An attacker could host a malicious server and convince a user to connect to it from an affected client; when the client processes the server's response, the attacker could execute code on the client system.


Table 14. Critical vulnerability in Remote Desktop Client **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62824 Remote Desktop Client Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Microsoft Office


[CVE-2026-63515](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63515) ,[CVE-2026-63532](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63532) ,[CVE-2026-64898](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64898) ,[CVE-2026-64903](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64903) ,[CVE-2026-64909](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64909) ,[CVE-2026-64910](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64910) ,[CVE-2026-64911](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64911) ,[CVE-2026-65657](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65657) , and[CVE-2026-70130](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-70130) are **Critical** RCE vulnerabilities in Microsoft Office, with **CVSS** scores of **8.4** (CVE-2026-70130) and **7.8** (all others). These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through the following flaws:


- Heap-based Buffer Overflow (CWE-122): CVE-2026-64898, CVE-2026-70130
- Out-of-bounds Read (CWE-125): CVE-2026-63515
- Integer Overflow or Wraparound (CWE-190): CVE-2026-63532, CVE-2026-64903, CVE-2026-64911
- Integer Underflow (CWE-191): CVE-2026-64909
- Use After Free (CWE-416): CVE-2026-65657
- Untrusted Pointer Dereference (CWE-822): CVE-2026-64910


Exploitation requires an attacker to convince a user to open a specially crafted malicious Office file. Preview Pane is an attack vector for CVE-2026-63515, CVE-2026-63532, CVE-2026-64898, CVE-2026-64903, CVE-2026-64909, and CVE-2026-65657. Preview Pane is not an attack vector for CVE-2026-64910 and CVE-2026-64911. CVE-2026-70130 does not specify if Preview Pane is an attack vector.


Table 15. Critical vulnerabilities in Microsoft Office **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.4 CVE-2026-70130 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-63515 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-63532 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-64898 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-64903 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-64909 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-64910 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-64911 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-65657 Microsoft Office Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows Device Health Attestation


[CVE-2026-66802](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-66802) and[CVE-2026-71331](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-71331) are **Critical** RCE vulnerabilities affecting the Microsoft Azure Attestation service and Device Health Attestation (DHA) service. Both carry a **CVSS** score of **8.1** .


CVE-2026-66802 stems from a race condition flaw (CWE-362) and requires the attacker to win a race condition to succeed. CVE-2026-71331 stems from an integer overflow or wraparound flaw (CWE-190) and instead requires the attacker to have deep knowledge of the target environment and configuration. Both vulnerabilities allow an unauthenticated attacker to execute code over a network with high attack complexity and no user interaction; in each case, an attacker could exploit the flaw by sending a specially crafted packet to an affected service to execute code on the target system.


Table 16. Critical vulnerabilities in Windows Device Health Attestation (DHA) **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.1 CVE-2026-66802 Windows Device Health Attestation (DHA) Remote Code Execution Vulnerability Yes


**Critical** 8.1 CVE-2026-71331 Windows Device Health Attestation (DHA) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Routing and Remote Access Service


[CVE-2026-62819](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62819) is a **Critical** RCE vulnerability affecting Windows Routing and Remote Access Service (RRAS) and has a **CVSS** score of **8.1** . RRAS is a Windows Server service that provides routing and remote access functionality, enabling organizations to deploy VPN, dial-up, and site-to-site connectivity solutions.


A use-after-free flaw (CWE-416) could allow an unauthenticated remote attacker to execute arbitrary code over a network. Successful exploitation requires an attacker to win a race condition. An attacker could exploit this vulnerability by sending a specially crafted packet to an affected service over the network, with no authentication or user interaction required.


Table 17. Critical vulnerability in Windows Routing and Remote Access Service (RRAS) **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.1 CVE-2026-62819 Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Secure Socket Tunneling Protocol


[CVE-2026-62889](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62889) is a **Critical** RCE vulnerability affecting Windows Secure Socket Tunneling Protocol (SSTP) and has a **CVSS** score of **8.1** . SSTP is a Microsoft VPN protocol that tunnels Point-to-Point Protocol (PPP) traffic through an SSL/TLS channel; it’s commonly used to provide secure remote access to corporate networks over HTTPS. A double free flaw (CWE-415) could allow an unauthenticated remote attacker to execute arbitrary code over a network. Successful exploitation requires an attacker to win a race condition. An attacker could exploit this vulnerability by sending a specially crafted packet to an affected service over the network, with no authentication or user interaction required.


Table 18. Critical vulnerability in Windows Secure Socket Tunneling Protocol (SSTP) **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.1 CVE-2026-62889 Windows Secure Socket Tunneling Protocol (SSTP) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Key Guard


[CVE-2026-66799](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-66799) is a **Critical** elevation of privilege vulnerability affecting Windows Key Guard and has a **CVSS** score of **7.8** . A heap-based buffer overflow flaw (CWE-122) allows an authenticated attacker to elevate privileges locally with low attack complexity. Successful exploitation could allow an attacker to gain Virtual Trust Level 1 (VTL1) privileges, escalating beyond the access normally permitted to a standard local user.


Table 19. Critical vulnerability in Windows Key Guard **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-66799 Windows Key Guard Elevation of Privilege Vulnerability Yes


## Critical Vulnerabilities in Microsoft Office Graphics Component


[CVE-2026-63513](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63513) ,[CVE-2026-63519](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63519) ,[CVE-2026-63526](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63526) ,[CVE-2026-65664](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65664) , and[CVE-2026-66807](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-66807) are **Critical** RCE vulnerabilities in the Microsoft Office Graphics Component, all with **CVSS** scores of **7.8** . These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through the following flaws:


- Stack-based Buffer Overflow (CWE-121): CVE-2026-63526, CVE-2026-66807
- Heap-based Buffer Overflow (CWE-122): CVE-2026-63513, CVE-2026-63519, CVE-2026-65664


Exploitation requires an attacker to convince a user to open a specially crafted malicious Office file. Preview Pane is an attack vector for CVE-2026-63513, CVE-2026-63519, CVE-2026-63526, and CVE-2026-66807, but is not an attack vector for CVE-2026-65664.


Table 20. Critical vulnerabilities in Microsoft Office Graphics Component **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-63513 Microsoft Office Graphics Component Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-63519 Microsoft Office Graphics Component Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-63526 Microsoft Office Graphics Component Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-65664 Microsoft Office Graphics Component Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-66807 Microsoft Office Graphics Component Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Microsoft Office Word


[CVE-2026-63518](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63518) ,[CVE-2026-63525](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63525) , and[CVE-2026-64907](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64907) are **Critical** RCE vulnerabilities in Microsoft Office Word, all with a **CVSS** score of **7.8** . These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through the following flaws:


- Stack-based Buffer Overflow (CWE-121): CVE-2026-64907
- Heap-based Buffer Overflow (CWE-122): CVE-2026-63518
- Numeric Truncation Error (CWE-197): CVE-2026-63525


Exploitation requires an attacker to convince a user to open a specially crafted malicious Office file. Preview Pane is not an attack vector for any of these vulnerabilities.


Table 21. Critical vulnerabilities in Microsoft Office Word **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-63518 Microsoft Office Word Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-63525 Microsoft Office Word Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-64907 Microsoft Office Word Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Microsoft Excel


[CVE-2026-68794](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68794) ,[CVE-2026-68804](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68804) , and[CVE-2026-68816](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68816) are **Critical** RCE vulnerabilities in Microsoft Excel, all with a **CVSS** score of **7.8** . These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through the following flaws:


- Stack-based Buffer Overflow (CWE-121): CVE-2026-68816
- Heap-based Buffer Overflow (CWE-122): CVE-2026-68794
- Numeric Truncation Error (CWE-197): CVE-2026-68804


Table 22. Critical vulnerabilities in Microsoft Excel **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-68794 Microsoft Excel Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-68804 Microsoft Excel Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-68816 Microsoft Excel Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Microsoft Exchange Server


[CVE-2026-62911](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62911) is a **Critical** elevation of privilege vulnerability affecting Microsoft Exchange Server and has a **CVSS** score of **8.0** . An authentication bypass by capture-replay flaw (CWE-294) could allow an authorized remote attacker to elevate their privileges over a network. An attacker who successfully exploited this vulnerability could take over the mailboxes of all Exchange users, send and read emails, and download attachments.


Table 23. Critical vulnerability in Microsoft Exchange Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.0 CVE-2026-62911 Microsoft Exchange Server Elevation of Privilege Vulnerability Yes


## Critical Vulnerabilities in Microsoft Teams


[CVE-2026-62896](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62896) ,[CVE-2026-62918](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62918) , and[CVE-2026-65667](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65667) are **Critical** vulnerabilities affecting Microsoft Teams, with **CVSS** scores of **10.0** (CVE-2026-65667), **9.6** (CVE-2026-62896), and **7.5** (CVE-2026-62918). CVE-2026-65667 and CVE-2026-62896 are elevation of privilege vulnerabilities, while CVE-2026-62918 is a spoofing vulnerability. These vulnerabilities exploit the following flaws:


- Improper Authentication (CWE-287): CVE-2026-62896
- Improper Verification of Cryptographic Signature (CWE-347): CVE-2026-62918
- Missing Authorization (CWE-862): CVE-2026-65667


Table 24. Critical vulnerabilities in Microsoft Teams **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 10.0 CVE-2026-65667 Microsoft Teams Elevation of Privilege Vulnerability No


**Critical** 9.6 CVE-2026-62896 Microsoft Teams Elevation of Privilege Vulnerability No


**Critical** 7.5 CVE-2026-62918 Microsoft Teams Spoofing Vulnerability No


## Critical Vulnerability in Microsoft Planetary Computer Pro


[CVE-2026-63508](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63508) is a **Critical** elevation of privilege vulnerability affecting Microsoft Planetary Computer Pro and has a **CVSS** score of **10.0** . Microsoft Planetary Computer Pro is a cloud-based geospatial analysis platform that provides access to large-scale environmental and earth observation datasets, primarily used by researchers and data scientists working with satellite imagery and climate data.


A missing authentication for a critical function flaw (CWE-306) allows an unauthenticated remote attacker to elevate privileges with no user interaction and low attack complexity. The vulnerability has a changed scope impact, affecting confidentiality and integrity but not availability.


Table 25. Critical vulnerability in Microsoft Planetary Computer Pro **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 10.0 CVE-2026-63508 Microsoft Planetary Computer Pro Elevation of Privilege Vulnerability No


## Critical Vulnerabilities in Azure SQL Database


[CVE-2026-56162](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56162) and[CVE-2026-63522](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63522) are **Critical** elevation of privilege vulnerabilities affecting Azure SQL Database and have **CVSS** scores of **10.0** and **7.8** , respectively. CVE-2026-56162 stems from an improper authentication flaw (CWE-287) and allows an unauthenticated remote attacker to elevate privileges over a network with no user interaction, low attack complexity, and a changed scope impact across confidentiality, integrity, and availability. CVE-2026-63522 stems from an incorrect permission assignment for a critical resource flaw (CWE-732) and allows a low-privileged local attacker to elevate privileges with no user interaction and low attack complexity. Successful exploitation of CVE-2026-63522 could allow an attacker to gain SQL sysadmin privileges.


Table 26. Critical vulnerabilities in Azure SQL Database **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 10.0 CVE-2026-56162 Azure SQL Database Elevation of Privilege Vulnerability No


**Critical** 7.8 CVE-2026-63522 Azure SQL Database Elevation of Privilege Vulnerability No


## Critical Vulnerability in Azure Service Bus


[CVE-2026-50515](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50515) is a **Critical** RCE vulnerability affecting Azure Service Bus and has a **CVSS** score of **9.9** . Azure Service Bus is a fully managed enterprise message broker service used to decouple applications and services from one another, commonly used to pass data between applications via message queues and publish-subscribe topics in cloud and hybrid architectures.


A deserialization of untrusted data flaw (CWE-502) allows a low-privileged remote attacker to execute code over a network with no user interaction, low attack complexity, and a changed scope impact across confidentiality, integrity, and availability.


Table 27. Critical vulnerability in Azure Service Bus **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-50515 Azure Service Bus Remote Code Execution Vulnerability No


## Critical Vulnerability in Microsoft Entra Provisioning Service


[CVE-2026-59115](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-59115) is a **Critical** elevation of privilege vulnerability affecting the Microsoft Entra Provisioning Service (SyncFabric) and has a **CVSS** score of **9.9** . The Microsoft Entra Provisioning Service is responsible for automating the synchronization of user identities and group memberships between Microsoft Entra ID (formerly Azure Active Directory) and connected applications and directories.


A path traversal flaw (CWE-35) allows a low-privileged remote attacker to elevate privileges over a network with no user interaction, low attack complexity, and a changed scope impact across confidentiality, integrity, and availability.


Table 28. Critical vulnerability in Microsoft Entra Provisioning Service **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-59115 Microsoft Entra Provisioning Service Elevation of Privilege Vulnerability No


## Critical Vulnerability in Azure Active Directory


[CVE-2026-50481](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50481) is a **Critical** elevation of privilege vulnerability affecting Azure Active Directory and has a **CVSS** score of **9.9** . A modification of assumed-immutable data flaw (CWE-471) allows a low-privileged remote attacker to elevate privileges over a network with no user interaction, low attack complexity, and a changed scope impact. Exploitation could allow an attacker to tamper with data that the system treats as trusted and unmodifiable, potentially enabling unauthorized privilege escalation within the identity platform.


Table 29. Critical vulnerability in Azure Active Directory **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-50481 Azure Active Directory Elevation of Privilege Vulnerability No


## Critical Vulnerability in Azure SRE Agent


[CVE-2026-62830](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62830) is a **Critical** elevation of privilege vulnerability affecting Azure SRE Agent and has a **CVSS** score of **9.9** . Azure SRE Agent is an AI-powered site reliability engineering service that autonomously monitors, diagnoses, and remediates issues in Azure-hosted applications and infrastructure.


A missing authorization flaw (CWE-862) allows a low-privileged remote attacker to elevate privileges over a network with no user interaction, low attack complexity, and a changed scope impact across confidentiality, integrity, and availability.


Table 30. Critical vulnerability in Azure SRE Agent **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-62830 Azure SRE Agent Elevation of Privilege Vulnerability No


## Critical Vulnerability in Microsoft 365 Admin Center


[CVE-2026-62873](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62873) is a **Critical** elevation of privilege vulnerability affecting the Microsoft 365 Admin Center and has a **CVSS** score of **9.8** . The Microsoft 365 Admin Center is the centralized web-based management portal used by organizational administrators to manage users, licenses, services, and security settings across the Microsoft 365 suite, making it a high-value target given the breadth of administrative access it provides.


An improper verification of cryptographic signature flaw (CWE-347) allows an unauthenticated remote attacker to elevate privileges over a network with no user interaction and low attack complexity, with full impact to confidentiality, integrity, and availability.


Table 31. Critical vulnerability in Microsoft 365 Admin Center **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-62873 Microsoft 365 Admin Center Elevation of Privilege Vulnerability No


## Critical Vulnerability in Microsoft SharePoint Online


[CVE-2026-70332](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-70332) is a **Critical** spoofing vulnerability affecting Microsoft SharePoint Online and has a **CVSS** score of **9.6** . A cross-site scripting flaw (CWE-79) allows an unauthenticated attacker to perform spoofing over a network.


Table 32. Critical vulnerability in Microsoft SharePoint Online **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.6 CVE-2026-70332 Microsoft SharePoint Spoofing Vulnerability No


## Critical Vulnerability in Azure Logic Apps


[CVE-2026-56161](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56161) is a **Critical** information disclosure vulnerability affecting Azure Logic Apps and has a **CVSS** score of **9.6** . An improper access control flaw (CWE-284) allows an authenticated attacker to disclose information over a network.


Azure Logic Apps is a cloud-based integration platform that lets organizations automate workflows and connect systems like SaaS applications, on-premises services, and databases with minimal custom code. Since these workflows often carry sensitive business data and credentials, an information disclosure flaw here could expose data flowing through an organization's connected systems.


Table 33. Critical vulnerability in Azure Logic Apps **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.6 CVE-2026-56161 Azure Logic Apps Information Disclosure Vulnerability No


## Critical Vulnerability in Microsoft Azure Kubernetes Service


[CVE-2026-50516](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50516) is a **Critical** elevation of privilege vulnerability affecting Microsoft Azure Kubernetes Service and has a **CVSS** score of **9.4** . A missing authentication for critical function flaw (CWE-306) allows an unauthenticated attacker to elevate privileges over a network.


Table 34. Critical vulnerability in Microsoft Azure Kubernetes Service **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.4 CVE-2026-50516 Microsoft Azure Kubernetes Service Elevation of Privilege Vulnerability No


## Critical Vulnerability in Copilot Cowork


[CVE-2026-59118](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-59118) is a **Critical** elevation of privilege vulnerability affecting Microsoft Copilot Cowork and has a **CVSS** score of **9.3** . An improper authorization flaw (CWE-285) allows an unauthenticated attacker to elevate privileges over a network with low attack complexity, requiring user interaction. The vulnerability has a changed scope impact, affecting confidentiality and integrity but not availability.


Microsoft Copilot Cowork is an AI-powered collaboration agent within the Microsoft 365 Copilot ecosystem, designed to work alongside users on shared tasks and content across Microsoft 365 apps. Because Cowork operates with access to organizational content and collaborates directly within user workflows, an authorization flaw could allow an attacker to escalate privileges and improperly access or manipulate data across connected Microsoft 365 services.


Table 35. Critical vulnerability in Copilot Cowork **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.3 CVE-2026-59118 Copilot Cowork Elevation of Privilege Vulnerability No


## Critical Vulnerability in Azure Confidential Ledger


[CVE-2026-68823](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68823) is a **Critical** RCE vulnerability affecting Azure Confidential Ledger and has a **CVSS** score of **9.1** . An exposed dangerous method or function flaw (CWE-749) allows an authenticated attacker to execute code over a network.


Azure Confidential Ledger is a tamper-proof, blockchain-based data store designed to maintain sensitive records with cryptographic integrity guarantees. Because organizations rely on it to preserve trust in critical audit and compliance data, a code execution flaw could undermine the integrity assurances the service is meant to provide.


Table 36. Critical vulnerability in Azure Confidential Ledger **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.1 CVE-2026-68823 Azure Confidential Ledger Remote Code Execution Vulnerability No


## Critical Vulnerability in Microsoft Purview eDiscovery


[CVE-2026-65668](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65668) is a **Critical** elevation of privilege vulnerability affecting Microsoft Purview eDiscovery and has a **CVSS** score of **8.8** . An improper access control flaw (CWE-284) allows an authenticated attacker to elevate privileges over a network. This vulnerability has already been fully mitigated by Microsoft; no customer action is required.


Microsoft Purview eDiscovery is a compliance tool used to search, hold, and export content such as emails and documents across Microsoft 365 for legal investigations and regulatory requirements. Because eDiscovery grants access to potentially privileged and sensitive organizational data during legal proceedings, an access control flaw could let an attacker escalate privileges and gain unauthorized visibility into confidential case-related content.


Table 37. Critical vulnerability in Microsoft Purview eDiscovery **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-65668 Microsoft Purview eDiscovery Elevation of Privilege Vulnerability No


## Critical Vulnerability in Application Insights Profiler


[CVE-2026-49163](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49163) is a **Critical** elevation of privilege vulnerability affecting Application Insights Profiler and has a **CVSS** score of **8.8** . Application Insights Profiler is a performance monitoring tool within Microsoft's Azure Application Insights platform that automatically collects detailed profiling data from live applications, helping developers identify and diagnose performance bottlenecks and slow code paths in production environments.


A path traversal flaw (CWE-22) allows an authenticated attacker to elevate privileges over a network. This vulnerability has already been fully mitigated by Microsoft; no customer action is required.


Table 38. Critical vulnerability in Application Insights Profiler **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-49163 Application Insights Profiler Elevation of Privilege Vulnerability No


## Critical Vulnerability in Microsoft Entra ID


[CVE-2026-62869](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62869) is a **Critical** spoofing vulnerability affecting Microsoft Entra ID and has a **CVSS** score of **8.8** . An insufficient verification of data authenticity flaw (CWE-345) allows an authenticated attacker to perform spoofing over a network.


Microsoft Entra ID (formerly Azure Active Directory) is Microsoft's cloud-based identity and access management service, providing authentication, authorization, and single sign-on capabilities for users accessing Microsoft and third-party applications and resources.


Table 39. Critical vulnerability in Microsoft Entra ID **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-62869 Microsoft Entra ID Spoofing Vulnerability No


## Critical Vulnerability in Azure SQL Managed Instance


[CVE-2026-62836](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62836) is a **Critical** elevation of privilege vulnerability affecting Azure SQL Managed Instance and has a **CVSS** score of **8.7** . Azure SQL Managed Instance is a fully managed cloud database service that provides broad SQL Server engine compatibility while running on Azure infrastructure. An improper restriction of communication channel to intended endpoints flaw (CWE-923) could allow an unauthenticated remote attacker to elevate their privileges over a network.


Table 40. Critical vulnerability in Azure SQL Managed Instance **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.7 CVE-2026-62836 Azure SQL Managed Instance Elevation of Privilege Vulnerability No


## Patch Tuesday Dashboard in the Falcon Platform


For a visual overview of the systems impacted by this month’s vulnerabilities, you can use our Patch Tuesday dashboard. This can be found in the CrowdStrike Falcon® platform within the Exposure Management > Vulnerability Management > Dashboards page. The preset dashboards show the most recent three months of Patch Tuesday vulnerabilities.


## Not All Relevant Vulnerabilities Have Patches: Consider Mitigation Strategies


As we have learned with other notable vulnerabilities, such as[Log4j](https://www.crowdstrike.com/log4j2/) , not every highly exploitable vulnerability can be easily patched. As is the case for the[ProxyNotShell](https://www.crowdstrike.com/blog/owassrf-exploit-analysis-and-recommendations/) vulnerabilities, it’s critically important to develop a response plan for how to defend your environments when no patching protocol exists.


Regular review of your patching strategy should still be a part of your program, but you should also look more holistically at your organization's methods for cybersecurity and improve your overall security posture.


**The CrowdStrike Falcon platform regularly collects and analyzes trillions of endpoint events every day from millions of sensors deployed across 176 countries.[Watch this demo to see the Falcon platform in action](https://go.crowdstrike.com/product-demo-platform.html) .**


## Learn More


Learn more about how CrowdStrike Falcon® Exposure Management can help you quickly and easily discover and prioritize vulnerabilities and other types of exposures[here](https://www.crowdstrike.com/products/exposure-management/falcon-exposure-management/) .


### About CVSS Scores


The[Common Vulnerability Scoring System](https://www.first.org/cvss/) (CVSS) is a free and open industry standard that CrowdStrike and many other cybersecurity organizations use to assess and communicate software vulnerabilities’ severity and characteristics. The CVSS Base Score ranges from 0.0 to 10.0, and the National Vulnerability Database (NVD) adds a severity rating for CVSS scores. Learn more about vulnerability scoring[in this article](https://www.crowdstrike.com/epp-101/vulnerability-management-programs/) .


#### Additional Resources


- *For more information on which products are in Microsoft’s Extended Security Updates program, refer to the vendor guidance[here](https://learn.microsoft.com/en-us/lifecycle/faq/extended-security-updates) .*
- *Learn how[Falcon Exposure Management](https://www.crowdstrike.com/products/exposure-management/falcon-exposure-management/) can help you discover and manage vulnerabilities and other exposures in your environments.*
- *Make prioritization painless and efficient. Watch how Falcon Exposure Management enables IT staff to improve visibility with[custom filters and team dashboards.](https://www.crowdstrike.com/tech-hub/?category=exposure-management)*
- *Test CrowdStrike next-gen antivirus for yourself with a[free trial of CrowdStrike® Falcon Prevent™](https://go.crowdstrike.com/try-falcon-prevent.html) .*
- *Experience Fal.Con 2026 from anywhere with[Fal.Con Digital](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) , featuring keynote livestreams and on-demand access to 100+ sessions.*
