---
schema_version: "1.0.0"
document_id: "02bb63c4641a12555f84b088521a1e725809b60881de58ecaada58a96693a825"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/cve-2026-32201-cve-2026-45659-cve-2026-56164-faq-sharepoint-server-exploitation"
published_at: "2026-07-16T16:00:26+00:00"
first_seen_at: "2026-07-24T03:36:59.179818+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:9505b8d54e4d4c967b5a4ec9a8b1e46c157ff33db9e9d0757e37990fa741d816"
---

# SharePoint CVEs FAQ: CVE-2026-56164, CVE-2026-32201, CVE-2026-45659 | Tenable®

**Four Microsoft SharePoint Server vulnerabilities are under active exploitation, prompting CISA to issue a hardening alert. An additional high-severity flaw recently patched adds pressure for organizations running on-premises deployments.**


## Key Takeaways


1. CISA confirmed active exploitation of three on-premises SharePoint Server vulnerabilities (CVE-2026-32201, CVE-2026-45659, CVE-2026-56164), used to gain unauthorized access, establish remote code execution, steal IIS machine keys and deploy malware for persistence.
2. Two additional SharePoint Server vulnerabilities disclosed on July 14, 2026, CVE-2026-55040 and CVE-2026-58644, were not yet observed exploited at the time of publication, but Microsoft has flagged CVE-2026-58644 as exploited on July 15.
3. Microsoft released patches for all five vulnerabilities and Microsoft Defender Antivirus detection signatures are available to identify exploitation activity for three of the actively exploited flaws.


## Background


Tenable's Research Special Operations (RSO) team has compiled this blog to answer Frequently Asked Questions (FAQ) regarding active exploitation of Microsoft SharePoint Server vulnerabilities.


## FAQ


**When did CISA issue an alert about SharePoint Server exploitation?**


On July 14, 2026, the U.S. Cybersecurity and Infrastructure Security Agency (CISA)[published an alert](https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations) confirming active exploitation of three on-premises SharePoint Server vulnerabilities: CVE-2026-32201, CVE-2026-45659, and CVE-2026-56164. The alert noted that these flaws had been used to gain unauthorized access to SharePoint deployments across all supported on-premises versions and flagged two additional high-risk vulnerabilities, CVE-2026-55040 and CVE-2026-58644, as not yet exploited but warranting immediate patching. However in an update to the security advisory on July 15, Microsoft confirmed CVE-2026-58644 has been exploited in the wild.


**What vulnerabilities are covered in this alert?**


Five Microsoft SharePoint Server vulnerabilities are covered in CISA’s alert: Four with confirmed active exploitation and one newly disclosed high-severity flaw that Microsoft assesses as “Exploitation More Likely” according to[Microsoft's Exploitability Index](https://www.microsoft.com/en-us/msrc/exploitability-index) . All five affect all supported on-premises SharePoint Server versions: Subscription Edition, 2019, and 2016.


**CVE** **Description** **CVSSv3** **VPR**


[CVE-2026-32201](https://www.tenable.com/cve/CVE-2026-32201) Microsoft SharePoint Server Spoofing Vulnerability 6.5 7.2


[CVE-2026-45659](https://www.tenable.com/cve/CVE-2026-45659) Microsoft SharePoint Remote Code Execution Vulnerability 8.8 9.4


[CVE-2026-56164](https://www.tenable.com/cve/CVE-2026-56164) Microsoft SharePoint Server Elevation of Privilege Vulnerability 9.8 - NVD5.3 - Microsoft 9.5


[CVE-2026-55040](https://www.tenable.com/cve/CVE-2026-55040) Microsoft SharePoint Server Security Feature Bypass Vulnerability 9.1 7.3


[CVE-2026-58644](https://www.tenable.com/cve/CVE-2026-58644) Microsoft SharePoint Server Remote Code Execution Vulnerability 9.8 7.9


**Please note: Tenable’s*[Vulnerability Priority Rating (VPR)](https://www.tenable.com/blog/what-is-vpr-and-how-is-it-different-from-cvss) *scores are calculated nightly. This blog post was published on July 16 and reflects VPR at that time.*


**What do the actively exploited vulnerabilities do?**


CVE-2026-32201 is a spoofing flaw rooted in improper input validation. An unauthenticated remote attacker can exploit it over a network without user interaction.


CVE-2026-45659 is a remote code execution (RCE) vulnerability involving deserialization of untrusted data. An authenticated attacker can exploit this flaw in order to execute code on an affected SharePoint server.


CVE-2026-56164 is an elevation of privilege vulnerability. An unauthenticated attacker can exploit it remotely to elevate privileges on a SharePoint Server.


CVE-2026-58644 is a RCE vulnerability that can be abused by an authenticated attacker with at least Site Owner permissions. Successful exploitation would allow the attacker to achieve code execution by exploiting a deserialization of untrusted data flaw.


**What post-exploitation activity has been observed?**


According to CISA, attackers have combined CVE-2026-32201, CVE-2026-45659 and CVE-2026-56164 to gain entry to on-premises SharePoint Server instances, then pursued several post-exploitation objectives: extracting IIS machine keys, leveraging deserialization techniques to establish persistence, and deploying malware. CISA notes that stolen machine keys can be used to forge requests enabling further exploitation of the server. The advisory notes that key rotation alone is not a complete remediation step, without first removing any key-harvesting artifacts.


**What is CVE-2026-55040, the vulnerability that has not yet been exploited?**


CVE-2026-55040 is a security feature bypass rooted in weak authentication. It was assigned a CVSSv3 score of 9.1 and rated as critical. An unauthenticated attacker can exploit it over the network, without user interaction allowing the attacker to impact both confidentiality and integrity.


Both CVE-2026-55040 and CVE-2026-58644 were disclosed on July 14, 2026, as part of[Microsoft's July 2026 Patch Tuesday](https://www.tenable.com/blog/microsofts-july-2026-patch-tuesday-addresses-569-cves-cve-2026-56155-cve-2026-56164) release. As of July 16, 2026, CVE-2026-55040 has not been observed being exploited in the wild.


**What is the history of exploitation for Microsoft SharePoint Server?**


Microsoft SharePoint Server has been a recurring target for threat actors across multiple years.[CISA's Known Exploited Vulnerabilities (KEV) catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) contains 12 SharePoint-related entries, including seven currently known to be used in ransomware campaigns. The table below outlines prior SharePoint CVEs added to the KEV catalog.


**CVE** **Date Added to KEV** **Known Ransomware** **Tenable Coverage**


CVE-2026-56164 2026-07-14 Unknown[July 2026 Patch Tuesday](https://www.tenable.com/blog/microsofts-july-2026-patch-tuesday-addresses-569-cves-cve-2026-56155-cve-2026-56164)


CVE-2026-45659 2026-07-01 Unknown --


CVE-2026-32201 2026-04-14 Unknown[April 2026 Patch Tuesday](https://www.tenable.com/blog/microsofts-april-2026-patch-tuesday-addresses-163-cves-cve-2026-32201)


CVE-2026-20963 2026-03-18 Unknown --


CVE-2025-49706 2025-07-22 Yes[July 2025 Patch Tuesday](https://www.tenable.com/blog/microsofts-july-2025-patch-tuesday-addresses-128-cves-cve-2025-49719)


CVE-2025-49704 2025-07-22 Yes[July 2025 Patch Tuesday](https://www.tenable.com/blog/microsofts-july-2025-patch-tuesday-addresses-128-cves-cve-2025-49719)


CVE-2025-53770 2025-07-20 Yes[FAQ Blog](https://www.tenable.com/blog/cve-2025-53770-frequently-asked-questions-about-zero-day-sharepoint-vulnerability-exploitation)


CVE-2024-38094 2024-10-22 Yes --


CVE-2023-24955 2024-03-26 Yes[Exploit Chain Released for Microsoft SharePoint Server Vulnerabilities](https://www.tenable.com/blog/cve-2023-29357-cve-2023-24955-exploit-chain-released-for-microsoft-sharepoint-server)


CVE-2023-29357 2024-01-10 Yes[June 2023 Patch Tuesday](https://www.tenable.com/blog/microsofts-june-2023-patch-tuesday-addresses-70-cves-cve-2023-29357)[Exploit Chain Released for Microsoft SharePoint Server Vulnerabilities](https://www.tenable.com/blog/cve-2023-29357-cve-2023-24955-exploit-chain-released-for-microsoft-sharepoint-server)


CVE-2019-0604 2021-11-03 Yes[Critical Microsoft SharePoint Remote Code Execution Flaw Actively Exploited](https://www.tenable.com/blog/cve-2019-0604-critical-microsoft-sharepoint-remote-code-execution-flaw-actively-exploited)


CVE-2020-1147 2021-11-03 Unknown --


**Which threat actors are exploiting these SharePoint vulnerabilities?**


As of July 16, 2026, neither CISA nor Microsoft has attributed the active exploitation of CVE-2026-32201, CVE-2026-45659, CVE-2026-56164 or CVE-2026-58644 to specific threat actors or groups.


**Is there a proof-of-concept available for any of these vulnerabilities?**


As of July 16, 2026, no public proofs-of-concept are available for any of the five vulnerabilities covered in this FAQ.


**Are patches and mitigations available?**


Microsoft released patches for all five vulnerabilities. The table below lists the fixed build numbers for each affected SharePoint version.


**CVE** **SharePoint Enterprise Server 2016** **SharePoint Server 2019** **SharePoint Server Subscription Edition**


CVE-2026-32201 16.0.5548.1003 16.0.10417.20114 16.0.19725.20210


CVE-2026-45659 16.0.5552.1002 16.0.10417.20128 16.0.19725.20280


CVE-2026-56164 16.0.5561.1001 16.0.10417.20175 16.0.19725.20434


CVE-2026-55040 16.0.5561.1001 16.0.10417.20175 16.0.19725.20434


CVE-2026-58644 16.0.5556.1005 16.0.10417.20153 16.0.19725.20384


The CISA advisory and several of the Microsoft security advisories recommend enabling[AMSI integration](https://learn.microsoft.com/en-us/sharepoint/security-for-sharepoint-server/configure-amsi-integration) on SharePoint and IIS worker processes and setting the Request Body Scan mode to Full to allow detection of malicious POST payloads. CISA's hardening guidance also advises against direct internet exposure of SharePoint Servers and recommends restricting external access to SharePoint Central Administration.


**Are there indicators of compromise?**


Yes. CISA and Microsoft have published AMSI and Microsoft Defender Antivirus detection signatures for the three actively exploited vulnerabilities.


**Signature** **Type** **Scope**


Exploit:Script/SuspSignoutReqBody.A AMSI Request body scanning; SharePoint Server Subscription Edition only; Microsoft reports active exploitation attempts have been blocked


Exploit:Script/ToolPaneAuthBypass.A AMSI Request header scanning; SharePoint Server 2016, 2019, and Subscription Edition


Exploit:Script/ToolPaneAuthBypass.C AMSI RCE coverage; SharePoint Server 2016, 2019, and Subscription Edition


Backdoor:MSIL/LeakFang.A!dha MDAV Post-exploitation activity; IIS machine key access


CISA recommends reviewing telemetry for anomalous requests, suspicious SharePoint worker-process activity, webshells and machine-key access activity.


**Has Tenable Research classified these vulnerabilities as part of Vulnerability Watch?**


Yes. Tenable Research has classified CVE-2026-32201, CVE-2026-45659 and CVE-2026-56164 and CVE-2026-58644 as part of[Vulnerability Watch](https://www.tenable.com/blog/reducing-remediation-time-remains-a-challenge-how-tenable-vulnerability-watch-can-help) . The designation applies to flaws with confirmed in-the-wild exploitation and the potential for widespread impact across affected organizations. We are actively tracking developments related to this activity and will update this post as new information becomes available.


**Has Tenable released product coverage for these vulnerabilities?**


A list of Tenable plugins can be found on the individual CVE pages:


- [CVE-2026-32201](https://www.tenable.com/cve/CVE-2026-32201/plugins)
- [CVE-2026-45659](https://www.tenable.com/cve/CVE-2026-45659/plugins)
- [CVE-2026-56164](https://www.tenable.com/cve/CVE-2026-56164/plugins)
- [CVE-2026-55040](https://www.tenable.com/cve/CVE-2026-55040/plugins)
- [CVE-2026-58644](https://www.tenable.com/cve/CVE-2026-58644/plugins)


This link will display all available plugins for these vulnerabilities, including upcoming plugins in our[Plugins Pipeline](https://www.tenable.com/plugins/pipeline) .


Additionally, Tenable Attack Surface Management customers can identify external-facing assets by leveraging the built-in subscription labeled **Microsoft Sharepoint Server - v1** .


### Get more information


- [CISA: CISA Urges SharePoint Hardening After New Exploitations](https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations)
- [Tenable: Microsoft's July 2026 Patch Tuesday Addresses 569 CVEs (CVE-2026-56155, CVE-2026-56164)](https://www.tenable.com/blog/microsofts-july-2026-patch-tuesday-addresses-569-cves-cve-2026-56155-cve-2026-56164)


***Join***[Tenable's Research Special Operations (RSO) Team](https://connect.tenable.com/category/news-you-need/discussions/vulnerability-watch) ***on Tenable Connect for further discussions on the latest cyber threats.***


***Learn more about***[Tenable One](https://www.tenable.com/products/tenable-one) ***, the Exposure Management Platform for the modern attack surface.***
