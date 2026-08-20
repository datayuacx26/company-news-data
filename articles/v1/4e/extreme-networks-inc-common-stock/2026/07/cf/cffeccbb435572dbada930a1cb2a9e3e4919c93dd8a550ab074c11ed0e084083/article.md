---
schema_version: "1.0.0"
document_id: "cffeccbb435572dbada930a1cb2a9e3e4919c93dd8a550ab074c11ed0e084083"
company_key: "extreme-networks-inc-common-stock"
company: "Extreme Networks Inc."
source_id: "extreme-networks-inc-common-stock-rss-0242d87c651c"
canonical_url: "https://extreme-networks.my.site.com/ExtrArticleDetail?an=000137694"
published_at: "2026-07-17T17:53:54+00:00"
first_seen_at: "2026-07-20T04:35:47.988677+00:00"
fetched_at: "2026-08-20T00:38:33.527756+00:00"
content_hash: "sha256:deea6c58f879d3ef7e70c5f42a509d1b31ac59b826816b620291cc99f92f2d31"
---

# SA-2026-087 - ExtremeXOS Debug-Mode Privilege Escalation via Weak PRNG (CVE-2026-8169)

ExtremeXOS uses a challenge-response mechanism to authorize access to the privileged debug-mode function. The challenge value is generated using an insufficiently random source, which under certain conditions may allow an attacker to predict the expected response and activate debug-mode without authorization. Depending on device configuration and version, this may enable escalation to root-level access and persistent modification of the device software stack. Exploitation requires either a valid low-privilege account on the device (remote scenario) or physical serial console access (local scenario).


Extreme would like to thank Hadrien Barral (Université Gustave Eiffel) and Georges-Axel Jaloyan (French Ministry of the Interior) for responsible disclosure of their findings.


Products not listed in the Impact Details section have not been evaluated. Furthermore, products that have exceeded any software maintenance time periods are also not evaluated and will not be published. Please consult[End of Sale and End of Service Life - Extreme Networks](https://www.extremenetworks.com/support/end-of-sale-and-end-of-support-products/) for the EOL notices related to the product under question.
