---
schema_version: "1.0.0"
document_id: "2cf8a71474135732793a33f5f2d182a6ceb895d1a28a354415ae33fd5c2bed45"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000123244"
published_at: "2026-06-25T17:25:08+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:b53b900296119d3c38beee83a7050b4ac3dd770ca1de2a3b45a11a7309f5d3e0"
---

# SA-2024-035 - Linux kernel netfilter use-after-free (CVE-2024-1086)

Use-after-free in the Linux kernel's netfilter: nf_tables component to achieve local privilege escalation. The nft_verdict_init() function allows positive values as drop errors within the hook verdict, and hence the nf_hook_slow() function can cause a double free issue when NF_DROP is issued with a drop error that resembles NF_ACCEPT.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
