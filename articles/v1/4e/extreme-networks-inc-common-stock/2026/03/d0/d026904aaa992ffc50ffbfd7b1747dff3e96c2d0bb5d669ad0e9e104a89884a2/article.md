---
schema_version: "1.0.0"
document_id: "d026904aaa992ffc50ffbfd7b1747dff3e96c2d0bb5d669ad0e9e104a89884a2"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000134761"
published_at: "2026-03-20T14:40:04+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-07-22T19:09:52.015654+00:00"
content_hash: "sha256:428e0ace902611006ac24054cf1dca8b88d4c0a446f838cc3068b83348f63c44"
---

# SA-2026-022 - OpenSSL PKCS#12 BMPString Corruption (CVE-2025-69419)

Calling PKCS12_get_friendlyname() function on a maliciously crafted PKCS#12 file with a BMPString (UTF-16BE) friendly name containing non-ASCII BMP code point can trigger a one byte write before the allocated buffer. The out-of-bounds write can cause a memory corruption which can have various consequences including a Denial of Service.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
