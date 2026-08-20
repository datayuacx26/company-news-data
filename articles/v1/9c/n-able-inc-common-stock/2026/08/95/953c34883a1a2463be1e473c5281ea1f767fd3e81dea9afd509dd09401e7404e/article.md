---
schema_version: "1.0.0"
document_id: "953c34883a1a2463be1e473c5281ea1f767fd3e81dea9afd509dd09401e7404e"
company_key: "n-able-inc-common-stock"
company: "N-able Inc."
source_id: "n-able-inc-common-stock-rss-2157b28f25ac"
canonical_url: "https://www.n-able.com/blog/n-central-security-update-august-10-2026"
published_at: "2026-08-09T23:22:07+00:00"
first_seen_at: "2026-08-03T00:05:49.675371+00:00"
fetched_at: "2026-08-03T04:26:23.705919+00:00"
content_hash: "sha256:f00744085ca8d408ccfad7dc24ddb9441e39b77ef418b9afd4a34313b2d1ce07"
---

# N‑central Security Update – August 10, 2026

Yesterday, N‑able posted an advisory to our[Uptime Page](https://uptime.n-able.com/) regarding exploitation against our N‑central platform in response to a security issue affecting customers running versions of N‑central prior to 2026.2. We had addressed this issue in later builds and were recommending that customers on older versions upgrade to version 2026.3 as an immediate protective measure. As our investigation continued, we identified an alternative method to exploit this vulnerability, which was not mitigated in our previous fix. This resulted in a hotfix for 2026.3, which was released this afternoon:[https://status.n-able.com/2026/08/02/n-central-2026-3-hotfix-1-mitigation-for-cve-2026-18577/](https://status.n-able.com/2026/08/02/n-central-2026-3-hotfix-1-mitigation-for-cve-2026-18577/)


## Background


On July 31, 2026, N‑able saw an increase in licensing issues for our on-premises N‑central customers. Licensing issues are not uncommon, but the volume was high and the engineering and security teams were engaged. On the morning of August 2, 2026 our analysis of a previously addressed vulnerability (CVE-2026-18556), which was fixed in 2026.2, exposed another vector to exploit this vulnerability. Our engineering team immediately developed a fix, which is available now to all customers. We’ve also released a new CVE for this finding, which is being released under[CVE-2026-18577](https://www.cve.org/CVERecord?id=CVE-2026-18577) .


## The attack


As we performed our analysis, we determined that an attacker had identified a vulnerability on all N‑central servers running a version prior to 2026.3.1.7, which allowed them to obtain administrative access remotely. Following exploitation, the attacker leveraged the Take Control feature and connected to systems within the N‑central managed environment. Once on those devices, the attackers registered a new service for a CloudFlare tunnel, enabling persistence into an environment after access to the N‑central server was revoked.


## The impact


A limited number of customers have been identified to be impacted by this, and, for those impacted customers, N‑able support has directly engaged. If you’re a customer who is not running the most recent version of N‑central, we strongly encourage you to upgrade to 2026.3.1.7.


## Indicators


N‑able had identified the following IP addresses being used in this attack:


173\[.\]249\[.\]252\[.\]200
87\[.\]249\[.\]138\[.\]34
37\[.\]19\[.\]210\[.\]32
37\[.\]153\[.\]90\[.\]88
92\[.\]118\[.\]112\[.\]181
68\[.\]235\[.\]46\[.\]214


Additional indicators will be shared as they become available.


CVE Public Link here:[CVE-2026-18577](https://www.cve.org/CVERecord?id=CVE-2026-18577)


## Security best practices


This incident is a reminder of how critical regular patching and strong security hygiene are in protecting your environment. Despite rigorous development and security practices, no software is immune to vulnerabilities, which is why a proactive security posture is essential. Many successful attacks exploit known vulnerabilities in outdated software, and staying current is one of your most effective defenses. We strongly encourage all customers to prioritize patching, enforce multi-factor authentication, routinely audit user access, and monitor for unusual activity in their environments.


©


N‑able Solutions ULC and N‑able Technologies Ltd. All rights reserved.


This document is provided for informational purposes only and should not be relied upon as legal advice. N‑able makes no warranty, express or implied, or assumes any legal liability or responsibility for the accuracy, completeness, or usefulness of any information contained herein.


The N-ABLE, N-CENTRAL, and other N‑able trademarks and logos are the exclusive property of N‑able Solutions ULC and N‑able Technologies Ltd. and may be common law marks, are registered, or are pending registration with the U.S. Patent and Trademark Office and with other countries. All other trademarks mentioned herein are used for identification purposes only and are trademarks (and may be registered trademarks) of their respective companies.
