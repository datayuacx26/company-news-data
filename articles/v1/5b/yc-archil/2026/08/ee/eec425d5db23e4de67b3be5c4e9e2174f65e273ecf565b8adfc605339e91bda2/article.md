---
schema_version: "1.0.0"
document_id: "eec425d5db23e4de67b3be5c4e9e2174f65e273ecf565b8adfc605339e91bda2"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-6ed493c8d31a"
canonical_url: "https://archil.com/article/aws-ebs-made-simple"
published_at: null
first_seen_at: "2026-08-09T19:14:46.011593+00:00"
fetched_at: "2026-08-09T19:14:47.557372+00:00"
content_hash: "sha256:509a64cad756d7857f2ccad6e397276fdda2c4deddb91ddcc76036a02222a3cc"
---

# AWS EBS Made Simple: Picking the Right Storage Without Overspending

AWS Elastic Block Store (EBS) is one of those core AWS services you end up using whether you plan to or not. Every EC2 instance needs storage, and that storage usually comes from EBS. Much like the hard drive in your laptop, an EBS volume can host your operating system, store application data, and keep files safe even if the instance is stopped or restarted.


Choosing an EBS volume might seem simple, but the options can quickly lead to overspending or over-provisioning. The goal is to match your workload to the right type from the start.


## Understanding EBS Volume Types and Their Use Cases


AWS offers several types of EBS volumes, and each one is built for a different balance of performance and cost. At first glance, they look similar, but the right choice really depends on what your workload needs in terms of input/output operations per second (IOPS), throughput, latency, and price.


Here’s a breakdown of the main types:


### General Purpose SSD (gp3, gp2)


If you just need a good balance of price and performance, General Purpose SSDs are usually the go-to. They come in two variants: gp3 and gp2.


- **gp3** is the recommended option for new deployments. AWS designed gp3 to deliver up to 20% lower costs compared to gp2, while allowing you to independently configure IOPS and throughput. This flexibility makes gp3 a cost-efficient choice without sacrificing much performance.
- **gp2** is the older standard. It’s still fully supported, and lots of workloads run on it today, but if you’re starting fresh, gp3 is simply better for the money.


Typical use cases: boot volumes, small to medium databases, virtual desktops, general-purpose applications, APIs, and containers.


### Provisioned IOPS SSD (io2, io2 Block Express, io1)


For mission-critical workloads demanding high durability and consistent performance, Provisioned IOPS SSD volumes are best. There are three versions: io2, io2 Block Express, and io1.


- **io2** volumes provide low-latency performance with up to 64,000 IOPS. They deliver 4x higher IOPS than gp3 and offer higher durability (99.999% vs. 99.9%), reducing the risk of data loss by 100x.
- **io2 Block Express** pushes the limits further with sub-millisecond latency. These volumes can deliver up to 256,000 IOPS and 4,000 MB/s of throughput per volume, making them 16x faster in IOPS and 4x faster in throughput than gp3.
- **io1** is still available but is now considered legacy. io2 offers better durability and efficiency at the same price point.


Typical use cases of IOPS include large transactional databases (e.g., Oracle, MySQL), latency-sensitive applications (e.g., SAP), and workloads requiring sustained, high-performance I/O.


### Throughput Optimized HDD (st1)


For workloads that rely on large, sequential operations, Throughput Optimized HDD volumes provide high throughput at a low price point. They are available in a single class: st1. st1 volumes can deliver up to 500 MB/s of throughput with a maximum of 500 IOPS. While far below SSD-backed standards, st1 is optimized for throughput rather than IOPS, making it suitable for workloads that can tolerate lower performance.


Typical use cases of st1 include big data applications (e.g., Hadoop), log processing (e.g., Splunk, ELK stacks), data warehouses, and analytics.


### Cold HDD (sc1)


For scenarios where minimizing costs is the primary goal (or for data that's infrequently accessed), Cold HDD volumes deliver the lowest cost EBS storage option. They also ship in a single class: sc1. sc1 is designed for workloads that don’t require sustained performance but need to store large amounts of data inexpensively. Performance is intentionally limited, so sc1 isn’t something you’d want to run production workloads on.


Typical use cases of sc1 include dev/test environments, data archives, and workloads where cost matters more than performance.


## How AWS Charges for EBS


Unlike object storage such as S3, which charges for both stored data and requests, EBS pricing is tied to provisioned resources. You are billed for the capacity and performance you reserve, not what you use.


Two main factors shape EBS charges:


1. Provisioned Storage Capacity
2. IOPS and Throughput


Pricing differs by region, but for clarity, we'll stick to the popular us-east-1 region for the following examples.


### 1. Provisioned Storage Capacity


Every EBS volume type has a per-GB monthly rate. This is charged on the amount you provision, not what you fill up. So, if you spin up a 500 GB gp3 volume but only store 50 GB of data on it, you still pay for 500 GB.


This cost is radically different for different volume classes:


- gp3: ~$0.08 per GB-month
- st1: ~$0.045 per GB-month
- sc1: ~$0.025 per GB-month
- io2: ~$0.125 per GB-month


To put that into perspective: a 500 GB gp3 volume costs about $40/month, while the same size io2 volume is closer to $62/month.


### 2. Provisioned Performance


EBS volumes that allow customizable performance (e.g., gp3, io1, io2) also incur charges for extra IOPS or throughput beyond an established baseline.


- gp3 includes 3,000 IOPS and 125 MB/s throughput at no extra cost. From there, you can scale up for a price by paying:


- ~$0.005 per provisioned IOPS above baseline per month
- ~$0.04 per provisioned MB/s above baseline per month


- io2/io1 have no included baseline like gp3. Instead, you provision the exact number of IOPS and throughput needed from the start.


Example: Let’s say you need a 500 GB gp3 volume with 10,000 IOPS and 500 MB/s throughput. The cost breaks down like this:


- Storage: $40/month
- Extra IOPS: (10,000 – 3,000) × $0.005 = $35/month
- Extra throughput: (500 – 125) × $0.04 = $15/month
- **Total: ~$90/month**


## Optional EBS Charges: Snapshots & Data Transfers


Storage volumes are only part of the EBS bill. If you use extras like snapshots or cross-region copies, you’ll see additional charges. The good news is these only incur charges if you enable them.


Snapshot fees fall into two categories:


1. Snapshot storage
2. Charges for copying or exporting the data


### Snapshot Storage


Think of snapshots as backups for your EBS volumes. The first snapshot captures the entire volume, while subsequent snapshots only store the changed blocks, making it a highly efficient backup strategy.


The cost is straightforward:


~$0.05 per GB-month. Accordingly, if your snapshot consumes 100 GB, you’d pay about $5/month.


Many teams schedule nightly EBS snapshots to ensure quick recovery in case of corruption or data loss.


### Data Transfers


Snapshots often need to move. You might replicate them across regions for disaster recovery, export them into an S3 for compliance, or share them across accounts. All of these operations introduce data transfer costs on top of storing your snapshots.


Here’s what to expect:


- Cross region copies are ~$0.02 per GB, plus destination storage
- Exports to S3 are $0.02 per GB exported, plus standard S3 storage fees


Example: Copying a 100 GB snapshot to another region:


- $2 in transfer + $5 in destination storage = $7/month


Example: Exporting the same snapshot to S3:


- $2 transfer + S3 storage (varies by class)


## Optimizing EBS Costs


EBS is one of the most widely used AWS services, yet it is often overlooked during cost reviews. Why? Because AWS automatically provisions volumes alongside EC2 instances, and those volumes can stick around even after the instance is stopped or terminated (unless you’ve checked the “Delete on Termination” box).


That’s how unused or forgotten volumes end up quietly racking up charges on your AWS bill. Here are a few simple strategies to keep EBS costs under control.


### Enable "Delete on Termination"


This prevents orphaned volumes. However, be cautious: deleting volumes with critical data can lead to unrecoverable loss.


### Tag Volumes to Track Ownership and Environment


It’s common for teams to lose track of who owns what. Tags solve that by adding labels like` env=dev` ,` owner=teamX` to allocate costs and enforce accountability. This integrates with AWS Cost Explorer.


### Leverage Native AWS Tools


AWS has built-in tools to spot waste and recommend optimizations:


- Use AWS Trusted Advisor to flag idle or orphaned volumes.
- Use AWS Compute Optimizer to recommend right-sizing underutilized volumes.


Using these regularly can save you from paying for volumes you don’t need or for performance you’re not using.


### Use Snapshots Strategically


Beyond simple backups, snapshots can double as a cost-saving tool. Snapshotting a volume and deleting the original can significantly cut storage costs. You only pay for changed data, and you can restore the volume later as needed.


### Archive Old Snapshots


AWS offers EBS Snapshot Archive, which cuts storage costs by up to 75% compared to standard snapshots. You can set lifecycle policies so older backups automatically move to the archive tier.


## Making the Most of EBS


The key to getting value from EBS is choosing the right volume type and actively managing usage. Treat EBS as a first-class resource, not an afterthought. With smart provisioning and continuous monitoring, you can run workloads efficiently without paying for unused capacity or performance.


‍
