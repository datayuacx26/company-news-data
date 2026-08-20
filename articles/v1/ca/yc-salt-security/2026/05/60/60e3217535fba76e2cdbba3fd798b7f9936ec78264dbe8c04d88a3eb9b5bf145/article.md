---
schema_version: "1.0.0"
document_id: "60e3217535fba76e2cdbba3fd798b7f9936ec78264dbe8c04d88a3eb9b5bf145"
company_key: "yc-salt-security"
company: "Salt Security"
source_id: "yc-salt-security-news-import-ef3d8363d45c"
canonical_url: "https://salt.security/blog/owasp-api-security-top-10-explained"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-22T12:39:57.986974+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:f93ad4d56fdda7e1114c59717ad5514cf8d5f191b3b4ce8dedc021ec45be6169"
---

# OWASP API Security Top 10 Explained - What is OWASP?

### [API1:2023 Broken Object Level Authorization (BOLA)](https://salt.security/blog/api1-2019-broken-object-level-authentication)


Broken object level authorization stems from a lack of proper access controls on API endpoints allowing unauthorized users to access and modify sensitive data. BOLA is represented in about 40% of all API attacks and is the most common API security threat. Broken object level authorization API vulnerabilities have been number one on the OWASP list since 2019 and have kept their top spot in the 2023 version.


### [API2:2023 Broken Authentication](https://salt.security/blog/api2-2019-broken-user-authentication)


Broken authentication enables attackers to use stolen authentication tokens, credential stuffing, and brute-force attacks to gain unauthorized access to applications. This API authentication security vulnerability has kept its number two spot on the OWASP list since 2019.


### [API3:2023 Broken Object Property Level Authorization](https://salt.security/blog/api3-2019-excessive-data-exposure)


Broken Object Property Level Authorization merges attacks that happen by gaining unauthorized access to sensitive information by way of[Excessive Data Exposure](https://salt.security/blog/api3-2019-excessive-data-exposure) (previously listed as number 3 in the 2019 OWASP API Security Top 10) or[Mass Assignment](https://salt.security/blog/api6-2019-mass-assignment) (previously in sixth place in the 2019 list). Both techniques are based on API endpoint manipulation to gain access to sensitive data.


### [API4:2023 Unrestricted Resource Consumption](https://salt.security/blog/api4-2019-lack-of-resources-rate-limiting)


This vulnerability originates in APIs that improperly implement or neglect to implement limits on resource consumption, leaving them highly susceptible to brute-force attacks. Unrestricted Resource Consumption has replaced the previous number 4 in the OWASP API Security Top 10, Lack of Resources and Rate Limiting. However, while the name changed, this vulnerability remains the same overall.


### [API5:2023 Broken Function Level Authorization (BFLA)](https://salt.security/blog/api5-2019-broken-function-level-authorization)


This threat takes shape when authorization is not properly implemented, leading to unauthorized users being able to execute API functions such as adding, updating, or deleting a customer record or a user role. BFLA has kept its fifth spot on the list since 2019.


### [API6:2023 Unrestricted Access to Sensitive Business Flows](https://salt.security/blog/api6-2019-mass-assignment)


This new threat, which has replaced Mass Assignment as number 6 on the OWASP API Security Top 10, manifests when an API exposes a business flow without compensating for how the functionality could cause harm if used excessively through automation. To exploit this vulnerability, an attacker will need to understand the business logic behind the API in question, find sensitive business flows and automate access to them in order to cause harm to the business.


### [API7:2023 Server Side Request Forgery (SSRF)](https://salt.security/blog/api7-2019-security-misconfiguration)


Server Side Request Forgery can occur when a user-controlled URL is passed over an API and is honored and processed by the back-end server. The API security risks materialize if the back-end server tries to connect to the user-supplied URL, which opens the door for SSRF. This threat has replaced[Mass Assignment](https://salt.security/blog/api6-2019-mass-assignment) as number 6 on the OWASP API Security Top 10 list.


### [API8:2023 Security Misconfiguration](https://salt.security/blog/api7-2019-security-misconfiguration)


[Security misconfiguration](https://salt.security/blog/api8-2023-security-misconfiguration) is a catch-all for a wide range of security misconfigurations that often negatively impact API security as a whole and introduce API vulnerabilities inadvertently. This threat has been number 7 on the OWASP API Security Top 10 list released in 2019 and it has remained in the same position in 2023.


### [API9:2023 Improper Inventory Management](https://salt.security/blog/api9-2019-improper-assets-management)


This threat is the result of an outdated or incomplete inventory which can create unknown gaps in the API attack surface, making it difficult to identify older versions of APIs that should be decommissioned. Improper Inventory Management has replaced Improper Assets Management as number 9 in the OWASP API Security Top 10 and, while the name has been changed to emphasize the importance of an accurate and up-to-date API inventory, the threat remains the same.


### [API10:2023 Unsafe Consumption of APIs](https://salt.security/blog/api10-2019-insufficient-logging-monitoring)


The Unsafe Consumption of APIs vulnerability stems from the improper usage of APIs by API clients, such as bypassing API authentication security controls or manipulating API responses, which can lead to unauthorized access and data exposure. This API vulnerability can be exploited via the consumption of API data itself or by abusing third-party integration issues. Unsafe Consumption of APIs has replaced[Insufficient Logging and Monitoring](https://salt.security/blog/api10-2019-insufficient-logging-monitoring) as number 10 in the OWASP API Security Top 10.


## Why you need to understand the OWASP API Security Top 10


APIs connect today’s modern applications, power business innovation and allow companies to meet their customers’ increasingly high expectations for digitalization and speed. But, by becoming an invaluable asset to organizations, they have also become a primary target for attackers.


If you want to learn more about Salt and how we can help you on your API Security journey through discovery, posture governance, and run-time threat protection, please[contact us](https://salt.security/contact-us) ,[schedule a demo](https://salt.security/demo-request) , or check out our[website](https://salt.security/) .
