---
schema_version: "1.0.0"
document_id: "5e3548e7f01e8807deb8f8a56930e8654ccdae0d82562d5465d573b7d9f0838e"
company_key: "yc-metoro"
company: "Metoro"
source_id: "yc-metoro-news-import-a8c8033caa18"
canonical_url: "https://metoro.io/blog/kubernetes-observability"
published_at: "2026-02-03T00:00:00+00:00"
first_seen_at: "2026-07-22T04:11:54.672278+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:5528dde1995c246114da6499c5ecb15208cd1e225974c21a870336d7a02eb450"
---

# Kubernetes Observability: The Complete Guide

Kubernetes has become the de facto standard for container orchestration, but running applications on k8s introduces observability challenges that don't exist in traditional environments. You're no longer dealing with a single application on a single server. You have containers that can be scheduled anywhere, pods that come and go, nodes that might fail, and a control plane that orchestrates it all.


If you're already comparing vendors rather than building the mental model, jump to[best Kubernetes observability tools](https://metoro.io/blog/best-kubernetes-observability-tools) . If you want a practical production monitoring checklist, read[Kubernetes monitoring](https://metoro.io/blog/kubernetes-monitoring) .


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Workloads
APP[Applications]
end
subgraph Infrastructure
NODES[Nodes]
CP[Control Plane]
end
end
APP -->|Metrics| OBS[Observability Platform]
APP -->|Logs| OBS
APP -->|Traces| OBS
NODES -->|Node Metrics| OBS
CP -->|Events| OBS
```


Kubernetes observability data flow from applications and infrastructure to the observability platform


Kubernetes observability is about understanding what's happening across all of these layers. It builds on the traditional pillars of observability (metrics, logs, traces, and profiling) but extends each one with Kubernetes-specific data sources and adds entirely new pillars like resource state tracking and cluster events.


In this guide, you'll learn:


- The six pillars of Kubernetes observability and how they differ from traditional monitoring
- How to collect metrics, logs, traces, and profiles from applications, containers, nodes, and the control plane
- When to build your own stack vs. use a platform (with a quiz to help you decide)
- What to alert on and how to set up effective monitoring for Kubernetes-specific issues


## What is Kubernetes Observability?


At its core, Kubernetes observability follows the same principles as traditional application observability. You're collecting telemetry data to understand system behavior, debug issues, and ensure reliability. But in a Kubernetes environment, each pillar has additional data sources that are unique to the platform.


**Traditional Pillars (extended for Kubernetes):**


- **Metrics** - Application metrics, plus container metrics, node metrics, and control plane metrics
- **Logs** - Application logs, plus control plane logs and audit logs
- **Traces** - Application traces, plus control plane component traces
- **Profiles** - Application profiling, with eBPF-based profiling for any container


**Kubernetes-Specific Pillars:**


- **Resource State** - Tracking Deployments, ConfigMaps, Secrets, and other Kubernetes resources over time
- **Kubernetes Events** - Pod scheduling, image pulls, container lifecycle, scaling decisions


The six pillars of Kubernetes observability


Let's break down what each pillar looks like in a Kubernetes context.


### Metrics


In traditional environments, you collect application metrics to understand performance and health. In Kubernetes, you need metrics at multiple layers:


**Application Metrics** - The metrics your application exposes about its own behavior: request counts, latencies, error rates, business metrics. This is the same as any other environment.


**Container Metrics** - Every application in Kubernetes runs inside a container, and containers expose a wealth of metrics about resource consumption. CPU usage, memory consumption, network I/O, filesystem reads and writes.


These metrics tell you how your application is actually running, not just what it reports about itself. A container hitting its memory limit behaves very differently than one with headroom.


**Node Metrics** - Containers run on nodes, and node health directly impacts the workloads running on them. CPU pressure, memory pressure, disk pressure, network saturation. When a node struggles, every pod on that node feels it.


**Control Plane Metrics** - The Kubernetes control plane itself (the API server, scheduler, controller manager, etcd) exposes metrics about cluster operations. How long are scheduling decisions taking? Is etcd healthy? Are API requests being throttled? These metrics help you understand cluster-level health.


Kubernetes metrics dashboard with HTTP response metrics and latency percentiles


### Logs


Logging in Kubernetes extends beyond application logs to include the platform itself:


**Application Logs** - Standard stdout/stderr output from your containers. Kubernetes captures these automatically and stores them on the node filesystem. Structured logging (JSON format) makes these much more useful for querying and correlation.


**Control Plane Logs** - The components that run your cluster generate their own logs. The kubelet on each node, the API server, etcd, the scheduler, controller managers, and any operators you've installed. When something goes wrong at the platform level, these logs are where you'll find answers.


**Audit Logs** - The Kubernetes API server can log every request made to the cluster. Who created that deployment? When was that secret modified? Which service account deleted those pods? Audit logs are essential for security, compliance, and debugging permission-related issues.


Aggregated logs view with filtering by service and log level


### Tracing


Distributed tracing helps you understand request flow through your services:


**Application Tracing** - Standard distributed tracing across your microservices. A request enters your system, flows through multiple services, and you need to understand the path it takes, where time is spent, and where failures occur.


**Control Plane Tracing** - Less commonly implemented, but the Kubernetes control plane components support OpenTelemetry tracing. This can help you understand internal cluster operations, particularly useful if you manage your own control plane or need to debug complex scheduling behavior. Note that managed Kubernetes services (EKS, GKE, AKS) typically don't expose full control plane tracing.


Distributed trace waterfall showing request flow across microservices


### Profiling


Profiling tells you *why* something is slow when metrics tell you *that* it's slow:


**Application Profiling** - CPU profiles, memory profiles, goroutine analysis (for Go applications), and similar data for other languages. Continuous profiling lets you understand resource consumption at the code level without waiting for an incident to attach a profiler.


In Kubernetes, profiling becomes particularly valuable because you can profile any container in your cluster, including third-party applications, using eBPF-based profilers that require no code changes.


CPU flame graph showing hot code paths and function call hierarchy


### Kubernetes Metadata


This is where Kubernetes observability diverges most significantly from traditional observability. Kubernetes maintains a rich set of metadata about your workloads that doesn't exist in other environments:


**Resource State** - Deployments, StatefulSets, DaemonSets, ReplicaSets, ConfigMaps, Secrets, Services, Ingresses. The current state of these resources, how they change over time, and how they relate to each other. When a deployment rolls out, you want to correlate that event with changes in your metrics and logs.


**Kubernetes Events** - The cluster generates events for significant occurrences: pods being scheduled, images being pulled, containers starting or being killed, volume mounts succeeding or failing, probes failing, HPA scaling decisions. Events are short-lived by default (often around an hour), so persisting them is critical for incident investigation.


Understanding what changed and when is often the key to debugging issues in Kubernetes. A spike in errors that started exactly when a ConfigMap was updated is a very different problem than one that correlates with increased traffic.


Timeline correlating Kubernetes events with application metrics


## Choosing Your Approach


Before diving into implementation details for each pillar, consider what level of control and coverage you need. There's no single right answer here. The best approach depends on your team's size, expertise, and what you're trying to accomplish. You have 4 main options:


- Specialized Kubernetes Observability Platform
- General Full Observability Platform
- Single Pillar Providers
- DIY - Build Your Own Stack


### Specialized Kubernetes Observability Platform


Platforms built specifically for Kubernetes observability. Examples include[Metoro](https://metoro.io/) ,[Komodor](https://komodor.com/) , and[Robusta](https://home.robusta.dev/) .


**Strengths**


- Deep Kubernetes-native understanding
- Purpose-built for k8s workflows and concepts
- Often better correlation of k8s metadata with telemetry
- Pricing models that understand k8s scale patterns


**Limitations**


- May not cover non-k8s workloads
- Smaller ecosystems


**Best for:** Teams that are 80-100% Kubernetes and want fast time-to-value with deep Kubernetes-native insights.


Metoro dashboard with correlated metrics, traces, and logs


### General Full Observability Platform


General-purpose observability platforms that support Kubernetes among many other environments. Examples include[Datadog](https://www.datadoghq.com/) ,[New Relic](https://newrelic.com/) ,[Dynatrace](https://www.dynatrace.com/) , and[Splunk](https://www.splunk.com/en_us/products/observability.html) .


**Strengths**


- Mature, feature-rich platforms
- Support for hybrid environments (not just k8s)
- Strong ecosystem and integrations
- Well-documented, large communities


**Limitations**


- Kubernetes support can feel bolted-on
- May require significant configuration for k8s-specific insights
- Pricing often not optimized for k8s cardinality
- Generic approach may miss k8s-specific context


**Best for:** Organizations with hybrid environments (Kubernetes + VMs + managed services) needing a single pane of glass.


### Single Pillar Providers


Use a managed service for just one type of telemetry, usually logs or metrics. Examples include[CloudWatch Logs](https://aws.amazon.com/cloudwatch/) ,[Papertrail](https://www.papertrail.com/) , or[Loggly](https://www.loggly.com/) for logs, and managed Prometheus offerings for metrics.


**Strengths**


- Lower complexity than full DIY
- Get value quickly for your most pressing need
- Can mix with DIY for other pillars
- Good stepping stone


**Limitations**


- Still need to solve other pillars separately
- No cross-pillar correlation
- May outgrow single-pillar solution


**Best for:** Teams with one pressing need (usually logs) who want quick wins before expanding.


### DIY - Build Your Own Stack


Collect, store, and query each telemetry type yourself using open source tools. A typical stack might include[Prometheus](https://prometheus.io/) and[Grafana](https://grafana.com/) for metrics,[Loki](https://grafana.com/oss/loki/) for logs, and[Jaeger](https://www.jaegertracing.io/) for traces.


**Strengths**


- Full control over your stack
- Can optimize for specific needs
- Often cheaper at scale if you have the expertise
- No vendor lock-in


**Limitations**


- Significant operational overhead
- Need to integrate multiple tools yourself
- Correlation across pillars is your problem to solve
- Expertise required for each component


**Best for:** Teams with strong platform engineering bandwidth who need full control and cost optimization.


### Find Your Observability Solution


Not sure which approach is right for you? Answer a few questions to get a recommendation.


Find Your Solution


1 of 5


### Is your environment Kubernetes-only?


## How Each Pillar Works


The sections below break down each observability pillar into its two core components: **collection** (how you gather the data) and **storage/querying** (where it goes and how you access it).


**Who should read this:**


- **DIY builders** - You'll find implementation guidance for each pillar, including tool choices and architectural patterns.
- **Provider evaluators** - Understanding how things work under the hood helps you ask the right questions and compare platforms effectively.
- **Platform users** - Even if your provider handles everything, knowing what's happening behind the scenes helps you debug issues and optimize your setup.


If you've chosen a specialized Kubernetes observability platform that handles collection and storage for you, feel free to skip ahead toMonitoring & Alerting .


### Metrics


Metrics are numerical measurements collected over time that tell you how your system is performing. In Kubernetes, you need metrics at multiple layers: your application, the containers it runs in, the nodes those containers run on, and the control plane that orchestrates everything.


#### Collection


In Kubernetes, there are two main paradigms for how applications emit metrics:


1. **Push-based** - Applications actively send metrics to a collector (OpenTelemetry approach)
2. **Pull-based** - A scraper periodically fetches metrics from application endpoints (Prometheus approach)


Both approaches work well in Kubernetes, but pull-based (Prometheus) is more common due to its deep integration with the k8s ecosystem.


##### Application Metrics


Let's look at both approaches in detail.


**OpenTelemetry Push-Based Metrics**


With OTEL, your application instruments metrics using the OpenTelemetry SDK and pushes them to an OTEL Collector. The collector then forwards metrics to your storage backend.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Deployment
A[App Pods]
end
subgraph DaemonSet
B[OTEL Collector]
end
end
A -->|push| B
B -->|export| C[Metrics Backend]
```


OpenTelemetry push-based metrics collection in Kubernetes


This approach is useful when:


- Your applications already use OpenTelemetry for tracing
- You want a unified instrumentation approach across metrics, traces, and logs
- You're running in environments where pull-based scraping is difficult


**Prometheus Pull-Based Metrics**


With Prometheus, your application exposes metrics on an HTTP endpoint (typically` /metrics` ). A Prometheus server periodically scrapes these endpoints to collect metrics.


```text
flowchart RL
subgraph Kubernetes Cluster
subgraph StatefulSet
P[Prometheus]
end
subgraph Deployment
A[App Pods]
end
subgraph Deployment2[Deployment]
K[kube-state-metrics]
end
end
P -->|scrape /metrics| A
P -->|scrape| K
```


Prometheus pull-based metrics scraping in Kubernetes


This is the most common approach in Kubernetes because:


- Prometheus is the de facto standard for k8s metrics
- Many applications and libraries expose Prometheus metrics out of the box
- Service discovery in Kubernetes makes it easy to find scrape targets


For high-scale environments, consider[Thanos](https://thanos.io/) or[Cortex](https://cortexmetrics.io/) on top of Prometheus for long-term storage and multi-cluster queries.


##### Control Plane Metrics


The Kubernetes control plane components expose their own metrics in Prometheus format:


- **kube-apiserver** - API request latencies, request counts, etcd cache hits
- **kube-scheduler** - Scheduling latencies, queue depths, scheduling failures
- **kube-controller-manager** - Work queue depths, reconciliation times
- **etcd** - Disk sync durations, database size, leader changes


These metrics help you understand cluster-level health and catch issues before they affect workloads.


```text
flowchart RL
subgraph Kubernetes Cluster
subgraph Control Plane
API[kube-apiserver]
SCHED[kube-scheduler]
CM[kube-controller-manager]
ETCD[etcd]
end
subgraph Monitoring
P[Prometheus]
end
end
P -->|scrape /metrics| API
P -->|scrape /metrics| SCHED
P -->|scrape /metrics| CM
P -->|scrape /metrics| ETCD
```


Prometheus scraping control plane components for cluster health metrics


##### Container Metrics


Container metrics come from the container runtime and tell you about resource consumption at the container level.[cAdvisor](https://github.com/google/cadvisor) (Container Advisor) is typically integrated with the kubelet and exposes metrics like:


- CPU usage (user, system, throttling)
- Memory usage (working set, RSS, cache)
- Network I/O (bytes sent/received, packets, errors)
- Filesystem I/O (reads, writes, usage)


These metrics are critical because they show you what's actually happening at runtime, not just what your application reports. A container approaching its memory limit will behave differently than one with headroom, even if your application metrics look normal.


Container metrics showing CPU, memory, and network I/O


cAdvisor collects container metrics by reading from Linux cgroups and the` /proc` filesystem. Every container on a node runs inside a cgroup, which the kernel uses to track and limit resource consumption. cAdvisor watches these cgroups and periodically samples CPU cycles, memory pages, network packets, and disk operations. In most Kubernetes distributions, cAdvisor is integrated with the kubelet and automatically discovers all containers on the node without any configuration. Prometheus scrapes the kubelet's` /metrics/cadvisor` endpoint to collect these metrics, which are already labeled with pod name, namespace, and container name for easy correlation.


```text
flowchart RL
subgraph Node
subgraph Pods
C1[Container]
C2[Container]
end
subgraph Kubelet
CA[cAdvisor]
end
end
subgraph Monitoring
P[Prometheus]
end
C1 -.->|resource usage| CA
C2 -.->|resource usage| CA
P -->|scrape /metrics/cadvisor| CA
```


cAdvisor collecting container resource metrics via the kubelet


##### Node Metrics


Node-level metrics come from the node itself and include:


- CPU utilization across all cores
- Memory pressure and availability
- Disk I/O and capacity
- Network bandwidth and errors


The[node-exporter](https://github.com/prometheus/node_exporter) (a Prometheus exporter) is commonly deployed as a DaemonSet to collect these metrics from every node. Node metrics help you understand infrastructure-level constraints that affect all pods on that node.


```text
flowchart RL
subgraph Node
NE[node-exporter]
HW[CPU / Memory / Disk / Network]
end
subgraph Monitoring
P[Prometheus]
end
HW -.->|expose| NE
P -->|scrape /metrics| NE
```


Node-exporter exposing host-level metrics for Prometheus scraping


##### Third-Party Exporters


Many third-party components don't expose Prometheus metrics natively. Exporters bridge this gap by collecting metrics from these systems and exposing them in Prometheus format.


Common exporters include:


- **[redis-exporter](https://github.com/oliver006/redis_exporter)** - Redis server metrics
- **[mysql-exporter](https://github.com/prometheus/mysqld_exporter)** - MySQL database metrics
- **[postgres-exporter](https://github.com/prometheus-community/postgres_exporter)** - PostgreSQL metrics
- **[kafka-exporter](https://github.com/danielqsj/kafka_exporter)** - Kafka broker and consumer metrics
- **[nginx-exporter](https://github.com/nginxinc/nginx-prometheus-exporter)** - NGINX web server metrics


#### Storage and Querying


You have two main choices for metrics storage: in-cluster or out-of-cluster.


**In-Cluster Storage**


Running your metrics storage inside the cluster.


**Strengths**


- Generally cheaper (no external service costs)
- Complete control over infrastructure
- Self-contained, no external dependencies
- Lower latency for queries


**Limitations**


- Cluster issues affect your observability data
- High-scale storage is complex to maintain
- Persistent storage in k8s can be challenging
- Difficult to aggregate across multiple clusters


Common in-cluster options:


- **[Prometheus](https://prometheus.io/)** - The standard choice, good for small to medium scale
- **[VictoriaMetrics](https://victoriametrics.com/)** - More efficient storage, better for high cardinality
- **[Mimir](https://grafana.com/oss/mimir/)** - Horizontally scalable, good for large deployments


**Out-of-Cluster Storage**


Sending metrics to an external service or self-hosted infrastructure outside the cluster.


This approach provides resilience (your metrics survive cluster failures) and makes it easier to aggregate data from multiple clusters. The tradeoff is cost and potential latency.


Options include managed Prometheus services (like[Grafana Cloud](https://grafana.com/products/cloud/) ,[Amazon Managed Prometheus](https://aws.amazon.com/prometheus/) ) or general observability platforms that accept Prometheus remote write.


#### Putting It Together


A typical Kubernetes metrics architecture combines multiple collection methods. Your application pods push OTLP metrics to an[OpenTelemetry](https://opentelemetry.io/) Collector running as a DaemonSet. The collector aggregates and processes these metrics, then exposes them on a Prometheus-compatible endpoint.


Prometheus sits at the center, scraping metrics from multiple sources:


- The OTEL Collector for application metrics
- cAdvisor (via the kubelet) for container resource metrics
- node-exporter for host-level metrics
- [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) for Kubernetes object state
- The control plane components for cluster health


Finally, Prometheus uses remote write to send metrics to your storage backend, whether that's in-cluster (VictoriaMetrics, Mimir) or a managed service.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Workloads
APP[App Pods]
end
subgraph DaemonSets
OTEL[OTEL Collector]
NE[node-exporter]
end
subgraph Kubelet
CA[cAdvisor]
end
subgraph Control Plane
API[kube-apiserver]
end
subgraph Monitoring
KSM[kube-state-metrics]
P[Prometheus]
end
end
APP -->|push OTLP| OTEL
OTEL -->|expose /metrics| P
P -->|scrape| CA
P -->|scrape| NE
P -->|scrape| API
P -->|scrape| KSM
P -->|remote write| Backend[Metrics Backend]
```


Complete Kubernetes metrics architecture with OTEL, Prometheus, and multiple data sources


### Tracing


Distributed tracing helps you understand request flow across services. When a request enters your system and touches multiple microservices, tracing shows you the complete path, where time is spent, and where failures occur.


Trace waterfall view with timing breakdown for each span


#### Collection


##### Application Tracing


[OpenTelemetry](https://opentelemetry.io/) is the modern standard for distributed tracing. If you encounter references to OpenTracing or the legacy Jaeger client libraries, those are deprecated - new projects should use OpenTelemetry.


There are three main approaches to instrumenting your applications:


**Manual Instrumentation**


You add tracing code explicitly using the OpenTelemetry SDK. This gives you the most control over what gets traced and what attributes are attached to spans.


```text
tracer  :=   otel .  Tracer  (  "my-service"  )
ctx ,   span  :=   tracer .  Start  (  ctx ,    "process-order"  )
defer   span .  End  (  )
// your code here


```


Manual instrumentation is essential for capturing business-specific context, but it requires code changes and developer discipline to maintain coverage.


**Auto-instrumentation**


For dynamic languages like Python, Java, and Node.js, OpenTelemetry provides agents that automatically instrument common libraries and frameworks. You get tracing for HTTP clients, database drivers, and message queues without changing your application code.


This approach gives you good baseline coverage quickly. The trade-off is less control over span names and attributes, and it only works for supported libraries.


**eBPF-based Tracing**


eBPF-based tracing instruments applications at the kernel level, requiring no code changes or language-specific agents. This is particularly powerful in Kubernetes because you can trace any container, including third-party applications deployed via Helm charts that you don't control.


```text
flowchart LR
subgraph Node
subgraph Pods
APP[Your App]
THIRD[Third-Party App]
end
subgraph DaemonSet
EBPF[eBPF Agent]
end
end
APP -.->|kernel tracing| EBPF
THIRD -.->|kernel tracing| EBPF
EBPF -->|export| Backend[Trace Backend]
```


eBPF-based tracing capturing kernel-level data from all containers


The eBPF agent runs as a DaemonSet and observes network calls and function invocations at the kernel level. This means you get traces for Redis, PostgreSQL, HTTP, and gRPC calls regardless of what language your application is written in.


eBPF-captured trace with auto-instrumented database calls


##### Control Plane Tracing


The Kubernetes control plane components (API server, scheduler, controller manager) support OpenTelemetry tracing via OTLP. This is less commonly needed than application tracing, but can be valuable if you manage your own control plane or need to debug complex scheduling behavior. Note that managed Kubernetes services often don't expose control plane tracing, so this is primarily relevant for self-managed clusters.


Control plane traces help you understand internal cluster operations: how long scheduling decisions take, what's happening during API request processing, and where bottlenecks exist in cluster operations.


#### Storage and Querying


**In-Cluster Storage**


Running your trace storage inside the cluster.


Common options:


- **[Jaeger](https://www.jaegertracing.io/)** - CNCF graduated project, widely adopted, good UI for trace exploration
- **[Tempo](https://grafana.com/oss/tempo/)** - Grafana's tracing backend, pairs well with Grafana for visualization
- **[Zipkin](https://zipkin.io/)** - One of the original distributed tracing systems, still used but less common for new deployments


**Out-of-Cluster Storage**


Sending traces to an external service provides the same benefits as out-of-cluster metrics storage: resilience to cluster failures and easier multi-cluster aggregation.


Most observability platforms accept OTLP traces, and there are managed Jaeger and Tempo offerings available.


#### Putting It Together


A typical Kubernetes tracing architecture combines SDK-based instrumentation for your own applications with eBPF-based tracing for third-party workloads.


Applications instrumented with OpenTelemetry send traces to an OTEL Collector running as a DaemonSet. The collector batches, processes, and exports traces to your backend. Meanwhile, an eBPF agent captures traces from uninstrumented workloads at the kernel level. If you've enabled control plane tracing, those components also send traces to the collector.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Workloads
APP[Instrumented Apps]
THIRD[Third-Party]
end
subgraph DaemonSets
OTEL[OTEL Collector]
EBPF[eBPF Agent]
end
subgraph Control Plane
API[kube-apiserver]
end
end
APP -->|OTLP| OTEL
THIRD -.->|kernel| EBPF
API -->|OTLP| OTEL
OTEL -->|export| Backend[Trace Backend]
EBPF -->|export| Backend
```


Kubernetes tracing architecture combining SDK instrumentation and eBPF


### Logs


Logging in Kubernetes follows a different model than traditional environments. Instead of writing to files that you configure, containers write to stdout and stderr, and Kubernetes captures these streams to the node filesystem.


#### Collection


##### Application Logs


When a container writes to stdout or stderr, Kubernetes captures that output and stores it on the node at` /var/log/containers/` . This happens automatically - you don't need to configure anything for basic log capture.


**Structured Logging**


If you have control over your applications, emit logs in JSON format. Structured logs are dramatically easier to query, filter, and correlate than plain text. Instead of parsing regex patterns, you can filter on fields like` level` ,` service` ,` request_id` , or any business-specific attribute.


**Node-level Agents (Recommended)**


The standard approach is to run a log collection agent as a DaemonSet. The agent tails log files from` /var/log/containers/` on each node and forwards them to your storage backend.


```text
flowchart LR
subgraph Node
subgraph Pod
C[Container]
end
FS[/var/log/containers/]
subgraph DaemonSet
FB[Fluent Bit]
end
end
C -->|stdout/stderr| FS
FB -->|tail| FS
FB -->|export| Backend[Log Backend]
```


Node-level log collection from container stdout/stderr


Common options:


- **[Fluent Bit](https://fluentbit.io/)** - Lightweight, low resource footprint, good for most use cases
- **[Fluentd](https://www.fluentd.org/)** - More plugins and flexibility, higher resource usage
- **[Vector](https://vector.dev/)** - Modern alternative with good performance


**Sidecar Pattern**


For cases where you need different log handling per pod (different parsing, different destinations), you can run a logging sidecar container alongside your application. This adds resource overhead and complexity, so only use it when node-level collection doesn't meet your needs.


##### Control Plane Logs


The Kubernetes control plane components generate their own logs:


- **kube-apiserver** - API request handling, authentication, authorization
- **kube-scheduler** - Scheduling decisions and failures
- **kube-controller-manager** - Controller reconciliation loops
- **etcd** - Cluster state storage operations
- **kubelet** - Pod lifecycle, container operations on each node


For self-managed clusters, access these via journalctl (for systemd-managed components) or` /var/log/` (for static pods).


For managed Kubernetes (EKS, GKE, AKS), control plane logs often need to be explicitly enabled and are delivered to the cloud provider's logging service. Don't assume they're available by default.


##### Audit Logs


The Kubernetes API server can log every request made to the cluster. Audit logs tell you who did what, when, and to which resources.


Audit policy levels control how much detail is captured:


- **None** - Don't log this event
- **Metadata** - Log request metadata (user, timestamp, resource) but not request/response bodies
- **Request** - Log metadata and request body
- **RequestResponse** - Log everything including response bodies


Audit logs are essential for:


- **Security** - Detecting unauthorized access attempts or suspicious activity
- **Compliance** - Meeting regulatory requirements for access logging
- **Debugging** - Understanding who modified a resource and when


#### Storage and Querying


**In-Cluster Storage**


Common options:


- **[Loki](https://grafana.com/oss/loki/)** - Grafana's log aggregation system, uses label-based indexing rather than full-text indexing, lightweight and cost-effective
- **[Elasticsearch](https://www.elastic.co/elasticsearch/) /[OpenSearch](https://opensearch.org/)** - Full-text search capabilities, more powerful querying but higher resource requirements


**Out-of-Cluster Storage**


Cloud provider logging services (CloudWatch Logs, Google Cloud Logging, Azure Monitor) integrate well with managed Kubernetes offerings. Managed Loki and Elasticsearch services are also available if you want the flexibility without the operational overhead.


#### Putting It Together


A typical Kubernetes logging architecture has a DaemonSet agent on each node collecting application logs from the filesystem. The same agent can collect control plane logs and receive audit logs from the API server. All logs flow to a central backend for storage and querying.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Node
subgraph Pods
APP[App Containers]
end
FS[Node Filesystem]
subgraph DaemonSet
FB[Fluent Bit]
end
end
subgraph Control Plane
API[kube-apiserver]
SCHED[kube-scheduler]
end
end
APP -->|stdout/stderr| FS
FB -->|collect| FS
API -->|audit logs| FB
SCHED -->|logs| FS
FB -->|export| Backend[Log Backend]
```


Kubernetes logging architecture with Fluent Bit and control plane logs


### Profiling


Metrics tell you *what* is slow. Profiling tells you *why* .


When your service latency spikes, metrics show you the symptom. Profiling shows you the exact function consuming CPU cycles, the memory allocation causing garbage collection pressure, or the goroutine blocking on a lock.


Modern continuous profiling has low overhead (typically 1-5%), making it safe to run in production. This means you can capture profiles before an incident occurs, not scramble to attach a profiler while the issue is happening.


#### Collection


##### Application Profiling


**pprof Endpoints**


Go applications expose profiling endpoints at` /debug/pprof/` by default. You can capture:


- **CPU profiles** - Where CPU time is spent
- **Heap profiles** - Memory allocation patterns
- **Goroutine profiles** - What goroutines are doing
- **Block profiles** - Where goroutines block on synchronization


Other languages have equivalent tools:


- **Python** -[py-spy](https://github.com/benfred/py-spy) for sampling profiler
- **Java** -[async-profiler](https://github.com/async-profiler/async-profiler) for low-overhead profiling
- **Node.js** - Built-in profiler or[clinic.js](https://clinicjs.org/)


**Continuous Profiling Agents**


Rather than manually capturing profiles, continuous profiling agents periodically collect profiles and send them to a backend for storage and analysis.


Common agents:


- **[Pyroscope](https://grafana.com/oss/pyroscope/) agent** - Supports multiple languages, integrates with Pyroscope server
- **[Parca](https://www.parca.dev/) agent** - eBPF-based, very low overhead


##### eBPF-based Profiling


eBPF-based profilers work at the kernel level, capturing CPU samples from any process without requiring application changes. This is particularly valuable in Kubernetes because you can profile any container - including third-party applications, databases, and message queues that you deploy via Helm charts but don't have source code access to.


```text
flowchart LR
subgraph Node
subgraph Pods
APP[Your App]
THIRD[Third-Party App]
end
subgraph DaemonSet
EBPF[eBPF Profiler]
end
end
APP -.->|kernel sampling| EBPF
THIRD -.->|kernel sampling| EBPF
EBPF -->|export| Backend[Profile Backend]
```


eBPF-based profiling capturing CPU samples from all containers


The trade-off compared to language-specific profilers is less detail. eBPF profilers typically capture CPU profiles but may not have visibility into language-specific constructs like goroutines or garbage collection. For applications you control, language-native profiling gives richer data. For everything else, eBPF profiling gives you visibility you wouldn't otherwise have.


Flame graph visualization for identifying performance bottlenecks


#### Storage and Querying


**In-Cluster Storage**


- **[Pyroscope](https://grafana.com/oss/pyroscope/)** - Open source continuous profiling platform with good visualization
- **[Parca](https://www.parca.dev/)** - Cloud-native profiling, pairs well with Parca agent


**Out-of-Cluster Storage**


Several observability platforms now offer continuous profiling as part of their product. Cloud provider offerings and managed Pyroscope instances are also available.


#### Putting It Together


A typical Kubernetes profiling setup combines language-native profiling for applications you control with eBPF-based profiling for third-party workloads.


Applications with pprof endpoints or language agents send profiles to a continuous profiling agent. The eBPF profiler running as a DaemonSet captures CPU profiles from all containers on each node. Both streams flow to a profile backend for storage, analysis, and flame graph visualization.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Workloads
GO[Go Apps]
OTHER[Other Apps]
THIRD[Third-Party]
end
subgraph DaemonSets
AGENT[Profiling Agent]
EBPF[eBPF Profiler]
end
end
GO -->|pprof| AGENT
OTHER -->|language agent| AGENT
THIRD -.->|kernel| EBPF
AGENT -->|export| Backend[Profile Backend]
EBPF -->|export| Backend
```


Kubernetes profiling setup with language agents and eBPF


### Kubernetes Metadata


This pillar is unique to Kubernetes. Traditional observability doesn't have an equivalent because traditional environments don't have the same concept of declarative resource state managed by an orchestrator.


In Kubernetes, understanding *what changed* is often the key to debugging issues. A spike in errors that started exactly when a ConfigMap was updated is a very different problem than one that correlates with increased traffic.


#### Resource State


Kubernetes maintains state for many resource types: Deployments, StatefulSets, DaemonSets, ReplicaSets, ConfigMaps, Secrets, Services, Ingresses, and more. Tracking how these resources change over time lets you correlate application issues with platform changes.


##### kube-state-metrics


[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) is a service that watches the Kubernetes API and exposes cluster state as Prometheus metrics. It gives you metrics like:


- Deployment replica counts (desired vs available)
- Pod phases (Pending, Running, Failed)
- Container states (waiting, running, terminated)
- Resource requests and limits
- Node conditions


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph API
APIS[kube-apiserver]
end
subgraph Deployment
KSM[kube-state-metrics]
end
subgraph Monitoring
P[Prometheus]
end
end
APIS -->|watch resources| KSM
KSM -->|expose /metrics| P
```


kube-state-metrics watching the Kubernetes API for resource state


This data is essential for understanding cluster state over time. When you see a latency spike at 2:34 PM, you can check if a deployment rolled out, if pod counts changed, or if resource limits were modified around that time.


#### Kubernetes Events


Kubernetes generates events for significant occurrences in the cluster:


- **Scheduling** - Pod assigned to node, or scheduling failed
- **Image pulls** - Successfully pulled image, or pull failed
- **Container lifecycle** - Container started, killed, OOMKilled
- **Scaling** - HPA scaled deployment up or down
- **Volume mounting** - Volume attached successfully or failed
- **Probes** - Liveness or readiness probe failed


Events are incredibly valuable for debugging, but there's a critical gotcha: **Kubernetes only retains events for a short period by default** (often around an hour, though this varies by cluster configuration). If you're investigating an incident that happened yesterday, the events are gone.


To preserve events, deploy an event exporter that watches for events and forwards them to your storage backend.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph API
APIS[kube-apiserver]
end
subgraph Deployment
EXP[Event Exporter]
end
end
APIS -->|watch events| EXP
EXP -->|export| Backend[Event Storage]
```


Event exporter preserving Kubernetes events beyond default retention


Tools like[kubernetes-event-exporter](https://github.com/resmoio/kubernetes-event-exporter) can send events to Elasticsearch, Loki, or webhook endpoints.


#### Change Tracking


Beyond raw events, you want to track higher-level changes: when deployments rolled out, what changed in the configuration, and who made the change.


If you're using GitOps tools like[ArgoCD](https://argo-cd.readthedocs.io/) or[Flux](https://fluxcd.io/) , you get audit trails automatically - every change is a git commit with a timestamp and author. For imperative changes (kubectl apply), consider recording deployment timestamps in your observability platform so you can overlay them on your dashboards.


The goal is to answer the question: "What changed around the time this incident started?" When you can quickly correlate a latency spike with a ConfigMap update or a new deployment, you've found your debugging starting point.


Event correlation showing deployment changes and latency impact


## Monitoring & Alerting


Collecting observability data is only useful if you act on it. Monitoring and alerting turn your telemetry into actionable insights and wake you up when things go wrong.


### What to Alert On


#### The Four Golden Signals


Google's SRE book introduced the four golden signals as the essential metrics for monitoring any system:


- **Latency** - How long requests take, particularly distinguishing between successful and failed requests
- **Traffic** - How much demand is hitting your system (requests per second, transactions, etc.)
- **Errors** - The rate of failed requests, whether explicit (5xx errors) or implicit (wrong content, slow responses)
- **Saturation** - How full your resources are (CPU, memory, disk, network)


These signals apply to any service, but in Kubernetes you have additional platform-specific concerns.


#### Kubernetes-Specific Alerts


These alerts don't exist in traditional environments, some examples would be:


- **Pod restarts / CrashLoopBackOff** - Containers repeatedly crashing indicate application bugs or misconfiguration
- **OOMKilled containers** - Memory limits being hit, need to increase limits or fix memory leaks
- **Node NotReady** - Nodes failing health checks, potential infrastructure issues
- **Node pressure conditions** - Memory, disk, or PID pressure on nodes affecting all pods
- **Pending pods** - Pods that can't be scheduled due to resource constraints or affinity rules
- **Failed deployments** - Rollouts stuck or rolled back
- **PVC binding failures** - Persistent volume claims that can't find matching volumes
- **Certificate expiration** - If using[cert-manager](https://cert-manager.io/) , alert before certificates expire
- **Control plane health** - API server latency, etcd health, scheduler queue depth


#### Application-Level Alerts


Beyond the golden signals, consider alerts specific to your application's behavior:


- **Error rate thresholds** - Alert when 5xx errors exceed a percentage (e.g., >1% of requests)
- **Latency percentiles** - Alert on p99 latency rather than averages to catch tail latency issues
- **Queue depth / consumer lag** - Message queues backing up indicate processing problems
- **Database connection pool exhaustion** - Running out of connections causes cascading failures
- **Business metrics** - Orders per minute dropping, signup conversion rate changing


### Alerting Tools


#### DIY Stack


If you're running your own observability infrastructure:


- **[Prometheus Alertmanager](https://github.com/prometheus/alertmanager)** - Receives alerts from Prometheus, handles deduplication, grouping, routing, and silencing
- **[Grafana Alerting](https://grafana.com/docs/grafana/latest/alerting/)** - Unified alerting across multiple data sources, can alert on Prometheus, Loki, and other backends


Both support routing alerts to different channels based on severity and labels.


```text
flowchart LR
subgraph Kubernetes Cluster
subgraph Monitoring
P[Prometheus]
AM[Alertmanager]
end
end
P -->|alert rules| AM
AM -->|critical| PD[PagerDuty]
AM -->|warning| Slack[Slack]
AM -->|all| Email[Email]
```


Alertmanager routing alerts to different channels by severity


#### Integration with Incident Management


Alerts need to reach the right people through the right channels:


- **Critical alerts** →[PagerDuty](https://www.pagerduty.com/) ,[OpsGenie](https://www.atlassian.com/software/opsgenie) , or similar for on-call rotation and escalation
- **Warning alerts** → Slack or Teams channels for awareness without paging
- **Runbook links** → Include links to documentation in alert annotations so responders know what to do


```text
flowchart LR
subgraph Alerting
AM[Alertmanager]
end
subgraph Incident Management
PD[PagerDuty/OpsGenie]
SLACK[Slack/Teams]
end
subgraph Context
RB[Runbooks]
DASH[Dashboards]
end
AM -->|critical| PD
AM -->|warning| SLACK
PD -.->|links to| RB
PD -.->|links to| DASH
```


Incident management integration with runbooks and dashboards


The goal is to reduce mean time to resolution (MTTR) by giving responders immediate context.


#### Platform Alerting


Most observability platforms include built-in alerting with Kubernetes-specific templates. This can accelerate your setup since you get pre-configured alerts for common issues like CrashLoopBackOff, OOMKilled, and node pressure without writing rules from scratch.


## Conclusion


Kubernetes observability isn't a fundamentally different discipline - it's traditional observability adapted for a container orchestration environment. You're still collecting metrics, logs, traces, and profiles. The difference is where that data comes from and the additional context Kubernetes provides.


The platform adds two pillars that don't exist elsewhere: resource state (tracking Deployments, ConfigMaps, and other Kubernetes objects over time) and Kubernetes events (scheduling decisions, container lifecycle, scaling actions). These pillars are often the key to understanding *why* something went wrong, not just *that* it went wrong.


Collection looks different too. DaemonSets replace traditional agents. eBPF gives you visibility into any container without code changes. kube-state-metrics exposes cluster state as Prometheus metrics. The kubelet's cAdvisor endpoint provides container resource consumption. These are Kubernetes-specific mechanisms, but they're serving the same goal: getting the telemetry you need to understand system behavior.


Whether you build your own stack or use a platform, the principles remain the same. Collect the right data, store it somewhere you can query it, alert on what matters, and make sure you can correlate across pillars when debugging. Kubernetes just gives you more data sources to work with - and more context to help you find answers faster.
