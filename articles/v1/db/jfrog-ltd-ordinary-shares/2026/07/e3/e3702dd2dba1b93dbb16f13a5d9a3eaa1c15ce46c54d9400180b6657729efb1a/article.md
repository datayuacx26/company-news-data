---
schema_version: "1.0.0"
document_id: "e3702dd2dba1b93dbb16f13a5d9a3eaa1c15ce46c54d9400180b6657729efb1a"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/where-cve-severity-scores-go-wrong/"
published_at: "2026-07-01T13:15:07+00:00"
first_seen_at: "2026-07-20T23:22:16.766689+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:1322682df2e5438e6639595f7c6a7148e4df4c49ecd0d34f8a2eab4bf01711ca"
---

# Where Severity Scores Go Wrong: “Just Add Prototype Pollution”

At JFrog, our Security Research team continuously monitors and analyzes newly disclosed CVEs across the open-source ecosystem. Throughout our research, we have repeatedly observed cases where the assigned severity score does not accurately reflect a vulnerability’s real-world impact or exploitability.


In fact, during 2025, JFrog researchers reassessed NVD critical-severity vulnerabilities and concluded that 96% warranted a lower severity rating. Similarly, 65% of NVD high-severity vulnerabilities were downgraded after considering practical exploitation requirements, environmental constraints, and realistic attack scenarios.


In this blog, we examine a recurring pattern in vulnerability scoring that can lead to inflated severity scores. Using Prototype Pollution vulnerabilities as a case study, we show how vulnerabilities that ultimately result in the same security impact can receive significantly different CVSS scores, and why organizations should look beyond the headlines when assessing severity and actual risk.


## What Is Prototype Pollution?


In JavaScript programming, every object created inherits properties from a prototype. When an application attempts to access a property that does not exist directly in an object, the JavaScript engine (such as V8 used in Chrome and[Node.js](http://node.js/) ) traverses the prototype chain until it finds the property or reaches the root prototype, Object.prototype which is linked to every JavaScript object.


Prototype pollution vulnerabilities allow attackers to modify properties on Object.prototype. As a result, attacker-controlled values may be inherited by numerous objects throughout the application, potentially influencing application behavior in unexpected ways.


Depending on how the polluted properties are used, prototype pollution vulnerabilities can lead to denial-of-service, cross-site scripting (XSS), privilege escalation, and in some cases, remote code execution. However, achieving these results typically requires application-specific conditions beyond the prototype pollution vulnerability itself.


## Axios’ Wave of Prototype Pollution Gadget Vulnerabilities


Since April 2026, the popular JavaScript HTTP client Axios has released advisories for 10 CVEs with one common characteristic: They all require a pre-existing prototype pollution vulnerability within the JavaScript process, introduced via a separate dependency..


Below is a summary of the affected CVEs and their severity ratings:


**CVE ID** **GitHub Severity** **NVD Severity** **JFrog Severity**


CVE-2026-44489 LOW MEDIUM LOW


CVE-2026-42042 MEDIUM – MEDIUM


CVE-2026-44495 HIGH – LOW


CVE-2026-42041 MEDIUM MEDIUM MEDIUM


CVE-2026-42033 HIGH – LOW


CVE-2026-42035 HIGH – LOW


CVE-2026-42044 MEDIUM CRITICAL MEDIUM


CVE-2026-44490 MEDIUM HIGH MEDIUM


CVE-2026-44494 HIGH – MEDIUM


CVE-2026-42264 HIGH CRITICAL MEDIUM


Although these vulnerabilities differ in their assigned severity, our research found a consistent requirement they share: **Axios is only exploitable in environments where prototype pollution already exists in the application context** .


## Example: CVE-2026-44494


To understand the nature of these issues, consider CVE-2026-44494.


*Advisory:*[GHSA-35jp-ww65-95wh](https://github.com/axios/axios/security/advisories/GHSA-35jp-ww65-95wh)


The conditions required for exploitation include:


1. The application uses a vulnerable version of Axios.
2. A prototype pollution condition exists in the same JavaScript runtime context.
3. The pollution occurs before Axios processes or merges the request configuration.
4. ` Object.prototype` has been polluted with the` proxy` property.
5. The user application must not override the` proxy` property polluted by the attacker, which is yet another precondition we found that is not mentioned by the advisory.
6.


Exploring these prerequisites highlights the important characteristics of this class of vulnerabilities, showing that Axios alone is not sufficient for exploitation.


This pattern is further illustrated by another recent example,[CVE-2026-44490](https://github.com/axios/axios/security/advisories/GHSA-898c-q2cr-xwhg) , which received a high-severity rating from NVD. While its advisory outlines a wide array of potential attack vectors depending on exactly which prototype is polluted, it shares the same fundamental roadblock: The exploit is entirely dependent on a pre-existing prototype pollution vulnerability elsewhere in the environment.


At first glance, these prerequisites may appear theoretical. To better understand their practical implications, we developed a proof of concept that demonstrates exactly what is required to exploit one of these vulnerabilities in a realistic scenario.


### Proof of Concept


To demonstrate that successful exploitation of these types of CVEs depends entirely on the presence of a separate prototype pollution vulnerability, we chained a known Lodash prototype pollution vulnerability (CVE-2019-10744) with the Axios proxy injection gadget (CVE-2026-44494).


The goal of the attack is straightforward: Use Lodash to pollute` Object.prototype` , then abuse Axios’ handling of inherited proxy configuration to redirect requests through an attacker-controlled server and ultimately achieve credential theft.


### PoC Environment


Successfully exploiting the Lodash vulnerability requires an extremely outdated version of the library, Lodash 4.17.11, which was released more than seven years ago!


Our PoC environment consists of three components:


- A legitimate backend server that provides data
- A vulnerable user application that uses both Lodash and Axios
- An attacker-controlled server that acts as a malicious proxy


An example of a standard backend server setup:


The vulnerable application exposes two endpoints:


- ` POST /config` for updating application configuration
- ` GET /api/data` for retrieving backend data


Let’s assume this application server, that contains the vulnerable prototype pollution configuration – by using Lodash’s vulnerable` defaultsDeep()` function when processing configuration updates received through` POST /config` .


Then we have the` /api/data` endpoint that uses Axios to fetch information from the backend server with the potentially polluted value.


To demonstrate successful exploitation, we configured an attacker-controlled server that acts as a malicious proxy and returns a distinctive response containing a` hijacked: true` flag.


With all components in place, we start the backend server, vulnerable application, and attacker server on separate ports.


### Stage 1: Prototype Pollution Through Lodash


The vulnerable Lodash` defaultsDeep()` function recursively merges user-supplied objects into existing objects, the vulnerable implementation allows attackers to supply special properties such as` __proto__` . Instead of being treated as normal data, these properties modify` Object.prototype` , enabling attacker-controlled values to be inherited by objects throughout the application.


In our attack, the attacker sends a malicious request to` POST /config` that pollutes the proxy property on` Object.prototype` .


As a result, instead of merely updating the application’s configuration, the request effectively creates an attacker-controlled` Object.prototype.proxy` value that can later be inherited by other objects.


At this stage, the application is already vulnerable to prototype pollution. However, no traffic has been intercepted and no credentials have been stolen yet. The attacker has only introduced a malicious value into the application’s runtime.


### Stage 2: Axios Consumes the Polluted Property


The` /api/data` endpoint invokes` axios.get()` to retrieve information from the backend server. Before prototype pollution occurs, Axios communicates directly with the legitimate backend and returns a normal response as expected.


When Axios processes a request, it checks whether a proxy configuration has been defined. In our application, no proxy is explicitly configured, so Axios normally communicates directly with the destination server.


After the attacker pollutes` Object.prototype.proxy` while exploiting the Lodash vulnerability, however, this changes. When Axios attempts to access the proxy configuration, JavaScript follows the prototype chain and discovers the attacker-controlled proxy property inherited from` Object.prototype` .


Axios then treats this inherited value as a legitimate proxy configuration and unknowingly routes outbound requests through the attacker-controlled server.


Importantly, Axios is not responsible for the prototype pollution itself. Rather, Axios acts as the gadget that consumes the polluted property and transforms it into a meaningful security impact. Without the Lodash vulnerability, no malicious proxy configuration would exist. Without Axios consuming that configuration, the polluted value would have no effect on network traffic.


### Stage 3: Achieve MitM


By combining the Lodash prototype pollution vulnerability with the Axios proxy injection gadget, the attacker gains control over the application’s network traffic.


**The attack chain is:**


Prototype Pollution in Lodash → Object.prototype.proxy Pollution → Axios Proxy Injection → Traffic Interception


Because all Axios requests are now routed through the attacker-controlled proxy, the attacker gains visibility into and control over the communication channel.


In our demonstration, the attacker returns a modified response containing the` hijacked: true` flag, proving that the original backend response has been replaced.


In applications that transmit sensitive information through Axios, such as API tokens, session cookies, or authentication credentials, all traffic is routed through the attacker-controlled proxy.


As shown below, the attacker can inspect and capture this information while still forwarding requests to the legitimate backend, resulting in credential theft and unauthorized access to protected resources.


This screencast below shows how the prototype pollution vulnerability escalated into a full Man-in-the-Middle (MITM) attack that can intercept, read, and modify all HTTP traffic including authentication credentials:


[https://speedmedia2.jfrog.com/08612fe1-9391-4cf3-ac1a-6dd49c36b276/media.jfrog.com/wp-content/uploads/2026/06/30143136/cve-severity-scores-poc.mp4](https://speedmedia2.jfrog.com/08612fe1-9391-4cf3-ac1a-6dd49c36b276/media.jfrog.com/wp-content/uploads/2026/06/30143136/cve-severity-scores-poc.mp4)


### What the PoC Actually Demonstrates


The most important takeaway from this demonstration is that the Axios vulnerability does not provide attackers with a path to prototype pollution. Instead, it assumes prototype pollution has already occurred.


In our attack chain, Lodash is the vulnerability that allows modification of` Object.prototype` , while Axios merely consumes the polluted value. Without the Lodash vulnerability, the attack fails. Likewise, without the Axios gadget, the polluted property has no effect on request routing.


This distinction is critical when assessing severity because the real-world risk depends on the simultaneous presence of multiple conditions rather than the Axios issue in isolation.


## Context Matters in Severity Assessment


All of these Axios vulnerabilities share similar exploitation requirements, yet their severity ratings vary from low to critical depending on the assumed impact of the polluted properties and downstream usage.


This highlights a key limitation in traditional severity scoring approaches, as they often evaluate the theoretical maximum impact of a vulnerability without fully accounting for the environmental conditions required in the wild.


In practice, the risk introduced by Axios is dependent on a pre-existing prototype pollution vulnerability. As a result, the severity assigned to these issues often reflects the impact of a complete attack chain rather than the standalone risk introduced by Axios itself.


At JFrog, we have long emphasized the importance of contextual vulnerability analysis, where exploitability is evaluated within realistic application conditions rather than in isolation. While the security impact of a successful prototype pollution-based attack chain can be significant, the majority of applications using Axios are not exposed to these prerequisites.


## Not a New Problem


Axios is not the first JavaScript project affected by prototype pollution gadget vulnerabilities.


In a 2024 DOMPurify vulnerability ([CVE-2024-48910](https://nvd.nist.gov/vuln/detail/CVE-2024-48910) ), both GitHub and NVD assigned a critical severity rating. However, JFrog researchers assessed the issue as low severity in most real-world scenarios due to the requirement that a separate prototype pollution vulnerability must already exist in the application context.


Similarly, a recent React Router vulnerability ([CVE-2026-42211](https://nvd.nist.gov/vuln/detail/CVE-2026-42211) ) and a Piscina vulnerability ([CVE-2026-55388](https://github.com/advisories/GHSA-x9g3-xrwr-cwfg) ) received a high severity rating from GitHub, despite requiring an existing prototype pollution condition to be exploitable.


These examples demonstrate a recurring pattern where gadget-style vulnerabilities are often scored based on their potential impact within an already compromised environment, rather than their standalone exploitability.


## Conclusion


Prototype pollution gadget vulnerabilities highlight a broader challenge in vulnerability management. Severity scores often describe the theoretical maximum impact of a vulnerability, but not necessarily the likelihood that this impact can be reached in real-world conditions.


As shown in the Axios case and related examples, many vulnerabilities require additional prerequisites, such as existing prototype pollution conditions, before they can be exploited. When these dependencies are not fully considered, severity scores can overstate the practical risk of a potential vulnerability.


Organizations should therefore treat CVSS and similar scoring systems as a starting point rather than a final assessment. Effective vulnerability prioritization requires contextual analysis that accounts for exploit prerequisites, environmental constraints, and application-specific behavior. Only by combining severity scores with real-world exploitability analysis can security teams make accurate and meaningful risk decisions.


To get a better understanding of how vulnerabilities can potentially affect your applications, visit the[JFrog Security Research Center](https://research.jfrog.com/) for the latest information.
