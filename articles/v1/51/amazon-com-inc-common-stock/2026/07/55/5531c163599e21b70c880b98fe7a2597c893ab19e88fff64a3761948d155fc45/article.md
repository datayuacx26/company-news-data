---
schema_version: "1.0.0"
document_id: "5531c163599e21b70c880b98fe7a2597c893ab19e88fff64a3761948d155fc45"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/knfsd-file-cache/"
published_at: "2026-07-20T08:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:3c2c990a0d618fb4e25234bafc3e32860409de09eb7df812b24fcc471221ddce"
---

# Introducing KNFSD File Cache - Now in Preview

Today, AWS announces the availability of KNFSD File Cache, an open-source, Apache-2.0 licensed solution for deploying a scalable, high-speed Network File System (NFS) cache on AWS. KNFSD File Cache mounts exports from one or more source NFS servers, whether on-premises, in another AWS Availability Zone or Region, or in another cloud over AWS Interconnect - multicloud, and re-exports them to NFS clients in AWS. You can front multiple on-premises filers, in-cloud file systems such as Amazon FSx for OpenZFS and Amazon FSx for NetApp ONTAP, and any other NFS v3, v4.1, or v4.2 compliant filer. Frequently read data is cached in memory and on local NVMe storage, so files cross the high-latency link to the source once and are then served to large compute fleets at local VPC speed. The solution is designed for read-heavy burst compute workloads such as visual effects rendering, simulation, financial services, health and life sciences, microprocessor design, weather forecasting, and energy.


KNFSD File Cache builds on standard Linux kernel technology: nfs-kernel-server provides NFS re-export, and FS-Cache provides the persistent disk cache. Because it uses the native NFS stack, it fully supports the NFS client-server protocol, including byte-range reads and writes, synchronous and asynchronous writes, and both write-through and write-around modes. You build the cache Amazon Machine Image (AMI) with Packer, then deploy the cluster with the included Terraform module. Cache nodes run in an Amazon Elastic Compute Cloud (Amazon EC2) Auto Scaling group on AMD, Intel, or AWS Graviton instances, with client traffic distributed by DNS round-robin or a Network Load Balancer, and optional automatic scaling based on the number of active NFS client connections. An Amazon CloudWatch dashboard provides more than 70 metrics through an OpenTelemetry-based agent, which can also publish to third-party tools such as Prometheus and Grafana.


KNFSD File Cache (preview) is available in all AWS Regions. There are no licensing costs; you pay only for the AWS resources you consume.


To get started, visit the KNFSD File Cache GitHub[repository](https://github.com/awslabs/knfsd-file-cache) , launch[blog](https://aws.amazon.com/blogs/media/introducing-knfsd-file-cache-extending-your-nfs-storage-into-the-cloud/) , and[AWS Solutions Guidance](https://docs.aws.amazon.com/solutions/knfsd-file-cache-on-aws) . For detailed deployment and configuration guidance, see the GitHub[documentation](https://github.com/awslabs/knfsd-file-cache/blob/main/docs/index.md) .
