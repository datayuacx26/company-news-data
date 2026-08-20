---
schema_version: "1.0.0"
document_id: "b918bb03e8a6cda592d025588ff51aca81114d6cbc62b6a27a36f7d13cf5d7a8"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-eks-efa-placement-groups/"
published_at: "2026-07-22T14:30:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:a746b5a594014478f2dcee1fb9091867557d8a2ac5bb88bc69a793cdbbd01128"
---

# Amazon EKS now supports EFA and placement groups on Amazon EKS Auto Mode and Karpenter

Amazon Elastic Kubernetes Service (EKS) now supports Amazon EC2 placement groups and Elastic Fabric Adapter (EFA) network device configuration for node pools on EKS Auto Mode and the open-source Karpenter project, enabling you to optimize EKS workloads for performance and availability. These capabilities allow you to control EFA network interface configuration and how EC2 instances are physically distributed across AWS infrastructure for distributed training and inference workloads.


With EKS Auto Mode and Karpenter’s EFA configuration, you can configure network interfaces as EFA-only or standard ENI on EFA-capable instances with both dynamic and static capacity node pools. EFA-only interfaces do not consume IP addresses, giving you fine-grained control over IP utilization in your VPC while achieving full interconnect bandwidth. With placement group support, you can launch EC2 instances using cluster, spread, or partition strategies directly from your EKS Auto Mode or Karpenter node pool configuration, giving you control over how instances are physically distributed without additional operational workarounds. Together, these capabilities let you optimize for the performance, availability, and fault isolation characteristics your workloads require, whether that's maximizing throughput for distributed training jobs or minimizing blast radius for critical production services.


These features are available in all AWS Regions where Amazon EKS is available. To get started and learn more, see the[EKS Auto Mode User Guide](https://docs.aws.amazon.com/eks/latest/userguide/create-node-class.html#static-network-interfaces) and[Karpenter documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specnetworkinterfaces) .
