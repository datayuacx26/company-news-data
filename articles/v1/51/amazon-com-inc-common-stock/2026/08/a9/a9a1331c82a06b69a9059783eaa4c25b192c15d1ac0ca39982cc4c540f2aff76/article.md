---
schema_version: "1.0.0"
document_id: "a9a1331c82a06b69a9059783eaa4c25b192c15d1ac0ca39982cc4c540f2aff76"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-eks-control-plane-configuration-parameters"
published_at: "2026-08-12T18:00:00+00:00"
first_seen_at: "2026-08-12T22:13:25.422376+00:00"
fetched_at: "2026-08-12T22:13:28.934657+00:00"
content_hash: "sha256:185cc71ed5e7c5b24f3aa54146ff99816426d98af47e3aa570e789fc3754b1a5"
---

# Amazon EKS now supports advanced Kubernetes control plane configuration parameters

[Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/pm/eks/) now supports configuring parameters for Kubernetes control plane components including the scheduler, controller manager, and API server. You can tune pod placement strategies to improve resource utilization, adjust how quickly horizontal pod autoscaling responds to changes in demand, set resource lifecycle parameters such as event retention duration, and more.


Cluster administrators now have more control over Kubernetes control plane parameters beyond the defaults. For example, you can set the scheduler's node resource fit strategy parameter to *MostAllocated* , which packs pods onto nodes that are already well utilized and helps you run the same workloads on fewer nodes. The default *LeastAllocated* strategy spreads pods across nodes, and you can keep it where headroom matters more than density.


You can configure Kubernetes control plane parameters in any AWS Region where Amazon EKS is available. For the full list of configurable parameters and to learn more, see Control plane configuration in the[Amazon EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/control-plane-configuration.html) .
