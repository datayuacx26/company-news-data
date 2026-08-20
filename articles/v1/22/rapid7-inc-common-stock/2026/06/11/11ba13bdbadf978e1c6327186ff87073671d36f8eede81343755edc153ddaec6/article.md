---
schema_version: "1.0.0"
document_id: "11ba13bdbadf978e1c6327186ff87073671d36f8eede81343755edc153ddaec6"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-weekly-metasploit-update-modules-for-audiobookshelf-litellm-next-js-dalfox-and-more"
published_at: "2026-06-26T19:32:52+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:fd61ee99c28092ef87ff6704e5c352551070f4e649e2e90cb0982f6491604f3a"
---

# Weekly Metasploit Update: Modules for Audiobookshelf, LiteLLM, Next.js, Dalfox and more

## Help shape the future of Metasploit Framework


We are planning future work in relation to the evasion capabilities present in Metasploit Framework, and how they function/are presented to users. We are currently accepting responses to our feedback form, which means that you can shape the future of how evasive capabilities are implemented in Metasploit Framework. The proposal for the changes can be found[here](https://gist.github.com/smcintyre-r7/09488f45904d73ff0ce0d5a7f7e5a830) , and you can submit your responses to the form[here](https://docs.google.com/forms/d/e/1FAIpQLSfa1JVJzqrQ2lh9a0peW8VGs3pNSb47vw5RJWVicfiQU5bpDg/viewform?usp=publish-editor) . The form will stop accepting responses on the 1st of July, 2026.


New module content and improvements have also been added this week. This includes a Next.js Middleware Authorization Bypass scanner, LiteLLM Proxy SQL Injection, an unauthenticated API authentication bypass scanner for Audiobookshelf, a deserialization RCE in Dalfox, and improvements to service and host reporting in bruteforce-related modules.


## New module content (4)


### Audiobookshelf Unauthenticated API Authentication Bypass Scanner


Authors: Kenneth LaCroix and swiftbird07


Type: Auxiliary


Pull request:[#21565](https://github.com/rapid7/metasploit-framework/pull/21565) contributed by[kenlacroix](https://github.com/kenlacroix)


Path: scanner/http/audiobookshelf_auth_bypass


AttackerKB reference:[CVE-2025-25205](https://attackerkb.com/search?q=CVE-2025-25205&referrer=blog)


Description: Adds audiobookshelf_auth_bypass, a detection module for CVE-2025-25205 — an unauthenticated API authentication bypass in Audiobookshelf (self-hosted audiobook/podcast server), affecting versions 2.17.0 – 2.19.0 (fixed in 2.19.1).


### BerriAI LiteLLM Proxy Pre-Auth SQL Injection Scanner


Authors: Kenneth LaCroix and Tencent YunDing Security Lab


Type: Auxiliary


Pull request:[#21567](https://github.com/rapid7/metasploit-framework/pull/21567) contributed by[kenlacroix](https://github.com/kenlacroix)


Path: scanner/http/litellm_proxy_sqli


AttackerKB reference:[CVE-2026-42208](https://attackerkb.com/search?q=CVE-2026-42208&referrer=blog)


Description: Adds auxiliary/scanner/http/litellm_proxy_sqli, a detection module for CVE-2026-42208 (CVSS 9.3, on the CISA KEV list) — a pre-authentication SQL injection in BerriAI LiteLLM proxy.


### Next.js Middleware Authorization Bypass Scanner


Authors: Kenneth LaCroix, Rachid Allam, and Yasser Allam


Type: Auxiliary


Pull request:[#21566](https://github.com/rapid7/metasploit-framework/pull/21566) contributed by[kenlacroix](https://github.com/kenlacroix)


Path: scanner/http/nextjs_middleware_auth_bypass


AttackerKB reference:[CVE-2025-29927](https://attackerkb.com/search?q=CVE-2025-29927&referrer=blog)


Description: Adds nextjs_middleware_auth_bypass, a detection module for CVE-2025-29927 (CVSS 9.1) — an authorization bypass in self-hosted Next.js applications.


### Dalfox Found-Action Deserialization RCE


Authors: Emmanuel David and Takahiro Yokoyama


Type: Exploit


Pull request:[#21493](https://github.com/rapid7/metasploit-framework/pull/21493) contributed by[Takahiro-Yoko](https://github.com/Takahiro-Yoko)


Path: linux/http/dalfox_server_rce_cve_2026_45087


AttackerKB reference:[CVE-2026-45087](https://attackerkb.com/search?q=CVE-2026-45087&referrer=blog)


Description: This adds an exploit module for Dalfox Server versions <= 2.12.0 which are vulnerable to an unauthenticated RCE tracked as CVE-2026-45087. The vulnerability allows attackers to send arbitrary commands via found-action post parameter which gets deserialized and run in the context of the user running the server.


## Enhancements and features (2)


- [#21396](https://github.com/rapid7/metasploit-framework/pull/21396) from[g0tmi1k](https://github.com/g0tmi1k) - This makes improvements to the auth_brute mixin. It adds report_host and report_service calls to the mixin and removes duplicate printing of IP:PORT in the print_brute statements.
- [#21562](https://github.com/rapid7/metasploit-framework/pull/21562) from[zeroSteiner](https://github.com/zeroSteiner) - Updated the usage of rex-socket's recvfrom method to align with the standard library implementation. This also allows rex-socket to now be used as a drop-in replacement for Ruby's UDPSocket.


## Documentation


You can find the latest Metasploit documentation on our docsite at[docs.metasploit.com](https://docs.metasploit.com/) .


## Get it


As always, you can update to the latest Metasploit Framework with msfupdate and you can get more details on the changes since the last blog post from GitHub:


- [Pull Requests 6.4.140...6.4.141](https://github.com/rapid7/metasploit-framework/pulls?q=is:pr+merged:%222026-06-22T13%3A23%3A28%2B01%3A00..2026-06-24T23%3A18%3A10Z%22)
- [Full diff 6.4.140...6.4.141](https://github.com/rapid7/metasploit-framework/compare/6.4.140...6.4.141)


If you are a git user, you can clone the[Metasploit Framework repo](https://github.com/rapid7/metasploit-framework) (master branch) for the latest. To install fresh without using git, you can use the open-source-only[Nightly Installers](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) or the commercial edition[Metasploit Pro](https://www.rapid7.com/products/metasploit/download/)
