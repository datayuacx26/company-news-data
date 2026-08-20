---
schema_version: "1.0.0"
document_id: "f515318bd5d659e09bdf08ea748bdbd1031fe2fd9f81ab8167f1aeb728161d55"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/cve-2026-15409-cve-2026-15410-sonicwall-sma-1000-zero-day-vulnerabilities-exploited-in-the"
published_at: "2026-07-15T17:14:09+00:00"
first_seen_at: "2026-07-24T03:36:59.179818+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:bb632f1dcf03e2d67f444ef08b1a2969a0e3730069ecf2df44262a743ed171ed"
---

# SonicWall CVE-2026-15409 and CVE-2026-15410 zero-day exploited | Tenable®

**SonicWall patched two recently exploited zero-day vulnerabilities in its SMA 1000 Series secure remote access appliances which may have been chained for unauthenticated remote code execution.**


## Key takeaways


1. CVE-2026-15409 and CVE-2026-15410 are a pair of exploited vulnerabilities that may have been chained together to allow for code execution on SonicWall SMA1000 series appliances.


2. Zero-day exploitation of these vulnerabilities has been observed and confirmed by SonicWall.


3. Patches and indicators of compromise are available and urgent patching is recommended.


## Background


SonicWall's Secure Mobile Access (SMA) 1000 Series appliances are enterprise-grade SSL VPN gateways which serve as the front door to organizational networks. The SMA series models sit at the edge of the network, internet-facing by design. Because SMA 1000 appliances aggregate remote access credentials and sit directly on the internet, they represent high-value targets for attackers. A compromise at the appliance level can yield administrator credentials, VPN session tokens, and detailed knowledge of the internal network architecture sitting behind the gateway.


On July 14, SonicWall[disclosed two vulnerabilities](https://psirt.global.sonicwall.com/vuln-detail/SNWLID-2026-0008) that are being exploited together in the wild:


**CVE** **Description** **CVSSv3**


CVE-2026-15409 SonicWall SMA 1000 server-side request forgery (SSRF) vulnerability 10


CVE-2026-15410 SonicWall SMA 1000 remote code execution vulnerability (RCE) 7.2


While the advisory does not specify if they were exploited in tandem, together they form a fully remote, unauthenticated path to arbitrary OS command execution on affected appliances.


## Analysis


[CVE-2026-15409](https://www.tenable.com/cve/CVE-2026-15409) is a SSRF vulnerability affecting the SMA 1000 Workplace interface. This flaw allows a remote, unauthenticated attacker to make network requests to locations of the attacker's choosing. In practice, SSRF on an internet-facing appliance can serve as a pivot, allowing an attacker to probe internal services, relay authentication material, or reach the AMC in a way that bypasses normal access controls.


[CVE-2026-15410](https://www.tenable.com/cve/CVE-2026-15410) is a code injection vulnerability in the Appliance Management Console (AMC). The AMC is the administrative interface used to configure the appliance, manage users, set access policies, and monitor sessions. While this flaw does require the user to be authenticated, the potential chaining of these vulnerabilities makes the exploitation path possible without authentication.


These flaws have been exploited in the wild as zero-days. While SonicWall has not provided any details on attribution of which threat actors may be behind the attacks, several SonicWall vulnerabilities have been targeted in the past, including the exploitation of zero-days.


**Historical exploitation of SonicWall vulnerabilities**


SonicWall products have been a frequent target for attackers over the years. Specifically, the SMA product line has been targeted in the past[by ransomware groups](https://cloud.google.com/blog/topics/threat-intelligence/unc2447-sombrat-and-fivehands-ransomware-sophisticated-financial-threat/) , as well as being featured in the[Top Routinely Exploited Vulnerabilities list](https://www.tenable.com/blog/aa23-215a-2022s-top-routinely-exploited-vulnerabilities) co-authored by multiple United States and International Agencies. Last year, a surge in ransomware activity was tied to the[exploitation of SonicWall Gen 7 Firewalls](https://www.tenable.com/blog/faq-about-sonicwall-gen-7-firewall-ransomware-activity-akira) , prompting warnings from multiple security vendors.


Given the historical exploitation of SonicWall devices, we put together the following list of known SMA vulnerabilities that have been exploited in the wild:


**CVE** **Description** **Tenable Blog Links** **Year**


[CVE-2019-7481](https://www.tenable.com/cve/CVE-2019-7481) SonicWall SMA100 SQL Injection Vulnerability[1](https://www.tenable.com/blog/sonicwall-urges-users-to-patch-several-vulnerabilities-in-secure-mobile-access-products-cve) 2019


[CVE-2019-7483](https://www.tenable.com/cve/CVE-2019-7483) SonicWall SMA100 Directory Traversal Vulnerability - 2019


[CVE-2021-20016](https://www.tenable.com/cve/CVE-2021-20016) SonicWall SSLVPN SMA100 SQL Injection Vulnerability[1](https://www.tenable.com/blog/cve-2021-20016-zero-day-vulnerability-in-sonicwall-secure-mobile-access-sma-exploited) ,[2](https://www.tenable.com/blog/sonicwall-urges-users-to-patch-several-vulnerabilities-in-secure-mobile-access-products-cve) ,[3](https://www.tenable.com/blog/behind-the-scenes-how-we-picked-2021s-top-vulnerabilities-and-what-we-left-out) ,[4](https://www.tenable.com/blog/securing-critical-infrastructure-what-weve-learned-from-recent-incidents) ,[5](https://www.tenable.com/blog/aa23-215a-2022s-top-routinely-exploited-vulnerabilities) 2021


[CVE-2021-20038](https://www.tenable.com/cve/CVE-2021-20038) SonicWall SMA100 Stack-based Buffer Overflow Vulnerability[1](https://www.tenable.com/blog/sonicwall-urges-users-to-patch-several-vulnerabilities-in-secure-mobile-access-products-cve) ,[2](https://www.tenable.com/blog/south-korean-and-american-agencies-release-joint-advisory-on-north-korean-ransomware) ,[3](https://www.tenable.com/blog/aa23-215a-2022s-top-routinely-exploited-vulnerabilities) 2021


[CVE-2025-23006](https://www.tenable.com/cve/CVE-2025-23006) SonicWall SMA 1000 Deserialization of Untrusted Data Vulnerability[1](https://www.tenable.com/blog/cve-2025-23006-sonicwall-secure-mobile-access-sma-1000-zero-day-reportedly-exploited) 2025


[CVE-2024-40766](https://www.tenable.com/cve/CVE-2024-40766) SonicWall SonicOS Improper Access Control Vulnerability[1](https://www.tenable.com/blog/faq-about-sonicwall-gen-7-firewall-ransomware-activity-akira) 2025


[CVE-2025-40602](https://www.tenable.com/cve/CVE-2025-40602) SonicWall SMA 1000 Privilege Escalation Vulnerability[1](https://www.tenable.com/blog/cve-2025-40602-sonicwall-secure-mobile-access-sma-1000-zero-day-exploited) 2025


Both CVE-2026-15409 and CVE-2026-15410 have been added to the Cybersecurity and Infrastructure Security Agency (CISA)[Known Exploited Vulnerabilities (KEV) Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) with a remediation date of Friday, July 17, despite only being added on July 14. Given the urgency surrounding these vulnerabilities and the historical exploitation of these devices, immediate patching is recommended.


## Proof of concept


At the time this blog was published, no proof-of-concept (PoC) code had been published for CVE-2026-15409 or CVE-2026-15410. If and when a public PoC exploit becomes available for these vulnerabilities, we anticipate an increase in exploitation as attackers will attempt to leverage these flaws as part of their attacks.


## Solution


SonicWall has released patches to address this vulnerability in SMA1000 models 6210, 7210 and 8200v as outlined in the table below:


**Affected Version** **Fixed Version**


12.4.3-03245 12.4.3-03453 and later versions


12.4.3-03387 12.4.3-03453 and later versions


12.4.3-03434 12.4.3-03453 and later versions


12.5.0-02283 12.5.0-02835 and later versions


12.5.0-02624 12.5.0-02835 and later versions


12.5.0-02800 12.5.0-02835 and later versions


While the advisory does not provide any workarounds, it does include indicators of compromise (IoCs) for threat hunters to determine if any exploitation has impacted their devices. We recommend reviewing the advisory for the most up to date IoCs.


## Identifying affected systems


A list of Tenable plugins for these vulnerabilities can be found on the individual CVE pages for[CVE-2026-15409](https://www.tenable.com/cve/CVE-2026-15409/plugins) and[CVE-2026-15410](https://www.tenable.com/cve/CVE-2026-15410/plugins) as they’re released. This link will display all available plugins for these vulnerabilities, including upcoming plugins in our[Plugins Pipeline](https://www.tenable.com/plugins/pipeline) .


Tenable Attack Surface Management customers are able to identify these assets using a filtered search for SonicWall devices:


### Get more information


- [SonicWall Security Advisory SNWLID-2026-0008](https://psirt.global.sonicwall.com/vuln-detail/SNWLID-2026-0008)


***Join***[Tenable's Research Special Operations (RSO) Team](https://connect.tenable.com/category/news-you-need/discussions/vulnerability-watch) ***on Tenable Connect for further discussions on the latest cyber threats.***


***Learn more about***[Tenable One](https://www.tenable.com/products/tenable-one) ***, the Exposure Management Platform for the modern attack surface.***
