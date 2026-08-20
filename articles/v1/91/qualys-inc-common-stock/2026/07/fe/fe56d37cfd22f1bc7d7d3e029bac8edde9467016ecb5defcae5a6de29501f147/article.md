---
schema_version: "1.0.0"
document_id: "fe56d37cfd22f1bc7d7d3e029bac8edde9467016ecb5defcae5a6de29501f147"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/2026/07/22/oracle-critical-patch-update-july-2026-security-update-review"
published_at: "2026-07-22T14:19:19+00:00"
first_seen_at: "2026-07-22T14:54:35.202404+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:ce914c333a6f21b728f578e18d0d08a76b8a715929f73cafc6cf14482228f26a"
---

# Oracle Critical Patch Update, July 2026 Security Update Review

#### Table of Contents


- Qualys QID Coverage
- Notable Oracle Vulnerabilities Patched


Oracle released its third quarterly edition of this year’s Critical Patch Update. The update received patches for **1449** security vulnerabilities. Some of the vulnerabilities addressed in this update impact more than one product. These patches address vulnerabilities in various product families, including third-party components in Oracle products.


In this quarterly Oracle Critical Patch Update, Oracle E-Business Suite received the highest number of patches, 410, constituting about 28% of the total patches released.


1235 of the 1449 (about 86%) security patches in the July Critical Patch Update are for non-Oracle CVEs, such as open-source components included in, and exploitable within, Oracle product distributions.


This batch of security patches received **72** updates for Oracle Database products. The following is the product-wise distribution:


- 15 new security updates for Oracle Database Server with a maximum reported CVSS Base Score of 9.9.


- Three of these updates apply to client-only deployments of the Oracle Database.


- Three new security updates for Oracle APEX with a maximum reported CVSS Base Score of 5.5.
- Four new security updates for the Oracle Autonomous Health Framework with a maximum reported CVSS Base Score of 8.1.
- One new security update for Oracle Essbase with a reported CVSS Base Score of 4.8.
- One new security update for Oracle Global Lifecycle Management with a reported CVSS Base Score of 8.1.
- 27 new security updates for Oracle GoldenGate with a maximum reported CVSS Base Score of 9.1.
- No new security updates for Oracle Graph Server and Client, but third-party patches are provided.
- One new security update for Oracle NoSQL Database.
- One new security update for Oracle Spatial Studio.
- Five new security updates for Oracle SQL Developer, all remotely exploitable without authentication (max CVSS not yet verified).
- 14 new security updates for Oracle TimesTen In-Memory Database, four of which are remotely exploitable without authentication.


In these security updates, Oracle has covered product families, including Oracle E-Business Suite, Oracle Fusion Middleware, Oracle Communications, Oracle PeopleSoft, Oracle Database Products, Oracle MySQL, Oracle Siebel CRM, Oracle Commerce, Oracle Supply Chain, Oracle Financial Services Applications, Oracle Analytics, Oracle Application Testing Suite, Oracle Construction and Engineering (Primavera), Oracle Enterprise Manager, Oracle Food and Beverage Applications (Hospitality Simphony), Oracle Health Sciences / HealthCare Applications, Oracle Hospitality (Cruise SPMS), Oracle Java SE, Oracle JD Edwards, Oracle Retail Applications, Oracle Systems (Solaris), and Oracle Virtualization (VM VirtualBox)


## **Qualys QID Coverage**


Qualys has released the following QIDS mentioned in the table:


**QIDs** **Title**


388003 Oracle JDeveloper Security Update (CPUJUL2026)


387986 Oracle Java Standard Edition (SE) July 2026 Critical Patch Update (CPUJUL2026)


387988 Oracle Coherence July 2026 Critical Patch Update (CPUJUL2026)


387996 Oracle Managed Virtualization (VM) VirtualBox July 2026 Critical Patch Update (CPUJUL2026)


387984 Oracle Hypertext Transfer Protocol (HTTP) Server July 2026 Critical Patch Update (CPUJUL2026)


296138 Oracle Solaris 11.4 Support Repository Update (SRU) 94.221.2 Missing (CPUJUL2026)


87614 Oracle WebLogic Server July 2026 Critical Patch Update (CPUJUL2026)


20599 Oracle E-Business Suite Security Update (CPUJUL2026)


20600 Oracle MySQL Server July 2026 Critical Patch Update (CPUJULY2026)


20601 Oracle E-Business Suite Security Update (CPUJUL2026)


**Note:** The table will be updated with additional QIDs once released.


## **Notable Oracle Vulnerabilities Patched**


### **Oracle E-Business Suite**


This Critical Patch Update for Oracle E-Business Suite received 410 security patches. Out of these, 45 vulnerabilities can be exploited over a network without user credentials.


CVE-2026-60880, CVE-2026-60773, CVE-2026-62549, and CVE-2026-62546 have critical severity ratings. Successful exploitation of these vulnerabilities can lead to remote code execution.


### **Oracle Fusion Middleware**


This Critical Patch Update for Oracle Fusion Middleware received 355 security patches. Out of these, 219 vulnerabilities can be exploited over a network without user credentials.


A total of 154 CVEs have critical severity ratings. Successful exploitation of these vulnerabilities can lead to remote code execution.


### **Oracle Communications**


This Critical Patch Update for Oracle Communications received 168 security patches. Out of these, 122 vulnerabilities can be exploited over a network without user credentials.


A total of 13 CVEs have critical severity ratings. Successful exploitation of these vulnerabilities can lead to remote code execution.


### **Oracle PeopleSoft**


This Critical Patch Update for Oracle PeopleSoft received 84 security patches. Out of these, 45 vulnerabilities can be exploited over a network without user credentials.


A total of 17 CVEs have critical severity ratings. Successful exploitation of these vulnerabilities can lead to remote code execution.


### **Oracle MySQL**


This Critical Patch Update for Oracle MySQL received 54 security patches. Out of these, nine vulnerabilities can be exploited over a network without user credentials.


None of the CVEs has a critical severity rating.
