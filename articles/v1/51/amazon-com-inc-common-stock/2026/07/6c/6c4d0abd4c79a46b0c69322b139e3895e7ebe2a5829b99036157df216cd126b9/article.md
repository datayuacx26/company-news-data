---
schema_version: "1.0.0"
document_id: "6c4d0abd4c79a46b0c69322b139e3895e7ebe2a5829b99036157df216cd126b9"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-network-load-balancer-supports-listener-rules/"
published_at: "2026-07-22T19:13:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:13f48c46915860c0ec855439cd221930c624e2a8e6f7fb698f6df077dcce662a"
---

# AWS Network Load Balancer now supports Listener Rules for custom traffic routing

Network Load Balancer (NLB) now supports listener rules allowing you to route connections to different target groups based on the source IP address type. With listener rules, a single dual-stack NLB sends IPv6 client traffic to IPv6 targets and IPv4 client traffic to IPv4 targets, preserving the original client IP address end to end for both address families.


Previously, serving both IPv4 and IPv6 clients from one NLB meant accepting a tradeoff: either run two separate load balancers (one per IP version) and split clients with DNS, or send all traffic to one target group and lose the original client IP through protocol translation. Listener rules remove that tradeoff by enabling conditional routing at Layer 3, directing each connection to a same-family target group with no translation and no additional infrastructure.


You can add listener rules to existing dual-stack NLBs without recreating them. Rules are supported on TCP, UDP, TCP_UDP, and TLS listeners and work alongside existing NLB features including connection draining, target group stickiness, cross-zone load balancing, weighted target groups, and client IP preservation.


Listener rules for Network Load Balancer are available in all AWS commercial Regions and the AWS GovCloud (US) Regions at no additional charge. Standard NLB pricing for load balancer hours and LCUs applies. To get started, see[this AWS blog](https://aws.amazon.com/blogs/networking-and-content-delivery/consolidate-dual-stack-architectures-with-listener-rules-for-network-load-balancer/) , and the[Network Load Balancer User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) .
