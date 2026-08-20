---
schema_version: "1.0.0"
document_id: "9c3c562f599297bee82c2b5b9a66a35b149be42b1b0b8b2468f662189d8c8a8e"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-direct-connect-bgp-visibility/"
published_at: "2026-07-30T20:21:00+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:4f7247f39d71516a6b9c42c6611e4fda8be0f867fcfe4c61b6a243210e0d3f17"
---

# AWS Direct Connect now supports BGP route visibility on Virtual Interfaces

AWS Direct Connect now provides Border Gateway Protocol (BGP) route visibility, allowing you to view the routes exchanged between AWS and your on-premises routers across your private, transit, and public virtual interfaces (VIFs). You can now see which routes AWS accepted from your router and which routes AWS is advertising to your router, along with their AS path and BGP community values. This visibility helps network administrators troubleshoot routing issues, verify route propagation, and monitor their hybrid network connectivity.


With this feature, you can view accepted routes (routes AWS received from your router) and advertised routes (routes AWS sends to your router) directly in the Direct Connect console or programmatically using the ListVirtualInterfaceRoutes API action. Each route displays its prefix, address family, AS path, community values, and installation timestamp, giving you comprehensive insight into your routing topology. You can filter routes by prefix, AS path, community, or address family to quickly identify specific routing behaviors. This capability is particularly valuable when managing complex multi-region architectures, validating BGP policy configurations, or diagnosing unexpected traffic patterns.


This feature is available in all AWS commercial Regions and the AWS China Regions (Beijing, operated by Sinnet, and Ningxia, operated by NWCD).


To learn more about BGP route visibility, visit the


[AWS Direct Connect documentation](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) or access the feature through the


[Direct Connect console](https://us-east-1.console.aws.amazon.com/directconnect/v2/home) .
