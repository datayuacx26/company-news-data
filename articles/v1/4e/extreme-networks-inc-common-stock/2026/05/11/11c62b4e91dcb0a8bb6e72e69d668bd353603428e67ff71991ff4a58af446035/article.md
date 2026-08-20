---
schema_version: "1.0.0"
document_id: "11c62b4e91dcb0a8bb6e72e69d668bd353603428e67ff71991ff4a58af446035"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000136573"
published_at: "2026-05-29T17:43:16+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:f3b9b5e7c2c20759e2e5ee051390c17efec6777a441ed4bb21513ace0ab8cbcc"
---

# SA-2026-048 - ExtremeCloud IQ Cross-Tenant Data Exposure via Extreme Platform One Authentication Race Condition (CVE-2026-9831)

A race condition in the shared Extreme Platform ONE IAM Gateway API-key authentication path could, under specific high-concurrency traffic conditions, intermittently allow requests authenticated with an Extreme Platform ONE/IAM-issued API key to receive response data for another tenant. The issue was observed through ExtremeCloud IQ/XIQ API endpoints and validated against both XIQ/XAPI and Extreme Platform ONE/Common Services API paths. XIQ-native tokens and standard OAuth/Bearer JWT authentication were not affected.


Extreme Networks thanks Sebastian Koller of Iteas IT Services GmbH for the responsible disclosure of this issue.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
