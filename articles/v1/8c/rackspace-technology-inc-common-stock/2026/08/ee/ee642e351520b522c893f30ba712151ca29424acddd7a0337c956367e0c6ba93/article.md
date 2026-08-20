---
schema_version: "1.0.0"
document_id: "ee642e351520b522c893f30ba712151ca29424acddd7a0337c956367e0c6ba93"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/kubernetes-performance-optimization"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-19T06:26:55.490646+00:00"
fetched_at: "2026-08-19T06:26:56.849467+00:00"
content_hash: "sha256:4a8e2cd347ae743b81c3bc095efaed87e4eb18c9db02eb239ef5ba16e005962c"
---

# Kubernetes Performance Optimization: A Complete Guide to Tuning, Scaling, and Troubleshooting

*"How do I get a service that's consuming 90% of my resources under control?"*


Throw more resources at the service, and hope you find what's actually eating them before it gets any worse.


That answer works often enough to be tempting, but only in the short term. Extra headroom buys time, and a heavy workload sometimes genuinely needs the extra capacity. But adding capacity just never tells you what caused the spike in the first place.


A pod throttled on an idle node needs a different fix than one saturating a busy node, so you need to confirm which one the metrics actually point to before changing anything.


[Kubernetes](https://kubernetes.io/) performance optimization means finding what's actually constrained, whether that's compute, scheduling, autoscaling reaction time, or network and storage I/O, and fixing that specific constraint instead of guessing.


This guide covers each of these constraints in the order most engineers hit them in production.


Jump to the section that matches what you're looking for:


- Pods throttled, restarting, or getting OOM-killed →Understanding bottlenecks and profiling
- Requests and limits feel like guesswork →Resource configuration and QoS classes
- Pods stuck pending or slow to schedule →Scheduling optimization and node tuning
- Autoscaling too slow or too jumpy →Autoscaling and capacity planning
- Can't tell what's actually slow →Monitoring, metrics, and network performance
- Stateful workloads with slow disk I/O →Storage optimization


**Try it yourself:** a test cluster on[Rackspace Spot](https://spot.rackspace.com/) runs for as little as $0.72 a month, cheap enough to test out every tuning pattern in this guide somewhere other than production.


‍


## Kubernetes performance optimization: identifying bottlenecks and profiling workloads


Before fixing anything, confirm what's actually wrong.


Some people might skip straight to a fix like "adding more resources," without first separating an application-level problem from a cluster-level one, and end up tuning the wrong layer. So first you have to actually identify the problem.


‍


### Identifying common performance bottlenecks


A Kubernetes performance problem usually traces back to one of a handful of root causes:


- **CPU starvation:** containers hitting their CPU limit and getting throttled by the kernel, even when the node has spare capacity
- **Memory pressure:** a container approaching its memory limit, risking an Out of Memory (OOM) kill, or a node approaching capacity across all its pods
- **Network congestion:** CNI overhead, DNS latency, or service mesh sidecar cost adding up under load
- **Resource contention between pods and nodes:** multiple workloads competing for the same CPU cores or memory pages on a single node
- **Noisy-neighbor effects:** one workload in a multi-tenant cluster consuming enough shared resources to degrade everything else on the same node
- **App-level vs. cluster-level bottlenecks:** slow response times that look like a Kubernetes problem but actually trace back to a slow database query, a blocking I/O call, or inefficient application code


The fastest way to tell these apart is to check whether the symptom correlates with **node-level metrics** (CPU/memory saturation on the host) or stays isolated to **one pod's application logs** .


If the node has headroom and one pod is still slow, the bottleneck is almost always in the application, not the cluster.


‍


### Profiling Kubernetes workloads


When the application turns out to be the cause of slowness,[profiling](https://grafana.com/docs/pyroscope/latest/introduction/what-is-profiling/) identifies where inside the application the time actually goes:


- **` perf`** for CPU-level profiling of the container's process directly on the node
- **` pprof`** for Go and other language-native profilers that expose CPU and memory profiles over HTTP
- **eBPF-based profilers** (Parca, Pyroscope) for continuous, low-overhead profiling across every pod on a node without instrumenting each application individually


Always remember to correlate whatever the profiler shows against cluster metrics from the same time period.


A profiler reports how long each part of your application took to run, but it can't tell whether that code was genuinely slow or whether Kubernetes paused the container partway through due to CPU throttling.


If Prometheus shows the pod hitting its CPU limit during that same period, the problem is cluster-level, and the CPU limit is what needs changing rather than the application code.


Once you know what to change, you also have to confirm it worked. Record what normal performance looks like before touching anything. Otherwise, when the graph improves afterwards, you have no way to tell whether your fix did it or maybe traffic simply dropped off on its own.


‍


### CPU throttling and CPU starvation analysis


When you set a CPU limit, Linux enforces it by dividing time into 100-millisecond windows and giving the container a fixed budget of CPU time in each one. A container limited to 500m, meaning half a CPU, gets 50 milliseconds of CPU time per window.


Once the container spends that budget, Linux stops running it until the next window opens. It sits frozen for the remainder, even when the node has CPU to spare, because a limit is a hard cap rather than a ceiling that only applies when the node is under pressure.


That's what makes throttling hard to catch. The node dashboard shows idle CPU and looks healthy, while the application is being paused several times a second.


The kubelet counts **those pauses** in` container_cpu_cfs_throttled_periods_total` , and dividing them by the total number of windows gives the share of time a pod spent frozen:


```text
sum(rate(container_cpu_cfs_throttled_periods_total[5m]))     by     (pod)
/
sum(rate(container_cpu_cfs_periods_total[5m]))     by     (pod)
```


A result above 0.1 means the pod was frozen in more than 10% of its windows. Average CPU usage won't reveal it, since a pod running flat out for half of each window and frozen for the rest averages to a healthy-looking 50%.


Removing the limit entirely, leaving only a request, stops the freezing completely, but it also lets that container take whatever CPU it wants from everything else sharing the node.


‍


### Memory pressure and OOM kills


Memory doesn't have Kubernetes' equivalent of CPU throttling.


A container that exceeds its memory limit gets killed immediately by the kernel's out-of-memory (OOM) handler, not throttled and slowed down.


Overcommitting memory at the node level (setting requests below what workloads actually use, on the assumption they won't all peak simultaneously) is what usually sets up the conditions for an OOM kill under real load.


Two signals catch an at-risk pod before it gets killed:


- Memory usage trending upward without a corresponding drop, which usually means a leak rather than normal working-set growth
- Usage sitting consistently close to the configured limit, leaving no headroom for a traffic spike


When the kill does happen,` kubectl describe pod` shows` OOMKilled` in the last-terminated-state, and` kubectl get events --field-selector reason=OOMKilling` surfaces it across the namespace.


The bigger risk is cascading failure: an OOM-killed pod restarts, drops whatever in-memory cache or connection pool it had built up, and briefly increases load on whatever else was sharing traffic with it, sometimes tipping a second pod into the same limit.


Getting requests and limits right in the first place prevents most of this before it starts.


‍


## Resource configuration, requests, limits, and QoS classes


Every pod that reaches memory pressure or CPU throttling traces back to how its requests and limits were set.


Getting this right is the single highest-leverage fix in this guide, and it's also where the Reddit-thread instinct of "just raise the CPU request" causes the most damage, since raising a request without evidence just moves the waste around instead of removing it.


‍


### Setting accurate pod resource requests and limits


A **request** is what the scheduler uses to decide which node a pod can fit on; it's a reservation, not a cap. A **limit** is the hard ceiling the kubelet enforces at runtime.


Setting a request too low lets the scheduler overpack a node, since it never accounts for what the pod actually needs once it's running.


Setting a limit too low throttles CPU or triggers an OOM kill regardless of how much headroom the node has.


The scheduler only looks at requests when placing a pod, so a node can be fully committed on paper (every pod's request adding up to 100% of capacity) while running well under actual utilization, or the reverse, where requests look conservative but real usage is already near the node's limit.


Base both values on observed usage under real load, not a guess made before the workload ever ran in production.


‍


### Kubernetes QoS classes and their performance implications


Kubernetes assigns every pod one of three Quality of Service classes automatically, based on how its requests and limits are set, and that class determines eviction order when a node runs out of resources:


QoS class How it's assigned Eviction priority Best for


Guaranteed Requests equal limits for every container in the pod Evicted last


Latency-sensitive or stateful workloads that can't tolerate disruption


Burstable Requests set, but lower than limits (or limits unset) Evicted after BestEffort


Most general-purpose workloads with variable but bounded usage


BestEffort No requests or limits set at all Evicted first


Batch jobs and workloads that tolerate interruption


Burstable and BestEffort pods carry real performance risk for anything latency-sensitive, since they're the first candidates evicted the moment a node comes under memory pressure, regardless of how healthy the pod itself is.


Guaranteed QoS is also the prerequisite for CPU pinning through the CPU Manager's static policy, covered in the node-tuning section below, since exclusive CPU cores only get allocated to pods with an integer CPU request and a matching limit.


‍


### Rightsizing containers and workloads


Rightsizing means setting requests and limits based on what a workload has actually used historically, not what it might theoretically need at peak.


The[Kubernetes resource optimization](https://spot.rackspace.com/blog/kubernetes-resource-optimization) guide covers the mechanics of measuring that usage in more depth.


The Vertical Pod Autoscaler is the standard tool for generating a rightsizing recommendation, covered further in the autoscaling section below, but rightsizing itself is a process, not a one-time setting:


1. Measure actual CPU and memory usage over a representative window, at least a full week to catch weekday/weekend variance
2. Adjust requests and limits to match observed usage plus a reasonable buffer
3. Validate that the workload still meets its SLA under the new values
4. Repeat on a schedule, since usage patterns drift as traffic and code both change


Balancing rightsizing against cost fails in both directions. Too aggressive, and a legitimate traffic spike gets throttled; too conservative, and the node runs at 20% utilization while the bill reflects 100%.


‍


### Resource management strategies for multi-workload clusters


A single cluster running multiple teams' workloads needs namespace-level guardrails, not just per-pod tuning.


` LimitRange` sets default and maximum request/limit values for any container that doesn't specify its own, and` ResourceQuota` caps total resource consumption per namespace:


Priority classes and preemption add a second layer on top of this, since a high-priority workload can preempt a lower-priority one for scheduling room, which protects critical services from being starved out by whatever happened to request resources first.


None of this replaces getting individual pod requests and limits right, but it stops one team's misconfigured workload from degrading everyone else's.


‍


## Scheduling optimization, pod affinity, and node tuning


Correct resource requests only help if the scheduler can actually place pods quickly and onto the right nodes.


This section covers what happens between a pod being created and a pod actually running.


The[Introduction to Kubernetes architecture](https://spot.rackspace.com/blog/kubernetes-architecture) guide covers how the control plane and scheduler fit together at a broader level, if that context is missing.


‍


### Kubernetes scheduler performance and configuration


The scheduler filters nodes down to ones that satisfy a pod's requirements, then scores the survivors to pick the best fit. In small clusters that's cheap.


In clusters with hundreds or thousands of nodes, scoring every single node for every single pod becomes the bottleneck itself, which is what` percentageOfNodesToScore` exists to control:


By default (a value of 0), the scheduler uses a linear formula that scores roughly 50% of nodes in a 100-node cluster, down to about 10% in a 5,000-node cluster, with a hard floor of 5% or 100 nodes, whichever is larger.


Clusters under a few hundred nodes never hit this ceiling and don't need to touch the setting at all.


Dropping the percentage below 10% trades placement accuracy for scheduling speed, worth it only when raw scheduling throughput matters more than picking the objectively best node for every pod.


Kubernetes 1.35 also introduced Opportunistic Batching, which caches filtering and scoring results across identical pod specs scheduled back to back, cutting scheduling time further for workloads that deploy many identical replicas at once.


Slow pod startup or a growing count of pending pods both point back to this layer.


Check scheduler queue depth and binding latency (covered in the monitoring section) before assuming the fix is a config change here.


‍


### Pod affinity, anti-affinity, and node affinity rules


` percentageOfNodesToScore` controls how many nodes the scheduler scores; affinity rules control which of those nodes actually qualify in the first place:


- **Node affinity** pins a pod to nodes with specific labels, like GPU-equipped or high-memory instances
- **Pod affinity** co-locates pods that benefit from proximity, such as a cache sitting next to the service that reads it most
- **Pod anti-affinity** spreads replicas apart, usually across failure domains, so a single node or zone failure doesn't take out every replica at once
- **Topology spread constraints** generalize anti-affinity across zones, regions, or any other topology label, with finer control over how evenly pods distribute


Complex affinity rules carry a real scheduling cost.


Every additional required rule narrows the set of nodes the scheduler considers feasible, and in a cluster already tight on capacity, an overly strict rule can leave pods pending indefinitely because no node satisfies every constraint simultaneously.


Combine affinity with taints and tolerations only where the isolation is actually necessary, not as a default on every workload.


‍


### Node-level tuning for Kubernetes performance


Some performance gains happen below the pod spec entirely, at the node's OS and kubelet configuration:


- **` sysctl` parameters** tune kernel-level network and memory behavior per node, such as connection tracking table size for high-connection-count workloads
- **IRQ affinity** pins interrupt handling to specific CPU cores, keeping network-heavy workloads from contending with application threads for the same cores
- **CPU Manager's` static` policy** grants exclusive whole CPU cores to Guaranteed QoS pods, instead of the default shared CFS scheduling
- **Huge pages** reduce memory management overhead for workloads with large memory footprints, like in-memory databases
- **Disk I/O tuning** at the node level, covered in more depth in the storage section, affects every pod scheduled to that node, not just one workload


These are node-pool-wide settings, so they affect every pod scheduled there, which makes them worth doing once for a class of workload rather than repeatedly for individual pods.


‍


### Topology-aware scheduling and NUMA optimization


The CPU Manager's` static` policy from the previous section pins exclusive cores to a pod, but pinning cores alone doesn't guarantee those cores sit close to the memory that pod uses.


On multi-socket nodes, memory access speed depends on which NUMA node a CPU core is physically closest to. A pod whose CPU cores and memory get allocated from different NUMA nodes pays a latency penalty on every memory access, one that shows up as inexplicably worse performance despite adequate CPU and memory headroom.


The Topology Manager coordinates CPU, memory, and device allocation to keep them aligned:


Policy Behavior Trade-off


none No topology alignment (default) Simplest, but no NUMA guarantee


best-effort Attempts alignment, schedules the pod even if alignment fails Best NUMA performance available without blocking scheduling


restricted Requires alignment; admits the pod only if achievable Predictable performance, but can leave pods pending


single-numa-node Requires all resources from a single NUMA node Strongest guarantee, most restrictive placement


This only matters for Guaranteed QoS pods requesting whole CPU cores; Burstable and BestEffort pods aren't eligible for topology alignment at all, since their CPU allocation isn't pinned to specific cores in the first place.


HPC workloads, real-time processing, and anything else sensitive to memory access latency are the workloads where this is worth configuring; a typical stateless web service won't notice the difference.


‍


## Autoscaling, cluster efficiency, and capacity planning


Scheduling places a pod once, against the capacity available at that moment. Autoscaling handles what happens afterwards, as traffic rises and falls and that capacity stops matching what the cluster needs.


‍


### Horizontal Pod Autoscaler (HPA) tuning and optimization


The[Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) adjusts replica count based on observed metrics, most commonly CPU utilization, though scaling on custom or external metrics through KEDA covers far more real-world cases, like queue depth or request latency.


The default CPU-only trigger works fine for CPU-bound workloads and badly for anything where CPU isn't the actual constraint, like an I/O-bound service that never spikes CPU no matter how backed up its request queue gets.


Stabilization windows prevent thrashing, replicas scaling up and back down repeatedly in response to noisy, short-lived metric spikes.


A longer stabilization window smooths that out at the cost of slower reaction to a genuine, sustained spike, so the setting is a direct trade-off between responsiveness and stability, tuned to how bursty the actual traffic pattern is.


Pair HPA with PodDisruptionBudgets so that scaling events and voluntary disruptions, like a node drain during a cluster upgrade, don't both try to remove capacity at the same time.


‍


### Cluster Autoscaler configuration and performance


HPA can only add replicas if the cluster has room to schedule them.


[Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) adds and removes nodes to match that demand, controlled by a few key settings:


- **Scan interval** sets how often it checks for pending pods or underutilized nodes
- **Scale-down thresholds** set how underused a node has to be before it's removed
- **Expander strategy** decides which node pool to grow when multiple pools qualify for a scale-up


Provisioning a new node isn't instant.


Cloud API calls, instance boot time, and kubelet registration all add up, and during that window pods sit pending, directly adding to the scheduling latency covered earlier in this guide.


Pre-warming a small buffer of idle capacity, or deliberately overprovisioning by a fixed percentage, absorbs traffic spikes without waiting on a fresh node to boot. Running that buffer on spot-priced node pools cuts the cost of keeping it warm.


‍


### Vertical Pod Autoscaler (VPA) for resource optimization


HPA and Cluster Autoscaler both react to demand; the[Vertical Pod Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) works the other direction, fixing the requests and limits from the resource-configuration section instead of scaling around them.


VPA continuously analyzes actual usage and recommends, or in` Auto` mode automatically applies, updated requests and limits.


Three update modes control how aggressively it acts:


Mode Behavior


Off Generates recommendations only, applies nothing


Initial Sets requests at pod creation, never changes a running pod


Auto Continuously updates requests, which currently requires restarting the pod to apply


That restart requirement is the reason` Auto` mode is risky for stateful or latency-sensitive workloads.


Losing a database connection pool or an in-memory cache mid-update can hurt more than the resource misconfiguration VPA was trying to fix.


` Off` mode, used purely as a recommendation engine feeding the rightsizing loop from the resource-configuration section, is the safer default for anything that can't tolerate an unplanned restart.


Running VPA and HPA on the same metric creates a conflict, since VPA adjusting CPU requests while HPA scales replica count based on CPU utilization means each controller reacts to a moving target the other one is causing.


The standard workaround is scoping VPA to memory only, or to an entirely different metric than whatever HPA scales on:


HPA VPA Cluster Autoscaler


What it scales Replica count Per-container requests/limits Node count


Trigger CPU, memory, or custom/external metrics Historical usage analysis Pending pods or underutilized nodes


Best for Handling variable traffic load Correcting misconfigured requests/limits Matching cluster capacity to total demand


‍


### Capacity planning for sustained cluster performance


Autoscaling reacts to load in real time; capacity planning sets the boundaries it reacts within.


Forecast node pool sizing from growth trends and historical utilization data, and target a utilization band, commonly 60 to 70%, that leaves headroom for a spike without paying for capacity that sits idle at 20% utilization.


Stateful workloads with persistent storage need their own capacity planning pass, separate from stateless compute, since storage can't scale down and back up on the same timescale a stateless pod can.


Mixing on-demand and spot-priced capacity is a common way to plan for this efficiently: keep on-demand or reserved capacity under stateful, disruption-sensitive workloads, and put elastic, interruption-tolerant capacity, batch jobs, CI runners, horizontally-scaled stateless services, on spot pricing.


Terraform or GitOps provisions both pools side by side in the same cluster.


‍


## Monitoring, metrics, and network performance


None of the tuning covered so far works without visibility into whether it actually helped.


‍


### Key performance metrics and monitoring tools


A handful of metrics cover most of what matters:


- **CPU and memory utilization** measured against configured requests and limits, not raw usage alone
- **Pod restart rates** , which surface OOM kills and crash loops before a user reports them
- **Scheduling latency** , the gap between a pod being created and actually bound to a node
- **API server latency** , the clearest signal of control-plane health under load


cAdvisor and kubelet expose the container-level metrics; the API server and etcd expose control-plane metrics.


Tool Type Best fit


Prometheus + Grafana Open-source metrics and dashboards Teams wanting full control and no per-host licensing cost


Metrics Server Lightweight, in-cluster Powering HPA/VPA directly; not a general observability tool


Datadog Commercial SaaS Teams wanting managed setup and cross-stack correlation out of the box


Dynatrace Commercial SaaS, AI-assisted Large enterprises wanting automated root-cause analysis


Jaeger / Tempo Distributed tracing End-to-end latency tracing across microservice call chains


SLO-based alerting, alerting on a defined error budget or latency target rather than an arbitrary CPU percentage, produces far fewer false-positive pages than threshold-based alerting tuned by guesswork.


A pod running at 85% CPU that's meeting its latency SLO doesn't need an alert; a pod at 40% CPU that's missing its SLO does.


‍


### Performance metrics for autoscaling and scheduling decisions


Beyond the general metrics above, a smaller set feeds directly back into the scheduling and autoscaling decisions covered earlier in this guide:


- **Scheduler queue depth and binding latency** , how long a pod sits pending before it's bound to a node
- **Preemption event counts** , which flag priority classes forcing out lower-priority pods more often than expected
- **API server audit logs** , for identifying which clients are generating the most control-plane load
- **etcd write latency and disk sync duration** , since etcd performance degrades control-plane responsiveness across the entire cluster


Dashboards that correlate node-level utilization against application-level latency in the same view make bottleneck diagnosis fast instead of a multi-tab guessing exercise.


‍


### Network performance optimization in Kubernetes


The API server and etcd metrics from the previous section cover the control plane while network performance covers everything moving between pods once traffic actually leaves them.


CNI plugin choice affects baseline network throughput and latency before any application traffic is involved:


CNI plugin Data plane eBPF support Complexity


Cilium eBPF-native Yes, core to the design


Moderate to set up; lowest per-packet overhead of the four once configured


Calico iptables, eBPF, or VPP (configurable) Optional


Moderate


Flannel VXLAN overlay No


Low, minimal feature set


Weave Net Mesh overlay No


Low, community-maintained since Weaveworks shut down in 2024


[Cilium's](https://cilium.io/) eBPF data plane bypasses much of the iptables-based packet processing overhead that Flannel and default-mode Calico both carry, which matters most at high pod density or high connection churn.


Service mesh sidecars (Istio, Linkerd) add their own latency cost on top of whatever the CNI contributes, typically single-digit milliseconds per hop.


Measure that cost directly for a latency-sensitive path instead of assuming it's negligible.


kube-proxy has a similar choice to make between three modes:


- **iptables** remains the default mode
- **IPVS** is now deprecated as of Kubernetes 1.35
- **nftables** , generally available since 1.33, is the recommended replacement for both, though clusters don't migrate to it automatically and it has to be set explicitly in the kube-proxy configuration


At high service or endpoint counts, nftables avoids the linear rule-matching cost that made iptables mode slow down as service count grew in the first place.


‍


## Storage optimization for Kubernetes performance


A database pod with plenty of CPU and memory can still run slowly, because every query it serves has to wait for a read or write to finish on disk.


A stateful workload runs only as fast as its disk lets it, which makes storage the last major lever in this guide.


Storage tuning starts with the StorageClass.


A faster class serves many more reads and writes per second than a standard one, and it returns each individual request in less time, which is what a database notices when a query sits waiting on a single read.


You have to pick the class based on how the workload actually uses the disk, whether that's many small random reads or fewer large sequential writes, rather than accepting whatever the cluster sets by default:


- **Sequential or random access, read-heavy or write-heavy.** Each pattern favors a different volume type
- **Mount options and filesystem choice.** A second tuning layer on top of whatever storage class is selected
- **Local persistent volumes.** Storage attached directly to the node, cutting network latency entirely. They suit latency-sensitive databases and caches, but the data doesn't survive a node replacement


Monitor IOPS, throughput, and latency per volume the same way CPU throttling gets monitored per pod.


A stateful workload that looks CPU- and memory-healthy but is still slow usually has its bottleneck here.


## Diagnosis comes before tuning


These six domains overlap in practice.


Bad CPU limits show up as slow scheduling once the scheduler starts packing around the contention, and a slow autoscaler looks identical to a network bottleneck until the metrics get checked.


Diagnosing the right layer before changing anything is what separates a fix from a guess.


## Frequently asked questions


### What is Kubernetes performance optimization?


Kubernetes performance optimization means finding which constraint is actually limiting a cluster, whether that's CPU throttling, memory pressure, slow scheduling, autoscaling, or network and storage, then fixing that one thing instead of adding resources everywhere.


‍


### How do you identify performance bottlenecks in a Kubernetes cluster?


Check whether the node is saturated or just the pod. If the node has CPU and memory to spare and one pod is still slow, the problem is in the application. Profilers like \`pprof\`, read against Prometheus metrics from the same window, narrow it down from there.


‍


### What causes CPU throttling in Kubernetes, and how do you detect it?


Linux gives each container a fixed budget of CPU time per 100-millisecond window. Spend it early and the container is frozen until the next window, even when the node has CPU free. Compare \`container_cpu_cfs_throttled_periods_total\` against the total windows per pod; above 10% is worth investigating.


‍


### What's the difference between resource requests and limits?


A request reserves capacity so the scheduler knows where the pod fits, and caps nothing at runtime. A limit is the hard stop the kubelet enforces, freezing a container that exceeds its CPU limit and killing one that exceeds its memory limit.


‍


### What are the Kubernetes QoS classes?


Kubernetes assigns each pod a class from how its requests and limits compare. Guaranteed means requests equal limits, and those pods are evicted last. Burstable means requests below limits. BestEffort means neither is set, and those go first. The class decides eviction order when a node runs short.


‍


### What's the difference between HPA, VPA, and the Cluster Autoscaler?


HPA adds and removes replicas as traffic changes. VPA corrects each container's requests and limits from what it actually uses. Cluster Autoscaler adds and removes nodes so both have room to work. Point HPA and VPA at different metrics, or they react to each other.


‍


### Which CNI plugin is best for Kubernetes network performance?


Cilium usually carries the least overhead, processing packets with eBPF in the kernel instead of walking iptables rules the way Flannel and default-mode Calico do. Calico can use eBPF too. Flannel and Weave Net cost more per packet but are simpler to run.


‍


### What are Kubernetes priority classes, and how do they affect performance?


Scheduling is otherwise first-come-first-served, so an important service can wait behind a batch job that claimed the last capacity. A priority class lets the scheduler evict the lower-priority pod to make room.


‍
