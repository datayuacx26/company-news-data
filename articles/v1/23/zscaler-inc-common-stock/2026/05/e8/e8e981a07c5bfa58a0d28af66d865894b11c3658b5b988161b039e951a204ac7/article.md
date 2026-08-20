---
schema_version: "1.0.0"
document_id: "e8e981a07c5bfa58a0d28af66d865894b11c3658b5b988161b039e951a204ac7"
company_key: "zscaler-inc-common-stock"
company: "Zscaler Inc."
source_id: "zscaler-inc-common-stock-news-import-d539219e7401"
canonical_url: "https://www.zscaler.com/blogs/product-insights/secure-sap-s-4hana-migration-top-4-challenges-companies-must-address"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-22T21:12:02.518827+00:00"
fetched_at: "2026-07-28T22:15:29.906392+00:00"
content_hash: "sha256:6a6d1f51a616dacd0b341b29116a7faa2132fdccf7283fdb7802ad29b0499fb0"
---

# Secure SAP S/4HANA Migration: Top 4 Challenges Companies Must Address

## Challenge #3: Secure the connectivity between S/4HANA and manufacturing floors


SAP S/4HANA requires connectivity to manufacturing floors to bridge the gap between high-level business planning and physical, real-time shop-floor execution. This hybrid approach allows companies to leverage the speed and innovation of the cloud while maintaining control over sensitive, real-time production data.


Relying on private networks, site-to-site VPNs, or firewalls to secure this connectivity can enable lateral threat movement from a compromised device to SAP-connected services. Companies need to enforce one-to-one, least-privileged connectivity without disrupting production.


Consider the risk introduced by a seemingly benign device, such as a networked printer on the factory floor. While these devices require connectivity to SAP S/4HANA to facilitate real-time production labeling and reporting, they are often notorious for unpatched vulnerabilities and weak security controls. When connected via traditional site-to-site VPNs or legacy firewalls, the printer is typically placed on a trusted network segment. If an attacker compromises this printer, the broad, network-level access provided by the VPN acts as an open corridor, allowing the threat to move laterally from the shop floor directly into the core SAP environment. This vulnerability highlights why organizations can no longer rely on 'flat' network connectivity; instead, they must enforce one-to-one, application-level, least-privileged access that ensures a compromise at the edge cannot jeopardize critical business operations.


Figure 4: Unreliable connectivity between SAP S/4HANA and manufacturing floors
