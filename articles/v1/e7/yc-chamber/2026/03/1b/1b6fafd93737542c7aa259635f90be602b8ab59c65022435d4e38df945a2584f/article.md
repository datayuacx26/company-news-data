---
schema_version: "1.0.0"
document_id: "1b6fafd93737542c7aa259635f90be602b8ab59c65022435d4e38df945a2584f"
company_key: "yc-chamber"
company: "Chamber"
source_id: "yc-chamber-news-import-3bd200813650"
canonical_url: "https://usechamber.io/blog/mig-vs-mps-vs-time-slicing-gpu-sharing-kubernetes"
published_at: "2026-03-03T00:00:00+00:00"
first_seen_at: "2026-07-24T23:55:17.000424+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:6d6bd8383b018b93921d9ffe58a52c3ca1a6a8cb007119136e961146d16e19b8"
---

# MIG vs MPS vs Time-Slicing: GPU Sharing for Kubernetes

# MIG vs MPS vs Time-Slicing: GPU Sharing Strategies for Kubernetes


A single H100 costs $30-50 per hour. When a researcher runs a notebook that uses 10% of that GPU, the other 90% sits idle. Multiply that across a fleet of hundreds of GPUs, and the waste adds up to millions annually.


Kubernetes treats GPUs as indivisible resources by default. One container gets one full GPU, regardless of how much compute it needs. Three NVIDIA technologies solve this problem, but each makes different trade-offs in isolation, performance, and hardware compatibility.


This guide explains how Multi-Instance GPU (MIG), Multi-Process Service (MPS), and time-slicing work, compares them across the dimensions that matter for production Kubernetes clusters, and provides a decision framework for choosing the right approach.


## The GPU Sharing Problem in Kubernetes


By default, the[NVIDIA device plugin for Kubernetes](https://github.com/NVIDIA/k8s-device-plugin) advertises each physical GPU as a single schedulable resource. When a pod requests` nvidia.com/gpu: 1` , it gets exclusive access to an entire GPU.


This creates a mismatch with how teams use GPUs. Many workloads, including inference serving, interactive notebooks, small training runs, and data preprocessing, need a fraction of a GPU's capacity. A notebook running exploratory analysis might use 5-10% of an A100's compute. An inference endpoint serving low-traffic predictions might need 2GB of an 80GB GPU's memory.


Without sharing, clusters waste capacity on partially utilized GPUs. Teams either overprovision (one GPU per small workload) or queue behind each other waiting for exclusive access. We talk to teams running thousands of GPUs where utilization hovers at 40-50% because of this exact dynamic.


The first step is visibility. You cannot optimize GPU sharing without knowing which GPUs are underutilized, which workloads are consuming what, and where capacity is sitting idle.[Chamber](https://www.usechamber.io/) provides real-time utilization monitoring across every GPU in your fleet, broken down by team, workload, and cluster, so you can see exactly where sharing will have the most impact before choosing a strategy.


NVIDIA provides three approaches that solve this differently: hardware partitioning (MIG), software sharing (MPS), and temporal sharing (time-slicing). Each operates at a different level of the stack, and understanding those differences determines which one fits your workloads.


## Multi-Instance GPU (MIG): Hardware Partitioning


MIG partitions a physical GPU into up to seven isolated instances, each with dedicated compute SMs, memory bandwidth, and L2 cache. This is hardware-level isolation. Each MIG instance behaves like a smaller, independent GPU with its own resources that cannot be accessed by other instances ([NVIDIA MIG User Guide](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/) ).


MIG requires NVIDIA data center GPUs from the Ampere generation or later. Supported models include the A100, A30, H100, H200, and B200. Older data center GPUs like the V100 and T4, and all consumer GPUs, do not support MIG.


### MIG Partition Profiles


Each GPU model supports specific partition profiles that define how the GPU is divided. You choose from predefined configurations rather than arbitrary splits ([NVIDIA MIG Supported Profiles](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html) ).


Profile Compute (SMs) Memory Use Case


**A100-80GB: 1g.10gb** 1/7 of SMs 10 GB Small inference, notebooks


**A100-80GB: 2g.20gb** 2/7 of SMs 20 GB Medium inference, small training


**A100-80GB: 3g.40gb** 3/7 of SMs 40 GB Large inference, medium training


**A100-80GB: 4g.40gb** 4/7 of SMs 40 GB Training with more compute


**A100-80GB: 7g.80gb** All SMs 80 GB Full GPU (no sharing)


**H100-80GB: 1g.10gb** 1/7 of SMs 10 GB Small inference, notebooks


**H100-80GB: 3g.40gb** 3/7 of SMs 40 GB Large inference, medium training


**H100-80GB: 7g.80gb** All SMs 80 GB Full GPU (no sharing)


The strongest argument for MIG is isolation. Each instance has its own memory space, compute resources, and fault domain. If one instance crashes or hits an out-of-memory error, other instances on the same physical GPU continue running unaffected.


The trade-off is rigidity. Changing MIG profiles requires resetting the GPU, which means draining all workloads first. You also cannot create arbitrary partition sizes. If your workload needs 15GB of memory on an A100, you must use the 2g.20gb profile and accept 5GB of unused memory per instance.


Choosing the right MIG profiles requires understanding your actual workload distribution. Chamber's historical usage analytics show memory and compute consumption per workload over time, so teams can match MIG partition sizes to their most common workload patterns rather than guessing.


## Multi-Process Service (MPS): Software Sharing


MPS is a binary-compatible CUDA implementation that routes multiple processes through a single CUDA context on the GPU ([NVIDIA MPS Documentation](https://docs.nvidia.com/deploy/mps/index.html) ). Instead of each process getting its own CUDA context (which consumes GPU resources), MPS multiplexes them through a shared context managed by an MPS server process.


MPS supports up to 48 concurrent clients per GPU on Volta and later architectures (16 on pre-Volta). It works on all NVIDIA data center GPUs, not just Ampere and later. This makes it the only option for GPU sharing with resource controls on V100 and T4 hardware.


### How MPS Isolation Works


MPS provides software-level resource controls rather than hardware isolation:


- **Active thread percentage.** Administrators can limit each client to a percentage of the GPU's compute resources. A client configured with 25% active threads cannot exceed that allocation even if the GPU has idle compute capacity.
- **Pinned memory limits.** Each client can be capped at a specific amount of GPU memory, preventing one process from consuming all available memory.


These controls are enforced in software by the MPS server, not by hardware. That means MPS does not provide fault isolation. If one client writes to corrupted memory addresses or triggers a GPU fault, all other clients sharing that GPU are affected. The entire GPU must be reset.


CUDA programs do not need modification to run under MPS. The MPS server intercepts CUDA calls and routes them through the shared context, making MPS transparent to applications. This is a strong fit for batch inference workloads where many small processes need concurrent GPU access without code changes.


The trade-off: MPS gives you granular sharing on any GPU with minimal performance overhead (estimated 1-5% from the shared context), but without the safety net of hardware isolation.


## Time-Slicing: Temporal Sharing


Time-slicing is the simplest GPU sharing mechanism. The GPU Operator's time-slicing feature oversubscribes a GPU by advertising it as multiple replicas through the[NVIDIA device plugin](https://github.com/NVIDIA/k8s-device-plugin) ConfigMap ([NVIDIA GPU Operator: Time-Slicing](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html) ).


When multiple pods land on a time-sliced GPU, they interleave execution through CUDA context switching. Each pod gets its turn on the GPU, similar to how a CPU time-slices between processes. The GPU rapidly switches between CUDA contexts, giving each workload a share of execution time.


### Configuration


Setting up time-slicing requires only a ConfigMap change. No GPU reset, no special hardware, no daemon processes:


```text
apiVersion: v1
kind: ConfigMap
metadata:
name: gpu-sharing-config
data:
config: |
version: v1
sharing:
timeSlicing:
resources:
- name: nvidia.com/gpu
replicas: 4


```


This advertises each physical GPU as four schedulable units. Pods request` nvidia.com/gpu: 1` as normal, and up to four pods share each physical GPU.


That simplicity comes at a cost. Time-slicing provides no memory isolation and no fault isolation. All pods sharing a GPU access the same memory space. One pod allocating too much GPU memory can cause out-of-memory errors for others. A crash in one pod can corrupt the shared GPU state.


Performance also degrades under contention. CUDA context switching introduces overhead, and when multiple workloads actively compete for the GPU, each experiences latency spikes and reduced throughput. Bursty, intermittent workloads (like notebooks) time-slice well. Sustained compute workloads (like training) suffer more because the context switching never lets them build momentum.


## Comparison Table: MIG vs MPS vs Time-Slicing


Feature MIG MPS Time-Slicing


**Isolation type** Hardware Software None


**Memory isolation** Yes (dedicated per instance) Configurable limits (software) No


**Fault isolation** Yes No No


**Performance overhead** None (dedicated hardware) Low (~1-5%) Medium (context switching)


**Supported GPUs** Ampere+ (A100, A30, H100, H200, B200) All NVIDIA data center GPUs All NVIDIA GPUs


**Max partitions/clients** 7 (A100/H100) 48 Configurable replicas


**K8s integration** Device plugin + MIG manager MPS-compatible device plugin GPU Operator ConfigMap


**Reconfiguration** Requires GPU reset Dynamic Dynamic


**Application changes** None None (binary-compatible) None


**Best for** Production inference, SLA workloads Multi-user batch inference, shared dev Notebooks, experiments, low-priority jobs


The three approaches form a clear spectrum. MIG provides the strongest guarantees but requires specific hardware and static configuration. Time-slicing offers the most flexibility but the weakest guarantees. MPS sits in the middle, providing meaningful resource controls without hardware requirements.


## When to Use Each Approach


### Use MIG When...


You need guaranteed performance isolation for production workloads. MIG is the right choice when running inference endpoints with SLAs, where one model's traffic spike must not degrade another model's latency. It is also the right choice when multiple teams share expensive GPUs and need hard guarantees that their allocation is protected.


If your fleet runs A100s or H100s, MIG should be your default for production workloads. The hardware isolation eliminates an entire class of noisy-neighbor problems that plague shared GPU environments.


The constraint is partition rigidity. If your workload memory needs do not align with the available MIG profiles, you waste capacity within each instance. Plan your MIG strategy around your most common workload sizes.


### Use MPS When...


Your fleet includes V100s or T4s and you need concurrent GPU sharing with resource controls. MPS is your only option on these older architectures.


MPS also works well when running many small, independent inference processes on newer GPUs. Batch inference pipelines where dozens of model instances serve different request streams benefit from MPS's transparent CUDA compatibility. Existing inference code runs without modification.


The risk is the shared fault domain. In environments where reliability matters more than density, a single misbehaving process can take down all workloads on that GPU. For production inference with uptime requirements, MIG is the safer bet if your hardware supports it.


### Use Time-Slicing When...


You need the simplest possible setup for non-critical workloads. Time-slicing is ideal for development namespaces where researchers run notebooks, prototype models, and experiment with training configurations. These workloads are typically bursty (active for minutes, idle for hours) and tolerant of performance variability. A ConfigMap change gets you GPU sharing in minutes.


Avoid time-slicing for production inference or training. The context switching overhead and lack of isolation make it unsuitable for workloads with latency or reliability requirements.


### Combining Approaches


The most effective GPU sharing strategies combine multiple approaches across different node pools or namespaces:


- **MIG for production, time-slicing for development.** Partition production GPUs with MIG for isolated inference serving. Time-slice development GPUs so researchers can share without infrastructure overhead.
- **MIG + time-slicing on the same GPU.** Partition a GPU with MIG, then time-slice individual MIG instances for even finer granularity. An A100 with 7 MIG instances, each time-sliced 2x, yields 14 schedulable GPU units from a single physical card.
- **KAI Scheduler for fractional GPU scheduling.** The[KAI Scheduler](https://github.com/NVIDIA/KAI-Scheduler) adds another layer by allowing pods to request GPU fractions (e.g., 0.5 GPUs or a specific memory amount) at the scheduling level. This works on any NVIDIA GPU without hardware reconfiguration. KAI does not enforce memory isolation — it relies on workloads staying within their requested allocation — so it complements rather than replaces MIG or MPS for isolation-sensitive environments.


This layered approach matches sharing strategy to workload criticality rather than applying one method across the entire cluster. Chamber's per-workload utilization data and team-level resource tracking help identify which node pools need MIG isolation for production SLAs and which can use time-slicing for bursty development workloads.


## Implementing GPU Sharing in Kubernetes


Each approach integrates with Kubernetes through the NVIDIA device plugin and GPU Operator, but the implementation paths differ.


### MIG Implementation


1. Enable MIG mode on target GPUs using` nvidia-smi -mig 1` (requires GPU reset).
2. Create MIG instances with the desired profiles using` nvidia-smi mig -cgi` and` nvidia-smi mig -cci` .
3. Deploy the NVIDIA MIG manager through the GPU Operator to automate profile management.
4. Configure the device plugin to advertise MIG instances as schedulable resources.


Pods then request specific MIG profiles (e.g.,` nvidia.com/mig-1g.10gb: 1` ) instead of full GPUs.


### MPS Implementation


1. Deploy an MPS-compatible configuration through the[NVIDIA device plugin](https://github.com/NVIDIA/k8s-device-plugin) .
2. Configure resource limits (active thread percentage, memory caps) per client.
3. The MPS server daemon runs on each node, managing GPU access for all pods.


### Time-Slicing Implementation


1. Create a ConfigMap defining replica counts per GPU resource.
2. Patch the cluster policy to reference the ConfigMap via the GPU Operator ([NVIDIA GPU Operator: Time-Slicing](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html) ).
3. Pods request GPU resources as normal. The scheduler places them on available replicas.


[Google Cloud's GKE documentation](https://cloud.google.com/kubernetes-engine/docs/concepts/timesharing-gpus) provides additional guidance on GPU time-sharing in managed Kubernetes environments.


One consideration for multi-cluster environments: the NVIDIA device plugin and GPU Operator configure sharing at the cluster level. Organizations running GPU workloads across multiple clusters or cloud providers need to manage sharing configurations separately per environment. Chamber operates across clusters and cloud providers (on-prem, AWS, GCP, Azure), giving teams a single view of GPU utilization and workload placement regardless of where the hardware lives. Instead of managing sharing configurations in isolation per cluster, platform teams can see fleet-wide utilization patterns and make informed sharing decisions from one dashboard.


## Frequently Asked Questions


### Which GPU sharing method has the least performance overhead?


MIG has the least performance overhead because each instance gets dedicated hardware resources including compute SMs, memory, and L2 cache. There is no context switching or resource contention. MPS adds roughly 1-5% overhead from its shared CUDA context, while time-slicing introduces the most overhead from CUDA context switching under contention.


### Can I use MIG on consumer GPUs like RTX 4090?


No. MIG requires NVIDIA data center GPUs from the Ampere generation or later. Supported GPUs include the A100, A30, H100, H200, and B200. Consumer GPUs like the RTX 4090, even though they use the Ada Lovelace architecture, do not support MIG. For consumer or older data center GPUs like V100 or T4, use MPS or time-slicing instead.


### What happens if one container crashes with GPU sharing?


It depends on the sharing method. With MIG, a crash in one instance does not affect other instances because each has hardware-level fault isolation. With MPS and time-slicing, there is no fault isolation. A crash or memory corruption in one container can bring down all other containers sharing the same GPU.


### How do I choose between MIG and time-slicing for inference?


Choose MIG for production inference with SLA requirements. MIG provides guaranteed performance isolation, so one model serving traffic spikes will not degrade another. Choose time-slicing for development or staging inference where simplicity matters more than isolation. Time-slicing requires only a ConfigMap change and works on any GPU.


### How does the KAI Scheduler fit in with MIG, MPS, and time-slicing?


KAI operates at the scheduling layer rather than the GPU driver or hardware layer. It allows pods to request fractional GPUs (e.g., 0.5) or specific memory amounts, and places multiple pods onto the same GPU. KAI does not enforce memory isolation between processes, so it is complementary to MIG and MPS rather than a replacement. Use KAI for scheduling-level GPU sharing on any NVIDIA GPU, and layer MIG or MPS underneath when you need hardware or software isolation.


## Key Takeaways


- MIG provides the strongest isolation (hardware-level) but requires Ampere or later GPUs (A100, H100, H200, B200) and cannot be reconfigured without a GPU reset.
- MPS offers software-level sharing with configurable resource limits on all NVIDIA data center GPUs, but lacks fault isolation.
- Time-slicing is the simplest to set up (one ConfigMap change) but provides no memory or fault isolation and suffers from context switching overhead under contention.
- Choose your GPU sharing strategy based on three factors: isolation requirements, GPU generation, and workload criticality.
- Combining approaches (MIG for production, time-slicing for development) across different node pools gives the most flexibility.
- For multi-cluster or multi-cloud GPU environments, a unified control plane avoids managing sharing configurations separately per cluster.
- GPU sharing is foundational to cost efficiency. Without it, Kubernetes treats every GPU as indivisible, wasting capacity on partially utilized hardware.


## The Bottom Line


GPU sharing is not optional for cost-efficient multi-tenant Kubernetes clusters. The right strategy depends on your hardware, your isolation requirements, and your workload mix. MIG gives you the strongest guarantees on modern hardware. MPS unlocks sharing on older GPUs. Time-slicing gets you started in minutes for non-critical workloads. Most production clusters will combine at least two approaches.


## How Chamber Simplifies GPU Sharing


GPU sharing solves the hardware problem, but operating shared GPUs across teams and clusters introduces its own complexity.[Chamber](https://www.usechamber.io/) is a GPU infrastructure observability and orchestration platform built to close that gap.


- **See utilization before you optimize.** Chamber provides real-time GPU utilization metrics broken down by team, workload, and cluster. Instead of guessing which GPUs are underutilized, platform teams can see exactly where sharing will have the most impact.
- **Pick the right MIG profiles.** Chamber's historical usage analytics show memory and compute consumption per workload over time, so teams can match MIG partition sizes to actual workload patterns rather than estimating.
- **Detect faults before they cascade.** Automatic GPU health monitoring identifies failing nodes before they corrupt training runs or disrupt inference on shared GPUs, which matters especially for MPS and time-sliced environments where fault isolation is limited.
- **Operate across clusters and clouds.** Chamber works across on-prem, AWS, GCP, and Azure environments, giving teams a single view of GPU utilization and workload placement instead of managing sharing configurations per cluster in isolation.
- **Fractional GPU scheduling built in.** Jobs submitted through Chamber can request a fraction of a GPU (e.g., 0.5 or 0.25) or a specific amount of GPU memory. Chamber places multiple workloads onto the same GPU automatically, combined with priority-based orchestration, and preemptive queuing.


Chamber deploys via a single Helm command with no code changes required.


## Sources


- NVIDIA. "Multi-Instance GPU User Guide."[https://docs.nvidia.com/datacenter/tesla/mig-user-guide/](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/)
- NVIDIA. "MIG Supported Profiles."[https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html)
- NVIDIA. "Multi-Process Service (MPS) Documentation."[https://docs.nvidia.com/deploy/mps/index.html](https://docs.nvidia.com/deploy/mps/index.html)
- NVIDIA. "GPU Operator: Time-Slicing GPUs in Kubernetes."[https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html)
- NVIDIA. "k8s-device-plugin." GitHub.[https://github.com/NVIDIA/k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin)
- NVIDIA. "KAI-Scheduler." GitHub.[https://github.com/NVIDIA/KAI-Scheduler](https://github.com/NVIDIA/KAI-Scheduler)
- NVIDIA. "KAI-Scheduler GPU Sharing." GitHub.[https://github.com/NVIDIA/KAI-Scheduler/tree/main/docs/gpu-sharing](https://github.com/NVIDIA/KAI-Scheduler/tree/main/docs/gpu-sharing)
- Google Cloud. "GPU sharing strategies in GKE."[https://cloud.google.com/kubernetes-engine/docs/concepts/timesharing-gpus](https://cloud.google.com/kubernetes-engine/docs/concepts/timesharing-gpus)
