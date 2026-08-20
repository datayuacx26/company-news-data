---
schema_version: "1.0.0"
document_id: "411d3213247bd03de9fa192998e167022cc2b82c806f7489da3dc031998939cd"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/nfs-cold-storage-backups"
published_at: "2025-10-02T08:05:09.740+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:bcc94833862d5f8ac6950180cf07c0543ae768ec43f7ad17d0c83415a0d2b5d9"
---

# Announcing cost-efficient storage with usage-based backups, cold storage, and Network file storage

As data footprints grow, businesses need cost-efficient storage for infrequently accessed data, high-performance file systems for collaborative work, and more aggressive data protection policies to meet strict recovery objectives. We’re introducing several significant enhancements to our storage portfolio to help you manage the challenges of data management, protection, and scaling.


**TL;DR**


**Network file storage solution for high-performance AI workloads and cloud applications, is now generally available** . You can access it in the[DigitalOcean console](https://cloud.digitalocean.com/nfs) . To learn more visit the product[documentation](https://docs.digitalocean.com/products/nfs/) page.


**Usage-based backups are now generally available to meet aggressive rpos.** Check out our[documentation](https://docs.digitalocean.com/products/backups/) to learn more and head over to the[DigitalOcean console](https://cloud.digitalocean.com/droplets/new) to enable backups for your Droplets or GPUs.


**Spaces cold storage for infrequently accessed data is now generally available.** Visit our[documentation](https://docs.digitalocean.com/products/spaces/details/features/#cold) to learn more and and head over to the[DigitalOcean console](https://cloud.digitalocean.com/spaces/new) to set up Spaces cold storage.


## Network file storage (NFS) for high-performance AI workloads


Data-intensive applications, particularly in AI and machine learning, require shared, high-performance file storage that is easy to provision and manage. Our Network file storage service is now generally available in our ATL1 , NYC2 and AMS3 data centers. We have also introduced a new Standard Tier designed for general-purpose cloud applications like App Platform workloads, web applications, CMS platforms, general compute tasks, and legacy applications requiring shared file systems. This tier provides predictable, cost-effective shared storage with ReadWriteMany semantics for cloud applications. Customers with high-throughput or data-intensive needs, such as AI/ML, GPU training, high-throughput analytics, or those requiring parallel multi-node access, should instead utilize the NFS High Performance Tier, which supports superior throughput scaling for multi-node environments and demanding data pipelines.


NFS supports share provisioning via UI/API and supports NFSv4.1, POSIX permissions, and VPC-based access so a single share can be mounted across multiple GPU/CPU Droplets and DOKS worker nodes for concurrent access. The service also provides snapshots for point-in-time restores and offers allocation-based pricing with discounts for GPU-committed customers. Provisioning is simple, and the service is designed for the high-throughput, low-latency demands of model training and inference. Unlike some competitors that start at 1TB+ increments with high minimums and complex pricing, our solution offers a more cost-effective entry point with shares starting from 50 GiB.


**Use cases:**


- **AI/ML Workloads** : Enable concurrent access to shared datasets and model artifacts for training and inference pipelines by mounting a single NFS share across multiple GPU Droplets.
- **Kubernetes applications** : Support stateful workloads requiring shared filesystem semantics (ReadWriteMany) across different pods and nodes within a Kubernetes cluster.
- **Media and content collaboration** : Facilitate multiple contributors working together on shared project files without the need to constantly move data between systems.
- **Cloud application assets** : Utilize for temporary uploads, exports, and shared application assets where the required filesystem interface cannot be easily replaced by object storage.


**Pricing & Performance:**


-


**Standard Tier** : $0.15 per GiB-month and offers a flat aggregated throughput of up to 1 Gbps per share, regardless of storage size.


-


**High Performance Tier** : $0.30 per GiB-month and starts at 8 Gbps throughput at 1 TiB, scaling linearly by increasing 1 Gbps for each additional TiB.


-


**Snapshots** : Billed separately at $0.06 per GiB-month based on restorable size.


-


Minimum public share size: 50 GiB. Maximum Share size: 32 TiB


-


Customers who need more storage or performance can[contact support.](https://docs.digitalocean.com/support/)


**Benefits:**


-


**Simplified operations** : Eliminate the overhead of self-managing a shared file system.


-


**Performance optimized** : Get high throughput and low latency tailored for AI/ML workloads.


-


**Cost-effective scaling** : Simple per-GiB pricing with transparent tiering and regional availability.


To learn more about Network file storage visit the product[documentation](https://docs.digitalocean.com/products/nfs/) page or access it directly in the[DigitalOcean console](https://www.digitalocean.com/blog/%5Bhttps://cloud.digitalocean.com/nfs%5D(https://cloud.digitalocean.com/nfs)) .


## Usage-based backups to meet aggressive RPOs


Our new usage-based backup service is now generally available to help users with strict Recovery Point Objectives (RPO) schedule backups every 4, 6, or 12 hours. Flexible retention policies for Droplets can be configured for 3 days to 6 months.


This feature is paired with a transparent, consumption-based billing model, charging only for the amount of restorable data used based on frequency.


-


**Weekly:** $0.04/GiB-Month


-


**Daily:** $0.03/GiB-Month


-


**12 Hour:** $0.02/GiB-Month


-


**6 Hour:** $0.015/GiB-Month


-


**4 Hour:** $0.01/GiB-Month


This means you only pay for what you actually use with no hidden fees for snapshot operations. This provides the flexibility to create a recovery plan that is both technically sound and financially viable, especially for high-change environments in regulated industries or for development environments.


**Use cases:**


-


**Compliance-driven organizations:** Supports processing workloads that are subject to stringent compliance standards like HIPAA and SOC 2 by enabling frequent backups that can be stored for longer duration to provide a granular audit trail for sensitive data.


-


**Gaming and AI startups:** Protect rapidly changing user data and AI models on GPU Droplets with high-frequency backups, allowing for quick rollbacks in case of an issue.


-


**Development environments:** Shorten Recovery Point Objectives (RPOs) in Continuous Integration (CI)/Continuous Deployment (CD) pipelines and other dev workflows by using 4-6 hour backups to protect code and data changes.


-


**SaaS environments:** Safeguard rapidly changing user data in customer support platforms and SaaS tools by implementing more frequent, reliable data protection.


**Benefits:**


-


**Enhanced data integrity:** Restore from a more recent point in time, reducing data loss in the event of an incident.


-


**Transparent billing:** A clear, predictable cost model based on actual stored data.


-


**Compliance-ready:** Configure granular RPO and retention policies to enable meeting internal and external compliance standards.


Check out our[documentation](https://docs.digitalocean.com/products/backups/) to learn more and head over to the[DigitalOcean console](https://cloud.digitalocean.com/droplets/new) to enable backups for your Droplets or GPUs.


## Spaces cold storage for infrequently accessed data


The rapid growth of AI has led digital-native enterprises to store vast quantities of data, much of which is rarely accessed.[DigitalOcean’s Spaces](https://www.digitalocean.com/products/spaces) cold storage is generally available to store such infrequently accessed objects at a price of $0.007/GiB per month. This includes retrieval of all cold data stored in a bucket up to once per month at no additional cost, after which retrieval overages are charged at $0.01 per GiB per month. This new cold storage bucket type provides a low-cost, S3-compatible solution for petabyte-scale datasets where data is accessed infrequently, needs to be retained for at least 30 days, and must be retrieved instantly. Spaces cold storage service will have a 99.5% uptime per month as defined in[service level agreement](https://www.digitalocean.com/sla/spaces-cold-storage) .


Spaces Cold Storage


**Use cases:**


-


**Backups and disaster recovery:** Store secondary copies of data that are rarely accessed but must be available instantly.


-


**Application logs and diagnostics:** Keep data that must be occasionally retrieved for incident investigation, security events, or regulatory needs.


-


**Archives:** Store user-generated content, scientific data, and older project files.


-


**AI/ML training and inference archives:** Archive large, infrequently accessed datasets or model checkpoints that can be retrieved on-demand.


**Benefits:**


-


**Cost-effective scaling:** Store petabytes of data at a fraction of the cost of standard storage tiers.


-


**Predictable pricing:** Our simple pricing model includes one retrieval per month, up to your average storage usage, at no additional cost, and predictable, transparent pricing for additional retrievals so you can avoid the high, unpredictable fees of some other providers.


-


**Instant retrieval:** Access your data within seconds, even when it’s stored in a cold tier.


Visit our[documentation](https://docs.digitalocean.com/products/spaces/details/features/#cold) to learn more and and head over to the[DigitalOcean console](https://cloud.digitalocean.com/spaces/new) to set up Spaces cold storage.


By building these new capabilities, we are providing a more robust, flexible, and cost-effective infrastructure to help you address the challenges of scaling your business and keeping costs under control.


Ready to get started?


-


**Try these features** by heading to the[DigitalOcean console](https://cloud.digitalocean.com/login) .


-


**Learn more** by visiting our[product documentation](https://www.digitalocean.com/docs) and[regional availability](https://docs.digitalocean.com/platform/regional-availability/) .


-


[Get expert guidance for free](https://www.digitalocean.com/landing/get-expert-guidance) to strengthen your cloud architecture, optimize costs, scale your infrastructure, and improve backup and disaster recovery.
