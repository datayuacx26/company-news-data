---
schema_version: "1.0.0"
document_id: "0d8fb9a3bfb74ff20d7ccb23d11764ed7b4ced7a497b4c9b15ca49d4e474e5d5"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-cve-2026-63077-critical-unauthenticated-remote-code-execution-in-jetbrains-teamcity"
published_at: "2026-07-29T16:16:48+00:00"
first_seen_at: "2026-07-29T17:43:09.865281+00:00"
fetched_at: "2026-07-29T17:43:10.561050+00:00"
content_hash: "sha256:904231a3af5ffab4899a657610651d31c3c4b26bf8dc25e4d5cbd0bfe6aa36bf"
---

# CVE-2026-63077: Critical unauthenticated remote code execution in JetBrains TeamCity

## Overview


On July 27, 2026, JetBrains published a


[security advisory](https://blog.jetbrains.com/teamcity/2026/07/cve-2026-63077/) for


[CVE-2026-63077](https://nvd.nist.gov/vuln/detail/CVE-2026-63077) , a critical unauthenticated vulnerability affecting all versions of TeamCity On-Premises. The issue is classified as deserialization of untrusted data and has a CVSS score of


[9.8](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H) . An unauthenticated remote attacker with HTTP(S) access to a TeamCity server can exploit the agent polling protocol to bypass authentication checks and execute arbitrary operating system commands with the privileges of the TeamCity server process.


In the


[blog post](https://blog.jetbrains.com/teamcity/2026/07/cve-2026-63077/) that JetBrains shared in tandem with CVE publication, they stated that attackers who exploit the vulnerability can read stored credentials and compromise CI/CD pipeline integrity. The impact of successful exploitation depends on the operating system privileges granted to the TeamCity server process. At the time of disclosure, JetBrains stated that they were not aware of active exploitation.


## Mitigation guidance


Organizations running TeamCity On-Premises should urgently prioritize updating to a fixed version, either via the TeamCity UI


[update workflow](https://blog.jetbrains.com/teamcity/2026/07/cve-2026-63077/) or by


[downloading and installing](https://www.jetbrains.com/teamcity/download/other.html) one of the following fixed versions:


-


TeamCity 2025.11.7


-


TeamCity 2026.1.3


All versions of TeamCity On-Premises are affected. Organizations that cannot upgrade can apply JetBrains'


[security patch plugin](https://blog.jetbrains.com/teamcity/2026/07/cve-2026-63077/) to TeamCity 2017.1 and later. The plugin addresses only CVE-2026-63077; JetBrains recommends upgrading to a fixed version to receive other security updates. TeamCity Cloud customers do not need to take action.


In addition to patching, as a defense-in-depth measure, Rapid7 recommends restricting network access to TeamCity servers to only users and systems that must have it. For the latest mitigation guidance, please refer to the


[JetBrains security advisory](https://blog.jetbrains.com/teamcity/2026/07/cve-2026-63077/) .


## Rapid7 customers


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-63077 with a vulnerability check available in the July 28 content release.


## Updates


-


**July 29, 2026:**


Initial publication.
