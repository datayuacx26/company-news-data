---
schema_version: "1.0.0"
document_id: "adca9752065aab7f128688a8401f9224f61bbea447ad88ad0458640ac03f6b80"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/2026/07/14/microsoft-and-adobe-patch-tuesday-july-2026-security-update-review"
published_at: "2026-07-14T21:23:09+00:00"
first_seen_at: "2026-07-20T03:31:07.801512+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:a65b752086c2bf7cc4317e3ba99de54a382f0366b2a9b3cf5ebe274d3c43001f"
---

# Microsoft and Adobe Patch Tuesday, July 2026 Security Update Review

#### Table of Contents


- Microsoft Patch Tuesday forJuly2026
- Adobe Patch for July 2026
- Zero-day Vulnerabilities Patched inJulyPatch Tuesday Edition
- Critical Severity Vulnerabilities Patched inJulyPatch Tuesday Edition
- Other Microsoft Vulnerability Highlights
- Microsoft Release Summary
- Discover and Prioritize Vulnerabilities inVulnerability Management, Detection & Response (VMDR)
- Rapid Response withTruRisk Eliminate
- EVALUATE Vendor-Suggested Mitigation with Policy Audit (PA)
- Qualys Monthly Webinar Series


Microsoft’s July 2026 Patch Tuesday delivers security updates for a broad range of products and services, including several vulnerabilities that pose significant risks to enterprise environments. As attackers continue to target unpatched systems, the timely deployment of these updates remains one of the most effective defenses against exploitation. This blog provides an overview of the month’s key security fixes, highlights the most critical vulnerabilities, and offers guidance for prioritizing patch deployment.


## Microsoft Patch Tuesday for July 2026


This month’s release addresses **a whopping 570** vulnerabilities, including **57** critical and **510** important-severity **** vulnerabilities.


In this month’s updates, Microsoft has addressed three zero-day vulnerabilities, with **two** exploited in attacks and **one** publicly disclosed.


There were also a massive **468** vulnerabilities in Microsoft Edge/Chromium this month. Of the 468 Edge CVEs, **360** were issued by the Chrome CNA (upstream Chromium fixes rolled into Edge), and the rest are Microsoft-issued CVEs for the Microsoft Edge (Chromium-based) and Microsoft Edge for Android products.


Microsoft Patch Tuesday, July edition, includes updates for vulnerabilities in Windows Media, Windows HTTP.sys, Windows Hyper-V, Windows NTFS, Windows BitLocker, Windows Bluetooth Port Driver, Windows Bluetooth Service, Microsoft Copilot, Microsoft Defender, Microsoft Exchange Server, and more.


This month’s release includes fixes for several high-severity issues that could potentially enable remote code execution, privilege escalation, or denial-of-service attacks. As always, timely patch deployment is crucial to reduce exposure and ensure systems remain resilient against exploitation attempts.


The July 2026 Microsoft vulnerabilities are classified as follows:


**Vulnerability Category** **Quantity** **Severities**


Spoofing Vulnerability 16 Critical: 1
Important: 15


Denial of Service Vulnerability 35 Important: 35


Elevation of Privilege Vulnerability 254 Critical: 7
Important: 247


Information Disclosure Vulnerability 102 Important: 102


Remote Code Execution Vulnerability 145 Critical: 48
Important: 97


Security Feature Bypass Vulnerability 17 Critical: 1
Important: 16


## Adobe Patch for July 2026


Adobe has released **12** security advisories addressing **89** vulnerabilities across Adobe Animate, Adobe ColdFusion, Adobe Bridge, Content Credentials SDK, Adobe Illustrator, Adobe After Effects, Adobe Creative Cloud Desktop Application, Adobe Premiere Pro, Adobe Experience Manager, Adobe Commerce, Adobe Media Encoder, and Adobe Audition. **63** of these vulnerabilities are rated critical. Successful exploitation of these vulnerabilities may lead to privilege escalation, Security feature bypass, arbitrary file system read, application denial-of-service, and arbitrary code execution.


## **Zero-day Vulnerabilities Patched in July Patch Tuesday Edition**


### **[CVE-2026-56155: Active Directory Federation Services Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2026-56155)**


Insufficient granularity of access control in Active Directory Federation Services (AD FS) could allow an authenticated attacker to elevate privileges locally. Successful exploitation of the vulnerability may allow an attacker to gain administrator privileges.


### [CVE-2026-50661: Windows BitLocker Security Feature Bypass Vulnerability](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2026-50661)


A protection mechanism failure in Windows BitLocker may allow an unauthenticated attacker to bypass a security feature with a physical attack. A successful attacker could bypass the BitLocker Device Encryption feature on the system storage device to gain access to encrypted data.


CISA added the vulnerability to its[Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) , urging users to patch it before July 28, 2026.


### [CVE-2026-56164: Microsoft SharePoint Server Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2026-56164)


Missing authentication for a critical function in Microsoft Office SharePoint could allow an unauthenticated attacker to elevate privileges over a network.


Microsoft mentioned in the advisory that enabling the Antimalware Scan Interface (AMSI) on the server and setting the Request Body Scan mode to Full can help mitigate this flaw.


CISA added the vulnerability to its[Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) , urging users to patch it before July 17, 2026.


## Critical Severity Vulnerabilities Patched in July Patch Tuesday Edition


### **[CVE-2026-48561: Microsoft Copilot for Android (Edge) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-48561)**


A command injection flaw in Microsoft Copilot may allow an unauthenticated attacker to execute code over a network.


### **[CVE-2026-55944: Microsoft Dynamics NAV and Microsoft Dynamics 365 Business Central Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55944)**


Deserialization of untrusted data in Microsoft Dynamics NAV may allow an unauthenticated attacker to execute code over a network. An attacker could exploit the vulnerability by sending a specially crafted login request to an affected Dynamics NAV or Business Central server.


### **[CVE-2026-55045: Microsoft Office Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55045)**


An out-of-bounds read flaw in Microsoft Office allows an unauthenticated attacker to execute code locally.


### **[CVE-2026-42982: Windows Secure Kernel Mode Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-42982)**


Improper validation of input consistency in Windows Secure Kernel Mode may allow an authenticated attacker to elevate privileges locally.


### **[CVE-2026-56188: Windows Server Network driver Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56188)**


A race condition flaw in the Windows Server Network driver may allow an unauthenticated attacker to execute code over a network.


### **[CVE-2026-49164: Windows Active Directory Domain Services Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49164)**


A heap-based buffer overflow flaw in Active Directory Domain Services may allow an unauthenticated attacker to execute code over a network.


### **[CVE-2026-54992: Microsoft Message Queuing Queue Manager Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54992)**


A heap-based buffer overflow flaw in Windows Message Queuing Queue Manager may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-54982: Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54982)**


An integer underflow flaw in the Reliable Multicast Transport Driver (RMCAST) may allow an unauthenticated attacker to execute code over an adjacent network.


### **[CVE-2026-54999: Windows TCP/IP Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54999)**


A race condition flaw in Windows TCP/IP may allow an unauthenticated attacker to execute code over an adjacent network.


### **[CVE-2026-54995: Windows Reliable Multicast Transport Driver (RMCAST) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54995)**


A use-after-free in the Reliable Multicast Transport Driver (RMCAST) may allow an unauthenticated attacker to execute code over a network.


### **[CVE-2026-55008: Microsoft Exchange Server Spoofing Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55008)**


A cross-site scripting flaw in Microsoft Exchange Server may allow an unauthenticated attacker to perform network spoofing.


### **[CVE-2026-55011: Microsoft Defender Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55011)**


An integer underflow flaw in Microsoft Defender may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55012: Microsoft Defender Remote Code Execution Vulnerability](http://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55012)**


An integer overflow flaw in Microsoft Defender may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-54117](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54117) &[CVE-2026-54118](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54118) : Microsoft SQL Server Remote Code Execution Vulnerability**


A deserialization of untrusted data flaw in SQL Server may allow an authenticated attacker to execute code over a network.


### [CVE-2026-50694: Windows Secure Socket Tunneling Protocol (SSTP) Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50694) ****


A use-after-free flaw in Windows Secure Socket Tunneling Protocol may allow an unauthenticated attacker to execute code over a network.


### **[CVE-2026-54127: Windows Hyper-V Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54127)**


A use-after-free flaw in Windows Hyper-V may allow an unauthenticated attacker to elevate privileges locally.


### [CVE-2026-58644](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58644) **&**[CVE-2026-50522](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50522) **: Microsoft SharePoint Remote Code Execution Vulnerability**


A deserialization-of-untrusted-data flaw in Microsoft Office SharePoint may allow an unauthenticated attacker to execute code over a network.


CISA added the CVE-2026-50522 to its[Known Exploited Vulnerabilities Catalog,](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) urging users to patch before July 25, 2026.


### **[CVE-2026-58608: Windows Print Spooler Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58608)**


A race condition flaw in Windows Print Spooler Components may allow an authenticated attacker to execute code over a network.


### **[CVE-2026-56159](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56159) ,[CVE-2026-50370](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50370) ,[CVE-2026-48564,](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-48564) &[CVE-2026-50518](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50518) : DHCP Server Service Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw in Windows DHCP Server may allow an authenticated attacker to execute code over a network.


### **[CVE-2026-49796](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49796) &[CVE-2026-50380](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50380) : Windows GDI+ Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw in Windows GDI+ may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-50392: Windows Secure Kernel Mode Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50392)


A use-after-free flaw in Windows Secure Kernel Mode may allow an authenticated attacker to elevate privileges locally.


### **[CVE-2026-50382: DirectX Graphics Kernel Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50382)**


An untrusted pointer dereference flaw in Windows DirectX may allow an authenticated attacker to execute code locally.


### **[CVE-2026-50474: Windows Remote Desktop Client Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50474)**


A use-after-free flaw in the Remote Desktop Client may allow an unauthenticated attacker to elevate privileges over a network.


### **[CVE-2026-50444: Windows Server Update Service (WSUS) Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50444)**


Missing authentication for a critical function in Windows Server Update Service may allow an authenticated attacker to elevate privileges over a network.


### **[CVE-2026-50327](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50327) ,[CVE-2026-50655](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58542) &[CVE-2026-58542](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58542) : Windows Media Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw in Windows Media may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-50680: Windows Hyper-V Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50680)**


A heap-based buffer overflow flaw in Windows Hyper-V may allow an authenticated attacker to elevate privileges locally.


### **[CVE-2026-54121: Active Directory Certificate Services Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54121)**


An improper authorization flaw in Active Directory Certificate Services (AD CS) may allow an authenticated attacker to gain administrative privileges.


### [CVE-2026-54128: Windows DHCP Client Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-54128) ****


A use-after-free flaw in Windows DHCP Client may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55010: Minecraft Bedrock Dedicated Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55010)**


A heap-based buffer overflow flaw in Minecraft Bedrock Dedicated Server may allow an unauthenticated attacker to execute code over a network.


### **[CVE-2026-50314](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50314) ,[CVE-2026-50467](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50467) , &[CVE-2026-55018](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55018) : Microsoft Office Remote Code Execution Vulnerability**


A use-after-free flaw in Microsoft Office may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55022: Microsoft Office Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55022)**


A type confusion flaw in Microsoft Office may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55049](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55049) ,[CVE-2026-55129](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55129) ,[CVE-2026-55140](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55140) , &[CVE-2026-55056](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55056) : Microsoft Office Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw in Microsoft Office may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55033: Microsoft Word Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55033)**


An integer overflow or wraparound flaw in Microsoft Office Word may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55127: Microsoft Word Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55127)**


A heap-based buffer overflow in Microsoft Office Word may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-55132: Microsoft Word Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55132)


A double-free flaw in Microsoft Office Word may allow an unauthenticated attacker to execute code locally.


### **[CVE-2026-55040: Microsoft SharePoint Server Security Feature Bypass Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55040)**


A weak authentication in Microsoft Office SharePoint may allow an unauthenticated attacker to bypass a security feature over a network.


### **[CVE-2026-55043](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55043) ,[CVE-2026-55123](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55123) , &[CVE-2026-55120](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-55120) : Microsoft PowerPoint Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw in Microsoft Office PowerPoint may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-56189](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56189) **,**[CVE-2026-57090](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57090) **,**[CVE-2026-57094](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57094) **, &**[CVE-2026-57087](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57087) **: Microsoft Windows Media Foundation Remote Code Execution Vulnerability**


A heap-based buffer overflow flaw in Microsoft Windows Media Foundation may allow an unauthenticated attacker to execute code locally.


### [CVE-2026-57092: Microsoft Windows VMSwitch Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-57092) ****


A use-after-free in Windows VMSwitch may allow an authenticated attacker to elevate privileges over a network.


## Other Microsoft Vulnerability Highlights


- **CVE-2026-49170** is an elevation of privilege vulnerability in the Windows StateRepository API Server file. Upon successful exploitation, this vulnerability could allow an attacker to gain SYSTEM privileges.
- **CVE-2026-54986** is an elevation of privilege vulnerability in the Windows Win32k. An attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-54114** is an elevation of privilege vulnerability in the Windows Win32k. A use-after-free vulnerability could allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-58631** is a remote code execution vulnerability in Windows Admin Center (WAC). An improper authorization flaw may allow an authenticated attacker to execute code locally.
- **CVE-2026-49795, CVE-2026-50436, and CVE-2026-50688** are elevation of privilege vulnerabilities in the Windows Kernel. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-49798** is an elevation of privilege vulnerability in the Windows Kernel. An unauthenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-49805** is an elevation of privilege vulnerability in Win32k. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50351** is an elevation of privilege vulnerability in the Windows Audio Compression Manager (ACM). An improper access control flaw may allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-50297 & CVE-2026-50325** are elevation of privilege vulnerabilities in win32k. An improper access control could allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-50332** is an elevation of privilege vulnerability in the Windows Kernel. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50329** is an elevation of privilege vulnerability in the Microsoft DWM Core Library. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50343** is an elevation of privilege vulnerability in the Microsoft Install Service. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50390** is an elevation of privilege vulnerability in the Windows Kernel. A type confusion flaw may allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-50375** is an elevation of privilege vulnerability in the DirectX Graphics Kernel. A heap-based buffer overflow flaw may allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-50433** is an elevation of privilege vulnerability in Windows Media. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50423** is an elevation of privilege vulnerability in the Windows Kernel. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50387** is an elevation of privilege vulnerability in the Windows GDI. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50454** is an elevation of privilege vulnerability in the Windows User Interface Core. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50475** is an information disclosure vulnerability in the Windows Kernel. A buffer over-read flaw may allow an authenticated attacker to disclose information locally.
- **CVE-2026-50476** is an elevation of privilege vulnerability in the Windows Network Connections Service. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50489** is an elevation of privilege vulnerability in the Win32k. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50509** is an elevation of privilege vulnerability in the Wireless Wide Area Network Service (WwanSvc). An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-50667** is an elevation of privilege vulnerability in the Windows Common Log File System Driver. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-57091** is an elevation of privilege vulnerability in the Windows File History Service. An authenticated attacker may exploit the vulnerability to gain SYSTEM privileges.
- **CVE-2026-58531** is an elevation of privilege vulnerability in the Windows SMB. A race condition flaw in Windows SMB could allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-58536** is an elevation of privilege vulnerability in the Windows Cloud Files Mini Filter Driver. A use-after-free flaw may allow an authenticated attacker to elevate privileges locally.
- **CVE-2026-58596** is an elevation of privilege vulnerability in Microsoft Edge (Chromium-based). Successful exploitation of the vulnerability may allow an unauthenticated attacker to elevate privileges locally.


## Microsoft Release Summary


This month’s release notes cover multiple Microsoft product families and products/versions affected, including, but not limited to, .NET, .NET Core, .NET Framework, Active Directory Certificate Services (AD CS), Active Directory Domain Services, Active Directory Federation Services (AD FS), Age of Empires II: Definitive Edition Game, ASP.NET Core, Azure Active Directory, Azure CycleCloud, Azure Monitor Agent, Azure Spring Apps, Code Integrity DLL (ci.dll), Composite Image File System Driver, Content Delivery Manager, Desktop Window Manager, Extensible Storage Engine (ESENT), Github Copilot, GitHub Copilot and Visual Studio, GitHub Copilot and Visual Studio Code, HTTP/2, Microsoft 365 Copilot for iOS, Microsoft Bing App for IOS, Microsoft Copilot, Microsoft Defender, Microsoft Defender for Endpoint, Microsoft Dynamics NAV, Microsoft Exchange Server, Microsoft Fabric Data Warehouse, Microsoft Graphics Component, Microsoft Input Method Editor (IME), Microsoft Install Service, Microsoft NAT Helper Components (ipnathlp.dll), Microsoft Office, Microsoft Office Excel, Microsoft Office OneNote, Microsoft Office PowerPoint, Microsoft Office SharePoint, Microsoft Office Word, Microsoft Printer Drivers, Microsoft Surface, Microsoft Windows, Microsoft Windows App Store, Microsoft Windows Codecs Library, Microsoft Windows Media Foundation, Microsoft Windows Search Component, Microsoft Windows Speech, Microsoft XML, Microsoft XML Core Services, Minecraft Bedrock Dedicated Server, Outlook Copilot, Power BI, Quality Windows Audio/Video Experience (QWAVE) service, Reliable Multicast Transport Driver (RMCAST), Remote Desktop Client, Role: DNS Server, RPC Runtime, SQL Server, SQL Server ODBC driver, Universal Plug and Play (upnp.dll), Virtual Hard Disk (VHD) Miniport Driver, Visual Studio, Visual Studio Code, Window PC Manager, Windows Active Directory, Windows Admin Center, Windows Ancillary Function Driver for WinSock, Windows App Installer, Windows Application Model, Windows AppX Deployment Service, Windows Audio Compression Manager (ACM), Windows Audio Service, Windows Backup Engine, Windows BitLocker, Windows Bluetooth Port Driver, Windows Bluetooth Service, Windows Boot Loader, Windows Brokering File System, Windows Client-Side Caching (CSC) Service, Windows Clip Service, Windows Clipboard Server, Windows Clipboard User Service, Windows Cloud Files Mini Filter Driver, Windows Common Log File System Driver, Windows Connected User Experiences and Telemetry, Windows Container Isolation FS Filter Driver (unionfs.sys), Windows CryptoAPI, Windows Cryptographic Services, Windows Data.dll, Windows Devices Human Interface, Windows DHCP Client, Windows DHCP Server, Windows DirectX, Windows DNS, Windows Domain Controller, Windows DWM, Windows DWM Core Library, Windows Event Logging Service, Windows File Explorer, Windows File History Service, Windows Filtering Platform (WFP), Windows FTP Service, Windows GDI, Windows GDI+, Windows Graphics Kernel, Windows Group Policy, Windows HTTP.sys, Windows Hyper-V, Windows Image Acquisition, Windows Installer, Windows Internal System User Profile, Windows Internal Task Bar, Windows Internet Key Exchange (IKE) Protocol, Windows Kernel, Windows Kernel Mode Driver, Windows Kernel-Mode Drivers, Windows Key Guard, Windows Local Security Authority Subsystem Service (LSASS), Windows LUAFV, Windows Management Services, Windows Media, Windows Message Queuing, Windows Message Queuing Queue Manager, Windows MIDI Service Module, Windows Narrator Braille, Windows Netlogon, Windows Network Address Translation (NAT), Windows Network File System, Windows Network Policy Server SNMP, Windows Notification, Windows NTFS, Windows OLE, Windows Operating Systems, Windows Overlay Filter, Windows PowerShell, Windows Print Spooler Components, Windows Projected File System, Windows Push Notifications, Windows Quality of Service (QoS) Packet Scheduler, Windows RDP, Windows Redirected Drive Buffering, Windows Remote Access Connection Manager, Windows Remote Access Service Infrastructure, Windows Remote Desktop Protocol, Windows Remote Desktop Services, Windows Remote Help Defense, Windows Resilient File System (ReFS), Windows Routing and Remote Access Service (RRAS), Windows RPC API, Windows Runtime, Windows Schannel, Windows Secure Boot, Windows Secure Kernel Mode, Windows Secure Socket Tunneling Protocol (SSTP), Windows Sensor Data Service, Windows Server, Windows Server Backup, Windows Server Network driver, Windows Server Update Service, Windows SMB, Windows SMB Server, Windows SMB Server Network Transport Driver (srvnet.sys), Windows Spaceport.sys, Windows StateRepository API, Windows Storage, Windows Storage Spaces Direct, Windows Subsystem for Linux, Windows System, Windows TCP/IP, Windows Telephony Service, Windows Terminal, Windows Trusted Runtime Interface Driver, Windows Unified Consent System, Windows Universal Disk Format File System Driver (UDFS), Windows USB Audio Class driver (usbaudio.sys), Windows USB Driver, Windows USB Hub Driver, Windows USB Print Driver, Windows USB Video Driver, Windows User Interface Core, Windows Virtual Filtering Platform (VFP), Windows VMSwitch, Windows WalletService, Windows Web Proxy Auto-Discovery Protocol (WPAD), Windows WebView, Windows Win32K, Windows Win32K – GRFX, Windows Wireless Networking, and Windows Wireless Wide Area Network Service.


## Discover and Prioritize Vulnerabilities in[Vulnerability Management, Detection & Response (VMDR)](https://www.qualys.com/apps/vulnerability-management-detection-response/)


Qualys VMDR automatically detects new Patch Tuesday vulnerabilities using continuous updates to its Knowledgebase (KB).


You can see all your impacted hosts by these vulnerabilities using the following QQL query:


```text
vulnerabilities.vulnerability: ( qid: 110529 or qid: 110530 or qid: 387867 or qid: 387868 or qid: 50148 or qid: 92416 or qid: 92417 or qid: 92418 or qid: 92419 or qid: 92420 or qid: 92421 or qid: 92422 or qid: 92423 or qid: 92424 or qid: 92425 or qid: 92426 or qid: 92427 or qid: 92428 or qid: 92430 or qid: 92431 )
```


## Rapid Response with[TruRisk™ Eliminate](https://www.qualys.com/apps/trurisk-eliminate/)


### Patch to the Latest Version


VMDR rapidly remediates Windows hosts by deploying the most relevant and applicable per-technology version patches. You can simply select respective QIDs in the Patch Catalog and filter on the “Missing” patches to identify and deploy the applicable, available patches with one click.


The following QQL will return the missing patches for this Patch Tuesday:


```text
( qid: 110529 or qid: 110530 or qid: 387867 or qid: 387868 or qid: 50148 or qid: 92416 or qid: 92417 or qid: 92418 or qid: 92419 or qid: 92420 or qid: 92421 or qid: 92422 or qid: 92423 or qid: 92424 or qid: 92425 or qid: 92426 or qid: 92427 or qid: 92428 or qid: 92430 or qid: 92431 )
```


### Mitigation: Reducing Risk Until Remediation


Not every team can patch immediately due to operational challenges. TruRisk™ Eliminate enables security teams to apply mitigation controls that immediately lower exposure and reduce the Qualys Detection Score (QDS).


As a first set of our mitigant signature set, we have Qualys-created mitigations for the following 629 vulnerabilities: CVE-2026-41109, CVE-2026-47282, CVE-2026-14084, CVE-2026-14083, CVE-2026-14074, CVE-2026-14082, CVE-2026-14080, CVE-2026-14081, CVE-2026-14079, CVE-2026-14078, CVE-2026-14077, CVE-2026-14076, CVE-2026-14073, CVE-2026-14071, CVE-2026-14072, CVE-2026-14070, CVE-2026-14069, CVE-2026-14026, CVE-2026-14068, CVE-2026-14065, CVE-2026-14064, CVE-2026-14085, CVE-2026-14086, CVE-2026-14089, CVE-2026-14110, CVE-2026-14109, CVE-2026-14108, CVE-2026-14107, CVE-2026-14091, CVE-2026-14106, CVE-2026-14105, CVE-2026-14098, CVE-2026-14103, CVE-2026-14087, CVE-2026-14104, CVE-2026-14100, CVE-2026-14097, CVE-2026-14094, CVE-2026-14093, CVE-2026-14095, CVE-2026-14092, CVE-2026-14090, CVE-2026-14088, CVE-2026-14063, CVE-2026-14102, CVE-2026-14061, CVE-2026-14058, CVE-2026-14062, CVE-2026-14035, CVE-2026-14033, CVE-2026-14034, CVE-2026-14030, CVE-2026-14032, CVE-2026-14031, CVE-2026-14027, CVE-2026-14024, CVE-2026-14025, CVE-2026-14023, CVE-2026-14022, CVE-2026-14021, CVE-2026-14020, CVE-2026-14019, CVE-2026-14017, CVE-2026-14016, CVE-2026-14018, CVE-2026-14015, CVE-2026-14014, CVE-2026-14036, CVE-2026-14038, CVE-2026-14039, CVE-2026-14037, CVE-2026-14060, CVE-2026-14111, CVE-2026-14057, CVE-2026-14054, CVE-2026-14056, CVE-2026-14053, CVE-2026-14055, CVE-2026-14052, CVE-2026-14051, CVE-2026-14059, CVE-2026-14050, CVE-2026-14048, CVE-2026-14047, CVE-2026-14046, CVE-2026-14045, CVE-2026-14044, CVE-2026-14043, CVE-2026-14041, CVE-2026-14042, CVE-2026-14040, CVE-2026-14049, CVE-2026-14113, CVE-2026-14118, CVE-2026-14112, CVE-2026-57974, CVE-2026-57975, CVE-2026-57977, CVE-2026-57993, CVE-2026-13835, CVE-2026-13837, CVE-2026-13838, CVE-2026-58282, CVE-2026-13839, CVE-2026-58283, CVE-2026-58284, CVE-2026-58285, CVE-2026-13840, CVE-2026-58286, CVE-2026-58287, CVE-2026-58288, CVE-2026-58289, CVE-2026-13836, CVE-2026-13832, CVE-2026-13954, CVE-2026-13811, CVE-2026-45489, CVE-2026-13815, CVE-2026-13818, CVE-2026-13814, CVE-2026-13817, CVE-2026-13821, CVE-2026-13820, CVE-2026-13823, CVE-2026-13841, CVE-2026-13824, CVE-2026-13829, CVE-2026-13830, CVE-2026-13810, CVE-2026-13806, CVE-2026-13800, CVE-2026-13831, CVE-2026-13799, CVE-2026-13834, CVE-2026-13796, CVE-2026-13828, CVE-2026-14115, CVE-2026-58290, CVE-2026-58292, CVE-2026-14142, CVE-2026-14140, CVE-2026-14139, CVE-2026-14134, CVE-2026-14132, CVE-2026-14131, CVE-2026-14133, CVE-2026-14130, CVE-2026-14129, CVE-2026-14127, CVE-2026-14122, CVE-2026-14125, CVE-2026-14124, CVE-2026-14121, CVE-2026-14116, CVE-2026-14120, CVE-2026-14119, CVE-2026-14013, CVE-2026-14117, CVE-2026-14135, CVE-2026-14138, CVE-2026-14144, CVE-2026-14143, CVE-2026-13844, CVE-2026-58293, CVE-2026-58294, CVE-2026-58295, CVE-2026-58298, CVE-2026-13774, CVE-2026-13961, CVE-2026-14156, CVE-2026-58291, CVE-2026-14154, CVE-2026-14155, CVE-2026-14151, CVE-2026-14149, CVE-2026-14150, CVE-2026-14152, CVE-2026-14147, CVE-2026-14148, CVE-2026-14146, CVE-2026-14145, CVE-2026-14153, CVE-2026-14012, CVE-2026-14141, CVE-2026-14010, CVE-2026-13861, CVE-2026-13864, CVE-2026-13860, CVE-2026-13854, CVE-2026-13859, CVE-2026-13858, CVE-2026-13865, CVE-2026-13857, CVE-2026-13853, CVE-2026-13849, CVE-2026-13848, CVE-2026-13804, CVE-2026-13803, CVE-2026-13802, CVE-2026-13855, CVE-2026-13801, CVE-2026-13867, CVE-2026-13871, CVE-2026-13891, CVE-2026-13890, CVE-2026-13884, CVE-2026-13888, CVE-2026-13886, CVE-2026-13883, CVE-2026-13869, CVE-2026-13881, CVE-2026-13879, CVE-2026-13875, CVE-2026-13877, CVE-2026-13876, CVE-2026-13874, CVE-2026-13873, CVE-2026-13882, CVE-2026-13798, CVE-2026-13797, CVE-2026-13794, CVE-2026-57991, CVE-2026-57988, CVE-2026-57987, CVE-2026-57992, CVE-2026-57986, CVE-2026-57984, CVE-2026-57983, CVE-2026-57981, CVE-2026-45488, CVE-2026-55945, CVE-2026-14011, CVE-2026-57985, CVE-2026-58597, CVE-2026-58276, CVE-2026-58278, CVE-2026-13793, CVE-2026-13790, CVE-2026-13787, CVE-2026-13786, CVE-2026-13784, CVE-2026-13783, CVE-2026-13782, CVE-2026-13780, CVE-2026-13779, CVE-2026-13776, CVE-2026-13775, CVE-2026-56646, CVE-2026-56645, CVE-2026-58524, CVE-2026-13845, CVE-2026-13893, CVE-2026-13894, CVE-2026-13781, CVE-2026-13896, CVE-2026-13979, CVE-2026-13978, CVE-2026-13977, CVE-2026-13976, CVE-2026-13973, CVE-2026-13971, CVE-2026-13982, CVE-2026-13972, CVE-2026-13988, CVE-2026-13966, CVE-2026-13967, CVE-2026-13965, CVE-2026-13963, CVE-2026-13962, CVE-2026-13970, CVE-2026-13984, CVE-2026-13985, CVE-2026-13986, CVE-2026-13993, CVE-2026-13990, CVE-2026-13895, CVE-2026-13996, CVE-2026-13999, CVE-2026-14000, CVE-2026-14001, CVE-2026-13989, CVE-2026-14002, CVE-2026-14003, CVE-2026-14004, CVE-2026-14006, CVE-2026-14007, CVE-2026-14008, CVE-2026-14009, CVE-2026-13960, CVE-2026-13959, CVE-2026-13968, CVE-2026-13958, CVE-2026-13935, CVE-2026-13930, CVE-2026-13933, CVE-2026-13934, CVE-2026-13931, CVE-2026-13957, CVE-2026-13921, CVE-2026-13903, CVE-2026-13928, CVE-2026-13909, CVE-2026-13925, CVE-2026-13919, CVE-2026-13911, CVE-2026-13920, CVE-2026-13906, CVE-2026-13900, CVE-2026-13899, CVE-2026-13937, CVE-2026-13956, CVE-2026-13953, CVE-2026-13897, CVE-2026-13952, CVE-2026-13951, CVE-2026-13950, CVE-2026-13948, CVE-2026-13901, CVE-2026-13922, CVE-2026-13898, CVE-2026-13945, CVE-2026-13941, CVE-2026-13942, CVE-2026-13940, CVE-2026-13938, CVE-2026-13947, CVE-2026-13777, CVE-2026-13778, CVE-2026-13785, CVE-2026-13788, CVE-2026-13791, CVE-2026-13792, CVE-2026-13795, CVE-2026-13805, CVE-2026-13807, CVE-2026-13808, CVE-2026-13809, CVE-2026-13812, CVE-2026-13813, CVE-2026-13816, CVE-2026-13819, CVE-2026-13822, CVE-2026-13825, CVE-2026-13826, CVE-2026-13827, CVE-2026-13833, CVE-2026-13842, CVE-2026-13843, CVE-2026-13846, CVE-2026-13847, CVE-2026-13850, CVE-2026-13851, CVE-2026-13852, CVE-2026-13856, CVE-2026-13862, CVE-2026-13863, CVE-2026-13866, CVE-2026-13868, CVE-2026-13870, CVE-2026-13872, CVE-2026-13878, CVE-2026-13880, CVE-2026-13885, CVE-2026-13887, CVE-2026-13889, CVE-2026-13892, CVE-2026-13902, CVE-2026-13904, CVE-2026-13905, CVE-2026-13907, CVE-2026-13908, CVE-2026-13910, CVE-2026-13912, CVE-2026-13913, CVE-2026-13914, CVE-2026-13915, CVE-2026-13916, CVE-2026-13917, CVE-2026-13918, CVE-2026-13923, CVE-2026-13924, CVE-2026-13926, CVE-2026-13927, CVE-2026-13929, CVE-2026-13932, CVE-2026-13936, CVE-2026-13939, CVE-2026-13943, CVE-2026-13944, CVE-2026-13946, CVE-2026-13949, CVE-2026-13955, CVE-2026-13964, CVE-2026-13969, CVE-2026-13974, CVE-2026-13975, CVE-2026-13980, CVE-2026-13981, CVE-2026-13983, CVE-2026-13987, CVE-2026-13991, CVE-2026-13992, CVE-2026-13994, CVE-2026-13995, CVE-2026-13997, CVE-2026-13998, CVE-2026-14005, CVE-2026-14028, CVE-2026-14066, CVE-2026-14067, CVE-2026-14075, CVE-2026-14096, CVE-2026-14099, CVE-2026-14101, CVE-2026-14114, CVE-2026-14123, CVE-2026-14126, CVE-2026-14128, CVE-2026-14136, CVE-2026-14137, CVE-2026-14382, CVE-2026-14385, CVE-2026-14386, CVE-2026-14388, CVE-2026-14390, CVE-2026-14391, CVE-2026-14392, CVE-2026-14393, CVE-2026-14394, CVE-2026-14395, CVE-2026-14396, CVE-2026-14397, CVE-2026-14398, CVE-2026-14399, CVE-2026-14400, CVE-2026-14401, CVE-2026-14402, CVE-2026-14403, CVE-2026-14404, CVE-2026-14405, CVE-2026-14406, CVE-2026-14407, CVE-2026-14408, CVE-2026-14409, CVE-2026-14410, CVE-2026-14411, CVE-2026-14412, CVE-2026-14413, CVE-2026-14414, CVE-2026-14415, CVE-2026-14416, CVE-2026-14417, CVE-2026-14418, CVE-2026-14419, CVE-2026-14420, CVE-2026-14421, CVE-2026-14422, CVE-2026-14423, CVE-2026-14424, CVE-2026-14425, CVE-2026-14426, CVE-2026-14427, CVE-2026-14428, CVE-2026-14429, CVE-2026-14430, CVE-2026-14431, CVE-2026-14432, CVE-2026-58281, CVE-2026-55005, CVE-2026-55006, CVE-2026-55008, CVE-2026-55009, CVE-2026-55049, CVE-2026-55017, CVE-2026-50467, CVE-2026-55129, CVE-2026-55035, CVE-2026-50314, CVE-2026-55057, CVE-2026-55045, CVE-2026-55056, CVE-2026-55042, CVE-2026-55139, CVE-2026-50301, CVE-2026-56195, CVE-2026-56192, CVE-2026-50665, CVE-2026-47290, CVE-2026-55140, CVE-2026-55047, CVE-2026-55023, CVE-2026-56193, CVE-2026-55022, CVE-2026-55027, CVE-2026-55028, CVE-2026-55125, CVE-2026-55018, CVE-2026-55026, CVE-2026-55039, CVE-2026-55122, CVE-2026-55024, CVE-2026-58618, CVE-2026-50408, CVE-2026-48580, CVE-2026-55053, CVE-2026-54131, CVE-2026-55898, CVE-2026-47642, CVE-2026-55947, CVE-2026-55949, CVE-2026-56156, CVE-2026-55025, CVE-2026-55046, CVE-2026-55137, CVE-2026-55131, CVE-2026-55058, CVE-2026-55029, CVE-2026-55041, CVE-2026-55138, CVE-2026-55136, CVE-2026-55141, CVE-2026-55948, CVE-2026-55899, CVE-2026-50675, CVE-2026-55048, CVE-2026-55036, CVE-2026-55054, CVE-2026-55037, CVE-2026-55044, CVE-2026-55031, CVE-2026-50678, CVE-2026-54988, CVE-2026-55133, CVE-2026-55043, CVE-2026-55120, CVE-2026-55123, CVE-2026-55034, CVE-2026-55020, CVE-2026-55019, CVE-2026-55021, CVE-2026-55126, CVE-2026-55052, CVE-2026-55030, CVE-2026-55040, CVE-2026-55016, CVE-2026-56157, CVE-2026-55135, CVE-2026-55051, CVE-2026-54108, CVE-2026-56164, CVE-2026-58277, CVE-2026-50522, CVE-2026-55132, CVE-2026-55142, CVE-2026-55134, CVE-2026-55055, CVE-2026-55032, CVE-2026-55124, CVE-2026-55050, CVE-2026-55038, CVE-2026-55130, CVE-2026-55033, CVE-2026-55127, CVE-2026-55128, CVE-2026-49171, CVE-2026-58528, CVE-2026-49794, CVE-2026-50453, CVE-2026-50330, CVE-2026-50474, CVE-2026-58594, CVE-2026-55003, CVE-2026-58539, CVE-2026-58546, CVE-2026-58535, CVE-2026-58533, CVE-2026-57982, CVE-2026-57979, CVE-2026-56647, CVE-2026-50426, CVE-2026-49169, CVE-2026-50295, CVE-2026-50465, CVE-2026-50495, CVE-2026-58547, CVE-2026-50455, CVE-2026-42975, CVE-2026-58613, CVE-2026-50374, CVE-2026-58536, CVE-2026-50437, CVE-2026-49787, CVE-2026-50420, CVE-2026-50680, CVE-2026-54127, CVE-2026-57092, CVE-2026-50432, CVE-2026-50447, CVE-2026-54992, CVE-2026-50439, CVE-2026-54982, CVE-2026-54995, CVE-2026-50334, CVE-2026-50363, CVE-2026-50339, CVE-2026-50430, CVE-2026-50383, CVE-2026-57085, CVE-2026-58608, CVE-2026-50499, CVE-2026-49166, CVE-2026-49792, CVE-2026-50362, CVE-2026-49793, CVE-2026-50357, CVE-2026-58530, CVE-2026-50318, CVE-2026-50407, CVE-2026-50441, CVE-2026-50668, CVE-2026-50451, CVE-2026-49791, CVE-2026-50365, CVE-2026-50444, CVE-2026-50328, CVE-2026-57973, CVE-2026-57968, CVE-2026-50669, CVE-2026-50421.


For vulnerabilities in Windows services with local or remote exploitation vectors, our mitigants modify configuration by altering registry keys and, where applicable, service policy files. These mitigations work for affected components such as the Resilient File System (ReFS), Remote Desktop Client, Remote Desktop Protocol (RDP), Print Spooler, DNS Server and Client, Message Queuing (MSMQ), Routing and Remote Access Service (RRAS), Windows Server Update Service (WSUS), Cloud Files Mini Filter Driver, USB Audio Class Driver, Push Notifications, and HTTP.sys, Hyper-V, and the Windows Subsystem for Linux (WSL2) kernel, as well as Microsoft Office applications (Excel, Word, PowerPoint, and OneNote), Microsoft SharePoint Server, Microsoft Exchange Server, Microsoft Edge (Chromium-based), and Visual Studio Code with GitHub Copilot.


Additionally, this mitigant set addresses remote code execution, elevation of privilege, information disclosure, denial-of-service, spoofing, tampering, and security feature bypass risks across these components.


Qualys TruRisk Mitigate product customers receive these scripts as part of the monthly Patch Tuesday signature set.


## EVALUATE Vendor-Suggested Mitigation with[Policy Audit (PA)](https://www.qualys.com/forms/policy-compliance)


With Qualys Policy Audit’s Out-of-the-Box Mitigation or Compensatory Controls, which reduce the risk of a vulnerability being exploited because the remediation (fix/patch) cannot be done now, these security controls are not recommended by any industry standards, such as CIS or DISA-STIG.


Qualys Policy Audit team releases these exclusive controls based on Vendor-suggested Mitigation/Workaround.


**Mitigation** refers to a setting, common configuration, or general best practice that exists in a default state and could reduce the severity of a vulnerability’s exploitation.


A **workaround** is a method, sometimes used temporarily, for achieving a task or goal when the usual or planned method isn’t working. Information technology often uses a workaround to overcome hardware, programming, or communication problems. Once a problem is fixed, a workaround is usually abandoned.


The following[Qualys Policy Audit Control IDs (CIDs) and System Defined Controls (SDC)](https://docs.qualys.com/en/vm/latest/about_qualys_vm_vmdr.htm) have been updated to support Microsoft-recommended mitigation(s) for this Patch Tuesday:


### **CVE-2026-56164: Microsoft SharePoint Server Elevation of Privilege Vulnerability**


This vulnerability has a CVSS: 3.1 5.3 / 4.9


Policy Audit Control IDs (CIDs):


- 30908 Status of the ‘Antimalware Scan Interface’ in SharePoint Server


The following QQL will return a posture assessment for the CIDs for this Patch Tuesday:


control.id: \[30908\]


The next Patch Tuesday is scheduled for August 11, and we will provide details and patch analysis then. Until next Patch Tuesday, stay safe and secure. Be sure to subscribe to the ‘This Month in Vulnerabilities and Patches’ webinar.’


## Qualys Monthly Webinar Series


The Qualys Research team hosts a monthly webinar series to help our existing customers leverage the seamless integration between[Qualys Vulnerability Management, Detection & Response (VMDR)](https://www.qualys.com/apps/vulnerability-management-detection-response/) , and[Qualys Patch Management](https://www.qualys.com/apps/patch-management/) . Combining these two solutions can reduce the median time to remediate critical vulnerabilities.


During the webcast, we will discuss this month’s high-impact vulnerabilities, including those highlighted in this month’s Patch Tuesday alert. We will walk you through the necessary steps to address the key vulnerabilities using Qualys VMDR and Qualys Patch Management.


---


### Join the webinar


### This Month in Vulnerabilities & Patches


[Register Now](https://www.brighttalk.com/webcast/11673/660897)
