---
schema_version: "1.0.0"
document_id: "3a2cf1e20d16cbf3a6982dda0f34b30dbf2a1382adb818dd71a9ea46f70e4d72"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-vpc-ipam-bgp-rpki-byoip/"
published_at: "2026-08-07T16:00:00+00:00"
first_seen_at: "2026-08-08T06:29:09.503565+00:00"
fetched_at: "2026-08-08T06:29:11.064011+00:00"
content_hash: "sha256:4266291d4526e6575c701d8b969ecdf1753bc26ab2fb3c54734d7d16b41f3813"
---

# Amazon VPC IPAM now supports BGP route protection monitoring and delegated RPKI for BYOIP prefixes

Amazon Virtual Private Cloud (VPC) IP Address Manager (IPAM) now supports BGP route protection monitoring and delegated Resource Public Key Infrastructure (RPKI) management for Bring Your Own IP (BYOIP) prefixes. Network administrators can centrally monitor BGP route protection and automate Route Origin Authorization (ROA) management across their organization.


Using BGP route monitoring, you can view RPKI validity status, ROA strength, and route overlap detection for all BYOIP prefixes across accounts and regions from a single dashboard. Administrators can identify prefixes with invalid or missing ROAs, detect route overlaps that may indicate hijacking, and distinguish between strict and permissive ROA configurations. With Delegated RPKI, administrators perform a one-time setup with their Regional Internet Registry (ARIN, RIPE, APNIC, or LACNIC), after which IPAM automatically creates ROAs during BYOIP provisioning, renews them before expiration, and manages ROAs for on-premises prefixes. Before this feature, customers had to manually create and renew ROAs at their Regional Internet Registry (RIR), validate ownership through WHOIS or DNS records, and rely on third-party tools to monitor route security.


The feature is available within Amazon VPC IPAM in all commercial AWS Regions, excluding the AWS GovCloud (US) Regions, and China (Beijing, operated by Sinnet) and China (Ningxia, operated by NWCD). To get started, please see the[BGP route protection documentation](https://docs.aws.amazon.com/vpc/latest/ipam/monitor-bgp-route-security.html) . To learn more about IPAM, view the[IPAM documentation](https://docs.aws.amazon.com/vpc/latest/ipam/what-it-is-ipam.html) . For details on pricing, refer to the IPAM tab on the[Amazon VPC Pricing Page](https://aws.amazon.com/vpc/pricing/) .
