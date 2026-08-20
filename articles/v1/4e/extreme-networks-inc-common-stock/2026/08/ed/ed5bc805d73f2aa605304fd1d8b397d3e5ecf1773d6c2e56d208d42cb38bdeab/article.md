---
schema_version: "1.0.0"
document_id: "ed5bc805d73f2aa605304fd1d8b397d3e5ecf1773d6c2e56d208d42cb38bdeab"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137696"
published_at: "2026-08-07T13:47:03+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:890f23a3bdd09f845a9d6b2a1c3eecfe519a07050133abe7c1a9592f532c24b2"
---

# SA-2026-088 - ExtremeXOS Privilege Escalation via Symlink Following in File Utilities (CVE-2026-8170)

The mv, cp, and rm file utilities exposed within the ExtremeXOS shell environment fail to safely canonicalize paths and follow symbolic links outside of the intended privilege boundary. An attacker with admin CLI access can create a symbolic link from a writable user directory to a privileged location (such as /dev) and then invoke the affected utilities to read, modify, or replace security-critical files outside of their authorized scope. Under certain conditions, this may enable escalation to root-level access and persistent modification of the device software stack. Exploitation is possible remotely by an attacker holding an admin account, or locally via the serial console.


Extreme would like to thank Hadrien Barral (Université Gustave Eiffel) and Georges-Axel Jaloyan (French Ministry of the Interior) for responsible disclosure of their findings.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
