---
schema_version: "1.0.0"
document_id: "d265339a52256f236f005b33a319088925b127998b6272519ea571c6f357c591"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-16232-critical-check-point-smartconsole-authentication-bypass-exploited-in-the-wild"
published_at: "2026-07-23T11:57:30+00:00"
first_seen_at: "2026-07-23T12:55:29.361431+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:4fe52a5ae2b99562e4f552691044df35489a4cc187f48b6a9d3479a9c081da47"
---

# CVE-2026-16232: Critical Check Point SmartConsole Authentication Bypass Exploited in the Wild

## Overview


On July 22, 2026, Check Point


[published a security advisory](https://blog.checkpoint.com/security/security-advisory-action-required-active-exploitation-of-check-point-smartconsole-authentication-bypass-cve-2026-16232/) for multiple vulnerabilities affecting Security Management, Multi-Domain Management, and firewall products. The most urgent of these is


[CVE-2026-16232](https://www.rapid7.com/db/vulnerabilities/cve-2026-16232/) , an authentication bypass in the SmartConsole login process classified as improper authentication (


[CWE-287](https://cwe.mitre.org/data/definitions/287.html) ). CVE-2026-16232 has been assigned a critical CVSS score of 9.1. The vulnerability allows an unauthenticated remote attacker to obtain an application login token and authenticate to the management server with full administrative privileges, enabling modification of security policies and configurations.


Check Point has confirmed that CVE-2026-16232 is being actively exploited in the wild, affecting what the vendor describes as a small number of customers. Remote exploitation requires network access to the Management Server IP address in environments that do not restrict Trusted Clients. On the same day as the advisory, CVE-2026-16232 was


[added](https://www.cisa.gov/known-exploited-vulnerabilities-catalog?field_cve=CVE-2026-16232) to the U.S. Cybersecurity and Infrastructure Security Agency's (CISA) list of known exploited vulnerabilities (KEV), with a remediation due date of July 25, 2026, giving organizations only three days to respond.


The advisory addresses three vulnerabilities in total:


**CVE**


**CVSS**


**Description**


**Affected Products**


**Exploitation Status**


CVE-2026-16232


Vendor: 9.3 (Critical)


CISA: 9.1 (Critical)


Authentication bypass via SmartConsole application token


Security Management, Multi-Domain Management


Exploited in the wild


CVE-2026-62144


Vendor: 9.3 (Critical)


CISA: 9.1 (Critical)


Management authentication bypass and privilege escalation


Security Management, Multi-Domain Management


No known exploitation


CVE-2026-62145


7.5 (High)


Local privilege escalation in GaiaOS WebUI


Firewall, Multi-Domain Management, Multi-Domain Log Server


No known exploitation


Compromise of a Security Management Server is particularly consequential because it sits at the top of the trust hierarchy. An attacker with administrative access can modify security policies across managed gateways, alter administrator permissions, manipulate VPN configurations, and potentially disable or tamper with logging and monitoring. According to Check Point's


[advisory](https://blog.checkpoint.com/security/security-advisory-action-required-active-exploitation-of-check-point-smartconsole-authentication-bypass-cve-2026-16232/) , the vulnerabilities were discovered during a routine internal review, with subsequent analysis revealing that CVE-2026-16232 had been exploited prior to the availability of a patch.


Check Point network security products have been targeted by multiple in-the-wild vulnerabilities over the past two years. In June 2026,


[CVE-2026-50751](https://www.rapid7.com/db/vulnerabilities/cve-2026-50751/) , a critical authentication bypass in Check Point Remote Access VPN, was exploited in the wild and added to the CISA KEV. In May 2024,


[CVE-2024-24919](https://www.rapid7.com/blog/post/2024/05/30/etr-cve-2024-24919-check-point-security-gateway-information-disclosure/) , a high-severity information disclosure vulnerability in Check Point Quantum Security Gateways, was also exploited in the wild. Organizations running affected Check Point management products should apply the available hotfixes on an emergency basis.


## Technical analysis


On July 28, 2026, Rapid7 Labs published a full root cause


[technical analysis](https://www.rapid7.com/blog/post/ra-check-point-smartconsole-authentication-bypass-technical-analysis-cve-2026-16232/) of CVE-2026-16232. Our analysis details the vulnerability and how an unauthenticated attacker can exploit the vulnerability to login to a vulnerable appliance via SmartConsole with full admin privileges.


## Mitigation guidance


Check Point released Jumbo Hotfixes on July 22, 2026, to remediate CVE-2026-16232, CVE-2026-62144, and CVE-2026-62145. Organizations running affected versions of Security Management or Multi-Domain Management should install the latest Jumbo Hotfix on an emergency basis, without waiting for a regular patch cycle to occur.


The following versions are affected by CVE-2026-16232:


-


R82.10


: fixed in Jumbo Hotfix Take 36 and later


-


R82


: fixed in Jumbo Hotfix Take 118 and later


-


R81.20


: fixed in Jumbo Hotfix Take 158 and later


-


R81.10


,


R81


,


R80.30


,


R80.20


,


R80.10


,


R80


, and


R77.30


: no fix specified


CVE-2026-62144 and CVE-2026-62145 affect the same release families (


R81.10


,


R81.20


,


R82


,


R82.10


) per the vendor advisory, with older versions also impacted.


Smart-1 Cloud customers are already protected according to Check Point. For on-premises deployments where the hotfix cannot be applied immediately, Check Point recommends the following steps to reduce exposure:


-


Restrict Trusted Clients (GUI clients) to trusted IP addresses or subnets


-


Protect Management access with a firewall and restrict access to trusted IP addresses


-


Verify that implied rules for control connections are enabled


These mitigations reduce the attack surface, but they do not address the underlying vulnerability. Installing the Jumbo Hotfix remains the priority.


Rapid7 strongly recommends investigating for signs of compromise even after applying the hotfix, particularly in environments where the Management Server has been accessible from the internet. Organizations should review administrator, SmartConsole, API, and application token activity, and search logs for the published indicators of compromise listed below.


For the latest mitigation guidance, please refer to the vendor


[advisory](https://support.checkpoint.com/results/sk/sk185169) .


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-16232, CVE-2026-62144, CVE-2026-62145 with authenticated vulnerability checks available in the 24 July content release.


## Indicators of compromise


Check Point has published the following IP addresses associated with observed exploitation of CVE-2026-16232:


-


151.241.99\[.\]207


-


151.241.99\[.\]233


-


158.62.198\[.\]182


-


192.142.10\[.\]99


-


139.28.37\[.\]250


-


194.213.18\[.\]137


Per the vendor, the presence of these indicators should prompt investigation, but the absence of these addresses does not confirm that an environment was unaffected.


## Updates


- **July 23, 2026**


: Initial publication.


- **July 24, 2026:**


Updated to reflect availability of vulnerability checks.


- **July 28, 2026:**


Added a Technical analysis section for the new Rapid7 Analysis.
