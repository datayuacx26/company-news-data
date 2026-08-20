---
schema_version: "1.0.0"
document_id: "9bf8c2332d83f1b495e1fcd0ed9b47b172daff30a76d7f49cae82168a867135b"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000134759"
published_at: "2026-06-04T12:17:55+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:718f961b2cd419d18225e23237843ee38dc2d94ec8ef3a0e59371724dc01fa1a"
---

# SA-2026-007 - OpenSSL CMS AuthEnvelopedData Buffer Overflow (CVE-2025-15467)

Parsing CMS AuthEnvelopedData message with maliciously crafted AEAD parameters can trigger a stack buffer overflow. A stack buffer overflow may lead to a crash, causing Denial of Service, or potentially remote code execution. When parsing CMS AuthEnvelopedData structures that use AEAD ciphers such as AES-GCM, the IV (Initialization Vector) encoded in the ASN.1 parameters is copied into a fixed-size stack buffer without verifying that its length fits the destination. An attacker can supply a crafted CMS message with an oversized IV, causing a stack-based out-of-bounds write before any authentication or tag verification occurs. Applications and services that parse untrusted CMS or PKCS#7 content using AEAD ciphers (e.g., S/MIME AuthEnvelopedData with AES-GCM) are vulnerable. Because the overflow occurs prior to authentication, no valid key material is required to trigger it. While exploitability to remote code execution depends on platform and toolchain mitigations, the stack-based write primitive represents a severe risk.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
