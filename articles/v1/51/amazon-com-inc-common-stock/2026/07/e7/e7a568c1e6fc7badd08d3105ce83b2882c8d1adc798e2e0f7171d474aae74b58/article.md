---
schema_version: "1.0.0"
document_id: "e7a568c1e6fc7badd08d3105ce83b2882c8d1adc798e2e0f7171d474aae74b58"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-eks-provisioned-control/"
published_at: "2026-07-28T17:18:00+00:00"
first_seen_at: "2026-07-28T23:43:23.890345+00:00"
fetched_at: "2026-07-28T23:43:26.468+00:00"
content_hash: "sha256:a718ee9649b3c1cbc5bbaddca5647406bc5d6f1b727b3ed514cb20a462de472d"
---

# Amazon EKS Provisioned Control Plane now delivers faster pod autoscaling

[Amazon EKS](https://aws.amazon.com/pm/eks) now delivers faster pod autoscaling across all[Provisioned Control Plane](https://docs.aws.amazon.com/eks/latest/userguide/eks-provisioned-control-plane.html) clusters by increasing[Horizontal Pod Autoscaler (HPA)](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/) sync concurrency to up to 40 times the default Kubernetes value. This reduces the time it takes for HPA-driven workloads to scale in response to increased load, enabling faster responsiveness to demand.


The Kubernetes Horizontal Pod Autoscaler (HPA) continuously monitors workload metrics and adjusts pod counts to match demand. In clusters running hundreds or thousands of HPA objects, the speed at which the Kubernetes control plane processes these objects determines how quickly workloads scale in response to changing demand. The HPA sync concurrency setting controls how many HPA objects the control plane evaluates in parallel. By increasing this value, Provisioned Control Plane clusters now process more HPA objects simultaneously, reducing the time between detecting increased load and scaling out pods.


This enhancement is available to all customers using EKS Provisioned Control Plane and requires no configuration changes. To learn more about this enhancement, see EKS Provisioned Control Plane in the EKS[User Guide](https://docs.aws.amazon.com/eks/latest/userguide/eks-provisioned-control-plane.html) .
