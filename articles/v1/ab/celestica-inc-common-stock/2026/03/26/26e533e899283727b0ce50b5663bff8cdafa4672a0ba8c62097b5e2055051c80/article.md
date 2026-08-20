---
schema_version: "1.0.0"
document_id: "26e533e899283727b0ce50b5663bff8cdafa4672a0ba8c62097b5e2055051c80"
company_key: "celestica-inc-common-stock"
company: "Celestica Inc."
source_id: "celestica-inc-common-stock-news-import-cf21b4db58b5"
canonical_url: "https://www.celestica.com/blog/article/powering-germanys-most-powerful-university-ai-supercomputer-celesticas-role-in-the-helma-project"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-24T08:31:35.893158+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:0182e940323a6063b4c92e388b2454bdac63dce231f36f4fb79fde970d8e54e1"
---

# Powering Germany’s Most Powerful University AI Supercomputer: Celestica’s Role in the Helma Project

WRITTEN BY


Celestica


share


- LinkedIn


2026-03-10


In the high-stakes world of Artificial Intelligence and High-Performance Computing (HPC), storage is often the "silent bottleneck." While GPUs get the attention for processing massive datasets, they are only as effective as the storage infrastructure feeding them. At the Friedrich-Alexander-Universität Erlangen-Nürnberg (FAU), researchers recently faced this challenge head-on when deploying Helma, the most powerful university-owned AI supercomputer in Germany.


To unlock the full potential of Helma’s 768 NVIDIA H100/H200 GPUs, NHR@FAU required a storage solution that could handle terabyte-scale training sets and frequent model checkpoints with ease. The answer lay in a collaborative design featuring[Celestica storage controllers](https://www.celestica.com/our-expertise/platform-solutions/storage) .


## The Foundation of Performance: Celestica SC6100


Celestica's SC6100


To meet NHR@FAU’s non-negotiable requirements for high availability and top-tier performance, MEGWARE (a system integrator specializing in HPC and AI infrastructure) and Xinnor (a software company that specializes in patented software RAID technology) selected the[Celestica SC6100](https://cls.celestica.com/hardware-platforms/sc6100/) . The SC6100 is a high-availability, all-flash PCIe Gen5 storage controller designed specifically for demanding workloads like AI/ML and atomistic simulation.


In the Helma configuration, ten (10) SC6100 controllers form the backbone of a half-rack, all-NVMe Lustre file system. Each controller is a powerhouse, featuring:


-


Dual-Node Redundancy: Two independent server nodes sharing 24 PCIe Gen5 NVMe SSDs and three PCIe Gen5 x16 slots for high speed network IO, ensuring the system continues to operate even if a node fails.


-


High-Speed Processing: Powered by AMD EPYC 9454P CPUs (48 cores) and 384 GB of DDR5 RAM per server node to handle massive metadata and throughput demands. (SC6100 can be configured with a range of AMD EPYC CPUs up to 64 cores and memory up to 1.5TB RAM - per server node


## Achieving "Podium" Performance


The results of this architecture speak for themselves. After deployment, the Helma cluster was submitted to[IO500](http://io500.org/) , the premier global ranking for high-performance storage. The Celestica-powered solution achieved:


-


#1 position among all Lustre-based solutions.


-


#3 ranking in the global IO500 benchmark.


-


1,798.77 GiB/s in IOR Easy Read performance.


-


Over 8.2 Million IOPS in metadata testing (MDtest Easy Stat).


Perhaps most impressively, Celestica helped deliver this world-class performance within a half-rack footprint, roughly one-tenth the size of many competing multi-rack appliances. This allows not just for less physical space, but less associated power and cooling along with lower hardware and software costs.


*“For a national supercomputing center like NHR@FAU, performance is vital, but reliability is non-negotiable. Our collaboration with Celestica and MEGWARE allowed us to build a seamless, high-availability architecture where xiRAID and Lustre work in lockstep with the SC6100’s dual-node redundancy. This setup ensures that whether it’s a drive failure or a complete node failure, the system remains online, protecting the massive datasets and long-running GPU training sessions that are critical to Germany’s AI research.” -- Dmitry Livshits, CEO, Xinnor*


## Why Celestica Matters for AI Infrastructure


Celestica’s contribution to the Helma supercomputer extends beyond raw speed. By utilizing off-the-shelf commodity hardware, Celestica enables NHR@FAU to avoid vendor lock-in and benefit from "commodity economics".


The SC6100’s integration with[Xinnor’s xiRAID](https://xinnor.io/what-is-xiraid/) and Lustre creates a "Green Efficiency" effect. By maximizing GPU utilization with fewer physical servers, the university reduces power and cooling OPEX expenditures while accelerating innovation in fields like quantum chemistry and biomedical image analysis.


## Looking Ahead


With the Helma supercomputer now live, NHR@FAU is equipped to support the next generation of AI research. The inclusion of PCIe Gen 5 NVMe within the Celestica controllers ensures the system is future-proof, leaving significant headroom for future expansions as AI models continue to grow in complexity. Moreover, MEGWARE and Xinnor are moving forward with similar high-performance storage solutions using the SC6100 for other customers with demanding HPC and AI workloads.


The Helma project proves that with the right hardware partner, academic institutions can achieve "exa-scale" performance without the "exa-scale" footprint or budget.


To learn more, and dive into the details of this solution,[read the full case-study](https://www.celestica.com/uploadedFiles/Site/our-expertise/platform-solutions/case-studies/Xinnor_Case_Study.pdf) .


- [ai](https://www.celestica.com/blog/list/tag/ai)


- [hpc](https://www.celestica.com/blog/list/tag/hpc)


- [storage](https://www.celestica.com/blog/list/tag/storage)
