---
schema_version: "1.0.0"
document_id: "607e502c5811ced7d7f8ed2ba39a3089093776becc74518e88d2f78679f8526f"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137800"
published_at: "2026-07-22T19:14:45+00:00"
first_seen_at: "2026-07-22T20:09:57.178797+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:1c5dfc15c4f918472005972f77f8aa7f94ec3b5d0c9bd8fcaab2e4877751ed28"
---

# SA-2026-052 - libcurl stale Host header cookie leakage (CVE-2026-6276)

A vulnerability in libcurl allows stale Host header state to persist across requests when reusing the same easy handle. If a custom Host: header is set for an initial request and not explicitly cleared before a subsequent request to a different host, libcurl may incorrectly include cookies associated with the original host in the follow-up request. This can result in unintended cross-domain cookie leakage, potentially exposing sensitive session information under specific application configurations that reuse connections and manipulate request headers.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
