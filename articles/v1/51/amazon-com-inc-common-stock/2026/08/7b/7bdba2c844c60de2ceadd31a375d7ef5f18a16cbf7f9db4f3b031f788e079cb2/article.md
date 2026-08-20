---
schema_version: "1.0.0"
document_id: "7bdba2c844c60de2ceadd31a375d7ef5f18a16cbf7f9db4f3b031f788e079cb2"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-elasticache-graviton4-m8g-r8g-c8gn/"
published_at: "2026-08-06T19:00:00+00:00"
first_seen_at: "2026-08-07T00:09:00.624860+00:00"
fetched_at: "2026-08-07T00:09:02.541202+00:00"
content_hash: "sha256:3a43e5bf60b7846b28531a3a86a7fc468111d1ecdf82f354d549c9e9deee69ef"
---

# Amazon ElastiCache now supports Graviton4-based M8g, R8g, and C8gn nodes

Amazon ElastiCache now supports Graviton4-based M8g, R8g, and C8gn node families for Valkey and Memcached. Graviton4-based nodes provide up to 47% higher throughput, up to 43% lower P99 latency, and up to 31% better price-performance for on-demand pricing over Graviton3-based nodes of equivalent sizes on Amazon ElastiCache for Valkey, depending on node family, size, and workload configuration.


Graviton4-based nodes also offer more memory per node compared to equivalent Graviton3-based nodes. As an example, an m8g.8xlarge provides 124.65 GiB versus 103.68 GiB on m7g.8xlarge, up to 20% more memory at the same node size. C8gn nodes offer up to 200 Gbps of network bandwidth, enabling you to scale performance and throughput while optimizing the cost of running network-intensive workloads.


M8g, R8g, and C8gn nodes are available in sizes from large to 16xlarge in over 30 AWS Regions, including the AWS GovCloud (US) Regions and the China Regions. For complete information on pricing and regional availability, please refer to the[Amazon ElastiCache pricing page](https://aws.amazon.com/elasticache/pricing/) . To get started, create a new cluster or modify an existing cluster using the[AWS Management Console](https://console.aws.amazon.com/elasticache/) , AWS SDK, or AWS CLI. To work with ElastiCache using AI coding agents, see[Agent tools for ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/AgentTools.html) . To learn more, see[Supported node types](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/CacheNodes.SupportedTypes.html) in the *Amazon ElastiCache User Guide* .
