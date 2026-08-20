---
schema_version: "1.0.0"
document_id: "dc6ee5839b74c059432d9d1f7029fa442b45f28f960edf0280fc1a0b51499d0b"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/kubernetes-deployment"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T13:44:31.368704+00:00"
fetched_at: "2026-08-05T13:44:32.060382+00:00"
content_hash: "sha256:a6a24a12364877ce6284c7aada688bfbddd549790a974732fa049f0101508888"
---

# Kubernetes Deployment: A complete guide to YAML, strategies, rollouts, and scaling (2026)

You've got a containerized app, and you need it to run reliably, update without downtime, and recover on its own when something breaks. In[Kubernetes](https://kubernetes.io/) , the object that handles all of that is the Deployment.


A Deployment is a declarative description of the application you want running. You tell Kubernetes the desired state, container image, replica count, update strategy, and the Deployment controller keeps the actual state matching it, continuously, without you managing individual Pods by hand.


This guide covers the full path, from the Deployment object itself through writing and structuring the YAML, all four deployment strategies, managing rollouts and rollbacks, health checks, scaling, and the production practices that keep Deployments reliable at scale.


**See it yourself:** Reading about rolling updates and self-healing Pods only gets you so far. Deploy a manifest on a real cluster, kill a Pod, watch a rollout, and you'll understand the mechanics faster than any explanation gets you there.


[Rackspace Spot](https://spot.rackspace.com/) nodes start at $0.72 a month each, cheap enough to run every example in this guide yourself.


## Understanding the Kubernetes deployment object


### What is a Kubernetes deployment?


A[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) is a higher-level Kubernetes API object for managing an application's lifecycle. You don't create and track Pods yourself, you describe the desired state in a Deployment, and the Deployment controller continuously reconciles the actual state to match it.


Managing Pods directly means handling failures, updates, and scaling manually. A Deployment automates all three, creating a ReplicaSet to maintain your Pod count, replacing failed Pods automatically, and coordinating rolling updates when you change the Pod template. It sits above ReplicaSets and Pods in the orchestration hierarchy, and it's the object almost every stateless workload in Kubernetes runs on.


‍


### Core components of a deployment


A Deployment is built from a small set of pieces that work together:


- **Pods:** the actual running containers, created from the Pod template
- [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) **:** the object the Deployment creates and manages to keep the specified number of Pod replicas running
- **Pod template** (` .spec.template` ): the blueprint for every Pod the Deployment creates
- **Selector** (` .spec.selector` ): the label query the Deployment uses to identify which Pods belong to it
- **Metadata:** the Deployment's own` name` ,` namespace` , and` labels`


When you update a Deployment's Pod template, it creates a new ReplicaSet and scales it up while scaling the old one down, according to whatever strategy you've configured. The Deployment itself never touches Pods directly. It manages ReplicaSets, and ReplicaSets manage Pods.


‍


### The deployment spec field explained


The` .spec` block controls everything about how the Deployment behaves:


- **` .spec.replicas` :** the desired number of Pod replicas
- **` .spec.selector.matchLabels` :** the label selector that must match` .spec.template.metadata.labels`
- **` .spec.template` :** the full Pod template, including containers, volumes, and Pod-level settings
- **` .spec.strategy` :** the update strategy,` RollingUpdate` (default) or` Recreate`
- **` .spec.minReadySeconds` :** how long a new Pod must stay ready before it's considered available
- **` .spec.progressDeadlineSeconds` :** how long the controller waits for a rollout to make progress before reporting it as stalled
- **` .spec.revisionHistoryLimit` :** how many old ReplicaSets to keep around for rollback


## Writing and structuring a Kubernetes deployment YAML file


### Anatomy of a deployment YAML manifest


Every Deployment manifest needs four top-level fields:


```text
apiVersion:     apps/v1
kind:     Deployment
metadata:
name:     my-app
namespace:     production
spec:
# replicas, selector, and template go here
```


` apiVersion: apps/v1` is the current, stable API version for Deployments, not` v1` (that's for core objects like Pods and Services) and not` extensions/v1beta1` (a much older API that was removed from Kubernetes years ago and will error out on any current cluster). If you copy a manifest from an old tutorial and it uses` extensions/v1beta1` , change it to` apps/v1` before applying it.


` metadata.name` identifies the Deployment itself, and` metadata.namespace` determines which namespace it lives in, separate from any labels or names you set inside the Pod template.


‍


### Defining containers within the pod template


The Pod template inside` .spec.template` describes every container the Deployment runs:


```text
spec:
template:
spec:
containers:
-     name:     my-app
image:     my-registry/my-app:1.4.2
imagePullPolicy:     IfNotPresent
ports:
-     containerPort:     8080
env:
-     name:     LOG_LEVEL
value:     "info"
resources:
requests:
cpu:     "250m"
memory:     "256Mi"
limits:
cpu:     "500m"
memory:     "512Mi"
```


` name` and` image` are required.` imagePullPolicy` controls whether Kubernetes re-pulls the image on every Pod start.` resources.requests` and` resources.limits` set the CPU and memory the scheduler reserves and enforces, covered in more depth in the best practices section below. For configuration and secrets, reference a ConfigMap or Secret instead of hardcoding values:


```text
env:
-     name:     API_KEY
valueFrom:
secretKeyRef:
name:     my-app-secrets
key:     api-key
```


` command` and` args` override the container image's default entrypoint and arguments when you need to.


‍


### Labels, selectors, and namespace configuration


Keep a consistent label schema across every Deployment, something like` app` ,` version` , and` environment` , so you can select and filter workloads predictably:


```text
metadata:
labels:
app:     my-app
version:     "1.4.2"
environment:     production
```


The single most common Deployment failure is a mismatch between` .spec.selector.matchLabels` and` .spec.template.metadata.labels` . The selector is immutable after creation, and if it doesn't match the Pod template's labels, the Deployment can't find or manage its own Pods, so` kubectl apply` fails outright with a selector-immutable error. Double-check these two label sets match exactly before applying.


` metadata.namespace` isolates workloads by team, application, or environment. Add a` kubernetes.io/change-cause` annotation whenever you apply a meaningful change too, since that's what shows up in rollout history later.


‍


### Sample deployment YAML walkthrough


Here's a complete, production-ready Deployment manifest:


```text
apiVersion:     apps/v1
kind:     Deployment
metadata:
name:     my-app
namespace:     production
labels:
app:     my-app
annotations:
kubernetes.io/change-cause:     "Initial deployment of v1.4.2"
spec:
replicas:     3
selector:
matchLabels:
app:     my-app
strategy:
type:     RollingUpdate
rollingUpdate:
maxUnavailable:     1
maxSurge:     1
minReadySeconds:     10
progressDeadlineSeconds:     300
revisionHistoryLimit:     5
template:
metadata:
labels:
app:     my-app
spec:
containers:
-     name:     my-app
image:     my-registry/my-app:1.4.2
ports:
-     containerPort:     8080
resources:
requests:
cpu:     "250m"
memory:     "256Mi"
limits:
cpu:     "500m"
memory:     "512Mi"
readinessProbe:
httpGet:
path:     /healthz
port:     8080
initialDelaySeconds:     5
periodSeconds:     10
```


Line by line,` replicas: 3` keeps three Pods running.` selector.matchLabels` and` template.metadata.labels` both use` app: my-app` , matching as required.` strategy` sets a rolling update with one Pod unavailable and one extra Pod allowed at a time. The Pod template defines the container, its resource limits, and a readiness probe.


The most common YAML pitfalls are indentation errors (YAML is whitespace-sensitive) and misspelled field names, which` kubectl` won't always catch until apply time. Validate before applying:


```text
kubectl apply --dry-run=client -f deployment.yaml
```


Then apply for real and confirm it rolled out:` ‍`


```text
kubectl apply -f deployment.yaml
kubectl rollout status deployment/my-app
```


‍


## Kubernetes deployment strategies


### Rolling update strategy (default)


` RollingUpdate` is the default strategy and the one most production Deployments use. Kubernetes replaces Pods incrementally, creating a new ReplicaSet and scaling it up gradually while scaling the old one down, controlled by` maxUnavailable` and` maxSurge` .


```text
strategy:
type:     RollingUpdate
rollingUpdate:
maxUnavailable:     25  %
maxSurge:     25  %
```


` maxUnavailable` caps how many Pods can be unavailable during the rollout, and` maxSurge` caps how many extra Pods can run above the desired count while the update is in progress. Both default to 25% if you omit them. The result is zero-downtime, but with a temporary window where old and new Pod versions run side by side, which matters if your app can't tolerate two versions talking to the same data at once.


‍


### Recreate strategy


` Recreate` terminates every existing Pod before creating any new ones:


```text
strategy:
type:     Recreate
```


No old-and-new-version overlap makes this the right choice for stateful applications, schema migrations, or anything that genuinely can't run two versions simultaneously. The tradeoff is real downtime between the old Pods terminating and the new ones becoming ready.


‍


### Canary deployment strategy


A canary deployment sends a small percentage of traffic to a new version before rolling it out fully. Kubernetes doesn't have a built-in canary primitive. You build it from two Deployments sharing a label and a single Service:


```text
# Stable Deployment: 9 replicas
metadata:
labels:
app:     my-app
track:     stable
---
# Canary Deployment: 1 replica
metadata:
labels:
app:     my-app
track:     canary
```


A Service selecting on` app: my-app` (without the` track` label) sends traffic to both, roughly proportional to each Deployment's replica count. For precise traffic-weight control instead of replica-count approximation, use an Ingress controller or a service mesh like Istio to split traffic explicitly. Monitor the canary's error rate and latency before promoting it to the full replica count.


‍


### Blue-green and other advanced strategies


Blue-green deployment runs two full Deployments, "blue" (current) and "green" (new), and switches a Service's selector from one to the other for an instant cutover:


```text
# Switch the Service to the new version
spec:
selector:
app:     my-app
track:     green      # was: blue
```


Rollback is just as instant, since switching the selector back reverses the cutover immediately. The cost is running two full-scale environments simultaneously during the transition.


The table below compares all four strategies:


Strategy Downtime Complexity Best for


Rolling update None Low (built in) Most stateless applications


Recreate Yes, during the swap Low (built in) Stateful apps, schema migrations


Canary None Medium (needs Ingress/mesh for precision) Risk-sensitive releases


Blue-green None Medium (2x resource cost during cutover) Instant rollback requirements


Choosing between them comes down to app type, team maturity, and how strict your SLAs are. Rolling update covers most cases by default, and only reach for the others when you have a specific reason to.


## Managing rollouts, rollbacks, and versioning


### Monitoring rollout status


Watch a rollout in progress with:


```text
kubectl rollout status deployment/my-app
```


Running this command blocks until the rollout completes or fails, and it's the first place to look when a rollout seems stuck. Common stall causes are image-pull errors, insufficient cluster resources, or Pods failing their readiness probe.` progressDeadlineSeconds` controls how long the controller waits before marking the rollout as failed instead of waiting indefinitely.


‍


### Performing and controlling rollbacks


Roll back to the previous revision:


```text
kubectl rollout undo deployment/my-app
```


Or target a specific revision:


```text
kubectl rollout undo deployment/my-app --to-revision=3
```


Kubernetes keeps old ReplicaSets around (up to` revisionHistoryLimit` ) specifically so a rollback doesn't require rebuilding anything. It just scales the target revision's ReplicaSet back up. Verify the rollback the same way you'd verify any rollout:


```text
kubectl rollout status deployment/my-app
kubectl describe deployment my-app
```


‍


### Restarting a deployment


Sometimes you need every Pod to restart without actually changing anything in the spec, picking up a rotated Secret, clearing a bad in-memory state, or forcing a fresh start after a dependency recovers. The command for that is:


```text
kubectl rollout restart deployment/my-app
```


A restart is different from scaling to zero and back up, since it still respects your configured rolling-update strategy and readiness probes, so it stays zero-downtime the same way a normal update does. Scaling to zero, by contrast, briefly takes your application fully offline.` rollout restart` is the correct tool any time you want a rolling restart without touching the manifest itself.


‍


### Revision history and change tracking


Every time you change the Pod template, whether through` kubectl apply` or` kubectl set image` , the Deployment creates a new revision. View the history with:


```text
kubectl     rollout     history     deployment/my-app
```


Older Kubernetes tutorials recommend the` --record` flag to annotate each change automatically, but that flag is deprecated. Use the` kubernetes.io/change-cause` annotation directly in your manifest instead:


```text
metadata:
annotations:
kubernetes.io/change-cause:     "Bump to v1.4.2, fixes memory leak"
```


` revisionHistoryLimit` controls how many old ReplicaSets Kubernetes retains for rollback purposes. Tune it down on high-churn Deployments to avoid accumulating stale ReplicaSets.


‍


### Pausing and resuming rollouts


Pause a rollout to batch multiple changes into a single update instead of triggering a new rollout for each one:


```text
kubectl rollout pause deployment/my-app
# make several changes
kubectl rollout resume deployment/my-app
```


Batching changes this way is especially useful in CI/CD pipelines that apply several related changes in sequence and only want one rollout to fire at the end.


## Health checks: Readiness and liveness probes


### Liveness probes


A liveness probe detects containers that are running but stuck, deadlocked, hung, unresponsive, and need a restart. Kubernetes restarts any container that fails its liveness probe:


```text
livenessProbe:
httpGet:
path: /healthz
port  :   8080
initialDelaySeconds  :   10
periodSeconds  :   10
timeoutSeconds  :   3
failureThreshold  :   3
```


You can probe with` httpGet` ,` tcpSocket` , or` exec` , depending on what your application exposes. A failing liveness probe also affects rollout progress, since a Pod that keeps restarting never reaches a stable ready state.


‍


### Readiness probes


A readiness probe controls whether a Pod receives traffic from a Service, without restarting it. A Pod that fails readiness is removed from Service endpoints but left running:


```text
readinessProbe:
httpGet:
path: /ready
port  :   8080
initialDelaySeconds  :   5
periodSeconds  :   5
```


Readiness probes matter most during rolling updates, since Kubernetes waits for new Pods to pass readiness before continuing the rollout and terminating old Pods, which is what keeps a rolling update from ever routing traffic to a Pod that isn't actually ready yet. Pair this with` minReadySeconds` for extra insurance against a Pod that reports ready too early.


‍


### Startup probes and combined probe strategies


Slow-starting applications, ones with a long initialization sequence, can fail liveness checks before they've even finished starting. A startup probe solves this by disabling liveness and readiness checks until the startup probe itself succeeds:


```text
startupProbe:
httpGet:
path:     /healthz
port:     8080
failureThreshold:     30
periodSeconds:     10
```


The recommended combination for most production workloads combines all three together, a startup probe for slow initialization, a liveness probe to catch hangs, and a readiness probe to gate traffic. Misconfigured probes, thresholds set too aggressively, are one of the most common causes of unnecessary Pod churn, so test probe timing in staging before shipping it to production.


## Scaling Kubernetes deployments


### Manual scaling


Change the replica count directly:


```text
kubectl scale deployment/my-app --replicas=5
```


Or edit` .spec.replicas` in the manifest and reapply. Manual scaling works fine for predictable, steady-state workloads, but it doesn't respond to real-time demand the way autoscaling does, and it has no effect on stateful workloads that can't simply add more identical replicas.


‍


### Horizontal pod autoscaling (HPA)


The[Horizontal Pod Autoscaler](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/) adjusts` .spec.replicas` automatically based on observed CPU, memory, or custom metrics:


```text
kubectl autoscale deployment/my-app --cpu-percent=70 --min=3 --max=10
```


Or as a manifest:


```text
apiVersion:     autoscaling/v2
kind:     HorizontalPodAutoscaler
metadata:
name:     my-app
spec:
scaleTargetRef:
apiVersion:     apps/v1
kind:     Deployment
name:     my-app
minReplicas:     3
maxReplicas:     10
metrics:
-     type:     Resource
resource:
name:     cpu
target:
type:     Utilization
averageUtilization:     70
```


HPA ships as a built-in Kubernetes controller, no separate installation required. If you also set` .spec.replicas` manually while HPA is active, HPA's target takes over on the next reconcile, so don't fight the autoscaler with manual scaling on the same Deployment.


‍


### Vertical pod autoscaling (VPA) and cluster autoscaling


Where HPA changes replica count, the[Vertical Pod Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) changes the resource requests and limits on existing Pods, useful for workloads where the right replica count is stable but the right container size isn't. Unlike HPA, VPA isn't built into core Kubernetes, since it's a separate project you install and run in your cluster.


[Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) can also use cheaper spot or preemptible nodes, and adding a node termination handler moves your Pods off before the node gets reclaimed instead of after.


A well-built Deployment (multiple replicas, self-healing) tolerates a node getting reclaimed, so[autoscaling onto spot-priced nodes](https://spot.rackspace.com/docs/en/autoscaling-a-spot-node-pool) cuts infrastructure cost without sacrificing availability.


HPA, VPA, and Cluster Autoscaler can run together, but HPA and VPA configured on the same metric (CPU, for instance) can conflict with each other, so most teams pick one to drive replica count and the other to drive Pod sizing, not both on the same signal.


‍


### High availability through replica management


Running more than one replica removes a single point of failure, so if one Pod dies, the others keep serving traffic while the ReplicaSet replaces it. Pod anti-affinity rules spread those replicas across different nodes or availability zones, so a single node or zone failure doesn't take out every replica at once:


```text
affinity:
podAntiAffinity:
preferredDuringSchedulingIgnoredDuringExecution:
-     weight:     100
podAffinityTerm:
labelSelector:
matchLabels:
app:     my-app
topologyKey:     kubernetes.io/hostname
```


The properties that make a Deployment highly available (multiple replicas, anti-affinity spread, self-healing) are the same properties that make it safe to run on interruptible spot capacity. A resilient Deployment is, by construction, a spot-ready Deployment.


## Kubernetes deployment best practices for production


### Resource management and configuration


A few practices separate production-grade Deployments from ones that work until they don't:


- **Set` resources.requests` and` resources.limits` on every container.** An unset request means the scheduler has no idea how much capacity to reserve, and an unset limit means one runaway container can starve everything else on the node.
- **Reference ConfigMaps and Secrets instead of hardcoding configuration values.**
- **Pin image tags to a specific digest or semantic version, not` latest` .**
- **Structure your YAML for GitOps:** version-controlled, fully declarative, and auditable through git history.


Right-sized requests and limits are also what keep a Deployment cheap to run, and cheapest on spot-priced managed Kubernetes, where you're paying for exactly the capacity you've actually reserved.


‍


### Security and namespace isolation


Production Deployments need a few security defaults in place before they go live:


- **Scope RBAC per namespace** rather than granting cluster-wide access by default
- **Give each team, application, and environment its own dedicated namespace**
- **Set` securityContext` to run containers as non-root** with a read-only root filesystem wherever the application allows it
- **Scan images for known vulnerabilities** before they reach production


‍


### Load balancing and service integration


A Deployment on its own has no stable network identity. Pair it with a Service for that. The Service's selector needs to match the Pod template's labels, the same matching requirement covered earlier for the Deployment's own selector, and an Ingress handles HTTP/HTTPS routing on top of the Service. Readiness probes keep this integration correct during rollouts, since the Service only routes to Pods that have actually passed their readiness check, at every point during an update.


‍


### CI/CD integration and DevOps workflows


A CI/CD pipeline built around Deployments typically combines a few pieces:


- **` kubectl set image`** for simple image bumps, or **` helm upgrade`** if you're managing the Deployment through a Helm chart
- **GitOps tools like Argo CD or Flux** , which reconcile your cluster state against manifests stored in Git automatically, removing manual` kubectl apply` from the pipeline entirely
- **` kubectl diff`** to preview a change before it applies
- **Rollback gates** that trigger` kubectl rollout undo` automatically when post-deploy health checks fail


GitOps-friendly managed Kubernetes makes this entire pipeline cheap to run end to end. Rackspace Spot supports[Terraform and GitOps workflows](https://spot.rackspace.com/docs/en/deploy-your-cloudspace-via-terraform) with bring-your-own Helm charts, so the same CI/CD pipeline you'd run anywhere else works unchanged on a lower-cost cluster.


‍


### Observability and ongoing maintenance


Ongoing maintenance for a Deployment comes down to a handful of habits:


- **` kubectl describe deployment` and` kubectl get events`** are the first stop for diagnosing a misbehaving Deployment
- **Feed Deployment metrics** (replica counts, rollout duration, restart rates) into your monitoring dashboards, whatever stack you're already running
- **Prune stale ReplicaSets** by tuning` revisionHistoryLimit`
- **Audit your manifests against current API versions periodically** , so you catch a deprecation before it breaks a cluster upgrade, the same way` extensions/v1beta1` broke anyone who hadn't already moved to` apps/v1`


## Running Kubernetes deployments cost-effectively


Everything above is about how you define and operate a Deployment. This section is about where you run it affordably.


A production-grade Deployment, multiple replicas, anti-affinity, readiness and liveness probes, rolling updates, self-healing, is inherently tolerant of interruption. That's exactly the property that makes it a natural fit for spot capacity instead of full on-demand pricing.


‍[Rackspace Spot](https://spot.rackspace.com/) runs fully managed Kubernetes through a spot-market auction, with a free hosted control plane, both spot and on-demand node pools.


The resilient Deployments this guide teaches you to build can run at a fraction of on-demand cost, connect with[kubectl the same way you would against any cluster](https://spot.rackspace.com/docs/en/access-your-cloudspace-via-kubectl) . Check out the[Rackspace Spot pricing page](https://spot.rackspace.com/pricing) for current rates.


## Frequently asked questions


### What is a Kubernetes Deployment?


A Deployment is a Kubernetes API object that manages the lifecycle of a stateless application. You describe the desired state, replica count, container image, and update strategy, and the Deployment controller continuously reconciles the actual cluster state to match it, creating and managing a ReplicaSet on your behalf.


‍


### What's the difference between a Deployment, a ReplicaSet, and a Pod?


A Pod is the smallest deployable unit, one or more containers running together. A ReplicaSet ensures a specified number of identical Pods are running at any time. A Deployment manages ReplicaSets, creating a new one on every update and controlling how traffic shifts from the old to the new. You typically only interact with the Deployment directly.


‍


### What is the difference between a Deployment and a StatefulSet?


A Deployment treats its Pods as interchangeable, so any replica can serve any request, and Pods get new names each time they're recreated. A StatefulSet gives each Pod a stable, unique identity and stable storage that persists across rescheduling, which is what databases and other stateful applications need.


‍


### How do I create and deploy an application on Kubernetes step by step?


Write a Deployment manifest with` apiVersion: apps/v1` , validate it with` kubectl apply --dry-run=client -f deployment.yaml` , apply it with` kubectl apply -f deployment.yaml` , and confirm the rollout with` kubectl rollout status deployment/<name>` . The full walkthrough above covers each field in the manifest.


‍


### How do I restart a Kubernetes Deployment?


Run` kubectl rollout restart deployment/<name>` . This restarts every Pod through a normal rolling update, respecting your configured strategy and readiness probes, without changing anything in the manifest itself.


‍


### How do I roll back a Kubernetes Deployment?


Run` kubectl rollout undo deployment/<name>` to revert to the previous revision, or add` --to-revision=<n>` to target a specific one. Kubernetes keeps old ReplicaSets around specifically to make this instant.


‍


### What apiVersion should I use for a Deployment?


` apps/v1` , the current, stable API version. Older manifests using` extensions/v1beta1` will fail on any current Kubernetes cluster. That API was removed years ago.


‍


### What are the Kubernetes deployment strategies?


Rolling update (the default, zero downtime, gradual replacement), Recreate (full downtime, no version overlap), Canary (small-percentage traffic to a new version before full rollout), and Blue-green (two full environments with an instant traffic switch).


‍


### How do I scale a Kubernetes Deployment?


Manually with` kubectl scale deployment/<name> --replicas=<n>` , or automatically with the Horizontal Pod Autoscaler based on CPU, memory, or custom metrics. Vertical Pod Autoscaler and Cluster Autoscaler handle Pod sizing and node capacity respectively, on top of HPA's replica-count decisions.


‍
