---
schema_version: "1.0.0"
document_id: "ffd6dfd9057719377e0a899f5464863ace0b0bb11c3a3fa81a553e1f811c58a8"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/2026/08/19/oracle-critical-patch-update-august-2026-security-update-review"
published_at: "2026-08-19T13:49:27+00:00"
first_seen_at: "2026-08-19T14:59:02.288117+00:00"
fetched_at: "2026-08-19T14:59:04.655506+00:00"
content_hash: "sha256:c5cda9c8db0729bf667199d815d80b65a355ba1dc27680130e99a23f33c15b3e"
---

# Oracle Critical Patch Update, August 2026 Security Update Review

#### Table of Contents


- Qualys QID Coverage
- Notable Oracle Vulnerabilities Patched


Oracle released its August edition of Critical Patch Update. The update received patches for **943** security vulnerabilities. Some of the vulnerabilities addressed in this update impact more than one product. These patches address vulnerabilities in various product families, including third-party components in Oracle products.


In this Oracle Critical Patch Update, Oracle Fusion Middleware and Oracle Hyperion received the highest number of patches, 262.


53 of the 943 (about 6%) security patches provided by the August Critical Patch Update are for non-Oracle CVEs, such as open-source components included in and exploitable within Oracle product distributions.


This batch of security patches received **17** updates for Oracle Database products. The following is the product-wise distribution:


- 6 new security updates for Oracle Database Server with a maximum reported CVSS Base Score of 9.6.


- None of these updates applies to client-only deployments of the Oracle Database.


- 7 new security updates for Oracle Autonomous Health Framework with a maximum reported CVSS Base Score of 8.8.
- 4 new security updates for Oracle Essbase with a maximum reported CVSS Base Score of 9.8.


In these security updates, Oracle has covered product families, including Oracle Database Server, Oracle Autonomous Health Framework, Oracle Essbase, Oracle Application Testing Suite, Oracle Commerce, Oracle Communications, Oracle Construction and Engineering, Oracle E-Business Suite, Oracle Enterprise Manager, Oracle Financial Services Applications, Oracle Food and Beverage Applications, Oracle Fusion Middleware, Oracle Analytics, Oracle Hospitality Applications, Oracle Hyperion, Oracle Java SE, Oracle JD Edwards, Oracle MySQL, Oracle PeopleSoft, Oracle Retail Applications, Oracle Siebel CRM, Oracle Supply Chain, Oracle Virtualization.


## **Qualys QID Coverage**


Qualys has released the following QIDS mentioned in the table:


**QIDs** **Title**


87617 Oracle WebLogic Server August 2026 Critical Patch Update (CSPUAUG2026)


388372 Oracle Managed Virtualization (VM) VirtualBox August 2026 Critical Patch Update (CSPUAUG2026)


388371 Oracle PeopleSoft Enterprise PeopleTools Product Multiple Vulnerabilities (CSPUAUG2026)


388370 Oracle Java Standard Edition (SE) August 2026 Critical Patch Update (CSPUAUG2026)


20609 Oracle Database 19c Critical Patch Update – Aug 2026


**Note:** The table will be updated with additional QIDs once released.


## **Notable Oracle Vulnerabilities Patched**


### **Oracle Fusion Middleware**


This Critical Patch Update for Oracle Fusion Middleware received 262 security patches. Out of these, 182 vulnerabilities can be exploited over a network without user credentials.


A total of 78 CVEs have critical severity ratings.


### **Oracle Hyperion**


This Critical Patch Update for Oracle Hyperion received 262 security patches. Out of these, 107 vulnerabilities can be exploited over a network without user credentials.


A total of 27 CVEs have critical severity ratings.


### **Oracle E-Business Suite**


This Critical Patch Update for Oracle E-Business Suite received 120 security patches. Out of these, 27 vulnerabilities can be exploited over a network without user credentials.


CVE-2026-60782 and CVE-2026-70926 have critical severity ratings with a CVSS score of 9.8. Successful exploitation of these vulnerabilities can lead to remote code execution.


### **Oracle Commerce**


This Critical Patch Update for Oracle Commerce received 66 security patches. Out of these, 47 vulnerabilities can be exploited over a network without user credentials.


A total of 18 CVEs have critical severity ratings.


### **Oracle Siebel CRM**


This Critical Patch Update for Oracle Siebel CRM received 50 security patches. Out of these, 21 vulnerabilities can be exploited over a network without user credentials.


A total of 10 CVEs have critical severity ratings.
