---
schema_version: "1.0.0"
document_id: "1d69a39c73a5dd1be429ca2e91472fcfebecfb2a3ea8fe42e667b2d00ab783bc"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137801"
published_at: "2026-07-22T19:13:46+00:00"
first_seen_at: "2026-07-22T20:09:57.178797+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:0ae692969a8a428ba6e31fa2aaf4f134687c4a812fa2c50c4e8f23c95e495378"
---

# SA-2026-053 - libpng ARM NEON palette expansion OOB (CVE-2026-33636)

LIBPNG is a reference library for use in applications that read, create, and manipulate PNG (Portable Network Graphics) raster image files. An out-of-bounds read and write exists in libpng's ARM/AArch64 Neon-optimized palette expansion path. When expanding 8-bit paletted rows to RGB or RGBA, the Neon loop processes a final partial chunk without verifying that enough input pixels remain. Because the implementation works backward from the end of the row, the final iteration dereferences pointers before the start of the row buffer (OOB read) and writes expanded pixel data to the same underflowed positions (OOB write). This is reachable via normal decoding of attacker-controlled PNG input if Neon is enabled.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
