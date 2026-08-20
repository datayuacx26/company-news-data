---
schema_version: "1.0.0"
document_id: "aaee7905b80c15aaa53b409d5bf385749c38456cd301cb6b73d4ffe1837ec387"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000134762"
published_at: "2026-03-20T14:41:18+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-07-22T19:09:52.015654+00:00"
content_hash: "sha256:ce5198fcf8c32cd1bd6aab7c83690de4ee2ce58b089e4027a4cfba5c56071239"
---

# SA-2026-021 - OpenSSL NULL Pointer DoS (CVE-2025-69421)

Processing a malformed PKCS#12 file can trigger a NULL pointer dereference in the PKCS12_item_decrypt_d2i_ex() function. A NULL pointer dereference can trigger a crash which leads to Denial of Service for an application processing PKCS#12 files. The PKCS12_item_decrypt_d2i_ex() function does not check whether the oct parameter is NULL before dereferencing it. When called from PKCS12_unpack_p7encdata() with a malformed PKCS#12 file, this parameter can be NULL, causing a crash. The vulnerability is limited to Denial of Service and cannot be escalated to achieve code execution or memory disclosure. Exploiting this issue requires an attacker to provide a malformed PKCS#12 file to an application that processes it.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
