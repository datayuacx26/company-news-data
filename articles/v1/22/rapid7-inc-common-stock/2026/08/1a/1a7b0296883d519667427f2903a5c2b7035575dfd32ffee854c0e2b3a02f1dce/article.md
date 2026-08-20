---
schema_version: "1.0.0"
document_id: "1a7b0296883d519667427f2903a5c2b7035575dfd32ffee854c0e2b3a02f1dce"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-19490-critical-vulnerability-affecting-citrix-netscaler-adc-and-netscaler-gateway"
published_at: "2026-08-19T16:46:06+00:00"
first_seen_at: "2026-08-19T17:52:28.512960+00:00"
fetched_at: "2026-08-19T17:52:31.933777+00:00"
content_hash: "sha256:8589cdde57552f1029bc6304a99ab0fe6a9122a55d381ca40f4f0d0b00c2a939"
---

# CVE-2026-19490: Critical Vulnerability Affecting Citrix NetScaler ADC and NetScaler Gateway

## Overview


On August 19, 2026, a


[security advisory](https://support.citrix.com/support-home/kbsearch/article?articleNumber=CTX696939) was published for


[CVE-2026-19490](https://nvd.nist.gov/vuln/detail/CVE-2026-19490) , a critical authentication bypass vulnerability affecting Citrix NetScaler ADC and NetScaler Gateway. The vulnerability carries a CVSS v4.0 base score of 9.3 and can be exploited remotely by an unauthenticated attacker over the network without user interaction or elevated privileges.


NetScaler ADC and NetScaler Gateway are widely deployed enterprise networking products commonly positioned at or near the network perimeter. NetScaler ADC provides application delivery, traffic management, load balancing, SSL/TLS offloading, and application security capabilities, while NetScaler Gateway provides secure remote access and VPN functionality. Because these systems are frequently deployed in enterprise DMZs and exposed to the public internet, authentication bypass vulnerabilities affecting Citrix products are nearly always exploited by threat actors.


CVE-2026-19490


[affects](https://support.citrix.com/support-home/kbsearch/article?articleNumber=CTX696939) the following systems:


-


**NetScaler ADC and NetScaler Gateway 14.1:**


Versions prior to 14.1-73.32


-


**NetScaler ADC and NetScaler Gateway 13.1:**


Versions prior to 13.1-63.21


-


**NetScaler ADC FIPS:**


Versions prior to 14.1-73.32 FIPS


-


**NetScaler ADC FIPS and NDcPP:**


Versions prior to 13.1-37.277


As of August 19, 2026, Rapid7 has not observed evidence that CVE-2026-19490 is being exploited in the wild. However, organizations should prioritize patching affected systems on an emergency basis, since Citrix products are high-value targets that tend to quickly see exploitation in the wild.


## Mitigation guidance


Organizations running affected NetScaler ADC or NetScaler Gateway appliances should review the official NetScaler


[advisory](https://support.citrix.com/support-home/kbsearch/article?articleNumber=CTX696939) and apply the required updates to affected systems on an emergency basis.


Fixed versions for affected products are listed below:


-


**NetScaler ADC and NetScaler Gateway**


14.1-73.32 and later releases


-


**NetScaler ADC and NetScaler Gateway**


13.1-63.21 and later releases of 13.1


-


**NetScaler ADC 14.1-FIPS**


14.1-73.32 FIPS and later releases of 14.1-FIPS


-


**NetScaler ADC 13.1-FIPS and 13.1-NDcPP**


13.1-37.277 and later releases of 13.1-FIPS and 13.1-NDcPP


According to Citrix, customers can determine whether affected systems are vulnerable to CVE-2026-19490 by inspecting their NetScaler configuration for the following configuration entries. If one or more of the following items are present, and if the systems are running affected versions, the system is likely to be exploitable:


-


**SAML action configuration is in place:**


-


**Auth or VPN vserver is configured:**


For the latest guidance, please refer to the official


[Citrix advisory](https://support.citrix.com/support-home/kbsearch/article?articleNumber=CTX696939) .


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Customers can assess exposure to CVE-2026-19490 on Citrix NetScaler ADC and Gateway using a vulnerability check expected to be available in the August 20 content release.


## Updates


-


**August 19, 2026:**


Initial publication.
