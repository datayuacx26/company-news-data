---
schema_version: "1.0.0"
document_id: "e1363b3755ad1467685a92a6abd7ebd9ae5beab0ec2ba9ccd66fe8863ab47677"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/2026/07/08/fortibleed-fortigate-credential-reuse-internet-exposed"
published_at: "2026-07-08T17:38:12+00:00"
first_seen_at: "2026-07-20T03:31:07.801512+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:70bca23e8cee4656893f881f28856a32c66a0bec48bcebd2f511718ff3b617b4"
---

# FortiBleed: Credential Reuse, Legacy Hashes, and the Risk of Internet-Exposed FortiGate Devices

#### Table of Contents


- Summary
- What Happened
- Who should pay attention
- WhyIt Matters/ Potential Impact
- Recommended Actions
- CVEs and Affected Components
- Exploitation Status / Threat Activity
- Remediation Long Tail: Why Historical Exposure Persists
- How Qualys Helps You Discover These Exposures
- Use Qualys QueryLanguageto PrioritizeFortiBleedExposure
- Detection and Threat Hunting Guidance
- Conclusion
- Contributor
- Frequently Asked Questions (FAQs)


### Key Takeaways


- FortiBleed refers to June 2026 public reporting of large-scale credential exposure and abuse targeting internet-reachable FortiGate management and SSL-VPN gateways driven by credential reuse and brute-force, not a single new zero-day.
- Risk is highest for internet-exposed FortiGate devices without MFA, with reused or legacy-hashed credentials, or prior exposure to known-exploited Fortinet CVEs.
- A patched device can remain exposed if credentials or configuration material were stolen before remediation, especially where PBKDF2 migration and legacy-hash cleanup are incomplete.
- Qualys provides direct QID coverage for eight relevant Fortinet CVEs, plus VMDR, ETM, and CSAM QQL queries to identify and prioritize exposed assets.
- Defenders should inventory internet-reachable services, patch where needed, revoke sessions, rotate exposed credentials, enforce MFA, and hunt for authentication anomalies and configuration changes.


## Summary


FortiBleed is a June 2026 public reporting cluster around large-scale credential exposure and credential abuse targeting internet-reachable FortiGate management and SSL-VPN gateways. Public reporting and Fortinet’s analysis point to credentials reportedly stolen, exposed, or reused from earlier incidents and replayed through brute-force and password-spraying, not a newly disclosed Fortinet vulnerability. Risk centers on internet-reachable FortiGate administrative interfaces and SSL-VPN gateways; this post maps eight Fortinet CVEs with associated Qualys detections, including CVE-2026-24858, CVE-2025-59718, and CVE-2025-59719.


Large-scale credential-abuse activity has been publicly reported, and[Fortinet confirmed credential reuse](https://www.fortinet.com/blog/psirt-blogs/analysis-of-reported-credential-compromise-of-fortigate-devices) , brute-force activity, and prior exploitation of earlier FortiCloud SSO issues. However, the public record does not confirm one campaign-wide zero-day, compromise of every reported record, or a universal exploit chain.


[Urgency is High](https://www.cisa.gov/news-events/alerts/2026/06/18/cisa-urges-hardening-fortinet-devices-after-reports-credential-exposure) for internet-exposed management or SSL-VPN deployments without MFA, with reused credentials, legacy hashes, or historical exposure; it is Medium-to-High for non-public deployments that share credentials, identity integrations, or management paths. A patched device can remain exposed when credentials or configuration material were copied before remediation, particularly where PBKDF2 migration and legacy-hash cleanup remain incomplete. This post maps each in-scope CVE to its Qualys detection (QID) and gives authentication, configuration, and identity threat-hunting guidance, plus VMDR, ETM, and CSAM QQL queries to identify and prioritize exposed assets.


## What Happened


On June 17, 2026, public reporting described attacker infrastructure and large datasets containing Fortinet-related URLs, device records, usernames, and credentials.[On June 18, 2026, CISA,](https://www.cisa.gov/news-events/alerts/2026/06/18/cisa-urges-hardening-fortinet-devices-after-reports-credential-exposure) the[UK NCSC](https://www.ncsc.gov.uk/news/advice-following-global-targeting-of-fortinet-firewalls-and-vpn-gateways) , and other government sources issued hardening and investigation guidance. Fortinet followed on June 19, 2026, assessing the activity as credential reuse from earlier incidents combined with brute-force or password-spraying—not a newly disclosed Fortinet vulnerability.


External reporting put the scale anywhere from roughly 30,800 validated records in one subset to about 74,000 records referenced by CISA, ~75,000 devices in independent analysis, ~73,900 unique firewall URLs, and 86,644 records in other reporting. These figures reflect different objects, snapshots, validation claims, and deduplication methods, so they should not be combined or treated as one verified count of compromised devices.


## Who should pay attention


Organizations with any FortiGate or FortiOS deployment(s) with an internet-reachable management interface or SSL-VPN, local administrator or VPN accounts without MFA, reused or legacy-hashed credentials, prior exposure to known-exploited Fortinet CVEs, or AD/LDAP/RADIUS credentials stored in the device configuration should confirm they are not exposed to this campaign. There is no single “FortiBleed version.” Rather, exposure is driven by credential history and reachability, not build number alone, so version checks must be paired with credential, session, and configuration review.


## Why It Matters/ Potential Impact


FortiGate devices sit at the network perimeter and often hold reusable enterprise secrets, so exposure rarely stays local. Large-scale credential-abuse activity against internet-facing Fortinet services has been publicly reported and is supported by vendor, government, and telemetry sources; Fortinet has separately confirmed credential reuse, brute-force activity, and prior exploitation of earlier FortiCloud SSO issues. Where credentials or a configuration were exposed, documented consequences include:


- **Unauthorized admin or SSL-VPN access** and persistent local accounts that can survive a patch. *(Confirmed as a risk class; per-device outcomes need evidence.)*


- **Configuration disclosure or alteration** , exposing rules, certificates, and connection material. *(Confirmed for the CVE-2026-24858 incident and 2022 config theft; claimed at FortiBleed scale but not quantified.)*


- **Exposure of embedded identity credentials** — AD/LDAP, RADIUS/TACACS+, API tokens — which Fortinet advises treating as potentially compromised where access is evident, enabling lateral movement into downstream identity systems. *(Confirmed as a risk condition; theft must be proven from logs.)*


- **Loss of edge-device trust** , since a patch alone does not restore trust after confirmed compromise.


For the business, this means potential remote-access disruption, security-control tampering, incident-response cost, and regulatory-notification exposure — highest where the appliance is publicly managed, identity-integrated, or shared across units or customers.


## Recommended Actions


Prioritize by exposure and evidence, not build number alone:


- **Inventory** internet-reachable management and SSL-VPN services, and reduce public exposure.


- **Patch or migrate** to fixed versions — necessary, but not sufficient alone.


- **Revoke sessions and rotate exposed or reused credentials** — local admin/VPN, AD/LDAP binds, RADIUS/TACACS+ secrets, API tokens, SNMP strings, and anything reused elsewhere.


- **Enforce MFA** on administrative and SSL-VPN access.


- **Complete PBKDF2 migration** , clear legacy hashes, and validate configuration integrity.


- **Treat QID and QQL matches below as investigation seeds** — “look here first,” not confirmed compromise.


- **Escalate to incident response** on evidence of unauthorized access, configuration change, persistence, log tampering, or downstream identity use.


## CVEs and Affected Components


The eight CVEs below — each with an associated Qualys detection (QID) — are assessed against this campaign. The first three are the priority; the rest supply historical credential- and integrity-theft context, not confirmed entry vectors.


**CVE** **QID** **QVSS** **What it is** **Exploitation & KEV** **Relevance to FortiBleed**


CVE-2026-24858 44914 9.5 Admin FortiCloud
SSO authentication bypass(FortiOS + FortiManager/
Analyzer/Proxy/
SwitchManager/
Web). Cross-account admin access when SSO enabled; documented config download and local-account persistence. Vendor-confirmed exploited. KEV (added 01-27-2026). Campaign-linked — Fortinet cites it as a source of credentials reused in June 2026. Highest-priority lookback. KEV membership is not proof it drove the full dataset.


CVE-2025-59718 44862 10 FortiCloud SSO SAML authentication bypass (FortiOS, FortiProxy, FortiSwitch
Manager). Admin auth bypass when the feature is enabled. Vendor-confirmed exploited. KEV (added 12-16-2025). Campaign-linked — explicitly cited as a reused-credential source. Prioritize SSO config review + session/credential revocation.


CVE-2025-59719 44862 9.5 Paired FortiCloud
SSO SAML verification issue (primarily
FortiWeb). Admin auth bypass when the feature is enabled. Vendor advisory marks it known exploited. No separate KEV listing (paired in CISA context). Campaign-linked (paired advisory). Add FortiWeb/SSO inventory. Don’t treat as a separate FortiGate exploit chain without product-specific evidence.


CVE-2018-13379 43702 10 FortiOS SSL-VPN path traversal / file disclosure. Unauthenticated file and credential disclosure. Known exploited (historical). KEV (11-03-2021). Public exploits widely available. Historical precedent only — credential-leak context. Not a confirmed June-2026 vector; don’t assume leaked credentials remain valid.


CVE-2022-40684 730623, 43921 10 Admin-interface authentication bypass (FortiOS, FortiProxy, FortiSwitch
Manager). Unauthenticated admin operations and configuration theft. Known exploited (historical). KEV (10-11-2022). Public exploits exist. Historical precedent — validated 2022 config theft. Models the post-patch credential-rotation lesson. The Belsen dataset is distinct from June 2026.


CVE-2022-42475 43944 10 FortiOS/FortiProxy
SSL-VPN heap overflow. Unauthenticated RCE. Known exploited (historical). KEV (12-13-2022). Public exploits exist. Historical persistence context (symbolic-link persistence). Not a confirmed
FortiBleed entry vector.


CVE-2023-27997 44059 10 FortiOS/FortiProy
SSL-VPN heap overflow. Unauthenticated RCE. Known exploited (historical). KEV (06-13-2023). Public exploits exist. Same persistence cluster as CVE-2022-42475. Device-integrity / rebuild context only.


CVE-2024-21762 44170 10 FortiOS/FortiProy
SSL-VPN out-of-bounds write. Unauthenticated RCE. Known exploited (historical). KEV (02-09-2024). Public exploits exist. Historical persistence context. Out-of-bounds write (not path traversal). No automatic FortiBleed linkage.


## Exploitation Status / Threat Activity


*Evidence for this campaign sits at very different confidence levels, so it is worth separating what is confirmed from what is only reported or theoretical.*


**Confirmed:** Large-scale credential attacks targeted internet-exposed Fortinet services. Fortinet confirms prior exploitation of CVE-2026-24858 and CVE-2025-59718; FG-IR-25-647 also marks paired CVE-2025-59719 known exploited. CVE-2018-13379, CVE-2022-40684, CVE-2022-42475, CVE-2023-27997, and CVE-2024-21762 are historically known exploited.


**Public PoC:** Public exploit references exist for the historical CVEs. Claimed repositories exist for CVE-2026-24858 and CVE-2025-59718, but were not validated; no separately validated PoC was identified for CVE-2025-59719.


**Reported, not independently verified campaign-wide:** Exact working-credential or compromise totals, broad configuration extraction, offline recovery, passive interception, persistence, and downstream compromise.


**Theoretical or not confirmed:** A new campaign-wide zero-day, one universal exploit chain, or every dataset record representing current compromise. No authoritative actor attribution exists; an initial-access-broker claim and Russian-language indicators remain unvalidated, so attribution confidence is Low. KEV membership confirms exploitation of a CVE, not its use in FortiBleed.


## Remediation Long Tail: Why Historical Exposure Persists


Among the historically known-exploited issues above, CVE-2023-27997 shows why ‘known exploited’ does not mean ‘fully remediated.’ It appears here as device-integrity context, not as a confirmed FortiBleed entry vector. Its remediation curve illustrates the long tail defenders face with known-exploited Fortinet SSL-VPN vulnerabilities: even with a median time-to-remediate of roughly two months, a residual population remains open well past six months — extending the period in which historical exposure, credential rotation, and configuration integrity still matter.


> *Figure: CVE-2023-27997 vulnerability survival curve. This single-CVE view illustrates long-tail remediation risk for a known-exploited Fortinet SSL-VPN heap-based buffer overflow (Chart markers: median 64 days, average 75 days to remediation). It should not be read as an all-CVE FortiBleed metric, a FortiBleed victim count, or evidence that CVE-2023-27997 was the June 2026 campaign entry vector.*


## How Qualys Helps You Discover These Exposures


The first operational step is to identify which assets are actually exposed. Correlate each QID finding with feature state, public reachability, credential history, logs, and configuration integrity — on its own a QID proves neither exploitability nor compromise.


• **CVE-2026-24858** : QID 44914 — FortiNet FortiManager/FortiAnalyzer Administrative Forticloud Sso Authentication Bypass Improper Access Control Vulnerability


• **CVE-2025-59718** and **CVE-2025-59719** : QID 44862 — FortiNet FortiWeb Improper Access Control Vulnerability


• **CVE-2018-13379** : QID 43702 — Fortinet Fortigate (FortiOS) System File Leak through Secure Sockets Layer (SSL) Virtual Private Network (VPN) via Specially Crafted Hypertext Transfer Protocol (HTTP) Resource Requests


• **CVE-2022-40684** : QID 730623 — FortiOS Authentication Bypass Vulnerability on Administrative Interface (HTTP/HTTPS) (Unauthenticated Check)


• **CVE-2022-40684** : QID 43921 — FortiOS Authentication Bypass Vulnerability on Administrative Interface (HTTP/HTTPS)


• **CVE-2022-42475** : QID 43944 — FortiOS Buffer OverFlow Vulnerability


• **CVE-2023-27997** : QID 44059 — Fortinet FortiOS Heap-Based Buffer Overflow Vulnerability


• **CVE-2024-21762** : QID 44170 — FortiOS Out-of-Bound Write Vulnerability in sslvpnd


QID 730623 is identified as an unauthenticated check, while QID 43921 is the companion detection for the same advisory; do not infer identical scan methods beyond the published titles.


## Use Qualys Query Language to Prioritize FortiBleed Exposure


Building on the QID coverage above, Qualys customers can use Qualys Query Language (QQL) to move from “which assets have relevant detections?” to “which assets should be remediated first?”[VMDR](https://www.qualys.com/apps/vulnerability-management-detection-response) is best suited for vulnerability triage by QID, ETM inside Risk Management helps prioritize high-risk findings across the full CVE scope, and[Qualys CyberSecurity Asset Management](https://www.qualys.com/apps/cybersecurity-asset-management) (CSAM) helps scope Fortinet inventory and internet-facing exposure. As with the QID coverage above, treat every returned asset as an investigation seed — a match means ‘look here first,’ not confirmed compromise.


### *VMDR — Identify QID-linked FortiBleed detections*


```text
vulnerabilities.vulnerability.qid  :[  44914  ,  44862  ,  43702  ,  730623  ,  43921  ,  43944  ,  44059  ,  44170  ]
and  (  vulnerabilities.riskFactor.isCisaKnownExploit  :  TRUE     or
vulnerabilities.vulnerability.isPatchAvailable  :  TRUE     or     vulnerabilities.status  :  Active  )
```


Use this VMDR query as the broad vulnerability-management sweep. It returns assets with any of the eight blog-scoped Fortinet QIDs where the finding is KEV-associated, patchable, or active.


### *ETM Inside Risk Management — Prioritize High-QDS KEV Findings*


```text
finding.cveId  :[  CVE-2026-24858  ,  CVE-2025-59718  ,  CVE-2025-59719  ,  CVE-2018-13379  ,  CVE-2022-40684  ,
CVE-2022-42475  ,  CVE-2023-27997  ,  CVE-2024-21762  ]   and     finding.qds  >=  70     and     finding.riskFactor.rti: Cisa_Known_Exploited_Vulns
```


Use this ETM query when the goal is risk prioritization rather than asset discovery. It focuses on findings tied to the eight scoped CVEs with QDS of 70 or higher and the CISA Known Exploited Vulnerability risk factor.


### *ETM TruConfirm — Confirm Exploitability For FortiBleed-Relevant CVEs*


Qualys TruConfirm is a module inside ETM that actively tests whether a vulnerability is truly exploitable — not just flagged because the software version matches. Two of the eight FortiBleed-relevant CVEs — CVE-2018-13379 and CVE-2022-40684 — have TruConfirm validation available, giving Qualys customers a path from a version-match detection to direct evidence of exploitability for those two findings.


### *CSAM — Scope FortiOS Software Inventory With FortiBleed-Relevant Detections*


```text
software  :(  name  :  FortiOS  )   and
vulnerabilities.vulnerability.qid  :[  44914  ,  44862  ,  43702  ,  43921  ,  43944  ,  44059  ,  44170  ,  730623  ]   and
vulnerabilities.vulnerability.threatIntel.isCisaKnownExploitedVuln  :  true
```


Use this CSAM query to find FortiOS software inventory records that also carry FortiBleed-relevant vulnerability detections with the CSAM CISA-known-exploited threat-intel flag.


### *CSAM — Scope Internet-Facing Fortinet Assets With Active FortiBleed-Relevant Findings*


```text


```


```text
asset.tag.name  :  "Internet Facing"     and     software  :(  name  :  FortiOS  )   and
vulnerabilities.vulnerability.qid  :[  44914  ,  44862  ,  43944  ,  44059  ,  44170  ,  730623  ]
and     vulnerabilities.status  :  Active
```


Use this CSAM query to narrow attention to FortiOS assets tagged as internet-facing and still carrying active FortiBleed-relevant findings.


## Detection and Threat Hunting Guidance


*With exposed assets scoped, hunt them for signs of credential abuse. Because a patched device can still be compromised through stolen credentials, detection should center on authentication, configuration, and identity telemetry rather than version state. Hunt across four areas:*


**Authentication anomalies — hunt for:**


- High-volume failed administrator or SSL-VPN logins followed by success


- One source testing many usernames


- One username tested across distributed sources


- First-seen administrator IP, ASN, country, interface, protocol, or time of day


- Successful access for a dormant account, especially without MFA


**Configuration and account changes — review:**


- Configuration backup, download, or broad administrative reads after unusual authentication


- Creation, enablement, rename, or privilege elevation of administrator or VPN accounts


- Changes to trusted hosts, local-in policy, MFA, SSO, logging, DNS/NTP, certificates, or management exposure


**Downstream identity activity — investigate:**


- AD/LDAP/RADIUS, API, backup, or service-account use from unexpected hosts.


- Where Windows auditing is enabled, correlate Events 4624, 4625, 4648, 4672, 4720, 4728/4732/4756, 4768/4769, 4776, and 6272/6273 (event generation depends on audit-policy configuration)


**Exposure and integrity — validate:**


- Internet-reachable management or SSL-VPN services


- Configuration drift, HA-member divergence


- Log-forwarding interruption, and clock changes


Correlate these signals before declaring an incident; a single alert, checker match, version finding, or QID is not proof of compromise.


## Conclusion


FortiBleed is not just a software-version problem; it is an exposure, credential, configuration, and device-trust problem. Defenders should inventory internet-reachable FortiGate management and SSL-VPN services, then identify applicable QID findings as investigation seeds. Patch or migrate affected systems, revoke active sessions, rotate exposed or reused credentials, enforce MFA, and validate PBKDF2 migration and configuration integrity. When evidence shows unauthorized access, configuration export or change, persistence, log tampering, or downstream identity use, move from vulnerability response to incident response. Fortinet-confirmed prior exploitation and publicly reported large-scale credential-abuse activity justify urgency, but declaring compromise still requires device-level evidence; treat uncertain credential or configuration state as exposure until disproven.


## Contributor


- Rushikesh Nandedkar, Principal Cyber Threat Intelligence Analyst, Qualys


Sign up for a demo of Qualys ETM to see TruConfirm, TruLens, and Agent Val in action.


[Sign Up Now](https://www.qualys.com/demo/enterprise-trurisk-management)


## Frequently Asked Questions (FAQs)


**Is FortiBleed a new zero-day vulnerability?**


No. FortiBleed refers to large-scale credential exposure and abuse targeting internet-exposed FortiGate devices. It is driven by reused or previously stolen credentials combined with brute-force and password-spraying attacks, not a newly disclosed Fortinet zero-day.


**Do I still need to take action if my FortiGate devices are already patched?**


Yes. A patched device can still be compromised if credentials or configuration material were stolen before remediation. You should still revoke active sessions, rotate exposed or reused credentials, enforce MFA, and complete PBKDF2 migration with legacy hash cleanup.


**Which Qualys QID should I check first for FortiBleed exposure?**


Start with **QID 44914** (CVE-2026-24858) and **QID 44862** (CVE-2025-59718 / 2025-59719). These are the CVEs most directly linked to the current campaign. Use the QQL queries in this post to prioritize assets with these detections.


**What should I do if I find matching QIDs in my environment?**


Treat every match as an investigation seed, not confirmed compromise. Inventory the asset, review authentication logs for anomalies, check for configuration changes, revoke sessions, rotate credentials, and enforce MFA. Escalate to incident response if you find evidence of unauthorized access or persistence.


**Can I use Qualys to find internet-facing FortiGate devices with these vulnerabilities?**


Yes. Use the CSAM query provided in this post that combines the “Internet Facing” tag with FortiOS software and active FortiBleed-relevant QIDs. This helps you quickly scope publicly reachable assets that need immediate attention.
