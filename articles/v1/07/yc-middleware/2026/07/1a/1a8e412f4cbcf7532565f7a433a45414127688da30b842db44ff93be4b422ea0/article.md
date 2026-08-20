---
schema_version: "1.0.0"
document_id: "1a8e412f4cbcf7532565f7a433a45414127688da30b842db44ff93be4b422ea0"
company_key: "yc-middleware"
company: "Middleware"
source_id: "yc-middleware-news-import-fe54c9028845"
canonical_url: "https://middleware.io/blog/kubernetes-crashloopbackoff-causes-diagnosis-and-fixes/"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-22T04:20:55.832849+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:f8ad0b10ab532d2542583d979c92fbccd723228fb3ec429f274c5720d8337d6f"
---

# Kubernetes CrashLoopBackOff: Causes, Diagnosis, and Fixes

***Summary** : CrashLoopBackOff is one of the most common and most frustrating Kubernetes errors engineers face in production. It means a container keeps crashing and restarting in a loop, but the error actually causing it is hidden inside logs and exit codes that most guides gloss over. This article covers every root cause in depth, gives you the exact kubectl commands to pinpoint the problem in under two minutes, and provides specific fixes for each scenario from OOMKilled memory issues and broken liveness probes to missing Secrets and wrong entrypoints.*


#### TL;DR


- CrashLoopBackOff means a container in your pod is starting, crashing, and being restarted in a loop. It is a pod state, not a single error.
- Kubernetes applies exponential backoff between each restart attempt: 10s, 20s, 40s, 80s, 160s, capped at 300s (5 minutes), to prevent resource exhaustion.
- The real error is always in the container logs or Events. CrashLoopBackOff itself is only the symptom.
- The six most common causes: application error on startup, misconfigured or missing environment variables/secrets, OOMKilled (memory limit too low), failing liveness probe, missing or unavailable dependency, and incorrect container command or entrypoint.
- Fastest diagnosis path: kubectl describe pod to read Events, then kubectl logs –previous to read the last crash output.
- Unlike ImagePullBackOff, CrashLoopBackOff means the image pulled successfully. The failure happens at runtime, inside the container.


## What Is Kubernetes CrashLoopBackOff?


CrashLoopBackOff is a Kubernetes pod status indicating that one or more containers in the pod are caught in a persistent restart loop: the container starts, fails, and is restarted by the kubelet, then crashes again, over and over. Kubernetes does not give up and mark the pod as permanently failed. Instead it keeps retrying with an increasing delay between each attempt, giving you time to diagnose and fix the problem.


The name breaks down exactly into what is happening: **Crash** (the container exited with a non-zero exit code or was killed), **Loop** (this keeps repeating), **BackOff** (Kubernetes waits progressively longer between each restart attempt).


CrashLoopBackOff is not itself an error. It is a state. The actual error lives inside the container logs or in the pod Events output. This is the most important thing to understand when troubleshooting it: the fix is never to suppress the restarts, it is to find and resolve what is causing the container to exit.


> **Key distinction:**[ImagePullBackOff](https://middleware.io/blog/kubernetes-imagepullbackoff/) means Kubernetes cannot even get the container image. CrashLoopBackOff means the image pulled fine. The failure happens at runtime, after the container starts.


When you see this status, the pod is still scheduled and the kubelet is still actively managing it. No application traffic is served by that pod. In a Deployment, other healthy replicas continue running, but if all replicas hit CrashLoopBackOff simultaneously (common during a bad rollout), the application goes down entirely.


## How the Backoff Mechanism Works


When a container exits unexpectedly, the kubelet does not immediately restart it. It applies an exponential backoff delay that increases with each successive failure:


Restart attempt Delay before next restart


1st failure 10 seconds


2nd failure 20 seconds


3rd failure 40 seconds


4th failure 80 seconds


5th failure 160 seconds


6th+ failure 300 seconds (5 minutes, maximum)


Once the pod has been running successfully for 10 minutes without crashing, the backoff counter resets. If the container crashes again after that, the delay starts over from 10 seconds.


The backoff serves two purposes: it prevents a crashing container from consuming excessive cluster resources through rapid restart cycling, and it gives the underlying cause time to recover before the next attempt. This means that if the issue is transient (a dependency momentarily unavailable, a temporary network partition), CrashLoopBackOff can resolve itself without manual intervention. If the issue is permanent (application code bug, wrong environment variable, memory limit too low), the pod stays in CrashLoopBackOff indefinitely until you fix it.


## Root Causes of CrashLoopBackOff


### 1. Application crashes on startup


The most common cause. The container image starts, the application process begins, and it immediately throws an unhandled exception, fails a startup check, or exits with a non-zero code. This happens when:


- Code has a bug triggered at initialization (null pointer, missing config file, failed database migration)
- The application requires a configuration file that doesn’t exist at the expected path inside the container
- A startup script exits with a non-zero code due to a failed command
- The entrypoint command in the Dockerfile or pod spec is wrong and the process doesn’t exist or exits immediately


Exit code from` kubectl describe pod` will be non-zero (commonly 1, 2, or an application-specific code). The container logs from the crashed instance will contain the actual error message.


### 2. Misconfigured or missing environment variables and secrets


Applications that require environment variables or Kubernetes Secrets at startup will crash if those values are absent, malformed, or pointing to a non-existent Secret or ConfigMap. Common patterns:


- A Secret referenced in` env.valueFrom.secretKeyRef` does not exist in the namespace
- A ConfigMap referenced in` envFrom.configMapRef` was deleted or renamed
- A required environment variable is present but contains an invalid value (wrong database URL format, invalid JSON, missing port number)
- A mounted volume from a Secret or ConfigMap fails to mount because the resource doesn’t exist


In the first two cases, the pod won’t even reach the running state. The Events section will show` Error: secret "mysecret" not found` before the container starts. In the last two cases, the container starts and then crashes when the application reads the bad value.


### 3. OOMKilled: memory limit too low


If a container’s memory usage exceeds the limit set in` resources.limits.memory` , the Linux kernel’s OOM killer terminates the process. Kubernetes records this as an OOMKilled event and restarts the container, which will likely hit the same memory usage again and be killed again, producing CrashLoopBackOff.


The exit code for OOMKilled is **137** (128 + signal 9, SIGKILL). You will also see` OOMKilled: true` in the container state output from` kubectl describe pod` .


### 4. Failing liveness probe


A liveness probe tells Kubernetes whether a container is healthy. If the probe fails, Kubernetes kills and restarts the container, which can trigger CrashLoopBackOff if the underlying condition persists. Common misconfiguration patterns:


- ` initialDelaySeconds` is too short: Kubernetes checks health before the application has finished starting up, kills it, restarts, and repeats
- The probe endpoint path is wrong (404 returns from a valid but wrong URL)
- ` timeoutSeconds` is too low: a slow health check times out even though the app is healthy
- ` failureThreshold` is too low: a single slow response triggers a restart


### 5. Missing or unavailable dependency


Applications that attempt to connect to a database, message queue, external API, or another service at startup will crash immediately if that dependency is unavailable. Common scenarios:


- Database is not yet running when the application pod starts (race condition in deployment ordering)
- Database credentials in the Secret are incorrect
- A required sidecar container (service mesh proxy, secrets injection agent) is not ready before the main container starts
- An external API the application calls at startup returns an error or is unreachable
- A Kubernetes Service the application connects to doesn’t exist or has no endpoints


### 6. Wrong container command or entrypoint


If the` command` or` args` fields in the pod spec override the Dockerfile’s` ENTRYPOINT` or` CMD` with an incorrect value, the process either doesn’t exist (exit code 127: command not found) or exits immediately because it’s not a long-running process. Running a one-shot command like` echo hello` as the container entrypoint will always produce CrashLoopBackOff because the process exits with code 0 immediately after running.


### 7. Insufficient CPU (throttling-induced failures)


If a container’s CPU request is too low and the node is heavily loaded, the container may be CPU-throttled to the point where startup takes longer than the application’s own startup timeout, causing it to kill itself. This is less common than OOMKilled but occurs in resource-constrained clusters.


### 8. Kubernetes node issues


Occasionally CrashLoopBackOff is caused at the infrastructure level: a node with disk pressure can cause containers to be evicted and fail to restart; a node with a corrupted container runtime may fail to start containers reliably. In these cases, the same pod spec will run correctly on another node.


## How to Diagnose CrashLoopBackOff: Step-by-Step


### Step 1: Identify crashing pods


```text
kubectl get pods -n <namespace>
```


Look for pods with` CrashLoopBackOff` in the STATUS column and a` RESTARTS` count above zero:


```text
NAME                        READY   STATUS             RESTARTS   AGE
api-server-7d9f6b-xk2p9    0/1     CrashLoopBackOff   7          18m
worker-5c8d4b-mn7q1         1/1     Running            0          18m
```


### Step 2: Describe the pod and read Events first


```text
kubectl describe pod <pod-name> -n <namespace>
```


Also check the container state block for the last exit code:


```text
Last State:     Terminated
Reason:       Error
Exit Code:    1
Started:      Wed, 24 Jun 2026 10:32:15 +0000
Finished:     Wed, 24 Jun 2026 10:32:16 +0000
```


Exit code interpretation:


Exit code What it usually means


1 Generic application error; check logs


2 Misuse of shell built-in / invalid argument


126 Permission denied; can’t execute the command


127 Command not found; wrong entrypoint


137 OOMKilled (SIGKILL, memory limit exceeded)


139 Segmentation fault


[143](https://middleware.io/blog/exit-code-143/) Graceful termination (SIGTERM); may be a liveness probe kill


### Step 3: Read the crashed container’s logs


Because the container keeps crashing and restarting,` kubectl logs <pod>` will show the current (possibly empty) container. You need the **previous** container’s logs:


```text
kubectl logs <pod-name> -n <namespace> --previous
```


If the pod has multiple containers, specify which one:


```text
kubectl logs <pod-name> -c <container-name> --previous
```


### Step 4: Check cluster-wide events


```text
kubectl get events -n <namespace> --sort-by='.lastTimestamp'
```


### Step 5: For OOMKilled, check resource usage


```text
kubectl top pod <pod-name> -n <namespace>
kubectl get pod <pod-name> -o jsonpath='{.spec.containers[*].resources}'
```


### Step 6: Debug with an interactive shell


If the container crashes before you can exec into it, override the entrypoint temporarily:


```text
kubectl run debug-pod --image=myapp:v2.1.0 --restart=Never
--command -- /bin/sh -c "sleep 3600"


kubectl exec -it debug-pod -- /bin/sh
```


## How to Fix CrashLoopBackOff: All Scenarios


### Fix 1: Application crash on startup


Read the logs from the previous container (` kubectl logs --previous` ) and fix whatever the application is reporting. Common resolutions:


- Fix the code bug or configuration file the application is failing to read
- Ensure the container image contains all files the application expects at startup
- Add proper startup error handling so the application logs a clear error before exiting


### Fix 2: Missing or misconfigured Secrets/ConfigMaps


```text
# Check Secret exists
kubectl get secret <secret-name> -n <namespace>


# Check ConfigMap exists
kubectl get configmap <configmap-name> -n <namespace>


# Check what keys the Secret actually contains
kubectl get secret <secret-name> -o jsonpath='{.data}' | base64 -d
```


To create a missing Secret:


```text
kubectl create secret generic <secret-name>
--from-literal=DATABASE_URL=postgres://user:pass@host:5432/db
-n <namespace>
```


### Fix 3: OOMKilled: increase memory limit


```text
kubectl set resources deployment/<name>
--limits=memory=512Mi
--requests=memory=256Mi
```


Use` kubectl top pod` over several hours under normal load to find actual peak usage, then set the limit to 1.5 to 2x that value as headroom. See our[OOMKilled guide](https://middleware.io/blog/kubernetes-oomkilled-error/) and[exit code 137 guide](https://middleware.io/blog/exit-code-137-in-kubernetes-causes-diagnosis-fixes/) for the full approach to right-sizing memory.


### Fix 4: Fix a misconfigured liveness probe


```text
livenessProbe:
httpGet:
path: /health
port: 8080
initialDelaySeconds: 30    # increase: give the app time to start
periodSeconds: 10
timeoutSeconds: 5           # increase if your health check is slow
failureThreshold: 3         # increase: don't kill on first slow response
```


Use a startup probe instead of relying on` initialDelaySeconds` for slow-starting applications:


```text
startupProbe:
httpGet:
path: /health
port: 8080
failureThreshold: 30        # 30 x 10s = 5 minutes max startup time
periodSeconds: 10
```


### Fix 5: Dependency not available at startup


Add an init container that waits for the dependency before the main container starts:


```text
initContainers:
- name: wait-for-db
image: busybox:1.28
command: ['sh', '-c',
'until nc -z database-service 5432; do echo waiting for database; sleep 2; done']
```


Also verify the Service exists and has endpoints:


```text
kubectl get endpoints <service-name>
```


### Fix 6: Wrong entrypoint or command


```text
kubectl get pod <pod-name>
-o jsonpath='{.spec.containers[*].command} {.spec.containers[*].args}'
```


Exit code 0 (container exits successfully but immediately) also causes CrashLoopBackOff. Kubernetes restarts any container that exits, regardless of exit code, unless` restartPolicy: Never` is set. For batch jobs, use a` Job` resource instead of a Deployment.


### Fix 7: CPU throttling causing startup timeout


```text
resources:
requests:
cpu: "250m"
limits:
cpu: "1000m"
```


For latency-sensitive or startup-critical containers, consider not setting CPU limits while keeping CPU requests. This is a common production configuration for applications with spiky startup CPU needs.


## Prevention: Stop CrashLoopBackOff Before It Happens


### Set accurate resource requests and limits


Profile your application’s actual memory usage in staging under realistic load before setting production limits. Use` kubectl top pod` or a monitoring platform to track usage over time and adjust proactively.


### Use startup probes for slow-starting applications


Never rely on` initialDelaySeconds` for applications with variable startup times. A startup probe with a high` failureThreshold` prevents liveness probes from killing a container that is simply taking longer than usual to start, without disabling liveness checking entirely once the application is running.


### Build resilient startup sequences


Applications should not crash if a dependency is temporarily unavailable at startup. They should retry with backoff. If changing the application is not feasible, use init containers to gate pod startup on dependency availability.


### Validate Secrets and ConfigMaps before deploying


```text
kubectl get secret <required-secret> -n <target-namespace> || exit 1
```


### Use readiness and liveness probes correctly


Probe type Failure action Use for


Startup probe Restarts container if fails past failureThreshold Giving slow-starting apps time to initialize


Liveness probe Restarts container Detecting deadlocks and hung processes


Readiness probe Removes pod from Service endpoints (no restart) Temporarily stopping traffic without killing the pod


Prefer readiness probes over liveness probes for most failure detection. Overly aggressive liveness probes are one of the leading causes of unnecessary CrashLoopBackOff in production clusters.


## CrashLoopBackOff Quick Reference


Symptom / exit code Root cause Fix


Exit code 1, error in logs Application crash on startup Fix the error in logs; check config files inside container


` secret "x" not found` in Events Missing Secret or ConfigMap Create the missing resource; fix key name reference


Exit code 137, OOMKilled in describe Memory limit too low Increase resources.limits.memory


Killed by liveness probe, exit code 143 Probe misconfiguration Increase initialDelaySeconds; add startup probe


Connection refused in logs Dependency unavailable Add init container; fix Service name; fix credentials


Exit code 127 Wrong entrypoint / command not found Fix command or args in pod spec


Exit code 0, immediate exit Non-long-running process as entrypoint Use a Job; fix entrypoint to a long-running process


Works on other nodes, fails on one Node-level issue (disk pressure, runtime corruption) Drain and cordon the node; investigate node logs


## Monitoring Kubernetes Pod Errors with Middleware


Manual` kubectl logs --previous` works for a single incident. In production clusters with dozens of namespaces and hundreds of pods, you need automated detection that catches CrashLoopBackOff before it affects users and surfaces the root cause without kubectl access.


[Middleware is a Kubernetes monitoring platform](https://middleware.io/solutions/kubernetes-monitoring/) built on OpenTelemetry that tracks pod lifecycle states including CrashLoopBackOff, OOMKilled, and ImagePullBackOff in real time across EKS, AKS, GKE, and self-managed clusters from a single dashboard. When a pod enters CrashLoopBackOff, Middleware surfaces the restart count, the exit code, the node, and the namespace immediately. No kubectl required.


Because Middleware is built on OpenTelemetry, the crashed container’s logs, the pod Events, and the surrounding infrastructure metrics (node CPU, memory pressure, disk usage) flow through a single pipeline and are correlated automatically. You see whether a CrashLoopBackOff event coincides with a node resource spike, a deployment rollout, a Secret update, or a configuration change.


For teams evaluating Kubernetes monitoring alternatives to Datadog, Middleware delivers full pod lifecycle visibility including CrashLoopBackOff detection, OOMKilled alerting, and deployment tracking with a 14-day free trial and unlimited ingestion, versus Datadog’s per-host pricing that scales steeply with cluster size.


With[OpsAI](https://middleware.io/blog/ops-ai-sre-agent/) , Middleware’s AI-powered incident management layer, CrashLoopBackOff events are investigated automatically: OpsAI reads the container logs, correlates the exit code with recent infrastructure changes, and surfaces the root cause and recommended fix. This is the same workflow that automatically resolves 80% or more of on-call incidents across beta customers. Instead of waking someone up at 3am to run` kubectl logs --previous` , OpsAI does it and tells you what’s wrong.


Key Middleware capabilities for CrashLoopBackOff monitoring:


- **Real-time pod status tracking** : CrashLoopBackOff, OOMKilled, and ImagePullBackOff states tracked across all namespaces with configurable alert thresholds
- **Crashed container log indexing** : previous container logs captured and indexed automatically so you can search crash output without needing kubectl or cluster access
- **Exit code correlation** : exit codes surfaced alongside logs and infrastructure state so OOMKilled events (137), bad entrypoints (127), and probe kills (143) are immediately identifiable
- **Deployment change tracking** : CrashLoopBackOff events automatically associated with the deployment revision that introduced them, cutting root cause time from minutes to seconds
- **Multi-cloud Kubernetes support** : EKS, AKS, GKE, and self-managed clusters monitored from a single dashboard with no per-provider configuration
- **OpsAI automated root cause analysis** : AI reads the crash context (logs, exit code, infrastructure state) and recommends a fix, reducing MTTR without requiring on-call runbooks


Start a[14-day free trial with unlimited ingestion](https://app.middleware.io/auth/register/) and get full Kubernetes pod lifecycle visibility across all your clusters from day one. No per-host pricing, no data caps.


## FAQs


### What is CrashLoopBackOff in Kubernetes?


CrashLoopBackOff is a pod status in Kubernetes indicating that one or more containers in the pod are repeatedly crashing and being restarted. Kubernetes applies an exponential backoff delay between restart attempts (starting at 10 seconds, capped at 5 minutes) to prevent resource exhaustion. It is a state, not an error. The actual error is in the container logs or pod Events.


### How do I fix CrashLoopBackOff?


Run` kubectl describe pod <name>` and read the Events section and the exit code. Then run` kubectl logs <pod> --previous` to read the crashed container’s output. The error message in the logs tells you the root cause. Common fixes: correct the application configuration, create missing Secrets/ConfigMaps, increase memory limits for OOMKilled errors, fix probe configuration, or add init containers for dependency timing issues.


### What causes CrashLoopBackOff?


The most common causes are: application code crashing on startup, missing or misconfigured environment variables or Secrets, memory limits that are too low (causing OOMKilled), a misconfigured liveness probe that kills a healthy container, a required dependency being unavailable at startup, and an incorrect container entrypoint command.


### Does CrashLoopBackOff fix itself?


It can, if the underlying cause is transient. For example, a dependency that was temporarily unavailable may recover before the next restart attempt. If the cause is permanent (code bug, wrong configuration, memory limit too low), the pod stays in CrashLoopBackOff indefinitely until you make a change.


### How is CrashLoopBackOff different from OOMKilled?


OOMKilled is one specific cause of CrashLoopBackOff. When a container exceeds its memory limit, it is killed with exit code 137 and Kubernetes restarts it, producing CrashLoopBackOff. OOMKilled is always visible as a named cause in` kubectl describe pod` . CrashLoopBackOff is the outer restart loop; OOMKilled is the reason it is happening.


### How is CrashLoopBackOff different from ImagePullBackOff?


ImagePullBackOff occurs when Kubernetes cannot pull the container image from the registry. The container never starts. CrashLoopBackOff occurs after the image has been pulled successfully. The container starts, runs, and then crashes. Both use the same exponential backoff mechanism, but they occur at completely different stages of the pod lifecycle.


### How long does Kubernetes wait before restarting a crashed container?


The first restart happens after 10 seconds. Subsequent delays double each time: 10s, 20s, 40s, 80s, 160s, then a maximum of 300 seconds (5 minutes). The counter resets if the container runs for 10 minutes without crashing.


### Can I stop Kubernetes from restarting the container?


Yes, by setting` restartPolicy: Never` or` restartPolicy: OnFailure` on the pod, or by using a Kubernetes Job instead of a Deployment. However, for production services this is almost never the right fix. The goal is to fix the underlying crash, not suppress the restart mechanism. Suppressing restarts just leaves your pod in a failed state permanently.


### Related Kubernetes Troubleshooting Guides


- [Diagnosing abnormal Kubernetes workload behavior](https://middleware.io/blog/diagnose-kubernetes-workload-issues/)
- [Understanding and fixing Kubernetes OOMKilled](https://middleware.io/blog/kubernetes-oomkilled-error/)
- [Kubernetes pod crash auto-remediation with OpsAI](https://middleware.io/blog/kubernetes-self-healing-opsai-pod-crash-auto-remediation/)
- [Best OpenTelemetry tools in 2026](https://middleware.io/blog/opentelemetry-tools/)
- [Middleware OpsAI: the AI SRE agent for production issues](https://middleware.io/blog/ops-ai-sre-agent/)
- [Top 10 Kubernetes troubleshooting techniques](https://middleware.io/blog/kubernetes-troubleshooting-techniques/)
- [kubectl restart pod: 5 ways to restart Kubernetes pods](https://middleware.io/blog/kubectl-restart-pod/)
- [Kubernetes exit code 143: meaning, causes, and debugging](https://middleware.io/blog/exit-code-143/)
- [Kubernetes common errors and how to fix them](https://middleware.io/blog/kubernetes-common-errors-fix/)
