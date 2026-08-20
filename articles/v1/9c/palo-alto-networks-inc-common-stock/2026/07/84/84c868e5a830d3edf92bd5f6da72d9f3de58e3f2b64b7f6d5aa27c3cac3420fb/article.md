---
schema_version: "1.0.0"
document_id: "84c868e5a830d3edf92bd5f6da72d9f3de58e3f2b64b7f6d5aa27c3cac3420fb"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/the-gentlemen-ransomware/"
published_at: "2026-07-10T22:00:39+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:668555f9d80d4c5572735f1833f8a3dd2e4a15d33309e2abae283f338fafb0da"
---

# No Manners Here: The Ruthless Rise of The Gentlemen Ransomware

## Executive Summary


The Gentlemen (aka[Storm-2697](https://www.microsoft.com/en-us/security/blog/2026/05/28/the-gentlemen-ransomware-dissecting-a-self-propagating-go-encryptor/) ) is a Ransomware-as-a-Service (RaaS) program active since at least July 2025.[Public reporting](https://catalyst.prodaft.com/public/report/inside-the-phantom-mantis-operation/overview#paragraph-1000%7C0) indicates that the operators were likely active months earlier as an affiliate (known as ArmCorp) of Qilin RaaS, which Unit 42 tracks as Spikey Scorpius. Their ransomware variants are[written in both C and Go programming languages](https://securelist.com/the-gentlemen-raas/120447/) , enabling the threat actors to spread their encryptors across different operating systems and virtual infrastructure. Figure 1 below illustrates the desktop wallpaper used by the ransomware after deployment.


Figure 1. Image of The Gentlemen ransomware’s wallpaper. Source: Krebs on Security.


[Additional public reporting](https://www.halcyon.ai/ransomware-research-reports/threat-assessment-the-gentlemen-ransomware-group) revealed that the operators (roughly 20 of them) likely morphed from a private entity into a RaaS model on or about September 2025. While traditional RaaS models typically offer affiliates a 70% to 80% cut of paid ransoms, The Gentlemen offer an unprecedented 90% payout.


## Background


Unit 42[and other security researchers](https://www.trendmicro.com/en_us/research/25/i/unmasking-the-gentlemen-ransomware.html) have observed The Gentlemen’s usage of a wide variety of initial access techniques similar to other RaaS operators since their inception, including the exploitation of vulnerabilities in edge devices (firewalls, VPNs), brute force attacks, obtaining leaked and/or stolen credentials and collaborating with initial access brokers (IABs).


More recently, researchers have identified The Gentlemen’s usage of a[custom Go-based backdoor](https://securelist.com/the-gentlemen-raas/120447/) , an EDR killer framework dubbed “[GentleKiller](https://www.welivesecurity.com/en/eset-research/killing-me-gently-inside-gentlemens-edr-killer-framework/) ” and the suspected usage of[an unspecified zero-day vulnerability exploit](https://expel.com/blog/not-very-gentlemanly-analyzing-a-zero-day-exploit-used-by-the-gentlemen-ransomware-to-disable-targets-edrs/) to amplify their defense evasion capabilities.


In May 2026, The Gentlemen announced a partnership with HasanBroker's BreachForums as a means to recruit affiliates, penetration testers and IABs. Figure 2 illustrates this announcement.


Figure 2. Image of partnership announcement between BreachForums and The Gentlemen. Source: Gurucul.


[Additional information](https://ransom-isac.org/blog/the-gentlemen-leak-analysis/) about The Gentlemen and their operational structure has emerged in recent months, following the leak of an internal database by an alleged insider in May 2026.


## Data Leak Site Insights


One of the most alarming trends observed thus far in 2026 by Unit 42[and other security researchers](https://research.checkpoint.com/2026/thus-spoke-the-gentlemen/) is the sheer increase in volume of total victims claimed by The Gentlemen in comparison to 2025. Through July 7,[one reputable source](https://ransomware.live/groupstats/thegentlemen) had counted a total of 580 victims claimed by The Gentlemen across 77 countries since their inception. Of those 580 victims, 103 operated within the manufacturing industry, a commonly targeted sector given the need for organizations to maintain operational uptime.


Figure 3 below represents the total number of victims claimed by The Gentlemen in 2025 compared to both Qilin and Akira, tracked by Unit 42 as Howling Scorpius, which led all RaaS programs in victims claimed last year.


Figure 3. Chart depicting total victims claimed by prominent RaaS programs in 2025. Source: Unit 42.


In comparison to the above statistics, Figure 4 below represents the total number of victims claimed by The Gentlemen thus far in 2026 (through July 3) compared to both Qilin and Akira.


Figure 4. Chart depicting total victims claimed by prominent RaaS programs in 2026. Source: Unit 42.


When comparing the last six months of 2025 to the first six months of 2026, the number of victims claimed by The Gentlemen increased by slightly more than 6x. What makes this even more concerning is that these threat actors were only active for the last four months of 2025.


Figure 5 below further illustrates the victims claimed by The Gentlemen per month since August 2025, one month prior to the official launch of their RaaS model. June 2026 represented their highest number of claimed victims to date with 117, just shy of a 4x increase from January 2026.


Figure 5. Chart depicting victims claimed by The Gentlemen per month since August 2025. Source: Ransomware.live.


## Conclusion


While legacy big-game hunting RaaS programs like Qilin and Akira continue to drive high volumes of victims by sticking to their established playbooks, The Gentlemen has solidified itself as the second most active RaaS program of 2026 in terms of victims. The combination of a lucrative affiliate payout structure to recruit affiliates, alongside the use of custom tooling across different phases of their attack lifecycle, make The Gentlemen a formidable threat for enterprise organizations to reckon with in the near and mid term future.


## Recommendations


**Initial Access:**


- Immediately scope for and patch the following vulnerabilities known to be exploited:


- CVE-2024-55591 ([Fortinet's FortiOS and FortiProxy](https://nvd.nist.gov/vuln/detail/cve-2024-55591) )
- CVE-2025-32433 ([Erlang/OTP SSH server](https://nvd.nist.gov/vuln/detail/CVE-2025-32433) )
- CVE-2025-33073 ([Windows SMB Client)](https://nvd.nist.gov/vuln/detail/CVE-2025-33073)
- CVE-2025-55182 ([React2Shell](https://nvd.nist.gov/vuln/detail/CVE-2025-55182) )


- Establish and maintain robust visibility into internet-facing systems and applications such as firewalls, VPNs and remote access gateways
- Audit for indicators of prior exploitation of edge devices and internet-facing RDP endpoints
- Establish strong security requirements for third-party dependencies and vendors, and monitor for breaches of any third-party tools or platforms


**Execution:**


- Create immediate, high-severity SIEM alerts for the creation, deletion or execution of any scheduled task matching the string *gentlemen**


**Privilege Escalation:**


- CVE-2025-7771 ( ThrottleStop.sys


driver)


**Defense Impairment:**


- Enable EDR Tamper Protection and monitor for the unexpected loading of unsigned or known vulnerable drivers
- Implement behavioral alerts for systems executing wevtutil


to clear Security/System logs


**Credential Access:**


- Deploy phishing-resistance multi-factor authentication (MFA) on all systems
- Regularly audit and rotate credentials


**Discovery:**


- Monitor for internal usage of tools such as Advanced IP Scanner, which the threat actors frequently use for internal network reconnaissance and mapping


**Lateral Movement:**


- Enforce strict SMB signing, disable SMBv1 completely, and restrict lateral network movement between internal segments to contain the self-propagation mechanism
- Ensure SSH is turned off on ESXi hosts by default and only enabled temporarily for explicit maintenance windows
- Treat your virtualized environment as tier-0 infrastructure and restrict ESXi management interfaces to a dedicated, isolated management VLAN


**Command and Control:**


- Monitor for anomalous outbound traffic over non-standard ports or traffic matching known SystemBC communication signatures


**Impact:**


- Maintain and validate offline backup and recovery capabilities
- Implement behavioral alerts for systems using vssadmin


and wmic


to delete Volume Shadow Copies
