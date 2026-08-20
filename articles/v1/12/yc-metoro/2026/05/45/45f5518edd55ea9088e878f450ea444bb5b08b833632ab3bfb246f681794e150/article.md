---
schema_version: "1.0.0"
document_id: "45f5518edd55ea9088e878f450ea444bb5b08b833632ab3bfb246f681794e150"
company_key: "yc-metoro"
company: "Metoro"
source_id: "yc-metoro-news-import-a8c8033caa18"
canonical_url: "https://metoro.io/blog/kubernetes-monitoring-best-practices"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-24T04:14:18.908490+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:e10f0b1a1e3732b2f6531f70e04961f208f1ef016e5aa12b5cce0b472c2e38ad"
---

# Kubernetes Monitoring Best Practices: A Production SRE Checklist

Kubernetes monitoring best practices are not about collecting every metric Kubernetes can emit. They are about noticing user pain early, proving what changed, and getting from alert to root cause without turning on-call into archaeology.


This post is narrower: a production checklist for teams that already run Kubernetes and want monitoring that works during incidents.


```text
flowchart TB
Alert["Alert fires"]
Symptom["User-facing symptom"]
Service["Service health"]
Runtime["Pod, node, and workload state"]
Signals["Logs, traces, events, metrics"]
Change["Deploy and config changes"]
Cause["Likely root cause"]
Action["Rollback, fix, scale, or tune"]


Alert --> Symptom
Symptom --> Service
Service --> Runtime
Runtime --> Signals
Signals --> Change
Change --> Cause
Cause --> Action
```


A useful Kubernetes monitoring system moves from symptom to evidence to action


## Quick Kubernetes Monitoring Checklist


Use this as the short version. Every row should have an owner, a dashboard, and a clear alerting policy.


Practice Why it matters Signals to collect Common mistake


Monitor service health first Users feel slow or broken services before they feel node CPU Request rate, errors, latency, saturation, SLO burn Starting with node dashboards and hoping they explain app failures


Cover every layer Kubernetes incidents move across layers quickly Control plane, nodes, pods, services, network, dependencies Monitoring pods but ignoring DNS, storage, ingress, and third-party calls


Standardize labels Queries and alerts need stable ownership metadata` service` ,` team` ,` environment` ,` cluster` ,` namespace` ,` version` Letting every team invent a different label vocabulary


Use RED and USE together App symptoms and resource pressure answer different questions Rate, errors, duration, utilization, saturation, errors Alerting on CPU without knowing whether users are affected


Correlate signals Incidents rarely fit into one telemetry type Metrics, logs, traces, events, deploys, configs Buying separate tools that cannot pivot between signals


Keep dashboards operational Dashboards should answer on-call questions fast Golden paths, dependencies, rollouts, capacity, noisy alerts Building decorative wallboards no one uses during a page


Alert on symptoms Pages should represent urgent, actionable risk SLO burn, high error rate, latency, failed rollouts, capacity risk Paging on every threshold breach, restart, or warning event


Attach change context Most incidents are caused by recent changes Deploys, images, ConfigMaps, Secrets, HPA changes, node churn Looking at metrics with no release timeline


Use eBPF for baseline coverage Not every workload is instrumented well Service maps, network calls, RED metrics, traces, profiling Waiting for every team to add SDKs before monitoring production


Automate first-pass triage Humans should review evidence, not gather it by hand Alert, runbook, traces, logs, events, ownership, code context Sending pages with no hypothesis or next step


## 1. Monitor Service Health Before Cluster Health


Start with the thing users feel. For most services, that means latency, traffic, errors, and saturation. Google's SRE book calls these the[four golden signals](https://sre.google/sre-book/monitoring-distributed-systems/) : latency, traffic, errors, and saturation. The names are old. The point is still right.


For an API, watch:


- Request rate by route, method, status code, and caller.
- Error rate split by user-visible failures and dependency failures.
- p50, p90, p95, and p99 latency, with failed requests separated.
- Saturation signals such as CPU throttling, queue depth, connection pool exhaustion, memory pressure, and worker lag.


Only after service health is visible should you walk down into cluster health. Node pressure matters. Pod restarts matter. Control plane health matters. But if a checkout API is serving 500s, the first question is not "what is node CPU?" It is "which user path is broken, when did it start, and what changed?"


Service health should be the first page of the incident, not an afterthought below node graphs


## 2. Cover The Kubernetes Layers That Actually Fail


A production Kubernetes monitoring setup needs more than pod CPU and memory. Kubernetes itself exposes component metrics in Prometheus format from` /metrics` endpoints, and the kubelet exposes additional cAdvisor, resource, and probe metrics according to the[Kubernetes observability docs](https://kubernetes.io/docs/concepts/cluster-administration/observability/) . Use those signals, but organize them by failure domain.


Monitor these layers:


- **Control plane:** API server latency and errors, scheduler health, controller-manager behavior, etcd health where available, API throttling, admission webhooks, and managed control plane incidents.
- **Nodes:** readiness, memory pressure, disk pressure, inode pressure, network saturation, CPU pressure, filesystem growth, and kernel-level drops. Kubernetes documents[node-pressure eviction](https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/) when memory, disk, or inodes cross thresholds.
- **Workloads:** pod phase, readiness, restarts, CrashLoopBackOff, ImagePullBackOff, OOMKilled, rollout status, replica availability, probe failures, and job completion.
- **Services and ingress:** status codes, latency, retries, TLS errors, route health, endpoint changes, and load balancer health.
- **Network and dependencies:** DNS errors, service-to-service latency, external API calls, database calls, queue latency, refused connections, and timeouts.
- **Storage:** PVC state, mount failures, attach errors, disk latency, IOPS saturation, and volume expansion issues.


The[resource metrics pipeline](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/) is useful for HPA and` kubectl top` , but Kubernetes is explicit that it only provides the minimum CPU and memory metrics for autoscaling. That is not enough for incident response.


Workload monitoring should preserve pod state, resource pressure, and rollout context in one place


## 3. Standardize Labels And Ownership Metadata Early


Bad labels make good telemetry useless.


Kubernetes says[labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) are key-value pairs used to organize and select objects, and the[recommended app.kubernetes.io labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/) exist so tools can understand applications consistently. That guidance is practical, not academic. During an incident, you need to answer:


- Which team owns this service?
- Which deploy changed it?
- Which environment and cluster are affected?
- Which namespace, workload, pod, node, and container produced the signal?
- Which customer, region, or tenant is affected if your system supports that split?


At minimum, enforce these across metrics, logs, traces, and events:


```text
app.kubernetes.io/name  :   checkout
app.kubernetes.io/instance  :   checkout -  prod
app.kubernetes.io/version  :    '2026.05.20.3'
app.kubernetes.io/component  :   api
app.kubernetes.io/part-of  :   storefront
app.kubernetes.io/managed-by  :   helm
team  :   payments
environment  :   production


```


Do not wait to clean this up later. Later means after a page, when every query has a different spelling for the same service.


## 4. Use RED And USE, Plus Kubernetes-Specific Signals


RED metrics tell you how the service is behaving:


- Rate: how much work is arriving.
- Errors: how much work is failing.
- Duration: how long work takes.


USE metrics tell you whether a resource is stressed:


- Utilization: how busy it is.
- Saturation: how much work is queued or throttled.
- Errors: whether the resource is failing.


Kubernetes adds its own signals:


- Pod readiness and availability.
- Restarts, OOM kills, and probe failures.
- Scheduling failures and pending pods.
- HPA decisions and scaling lag. The[HPA controller](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/) scales from observed metrics, but missing metrics and not-yet-ready pods can dampen scaling behavior.
- Deployment progress, rollback events, image changes, and config changes.
- Kubernetes events. The[Event API docs](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/event-v1/) describe events as best-effort supplemental data with limited retention, so persist them if you want them during post-incident review.


You want both views. RED says "checkout p95 latency doubled." USE plus Kubernetes state says "new pods are throttled, two are not ready, and HPA is waiting on metrics."


## 5. Collect Logs, Traces, Events, And Metrics Together


Metrics detect many problems. They rarely explain the whole problem.


Kubernetes logging also has a trap: container logs are easy to access with` kubectl logs` , but Kubernetes does not provide a native cluster-level log storage backend. The[logging architecture docs](https://kubernetes.io/docs/concepts/cluster-administration/logging/) recommend separate storage and commonly use a node-level logging agent, usually as a DaemonSet.


That gives you log collection. It does not give you incident context by itself.


A good setup lets you pivot:


- From an alert to the affected service.
- From the service to the exact pod, node, image, and rollout.
- From the pod to logs, traces, events, and resource metrics in the same time window.
- From a slow endpoint to the downstream service, database, queue, or external API.
- From a deploy to the telemetry that changed after it shipped.


This is where[Kubernetes observability](https://metoro.io/blog/kubernetes-observability) matters. Not as a buzzword. As a navigation problem.


A service map turns dependency behavior into an incident surface, not just a diagram


## 6. Keep Dashboards Operational, Not Decorative


A production dashboard should answer questions an on-call engineer asks under pressure.


Good dashboards show:


- Is the service healthy?
- Which routes, consumers, or tenants changed?
- Which dependencies are slow or failing?
- Which pods are serving the bad traffic?
- Did this start after a deployment, config change, autoscaler event, or node event?
- Is the problem isolated to one cluster, namespace, node pool, or availability zone?
- What is the next useful drilldown?


Bad dashboards show twenty charts because those metrics were easy to collect.


Prometheus and Grafana are excellent building blocks. Prometheus scrapes and stores metrics, Grafana visualizes them, and both are common in Kubernetes stacks. But dashboards stop being enough when the responder has to manually join metric labels, logs, trace IDs, pod state, and deploy history across five tabs.


[Metoro Kubernetes dashboards and metrics](https://metoro.io/features/kubernetes-dashboarding-and-metrics) is built around that gap: PromQL-compatible querying, Kubernetes templates, Grafana import, metrics, logs, traces, and resource state on the same canvas.


## 7. Alert On Symptoms And SLO Burn, Not Every Cause


Prometheus gives the clearest short version of alerting best practice: keep alerts simple, alert on symptoms, and avoid pages where there is nothing to do. The[Prometheus alerting docs](https://prometheus.io/docs/practices/alerting/) also recommend paging on high latency and error rates high in the stack.


That should shape Kubernetes alerting.


Page on:


- Sustained user-visible error rate.
- Latency SLO burn for a critical service.
- Failed critical jobs when the missed job will hurt users.
- Failed rollouts that reduce availability.
- DNS failure affecting live traffic.
- Capacity exhaustion that will become an outage soon.
- Repeated OOM kills or restarts tied to service impact.


Ticket or notify on:


- A single pod restart with no service impact.
- CPU above a generic threshold for a short period.
- A non-critical warning event.
- A namespace nearing quota days before impact.
- A low-priority deployment taking longer than usual.


Google's[SRE workbook](https://sre.google/workbook/alerting-on-slos/) recommends multi-window burn-rate alerts because they catch fast budget burn while reducing false positives. For a 99.9 percent SLO, their starting page thresholds include 2 percent budget consumption in one hour and 5 percent in six hours.


## 8. Write Alerts Like A Runbook Entry


Alert quality is easiest to see in examples.


Alert Verdict Why


` CPU > 80% for pod checkout-abc123 for 5m` Bad page It may be normal load, and it points at an individual pod rather than user impact


` checkout p95 latency > 750ms and error budget burn > 14x for 5m and 1h` Good page It maps to user pain, urgency, and an SLO


` CrashLoopBackOff exists in namespace prod` Bad page One crash loop can be harmless if no served path is affected


` checkout ready replicas < desired replicas for 10m after rollout and 5xx rate > 2%` Good page It combines rollout failure with service impact


` Kubernetes Warning event count > 0` Bad page Events are noisy and best-effort; most need context before they deserve a human


` CoreDNS error rate > 5% and checkout dependency timeouts increased for 10m` Good page It connects platform failure to affected services


Every page should include:


- Affected service and owning team.
- SLO or user symptom.
- Start time and current severity.
- Recent deploys and config changes.
- Top traces, logs, events, and dashboards.
- Known runbook or likely next action.


If an alert cannot include that context, keep it as a ticket until it can.


## 9. Attach Deploy And Change Context To Every Signal


Kubernetes changes constantly. Pods churn. ReplicaSets rotate. Autoscalers move targets. ConfigMaps and Secrets change. Nodes drain and rejoin.


Monitoring without change context forces people to guess.


Persist and correlate:


- Deployment time, image, commit, author, and rollout status.
- ReplicaSet changes and rollback events.
- ConfigMap and Secret changes.
- HPA decisions and replica count changes.
- Node upgrades, drains, taints, and autoscaler events.
- Ingress, service, endpoint, and network policy changes.


The useful incident question is often not "what is the value of this metric?" It is "what changed five minutes before this metric moved?"


[Metoro Kubernetes APM](https://metoro.io/features/kubernetes-apm-application-performance-monitoring) and[Kubernetes logging](https://metoro.io/features/kubernetes-logging) both lean on that correlation: traces, logs, metrics, Kubernetes state, and deployment history should be part of the same investigation.


## 10. Use eBPF For Baseline Visibility


Manual instrumentation is still valuable. OpenTelemetry is the right standard for custom spans, metrics, logs, and vendor-neutral pipelines, and the[OpenTelemetry Kubernetes docs](https://opentelemetry.io/docs/platforms/kubernetes/) exist because Kubernetes users need consistent observability tooling.


But production clusters always contain gaps:


- Services without SDKs.
- Third-party containers.
- Legacy apps.
- Jobs and internal tools no one instrumented.
- New services that shipped before telemetry was finished.


eBPF helps fill that baseline. The[eBPF project](https://ebpf.io/what-is-ebpf/) describes eBPF as a way to run sandboxed programs in privileged kernel contexts without changing kernel source or loading kernel modules. For monitoring, that means you can capture useful runtime and network behavior from the node. The[Grafana Beyla docs](https://grafana.com/docs/beyla/latest/) describe eBPF auto-instrumentation capturing RED metrics and trace spans without application code changes.[Pixie](https://docs.px.dev/about-pixie/what-is-pixie/) similarly documents automatic Kubernetes telemetry without manual instrumentation.


That does not make SDKs obsolete. It means your default coverage is not blocked on every application team doing perfect instrumentation first.


## 11. Use AI For First-Pass Triage, Not Blind Autopilot


AI alert investigation is useful when it does the boring first ten minutes:


- Pull the alert and its thresholds.
- Identify the affected service, owner, and recent changes.
- Check service health, dependency behavior, and rollout state.
- Gather relevant traces, logs, Kubernetes events, and metrics.
- Compare the current incident to previous ones.
- Suggest likely root cause and next steps.
- Decide whether the alert is noisy, actionable, or missing context.


That is not magic. It is structured evidence gathering.


[Metoro AI Alert Investigation](https://metoro.io/features/ai-alert-investigation) investigates firing alerts with telemetry, deploy history, Kubernetes metadata, runbooks, and prior context.[Metoro AI SRE](https://metoro.io/ai-sre-agent) extends the same workflow toward remediation: root cause, evidence, and proposed fixes or PRs when the next step is clear.


AI triage is useful when it brings evidence, not when it guesses from a metric name


## MTTR Work vs Telemetry Work


More telemetry can help. It can also make incidents slower if responders have to stitch it together manually.


Reduces MTTR Mostly adds telemetry


Alerts tied to SLO burn and service ownership Alerts on every pod restart


Service maps with live latency, error, and request volume Static architecture diagrams


Logs, traces, metrics, and events filtered to the same service and time window Separate tools with different label schemes


Deployment and config changes on the incident timeline Release notes in a different system


Dashboards built around incident questions Dashboards built around every available metric


eBPF baseline coverage for uninstrumented workloads Waiting for every service to add custom SDKs


AI triage that collects evidence and suggests next actions AI summaries with no links back to data


## Example Alert-To-Root-Cause Workflow


Here is what the workflow should feel like.


1. A page fires:` checkout` is burning error budget and p95 latency crossed the page threshold.
2. The service dashboard shows latency rose after the last deployment.
3. The service map shows new calls from` checkout` to` pricing-cache` .
4. Traces show most slow requests wait on` pricing-cache` .
5. Logs show connection pool exhaustion in the` checkout` pods.
6. Kubernetes events show new pods are ready, but HPA scale-up lagged because metrics were briefly missing.
7. The deploy timeline shows a pool-size change in the new image.
8. The responder rolls back or patches the pool config, then keeps a ticket to tune the HPA and dashboard.


That is Kubernetes monitoring working. The page started at a user symptom. The system kept enough correlated evidence to find the cause. The next action was obvious.


## The Short Version


Kubernetes monitoring best practices are mostly discipline:


- Watch user-facing service health before low-level infrastructure.
- Cover control plane, nodes, workloads, services, network, dependencies, logs, traces, and events.
- Keep labels and ownership metadata boring and consistent.
- Use RED and USE together.
- Build dashboards for incident questions.
- Page on symptoms, SLO burn, and imminent risk.
- Attach deploy and config context everywhere.
- Use eBPF to cover what manual instrumentation misses.
- Use AI to gather evidence and shorten triage, while humans stay in control.


Metoro is built for this model:[Kubernetes APM](https://metoro.io/features/kubernetes-apm-application-performance-monitoring) ,[logs](https://metoro.io/features/kubernetes-logging) ,[dashboards and metrics](https://metoro.io/features/kubernetes-dashboarding-and-metrics) , service maps, traces, events, eBPF telemetry, and AI alert investigation in one Kubernetes-native workflow.


## FAQ


What are the most important Kubernetes monitoring best practices?


Start with user-facing service health, then monitor the cluster layers that explain it: control plane, nodes, pods, workloads, services, network, dependencies, logs, traces, events, and recent changes. Use consistent labels, alert on symptoms and SLO burn, and correlate signals in one workflow.


What should I monitor in Kubernetes?


Monitor request rate, error rate, latency, saturation, pod readiness, restarts, OOM kills, probe failures, deployment status, node pressure, scheduling failures, DNS health, ingress and egress latency, dependency errors, logs, traces, Kubernetes events, and autoscaler behavior.


What is the best way to alert on Kubernetes issues?


Page on user-visible symptoms and imminent risk: SLO burn, high error rate, high latency, unavailable services, failed critical jobs, failed rollouts, DNS failure affecting traffic, repeated OOM kills tied to impact, or capacity exhaustion. Use tickets for hygiene alerts and isolated low-impact events.


Is Prometheus and Grafana enough for Kubernetes monitoring?


Prometheus and Grafana can be enough for metrics and dashboards, especially for teams with strong platform engineering. They are not the whole incident workflow by themselves. Production teams still need logs, traces, Kubernetes events, deployment history, ownership metadata, alert routing, and fast correlation.


How does eBPF help Kubernetes monitoring?


eBPF can collect runtime and network telemetry from the node without requiring every workload to add an SDK. That helps teams see service dependencies, request behavior, RED metrics, traces, and profiling signals for services that are uninstrumented, third-party, or newly shipped.


How do Kubernetes observability best practices differ from monitoring best practices?


Monitoring watches known symptoms with dashboards and alerts. Observability helps investigate unknown causes by correlating metrics, logs, traces, profiles, Kubernetes events, resource state, deployment history, and ownership metadata. Good Kubernetes monitoring depends on enough observability context to explain alerts.
