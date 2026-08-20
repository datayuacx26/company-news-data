---
schema_version: "1.0.0"
document_id: "cdc67c17872e1c29de7cdbccb2dfe617c0c50b853daca5ce260a0bda5ff314e2"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/announcing-depot-metal"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:00358f3fb828a3695471eef82bf0ea3530ce0bcef20f10614ac0ea1acb1e4914"
---

# Announcing Depot Metal

Your Depot CI and Sandbox workloads just got faster thanks to **Depot Metal** , the next generation of our compute platform.


Our benchmarks already show workloads running 30% faster, with more optimizations incoming. If you use Depot CI or Depot Sandboxes, they're already running on Depot Metal at no additional cost.


## What is Depot Metal?


Depot Metal is the 4th iteration of our compute and storage platform, combining the learnings from all previous storage and compute at Depot into one unified platform:


- **Bare-metal EC2 instances** : from AWS, powered by 5th-generation AMD EPYC processors, running microVMs
- **Dedicated EC2 storage instances** : providing microVM root disks, backed by high-performance NVMe local SSDs
- **AWS S3** : as the ultimate storage location for microVM disk blocks


This architecture gives us direct control over each tier: direct control over compute with EC2 bare metal, ultra-fast NVMe to cache the VM disk blocks, and S3 to durably save base images and snapshots.


## Technical background


The Depot Metal compute and storage tiers


Depot Metal combines the best of Depot's previous architectures into one unified compute and storage platform.


This delivers something unique within AWS: combining the fastest compute instance type with the fastest storage instance type, and replacing EBS, to create the highest performance compute platform for all Depot workloads.


### Compute


From benchmarking, EC2 instances with **AMD EPYC CPUs** deliver the highest performance for build and test workloads running on Depot. This is due in part to the raw performance that the CPUs deliver, as well as the fact that each vCPU is backed by one physical CPU core; a 1 to 1 layout of CPU threads to CPU cores. This ensures that even single-threaded workloads compute quickly.


EC2 bare metal also gives us **direct control** over the physical hardware and microVM hypervisor; Depot Metal has been tuned to take full advantage of the compute hosts. Moving to a microVM architecture allows us to run workload accelerators (for example, memory caching) and job observability outside the job VMs, directly on the host, freeing resources inside the VMs for user jobs.


The new separation created by bare metal + hypervisor also allows for enhanced security, as orchestration components no longer need to run inside the VMs next to user jobs, but can now observe jobs from outside, on the metal host itself.


This new architecture also dramatically decreases the time to start new workload VMs: previously each CI job would need to wait for a new EC2 host to start. We[optimized this time](https://depot.dev/blog/faster-ec2-boot-time) to under 10s, but using microVMs, this can now be sub-second.


### Storage


Up until now, we have used a few different storage technologies depending on the workflow or type of data being stored:


- **S3** : used to store cache blobs produced by CI jobs and build tools like Bazel, Gradle, Go
- **Distributed block device (Ceph)** : used to store Docker image build cache on full block devices, backed by NVMe SSDs
- **EBS** : single-use ephemeral root disks, used for just one job and then destroyed
- **RAM disk cache** : used inside CI runners to accelerate both reads and writes from EBS using host memory


Depot Metal combines the learnings of all four of these technologies into one accelerated storage platform:


1. S3 is the ultimate durability layer for VM base images and snapshots, these are stored as real ext4 filesystems / block-level snapshots, for maximum performance and compatibility with all workloads running inside Depot Metal.
2. Dedicated EC2 instances act as storage servers for multiple bare metal compute hosts, these servers read and write disk content from S3 and store it on high-performance local NVMe SSDs. These servers replace Ceph and EBS, delivering the microVM disks to the compute servers via NVMe-oF/TCP.
3. Both the compute servers and the storage servers cache block reads and writes in host memory, mirroring[our previous learnings](https://depot.dev/blog/introducing-github-actions-ultra-runners) from using memory as a disk cache.


## Using Depot Metal


All[Depot CI](https://depot.dev/products/ci) workloads and[Depot Sandboxes](https://depot.dev/blog/now-available-the-depot-sandbox-sdk) are **now running on Depot Metal** ! You do not need to do anything to take advantage of the new performance.


## What's next?


We're excited by the performance Depot Metal delivers today, and we're already working on additional optimizations. We'll migrate Depot GitHub Actions runners and container builds to Depot Metal over the next few months. And we'll share details and more technical deep dives as we go.


## FAQ


Why did Depot launch Metal?


We had been running different compute and storage technologies for different workloads: S3 for cache blobs, Ceph for Docker build cache, EBS for ephemeral root disks, and RAM disk caching on top. Depot Metal folds the learnings from all of them into one unified platform. Building on bare-metal EC2 gives Depot direct control over every tier, from the CPU and hypervisor down to the NVMe storage, which means better performance, and moving orchestration out of the job VMs also improves security.


Do I need to do anything to run my workloads on Depot Metal?


No. Depot CI workloads and Depot Sandboxes are already running on Depot Metal, at no additional cost. There's no config change, no opt-in, and no migration on your end. You just get the faster performance.


Where does the speedup come from?


A few things stack up. The AMD EPYC CPUs back each vCPU with a physical core in a 1:1 layout, so even single-threaded work runs fast. Running on bare metal with a microVM hypervisor lets Depot move accelerators like memory caching and job observability out of the job VM and onto the host, freeing resources for your work. And the storage tier replaces EBS and Ceph with NVMe-backed servers over NVMe-oF/TCP, with block reads and writes cached in host memory. Separately, microVMs start in sub-second time instead of waiting for a new EC2 host to boot, which cuts startup latency.


Are Depot container builds and GitHub Actions runners running on Depot Metal too?


Not yet. Today Depot Metal powers Depot CI and Depot Sandboxes. GitHub Actions runners and container builds will move to Metal over the next few months, and we'll share more technical detail as we go.


## Related posts


- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [Now available: Depot Sandbox SDK](https://depot.dev/blog/now-available-the-depot-sandbox-sdk)
- [Optimizing microVM boot times](https://depot.dev/blog/optimizing-microvm-boot-times)


Jacob Gillespie


CTO & Co-founder of Depot
