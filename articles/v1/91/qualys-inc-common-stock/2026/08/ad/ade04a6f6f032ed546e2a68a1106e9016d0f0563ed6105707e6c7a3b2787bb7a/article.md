---
schema_version: "1.0.0"
document_id: "ade04a6f6f032ed546e2a68a1106e9016d0f0563ed6105707e6c7a3b2787bb7a"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/patch-tuesday/2026/08/11/microsoft-patch-tuesday-august-2026-security-update-review"
published_at: "2026-08-11T21:55:19+00:00"
first_seen_at: "2026-08-12T00:02:31.456799+00:00"
fetched_at: "2026-08-12T00:02:33.001294+00:00"
content_hash: "sha256:867daaff512d5806fbcc3fbd0dac58c6a9bf253912a45bb2a7b4e3c57f0346ff"
---

# Microsoft and Adobe Patch Tuesday, August 2026 Security Update Review

#### Table of Contents


- Microsoft Patch Tuesday forAugust2026
- Zero-day Vulnerabilities Patched inAugustPatch Tuesday Edition
- Critical Severity Vulnerabilities Patched inAugustPatch Tuesday Edition
- Other Microsoft Vulnerability Highlights
- Microsoft Release Summary
- Qualys Monthly Webinar Series


The August 2026 Microsoft Patch Tuesday release delivers security fixes for vulnerabilities affecting a wide range of Microsoft products and services. As attackers continue to exploit unpatched vulnerabilities, timely patching remains critical for reducing exposure and strengthening enterprise security.


## **Microsoft Patch Tuesday for August 2026**


This month’s release addresses **421** vulnerabilities, including **62** critical and **357** important-severity **** vulnerabilities.


In this month’s updates, Microsoft has addressed **three** zero-day vulnerabilities: **two** publicly disclosed and **one** exploited in the wild.


Microsoft has not addressed any vulnerabilities in Microsoft Edge (Chromium-based) in this month’s update.


Microsoft Patch Tuesday, August edition, includes updates for vulnerabilities in Windows HTTP.sys, Windows Hyper-V, Windows NTFS, Desktop Window Manager, Dynamics Business Central, GitHub Copilot and Visual Studio Code, Microsoft Exchange Server, and more.


This month’s release includes fixes for several high-severity issues that could potentially enable remote code execution, privilege escalation, or denial-of-service attacks. As always, timely patch deployment is crucial to reduce exposure and ensure systems remain resilient against exploitation attempts.


The August 2026 Microsoft vulnerabilities are classified as follows:


**Vulnerability Category** **Quantity** **Severities**


Spoofing Vulnerability 20 Critical: 3
Important: 17


Denial of Service Vulnerability 12 Important: 12


Elevation of Privilege Vulnerability 178 Critical: 19
Important: 159


Information Disclosure Vulnerability 84 Important: 84


Remote Code Execution Vulnerability 109 Critical: 39
Important: 70


Security Feature Bypass Vulnerability 11 Important: 11


## **Zero-day Vulnerabilities Patched in August Patch Tuesday Edition**


### [CVE-2026-72971: Windows Container Isolation FS Filter Driver (unionfs.sys) Tampering Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-72971) ****


This is a link following flaw that may allow an authenticated attacker to perform tampering locally.


### [CVE-2026-62832: Windows User Profile Service Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62832) ****


This is a link following flaw that may allow an authenticated attacker to elevate privileges locally. Upon successful exploitation, this vulnerability may allow an attacker to gain ADMINISTRATOR privileges.


### [CVE-2026-68820: Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68820) ****


The use-after-free flaw in the Windows Ancillary Function Driver for WinSock may allow an authenticated attacker to elevate privileges locally. Successful exploitation of the vulnerability may allow an attacker to gain SYSTEM privileges.


## **Critical Severity Vulnerabilities Patched in August Patch Tuesday Edition**


### [CVE-2026-49163: Application Insights Profiler Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49163) ****


This is a path traversal flaw that may allow an authenticated attacker to elevate privileges over a network.


### [CVE-2026-50481: Azure Active Directory Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50481) ****


Successful exploitation of the vulnerability may allow an authenticated attacker to elevate privileges over a network.


### [CVE-2026-68823: Azure Confidential Ledger Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68823) ****


Successful exploitation of the vulnerability may allow an authenticated attacker to execute code over a network.


### [CVE-2026-62869: Azure Entra ID Spoofing Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62869) ****


Successful exploitation of the vulnerability may allow an authenticated attacker to perform spoofing over a network.


### [CVE-2026-56161: Azure Logic Apps Information Disclosure Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56161) ****


Microsoft has not provided any information about the vulnerability.


### [CVE-2026-50515: Azure Service Bus Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50515) ****


This is a deserialization of untrusted data that may allow an authenticated attacker to execute code over a network.


### [CVE-2026-63522: Azure SQL Database Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63522) ****


### [CVE-2026-56162: Azure SQL Database Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56162) ****


This is an improper authentication in Azure SQL Database that may allow an unauthenticated attacker to elevate privileges over a network.


### [CVE-2026-62836: Azure SQL Managed Instance Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62836) ****


Successful exploitation of the vulnerability may allow an unauthenticated attacker to elevate privileges over a network.


### [CVE-2026-62830: Azure SRE Agent Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62830) ****


### [CVE-2026-62873: Microsoft 365 Admin Center Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62873) ****


### [CVE-2026-71331: Windows Device Health Attestation (DHA) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-71331) ****


An integer overflow flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-66802: Windows Device Health Attestation (DHA) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-66802) ****


A race condition flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-50516: Microsoft Azure Kubernetes Service Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50516) ****


A missing authentication flaw that may allow an unauthenticated attacker to elevate privileges over a network.


### [CVE-2026-59115: Microsoft Entra Provisioning Service Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-59115) ****


### [CVE-2026-62911: Microsoft Exchange Server Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62911) ****


### [CVE-2026-63515: Microsoft Office Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63515) ****


An out-of-bounds read flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-63513: Microsoft Office Graphics Component Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63513) ****


This is a heap-based buffer overflow flaw that that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-64910: Microsoft Office Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64910) ****


An untrusted pointer dereference flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-64909: Microsoft Office Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64909) ****


An integer underflow flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-63532,](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63532) ****[CVE-2026-64903](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64903) **, &**[CVE-2026-64911](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64911) **: Microsoft Office Remote Code Execution Vulnerability**


An integer overflow flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-64898](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64898) **&[CVE-2026-70130](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-70130) : Microsoft Office Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-66807](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-66807) **&**[CVE-2026-63526](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63526) **: Microsoft Office Graphics Component Remote Code Execution Vulnerability**


A stack-based buffer overflow flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-62815: Microsoft QUIC Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62815) ****


A use-after-free flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-62816: Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62816) ****


A heap-based buffer overflow flaw that may allow an unauthenticated attacker to execute code over an adjacent network.


### [CVE-2026-62817: Windows DNS Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62817) ****


An out-of-bounds write flaw in Windows DNS that may allow an unauthenticated attacker to execute code over an adjacent network.


### [CVE-2026-62818: Windows Active Directory Certificate Services (AD CS) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62818) ****


A use-after-free flaw that may allow an authenticated attacker to execute code over a network.


### [CVE-2026-62819: Windows Routing and Remote Access Service (RRAS) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62819) ****


Successful exploitation of the vulnerability may allow an unauthenticated attacker to execute code remotely.


### [CVE-2026-62820: Windows DNS Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62820) ****


### [CVE-2026-62878: Windows DNS Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62878) ****


A stack-based buffer overflow flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-62889: Windows Secure Socket Tunneling Protocol (SSTP) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62889) ****


A double-free flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-65791: Windows iSCSI Target Service Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65791) ****


A heap-based buffer overflow flaw that may allow an unauthenticated attacker to execute code over a network.


### [CVE-2026-66799: Windows Key Guard Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-66799) ****


A heap-based buffer overflow flaw that may allow an authenticated attacker to elevate privileges locally.


### [CVE-2026-62890: Windows GDI+ Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62890) ****


This is a heap-based buffer overflow flaw that may allow an authenticated attacker to execute code locally.


### [CVE-2026-62893: Windows Deployment Services TFTP Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62893) ****


### [CVE-2026-63518: Microsoft Office Word Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63518) ****


### [CVE-2026-63519: Microsoft Office Graphics Component Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63519) ****


This is a heap-based buffer overflow flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-65657: Microsoft Office Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65657) ****


A use-after-free flaw that may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-65664: Microsoft Office Graphics Component Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65664) ****


### [CVE-2026-65665: Microsoft SharePoint Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65665) ****


A deserialization of untrusted data flaw that may allow an authenticated attacker to execute code over a network.


### [CVE-2026-65789: Windows DNS Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65789) ****


### [CVE-2026-68794: Microsoft Excel Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68794) ****


### [CVE-2026-68804: Microsoft Excel Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68804) ****


Successful exploitation of the vulnerability may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-68816: Microsoft Excel Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-68816) ****


### [CVE-2026-62827: Microsoft SharePoint Server Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62827) ****


An improper authentication flaw that may allow an authenticated attacker to elevate privileges over a network.


### [CVE-2026-64921: Microsoft SharePoint Server Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64921) ****


### [CVE-2026-70332: Microsoft Office SharePoint Spoofing Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-70332) ****


A cross-site scripting flaw that may allow an unauthenticated attacker to perform spoofing over a network.


### [CVE-2026-64907: Microsoft Office Word Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-64907) ****


### [CVE-2026-63525: Microsoft Office Word Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63525) ****


### [CVE-2026-63508: Microsoft Planetary Computer Pro Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-63508) ****


### [CVE-2026-59118: Copilot Cowork Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-59118) ****


An improper authorization flaw that may allow an unauthenticated attacker to elevate privileges over a network.


### [CVE-2026-65668: Microsoft Purview eDiscovery Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65668) ****


An improper access control flaw that may allow an authenticated attacker to elevate privileges over a network.


### [CVE-2026-62896: Microsoft Teams Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62896) ****


### [CVE-2026-65667: Microsoft Teams Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-65667) ****


A missing authorization flaw that may allow an unauthenticated attacker to elevate privileges over a network.


### [CVE-2026-62918: Microsoft Teams Spoofing Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62918) ****


### [CVE-2026-62824: Remote Desktop Client Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62824) ****


### [CVE-2026-62823: Windows DHCP Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-62823) ****


## **Other Microsoft Vulnerability Highlights**


- **CVE-2026-59124** is a remote code execution vulnerability in Microsoft High-Performance Computing (HPC). Successful exploitation of the vulnerability may allow an unauthenticated attacker to execute code over a network.
- **CVE-2026-59133** is an elevation of privilege vulnerability in Microsoft High-Performance Computing (HPC). An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-59132** is a denial-of-service vulnerability in Windows TCP/IP. The vulnerability may allow an unauthenticated attacker to deny service over a network.
- **CVE-2026-61348** is an elevation of privilege vulnerability in the Windows Ancillary Function Driver for WinSock. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-61925** is an elevation of privilege vulnerability in the Windows Installer. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-61930** is an elevation of privilege vulnerability in the Windows Kernel. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62688** is an elevation of privilege vulnerability in the Windows MIDI Service Module. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62696** is an elevation of privilege vulnerability in the Windows Program Compatibility Assistant Service. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62713** is an elevation of privilege vulnerability in the Windows Cloud Files Mini Filter Driver. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62712** is an elevation of privilege vulnerability in the Windows Win32k. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62735** is an elevation of privilege vulnerability in the Windows HTTP.sys. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62737** is an elevation of privilege vulnerability in the Windows Kernel. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62783** is an elevation of privilege vulnerability in the Windows Remote Access Connection Manager. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62766** is an elevation of privilege vulnerability in the Windows Kerberos. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-63520** is a remote code execution vulnerability in Microsoft SharePoint Server. Successful exploitation of the vulnerability may allow an unauthenticated attacker to execute code over a network.
- **CVE-2026-65788** is an elevation of privilege vulnerability in Desktop Window Manager. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-69278** is a security feature bypass vulnerability in Visual Studio Code. Successful exploitation of the vulnerability may allow an unauthenticated attacker to bypass a security feature locally.
- **CVE-2026-61358** is an elevation of privilege vulnerability in the Windows Accessibility Infrastructure (ATBroker.exe). An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-61929** & **CVE-2026-62788** are elevation of privilege vulnerabilities in the Windows Kernel. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62698** is an elevation of privilege vulnerability in Microsoft Digest Authentication. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62721** is an elevation of privilege vulnerability in the Windows User-Mode Power Service (UMPS). An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62741** is an elevation of privilege vulnerability in the Windows HTTP.sys. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-62888** is an elevation of privilege vulnerability in the Windows DWM Core Library. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.
- **CVE-2026-65775** is an elevation of privilege vulnerability in the Windows Win32k. An attacker who successfully exploited this vulnerability could gain SYSTEM privileges.


## **Microsoft Release Summary**


This month’s release notes cover multiple Microsoft product families and products/versions affected, including, but not limited to, .NET, .NET Framework, Active Directory Certificate Services (AD CS), AMD Zen, Application Information Services, Application Insights Profiler, Azure Active Directory, Azure Confidential Ledger, Azure CycleCloud, Azure Entra ID, Azure Logic Apps, Azure Monitor Agent, Azure Service Bus, Azure SQL Database, Azure SQL Managed Instance, Azure SRE Agent, Azure Storage Explorer, Capability Access Management Service (camsvc), Microsoft 365 Admin Center, Microsoft Azure Attestation service and Device Health Attestation Service, Microsoft Azure Kubernetes Service, Microsoft COM for Windows, Microsoft Defender for Endpoint, Microsoft Digest Authentication, Microsoft Dynamics 365 (on-premises), Microsoft Entra Connect Sync, Microsoft Entra Provisioning Service (SyncFabric), Microsoft High Performance Computing (HPC) Pack, Microsoft Local Security Authority Server (lsasrv), Microsoft Office, Microsoft Office Access, Microsoft Office Excel, Microsoft Office Outlook, Microsoft Office PowerPoint, Microsoft Office SharePoint, Microsoft Office Word, Microsoft OneDrive, Microsoft Planetary Computer Pro, Microsoft Power Apps, Microsoft PowerShell, Microsoft PowerShell Core, Microsoft Purview eDiscovery, Microsoft QUIC, Microsoft Remote Registry Service, Microsoft Teams, Microsoft Teams for Android, Microsoft Teams Mobile, Microsoft Windows Search Component, Power BI, Reliable Multicast Transport Driver (RMCAST), Remote Desktop Client, RPC Runtime, User-Mode Power Service (UMPS), Virtual Hard Disk (VHD) Miniport Driver, Visual Studio Code, Visual Studio Code – Python extension, Visual Studio Code CoPilot Chat Extension, Windows Accessibility Infrastructure (ATBroker.exe), Windows Active Directory, Windows Ancillary Function Driver for WinSock, Windows Autopilot, Windows Backup Engine, Windows Bind Filter Driver, Windows Cloud Files Mini Filter Driver, Windows Common Log File System Driver, Windows Container Isolation FS Filter Driver (unionfs.sys), Windows Cross Device Service, Windows Defender Firewall Service, Windows Deployment Services, Windows Device Association Service, Windows DHCP Client, Windows DHCP Server, Windows Display Enhancement Service, Windows DNS, Windows DWM Core Library, Windows Encrypting File System (EFS), Windows Event Logging Service, Windows GDI, Windows GDI+, Windows Graphics Kernel, Windows Hello, Windows HTTP Protocol Stack, Windows Imaging Component, Windows Installer, Windows iSCSI Target Service, Windows Kerberos, Windows Kernel, Windows Key Guard, Windows LDAP – Lightweight Directory Access Protocol, Windows License Manager, Windows LUAFV, Windows Management Instrumentation, Windows Management Services, Windows Message Queuing, Windows MIDI Service Module, Windows Modern Device Management (MDM), Windows Narrator Braille, Windows Network Address Translation (NAT), Windows Network Connection Broker, Windows Network File System, Windows Package Manager, Windows Program Compatibility Assistant Service, Windows Projected File System, Windows Push Notifications, Windows Remote Access API, Windows Remote Access Connection Manager, Windows Remote Desktop Services, Windows Routing and Remote Access Service (RRAS), Windows RPC API, Windows Schannel, Windows Secure Socket Tunneling Protocol (SSTP), Windows Sensor Data Service, Windows Shell, Windows SMB Client, Windows SMB Server, Windows Storage, Windows Storage Port Driver, Windows TCP/IP, Windows Telephony Service, Windows TPM, Windows Universal Disk Format File System Driver (UDFS), Windows USB Driver, Windows User Profile Service, Windows Win32K, Windows Wired AutoConfig Service, Windows Work Folder Service, and Winlogon.


The next Patch Tuesday is scheduled for September 8, and we will provide details and patch analysis then. Until next Patch Tuesday, stay safe and secure. Be sure to subscribe to the ‘This Month in Vulnerabilities and Patches’ webinar.’


## Qualys Monthly Webinar Series


The Qualys Research team hosts a monthly webinar series to help our existing customers leverage the seamless integration between[Qualys Vulnerability Management, Detection & Response (VMDR)](https://www.qualys.com/apps/vulnerability-management-detection-response/) , and[Qualys Patch Management](https://www.qualys.com/apps/patch-management/) . Combining these two solutions can reduce the median time to remediate critical vulnerabilities.


During the webcast, we will discuss this month’s high-impact vulnerabilities, including those highlighted in this month’s Patch Tuesday alert. We will walk you through the necessary steps to address the key vulnerabilities using Qualys VMDR and Qualys Patch Management.


### Join the webinar


### This Month in Vulnerabilities & Patches


[Register Now](https://www.brighttalk.com/webcast/11673/668916)
