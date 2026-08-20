---
schema_version: "1.0.0"
document_id: "60c307b1fc3ee1cd8d9ded239c91b79631a868455b5b64bd9e3d1caaa82c54ef"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-transit-gateway-policy-based-routing/"
published_at: "2026-07-30T15:39:00+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:0d7b753f3878004549fd9c95c6a3e36567143e3279102b90c7841e68ea289237"
---

# AWS announces general availability of Policy-Based Routing on AWS Transit Gateway

AWS Transit Gateway now supports Policy-Based Routing (PBR), giving network administrators granular control over how traffic is forwarded across their AWS network. With PBR, forwarding decisions can be based on a combination of packet attributes including source and destination IP addresses, ports, and protocol rather than destination IP address alone.


Previously, customers needing traffic steering or workload isolation had to build multi-VPC architectures with additional routing hops, adding complexity and operational overhead. PBR eliminates this by extending Transit Gateway's native routing capabilities, enabling security architects and enterprise network teams to classify and direct traffic inline without extra infrastructure. Customers associate a policy table with a Transit Gateway attachment and define an ordered set of rules. Each rule classifies traffic and directs matching packets to a specified route table using first-match-wins logic. This supports use cases such as steering sensitive workloads through AWS Network Firewall or third-party inspection appliances, routing application traffic over AWS Direct Connect or AWS VPN paths based on source, port, or protocol, and isolating production and development environments into separate routing domains to limit lateral movement.


Policy-Based Routing for AWS Transit Gateway is available in all commercial[AWS Regions](https://docs.aws.amazon.com/network-manager/latest/cloudwan/what-is-cloudwan.html#cloudwan-available-regions) where Transit Gateway is available. You can configure PBR using the AWS Management Console, AWS Command Line Interface (CLI), and the AWS Software Development Kit (SDK). PBR incurs no additional charge beyond standard Transit Gateway fees. To learn more about Policy-Based Routing for AWS Transit Gateway, visit the[AWS Transit Gateway product page](https://aws.amazon.com/transit-gateway/) .
