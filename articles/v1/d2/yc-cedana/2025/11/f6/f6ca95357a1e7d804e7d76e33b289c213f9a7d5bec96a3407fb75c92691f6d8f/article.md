---
schema_version: "1.0.0"
document_id: "f6ca95357a1e7d804e7d76e33b289c213f9a7d5bec96a3407fb75c92691f6d8f"
company_key: "yc-cedana"
company: "Cedana"
source_id: "yc-cedana-news-import-d2b6a3d3a0df"
canonical_url: "https://cedana.com/post/the-utilization-ceiling-why-ai-and-hpc-schedulers-hit-30-and-how-to-fix-this/"
published_at: "2025-11-12T00:00:00+00:00"
first_seen_at: "2026-07-24T23:42:29.819860+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:c931dbfb909c51a32832588839b08f3b6385094e66d7d18bcbadac81a70cba5f"
---

# The Utilization Ceiling: Why AI and HPC Schedulers Hit 30% and How to Fix This

cedana / blog


# The Utilization Ceiling: Why AI and HPC Schedulers Hit 30% and How to Fix This


GPU utilization across AI and HPC workloads is fundamentally capped at 30% because schedulers cannot migrate running jobs. Cedana's CPU and GPU migration capability surpasses this limitation, unlocking near-full utilization.


[All posts](https://cedana.com/blog/)[Schedule a demo](https://cedana.com/connect/)[Read the docs](https://docs.cedana.ai/)


Published 2025-11-12 by Cedana


There is no shortage of blogs or LinkedIn posts concerning this topic, so why write another one? Based on our customer experience across Fortune 100 firms, academic centers, and AI research labs, there’s a less obvious yet more pertinent cause to underutilization.


Despite decades of optimization, world-class HPC schedulers such as Slurm, Kueue, Ray, and Kubernetes still fail to fully utilize their hardware. Average cluster activity remains below 60%, and, shockingly, GPU utilization[rarely surpasses 30%](https://www.bu.edu/peaclab/files/2025/07/GPU_Utilization_Analysis_PEARC25.pdf) .


This problem cannot be solved by scheduling alone, as schedulers[create](https://www.rc.fas.harvard.edu/blog/cluster-fragmentation/) cluster fragmentation. As jobs are launched or fail across a fixed pool of compute,[idle gaps begin to emerge](https://arxiv.org/html/2501.18191v1) . This fragmentation skews priority: large jobs wait in a queue, while smaller ones fill the cracks, leaving vast amounts of hardware underutilized and resources wasted.


This is due to the fact that schedulers can only predict what resources workloads might need, not adapt to how they actually behave once running. Once jobs start, resources remain statically bound, regardless of changing runtime conditions.


They’re constrained by their own inability to dynamically resize or rebalance clusters. The best they can do is estimate, leaving consequences of missed performance, added expenses, manual overhead, and significantly delayed R&D.


To truly maximize productivity, we need a system that goes beyond scheduling: one that can break the **scheduler bottleneck** .


With the ability to save, migrate, and resume CPU and GPU workloads Cedana breaks this ceiling, enabling our customers to achieve over **80% utilization.**


Lets briefly touch on existing scheduler optimization techniques.


## Slurm


There is a vast amount of literature on how to[improve scheduling](https://dl.acm.org/doi/abs/10.1145/3219104.3219111) and[utilization](https://acta.uni-obuda.hu/Emodi_Banfi_Kovacs_164.pdf) for Slurm, given its significant role in AI and HPC over the past 20 years.


One of the key challenges is that ****[Slurm’s fair-share scheduling](https://slurm.schedmd.com/fair_tree.html) was designed to allocate resources equitably, but in practice it often leaves GPUs stranded.


If a user group is allocated a cluster of GPUs, they may start as being fully utilized. However, as some jobs finish, these GPUs stay reserved, remaining idle under the ownership of the same group and forcing other jobs to wait in a queue. These idle GPUs remain unclaimed because Slurm lacks the ability to transparently checkpoint and migrate workloads. This undermines utilization, prolongs R&D, and delays valuable insights.


While some[checkpointing mechanisms have been developed to break this bottleneck](https://hpc.rz.rptu.de/documentation/checkpoint_blcr.html) , they require onerous workflow changes including manual checkpointing and restarts. This greatly disrupts the user experience, adding overhead and frustration. Additionally, to our knowledge, none of these systems support GPU or distributed workloads.


## Kubernetes


Similarly to Slurm, the Kubernetes’ default scheduler suffers from fragmentation: once a pod is placed the workload cannot be moved. As they cannot migrate, clusters accumulate “holes” of unusable capacity between long-lived pods.


This[post](https://kubernetes.io/blog/2023/11/16/the-case-for-kubernetes-resource-limits/) from the CNCF (stewards of Kubernetes) exemplifies how static requests and limits create persistent waste. Entire companies now exist to predict, right-size, and pre-plan utilization. Nevertheless, they're all stuck in the same paradigm of trying to guess resource allocation.


This regularly results in persistent issues of under- or over-allocation. Failures, maintenance, and other factors make this even more inefficient and difficult to solve. Until workloads can migrate while running, Kubernetes utilization remains capped.


## Ray


Coincidingly, while Ray was designed for dynamic AI workloads, it cannot move live jobs once they have been scheduled. The official[Ray scheduling docs](https://docs.ray.io/en/latest/ray-core/scheduling/index.html) describe placement logic that scores nodes before startup, not during execution. In production, this leads to **stranded capacity** .


Uber’s engineering team describes in[Uber’s Journey to Ray on Kubernetes](https://www.uber.com/blog/ubers-journey-to-ray-on-kubernetes-ray-setup/) how they had to **detect idle Ray clusters and tear them down** to reclaim GPU capacity: a reactive fix for a system that can’t migrate active work. Instead of shifting jobs to fill gaps, Ray destroys idle clusters entirely, leading to redundancy, churn, and lower throughput.


## Kueue


[Kueue](https://kueue.sigs.k8s.io/) is Kubernetes’ batch scheduler. It’s the CNCF’s answer to Slurm. However, it inherits the same immobility and assumption of stateless workloads; jobs are bound to nodes until completion or preemption.


There’s no continuous checkpointing or migration mechanism, so utilization remains limited by static scheduling logic. Kueue can reorder pending work, but it can’t touch running work. As a result, inefficiencies persist just as they do in core Kubernetes, and while possessing a fair sharing mechanism, it suffers the same issues as Slurm does.


## How to Solve This


**Cedana breaks the long running scheduler bottleneck** .


Cedana seamlessly enhances existing schedulers with transparent save-migrate-resume capabilities, breaking the critical bottleneck that keeps utilization low.


Jobs can be paused, moved, and resumed across nodes or clusters , without restarting or losing progress. This unlocks continuous repacking of clusters as jobs finish, GPUs fail, or demand changes. The result is **higher utilization, higher throughput, and fewer wasted GPU hours** , all without modifying user code or rewriting scheduler logic.


Cedana’s solution is designed from the ground-up working for High Performance Compute and large scale AI workloads, and works for **distributed CPU and GPU workloads** .


A critical criteria for our customers is that it works with their existing schedulers: Kubernetes, Slurm, Kueue, Ray, Armada, and others. No changes to code, configuration, or user workflows. Works on distributed inference, training, and HPC workloads.


## Conclusion


Utilization is not solely a scheduling problem. It’s a dynamic **state** problem. Schedulers rely on predictions resulting in under- or over- allocation, and are ultimately limited in their ability to improve utilization.


Cedana’s **Compute Shaping** solution dynamically migrates and elastically resizes compute based on real-time resource availability, performance and resource requirements, while enforcing job-level SLAs and priorities.


With Cedana, compute becomes durable and elastic in the way storage and networking already are: continuously adapting to real-time conditions and failure-resilient.


### About Cedana


Our team’s experience in AI and HPC spans over 20 years and includes:


- Developing the first published formal methods for guaranteeing convergence in distributed training.
- Peer-reviewed Computational Biology HPC and AI research.
- Building robots and warehouse automation systems deployed in real-world environments at Shopify.
- Building large-scale robotics and computer vision systems at MIT CSAIL, creating clusters and resource management software from the ground up.
- Building power systems device drivers that are now shipping in millions of Apple products.
- Developing secure AI solutions for sensitive patient data, deployed in highly regulated healthcare systems across the country.
- Publishing in NeurIPS, ICCV, CVPR, and other leading conferences and journals.


Our multi-disciplinary team has unique expertise, enabling us to build breakthrough distributed CPU and GPU job migration capabilities.
