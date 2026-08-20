---
schema_version: "1.0.0"
document_id: "a961c50aed38951eaceaca4fe476e586d30dd06d7b08b32d12fe4efbb22e778a"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137221"
published_at: "2026-07-01T17:31:12+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:ec15abd23ef367cd76c6d253d407178216be868dcb5cb0fb69986d74fb51ed9a"
---

# SA-2026-045 - Linux Kernel LPE RxRPC Page-Cache Write (CVE-2026-43500)

A flaw was found in the Linux kernel's RxRPC networking subsystem. When a non-linear socket buffer carrying a splice-pinned page-cache reference reaches the RxRPC authentication verification path, the kernel performs an in-place pcbc(fcrypt) decryption directly on the referenced page-cache page without first isolating the buffer with skb_copy(). An unprivileged local attacker can exploit this behavior to corrupt the page-cache contents of readable files, including sensitive system files such as /etc/passwd, and obtain root privileges. Unlike the ESP/XFRM variant, exploitation does not require unprivileged user or network namespaces, but depends on the RxRPC protocol stack (rxrpc.ko) being available on the target system.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
