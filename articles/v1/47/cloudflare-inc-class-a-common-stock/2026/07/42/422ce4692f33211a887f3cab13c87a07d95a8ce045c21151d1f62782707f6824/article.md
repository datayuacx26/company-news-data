---
schema_version: "1.0.0"
document_id: "422ce4692f33211a887f3cab13c87a07d95a8ce045c21151d1f62782707f6824"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/wordpress-vulnerabilities/"
published_at: "2026-07-17T21:30:43+00:00"
first_seen_at: "2026-07-20T03:32:14.182345+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:0987eb5fde9993fb14ef43d3805eefcde1e8e4c7ca3d4849640b09493d64f837"
---

# Cloudflare WAF protects WordPress applications from two high-severity vulnerabilities

Cloudflare has deployed new Web Application Firewall (WAF) protections for two critical vulnerabilities affecting WordPress. The protections address an Unauthenticated Remote Code Execution (RCE) vulnerability in WordPress's REST API and a related SQL Injection vulnerability.


The WordPress security team disclosed the vulnerabilities to Cloudflare before public release so that we could prepare protections for customers. **Cloudflare has deployed the new rules to protect all customers, including those on free and paid plans, as long as their application traffic is proxied through the Cloudflare WAF.** The rules were deployed at 17:03 UTC on July 17 2026.


WAF protections reduce exposure while customers update, but they are not a substitute for patching. WordPress has released fixes in version 7.0.2, with backports to affected earlier branches: 6.9.5, 6.8.6, and 7.1 Beta 2 ([see release details](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/) ). Versions earlier than 6.8 are not affected. WordPress is treating this as its highest-severity, highest-priority class of issue and is forcing automatic updates to affected sites, so most sites will be updated automatically. We still recommend confirming that you are on a patched release or the backports for your branch and follow the guidance in the official WordPress security release[announcement](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/) .


### What you need to know


The vulnerabilities affect different parts of the request path:


- **CVE-2026-60137: SQL injection.** A vulnerability in WordPress version 6.8 and later allows crafted input to alter a database query. Rating High.
- **CVE-2026-63030: Unauthenticated remote code execution.** A vulnerability in WordPress version 6.9 and later allows an unauthenticated attacker to execute code through the batch endpoint of the REST API when a persistent object cache is not in use. This vulnerability is related to the SQL injection described above. No login or user interaction is required to exploit this vulnerability. Rating Critical.


The SQL injection vulnerability is present from version 6.8 onwards, while the RCE only affects versions from 6.9. So 6.8.6 addresses the SQLi only since the RCE isn't present on 6.8, while 6.9.5, 7.0.2, and 7.1 Beta 2 get fixes for both.


Cloudflare created two rules to detect requests associated with these vulnerabilities:


**Rule description**


**CVE**


**Rule ID for Managed Ruleset**


**Rule ID for Free Ruleset**


**Default action**


Wordpress - SQL Injection - CVE:CVE-2026-60137


CVE-2026-60137


` 1c060d3a371549219ee290d7ed933fcc`


` db003b39b7774859a8d588ce33697a1a`


Block


Wordpress - Remote Code Execution - CVE:CVE-2026-63030


CVE-2026-63030


` 7dfb2bd4708d4b88b9911dc0550664b6`


` ebd3f2df15c74ddcbf6220c9b5ec246a`


Block


Cloudflare customers running WordPress sites on Pro, Business, or Enterprise plans should ensure that Cloudflare Managed Rules are enabled. Customers can follow the steps in our[WAF Managed Rules documentation](https://developers.cloudflare.com/waf/get-started/#1-deploy-the-cloudflare-managed-ruleset) . Customers on free plans are automatically protected through the Free Ruleset.


The new rules are deployed with the default Managed Ruleset action of Block. Customers running WordPress sites should review any ruleset-level overrides, including those that change all rules from Block to Log, and ensure the new rules use the recommended action while they update WordPress. Cloudflare customers should also monitor Security Events for requests matching either rule.


### Defense in depth while you patch


The SQL injection rule detects crafted parameter values before they reach WordPress. The unauthenticated RCE rule targets requests attempting to reach the remote code execution path. Together, they detect the attack at two different points.


These rules reduce risk while organizations update affected systems; they do not fix the underlying vulnerable code. Updating WordPress remains the most effective way to address the vulnerabilities.
If an immediate update is not possible, verify that both Cloudflare rules are active with the recommended action and review logs for suspicious requests to the affected REST API endpoint.


### Looking forward


Cloudflare will monitor matching traffic and test the rules against new attack variations, updating detections when needed.


We thank the WordPress security team for coordinating with Cloudflare and other infrastructure providers to help protect users before details of the vulnerabilities became public.
