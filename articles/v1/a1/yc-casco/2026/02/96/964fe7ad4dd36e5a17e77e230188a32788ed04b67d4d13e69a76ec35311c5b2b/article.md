---
schema_version: "1.0.0"
document_id: "964fe7ad4dd36e5a17e77e230188a32788ed04b67d4d13e69a76ec35311c5b2b"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/how-to-hack-ford-for-130k-dollars"
published_at: "2026-02-26T20:26:00+00:00"
first_seen_at: "2026-07-21T12:40:29.812234+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:7a3a0398edb927f74336c61cb6441697fd96c80bc8380184c775aa0e3d909cbd"
---

# How to Hack Ford.com for $130,000

# How to Hack Ford.com for $130,000


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Thu Feb 26 2026


*Disclaimer: The vulnerability discussed in this post was responsibly disclosed to the Ford security team. The issue has been addressed and is no longer exploitable.*


Sometimes the most critical flaws are simply operational oversights that no code analysis could ever catch. Casco discovered a vulnerability in Ford's infrastructure. A "dangling pointer" in their DNS could have allowed an attacker to fully compromise user accounts.


Here is how a forgotten subdomain and a $130,000 domain purchase could have led to a massive breach.


## The Discovery: A Dangling Subdomain


While mapping the attack surface of` ford.com` , we identified a subdomain that looked interesting:


m.dominicana.ford.com


This subdomain was configured with a CNAME record pointing to an external service. Specifically, it was pointing to a domain managed by` redirect.ford.icm.io` .


When we visited the subdomain and saw a Godaddy domain offered up for purchase. This is the classic signature of a **Subdomain Takeover** .


In a standard takeover, an attacker simply registers the unclaimed resource at the third-party provider (like claiming an unused S3 bucket name) for free. But this case was different.


## The $130,000 Paywall (well, technically, it’s $129,500)


The` icm.io` domain that` m.dominicana.ford.com` pointed to was available for purchase.


We reached out to a domain broker to see what it would take to acquire the target domain. The response was unexpected. The price tag to acquire the necessary` icm.io` domain was **$129,500** .


At first glance, this seems like a deterrent. Why would a hacker spend nearly $130,000 to perform a subdomain takeover? To understand the ROI for an attacker, we have to look at the potential impact.


## The Chain of Impact: Broadly Scoped Cookies


We analyzed the authentication mechanisms on the main` ford.com` website. The cookies were set with the Domain attribute pointed to` .ford.com` . This is a common configuration that allows a user to stay logged in as they navigate between different subdomains (e.g.,` parts.ford.com` or` finance.ford.com` ).


However, this convenience feature becomes a critical vulnerability when a subdomain is compromised.


### The Attack Path


If a sophisticated threat actor had purchased the` icm.io` domain, they would have gained full control over the content served at` m.dominicana.ford.com` . Here is what they could have done:


1. **Legitimacy Cloaking:** The attacker hosts a phishing page at m.dominicana.ford.com. To the user, this looks 100% legitimate. It has a valid ford.com URL and likely a valid SSL certificate.
2. **Session Hijacking:** Because the main application scopes cookies to .ford.com, when a logged-in user visits the attacker's controlled subdomain, the browser *automatically* sends the user's valid session cookies to the attacker's server.
3. **Credential Siphoning:** The attacker's server logs these requests, harvesting active session tokens. They can now impersonate those users on the main Ford website, bypassing passwords and 2FA.


For a criminal organization, investing $130,000 to gain persistent access to Ford user accounts, potentially including financing info, vehicle controls, or personal data could be seen as a viable business expense.


## Remediation


We promptly reported this issue to the Ford security team. They have since removed the dangling DNS record, effectively addressing the vulnerability.


Old infrastructure does not just disappear; it becomes a liability.


## Disclosure Timeline


- **\[2026-01-05\]:** Vulnerability discovered by Casco.
- **\[2026-01-06\]:** Report submitted to Ford Security Team.
- **\[2026-01-07\]:** Dangling DNS record removed.


*Casco is an autonomous security testing company. Our solution outperforms human-only pentesters by continuously monitoring your attack surface for issues before an attacker can exploit them.*
