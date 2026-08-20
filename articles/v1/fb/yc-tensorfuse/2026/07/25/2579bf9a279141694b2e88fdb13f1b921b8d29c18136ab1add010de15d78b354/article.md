---
schema_version: "1.0.0"
document_id: "2579bf9a279141694b2e88fdb13f1b921b8d29c18136ab1add010de15d78b354"
company_key: "yc-tensorfuse"
company: "Tensorfuse"
source_id: "yc-tensorfuse-news-import-97ab2b67de47"
canonical_url: "https://tensorfuse.io/docs/blogs/snapshotter_graphs"
published_at: null
first_seen_at: "2026-07-22T16:02:52.953662+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:daacb6669b434c7898e7c94e5d18ac1b9c6a88899c0f3a993895493c12c5dc07"
---

# Introducing Fastpull: Start AI containers in seconds

## ​


The Cold Start Problem


AI and ML container images like vLLM, SGLang, and TensorRT-LLM are large, typically exceeding 10 GB. With traditional Docker using overlayfs, pulling a 10 GB image from a registry to an instance takes 7-10 minutes. This delay creates three critical problems for production AI deployments:


**Overprovisioning of GPU infrastructure:** To handle traffic spikes without user-facing latency, operators maintain 3-7x more GPU capacity than base load requires. On AWS, an H100 GPU instance costs $7.57/hour, meaning a deployment provisioned for 100 requests/second at peak but averaging 30 requests/second wastes approximately $1,634 per day per 8-GPU instance in idle costs.


**High idle costs during scale-down:** GPU instances cannot be immediately terminated after traffic decreases because the next spike would trigger 7-10 minute cold starts. Operators extend scale-down windows to 5-20 minutes, accumulating significant idle compute costs.


**Poor user experience during traffic spikes:** When traffic exceeds provisioned capacity, new requests queue for minutes while containers start. For latency-sensitive inference workloads, this is unacceptable.


### ​


Cost Impact: H100 Example


Consider a typical AI inference service with variable traffic patterns:


**Traffic Pattern Assumptions:**


- Peak hours: 100 requests/second (8 hours/day during business hours)
- Base load: 30 requests/second (16 hours/day during off-peak)
- GPU capacity: Each H100 can handle ~12-15 requests/second for typical 8B parameter models at target latency
- Safety margin: 20% headroom to handle request bursts and maintain SLA


**Scenario 1: Overprovisioning with 10-Minute Cold Starts (Baseline)**


With 10-minute cold starts, scaling is impractical during traffic spikes. Missing a traffic surge means 10 minutes of degraded service, so operators provision for peak continuously:


- GPUs needed: 100 req/s ÷ 12 req/s per GPU × 1.2 safety = 8 H100 GPUs running 24/7
- Monthly cost: 8 GPUs × 7.57/hour×720hours=7.57/hour × 720 hours =


7.57/


h


o


u


r


×


720


h


o


u


rs


=


43,603/month
- Utilization breakdown:
- Peak hours (8h/day): 8 GPUs serving 100 req/s = 100% utilized
- Off-peak hours (16h/day): 8 GPUs serving 30 req/s = 31% utilized
- Wasted capacity: During 480 off-peak hours/month, 5.5 GPUs sit mostly idle
- Idle cost: 5.5 GPUs × 480 hours × 7.57=7.57 =


7.57


=


19,994/month in wasted compute


**Scenario 2: Optimized Autoscaling with 3-Minute Cold Starts**


With 3-minute cold starts (using lazy loading snapshotters + high-throughput storage), autoscaling becomes viable with careful buffer management:


- Base capacity: 30 req/s ÷ 12 req/s per GPU × 1.2 = 3 GPUs, but bump to 4 GPUs to handle the 3-minute scale-up lag during traffic ramps
- Peak scaling: Add 4 additional GPUs during peak hours
- Scale-down strategy: 15-minute delay after traffic drops (prevents thrashing, balances responsiveness with cost)


Monthly cost breakdown:


- Base tier (4 GPUs running 24/7): 4 × 720 hours × 7.57=7.57 =


7.57


=


21,842
- Peak tier (4 additional GPUs for 8.25h/day): 4 × 247.5 hours × 7.57=7.57 =


7.57


=


7,494
- 8 hours peak + 0.25 hours scale-down delay per day
- Total monthly cost: $29,336/month
- **Savings: 43,603−43,603 - 43 , 603 − 29,336 = $14,267/month (33% reduction)**


## ​


Cold Start Stages


Complete cold start time consists of three stages:


1. **Node provisioning:** When autoscaling from zero or adding nodes, cloud providers take 80-120 seconds to provision and boot new GPU instances. This is entirely cloud-dependent and outside user control.


2. **Container start:** Time to pull container image from registry, extract layers, and reach first application log. This stage is the focus of this analysis.


3. **Model download and load:** Time to download model weights from repositories like Hugging Face and load them into GPU memory. Dependent on network bandwidth, disk I/O, and model size.


For a typical 10 GB container with a 16 GB model on standard infrastructure, total time is approximately 10 minutes using default Docker configuration. This blog focuses on optimizing stage 2 (container start) through lazy loading snapshotters.


### ​


Container Startup Breakdown


We measure container startup using five metrics:


- **Time to First Log:** Container reaches first application log output
- **First Log to Model Download Start:** Application initialization (library loading, CUDA setup)
- **Total to Model Download:** Sum of above two phases
- **Model Download:** Time downloading model weights from registry
- **Weights to GPU:** Time loading model weights into GPU memory
- **Graph Capture:** Framework-specific compilation (e.g., CUDA graph capture)


The primary optimization target is Time to Workload Start (sum of all phases).


## ​


The Lazy Loading Solution


Analysis shows that for typical AI workloads, 76% of startup time is spent downloading container images, yet only 6.4% of image data is accessed during application startup. This extreme inefficiency creates an optimization opportunity: defer downloading unused files until actually accessed.


### ​


Snapshotter Mechanisms


Snapshotters are containerd components that manage filesystem layers for containers. They determine when and how image data is retrieved from registries.


**OverlayFS (Eager Loading - Default)**


overlayfs downloads all compressed image layers before container start. It extracts each layer to local disk and stacks them to create a unified filesystem view. The container process begins only after complete download and extraction.


**SOCI (Index-based Lazy Loading)**


SOCI (Seekable OCI) operates without converting container images. It generates a separate index file mapping files to byte ranges within compressed layers. At container start, SOCI mounts the filesystem immediately. When the application requests a file not yet local, SOCI uses HTTP range requests to fetch only the necessary compressed bytes for that specific file.


**Nydus (Chunk-based Lazy Loading)**


Nydus requires converting OCI images to RAFS (Registry Acceleration File System) format. During conversion, Nydus splits files into deduplicated chunks and builds a metadata tree. The container mounts a FUSE-based filesystem. File access requests traverse the metadata tree, fetching only required data chunks on-demand. Nydus supports two backend modes: FUSE (userspace) and EROFS+fscache (kernel-based). EROFS+fscache requires custom-built kernels with experimental features enabled and provides better performance but reduced compatibility.


**eStargZ (Layer-based Lazy Loading)**


eStargZ embeds a table of contents within image layers. It requires full image conversion to eStargZ format. Similar to SOCI, it enables on-demand file fetching via HTTP range requests.


## ​


Benchmark Configuration


**Infrastructure**


- Instance: AWS EC2 g6e.xlarge (4 vCPUs, 32 GB RAM, 1 × NVIDIA L40S GPU with 48 GB memory)
- Storage: EBS gp3 volumes with two configurations tested:
- Low throughput: 125 MB/s, 3,000 IOPS (baseline gp3)
- High throughput: 1,000 MB/s, 4,000 IOPS (provisioned)
- AMI: Amazon Linux 2023 with kernel 6.1
- Container Registry: AWS Elastic Container Registry (ECR)
- Model Registry: Hugging Face with hf-xet for optimized download throughput


**Test Workloads**


Image Name Size Workload


vLLM Server 10 GB Serve Qwen2-8B model


SGLang Server 15 GB Serve Qwen2-8B model


TensorRT Server 30 GB Serve Qwen2-8B model


Triton Server 11 GB Run CSM 1B model


CUDA SAM2 Server 13 GB Serve SAM2 with FastAPI


Axolotl Finetuning 10 GB Finetune Qwen2.5-7B-Instruct


## ​


Results


### ​


Low Throughput Disk (125 MB/s, 3K IOPS)


SOCI and Nydus demonstrate 24-61% reduction in completion time compared to overlayfs across all workloads. The largest improvements occur with the largest images (TensorRT: 55% reduction with SOCI). eStargZ shows no improvement over overlayfs due to higher on-demand fetch overhead.


\[GRAPH: Bar chart comparing completion times across snapshotters for 125 MB/s disk configuration, grouped by workload\]


### ​


High Throughput Disk (1000 MB/s, 4K IOPS)


With high-throughput disks, Nydus and SOCI maintain 14-43% time reduction compared to overlayfs. The performance advantage narrows compared to low-throughput disks, indicating that network and application initialization become larger bottlenecks as disk I/O improves.


\[GRAPH: Stacked bar chart showing snapshotter + disk configuration combinations on X-axis, completion time on Y-axis, with clear comparison of how disk throughput affects each snapshotter\]


### ​


Detailed Analysis: vLLM Startup


**Low Throughput Disk (125 MB/s)**


Lazy loading reduces Time to First Log by 93-94% (200+ seconds). However, First Log to Model Download increases by 45-92 seconds due to on-demand fetching of libraries required for application initialization (CUDA, Python dependencies). Net reduction in Total to Model Download: 64% for SOCI, 47% for Nydus.


Model Download, Weights to GPU, and Graph Capture phases show similar durations across snapshotters. Graph Capture is compute-bound and unaffected by storage configuration.


**High Throughput Disk (1000 MB/s)**


Higher disk throughput reduces all phases except Graph Capture (compute-bound). Model Download time decreases by 74% (230s → 60s) due to faster write speeds to local storage. Lazy loading continues to provide 35-42% reduction in Total to Model Download even with high-performance storage.


\[GRAPH: Side-by-side phase breakdown comparing overlayfs vs soci vs nydus, showing how lazy loading shifts time from “Time to First Log” to “First Log to Model Download”\]


## ​


Disk Throughput Impact


Upgrading from 125 MB/s to 1,000 MB/s EBS gp3 storage yields:


- OverlayFS: 678s → 356s (47% reduction)
- Nydus: 583s → 306s (47% reduction)
- SOCI: 516s → 300s (42% reduction)


The performance gain from upgrading disk throughput exceeds the gain from switching to lazy loading on slower disks for most workloads. This indicates that disk I/O is the dominant bottleneck during model download and loading phases. On AWS EBS gp3, provisioning 1,000 MB/s throughput costs an additional $35/month per volume compared to baseline 125 MB/s.


\[GRAPH: Line chart showing completion time vs disk throughput for each snapshotter, demonstrating how disk performance affects different snapshotters\]


## ​


Parallel Pull: Optimized Eager Loading


SOCI provides a “Parallel Pull” eager loading mode using multiple concurrent download and decompression streams. On g6e.xlarge (4 vCPUs), performance was comparable to overlayfs. Testing on g6e.16xlarge (64 vCPUs, 256 GB RAM) with 1,000 MB/s disk:


Snapshotter SGLang Startup Time


overlayfs 270s


soci (lazy) 231s


nydus (lazy) 240s


soci (parallel pull) 221s


With sufficient CPU cores, SOCI parallel pull achieves the lowest startup time. However, the relative improvement over overlayfs is smaller (18%) than on smaller instances (43% on g6e.xlarge). This suggests that on high-CPU instances, overlayfs already achieves better parallelization, reducing the advantage of lazy loading.


\[DIAGRAM: Performance comparison chart showing how parallel pull scales with CPU cores compared to lazy loading and standard overlayfs\]


## ​


Implementation Considerations


### ​


Image Format Requirements


**SOCI:** No image conversion required. Generates separate index files stored alongside images in the registry. Compatible with existing OCI images.


**Nydus:** Requires converting OCI images to RAFS format. Adds an image management lifecycle step. Supports chunk-level deduplication, potentially reducing total storage for similar images.


**eStargZ:** Requires full conversion to eStargZ format. Adds conversion overhead without performance benefits observed in our benchmarks.


### ​


Filesystem Backend Trade-offs


**FUSE (Userspace):** All snapshotters support FUSE. Compatible with standard kernels. Performance overhead from userspace-kernel context switches.


**EROFS+fscache (Kernel):** Nydus supports EROFS with fscache for kernel-level I/O. Requires custom kernel builds with experimental features enabled. Improved performance but reduced operational simplicity.


### ​


Operational Dependencies


Lazy loading introduces runtime dependencies on registry availability. Network disruptions during container runtime can cause application failures if unaccessed files are subsequently requested. Overlayfs eliminates this risk by completing all downloads before container start.


## ​


Recommendations


### ​


For Large Container Images (>20 GB)


Lazy loading snapshotters provide 40-55% startup time reduction. SOCI is recommended for production deployments due to no image conversion requirement and OCI compatibility. For the 30 GB TensorRT image, SOCI reduced startup from 1430s to 638s on low-throughput disks (55% reduction).


### ​


For Large Models (>15 GB)


Disk and network throughput are primary bottlenecks. Upgrading from baseline EBS gp3 (125 MB/s) to provisioned throughput (1,000 MB/s) reduced model download and load times by 70%. Cost: $35/month per volume for throughput provisioning. This optimization is effective regardless of snapshotter choice.


### ​


For Multi-CPU Instances (>32 vCPUs)


SOCI parallel pull mode achieves the lowest startup times by leveraging high CPU core counts for concurrent decompression. On g6e.16xlarge, SOCI parallel pull outperformed lazy loading modes by 4-8%.


### ​


Combined Strategy


For production AI inference deployments on H100 instances with large images and models:


1. Use SOCI lazy loading for initial container start optimization (40-55% reduction)
2. Provision EBS gp3 with 1,000 MB/s throughput for model download acceleration (70% reduction in model load time)
3. Configure autoscaling with 60-120 second scale-down windows to balance cold start frequency with idle costs


This combination reduces total cold start time from 10+ minutes to under 2 minutes for typical 10 GB container + 16 GB model workloads, enabling aggressive scale-to-zero strategies that reduce monthly GPU costs by 40-50%.


\[DIAGRAM: Decision tree flowchart showing optimization paths based on bottleneck type - large container images lead to SOCI/Nydus, large models lead to disk throughput upgrades, high-CPU instances lead to parallel pull\]


## ​


Conclusion


Cold start optimization directly impacts AI infrastructure economics. Reducing startup times from 10 minutes to under 2 minutes enables scaling strategies that cut H100 deployment costs by $20,000+/month through reduced overprovisioning. The optimization approach must match the bottleneck: lazy loading snapshotters for large container images, high-throughput storage for large models, or parallel pull for high-CPU instances.


Fastpull provides scripts and configurations to replicate these benchmarks and implement lazy loading snapshotters on AWS, GCP, and Azure infrastructure. Implementation details and automation scripts are available at \[repository link\].


---


*Note: All benchmark data collected October 2025 on AWS EC2 g6e instances in us-east-1 region. Results may vary based on registry performance, network conditions, and specific workload characteristics.*
