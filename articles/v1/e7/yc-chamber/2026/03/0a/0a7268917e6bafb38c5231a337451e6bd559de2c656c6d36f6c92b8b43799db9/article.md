---
schema_version: "1.0.0"
document_id: "0a7268917e6bafb38c5231a337451e6bd559de2c656c6d36f6c92b8b43799db9"
company_key: "yc-chamber"
company: "Chamber"
source_id: "yc-chamber-news-import-3bd200813650"
canonical_url: "https://usechamber.io/blog/how-to-monitor-gpu-utilization-kubernetes"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-24T23:55:17.000424+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:bef2ea4ce6669467d6f23b81a01049283a1682dd40d2a5b381f8f3798186e5a2"
---

# How to Monitor GPU Utilization in Kubernetes

# How to Monitor GPU Utilization in Kubernetes with DCGM, Prometheus, and Grafana


You can't optimize what you don't measure. Most GPU clusters run blind: teams know their GPUs are expensive, but they don't know which ones are idle right now, which jobs are memory-bound, or which nodes are approaching thermal limits.


The default Kubernetes tooling does not help.` kubectl top` shows CPU and memory. It says nothing about GPU utilization, tensor core activity, or XID errors. Getting real GPU telemetry requires a dedicated monitoring stack, and the[Kubernetes GPU scheduling documentation](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) confirms that GPU metrics are outside the scope of default resource reporting.


We have managed GPU infrastructure at Amazon and built monitoring systems for large scale GPU clusters and learned from hundreds of AIML teams across industries and use cases. Most attempt to use an open-source stack as a starting point: DCGM Exporter, Prometheus, and Grafana. This guide walks you through deploying that stack from zero to a working dashboard in under an hour. However, often these solutions require extensive engineering hours to maintain, customize, and extend in the long run, prompting most to begin searching for alternative solutions. In this guide you'll also learn more about how Chamber provides GPU monitoring for Kubernetes out of the box with zero setup required.


## What You Need Before Starting


Before deploying GPU monitoring, verify your cluster meets these prerequisites.


Prerequisite Minimum Version Notes


Kubernetes cluster 1.21+ Must have NVIDIA GPU nodes ([Kubernetes GPU Scheduling Docs](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) )


NVIDIA drivers 450.80+ Installed on all GPU nodes ([NVIDIA GPU Operator Installation Guide](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html) )


Helm 3.x For chart-based installation ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) )


kubectl access cluster-admin Required for DaemonSet and ServiceMonitor creation


For new clusters, the NVIDIA GPU Operator is the simplest path. It installs drivers, the device plugin, and DCGM Exporter together in a single Helm release ([NVIDIA GPU Operator Installation Guide](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html) ). For existing clusters with drivers already configured, you can install DCGM Exporter standalone.


## GPU Monitoring Architecture in Kubernetes


The monitoring stack has four components, each handling one responsibility in the data pipeline.


**DCGM Exporter** runs as a DaemonSet on every GPU node. It connects to the NVIDIA Data Center GPU Manager (DCGM) library to collect GPU telemetry: utilization, memory, temperature, power, errors. It exposes these metrics on a` /metrics` endpoint in Prometheus format. DCGM Exporter also connects to the kubelet pod-resources socket to map GPU metrics to pod and namespace labels ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ). Without this mapping, you get node-level metrics but cannot attribute GPU usage to specific workloads.


**Prometheus** scrapes the` /metrics` endpoint from every DCGM Exporter pod on a configurable interval (default: 15 seconds). It stores the time-series data and provides the query engine.


**Grafana** connects to Prometheus as a data source and visualizes GPU metrics in dashboards. Panels show utilization trends, memory pressure, thermal status, and error counts.


**Alertmanager** (optional but recommended) evaluates Prometheus alerting rules and routes notifications to Slack, PagerDuty, or email when GPU health degrades.


Why DCGM over nvidia-smi? nvidia-smi is a point-in-time CLI snapshot. DCGM provides continuous streaming telemetry with profiling-grade metrics that nvidia-smi cannot access, including Tensor Core utilization (` DCGM_FI_PROF_PIPE_TENSOR_ACTIVE` ) and NVLink traffic ([NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) ). For production Kubernetes monitoring, there is no substitute.


## How to Install DCGM Exporter with Helm


Two installation paths, depending on your cluster state.


### Option A: NVIDIA GPU Operator (Recommended for New Clusters)


The GPU Operator bundles drivers, device plugin, container runtime, and DCGM Exporter into a single managed deployment ([NVIDIA GPU Operator Installation Guide](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html) ).


```text
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
helm repo update


helm install gpu-operator nvidia/gpu-operator \
--namespace gpu-operator \
--create-namespace \
--set dcgmExporter.enabled=true \
--set dcgmExporter.serviceMonitor.enabled=true


```


The` serviceMonitor.enabled=true` flag creates a Prometheus ServiceMonitor automatically, which saves a manual configuration step.


### Option B: Standalone DCGM Exporter


For clusters that already have NVIDIA drivers and the device plugin installed ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ).


```text
helm repo add gpu-helm-charts https://nvidia.github.io/dcgm-exporter/helm-charts
helm repo update


helm install dcgm-exporter gpu-helm-charts/dcgm-exporter \
--namespace monitoring \
--create-namespace \
--set serviceMonitor.enabled=true


```


To customize which metrics DCGM Exporter collects, create a ConfigMap with your metrics file. The default configuration exposes 20+ metrics. For most clusters, the defaults are sufficient. If you need profiling metrics (Tensor Core utilization, NVLink bandwidth), enable them in the metrics ConfigMap ([NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) ).


### Verify the Installation


Confirm DCGM Exporter pods are running on every GPU node:


```text
kubectl get pods -n monitoring -l app.kubernetes.io/name=dcgm-exporter


# Expected: one pod per GPU node, all Running


```


Test that metrics are flowing by port-forwarding to any DCGM Exporter pod:


```text
kubectl port-forward -n monitoring <dcgm-exporter-pod> 9400:9400
curl localhost:9400/metrics | grep DCGM_FI_DEV_GPU_UTIL


```


You should see lines like` DCGM_FI_DEV_GPU_UTIL{gpu="0",...} 45.0` . If no metrics appear, check that NVIDIA drivers are loaded on the node and that the DCGM Exporter has permission to access the GPU device files.


## How to Connect Prometheus to DCGM Exporter


If you enabled` serviceMonitor.enabled=true` during installation and your cluster runs the kube-prometheus-stack (or any Prometheus Operator), scraping is automatic. The ServiceMonitor resource tells Prometheus where to find DCGM Exporter endpoints.


For clusters using standalone Prometheus without the Operator, add a scrape config manually:


```text
scrape_configs:
- job_name: 'dcgm-exporter'
kubernetes_sd_configs:
- role: endpoints
namespaces:
names:
- monitoring
relabel_configs:
- source_labels: [__meta_kubernetes_service_name]
regex: dcgm-exporter
action: keep


```


Add this block to your` prometheus.yml` and reload Prometheus.


Verify that Prometheus is scraping by navigating to the Prometheus UI targets page (` /targets` ). The DCGM Exporter endpoints should show as` UP` . You can also run a test query in the Prometheus expression browser:


```text
DCGM_FI_DEV_GPU_UTIL


```


This should return utilization values for every GPU in the cluster.


## How to Build a GPU Monitoring Dashboard in Grafana


The fastest path to a working dashboard is importing the official NVIDIA DCGM Exporter dashboard.


In Grafana, navigate to Dashboards > Import and enter dashboard ID **12239** ([Grafana DCGM Exporter Dashboard](https://grafana.com/grafana/dashboards/12239-nvidia-dcgm-exporter-dashboard/) ). Select your Prometheus data source and click Import. You immediately get panels for GPU utilization, memory usage, temperature, and power draw per GPU.


The default dashboard gives you node-level visibility. For production clusters, add these custom panels:


**Per-namespace GPU utilization.** Group by the` namespace` label to see which teams consume the most GPU capacity. This query shows average utilization by namespace:


```text
avg by (namespace) (DCGM_FI_DEV_GPU_UTIL)


```


**Fleet-wide utilization summary.** A single-stat panel showing the average utilization across all GPUs in the cluster. This is the number your infrastructure leadership cares about:


```text
avg(DCGM_FI_DEV_GPU_UTIL)


```


**XID error timeline.** A time-series panel tracking` DCGM_FI_DEV_XID_ERRORS` to identify nodes with recurring hardware faults. Any non-zero value warrants investigation.


A healthy dashboard shows GPU utilization between 70-95% for training workloads, temperatures below 85°C, and zero XID errors. If your fleet-wide average sits below 50%, you are leaving significant capacity on the table. For strategies to improve utilization after you have visibility, see our[GPU utilization optimization guide](https://www.usechamber.io/blog/gpu-utilization-optimization-complete-guide) .


## Key GPU Metrics and What They Mean


DCGM exposes dozens of metrics. These six are the ones that matter for day-to-day operations ([NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) ).


Metric DCGM Field What It Measures Healthy Range Action If Outside Range


GPU utilization` DCGM_FI_DEV_GPU_UTIL` % of time SMs are active 70-95% for training Investigate scheduling and workload placement


Tensor Core utilization` DCGM_FI_PROF_PIPE_TENSOR_ACTIVE` % of time Tensor Cores are active 60-85% for mixed precision Verify mixed precision is enabled in training code


Memory utilization` DCGM_FI_DEV_FB_USED` GPU memory (VRAM) in use Varies by model size Right-size GPU allocation or enable MIG partitioning


Temperature` DCGM_FI_DEV_GPU_TEMP` GPU core temperature in °C Below 85°C Check cooling, airflow, and rack density


Power draw` DCGM_FI_DEV_POWER_USAGE` Watts consumed Below TDP rating Monitor for thermal throttling


XID errors` DCGM_FI_DEV_XID_ERRORS` Most recent NVIDIA error code 0 (no errors) Drain node, investigate fault ([NVIDIA XID Errors Documentation](https://docs.nvidia.com/deploy/xid-errors/index.html) )


One distinction worth understanding: SM utilization and actual compute throughput are not the same thing. SM utilization (` DCGM_FI_DEV_GPU_UTIL` ) tells you the GPU cores are active, but not what they are doing. A GPU running unoptimized CUDA kernels can show 90% SM utilization while its Tensor Cores sit idle. Track both SM utilization and Tensor Core utilization together to understand whether the GPU is busy *and* productive.


If GPU utilization is consistently below 35%, the workload is likely CPU-bound and could run on a less expensive GPU type. If GPU memory utilization stays below 50%, the workload is overprovisioned and should be tested on a smaller GPU. These are low-hanging-fruit cost savings that monitoring reveals immediately.


## How to Set Up GPU Alerts


Monitoring dashboards are useful when someone is watching. Alerts catch problems when nobody is.


Define Prometheus alerting rules for these critical conditions:


```text
groups:
- name: gpu-health
rules:
- alert: GPUUtilizationLow
expr: avg_over_time(DCGM_FI_DEV_GPU_UTIL[30m]) < 20
for: 30m
labels:
severity: warning
annotations:
summary: "GPU {{ $labels.gpu }} on {{ $labels.instance }} below 20% utilization for 30+ minutes"
description: "Wasted capacity. Investigate whether the workload has stalled or the GPU should be released."


- alert: GPUTemperatureHigh
expr: DCGM_FI_DEV_GPU_TEMP > 85
for: 5m
labels:
severity: critical
annotations:
summary: "GPU {{ $labels.gpu }} on {{ $labels.instance }} above 85°C"
description: "Thermal throttling risk. Check cooling and consider draining the node."


- alert: GPUXIDError
expr: DCGM_FI_DEV_XID_ERRORS > 0
for: 1m
labels:
severity: critical
annotations:
summary: "XID error {{ $value }} on GPU {{ $labels.gpu }}, node {{ $labels.instance }}"
description: "Hardware fault detected. Drain node and investigate. See NVIDIA XID error documentation."


- alert: GPUMemoryNearFull
expr: DCGM_FI_DEV_FB_USED / (DCGM_FI_DEV_FB_USED + DCGM_FI_DEV_FB_FREE) * 100 > 95
for: 5m
labels:
severity: warning
annotations:
summary: "GPU {{ $labels.gpu }} memory above 95% on {{ $labels.instance }}"
description: "OOM risk. Consider reducing batch size or enabling gradient checkpointing."


```


Route these alerts to your team's Slack channel, PagerDuty rotation, or email via Alertmanager. GPU temperature and XID errors should page immediately. Low utilization alerts are better sent to a monitoring channel for triage during business hours.


For deeper coverage of XID error handling and automated node remediation, see our guide to[GPU fault detection in Kubernetes](https://www.usechamber.io/blog/gpu-fault-detection-kubernetes) .


## GPU Monitoring Tools Compared


DCGM + Prometheus + Grafana is the production standard for Kubernetes GPU monitoring, but it is not the only option. Here is how the main tools compare.


Tool Type GPU Metrics K8s Labels Historical Data Alerting Setup Complexity


nvidia-smi CLI Basic (utilization, memory, temp) No No No Trivial


gpustat CLI Basic + per-process No No No Trivial


nvtop / nvitop TUI Detailed, interactive No No No Easy


DCGM + Prometheus + Grafana Stack Comprehensive + profiling metrics Full (pod, namespace) Yes Yes Moderate


Datadog GPU Monitoring SaaS Comprehensive Full (pod, namespace) Yes Yes Easy (paid)


For production Kubernetes clusters, DCGM + Prometheus + Grafana is the standard ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ). It gives you full Kubernetes-aware metrics, historical data for capacity planning, and alerting for operational health.


For development machines and quick debugging,[gpustat or nvitop](https://lambdalabs.com/blog/keeping-an-eye-on-your-gpus-2) is sufficient. These tools install in seconds and give you real-time GPU status without any infrastructure setup.


For teams that want the depth of DCGM metrics without maintaining a Prometheus stack, a managed GPU monitoring platform eliminates the operational overhead. For a broader comparison of monitoring approaches, see our[GPU monitoring tools comparison](https://www.usechamber.io/blog/gpu-monitoring-tools-compared) .


## Frequently Asked Questions


### What is the difference between nvidia-smi and DCGM?


nvidia-smi is a command-line utility that provides point-in-time GPU snapshots. DCGM (Data Center GPU Manager) provides continuous monitoring with profiling-grade metrics and native Kubernetes integration. DCGM exposes metrics like Tensor Core utilization and NVLink traffic that nvidia-smi cannot access ([NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) ).


### Does DCGM Exporter work with AMD GPUs?


No. DCGM is an NVIDIA-only tool built on top of NVIDIA's management libraries ([NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) ). AMD GPUs require ROCm-based monitoring solutions.


### How much overhead does GPU monitoring add?


DCGM Exporter adds less than 1% GPU utilization impact ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ). The exporter reads telemetry from DCGM's shared memory interface without interfering with GPU workloads.


### Can I monitor GPU utilization per pod in Kubernetes?


Yes. DCGM Exporter maps GPU metrics to pod and namespace labels by connecting to the kubelet pod-resources API ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ). This gives you per-workload attribution for utilization, memory, temperature, and error metrics.


### What are XID errors and should I worry about them?


XID errors are NVIDIA driver-reported error codes indicating hardware or software faults. Some are informational (XID 13: graphics engine exception, often software-related). Others are critical: XID 79 (GPU has fallen off the bus) means the GPU is no longer communicating with the system and requires immediate action ([NVIDIA XID Errors Documentation](https://docs.nvidia.com/deploy/xid-errors/index.html) ).


### What actions can I take based on GPU monitoring metrics?


Monitoring reveals cost-saving opportunities directly. If GPU utilization is consistently below 35%, the workload is likely CPU-bound and should be tested on a less expensive GPU type. If GPU memory utilization stays below 50%, the workload is overprovisioned and should run on a smaller GPU. If utilization is high but Tensor Core activity is low, enabling mixed precision training can improve throughput without additional hardware.


## How Chamber simplifies GPU monitoring


Now that we understand the open-source way to monitor GPUs, let's explore how Chamber simplifies this for AI/ML teams. Configuring, scaling and maintaining open-source metrics and dashboards can eat up valuable engineering resources. That's why many teams opt for managed services like Chamber that can provide the full GPU observability stack out-of-the-box without any manual setup.


At Chamber our mission is to make monitoring GPU usage, attributing cost, wasted resources and more by cluster, GPU type, team, user and workload seamless. Chamber not only provides the monitoring to understand how to get the most ROI out of your GPUs, but also the intelligent scheduling and GPU orchestration layer for all of your Kubernetes clusters, across all clouds and on-prem.


When you deploy the Chamber agent and have the Nvidia DCGM exporter enabled, Chamber automatically discovers your GPU resources, workloads and produces key utilization metrics. With no additional effort or customization, you immediately gain visibility across all of your clusters from a single view, with drill down capaibilities into cluster, team, user and workloads.


Teams tell us that they lack a single unified view of their job history that they can slice and dice across any dimension to quickly find their historical and active workkloads. Moreever, they also tell us that creating custom dashboarsd for each team, and executive level usage dashboards is a heavy timeconsuming process. With Chamber you don't need to spend time setting up custom dashboards, and instead get insights instantly without any manual effort.


*See how Chamber's monitoring and debugging platform works.*


Interested to learn more about how to get started with Chamber?


[Book a time to talk](https://www.usechamber.io/get-access)


For additional information on how the Chamber Agent works and which metrics are collected, please see[Chamber's agent documentation](https://docs.usechamber.io/agent/overview) .


## Key Takeaways


- The standard GPU monitoring stack for Kubernetes is DCGM Exporter + Prometheus + Grafana. You can deploy it in under an hour ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ).
- Default Kubernetes tooling does not expose GPU metrics.` kubectl top` shows CPU and memory only ([Kubernetes GPU Scheduling Docs](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) ).
- Track both SM utilization and Tensor Core utilization to distinguish "busy" from "productive" ([NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) ).
- Set alerts for low utilization (wasted capacity), high temperature (throttling risk), XID errors (hardware faults), and near-full GPU memory (OOM risk).
- DCGM Exporter maps GPU metrics to Kubernetes pod and namespace labels, enabling per-workload cost attribution ([NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) ).
- nvtop and gpustat are better choices for dev machines; DCGM + Prometheus + Grafana is the standard for production clusters ([Lambda.ai GPU Guide](https://lambdalabs.com/blog/keeping-an-eye-on-your-gpus-2) ).
- Monitoring is step one. The data tells you where to optimize scheduling, right-size allocations, and reduce GPU spend. This full lifecycle is what tools like Chamber are on a mission to solve.


## The Bottom Line


GPU monitoring is the prerequisite for every other optimization. Without per-workload utilization data, scheduling improvements are guesswork, capacity planning is speculation, and cost attribution is impossible.


The DCGM + Prometheus + Grafana stack covered in this guide gives you production-grade GPU observability with full Kubernetes awareness. The investment is an hour of setup time and moderate operational overhead to maintain Prometheus and Grafana.


*Chamber deploys in minutes via Helm and gives you immediate cross-cluster, cross-cloud visibility into GPU utilization, health, and cost without maintaining a Prometheus stack. Start with monitoring to understand your GPU landscape, usage, and cost before optimizing scheduling.*


For what to do with the monitoring data once you have it, see our[GPU utilization optimization guide](https://www.usechamber.io/blog/gpu-utilization-optimization-complete-guide) .


## Sources


- [NVIDIA DCGM Documentation](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/feature-overview.html) . "DCGM Feature Overview." 2024.
- [NVIDIA GPU Operator Installation Guide](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html) . "Getting Started with the GPU Operator." 2024.
- [NVIDIA XID Errors Documentation](https://docs.nvidia.com/deploy/xid-errors/index.html) . "XID Errors." 2024.
- [Grafana DCGM Exporter Dashboard](https://grafana.com/grafana/dashboards/12239-nvidia-dcgm-exporter-dashboard/) . "NVIDIA DCGM Exporter Dashboard." Dashboard ID 12239.
- [NVIDIA dcgm-exporter GitHub](https://github.com/NVIDIA/dcgm-exporter) . Helm chart and Prometheus integration reference.
- [Kubernetes GPU Scheduling Documentation](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) . "Schedule GPUs." 2024.
- [Lambda Labs](https://lambdalabs.com/blog/keeping-an-eye-on-your-gpus-2) . "Keeping an Eye on Your GPUs." GPU monitoring tool comparison.
