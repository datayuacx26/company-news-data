---
schema_version: "1.0.0"
document_id: "2975ab35f886f42be8db08b55e2db6df1e8b06c8359509c3ab81fdc93b6ebe87"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137799"
published_at: "2026-07-22T19:15:13+00:00"
first_seen_at: "2026-07-22T20:09:57.178797+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:f45f31893a563d5a4c7a5f5dfb4d98aeb65516835cfc7ceb9552d87d7c726f9a"
---

# SA-2026-051 - libpng Use-After-Free in tRNS PLTE Handling (CVE-2026-33416)

LIBPNG is a reference library for use in applications that read, create, and manipulate PNG (Portable Network Graphics) raster image files. \`png_set_tRNS\` and \`png_set_PLTE\` each alias a heap-allocated buffer between \`png_struct\` and \`png_info\`, sharing a single allocation across two structs with independent lifetimes. The \`trans_alpha\` aliasing has been present, and the \`palette\` aliasing. Both affect all prior release lines \`png_set_tRNS\` sets \`png_ptr->trans_alpha = info_ptr->trans_alpha\` (256-byte buffer) and \`png_set_PLTE\` sets \`info_ptr->palette = png_ptr->palette\` (768-byte buffer). In both cases, calling \`png_free_data\` (with \`PNG_FREE_TRNS\` or \`PNG_FREE_PLTE\`) frees the buffer through \`info_ptr\` while the corresponding \`png_ptr\` pointer remains dangling. Subsequent row-transform functions dereference and, in some code paths, write to the freed memory. A second call to \`png_set_tRNS\` or \`png_set_PLTE\` has the same effect, because both functions call \`png_free_data\` internally before reallocating the \`info_ptr\` buffer.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
