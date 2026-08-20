---
schema_version: "1.0.0"
document_id: "ffe9e3f875904cea5e9e1dfc64ce4b5399ad771adda5d0989e7a0f399b21b115"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/hyperpod-partition-topology-slurm/"
published_at: "2026-07-17T15:16:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:89e2a71b9e8550da77b590a7ef974de2706800dd79986783121cbf6a5ea0cca2"
---

# Amazon SageMaker HyperPod now supports partition-level topology for Slurm orchestrated clusters

Amazon SageMaker HyperPod now supports network topology configuration at the partition level for Slurm orchestrated clusters. A single cluster can now run tree topology in one partition and block topology in another, with each partition using the topology best suited to its instance types. This improves distributed training performance by keeping job placement aligned with the interconnect characteristics of each instance type, so GPU-to-GPU communication is faster, NCCL collective operations are more efficient, and training throughput improves.


HyperPod determines the topology for each partition based on the instance types of its compute instance groups. Partitions with Amazon EC2 UltraServer instance types such as ml.p6e-gb200.36xlarge use block topology, and those with hierarchical-interconnect instance types such as ml.p5.48xlarge, ml.p5e.48xlarge, and ml.p5en.48xlarge use tree topology, while partitions with instance types that don't provide network topology information remain fully schedulable. HyperPod maintains this configuration automatically as the cluster changes through scale-up, scale-down, and node replacement events, so each partition's topology always reflects the current state of the cluster.


To get started, create or update a SageMaker HyperPod Slurm cluster running Slurm 25.11 or later with supported GPU instance types. Topology-aware scheduling is enabled by default and requires no configuration. This feature is available in all AWS Regions where Amazon SageMaker HyperPod is supported. To learn more, see[Using topology-aware scheduling in Amazon SageMaker HyperPod](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-hyperpod-topology.html) .
