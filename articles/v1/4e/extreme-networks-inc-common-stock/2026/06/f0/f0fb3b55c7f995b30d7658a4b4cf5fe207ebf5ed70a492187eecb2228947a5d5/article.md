---
schema_version: "1.0.0"
document_id: "f0fb3b55c7f995b30d7658a4b4cf5fe207ebf5ed70a492187eecb2228947a5d5"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137150"
published_at: "2026-06-25T17:27:05+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:aa8567a023f38338ff921eb321d7fb9a431148629af89c39a10aa9d40f4ec713"
---

# SA-2026-049 - nghttp2 HTTP2 Assertion Failure DoS (CVE-2026-27135)

nghttp2 is an implementation of the Hypertext Transfer Protocol version 2 in C. The nghttp2 library stops reading the incoming data when user facing public API \`nghttp2_session_terminate_session\` or \`nghttp2_session_terminate_session2\` is called by the application. They might be called internally by the library when it detects the situation that is subject to connection error. Due to the missing internal state validation, the library keeps reading the rest of the data after one of those APIs is called. Then receiving a malformed frame that causes FRAME_SIZE_ERROR causes assertion failure.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
