---
schema_version: "1.0.0"
document_id: "6e02445d29e9eb8610c37afce757940a8fc98a541f9a757bf69cc925813b8a4b"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/patch-tuesday-analysis-july-2026/"
published_at: null
first_seen_at: "2026-07-20T23:21:19.193454+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:3df80e9c8976dde6442c6d811dce7ecdc2e81731da1ebba133ed005d323eaac8"
---

# July 2026 Patch Tuesday: Microsoft Patches 622 Vulnerabilities Including Two Exploited Zero-Days

Microsoft has addressed 622 vulnerabilities in its July 2026 security update release. Based on CrowdStrike’s analysis, this represents roughly triple the number of vulnerabilities in June, and almost five times the number of vulnerabilities in May. This month's patches include fixes for two exploited zero-day vulnerabilities, one publicly disclosed zero-day vulnerability, and 62 **Critical** vulnerabilities, along with 557 additional vulnerabilities of varying severity levels. Separate from Microsoft’s disclosures, one unpatched disclosed zero-day vulnerability was also published today. CrowdStrike’s Counter Adversary Operations Advanced Research Team was credited for discovering four of the CVEs patched in this month’s release.


## July 2026 Risk Analysis


This month's leading risk types by exploitation technique are[elevation of privilege](https://www.crowdstrike.com/cybersecurity-101/privilege-escalation/) with 255 vulnerabilities (41%),[remote code execution](https://www.crowdstrike.com/cybersecurity-101/remote-code-execution-rce/) (RCE) with 166 (27%), and information disclosure with 109 (18%).


Figure 1. Breakdown of July 2026 Patch Tuesday exploitation techniques


Microsoft Windows received the most patches this month with 413, followed by Extended Security Updates (ESU) with 335 and Microsoft Office with 95.


Figure 2. Breakdown of product families affected by July 2026 Patch Tuesday


## Exploited Zero-Day Vulnerability in Active Directory Federation Services


[CVE-2026-56155](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56155) is an **Important** elevation of privilege vulnerability affecting Active Directory Federation Services (AD FS) and has a **CVSS** score of **7.8** . An insufficient granularity of access control flaw (CWE-1220) allows a low-privileged local attacker to elevate privileges with no user interaction and low attack complexity. Successful exploitation could grant an attacker administrator privileges.


This vulnerability has been exploited in the wild, though no exploit code has been publicly disclosed at the time of this writing.


Table 1. Exploited zero-day vulnerability in Active Directory Federation Services **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 7.8 CVE-2026-56155 Active Directory Federation Services Elevation of Privilege Vulnerability Yes


## Exploited Zero-Day Vulnerability in SharePoint


[CVE-2026-56164](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56164) is a **Moderate** elevation of privilege vulnerability affecting Microsoft SharePoint and has a **CVSS** score of **5.3** . A missing authentication for a critical function flaw (CWE-306) allows an unauthenticated remote attacker to elevate privileges over a network with no user interaction and low attack complexity.


This vulnerability has been exploited in the wild, though no exploit code has been publicly disclosed at the time of this writing. As a pre-patch mitigation, Microsoft recommends ensuring AMSI (Anti-Malware Scan Interface) is actively integrated and scanning SharePoint and IIS worker process memory, with the Request Body Scan mode set to Full so that POST body payloads are detected.


Table 2. Exploited zero-day vulnerability in SharePoint **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Moderate** 5.3 CVE-2026-56164 Microsoft SharePoint Server Elevation of Privilege Vulnerability Yes


## Disclosed Zero-Day Vulnerability in Windows BitLocker


[CVE-2026-50661](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50661) is an **Important** security feature bypass vulnerability affecting Windows BitLocker and has a **CVSS** score of **6.1** . A protection mechanism failure flaw (CWE-693) allows an unauthenticated attacker with physical access to bypass BitLocker Device Encryption and gain access to encrypted data on the system storage device. The attack requires no privileges or user interaction and has low attack complexity.


This vulnerability was publicly disclosed, though there is no evidence of exploitation in the wild. Microsoft assesses exploitation as less likely.


While not confirmed at this time, this CVE may be the patch for GreatXML, a BitLocker bypass exploit released by the Nightmare-Eclipse persona.


Table 3. Disclosed zero-day vulnerability in Windows BitLocker **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 6.1 CVE-2026-50661 Windows BitLocker Security Feature Bypass Vulnerability Yes


## Disclosed Zero-Day Vulnerability in Windows User Profile Service


Roughly half an hour after Microsoft’s Patch Tuesday disclosures, details emerged of an unpatched privilege escalation vulnerability affecting the Windows User Profile Service (profsvc) on all currently supported Windows desktop and server versions. A proof of concept (PoC) exploit dubbed LegacyHive, released by researcher MSNightmare, allegedly demonstrates the ability for a standard user to abuse the service's hive loading mechanism to mount arbitrary registry hives with elevated privileges by coercing the User Profile Service, which operates at SYSTEM integrity, into loading an attacker-controlled hive file. The publicly released PoC is reported to be deliberately stripped down and requires credentials for a secondary user account; however, the author claims the original version imposes no such restrictions and is not limited to UsrClass.dat, potentially allowing any registry hive to be loaded. CrowdStrike has not verified this claim at this time. Successful exploitation could enable registry-based persistence, credential theft, or security product tampering.


At the time of this writing, no CVE has been assigned, and no patch is available. MSNightmare is associated with the Nightmare-Eclipse persona, some of whose previously released vulnerabilities have been confirmed to be exploited in the wild. CrowdStrike will continue to monitor for updates.


## Critical Vulnerability in Microsoft Windows VMSwitch


[CVE-2026-57092](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57092) is a **Critical** elevation of privilege vulnerability affecting Microsoft Windows VMSwitch and has a **CVSS** score of **9.9** . A use-after-free flaw (CWE-416) allows an authenticated attacker to elevate privileges over a network with low attack complexity. Exploitation involves executing code within a guest virtual machine and sending specially crafted network-related requests to the host via the Hyper-V Virtual Switch, potentially forcing the host to access memory that has been freed. Successful exploitation could allow an attacker to gain unauthorized elevated privileges on the host system, bypassing the guest VM boundary.


Table 4. Critical vulnerability in Windows VMSwitch **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-57092 Windows VMSwitch Elevation of Privilege Vulnerability Yes


## Critical Vulnerability in Windows Server Network Driver


[CVE-2026-56188](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56188) is a **Critical** RCE vulnerability affecting the Windows Server Network driver and has a **CVSS** score of **9.8** . This race condition flaw (CWE-362) allows an unauthenticated remote attacker to execute code with no user interaction and low attack complexity. An attacker could exploit this by sending specially crafted malicious network traffic to a vulnerable server.


Table 5. Critical vulnerability in Windows Server Network driver **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-56188 Windows Server Network Driver Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows DHCP


[CVE-2026-50518](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50518) and[CVE-2026-56159](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56159) are **Critical** RCE vulnerabilities affecting Windows DHCP Server and have a **CVSS** score of **9.8** . Both stem from heap-based buffer overflow flaws (CWE-122) and allow unauthenticated remote attackers to execute code with no user interaction and low attack complexity. CVE-2026-50518 can be triggered by sending specially crafted network requests containing malicious domain name data, causing memory corruption due to insufficient input size validation. CVE-2026-56159 can be triggered by sending a specially crafted packet to a DHCP server configured to provide data for Option 43, a vendor-specific information field commonly used to deliver configuration data to network devices such as VoIP phones and wireless access points.


[CVE-2026-48564](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-48564) and[CVE-2026-50370](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50370) are **Critical** RCE vulnerabilities affecting Windows DHCP Server and have a **CVSS** score of **8.8** . Both stem from heap-based buffer overflow flaws (CWE-122). CVE-2026-48564 requires low privileges and can be triggered via a specially crafted RPC call to the DHCP service. CVE-2026-50370 requires no privileges but is limited to adjacent network attackers.


[CVE-2026-54128](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54128) is a **Critical** RCE vulnerability affecting the Windows DHCP Client and has a **CVSS** score of **8.4** . A use-after-free flaw (CWE-416) allows an unauthenticated attacker to execute code locally with no user interaction and low attack complexity.


Table 6. Critical vulnerabilities in Windows DHCP **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-50518 Windows DHCP Server Remote Code Execution Vulnerability Yes


**Critical** 9.8 CVE-2026-56159 DHCP Server Service Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-48564 DHCP Server Service Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-50370 DHCP Server Service Remote Code Execution Vulnerability Yes


**Critical** 8.4 CVE-2026-54128 Windows DHCP Server Service Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in SharePoint Server


[CVE-2026-50522](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50522) and[CVE-2026-58644](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58644) are **Critical** RCE vulnerabilities affecting Microsoft SharePoint Server, both with a **CVSS** score of **9.8** . Both vulnerabilities stem from deserialization of untrusted data flaws (CWE-502) and are remotely exploitable with low attack complexity. In a network-based attack, an attacker authenticated as at least a Site Owner could inject and execute arbitrary code remotely on the SharePoint Server.


[CVE-2026-55040](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55040) is a **Critical** security feature bypass vulnerability affecting Microsoft SharePoint Server and has a **CVSS** score of **9.1** . A weak authentication flaw (CWE-1390) could allow an unauthenticated remote attacker to bypass authentication and make an anonymous connection, enabling impersonation. Successful exploitation could allow an attacker to disclose files and modify data, though availability of the system would not be impacted.


Table 7. Critical vulnerabilities in SharePoint Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-50522 Microsoft SharePoint Remote Code Execution Vulnerability Yes


**Critical** 9.8 CVE-2026-58644 Microsoft SharePoint Remote Code Execution Vulnerability Yes


**Critical** 9.1 CVE-2026-55040 Microsoft SharePoint Server Security Feature Bypass Vulnerability Yes


## Critical Vulnerability in Dynamics NAV and Dynamics 365 Business Central


[CVE-2026-55944](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55944) is a **Critical** RCE vulnerability affecting Microsoft Dynamics NAV and Microsoft Dynamics 365 Business Central (On Premises) and has a **CVSS** score of **9.8** . A deserialization of untrusted data flaw (CWE-502) could allow an unauthenticated remote attacker to execute arbitrary code over a network. An attacker could exploit this vulnerability by sending a specially crafted login request to an affected Dynamics NAV or Business Central server, with no authentication or user interaction required


Table 8. Critical vulnerability in Dynamics NAV and Dynamics 365 Business Central **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-55944 Microsoft Dynamics NAV and Microsoft Dynamics 365 Business Central (On Premises) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Exchange Server


[CVE-2026-55008](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55008) is a **Critical** spoofing vulnerability affecting Microsoft Exchange Server and has a **CVSS** score of **9.6** . A cross-site scripting flaw (CWE-79) could allow an unauthenticated remote attacker to perform spoofing over a network. An attacker could exploit this vulnerability by sending a specially crafted malicious email to a user. If the user opens the email in Outlook Web Access and certain conditions are met, arbitrary JavaScript could be executed in the browser context.


Table 9. Critical vulnerability in Exchange Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.6 CVE-2026-55008 Microsoft Exchange Server Spoofing Vulnerability Yes


## Critical Vulnerability in Microsoft Copilot


[CVE-2026-48561](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-48561) is a **Critical** RCE vulnerability affecting Microsoft Copilot and has a **CVSS** score of **9.6** . A command injection flaw (CWE-77) allows an unauthenticated remote attacker to execute code with a changed scope impact, requiring only user interaction and low attack complexity.


Table 10. Critical vulnerability in Microsoft Copilot **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.6 CVE-2026-48561 Microsoft Copilot Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Graphics Components


[CVE-2026-50380](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50380) is a **Critical** RCE vulnerability affecting Windows GDI+ and has a **CVSS** score of **9.6** . A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated remote attacker to execute code with a changed scope impact, requiring only user interaction and low attack complexity. An attacker could exploit this vulnerability by convincing a user to open a specially crafted EMF+ file, causing the system to write beyond the bounds of allocated memory when rendering the malicious image data.


[CVE-2026-50382](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50382) is a **Critical** RCE vulnerability affecting the DirectX Graphics Kernel and has a **CVSS** score of **8.8** . An untrusted pointer dereference flaw (CWE-822) allows a low-privileged local attacker to execute code with a changed scope impact, with no user interaction and low attack complexity. An attacker could exploit this vulnerability by gaining access to a guest VM to attack the host OS, making this particularly relevant in virtualized environments.


[CVE-2026-49796](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49796) is a **Critical** RCE vulnerability affecting Windows GDI+ and has a **CVSS** score of **7.8** . A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated attacker to execute code locally, requiring user interaction and low attack complexity.


Windows GDI+ is the Windows graphics rendering subsystem responsible for processing image formats and rendering visual content; the DirectX Graphics Kernel handles GPU communication and graphics acceleration at the kernel level.


Table 11. Critical vulnerabilities in Windows Graphics Components **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.6 CVE-2026-50380 Windows GDI+ Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-50382 DirectX Graphics Kernel Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-49796 Windows GDI+ Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Active Directory


[CVE-2026-54121](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54121) is a **Critical** elevation of privilege vulnerability affecting Active Directory Certificate Services and has a **CVSS** score of **8.8** . An improper authorization flaw (CWE-285) allows a low-privileged remote attacker to elevate to administrator privileges with no user interaction and low attack complexity. Successful exploitation could allow an attacker to abuse the certificate enrollment process to authenticate as a high-value domain account, potentially including Domain Controllers, enabling privileged Active Directory operations.


[CVE-2026-49164](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49164) is a **Critical** RCE vulnerability affecting Active Directory Domain Services and has a **CVSS** score of **8.1** . A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated remote attacker to execute code with no user interaction, though high attack complexity requires the attacker to take additional preparatory actions prior to exploitation.


Table 12. Critical vulnerabilities in Active Directory **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-54121 Windows Active Directory Certificate Services Elevation of Privilege Vulnerability Yes


**Critical** 8.1 CVE-2026-49164 Windows Active Directory Domain Services Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows Reliable Multicast Transport Driver


[CVE-2026-54982](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54982) is a **Critical** RCE vulnerability affecting RMCAST and has a **CVSS** score of **8.8** . An integer underflow flaw (CWE-191) allows an unauthenticated attacker to execute code with no user interaction and low attack complexity. The attack vector is an adjacent network, meaning exploitation is limited to systems on the same network segment, switch, or virtual network as the attacker.


[CVE-2026-54995](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54995) is a **Critical** RCE vulnerability affecting RMCAST and has a **CVSS** score of **8.1** . A use-after-free flaw (CWE-416) allows an unauthenticated remote attacker to execute code with no user interaction, though high attack complexity requires the attacker to take additional preparatory actions prior to exploitation.


Windows Reliable Multicast Transport Driver (RMCAST) is a Windows kernel driver that enables reliable multicast communication over UDP, commonly used in enterprise environments for high-performance one-to-many data distribution such as financial market data feeds, live media streaming, and large-scale software deployment.


Table 13. Critical vulnerabilities in Windows Reliable Multicast Transport Driver **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-54982 Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability Yes


**Critical** 8.1 CVE-2026-54995 Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows TCP/IP


[CVE-2026-54999](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54999) is a **Critical** RCE vulnerability affecting Windows TCP/IP and has a **CVSS** score of **8.8** . A race condition flaw (CWE-362) allows an unauthenticated attacker to execute code with no user interaction and low attack complexity. The attack vector is an adjacent network, meaning exploitation is limited to systems on the same network segment, switch, or virtual network as the attacker.


Table 14. Critical vulnerability in Windows TCP/IP **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-54999 Windows TCP/IP Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Remote Desktop Client


[CVE-2026-50474](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50474) is a **Critical** RCE vulnerability affecting Remote Desktop Client and has a **CVSS** score of **8.8** . A use-after-free flaw (CWE-416) allows an unauthenticated remote attacker to execute code with low attack complexity, though it requires user interaction. An attacker controlling a malicious Remote Desktop server could execute code on a victim's machine when the victim connects using a vulnerable Remote Desktop Client.


Table 15. Critical vulnerability in Remote Desktop Client **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-50474 Remote Desktop Client Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Server Update Services


[CVE-2026-50444](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50444) is a **Critical** elevation of privilege vulnerability affecting Windows Server Update Services (WSUS) and has a **CVSS** score of **8.8** . A missing authentication for a critical function flaw (CWE-306) allows a low-privileged remote attacker to elevate privileges with no user interaction and low attack complexity.


Table 16. Critical vulnerability in Windows Server Update Services **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-50444 Windows Server Update Service (WSUS) Elevation of Privilege Vulnerability Yes


## Critical Vulnerability in Windows Print Spooler


[CVE-2026-58608](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58608) is a **Critical** RCE vulnerability affecting Windows Print Spooler and has a **CVSS** score of **8.8** . Race condition and use-after-free flaws (CWE-362, CWE-416) allow a low-privileged remote attacker to execute code with no user interaction and low attack complexity. An attacker could exploit this vulnerability by manipulating the Print Spooler service into referencing freed memory, achieved by creating and closing a printer handle while leaving a related notification handle in an invalid state.


Table 17. Critical vulnerability in Windows Print Spooler **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-58608 Windows Print Spooler Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in SQL Server


[CVE-2026-54117](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54117) and[CVE-2026-54118](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54118) are **Critical** RCE vulnerabilities affecting Microsoft SQL Server, both with a **CVSS** score of **8.8** . Both vulnerabilities stem from deserialization of untrusted data flaws (CWE-502) and allow authenticated attackers with explicit permissions to execute arbitrary code over a network. An attacker who successfully exploited these vulnerabilities could log in to the SQL Server and elevate their privileges to sysadmin.


Table 18. Critical vulnerabilities in SQL Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-54117 Microsoft SQL Server Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-54118 Microsoft SQL Server Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows Media Foundation


[CVE-2026-57087](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57087) ,[CVE-2026-57090](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57090) ,[CVE-2026-57094](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57094) ,[CVE-2026-50655](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50655) , and[CVE-2026-56189](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56189) are **Critical** RCE vulnerabilities in Windows Media Foundation, with a **CVSS** score of **8.8** (CVE-2026-57087, CVE-2026-57090, CVE-2026-57094) or **7.8** (CVE-2026-50655, CVE-2026-56189). Windows Media Foundation is Microsoft's multimedia processing pipeline API, providing a framework for handling audio and video content on Windows systems. All five vulnerabilities stem from heap-based buffer overflow flaws (CWE-122).


CVE-2026-57087, CVE-2026-57090, and CVE-2026-57094 allow unauthenticated attackers to execute arbitrary code over a network, while CVE-2026-50655 and CVE-2026-56189 are exploitable locally. Exploitation of CVE-2026-57087 and CVE-2026-57094 requires an attacker to convince a user to open a specially crafted media file.


Table 19. Critical vulnerabilities in Windows Media Foundation **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-57087 Microsoft Windows Media Foundation Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-57090 Microsoft Windows Media Foundation Remote Code Execution Vulnerability Yes


**Critical** 8.8 CVE-2026-57094 Microsoft Windows Media Foundation Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-50655 Microsoft Windows Media Foundation Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-56189 Microsoft Windows Media Foundation Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Windows Message Queuing


[CVE-2026-54992](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54992) is a **Critical** RCE vulnerability affecting Windows Message Queuing (MSMQ) Queue Manager and has a **CVSS** score of **8.4** . MSMQ is a Windows messaging infrastructure component that enables applications to communicate asynchronously by sending and receiving messages via queues. A heap-based buffer overflow flaw (CWE-122) allows an unauthenticated attacker to execute code locally with no user interaction and low attack complexity.


Table 20. Critical vulnerability in Windows Message Queuing **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.4 CVE-2026-54992 Microsoft Message Queuing Queue Manager Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Office


[CVE-2026-55045](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55045) ,[CVE-2026-50314](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50314) ,[CVE-2026-50467](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50467) ,[CVE-2026-55018](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55018) ,[CVE-2026-55022](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55022) ,[CVE-2026-55049](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55049) ,[CVE-2026-55056](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55056) ,[CVE-2026-55129](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55129) , and[CVE-2026-55140](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55140) are **Critical** RCE vulnerabilities in Microsoft Office, with a **CVSS** score of **8.4** (CVE-2026-55045) or **7.8** (all others). These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through heap-based buffer overflow flaws (CVE-2026-55049, CVE-2026-55056, CVE-2026-55129, CVE-2026-55140), use-after-free flaws (CVE-2026-50314, CVE-2026-50467, CVE-2026-55018), a type confusion flaw (CVE-2026-55022), and an out-of-bounds read flaw (CVE-2026-55045). The Preview Pane is an attack vector for all nine vulnerabilities. Exploitation otherwise requires an attacker to convince a user to open a specially crafted malicious Office file.


Table 21. Critical vulnerabilities in Office **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.4 CVE-2026-55045 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-50314 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-50467 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55018 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55022 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55049 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55056 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55129 Microsoft Office Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55140 Microsoft Office Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows Hyper-V


[CVE-2026-50680](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50680) and[CVE-2026-54127](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54127) are **Critical** elevation of privilege vulnerabilities affecting Windows Hyper-V, with **CVSS** scores of **8.2** and **7.4** , respectively. These vulnerabilities could allow attackers to elevate their privileges locally through a heap-based buffer overflow flaw (CVE-2026-50680) and a use-after-free flaw (CVE-2026-54127). An attacker who successfully exploited CVE-2026-50680 could gain SYSTEM privileges.


Table 22. Critical vulnerabilities in Windows Hyper-V **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.2 CVE-2026-50680 Windows Hyper-V Elevation of Privilege Vulnerability Yes


**Critical** 7.4 CVE-2026-54127 Windows Hyper-V Elevation of Privilege Vulnerability Yes


## Critical Vulnerability in Windows Secure Socket Tunneling Protocol


[CVE-2026-50694](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50694) is a **Critical** RCE vulnerability affecting Windows Secure Socket Tunneling Protocol (SSTP) and has a **CVSS** score of **8.1** . Windows SSTP is a VPN tunneling protocol that encapsulates PPP traffic over HTTPS, commonly used to provide remote access through firewalls and proxies that block traditional VPN protocols. A use-after-free flaw (CWE-416) allows an unauthenticated remote attacker to execute code by sending a specially crafted SSTP packet to a vulnerable server, with no user interaction required. High attack complexity requires the attacker to take additional preparatory actions prior to exploitation.


Table 23. Critical vulnerability in Windows Secure Socket Tunneling Protocol (SSTP) **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.1 CVE-2026-50694 Windows Secure Socket Tunneling Protocol (SSTP) Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows Secure Kernel Mode


[CVE-2026-42982](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-42982) and[CVE-2026-50392](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50392) are **Critical** elevation of privilege vulnerabilities affecting Windows Secure Kernel Mode and have **CVSS** scores of **7.8** and **7.0** , respectively. Windows Secure Kernel Mode is the isolated, high-privilege execution environment that underpins Windows security features such as Credential Guard and Virtualization-Based Security; vulnerabilities here can allow attackers to break out of security boundaries and gain SYSTEM privileges. Both vulnerabilities allow a low-privileged local attacker to elevate to SYSTEM privileges with no user interaction. CVE-2026-42982 stems from an improper validation of consistency within input flaw (CWE-1288) with low attack complexity, while CVE-2026-50392 stems from a use-after-free flaw (CWE-416) with high attack complexity, requiring an attacker to win a race condition prior to exploitation.


Table 24. Critical vulnerabilities in Windows Secure Kernel Mode **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-42982 Windows Secure Kernel Mode Elevation of Privilege Vulnerability Yes


**Critical** 7.0 CVE-2026-50392 Windows Secure Kernel Mode Elevation of Privilege Vulnerability Yes


## Critical Vulnerabilities in Microsoft Defender


[CVE-2026-55011](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55011) and[CVE-2026-55012](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55012) are **Critical** RCE vulnerabilities affecting Microsoft Defender's Malware Protection Engine, both with a **CVSS** score of **7.8** . These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through an integer underflow flaw (CVE-2026-55011) and an integer overflow flaw (CVE-2026-55012). Exploitation requires an attacker to convince a user to download and open a specially crafted file.


Table 25. Critical vulnerabilities in Microsoft Defender **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-55011 Microsoft Defender Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55012 Microsoft Defender Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Windows Media


[CVE-2026-50327](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50327) and[CVE-2026-58542](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58542) are **Critical** RCE vulnerabilities in Windows Media, both with a **CVSS** score of **7.8** . Windows Media is a Microsoft framework for playing, organizing, and managing digital media such as audio and video on Windows systems. Both vulnerabilities allow attackers to execute arbitrary code locally through heap-based buffer overflow flaws (CWE-122).


Table 26. Critical vulnerabilities in Windows Media **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-50327 Windows Media Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-58542 Windows Media Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in PowerPoint


[CVE-2026-55043](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55043) ,[CVE-2026-55120](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55120) , and[CVE-2026-55123](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55123) are **Critical** RCE vulnerabilities in Microsoft PowerPoint, all with a **CVSS** score of **7.8** . All three vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through heap-based buffer overflow flaws (CWE-122). Exploitation requires an attacker to convince a user to open a specially crafted malicious Office file. The Preview Pane is not an attack vector for any of these vulnerabilities.


Table 27. Critical vulnerabilities in PowerPoint **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-55043 Microsoft PowerPoint Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55120 Microsoft PowerPoint Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55123 Microsoft PowerPoint Remote Code Execution Vulnerability Yes


## Critical Vulnerabilities in Word


[CVE-2026-55033](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55033) ,[CVE-2026-55127](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55127) , and[CVE-2026-55132](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55132) are **Critical** RCE vulnerabilities in Microsoft Word, all with a **CVSS** score of **7.8** . These vulnerabilities allow unauthenticated attackers to execute arbitrary code locally through an integer overflow flaw (CVE-2026-55033), a heap-based buffer overflow flaw (CVE-2026-55127), and a double free flaw (CVE-2026-55132). The Preview Pane is an attack vector for all three vulnerabilities. Exploitation otherwise requires an attacker to convince a user to open a specially crafted malicious Office file.


Table 28. Critical vulnerabilities in Word **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 7.8 CVE-2026-55033 Microsoft Word Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55127 Microsoft Word Remote Code Execution Vulnerability Yes


**Critical** 7.8 CVE-2026-55132 Microsoft Word Remote Code Execution Vulnerability Yes


## Critical Vulnerability in Azure OpenAI


[CVE-2026-45499](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-45499) is a **Critical** elevation of privilege vulnerability affecting Azure OpenAI and has a **CVSS** score of **9.9** . Azure OpenAI is Microsoft's cloud-hosted service providing access to OpenAI's large language models, including GPT and other AI capabilities, integrated into the Azure platform for enterprise use. A server-side request forgery flaw (CWE-918) allows a low-privileged remote attacker to elevate privileges with no user interaction, low attack complexity, and a changed scope impact. As a fully cloud-hosted service, this vulnerability was mitigated entirely on Microsoft's infrastructure, and no customer action is required.


Table 29. Critical vulnerability in Azure OpenAI **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-45499 Azure OpenAI Elevation of Privilege Vulnerability No


## Critical Vulnerability in Entra Provisioning Service


[CVE-2026-57100](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57100) is a **Critical** elevation of privilege vulnerability affecting Microsoft Entra Provisioning Service and has a **CVSS** score of **9.9** . Microsoft Entra Provisioning Service (SyncFabric) is a cloud-based identity provisioning platform that automates the creation, management, and synchronization of user identities between Microsoft Entra ID and connected applications and directories. A server-side request forgery flaw (CWE-918) allows a low-privileged remote attacker to elevate privileges with no user interaction, low attack complexity, and a changed scope impact.


Table 30. Critical vulnerability in Entra Provisioning Service **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.9 CVE-2026-57100 Microsoft Entra Provisioning Service Elevation of Privilege Vulnerability No


## Critical Vulnerability in Minecraft Bedrock Dedicated Server


[CVE-2026-55010](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55010) is a **Critical** RCE vulnerability affecting Minecraft Bedrock Dedicated Server and has a **CVSS** score of **9.8** . A heap-based buffer overflow flaw (CWE-122) could allow an unauthenticated remote attacker to execute arbitrary code over a network.


Table 31. Critical vulnerability in Minecraft Bedrock Dedicated Server **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.8 CVE-2026-55010 Minecraft Bedrock Dedicated Server Remote Code Execution Vulnerability No


## Critical Vulnerability in Microsoft 365 Copilot


[CVE-2026-41106](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-41106) is a **Critical** elevation of privilege vulnerability affecting Microsoft 365 Copilot and has a **CVSS** score of **9.3** . A URL redirection to an untrusted site flaw (CWE-601) could allow an unauthenticated remote attacker to elevate their privileges over a network.


Table 32. Critical vulnerability in Microsoft 365 Copilot **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 9.3 CVE-2026-41106 Microsoft 365 Copilot Elevation of Privilege Vulnerability No


## Critical Vulnerability in Exchange Online


[CVE-2026-54998](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54998) is a **Critical** elevation of privilege vulnerability affecting Microsoft Exchange Online and has a **CVSS** score of **8.8** . An incorrect authorization flaw (CWE-863) could allow an authorized attacker to elevate their privileges over a network.


Table 33. Critical vulnerability in Exchange Online **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 8.8 CVE-2026-54998 Microsoft Exchange Online Elevation of Privilege Vulnerability No


## Critical Vulnerability in Azure Synapse


[CVE-2026-26145](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-26145) is a **Critical** elevation of privilege vulnerability affecting Azure Synapse and has a **CVSS** score of **4.8** . Azure Synapse is a cloud-based analytics service that combines enterprise data warehousing and big data analytics. An improper access control flaw (CWE-284) could allow an authenticated remote attacker to elevate their privileges over a network.


Table 34. Critical vulnerability in Azure Synapse **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Critical** 4.8 CVE-2026-26145 Microsoft Azure Synapse Elevation of Privilege Vulnerability No


## Important Vulnerability in Windows Remote Desktop Protocol


CrowdStrike's Counter Adversary Operations Advanced Research Team discovered[CVE-2026-56190](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56190) , an **Important** RCE vulnerability affecting Windows Remote Desktop Protocol (RDP) with a **CVSS** score of **9.8** . A use of uninitialized resource flaw (CWE-908) allows an unauthenticated remote attacker to execute code with no user interaction and low attack complexity. An attacker could exploit this vulnerability by sending specially crafted RDP traffic to a system where Network Level Authentication (NLA) is disabled, causing memory corruption that may allow an attacker to write data to unintended memory locations.


Table 35. Important vulnerability in Windows Remote Desktop Protocol **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 9.8 CVE-2026-56190 Remote Desktop Protocol Remote Code Execution Vulnerability Yes


## Important Vulnerability in Windows Audio Compression Manager


CrowdStrike's Counter Adversary Operations Advanced Research Team discovered[CVE-2026-50351](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50351) , an **Important** elevation of privilege vulnerability affecting Windows Audio Compression Manager (ACM) with a **CVSS** score of **7.8** . An improper access control flaw (CWE-284) allows a low-privileged local attacker to elevate privileges with no user interaction and low attack complexity.


Table 36. Important vulnerability in Windows Audio Compression Manager **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 7.8 CVE-2026-50351 Windows Audio Compression Manager (ACM) Elevation of Privilege Vulnerability Yes


## Important Vulnerability in Windows Human Interface Device


CrowdStrike's Counter Adversary Operations Advanced Research Team discovered[CVE-2026-50310](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50310) , an **Important** information disclosure vulnerability affecting Windows Human Interface Device (HID) with a **CVSS** score of **4.7** . An integer overflow flaw (CWE-190) allows a low-privileged local attacker to disclose kernel memory addresses, though high attack complexity requires the attacker to take additional preparatory actions prior to exploitation. Knowledge of kernel memory addresses could potentially be leveraged to facilitate further exploitation of the affected system.


Table 37. Important vulnerability in Windows Human Interface Device **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 4.7 CVE-2026-50310 Windows Human Interface Device Information Disclosure Vulnerability Yes


## Important Vulnerability in Win32k


CrowdStrike's Counter Adversary Operations Advanced Research Team discovered[CVE-2026-50416](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50416) , an **Important** information disclosure vulnerability affecting Windows Win32k with a **CVSS** score of **3.3** . An exposure of sensitive information flaw (CWE-200) allows a low-privileged local attacker to read small portions of heap memory with no user interaction and low attack complexity, though the scope of information disclosure is limited and the attacker cannot modify disclosed information or impact availability.


Table 38. Important vulnerability in Win32k **Severity** **CVSS Score** **CVE** **Description** **Action Required?**


**Important** 3.3 CVE-2026-50416 Win32k Information Disclosure Vulnerability Yes


## Patch Tuesday Dashboard in the Falcon Platform


For a visual overview of the systems impacted by this month’s vulnerabilities, CrowdStrike customers can use our Patch Tuesday dashboard. This can be found in the CrowdStrike Falcon® platform within the Exposure Management > Vulnerability Management > Dashboards page. The preset dashboards show the most recent three months of Patch Tuesday vulnerabilities.


## New AI-Powered Capabilities in Falcon Exposure Management


With CrowdStrike Falcon® Exposure Management, you can automatically classify and prioritize assets, show attack paths targeting client-side exploitation of devices, and integrate with CrowdStrike Falcon® Next-Gen SIEM. Learn more in this blog post:


[Falcon Exposure Management’s AI-Powered Risk Prioritization Shows Organizations What to Fix First](https://www.crowdstrike.com/en-us/blog/falcon-exposure-management-ai-driven-risk-prioritization-shows-what-to-fix-first/)


## Not All Relevant Vulnerabilities Have Patches: Consider Mitigation Strategies


As we have learned with other notable vulnerabilities, such as[Log4j](https://www.crowdstrike.com/log4j2/) , not every highly exploitable vulnerability can be easily patched. As is the case for the[ProxyNotShell](https://www.crowdstrike.com/blog/owassrf-exploit-analysis-and-recommendations/) vulnerabilities, it’s critically important to develop a response plan for how to defend your environments when no patching protocol exists.


Regular review of your patching strategy should still be a part of your program, but you should also look more holistically at your organization's methods for cybersecurity and improve your overall security posture.


## Learn More


The CrowdStrike Falcon platform regularly collects and analyzes trillions of endpoint events every day from millions of sensors deployed across 176 countries.[Watch this demo to see the Falcon platform in action](https://go.crowdstrike.com/product-demo-platform.html) .


Learn more about how CrowdStrike Falcon Exposure Management can help you quickly and easily discover and prioritize vulnerabilities and other types of exposures[here](https://www.crowdstrike.com/products/exposure-management/falcon-exposure-management/) .


### About CVSS Scores


The[Common Vulnerability Scoring System](https://www.first.org/cvss/) (CVSS) is a free and open industry standard that CrowdStrike and many other cybersecurity organizations use to assess and communicate software vulnerabilities’ severity and characteristics. The CVSS Base Score ranges from 0.0 to 10.0, and the[National Vulnerability Database](https://nvd.nist.gov/) (NVD) adds a severity rating for CVSS scores. Learn more about vulnerability scoring[in this article](https://www.crowdstrike.com/epp-101/vulnerability-management-programs/) .


#### Additional Resources


- *For more information on which products are in Microsoft’s Extended Security Updates program, refer to the vendor guidance[here](https://learn.microsoft.com/en-us/lifecycle/faq/extended-security-updates) .*
- *Learn how[Falcon Exposure Management](https://www.crowdstrike.com/products/exposure-management/falcon-exposure-management/) can help you discover and manage vulnerabilities and other exposures in your environments.*
- *Make prioritization painless and efficient. Watch how Falcon Exposure Management enables IT staff to improve visibility with[custom filters and team dashboards.](https://www.crowdstrike.com/tech-hub/?category=exposure-management)*
- *Find out how[CrowdStrike Falcon® Next-Gen Identity Security](https://www.crowdstrike.com/products/identity-protection/) products can stop workforce identity threats faster.*
- *Test CrowdStrike next-gen antivirus for yourself with a[free trial of CrowdStrike® Falcon Prevent™](https://go.crowdstrike.com/try-falcon-prevent.html) .*
- *Fal.Con 2026 registration is now open.[Join us](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) in Las Vegas to explore what’s next in cybersecurity.*
