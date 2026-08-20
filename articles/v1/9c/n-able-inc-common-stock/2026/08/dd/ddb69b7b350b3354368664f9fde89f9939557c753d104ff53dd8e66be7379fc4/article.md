---
schema_version: "1.0.0"
document_id: "ddb69b7b350b3354368664f9fde89f9939557c753d104ff53dd8e66be7379fc4"
company_key: "n-able-inc-common-stock"
company: "N-able Inc."
source_id: "n-able-inc-common-stock-rss-2157b28f25ac"
canonical_url: "https://www.n-able.com/blog/n-central-security-update-august-1-2026"
published_at: "2026-08-01T21:44:57+00:00"
first_seen_at: "2026-08-01T22:30:55.095639+00:00"
fetched_at: "2026-08-01T22:30:56.000681+00:00"
content_hash: "sha256:43f73a7ae5530a32bacaf08f11b3651d6ff22cb193e05ed3ced72060a6591734"
---

# N‑central Security Update – August 1, 2026

Earlier today, N‑able posted an advisory to our[Uptime Page](https://uptime.n-able.com/) regarding exploitation against our N‑central platform in response to a security issue affecting customers running versions of N‑central prior to 2026.2. We addressed this issue in later builds and are recommending that customers on older versions upgrade to version 2026.3 as an immediate protective measure. Our investigation is ongoing, and we are consistently updating customers with further guidance as additional information becomes available—via this blog post.


## Background


On July 31, 2026, N‑able saw an increase in licensing issues for our on-premises N‑central customers. Licensing issues are not uncommon, but the volume was high and the engineering and security teams were engaged.


## The attack


As we performed our analysis, we determined that an attacker had identified a vulnerability on N-Central servers running v2026.1 and below which allowed them to obtain administrative access remotely. Following exploitation, the attacker leveraged the Take Control feature and connected to systems within the N‑central managed environment. Once on those devices, the attackers registered a new service for a CloudFlare tunnel, enabling persistence into an environment after access to the N‑central server was revoked.


## The impact


A limited number of customers have been identified to be impacted by this, and, for those impacted customers, N‑able support has directly engaged. If you’re a customer who is not running the most recent version of N‑central, we strongly encourage you to upgrade to 2026.3.


## Indicators


N‑able had identified the following IP addresses being used in this attack:


173\[.\]249\[.\]252\[.\]200
87\[.\]249\[.\]138\[.\]34
37\[.\]19\[.\]210\[.\]32
68\[.\]235\[.\]46\[.\]214


Additional indicators will be shared as they become available.


CVE Public Link here:[https://www.cve.org/CVERecord?id=CVE-2026-18556](https://www.cve.org/CVERecord?id=CVE-2026-18556)


## Security best practices


This incident is a reminder of how critical regular patching and strong security hygiene are in protecting your environment. Despite rigorous development and security practices, no software is immune to vulnerabilities, which is why a proactive security posture is essential. Many successful attacks exploit known vulnerabilities in outdated software, and staying current is one of your most effective defenses. We strongly encourage all customers to prioritize patching, enforce multi-factor authentication, routinely audit user access, and monitor for unusual activity in their environments.
