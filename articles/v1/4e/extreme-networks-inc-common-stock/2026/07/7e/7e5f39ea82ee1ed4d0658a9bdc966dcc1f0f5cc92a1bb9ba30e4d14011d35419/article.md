---
schema_version: "1.0.0"
document_id: "7e5f39ea82ee1ed4d0658a9bdc966dcc1f0f5cc92a1bb9ba30e4d14011d35419"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137802"
published_at: "2026-07-22T19:11:26+00:00"
first_seen_at: "2026-07-22T20:09:57.178797+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:bfb144106a698fcff302ad6e57dcb79f25d9b51d6461b88293dd30bec1153c73"
---

# SA-2026-056 - Linux Kernel "ssh‑keysign‑pwn" ptrace ACL (CVE-2026-46333)

Linux kernel vulnerability in the ptrace access-control logic that allows a local, unprivileged user to bypass security checks during the brief termination phase of a privileged process. Due to a race condition, the kernel releases the process’s memory context before closing its open file descriptors, enabling an attacker to access sensitive resources such as SSH private keys or password hashes and, in some cases, execute actions with elevated privileges. The issue affects a wide range of Linux distributions and is mitigated by applying updated kernel patches or restricting ptrace functionality.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
