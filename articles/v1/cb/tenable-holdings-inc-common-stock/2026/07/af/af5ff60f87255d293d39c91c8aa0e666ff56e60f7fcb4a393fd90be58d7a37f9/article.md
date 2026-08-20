---
schema_version: "1.0.0"
document_id: "af5ff60f87255d293d39c91c8aa0e666ff56e60f7fcb4a393fd90be58d7a37f9"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/wp2shell-cve-2026-63030-cve-2026-60137-frequently-asked-questions-about-remote-code-execution"
published_at: "2026-07-20T13:36:32+00:00"
first_seen_at: "2026-07-24T03:36:59.179818+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:7719ec97b38824d2f8480cf2e55ef5b49a6163a07752b8f0f372163eb5c4127e"
---

# wp2shell: WordPress Core Pre-Auth RCE FAQ | Tenable®

**An unauthenticated attacker can chain two WordPress Core vulnerabilities, CVE-2026-63030 and CVE-2026-60137, to achieve remote code execution against affected WordPress installations. Multiple security firms have confirmed active in-the-wild exploitation within days of public disclosure, and public proof-of-concept exploits are circulating.**


## Key takeaways:


1. Two WordPress Core vulnerabilities, CVE-2026-63030 and CVE-2026-60137, can be chained together to achieve pre-authentication remote code execution against WordPress 6.9.x and 7.0.x installations.


2. Multiple security firms have confirmed in-the-wild exploitation, with public proof-of-concept exploits appearing within hours of the July 17, 2026 disclosure.


3. Patches are available in WordPress 7.0.2 and 6.9.5; WordPress.org has enabled forced automatic updates across affected supported installations.


## Background


Tenable's Research Special Operations (RSO) team has compiled this blog to answer Frequently Asked Questions (FAQ) regarding wp2shell, two vulnerabilities in WordPress Core that can be chained together to achieve pre-authentication remote code execution.


## FAQ


**What is wp2shell?**


wp2shell is the name given to two vulnerabilities in WordPress Core.


**When was wp2shell first disclosed?**


On July 17, 2026, WordPress released security updates addressing the wp2shell vulnerabilities alongside two GitHub Security Advisories. Adam Kues of Searchlight Cyber, who discovered and disclosed CVE-2026-63030,[published research on the same day](https://slcyber.io/research-center/wp2shell-pre-authentication-rce-in-wordpress-core/) and chose to hold back the technical specifics given the severity of the finding. Searchlight Cyber also launched[wp2shell.com](https://wp2shell.com/) , a testing tool that allows administrators to check whether their WordPress installation is vulnerable. On July 20, Searchlight Cyber published a[full technical breakdown](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/) of the attack chain.


**What are the vulnerabilities associated with wp2shell?**


wp2shell is a two-vulnerability exploit chain affecting WordPress Core.


**CVE** **Description** **CVSSv3**


[CVE-2026-63030](https://www.tenable.com/cve/CVE-2026-63030) WordPress Core REST API Batch-Route Confusion Remote Code Execution Vulnerability 9.8


[CVE-2026-60137](https://www.tenable.com/cve/CVE-2026-60137) WordPress Core WP_Query author__not_in SQL Injection Vulnerability 5.9


CVE-2026-63030 is a REST API batch-route confusion weakness introduced in WordPress 6.9. CVE-2026-60137 is a SQL injection flaw in the *author__not_in* parameter of *WP_Query* , present in WordPress 6.8 and later. When chained on WordPress 6.9.0 through 7.0.1, the two flaws allow an unauthenticated attacker to reach the REST API batch endpoint at */wp-json/batch/v1* and achieve remote code execution. CVE-2026-60137 was discovered and disclosed by security researchers TF1T, dtro, and haongo.


CVE-2026-60137 also affects WordPress 6.8.0 through 6.8.5 as a standalone SQL injection issue. Because CVE-2026-63030 was introduced in WordPress 6.9, the full RCE chain is only achievable on 6.9.x and 7.0.x installations.


**How severe is the wp2shell vulnerability chain?**


An anonymous, unauthenticated user can execute the chain against a default WordPress installation with no plugins required. No preconditions exist beyond the default WordPress configuration. Cloudflare[notes](https://blog.cloudflare.com/wordpress-vulnerabilities/) that the vulnerable code path is reached when “a persistent object cache is not in use.”


Note: wp2shell targets WordPress Core itself rather than a plugin or theme. All four prior WordPress-related entries in the CISA Known Exploited Vulnerabilities (KEV) catalog involve plugins, not core. Pre-authentication remote code execution in WordPress Core is uncommon.


**CVE** **Product** **Added to KEV** **Ransomware**


CVE-2026-41940 WebPros cPanel & WHM and WP2 (WordPress Squared) April 30, 2026 Known


CVE-2020-25213[WordPress File Manager Plugin](https://www.tenable.com/blog/critical-vulnerability-in-file-manager-wordpress-plugin-exploited-in-the-wild) November 3, 2021 Unknown


CVE-2020-11738 WordPress Snap Creek Duplicator Plugin November 3, 2021 Unknown


CVE-2019-9978 WordPress Social Warfare Plugin November 3, 2021 Unknown


**How widespread are the attacks exploiting wp2shell?**


WordPress is the most widely deployed content management system in the world. Some hosted installations will receive patches automatically from their hosting providers; many self-managed installations will not.


Hexastrike began observing exploitation attempts in honeypots over the weekend following the July 17 disclosure and has since assisted with incident response in several confirmed attacks. Patchstack has also confirmed in-the-wild exploitation. Other researchers have reported seeing active exploitation in the wild.


> It's starting. Seeing first signs of wp2shell RCE exploit actually being used in the wild -[pic.twitter.com/VkNcCVwcLV](https://t.co/VkNcCVwcLV)
>
>
> — rahul (@rahulgovind517)[July 20, 2026](https://x.com/rahulgovind517/status/2078998010969866538?ref_src=twsrc%5Etfw)


**Which threat actors are exploiting wp2shell?**


As of July 20, 2026, no specific threat actor or group has been publicly attributed to wp2shell exploitation. This post will be updated if attribution becomes available.


**Is there a proof-of-concept available for wp2shell?**


Yes. Multiple public proof-of-concept (PoC) exploits appeared on GitHub within hours of the July 17 disclosure. The presence of these PoCs is being attributed to AI-assisted tooling, which makes patch diffing and exploit development easier for both defenders and attackers. Kues confirmed this directly in[Searchlight Cyber's technical blog](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/) , stating that "no security researcher could have found and completed this exploit chain in 10 hours without AI."


> Seems that wp2shell PoCs are now floating around the internet, so we've published our blog post including our research methodology for finding the bug as well as a deep dive into the chain itself -[https://t.co/iuU0yiYJBT](https://t.co/iuU0yiYJBT)
>
>
> — hashkitten (@hash_kitten)[July 20, 2026](https://x.com/hash_kitten/status/2079116692375089641?ref_src=twsrc%5Etfw)


**Are patches or mitigations available for wp2shell?**


Yes. Patches were released on July 17, 2026. WordPress.org has enabled forced automatic updates for supported installations running affected versions.


**WordPress Branch** **Affected Versions** **Fixed Versions** **Applicable CVE(s)**


6.8.x 6.8.0 - 6.8.5 6.8.6 CVE-2026-60137


6.9.x 6.9.0 - 6.9.4 6.9.5 CVE-2026-63030, CVE-2026-60137


7.0.x 7.0.0 - 7.0.1 7.0.2 CVE-2026-63030, CVE-2026-60137


7.1 beta 7.1 beta 7.1 beta2 CVE-2026-63030, CVE-2026-60137


For installations that cannot immediately update, Searchlight Cyber offers three temporary options.


1. Install a plugin that blocks unauthenticated users from accessing the REST API
2. Block **/wp-json/batch/v1** and **?rest_route=/batch/v1** at the web application firewall (WAF) level; ensure both patterns are covered
3. Deploy a custom PHP plugin available on[wp2shell.com](https://wp2shell.com/) that restricts unauthenticated access to the batch endpoint specifically


All three are interim measures and are not a substitute for applying the available patches.


Cloudflare has[deployed WAF rules covering both CVE-2026-63030 and CVE-2026-60137](https://blog.cloudflare.com/wordpress-vulnerabilities/) across all plans, including free accounts, for sites proxied through its platform.


**Are there any indicators of compromise for wp2shell?**


As of July 20, 2026, no specific indicators of compromise (IoCs) have been publicly released for wp2shell exploitation. This post will be updated if IoCs become publicly available.


**Has Tenable Research classified wp2shell as part of Vulnerability Watch?**


Yes. Tenable Research has classified CVE-2026-63030 and CVE-2026-60137 as part of[Vulnerability Watch](https://www.tenable.com/blog/reducing-remediation-time-remains-a-challenge-how-tenable-vulnerability-watch-can-help) , and both CVEs have been tagged as a Vulnerability of Interest. We are actively monitoring exploitation activity and tracking new developments. We will update this post as additional information becomes available.


**Has Tenable released product coverage for wp2shell?**


A list of Tenable plugins for these vulnerabilities can be found on the individual CVE pages:


- [CVE-2026-63030](https://www.tenable.com/cve/CVE-2026-63030/plugins)
- [CVE-2026-60137](https://www.tenable.com/cve/CVE-2026-60137/plugins)


These links will display all available plugins for these vulnerabilities, including upcoming plugins in our[Plugins Pipeline](https://www.tenable.com/plugins/pipeline) .


### Get more information


- [Searchlight Cyber: wp2shell Pre-Authentication RCE in WordPress Core](https://slcyber.io/research-center/wp2shell-pre-authentication-rce-in-wordpress-core/)
- [Searchlight Cyber: Technical Analysis of the wp2shell Attack Chain](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/)
- [WordPress 7.0.2 Security Release](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/)
- [GitHub Advisory: GHSA-ff9f-jf42-662q (CVE-2026-63030)](https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-ff9f-jf42-662q)
- [GitHub Advisory: GHSA-fpp7-x2x2-2mjf (CVE-2026-60137)](https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-fpp7-x2x2-2mjf)


***Join***[Tenable's Research Special Operations (RSO) Team](https://connect.tenable.com/category/news-you-need/discussions/vulnerability-watch) ***on Tenable Connect for further discussions on the latest cyber threats.***


***Learn more about***[Tenable One](https://www.tenable.com/products/tenable-one) ***, the Exposure Management Platform for the modern attack surface.***
