---
schema_version: "1.0.0"
document_id: "d2b6d7f5b0cd85b00c9d9c4e2c6044a98637e858334dee2f89ef26bca806bb4e"
company_key: "yc-cedana"
company: "Cedana"
source_id: "yc-cedana-news-import-d2b6a3d3a0df"
canonical_url: "https://cedana.com/blog/supercharging-scientific-computing/"
published_at: "2025-06-09T00:00:00+00:00"
first_seen_at: "2026-08-18T05:08:30.961906+00:00"
fetched_at: "2026-08-18T05:08:32.098954+00:00"
content_hash: "sha256:66542ade475101f0ad9ee24e740a535c01fe2ea53f8a7e3d23a2e4227e3693e8"
---

# Supercharging Scientific Computing with Cedana

At Cedana, our bread and butter is checkpoint/restore and live migration to optimize your GPU/CPU clusters. Check out our website (cedana.com) to learn more about what we’re building!


### Scientific Computing - A Multivariate Problem


Generally, we at Cedana see scientific computing as a way for research to keep up with the increasing complexity of the real world. Modeling multivariate and complex systems via physically simulated models (that often are already approximations) breaks down when getting more fine-grained and when investigating interdependent phenomena.


[Newton](https://en.wikipedia.org/wiki/Newton_(Blake)) , - William Blake


Trying to bridge the gap between theory and the real world depends then on being able to simulate phenomena at scale, which quickly becomes a thorny multi-disciplinary problem. It combines systems engineering, whatever core scientific discipline (biology <> compBio, physics <> CAD, etc) and software; a skillset in rare supply.


Problems with running these systems at scale have been solved to varying degrees - but remains a messy field. At Cedana, we’re thinking about how we can solve a few pertinent problems for the space where it is right now:


- SLURM reigns supreme for HPC, and from our conversations with reseachers it tends to be what they’re most comfortable with. Squaring the circle with modern workloads and workflows though isn’t always easy.
- Labs are looking for more ways to integrate SOTA ML systems into existing workflows (i.e protein synthesis and simulations w/ LLM training and inference).
- Balancing multiple research groups with varying compute requirements, skillsets and experience.


All of these combine to slow down **time-to-insight** . For scientific compute, we’re fundamentally seeking to answer the question:


If I (as a PI or lab admin) have invested $200K of grant money into a DGX cluster, how soon can I see insights? How soon after can I get something into a clinical trial?


### Leveraging Kueue


Part of our engineering philosophy when designing Cedana has been to keep our systems flexible and maintain separation of concerns. We’ve built some incredibly performant low-level tooling around GPU checkpoint/restore and live migration, network migration and more - leaving room to quickly scaffold and integrate in existing systems.


One example is with Kubernetes, and specifically to Scientific Computing - Kueue.


[Kueue](https://kueue.sigs.k8s.io/) is a cloud-native job queueing system for batch, HPC, AI/ML in Kubernetes. It sets out to solve some of the pain points with bringing HPC over to cloud-native, seeing serious adoption at places like[Apple](https://youtu.be/GYiuTQCvTx8) and[CERN](https://youtu.be/xMmskWIlktA) .


Kueue introduces familiar concepts like job queues and resource quotas into Kubernetes, allowing you to manage shared resources across multiple tenants or research groups. It provides the primitives to smooth over the pain points of bringing a SLURM-like workflow into a modern, container-orchestrated environment.


### Dynamic Compute


In any multi-tenant system though, priorities change. A critical job needs to run now, and that means something else has to be booted off the nodes. Kueue has primitives for this. It can issue a preemption notice, terminate a lower-priority pod, and allow a higher-priority one to take its place.


The problem is that vanilla Kubernetes preemption is a disruptive operation. The application running in the pod receives a` SIGTERM` signal and is expected to shut down gracefully. For a stateless web server, that's generally OK, but for a multi-body simulation that’s been chugging away for 72 hours, a sudden` SIGTERM` means all that progress is lost unless the application itself has been meticulously engineered with its own checkpointing logic. This puts a heavy burden on the researchers and developers to build resilience into their applications, which is often not feasible for complex or legacy codebases.


The cost of a preemption


72 hours


of progress lost


A multi-body simulation, wiped by a sudden SIGTERM with no application-level checkpointing.


This is where Cedana comes in. We work with Kueue to make its preemption capabilities truly non-disruptive. Instead of just sending a` SIGTERM` and hoping for the best, Cedana hooks into the preemption notice to trigger a system-level checkpoint.


Our technology safely freezes the entire state of the running workload—the process memory, CPU registers, and even the complex state of the GPU—and saves it to storage. The pod can then be safely evicted. When resources become available again, or when the workload is rescheduled on a different node (even in a different cluster!), Cedana restores that saved state, and the application resumes execution as if nothing ever happened.


The workload is completely unaware it was ever stopped and moved.


### How it Works in Practice: The Caltech Use Case


At Caltech, we're seeing this play out in a real-world scientific environment. They are running a mixed workload pipeline: using GROMACS and Boltz-2 in tandem.


And so:


- The Challenge: How do you fairly schedule and, when necessary, preempt a 30y old FORTRAN/C-based MPI application alongside a modern Python/CUDA workload? Asking a research group to refactor GROMACS for cloud-native checkpointing is a non-starter.
- The Solution: Caltech uses Kueue to manage the queue and enforce fair-sharing policies between research groups. When a high-priority job comes in, Kueue initiates a preemption. Cedana intercepts this, performs a live checkpoint of the GROMACS or Boltz-2 job, and allows the eviction to proceed safely.
- The Result: The lower-priority job is safely paused. Once the high-priority job is complete, the preempted workload is restored from its checkpoint and continues on its merry way. Time-to-insight is accelerated because expensive GPU cycles aren't wasted on rerunning failed jobs, and the cluster can be managed with the flexibility modern research demands.


By combining the Kubernetes-native scheduling of Kueue with Cedana's transparent checkpoint/restore capabilities, you can build a truly robust and efficient platform for scientific computing. You give researchers the flexibility they need without forcing them to become distributed systems engineers, and you ensure that your $200K DGX cluster is always running at its maximum potential.


On top of all this, they ran the cluster on Spot instances - saving ~80% in costs.


The Caltech use case


~80%


cost savings


Running the mixed GROMACS and Boltz-2 pipeline on Spot instances.


While keeping a **$200K DGX cluster** at its maximum potential.
