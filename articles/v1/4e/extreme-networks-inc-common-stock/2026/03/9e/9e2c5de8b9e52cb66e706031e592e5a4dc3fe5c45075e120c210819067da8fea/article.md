---
schema_version: "1.0.0"
document_id: "9e2c5de8b9e52cb66e706031e592e5a4dc3fe5c45075e120c210819067da8fea"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000134765"
published_at: "2026-03-20T14:47:05+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:ab8073f6e2dbbdd3d4d9aad767df1d0d8fedf46861c257f29197fcb76960a90e"
---

# SA-2026-027 - glibc wordexp Uninitialized Memory (CVE-2025-15281)

Calling wordexp with WRDE_REUSE in conjunction with WRDE_APPEND in the GNU C Library may cause the interface to return uninitialized memory in the we_wordv member, which on subsequent calls to wordfree may abort the process.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
