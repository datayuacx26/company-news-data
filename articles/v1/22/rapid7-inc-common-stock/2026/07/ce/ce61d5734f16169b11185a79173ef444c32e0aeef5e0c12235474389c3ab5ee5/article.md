---
schema_version: "1.0.0"
document_id: "ce61d5734f16169b11185a79173ef444c32e0aeef5e0c12235474389c3ab5ee5"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-63030-wp2shell-a-critical-remote-code-execution-vulnerability-in-wordpress-core"
published_at: "2026-07-17T22:23:03+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:c58d3f9a9369f3a2e13b35a8d03ac3b26ba1aa8f590abe9093cdf02f90786db9"
---

# CVE-2026-63030: wp2shell a Critical Remote Code Execution Vulnerability in WordPress Core

## Overview


On July 17, 2026, a GitHub Security Advisory was


[published](https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-ff9f-jf42-662q) for


[CVE-2026-63030](https://nvd.nist.gov/vuln/detail/CVE-2026-63030) , a critical unauthenticated remote code execution vulnerability affecting


[WordPress Core](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/) . While the official GitHub security advisory classifies the severity as Critical, the vulnerability has currently been assigned a CVSS score of 7.5. WordPress is one of the most widely deployed content management systems, making vulnerabilities in its core software potentially significant for organizations operating public-facing websites. The vulnerability


[reportedly allows](https://slcyber.io/research-center/wp2shell-pre-authentication-rce-in-wordpress-core/) an unauthenticated attacker to execute code via the WordPress REST API batch endpoint, potentially resulting in complete compromise of the website and its underlying data. No valid account or user interaction is required.


According to the


[advisory](https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-ff9f-jf42-662q) , the vulnerability affects WordPress versions 6.9.0 through 6.9.4 and versions 7.0.0 through 7.0.1. The issue is fixed in WordPress 6.9.5 and 7.0.2. A fix is also included in WordPress 7.1 Beta 2.


Cloudflare


[reported](https://blog.cloudflare.com/wordpress-vulnerabilities/) that the vulnerable code path can be reached when a persistent object cache is not in use. Searchlight Cyber, whose researchers identified the vulnerability, stated that it can be exploited remotely against a default WordPress installation without requiring additional plugins.


***Update, July 22:***


****


Since initial publication, Searchlight Cyber has


[published full technical details](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/) of the exploit chain, multiple public proof-of-concept exploits have surfaced, and both CVEs were added to CISA's Known Exploited Vulnerabilities (KEV) catalog on July 21 (


[CVE-2026-63030](https://nvd.nist.gov/vuln/detail/CVE-2026-63030) ,


[CVE-2026-60137](https://nvd.nist.gov/vuln/detail/CVE-2026-60137) ), confirming active exploitation.


## Technical overview


CVE-2026-63030 is a logic flaw in the WordPress REST API batch processor (


/wp-json/batch/v1


). The batch API performs validation and execution in separate loops. When


wp_parse_url()


fails on a sub-request path, the error is pushed to the validation array but not the matches array. This desynchronizes the arrays, causing every subsequent request to dispatch under the wrong handler.


CVE-2026-60137 is a SQL injection in the


author__not_in


parameter of the posts endpoint. The parameter is interpolated directly into raw SQL when provided as a scalar string. Parameter validation normally prevents this, but the batch API desynchronization allows bypass. A recursive batch call bypasses GET restrictions, yielding pre-authentication UNION-based SQL injection.


When both vulnerabilities are present, escalation to RCE chains WordPress internals to create an administrator account. The attacker logs in and uploads a malicious plugin for code execution. Neither vulnerability alone is sufficient for unauthenticated code execution. Searchlight Cyber's


[full technical writeup](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/) details each step of the chain.


## Mitigation guidance


Organizations operating affected WordPress installations should prioritize upgrading immediately. Applying the WordPress-provided update is the most effective way to remediate CVE-2026-63030.


Affected and fixed versions include:


WordPress branch


Affected versions


Fixed version


Earlier than 6.9


Not affected by CVE-2026-63030


No action required for this CVE


6.9


6.9.0 through 6.9.4


6.9.5


7.0


7.0.0 through 7.0.1


7.0.2


7.1 beta


Affected beta versions were not fully specified


7.1 Beta 2


WordPress maintainers


[stated](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/) they are forcing updates for affected installations with automatic updates enabled. Administrators should nevertheless verify that each internet-facing WordPress website has successfully upgraded to WordPress 6.9.5, 7.0.2, or another fixed release appropriate for its branch.


As a temporary mitigation, organizations that cannot immediately update can block the


/wp-json/batch/v1


endpoint (or


?rest_route=/batch/v1


) at a web application firewall, or disable anonymous REST API access using a plugin.


Given confirmed exploitation in the wild, Rapid7 strongly recommends investigating for signs of compromise even after patching. Defenders should review HTTP access logs for anomalous batch endpoint requests and check for unfamiliar plugins or PHP files.


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-63030 with unauthenticated vulnerability checks available in the July 20th, 2026 content release.


## Updates


- **July 17, 2026:**


Initial publication.


- **July 22, 2026:**


Updated post to reflect public reporting of exploitation in the wild; added technical overview following Searchlight Cyber's full disclosure; added temporary WAF mitigation guidance; updated Overview to note CISA KEV addition (July 21).
