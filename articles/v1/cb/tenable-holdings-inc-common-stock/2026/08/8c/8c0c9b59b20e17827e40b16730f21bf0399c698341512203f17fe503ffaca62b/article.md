---
schema_version: "1.0.0"
document_id: "8c0c9b59b20e17827e40b16730f21bf0399c698341512203f17fe503ffaca62b"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/microsofts-august-2026-patch-tuesday-addresses-398-cves-cve-2026-68820"
published_at: "2026-08-11T18:04:04+00:00"
first_seen_at: "2026-08-12T00:57:38.150018+00:00"
fetched_at: "2026-08-12T00:57:39.594857+00:00"
content_hash: "sha256:2db435fe41ae340b66ef14f89075a2e52f1948853a8313724a9de57bbbc850ba"
---

# August 2026 Microsoft Patch Tuesday | Tenable®

1. 42


Critical
2. 355


Important
3. 1


Moderate
4. 0


Low


**Microsoft addresses 398 CVEs in the eighth Patch Tuesday of 2026, with three zero-days, including one that was exploited in the wild.**


Microsoft patched 398 CVEs in its August 2026 Patch Tuesday release, with 42 rated critical, 355 rated as important and one rated as moderate. Our counts omitted two CVEs assigned by MITRE; CVE-2026-6726 and CVE-2026-6727.


This month’s update includes patches for:


- .NET
- .NET Core
- .NET Framework
- AMD Zen
- Active Directory Certificate Services (AD CS)
- Application Information Services
- Azure Active Directory
- Azure CycleCloud
- Azure Monitor Agent
- Azure Storage Explorer
- Capability Access Management Service (camsvc)
- Desktop Window Manager
- Dynamics Business Central
- GitHub Copilot and Visual Studio Code
- Microsoft Azure Attestation service and Device Health Attestation Service
- Microsoft COM for Windows
- Microsoft Defender for Endpoint
- Microsoft Digest Authentication
- Microsoft Dynamics 365 (on-premises)
- Microsoft Entra Connect Sync
- Microsoft Exchange Server
- Microsoft High Performance Computing (HPC) Pack
- Microsoft Identity Services
- Microsoft Local Security Authority Server (lsasrv)
- Microsoft Office
- Microsoft Office Access
- Microsoft Office Excel
- Microsoft Office Graphics Component
- Microsoft Office Outlook
- Microsoft Office PowerPoint
- Microsoft Office SharePoint
- Microsoft Office Word
- Microsoft OneDrive
- Microsoft PowerShell
- Microsoft PowerShell Core
- Microsoft QUIC
- Microsoft Remote Registry Service
- Microsoft Teams Mobile
- Microsoft Teams for Android
- Microsoft Windows Codecs Library
- Microsoft Windows Media Foundation
- Microsoft Windows Search Component
- Power BI
- RPC Runtime
- Reliable Multicast Transport Driver (RMCAST)
- Remote Desktop Client
- User-Mode Power Service (UMPS)
- Virtual Hard Disk (VHD) Miniport Driver
- Visual Studio Code
- Visual Studio Code - Python extension
- Visual Studio Code CoPilot Chat Extension
- Windows Accessibility Infrastructure (ATBroker.exe)
- Windows Active Directory
- Windows Ancillary Function Driver for WinSock
- Windows Autopilot
- Windows Backup Engine
- Windows Bind Filter Driver
- Windows Cloud Files Mini Filter Driver
- Windows Common Log File System Driver
- Windows Container Isolation FS Filter Driver (unionfs.sys)
- Windows Cross Device Service
- Windows DHCP Client
- Windows DHCP Server
- Windows DNS
- Windows DWM Core Library
- Windows Defender Firewall Service
- Windows Deployment Services
- Windows Device Association Service
- Windows Display Enhancement Service
- Windows Encrypting File System (EFS)
- Windows Event Logging Service
- Windows GDI
- Windows GDI+
- Windows Graphics Kernel
- Windows HTTP Protocol Stack
- Windows HTTP.sys
- Windows Hello
- Windows Hyper-V
- Windows Imaging Component
- Windows Installer
- Windows Kerberos
- Windows Kernel
- Windows Key Guard
- Windows LDAP - Lightweight Directory Access Protocol
- Windows LUAFV
- Windows License Manager
- Windows MIDI Service Module
- Windows Management Instrumentation
- Windows Management Services
- Windows Message Queuing
- Windows Modern Device Management (MDM)
- Windows NTFS
- Windows Narrator Braille
- Windows Network Address Translation (NAT)
- Windows Network Connection Broker
- Windows Network File System
- Windows Package Manager
- Windows Program Compatibility Assistant Service
- Windows Projected File System
- Windows Push Notifications
- Windows RPC API
- Windows Remote Access API
- Windows Remote Access Connection Manager
- Windows Remote Desktop Services
- Windows Remote Help
- Windows Remote Help Defense
- Windows Routing and Remote Access Service (RRAS)
- Windows SMB Client
- Windows SMB Server
- Windows Schannel
- Windows Secure Socket Tunneling Protocol (SSTP)
- Windows Sensor Data Service
- Windows Shell
- Windows Storage
- Windows Storage Port Driver
- Windows TCP/IP
- Windows Telephony Service
- Windows USB Driver
- Windows Universal Disk Format File System Driver (UDFS)
- Windows User Profile Service
- Windows Win32K
- Windows Wired AutoConfig Service
- Windows Work Folder Service
- Windows iSCSI Target Service
- Winlogon


Elevation of Privilege (EoP) vulnerabilities accounted for 40.7% of the vulnerabilities patched this month, followed by remote code execution (RCE) vulnerabilities at 27.1%.


Important


## CVE-2026-68820 | Windows Ancillary Function Driver for WinSock Elevation of Privilege Vulnerability


[CVE-2026-68820](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-68820) is an EoP vulnerability affecting Windows Ancillary Function Driver for WinSock. It received a CVSSv3 score of 7.0 and was rated as important. A local attacker could exploit this vulnerability to elevate to SYSTEM privileges. According to Microsoft, this vulnerability was exploited in the wild as a zero-day.


Two additional EoP vulnerabilities affecting this driver were patched this month.[CVE-2026-61348](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-61348) and[CVE-2026-70307](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-70307) also received CVSSv3 scores of 7.0, however no exploitation has been reported for these flaws. Both were assessed as "Exploitation More Likely" according to[Microsoft's Exploitability Index](https://www.microsoft.com/en-us/msrc/exploitability-index) .


Prior zero-days in this driver include[CVE-2025-32709](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2025-32709) in[May 2025](https://www.tenable.com/blog/microsofts-may-2025-patch-tuesday-addresses-71-cves-cve-2025-32701-cve-2025-32706) ,[CVE-2025-21418](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2025-21418) in[February 2025](https://www.tenable.com/blog/microsofts-february-2025-patch-tuesday-addresses-55-cves-cve-2025-21418-cve-2025-21391) , and[CVE-2024-38193](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2024-38193) in[August 2024](https://www.tenable.com/blog/microsofts-august-2024-patch-tuesday-addresses-88-cves) .


Important


## CVE-2026-62832 | Windows User Profile Service Elevation of Privilege Vulnerability


[CVE-2026-62832](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62832) is an elevation of privilege vulnerability affecting Windows User Profile Service. It received a CVSSv3 score of 7.8 and is rated as important. A local attacker could exploit this vulnerability to gain ADMINISTRATOR privileges. It was publicly disclosed prior to a patch being available and was assessed as “Exploitation More Likely.”


Historically, the Windows User Profile Service has received four total CVEs since January 2022. Prior zero-days in this family include[CVE-2022-21919](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2022-21919) in[January 2022](https://www.tenable.com/blog/microsofts-january-2022-patch-tuesday-addresses-97-cves-cve-2022-21907) and[CVE-2022-26904](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2022-26904) in[April 2022](https://www.tenable.com/blog/microsofts-april-2022-patch-tuesday-addresses-117-cves-cve-2022-24521) .


Important


## CVE-2026-72971 | Windows Container Isolation FS Filter Driver (unionfs.sys) Tampering Vulnerability


[CVE-2026-72971](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-72971) is a tampering vulnerability affecting the Windows Container Isolation FS Filter Driver (unionfs.sys). It received a CVSSv3 score of 5.5 and is rated as important. It was publicly disclosed prior to a patch being available. Successful exploitation would allow a local attacker to perform tampering. Despite being publicly disclosed, Microsoft assesses this vulnerability as “Exploitation Unlikely.”


Critical


## CVE-2026-62893 | Windows Deployment Services TFTP Server Remote Code Execution Vulnerability


[CVE-2026-62893](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62893) is a remote code execution vulnerability affecting Windows Deployment Services Trivial File Transfer Protocol (TFTP) Server. It received a CVSSv3 score of 9.8 and is rated as critical. It was assessed as "Exploitation More Likely." Successful exploitation of this flaw could occur when a remote, unauthenticated attacker sends crafted packets to a vulnerable service, resulting in code execution. It was reported to Microsoft by Nikolai Skliarenko of TrendAI Research.


Critical


## CVE-2026-62823 | Windows DHCP Server Remote Code Execution Vulnerability


[CVE-2026-62823](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62823) is a remote code execution vulnerability affecting Windows DHCP Server. It received a CVSSv3 score of 8.8 and is rated as critical. It was assessed as "Exploitation More Likely" according to Microsoft's Exploitability Index. Successful exploitation would allow a remote, unauthenticated attacker to execute code over an adjacent network by exploiting a heap-based buffer overflow flaw using a crafted packet.


13 additional Windows DHCP server vulnerabilities were patched this month, however these flaws were only rated as important. The flaws include eight information disclosure vulnerabilities with CVSSv3 scores of 6.5 ([CVE-2026-62714](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62714) ,[CVE-2026-62715](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62715) ,[CVE-2026-62716](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62716) ,[CVE-2026-62718](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62718) ,[CVE-2026-62720](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62720) ,[CVE-2026-62742](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62742) ,[CVE-2026-62745](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62745)[and](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62745)[CVE-2026-62814](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62814) ) and five EoP vulnerabilities with CVSSv3 scores of 7.8 ([CVE-2026-62761](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62761) ,[CVE-2026-62776](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62776) ,[CVE-2026-62803](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62803) ,[CVE-2026-62807](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62807) and[CVE-2026-62812](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62812) ).


Critical


## Multiple CVEs | Microsoft Office SharePoint Spoofing, Remote Code Execution, Elevation of Privilege, Information Disclosure and Tampering Vulnerabilities


This month's update includes patches for 29 CVEs affecting Microsoft Office SharePoint. Of the 29 CVEs, three were rated as critical and three were assessed as 'Exploitation More Likely.' A breakdown of the CVEs can be found in the table below:


**CVE** **Description** **CVSSv3** **Severity** **Exploitability Index**


[CVE-2026-70306](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-70306) Microsoft Office SharePoint Spoofing 9.3 Important Exploitation Less Likely


[CVE-2026-62827](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62827) Microsoft SharePoint Server Elevation of Privilege 8.8 Critical Exploitation Less Likely


[CVE-2026-65665](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-65665) Microsoft SharePoint Server Remote Code Execution 8.8 Critical Exploitation More Likely


[CVE-2026-64921](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64921) Microsoft SharePoint Server Elevation of Privilege 8.8 Critical Exploitation Less Likely


[CVE-2026-63514](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-63514) Microsoft SharePoint Server Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-64901](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64901) Microsoft SharePoint Server Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-65658](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-65658) Microsoft SharePoint Server Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-65663](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-65663) Microsoft SharePoint Server Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-66805](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-66805) Microsoft SharePoint Server Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-66808](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-66808) Microsoft SharePoint Server Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-70321](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-70321) Microsoft SharePoint Remote Code Execution 8.8 Important Exploitation Less Likely


[CVE-2026-70324](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-70324) Microsoft SharePoint Elevation of Privilege 8.8 Important Exploitation Less Likely


[CVE-2026-70326](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-70326) Microsoft SharePoint Server Elevation of Privilege 8.8 Important Exploitation Less Likely


[CVE-2026-63520](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-63520) Microsoft SharePoint Server Remote Code Execution 8.1 Important Exploitation More Likely


[CVE-2026-57105](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-57105) Microsoft Office SharePoint Spoofing 8.0 Important Exploitation Less Likely


[CVE-2026-70355](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-70355) Microsoft SharePoint Server Elevation of Privilege 7.3 Important Exploitation More Likely


[CVE-2026-58639](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-58639) Microsoft SharePoint Server Spoofing 6.5 Important Exploitation Less Likely


[CVE-2026-62837](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62837) Microsoft SharePoint Server Information Disclosure 6.5 Important Exploitation Less Likely


[CVE-2026-62839](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62839) Microsoft SharePoint Server Spoofing 6.5 Important Exploitation Less Likely


[CVE-2026-63512](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-63512) Microsoft SharePoint Server Tampering 6.5 Important Exploitation Less Likely


[CVE-2026-63516](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-63516) Microsoft SharePoint Server Spoofing 6.5 Important Exploitation Less Likely


[CVE-2026-65660](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-65660) Microsoft SharePoint Server Spoofing 6.5 Important Exploitation Less Likely


[CVE-2026-62829](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62829) Microsoft SharePoint Server Spoofing 4.6 Important Exploitation Less Likely


[CVE-2026-62917](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-62917) Microsoft SharePoint Server Spoofing 4.6 Important Exploitation Less Likely


[CVE-2026-64897](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64897) Microsoft SharePoint Server Spoofing 4.6 Important Exploitation Less Likely


[CVE-2026-64922](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64922) Microsoft SharePoint Server Spoofing 4.6 Important Exploitation Less Likely


[CVE-2026-64900](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64900) Microsoft SharePoint Server Spoofing 7.3 Important N/A


[CVE-2026-64902](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64902) Microsoft SharePoint Server Spoofing 4.6 Important N/A


[CVE-2026-64916](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2026-64916) Microsoft SharePoint Server Spoofing 4.6 Important Exploitation Unlikely


## Tenable Solutions


A list of all the plugins released for Microsoft's August 2026 Patch Tuesday update can be found[here](https://www.tenable.com/plugins/search?q=%22August+2026%22+AND+script_family%3A%28%22Windows%22+OR+%22MacOS+X+Local+Security+Checks%22+OR+%22Windows+%3A+Microsoft+Bulletins%22%29&sort=&page=1) . As always, we recommend patching systems as soon as possible and regularly scanning your environment to identify those systems yet to be patched.


For more specific guidance on best practices for vulnerability assessments, please refer to our blog post on[How to Perform Efficient Vulnerability Assessments with Tenable](https://www.tenable.com/blog/how-to-perform-efficient-vulnerability-assessments-with-tenable) .


### Get more information


- [Microsoft's August 2026 Security Updates](https://msrc.microsoft.com/update-guide/en-us/releaseNote/2026-Aug)
- [Tenable plugins for Microsoft August 2026 Patch Tuesday Security Updates](https://www.tenable.com/plugins/search?q=%22August+2026%22+AND+script_family%3A%28%22Windows%22+OR+%22MacOS+X+Local+Security+Checks%22+OR+%22Windows+%3A+Microsoft+Bulletins%22%29&sort=&page=1)


***Join***[Tenable's Research Special Operations (RSO) Team](https://connect.tenable.com/category/news-you-need/discussions/vulnerability-watch) ***on Tenable Connect for further discussions on the latest cyber threats.***


***Learn more about***[Tenable One](https://www.tenable.com/products/tenable-one) ***, the Exposure Management Platform for the modern attack surface.***
