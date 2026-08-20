---
schema_version: "1.0.0"
document_id: "1ca60bb35e5e4869376506baa617e6295352291ec5b5a40db875e5e0f7f73f29"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-application-network/"
published_at: "2026-08-04T18:28:00+00:00"
first_seen_at: "2026-08-04T20:32:41.789283+00:00"
fetched_at: "2026-08-04T21:21:09.879682+00:00"
content_hash: "sha256:592f4ae03347fcd942407495aae9556cd39f9229e62c2360492811b461afc241"
---

# AWS Application and Network Load Balancers now support RFC 9151 compliant security policies

AWS Application Load Balancer (ALB) and Network Load Balancer (NLB) now support new TLS-based security policies that comply with RFC 9151 TLS server requirements for Commercial National Security Algorithm (CNSA) 1.0 suite requirements. These policies implement the cryptographic requirements defined by the US National Security Agency (NSA) for secure communications using TLS 1.2 and TLS 1.3 protocols.


Customers who are required to meet CNSA 1.0 TLS security requirements can now use ALB and NLB with RFC 9151 compliant security policies. Broader interoperability policies are also supported, allowing you to implement CNSA by default while maintaining compatibility with non-CNSA clients during their transition to RFC 9151 compliance, minimizing service disruption.


This feature is available for ALB and NLB in all AWS Commercial Regions, the AWS GovCloud (US) Regions, and the China region at no additional cost. To use this capability, update your existing ALB HTTPS listeners or NLB TLS listeners to a RFC 9151 compliant security policy, or select a compliant policy when creating new listeners through the AWS Management Console, CLI, API, or SDK.


To learn more, visit the[ALB User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/describe-ssl-policies.html) and[NLB User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/describe-ssl-policies.html) documentation. Get started with[Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/) .
