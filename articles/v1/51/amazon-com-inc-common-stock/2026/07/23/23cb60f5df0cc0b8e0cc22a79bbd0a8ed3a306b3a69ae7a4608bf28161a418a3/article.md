---
schema_version: "1.0.0"
document_id: "23cb60f5df0cc0b8e0cc22a79bbd0a8ed3a306b3a69ae7a4608bf28161a418a3"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/ecs-service-connect-zone-aware/"
published_at: "2026-07-23T23:46:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:33:07.280239+00:00"
content_hash: "sha256:67b2fcb5cf4e430eab1a249410cca1b3adacb25fbb0a3a1c5c86a0221a08be97"
---

# Amazon ECS Service Connect now supports Zone-Aware routing

[Amazon Elastic Container Service](https://aws.amazon.com/ecs/) (Amazon ECS) introduces zone-aware routing for ECS Service Connect, enabling customers to reduce cross Availability Zone (AZ) data transfer costs and latency by automatically prioritizing service-to-service traffic within the same AZ.


With this launch, ECS Service Connect preferentially routes requests to endpoints in the same AZ as the originating task while dynamically adjusting traffic weights as endpoints scale to maintain balanced load across target services. Previously, as customers distributed their applications across AZs for resiliency, service-to-service traffic led to significant cross-zone data transfer, requiring trade-offs between cost and resilience. Zone-aware routing eliminates this trade-off, and when local endpoints become unhealthy or fall below capacity thresholds, traffic automatically redistributes across healthy AZs to maintain availability without overloading any single zones.


Zone-aware routing is enabled by default for all new and existing services and requires no additional infrastructure or application code changes. Existing services require a one-time redeployment to enable the new routing behavior. You can use Amazon VPC Flow Logs with AZ metadata to monitor cross-AZ traffic patterns and validate routing effectiveness. This feature is available in all AWS commercial and AWS GovCloud (US)[Regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) , where ECS Service Connect is supported at no additional cost. For more details, refer to our[documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-connect-zone-aware-routing.html) and launch[blog post](https://aws.amazon.com/blogs/containers/announcing-zone-aware-routing-in-amazon-ecs-service-connect/) .
