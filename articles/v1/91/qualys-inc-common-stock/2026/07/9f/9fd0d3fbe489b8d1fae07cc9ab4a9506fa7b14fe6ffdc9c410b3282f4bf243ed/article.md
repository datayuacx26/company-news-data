---
schema_version: "1.0.0"
document_id: "9fd0d3fbe489b8d1fae07cc9ab4a9506fa7b14fe6ffdc9c410b3282f4bf243ed"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/07/06/owasp-top-10-2025-appsec-coverage-gap"
published_at: "2026-07-06T15:00:00+00:00"
first_seen_at: "2026-07-20T03:32:58.378885+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:ab8701d13601e9cbbd6638189b9fadd999b5b567bd01ff109a2e2b3e3e1c5a00"
---

# Is Your AppSec Program Built to Close the OWASP Top 10 2025 Coverage Gap?

#### Table of Contents


- Can Your Current AppSec Program Actually Cover All 10 Categories?
- The Coverage Gaps Most Programs Are Already Carrying
- What This Already Looks Like for Teams Running TotalAppSec
- Qualys TotalAppSec Closes This Execution Gap
- OWASP Top 10 2025 and TotalAppSec Coverage Map
- What This Looks Like in Practice
- Close the Coverage Gaps Before Your Next Audit Cycle
- Frequently Asked Questions


### Key Takeaways


- Most AppSec programs treat API-layer coverage as a DAST extension, but BOLA, BFLA, and SSRF require authenticated multi-role testing that traditional scanners weren’t built to run at scale.


- Modern authentication flows (OAuth2, JWT validation, MFA-protected sessions) often fall outside standard scan scope, meaning the exact endpoints where account takeover occurs go untested.


- TotalAppSec maps to all 10 OWASP 2025 categories on one platform: AI-powered DAST, dedicated API security testing, deep learning malware detection, modern auth coverage, and TruRisk prioritization that eliminates ~95% of alert noise.


- Signature-dependent detection misses the categories OWASP elevated most aggressively in 2025 – supply chain compromise, third-party script injection, and fail-open behavior frequently appear before a CVE exists.


- The programs that audit their coverage gaps against the 2025 list and close discovery blind spots before the next audit cycle will avoid spending the next quarter in reactive remediation.


If you’ve read[What Changed in OWASP Top 10 2025 and Recommendations for Each Category,](https://blog.qualys.com/qualys-insights/2026/06/15/what-changed-in-owasp-top-10-2025-and-recommendations-for-each-category) you already know what the 2025 update changed and what OWASP recommends for each of the 10 categories. This post is the practitioner’s guide to implementing recommendations in real-world AppSec environments.


[OWASP 2025](https://owasp.org/Top10/2025/) changed the standard. Now the question is whether your OWASP Top 10 2025 coverage actually matches the categories being exploited right now.


## Can Your Current AppSec Program Actually Cover All 10 Categories?


Consider your current program, whatever scanners, schedules, and workflows you run today, and answer these questions honestly against the 2025 categories. This is where most AppSec program gaps, OWASP 2025 introduced, hide in plain sight.


- ***In an API-heavy app, can you authenticate requests across multiple user roles and manipulate object references to confirm that a user cannot access another user’s records? Or does testing stop at a single documented role?***
- ***Can your scanning frequency keep up with accelerated release cycles, or does a full scan take hours and overload production, forcing you to throttle it to a fixed schedule?***
- ***Is your program able to catch a malicious third-party script injected into a page you do not control, or a tampered dependency delivered through a legitimate update?***
- ***Can your scanner complete an OAuth2 flow, hold and refresh a token through a multi-step session, and flag a weak JWT at scan time? Or does authenticated scanning fall back to a single static session, leaving every token-handling endpoint untested?***
- ***How many tools and manual hand-offs did your last real finding pass through before it was remediated? How long did it remain unassigned after being flagged?***


If any of those questions created uncertainty, the issue usually isn’t scanner quality alone. It’s the gap between detection and remediation: validation, ownership, and response.


## The Coverage Gaps Most Programs Are Already Carrying


Most AppSec teams find the same pattern when mapping current coverage to the 2025 categories. This program looks strong across the documented web layer and known-vulnerability signatures, but weaker where modern attack paths have shifted. These gaps cluster around API authorization, deployment velocity, and signature-dependent detection. Closing that execution gap is what the rest of this post covers.


- The API surface behind web forms is rarely tested beyond the documented happy path, leaving BOLA, BFLA, IDOR, and SSRF exposure behind “covered” applications. That’s a direct gap in OWASP Top 10 2025 API security coverage that traditional scanners weren’t designed to close. Knowing how to test BOLA and BFLA properly, meaning authenticated, multi-role validation rather than a single documented user path, is the difference between a program that looks covered and one that actually is.
- Scan cadence falls behind deployment velocity, creating exposure windows in which misconfigurations, verbose errors, exposed admin interfaces, and IaC drift remain live between releases. Signature-dependent detection misses the categories OWASP 2025 elevated most aggressively, because supply-chain compromise, third-party script injection, fileless payloads, and fail-open behavior frequently emerge before a CVE or signature exists.
- At the same time, modern authentication flows increasingly sit entirely outside the scan scope. OAuth2 grant types, JWT validation paths, MFA-protected sessions, and machine-to-machine token infrastructure are difficult to maintain through traditional authenticated scanning, even though those endpoints now sit directly in the path of account takeover activity.


For AppSec and DevSecOps practitioners who manage option profiles, scan configurations, QID libraries, and the remediation tickets that those scans generate, this OWASP Top 10 2025 practitioner guide will help them close coverage gaps, not just catalog them.


## What This Already Looks Like for Teams Running TotalAppSec


One insurance multinational used TotalAppSec’s continuous discovery to surface roughly 250 web applications and 750 Swagger files it didn’t know it had. Closing OWASP Top 10 2025 coverage gaps starts with knowing your real attack surface. Another customer used TotalAppSec’s prioritization and workflow together to scale AppSec coverage 400% across a multi-team SaaS portfolio, while maintaining zero critical AppSec failures over 15+ years.


## Qualys TotalAppSec Closes This Execution Gap


Qualys[TotalAppSec](https://www.qualys.com/apps/totalappsec) is built for the conditions the 2025 OWASP update explicitly specifies. One platform covering web and API surfaces, AI-powered scanning that learns behaviorally and runs continuously without breaking production.


It also supports modern authentication flows that follow OAuth2 and JWT into the endpoints most targeted in account takeover attacks. Deep learning detection that catches what signature stacks miss, while business-risk prioritization helps teams focus on what to fix first.


---


#### Qualys Blog


### Already running Qualys WAS or TotalAppSec? Read the full release notification for the OWASP 2025 update to understand the latest detection coverage, what changed automatically, and what requires action on your side.


Read Now


---


## OWASP Top 10 2025 and TotalAppSec Coverage Map


The following table maps TotalAppSec capabilities to the OWASP Top 10 2025 categories.


**Notes:**


- *A06 (Insecure Design): A06 is a program-level discipline no scanner owns end-to-end. TotalAppSec contributes to detection symptoms and TruRisk-driven views, but complete coverage also requires architectural review.*
- *API Security: This update changes only to the OWASP Top 10 for Web Applications. OWASP API Security Top 10 2023 is unaffected. TotalAppSec’s API Security capability (~600 QIDs covering BOLA, BFLA, OAS non-compliance, sensitive data exposure across REST and SOAP) operates against that separate standard.*


## What This Looks Like in Practice


### Visibility expands beyond the known attack surface


Most AppSec teams only secure the web applications and APIs they know about. The first step toward operationalizing AppSec is identifying assets outside the known attack surface, because you can’t protect what you can’t see.


TotalAppSec continuously discovers web applications and APIs across multi-cloud environments, including API gateways such as MuleSoft, AWS API Gateway, Azure APIM, and Apigee. It also encompasses CSAM, VMDR, third-party imports, and AI/API discovery during active scans. This broad discovery can significantly increase the visibility of web assets. For example, one multinational insurance customer discovered approximately 250 web applications and 750 Swagger files, resulting in a sevenfold increase in visibility across its application estate.


### Exposure windows shrink between deployments


Cluster creation groups similar vulnerabilities together. Deep learning then predicts related vulnerabilities within each cluster and refines its focus as the scan progresses. This behavioral approach reduces scan time by approximately 50% and requires 65% fewer requests, all without compromising detection accuracy. Additionally, the same engine strengthens OWASP A10 coverage by identifying fail-open patterns and verbose error responses that signature-based scanners often overlook.


### Modern authentication flows stay inside the testing scope


OAuth2 first-class support for Okta, Auth0, and Azure AD with full grant-type coverage and auto-refresh. JWT weakness flagging at scan time. MFA and SSO via Qualys Browser Recorder. Secure header injection for bearer tokens. The endpoints where account takeover actually happens stay inside the scan scope.


### Threats get detected before the signatures exist


TotalAppSec delivers approximately 99% detection accuracy for zero-day, fileless, and supply chain attacks without relying solely on prior signatures. It achieves this by combining heuristic, behavioral, reputational, and deep learning analysis with neural fingerprinting to identify both known and emerging malware.


### The findings that materially change risk are prioritized


TotalAppSec reduces alert noise by up to 95% by combining asset criticality, threat intel, and exploitability into a single, risk-based score. One Qualys customer used the prioritization and workflow approach to scale AppSec coverage by 400% across a multi-team SaaS portfolio, while maintaining zero critical AppSec failures over 15+ years.


### Operational gaps across fragmented AppSec stacks get closed


Most enterprise AppSec programs depend more on multiple DAST and AST tools. The challenge isn’t the licensing, but the manual effort required to reconcile findings across tools, dashboards, and ticket queues, where high-impact vulnerabilities can remain unresolved.


[ETM](https://www.qualys.com/apps/enterprise-trurisk-management) centralizes TotalAppSec findings alongside DAST results from Checkmarx One and Veracode under a unified TruRisk-based prioritization view. For teams not ready to consolidate vendors, ETM enables teams to consistently prioritize risks and decrease operational overhead by managing fewer tools.


## Close the Coverage Gaps Before Your Next Audit Cycle


The OWASP Top 10 2025 isn’t simply a reshuffled list. It’s a reflection of how modern applications are being attacked today. Built on four years of real-world breach data, the updated categories and expanded CWE coverage highlight the gaps that traditional AppSec programs can no longer afford to ignore.


With the growing role of AI in vulnerability discovery and the shrinking time between disclosure and exploitation, organizations are encouraged to move beyond periodic scanning and signature-based detection. Effective strategies involve continuously discovering web and API assets, validating modern authentication flows, prioritizing risks according to business impact, and linking detection directly to remediation efforts.


This shift is what the OWASP Top 10 2025 signifies. It aligns with the approach that TotalAppSec aims to facilitate, emphasizing the operationalization of modern application security through continuous visibility, risk-based prioritization, and cohesive web and API security.


---


**Start a 30-day free trial of TotalAppSec and run a coverage check against all 10 OWASP 2025 categories on your own attack surface.**


[Start your Free Trial](https://www.qualys.com/free-trial-new/totalappsec)


---


## Frequently Asked Questions


### What is BOLA and how do you test for it under OWASP 2025?


Broken Object Level Authorization (BOLA) allows attackers to access another user’s data by manipulating API object references. Testing for it requires authenticated scanning across multiple user roles, enabling the scanner to verify that one user cannot access another user’s records.


### How does OAuth2 and JWT testing fit into OWASP 2025 coverage?


A07 requires testing the full token lifecycle: grant types, refresh flows, and JWT claim validation. Most traditional DAST tools focus primarily on web login pages, leaving machine-to-machine communications and token-handling endpoints outside the scan scope.


### What does “fail closed” mean under OWASP A10?


Fail closed means a system denies access when an error occurs, rather than allows it by default. Fail-open logic, where an unreachable auth service defaults to permissive access, is exactly what A10 addresses.


### How does TruRisk prioritization help with OWASP 2025 remediation?


TruRisk prioritizes vulnerabilities by combining asset criticality, threat intelligence, and exploitability into a single, risk-based score. This eliminates 95% of alert noise, so teams focus on the findings that materially affect business risk.


### Can one platform cover both OWASP Top 10 2025 and OWASP API Security Top 10 2023?


Yes. TotalAppSec supports both standards through a single platform. It addresses the OWASAP Top 10 2025 for Web Applications with AI-powered DAST, while also providing API Security capabilities that cover BOLA, BFLA, OAS non-compliance, and sensitive data exposure across REST and SOAP.
