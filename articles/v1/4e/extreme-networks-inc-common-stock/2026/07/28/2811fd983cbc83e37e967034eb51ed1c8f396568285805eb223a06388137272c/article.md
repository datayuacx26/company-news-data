---
schema_version: "1.0.0"
document_id: "2811fd983cbc83e37e967034eb51ed1c8f396568285805eb223a06388137272c"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137220"
published_at: "2026-07-01T17:30:41+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:be5935a2fd0beac8239f789dcdb8686fa0fdbda8809065e9695a4b2ad90dd1a0"
---

# SA-2026-044 - Linux Kernel "Dirty Frag" xfrm ESP IPSec (CVE-2026-43284)

The “Dirty Frag” vulnerability is a local privilege escalation (LPE) issue in the Linux kernel that combines flaws in the ESP/XFRM and RXRPC subsystems to allow an unprivileged local attacker to gain root access on major Linux distributions (using any of these two: ESP/XFRM or RXRPC flaws). The attack abuses kernel page-cache manipulation and network protocol handling to overwrite privileged binaries and execute arbitrary code with elevated privileges. Exploitation differs by distribution: the ESP issue affects systems permitting unprivileged user namespaces, while the RXRPC issue impacts distributions with RXRPC enabled, such as Ubuntu. Together, the vulnerabilities provide broad cross-distribution root compromise capability, with mitigations involving disabling vulnerable kernel modules (esp4, esp6, and rxrpc) until upstream patches are fully merged and deployed.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
