---
schema_version: "1.0.0"
document_id: "5f9f9ca334307fa59ba357864c9fea39ba9d963398a503605b065b038aa40079"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/does-mhz-still-matter"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:21b37347459da9f6fa4855a7ecbeb4e92e98a044358e0563be96b71c58fa0849"
---

# Does MHz still matter?

## **Does MHz still matter?**


August 22, 2025 · 4 min read


Furkan Sahin


Senior Software Engineer


To provide VMs of any size, we slice bare metal into smaller VMs, sometimes even 1 vCPU. So, the performance of one fast core matters a lot. We evaluated new servers from Hetzner with AMD EPYC and Ryzen CPUs to add to our fleet. Ryzen is a CPU from AMD’s gaming line-up and it has better single core performance numbers compared to the EPYC which is a standard datacenter CPU. We weren’t sure if Ryzen’s single core edge would show up in real workloads. We ran our own tests across CPU‑heavy and mixed tasks. We saw clear wins on CPU‑bound jobs and unexpected gains in disk IO, because of the encryption load in the IO path.


### Single-core CPU Benchmarks


We began with a single-core CPU benchmark using[PassMark’s PerformanceTest Linux benchmarking tool](https://www.passmark.com/products/pt_linux) . We ran 2 threaded tests on 2 vCPU VMs, saturating a single physical core to establish a baseline.


P50 P90 P99


AMD EPYC 9454P


2840.14


2913.66


2953.17


AMD Ryzen 9 7950X3D


3762.05


3869.70


3909.40


Performance difference


+32.46%


+32.81%


+32.38%


PassMark’s website shows a similar score difference. There is about a 28% increase in single-threaded performance from the AMD EPYC 9454P to the AMD Ryzen 9 7950X3D. You can see this in[cpubenchmark.net](https://www.cpubenchmark.net/compare/5596vs5234/AMD-EPYC-9454P-vs-AMD-Ryzen-9-7950X3D) , too. There is slight varience with our results but it is still acceptable. However, a single core score alone does not fully reflect real-world performance.


### Github Action Experiment


We wanted to test Ryzen CPU performance for real-world scenarios. So we ran a trial using our GitHub Actions Runners. At Ubicloud, our service creates hundreds of thousands of runners, daily. Each runner has its own performance traits. This experiment aims to determine if Ryzen CPUs perform better in real world use.


In this experiment, we have run the same jobs in two instance types from Hetzner


- AX102 (AMD Ryzen™ 9 7950X3D 16-Core)
- AX162 (AMD EPYC™ 9454P 48-Core)


Each job is run in an individual VM with isolated networking, memory and disks. We also upgraded the server’s uplink to 10Gbit.


Model Jobs (count) p50 (sec.) p75 (sec.) p90 (sec.) p99 (sec.)


AX102


146725


80


228


412


1077


AX162


206411


129


305


512


1234


Experiment results indicate that Ryzen instances outperform EPYC for most workloads. The performance gain is big in many cases. Yet, it gets smaller over longer runs. Analysing these results show that there are some other factors we need to consider.


Longer jobs saw smaller performance gains from the stronger CPU. This is because even with improved single core CPU performance, networking became a bottleneck. We realised that longer jobs often hit limits in networking. Once network IO became the main bottleneck, the faster CPU mattered less. An example job downloads a ~200GB docker image and then performs a CPU heavy job for 10 seconds. So, the single core performance gain has a much lower impact for that job. Because the runtime is dominated by data transfer speeds.


Interestingly, shorter jobs at the P50 show improved performance with ~38% compared to CPU-only tests ~32%. This is interesting, because there must be a different factor that gives us that boost. Therefore, we dug in further to see why. How come a CPU upgrade can give us a better result than the difference in CPU performance?


### PostgreSQL Benchmarks


We ran another round of benchmarks using Ubicloud PostgreSQL. The goal was to use a more controlled environment with an application that has clearer performance characteristics. The previous benchmark provided a general view of the performance difference, but identifying bottlenecks requires a controlled setup.


Using pgbench, we have first performed a CPU intensive (SELECT only, in-memory) benchmark and seen that the AX102 performed better. This was expected as we have seen before with the single core performance gains.


Increasing the coverage of the benchmark further, we have run another one where the data does not fit into the memory. This way, our benchmark saturated both the CPU and the disk IO. The results showed that the benefits were even larger with ~60% performance gains.


This was surprising. However, soon we realised that the encryption work in our disk-IO path is CPU bound. We are using SPDK to provide block storage devices with encryption. We give it its own CPU cores, and encryption runs on those cores for every read and write. When the CPU is faster, each IO de/encryption takes less time. It also lets us send more IOs at the same time. The result is a bigger gain in disk performance than the single-core score suggests.


### Conclusion


Our tests point to a clear theme: stronger single‑core performance translated into real wins across our fleet. Moving the same workloads from EPYC 9454P to Ryzen 9 7950X3D delivered a steady 32–38% improvement, and IO‑heavy cases often gained more because our encrypted storage path puts extra work on the CPU. We can conclude that one faster core lifts short jobs, reduces IO wait, and keeps NVMe busier. We’ll keep bringing in newer, stronger hardware on that basis, so workloads on Ubicloud speed up without code changes.
