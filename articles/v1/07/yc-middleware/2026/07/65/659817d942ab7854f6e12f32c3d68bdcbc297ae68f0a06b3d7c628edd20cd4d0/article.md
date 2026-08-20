---
schema_version: "1.0.0"
document_id: "659817d942ab7854f6e12f32c3d68bdcbc297ae68f0a06b3d7c628edd20cd4d0"
company_key: "yc-middleware"
company: "Middleware"
source_id: "yc-middleware-news-import-fe54c9028845"
canonical_url: "https://middleware.io/blog/kubernetes-imagepullbackoff/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-22T04:20:55.832849+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:5c89281ce8f4def30d9da5303096f7c300d46f70f1451908b7a2afefba21211c"
---

# What Is Kubernetes ImagePullBackOff Error and How to Fix It

***Summary:** ImagePullBackOff is a Kubernetes pod status that prevents your application from starting before it can pull the image. The container image can’t be pulled from the registry, so nothing runs. It’s one of the first errors engineers hit when deploying to Kubernetes, and while the fix is almost always straightforward once you know the cause, the error message itself tells you almost nothing. This guide covers every root cause, the exact kubectl commands to identify which one you’re dealing with, and specific fixes to get your pods running again quickly.*


#### TL;DR


- ImagePullBackOff means the kubelet on a worker node cannot pull a container image from a registry. The pod is stuck before any application code runs.
- Kubernetes first logs ErrImagePull on the initial failure, then switches to ImagePullBackOff as it retries with an exponentially increasing delay starting at 5 seconds and capping at 5 minutes.
- Five most common root causes: wrong image name or tag, missing or expired registry credentials (imagePullSecrets), private registry unreachable from the cluster, Docker Hub rate limit exceeded, and architecture mismatch between image and node.
- Fastest diagnosis: run kubectl describe pod and read the Events section. The error message there almost always names the exact cause.
- Unlike CrashLoopBackOff, the failure happens before the container starts. Fixing the image reference or credentials is enough; no application-level debugging needed.


## What Is Kubernetes ImagePullBackOff?


When you deploy a pod in Kubernetes, the kubelet process running on the assigned worker node is responsible for pulling every container image listed in the pod spec. If a pull fails, Kubernetes marks the pod with the status **ImagePullBackOff** a signal that image retrieval has broken down and that Kubernetes is backing off before retrying.


The pod stays stuck in this state, unable to start, until either the underlying issue is resolved or the image becomes accessible. No containers run. No application traffic is served. In production environments, this directly translates to degraded availability or a failed rollout.


ImagePullBackOff is one of the most common Kubernetes errors engineers encounter alongside CrashLoopBackOff and[OOMKilled](https://middleware.io/blog/kubernetes-oomkilled-error/) and it is also one of the most straightforward to diagnose once you know where to look.


> **Key point:** ImagePullBackOff is not a crash. The application never even starts. The problem exists entirely at the container runtime and image registry layer, before your code runs.


## ErrImagePull vs ImagePullBackOff: What Is the Difference?


These two statuses are closely related and share the same root causes, but they represent different stages of the same failure:


Status When it appears What it means


` ErrImagePull` First pull attempt fails Kubelet tried to pull the image and got an error


` ImagePullBackOff` Subsequent retries also fail Kubernetes is backing off, waiting longer between each retry attempt


In practice,` ErrImagePull` disappears quickly because Kubernetes retries almost immediately after the first failure. By the time most engineers look at the pod, it already shows` ImagePullBackOff` . You can treat both statuses as interchangeable when troubleshooting the diagnosis and fix are identical.


## What Happens During an ImagePullBackOff?


Understanding the internal sequence helps you troubleshoot more precisely. When a pod is scheduled and a container image needs to be pulled, the kubelet performs these steps in order:


1. **Parse the image reference** kubelet reads the` spec.containers\[\].image` field in your pod manifest. If no registry hostname is specified, it defaults to Docker Hub (` docker.io` ).
2. **Check local cache** depending on the pod’s` imagePullPolicy` , kubelet may use a locally cached image instead of pulling. With` Always` , it always attempts to pull from the registry, even if the image is cached locally. With` IfNotPresent` , it skips the pull if the image already exists on the node.
3. **Connect to the registry** kubelet resolves the registry hostname via DNS and opens a connection over port 443 (HTTPS). Network failures, DNS misconfiguration, or firewall rules blocking outbound traffic all cause failures here.
4. **Authenticate** for private registries, kubelet presents credentials from the` imagePullSecrets` referenced in the pod spec. Missing or invalid credentials result in an` authorization failed` or` no pull access` error.
5. **Pull image layers** kubelet downloads the image manifest and then each layer. If the specified tag or digest does not exist, the registry returns a “manifest not found” error.


If any step fails, Kubernetes logs an` ErrImagePull` event, then retries. After the first retry fails, the pod enters` ImagePullBackOff` and Kubernetes begins an exponential backoff: 5s → 10s → 20s → 40s → … up to a ceiling of 300 seconds (5 minutes). It will keep retrying on that schedule indefinitely until the issue is resolved or the pod is deleted.


The increasing delay is intentional, it prevents a misconfigured pod from hammering a registry with thousands of failed pull requests per hour.


## All Root Causes of ImagePullBackOff


### 1. Wrong image name or tag


A typo in the image name or an incorrect/non-existent tag is the single most common cause. Kubernetes pulls from Docker Hub by default, so a reference like` nginx:lates` (instead of` nginx:latest` ) will fail with a “manifest not found” error. Tags are case-sensitive and version-specific` myapp:v1.2` and` myapp:V1.2` are different.


This also happens when an image tag that previously existed is deleted or overwritten in the registry. A pod spec that was working yesterday can start producing ImagePullBackOff today if someone removed that tag from the registry.


**Event message to look for:**


```text
Failed to pull image "nginx:lates": ... not found
```


```text
manifest unknown: manifest tagged by "lates" is not found
```


### 2. Missing or incorrect registry credentials


Private container registries including private repos on Docker Hub, AWS ECR, Google Artifact Registry, Azure ACR, or self-hosted registries require authentication. If the pod spec does not include the correct` imagePullSecrets` , or the secret contains outdated credentials, the pull will be denied.


AWS ECR credentials are time-limited (they expire every 12 hours), which is a common source of this error in AWS-based clusters that don’t rotate credentials automatically.


**Event message to look for:**


```text
Failed to pull image "...: pull access denied ... repository does not exist or may require 'docker login'
```


```text
authorization failed: no basic auth credentials
```


### 3. Private registry unreachable from the cluster


Even with valid credentials, the pull fails if the worker nodes cannot reach the registry over the network. This includes:


- Network policies blocking egress to external registries
- Firewall or security group rules that don’t allow outbound HTTPS (port 443)
- DNS misconfiguration on nodes preventing registry hostname resolution
- The registry itself being down or under maintenance
- Self-signed TLS certificates on private registries that the container runtime doesn’t trust


**Event message to look for:**


```text
net/http: request canceled while waiting for connection (Client.Timeout exceeded)
```


```text
dial tcp: lookup registry.example.com: no such host
```


### 4. Registry rate limits exceeded


Docker Hub imposes pull rate limits: unauthenticated pulls from a shared IP are capped at 100 pulls per 6 hours. In Kubernetes clusters where many pods share a single NAT IP (common in cloud-managed clusters), this limit is easily exceeded during deployments or autoscaling events. The pull doesn’t fail permanently it recovers once the rate limit window resets but it can cause intermittent ImagePullBackOff during high-traffic periods.


**Event message to look for:**


```text
toomanyrequests: You have reached your pull rate limit
```


```text
429 Too Many Requests
```


### 5. Architecture mismatch


If you’re running a mixed-architecture cluster (e.g., both amd64 and arm64 nodes), and the image in your registry does not include a layer for the node’s architecture, the pull will fail. This is increasingly common as teams adopt ARM-based instances (AWS Graviton, Apple M-series dev machines) without ensuring images are built as multi-arch.


**Event message to look for:**


```text
no matching manifest for linux/arm64/v8 in the manifest list entries
```


### 6. ErrImageNeverPull


A related but distinct error: if a pod’s` imagePullPolicy` is set to` Never` and the image is not already cached on the node, Kubernetes will not attempt to pull it at all and the pod will fail immediately. This is most commonly encountered in air-gapped or local development setups where images must be pre-loaded onto nodes.


## How to Diagnose ImagePullBackOff: Step-by-Step


### Step 1: List pods and identify affected ones


```text
kubectl get pods -n <namespace>
```


Look for pods showing` ImagePullBackOff` or` ErrImagePull` in the STATUS column:


```text
NAME           READY   STATUS             RESTARTS   AGE
api-server     0/1     ImagePullBackOff   0          4m
worker-7xkp2   1/1     Running            0          12m
```


### Step 2: Describe the pod and read the Events section


This is your primary diagnostic command. Run it and save the output:


```text
kubectl describe pod <pod-name> -n <namespace>
```


Or save to a file for easier searching:


```text
kubectl describe pod <pod-name> -n <namespace> > /tmp/pod-describe.txt
```


Scroll to the bottom of the output to find the **Events** section. This is where the root cause lives. You’re looking for one of these message patterns:


Message in Events What it means


` manifest ... not found` Wrong tag or digest, or image was deleted from registry


` no pull access` /` repository does not exist` Wrong image path, or credentials missing/insufficient


` authorization failed` Invalid or expired credentials in` imagePullSecrets`


` i/o timeout` /` TLS handshake timeout` Registry unreachable network, firewall, or DNS issue


` toomanyrequests` /` 429` Registry rate limit exceeded


` no matching manifest for linux/arm64` Architecture mismatch


Example Events output for a typo in the image tag:


```text
Events:
Type     Reason     Age              From               Message
----     ------     ----             ----               -------
Normal   Scheduled  2m               default-scheduler  Successfully assigned default/api-pod to node-1
Normal   Pulling    90s (x3 over 2m) kubelet            Pulling image "myapp:v1.2.3-stabel"
Warning  Failed     89s (x3 over 2m) kubelet            Failed to pull image "myapp:v1.2.3-stabel": rpc error: ... not found
Warning  Failed     89s (x3 over 2m) kubelet            Error: ErrImagePull
Normal   BackOff    75s (x3 over 2m) kubelet            Back-off pulling image "myapp:v1.2.3-stabel"
Warning  Failed     75s (x3 over 2m) kubelet            Error: ImagePullBackOff
```


### Step 3: Verify the image reference in your manifest


Check the exact image string your pod is trying to pull:


```text
kubectl get pod <pod-name> -o jsonpath='{.spec.containers[*].image}'
```


Confirm the image and tag actually exist in your registry before making any other changes.


### Step 4: Check imagePullSecrets configuration


```text
kubectl get pod <pod-name> -o jsonpath='{.spec.imagePullSecrets}'
```


Verify the secret exists and hasn’t expired:


```text
kubectl get secret <secret-name> -n <namespace> -o yaml
```


### Step 5: Test registry connectivity from the node (for network issues)


Find which node the pod was scheduled to:


```text
kubectl get pod <pod-name> -o wide
```


SSH into that node and test the registry connection directly:


```text
# Test DNS resolution
nslookup registry.example.com


# Test HTTPS connectivity
curl -I https://registry.example.com/v2/
```


If DNS resolution fails or the HTTPS request times out, the problem is network-level not image or credential related.


## How to Fix ImagePullBackOff: All Scenarios


### Fix 1: Correct a wrong image name or tag


Edit your deployment, daemonset, or pod manifest to fix the image reference:


```text
kubectl edit deployment <deployment-name> -n <namespace>
```


Or update via` kubectl set image` :


```text
kubectl set image deployment/<name> <container-name>=myapp:v1.2.3-stable -n <namespace>
```


If you patched a standalone pod (not managed by a deployment), you’ll need to delete and recreate it:


```text
kubectl delete pod <pod-name>
kubectl apply -f pod.yaml
```


Once you fix the image reference, Kubernetes will retry and the pod should transition to` Running` on the next pull attempt.


### Fix 2: Add or update registry credentials


Create a` docker-registry` secret with your registry credentials:


```text
kubectl create secret docker-registry <secret-name> \
--docker-server=<registry-url> \
--docker-username=<username> \
--docker-password=<password-or-token> \
--docker-email=<email> \
-n <namespace>
```


For AWS ECR specifically, the token expires every 12 hours. A common fix is to use a cron job or the[imagepullsecret-patcher](https://github.com/titansoft-pte-ltd/imagepullsecret-patcher) tool to automatically rotate the secret.


Then reference the secret in your pod spec or attach it to your service account:


```text
# In your pod spec
spec:
imagePullSecrets:
- name: <secret-name>
containers:
- name: myapp
image: registry.example.com/myapp:v1.2.3
```


Or attach to a service account so all pods using that service account automatically inherit the credentials:


```text
kubectl patch serviceaccount default \
-p '{"imagePullSecrets": [{"name": "<secret-name>"}]}' \
-n <namespace>
```


### Fix 3: Resolve network connectivity issues


Network-level ImagePullBackOff requires diagnosing where the connectivity breaks:


- **If DNS fails:** check CoreDNS health (` kubectl get pods -n kube-system` ) and verify your node’s` /etc/resolv.conf` points to the cluster DNS.
- **If firewall is blocking:** ensure outbound HTTPS (port 443) is allowed from worker nodes to the registry endpoint. For AWS, check security group egress rules.
- **If the registry is down:** confirm via its status page. Once the registry recovers, Kubernetes retries automatically.
- **For private registries with self-signed certs:** configure the container runtime (containerd or Docker) to trust the certificate, or add the cert to the node’s trusted CA bundle.


### Fix 4: Handle Docker Hub rate limits


The most sustainable fix is to authenticate all Docker Hub pulls, which raises the limit to 200 pulls per 6 hours per account (free) or unlimited (paid):


```text
kubectl create secret docker-registry dockerhub-creds \
--docker-server=https://index.docker.io/v1/ \
--docker-username=<dockerhub-username> \
--docker-password=<dockerhub-token> \
-n <namespace>
```


For high-volume production clusters, set up a **pull-through registry cache** (using Harbor, Nexus, or a cloud provider’s private registry) so images are cached internally and Docker Hub is only hit once per unique image.


You can also reduce pull stampedes during rolling deployments by tuning` maxSurge` and` maxUnavailable` in your deployment strategy:


```text
strategy:
type: RollingUpdate
rollingUpdate:
maxSurge: 1
maxUnavailable: 0
```


### Fix 5: Fix an architecture mismatch


The correct fix is to build a **multi-arch image** using Docker Buildx that includes layers for both` linux/amd64` and` linux/arm64` :


```text
docker buildx build \
--platform linux/amd64,linux/arm64 \
-t myregistry/myapp:v1.2.3 \
--push .
```


As a temporary workaround, you can constrain the pod to only schedule on nodes with a matching architecture using` nodeSelector` :


```text
spec:
nodeSelector:
kubernetes.io/arch: amd64
```


### Fix 6: Fix ErrImageNeverPull


Change the` imagePullPolicy` in your pod spec from` Never` to` IfNotPresent` or` Always` , or pre-load the image onto the node before scheduling the pod:


```text
containers:
- name: myapp
image: myapp:v1.2.3
imagePullPolicy: IfNotPresent
```


## Prevention: Stop ImagePullBackOff Before It Happens


### Pin image digests instead of tags


Tags are mutable someone can overwrite` :latest` or` :v1.2.3` at any time, and your cluster may get a different (or deleted) image than expected. Image digests are immutable. Pinning by digest guarantees the exact same image bytes on every pull:


```text
image: myapp@sha256:a3b5c7d...
```


Reference digests in production deployments and use tags only in development and CI.


### Validate image references in CI before deploying


Add an image existence check to your CI/CD pipeline before the manifest reaches your cluster. Tools like` crane` (from Google’s go-containerregistry) can validate that an image and tag exist:


```text
crane manifest myregistry/myapp:v1.2.3
```


If the command fails, block the deployment before it ever reaches Kubernetes.


### Use an admission webhook to catch bad image references


A validating admission webhook can reject pod creation attempts where the image reference doesn’t exist in the registry, blocking the problem at the API server level before any pod is scheduled.


### Set up a registry mirror for Docker Hub images


For any production cluster pulling public images, a pull-through registry cache reduces dependency on Docker Hub’s availability and rate limits. Cloud providers offer managed options: AWS ECR Public Gallery, Google Artifact Registry, Azure Container Registry all support mirroring.


### Rotate short-lived credentials automatically


For ECR and other registries with short-lived tokens, automate credential rotation. The most reliable approach is using the cloud provider’s native integration (e.g., IRSA for ECR in EKS) rather than managing tokens manually.


### Set the right imagePullPolicy for your environment


Environment Recommended policy Why


Production` IfNotPresent` Avoids unnecessary pulls; use digest pinning for immutability


Staging / pre-prod` Always` Ensures every deploy gets the latest pushed image


Local dev / air-gapped` Never` Only use images already on the node; requires pre-loading


## ImagePullBackOff Quick Reference


Use this cheatsheet when you’re under pressure and need the fastest path to resolution:


Event message Root cause Fix


` manifest ... not found` Wrong tag or deleted image Fix tag in manifest; push missing image


` no pull access` /` repository does not exist` Wrong image path or insufficient auth Verify repo name; add/fix` imagePullSecrets`


` authorization failed` Invalid or expired credentials Recreate the secret; rotate credentials


` i/o timeout` /` TLS handshake timeout` Registry unreachable Check DNS, firewall, and egress rules on nodes


` toomanyrequests` /` 429` Rate limit hit Authenticate pulls; add registry mirror


` no matching manifest for linux/arm64` Architecture mismatch Build multi-arch image; or use nodeSelector


` ErrImageNeverPull`` imagePullPolicy: Never` + image not cached Change pull policy or pre-load the image


## Monitoring Kubernetes Pod Errors with Middleware


Running` kubectl describe pod` by hand works for a single incident. In a production Kubernetes cluster with dozens of namespaces and hundreds of pods, you need automated detection across every pod lifecycle state not a manual grep workflow per error.


[Middleware is a Kubernetes monitoring platform](https://middleware.io/solutions/kubernetes-monitoring/) built on OpenTelemetry that surfaces ImagePullBackOff, ErrImagePull, CrashLoopBackOff, and OOMKilled events in real time across your entire cluster on EKS, AKS, GKE, or self-managed Kubernetes without separate integrations per cloud provider. Instead of reacting to a failed deployment, you see pull errors as they occur, with the full pod context: which node, which namespace, which image, which registry.


Because Middleware is built on[OpenTelemetry](https://middleware.io/blog/opentelemetry-tools/) , all Kubernetes events, pod states, and node-level metrics flow through a single pipeline. ImagePullBackOff errors are correlated with the surrounding infrastructure state you see immediately whether a spike in pull failures coincides with a node restart, a network policy change, a deployment rollout, or a registry outage. No pivoting between tools, no manual correlation.


If your cluster is also producing CrashLoopBackOff or OOMKilled errors alongside ImagePullBackOff, Middleware surfaces all three in a unified Kubernetes events view so you can triage across pod lifecycle states in one place. This matters because these errors often co-occur: a bad image reference triggers ImagePullBackOff; once fixed, a misconfigured container may immediately produce CrashLoopBackOff. Seeing both in the same timeline eliminates the back-and-forth between kubectl commands and log files.


For teams evaluating Kubernetes monitoring alternatives to Datadog, Middleware delivers full-stack cluster visibility pods, nodes, deployments, namespaces, and infrastructure metrics with a 14-day free trial and unlimited ingestion, versus Datadog’s per-host pricing that scales steeply with cluster size.


With[OpsAI](https://middleware.io/blog/ops-ai-sre-agent/) , Middleware’s AI-powered incident management layer, recurring patterns like rate limit errors or credential expiry are flagged proactively before they trigger a full outage. OpsAI automatically resolves 80% or more of on-call incidents including pod lifecycle failures like ImagePullBackOff by correlating signal across logs, metrics, and Kubernetes events and surfacing root causes in seconds rather than minutes of manual investigation.


Specific capabilities relevant to ImagePullBackOff and Kubernetes pod error monitoring:


- **Pod status monitoring** real-time tracking of pod phase transitions across all namespaces, including ErrImagePull and ImagePullBackOff states, with configurable alerts on threshold breach
- **Kubernetes event streaming** every kubelet event ingested and indexed, so pull failure messages are searchable across your full cluster history without kubectl access
- **Node-level visibility** pull failures correlated with node network health, disk pressure, and resource constraints so you know whether the problem is the image, the registry, or the node
- **Deployment tracking** ImagePullBackOff automatically associated with the deployment revision that introduced the bad image reference, cutting root cause time from minutes to seconds
- **Multi-cloud Kubernetes support** EKS, AKS, GKE, and self-managed clusters monitored from a single dashboard with no per-provider configuration


Start a[14-day free trial with unlimited ingestion](https://app.middleware.io/auth/register/) and get full Kubernetes visibility across all your clusters from day one no per-host pricing, no data caps.


## FAQs


### Does ImagePullBackOff fix itself?


It depends on the cause. If the error is due to a temporary network issue or registry downtime, Kubernetes will retry automatically and the pod may eventually recover without manual intervention. If the cause is a typo in the image name, a deleted tag, or missing credentials, it will never fix itself you need to correct the underlying configuration.


### How long does Kubernetes keep retrying?


Indefinitely, with exponential backoff up to a 5-minute maximum interval between attempts. Kubernetes does not give up and mark the pod as failed due to ImagePullBackOff alone. The pod stays in the ImagePullBackOff state until you delete it, fix the issue, or change the pod spec.


### Will deleting the pod fix ImagePullBackOff?


Deleting and recreating the pod does nothing if you haven’t fixed the underlying issue. It just resets the retry backoff timer, so you’ll see the error again within minutes. Fix the root cause first, then the pod will recover either through automatic retry or by being recreated.


### Can ImagePullBackOff happen with a public image?


Yes. Public images on Docker Hub can trigger ImagePullBackOff due to: a typo in the image name or tag, the image being removed or made private, or Docker Hub rate limits being hit by your cluster’s IP address.


### What is the difference between imagePullPolicy Always and IfNotPresent?


With` Always` , the kubelet contacts the registry on every pod start, even if the image is already cached on the node this means a registry outage can prevent pods from starting even on a node restart. With` IfNotPresent` , the kubelet uses the locally cached image if it exists, making pod starts more resilient to registry availability issues. For production workloads with pinned digests,` IfNotPresent` is generally the safer choice.


### How do I fix ImagePullBackOff in a namespace where I can't edit the pod spec?


If the pod is managed by a Deployment, DaemonSet, or StatefulSet, edit the parent resource instead the pod will be replaced automatically. If you don’t have write access to the namespace, you’ll need to work with whoever manages access to either fix the image reference or add the correct imagePullSecrets.


### Related Kubernetes Troubleshooting Guides


- [Troubleshooting Kubernetes workloads with Middleware](https://middleware.io/blog/kubernetes-workload-troubleshooting/)
- [Understanding and fixing Kubernetes OOMKilled](https://middleware.io/blog/kubernetes-oomkilled-error/)
- [Best OpenTelemetry tools in 2026](https://middleware.io/blog/opentelemetry-tools/)
- [Kubernetes pod crash auto-remediation with OpsAI](https://middleware.io/blog/kubernetes-self-healing-opsai-pod-crash-auto-remediation/)
- [Docker Swarm vs Kubernetes](https://middleware.io/blog/docker-swarm-vs-kubernetes/)
- [AWS ECS vs EKS: What’s the Difference](https://middleware.io/blog/aws-ecs-vs-eks/)
- [How to Analyze AWS CloudWatch Resources](https://middleware.io/blog/analyze-aws-cloudwatch-resources/)
