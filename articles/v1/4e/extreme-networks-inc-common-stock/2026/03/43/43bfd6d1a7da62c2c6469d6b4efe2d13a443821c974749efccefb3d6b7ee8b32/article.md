---
schema_version: "1.0.0"
document_id: "43bfd6d1a7da62c2c6469d6b4efe2d13a443821c974749efccefb3d6b7ee8b32"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000134763"
published_at: "2026-03-20T14:42:04+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-07-22T19:09:52.015654+00:00"
content_hash: "sha256:ee48741427e3424969e047ff89d9a9fef3248f3561d8227bc411e5ea1627c409"
---

# SA-2026-029 - glibc getnetbyaddr DNS Stack Leak (CVE-2026-0915)

Calling getnetbyaddr or getnetbyaddr_r with a configured nsswitch.conf that specifies the library's DNS backend for networks and queries for a zero-valued network in the GNU C Library can leak stack contents to the configured DNS resolver.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
