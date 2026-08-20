---
schema_version: "1.0.0"
document_id: "b58ee579d81d9201141f602d1686598c79e850903cae75101b00bed4a9ee6fbc"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-pcs-node-lifecycle-actions/"
published_at: "2026-07-23T12:28:00+00:00"
first_seen_at: "2026-07-30T14:41:00.348313+00:00"
fetched_at: "2026-07-30T14:41:02.702583+00:00"
content_hash: "sha256:936c9388fddbf0009942204efe975293ff1a9b20af0165692ac4c628995c31da"
---

# AWS Parallel Computing Service now supports node lifecycle actions

Today, AWS announces the general availability of node lifecycle actions in AWS Parallel Computing Service (PCS). With node lifecycle actions, you can run custom scripts automatically at defined points in a compute node's lifecycle. You can use them to prepare your nodes for work. For example, you can mount shared storage, join a directory service, install software, or set up monitoring.


You define node lifecycle actions in your PCS compute node group configuration when you create or update the group, and you can reuse the same script across multiple compute node groups and clusters. For each script, you set its location as an Amazon S3 or HTTPS URI, the arguments to pass, which lifecycle stage it runs in, whether it re-runs on reboot, and its error-handling behavior. AWS PCS writes the output to a dedicated log file, giving you visibility into what ran.


AWS PCS is a managed service that simplifies running and scaling high performance computing (HPC) workloads on AWS using Slurm. You can build complete, elastic environments that integrate compute, storage, networking, and visualization tools, and the service manages cluster updates and provides built-in observability.


Node lifecycle actions are available in all AWS Regions that support AWS PCS. To learn more, see the[AWS PCS User Guide](https://docs.aws.amazon.com/pcs/latest/userguide/cng-node-lifecycle-actions.html) .
