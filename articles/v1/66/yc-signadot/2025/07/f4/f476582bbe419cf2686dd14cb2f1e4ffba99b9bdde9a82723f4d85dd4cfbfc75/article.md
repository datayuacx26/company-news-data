---
schema_version: "1.0.0"
document_id: "f476582bbe419cf2686dd14cb2f1e4ffba99b9bdde9a82723f4d85dd4cfbfc75"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-test-temporal-services-using-signadot-sandboxes/"
published_at: "2025-07-22T16:26:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:1cdb8ca8103eaae2d3eeeade239c6decbb4dc9f2185a970e8d718c58f4ab92ea"
---

# How to test Temporal Services using Signadot Sandboxes

*Co-authored by Arjun Iyer and Nimesha Jinarajadasa.*


## Introduction


Welcome to the Temporal + Signadot Sandboxes tutorial! In this guide, you’ll learn how to quickly test new versions of Temporal worker code, either in a pull request or during local development, by leveraging a Temporal setup running in a remote Kubernetes cluster. This setup enables you to:


- Rapidly iterate on worker code without redeploying the entire stack
- Use sandboxes to isolate and test changes alongside a stable baseline
- Observe workflow execution and routing in real time


**Estimated time to complete:** 10-15 minutes


**What you’ll accomplish:**


- Deploy Temporal and Signadot in Kubernetes
- Run baseline client and worker services
- Submit and observe a money transfer workflow
- Deploy a sandboxed worker and route traffic to it
- Understand how context propagation, selective task execution, and routing key propagation into activities work under the hood
- See how the same pattern is built with the Temporal TypeScript SDK


## Prerequisites


Before you begin, ensure you have the following prerequisites set up:


**1) Docker Desktop with Kubernetes Enabled**


- [Download Docker Desktop](https://www.docker.com/products/docker-desktop/) and install it.
- In Docker Desktop settings, enable the Kubernetes option.
- Wait for the cluster to be ready (look for a green indicator).


**2) kubectl Installed**


- [Install kubectl](https://kubernetes.io/docs/tasks/tools/) and ensure it is available in your PATH.
- *Alternative:* You may use[minikube](https://minikube.sigs.k8s.io/docs/) or[kind](https://kind.sigs.k8s.io/) if you prefer.


Set your context to the Docker Desktop Kubernetes cluster:


```text
kubectl   config   use-context   docker-desktop
```


**3) Signadot Account and Operator**


- [Sign up for a free Signadot account](https://www.signadot.com/) .
- Install the Signadot Operator in your cluster by following the[Signadot Operator installation guide](https://www.signadot.com/docs/installation/signadot-operator) .


**4) Signadot CLI (Optional, but recommended for sandbox management)**


[Install the Signadot CLI](https://www.signadot.com/docs/getting-started/installation/signadot-cli) :


```text
brew   tap   signadot/tap
brew   install   signadot-cli
```


## Project Setup


First, clone the[Signadot examples repository](https://github.com/signadot/examples/tree/main/temporal-tutorial) and navigate to the Temporal tutorial directory:


```text
git   clone   https://github.com/signadot/examples.git
cd   examples/temporal-tutorial
```


The example includes two worker implementations: a Python worker (` temporal_worker/` , built on temporalio 1.18.2) and a TypeScript worker (` ts_worker/` , built on @temporalio 1.19.0), both using OpenTelemetry 1.41.x for context propagation. This tutorial walks through the Python worker; the TypeScript worker gets its own section at the end.


Next, deploy the Temporal server components into your cluster:


```text
kubectl   create   namespace   temporal
kubectl   apply   -f   k8s/temporal/
```


## Step 1: Deploy Baseline Client and Worker


Deploy the baseline worker and the client UI:


```text
kubectl   apply   -f   k8s/worker-deployment.yaml
kubectl   apply   -f   k8s/temporal-py-client-ui-deployment.yaml
```


Verify the deployments:


```text
kubectl   get   pods   -n   temporal
kubectl   get   services   -n   temporal
```


You should see something like the following for the pods:


And this for the services:


## Step 2: Submit a Money Transfer Workflow


Open the client UI in your browser. If using a local cluster, port-forward:


```text
kubectl   port-forward   svc/temporal-py-client-ui   8080:8080   -n   temporal
```


Then visit[http://localhost:8080](http://localhost:8080/) , fill out the form, and submit a transfer. You should see a confirmation and workflow ID.


Next, port-forward the Temporal UI:


```text
kubectl   port-forward   svc/temporal-ui-service   8088:80   -n   temporal
```


Then visit[http://localhost:8088](http://localhost:8088/) and search for your workflow ID to inspect its execution.


You should see something like this in the worker logs:


## Step 3: Deploy a Signadot Sandbox Worker


Now fork the worker deployment into a sandbox. The provided sandbox spec (` sandbox/worker-sandbox.yaml` ) forks the baseline worker deployment using the same image, so no code changes are needed:


```text
signadot   sandbox   apply   -f   sandbox/worker-sandbox.yaml   --set   cluster=  <  your-cluste  r  >
```


Once the sandbox is ready, confirm a new worker pod is running in the temporal namespace:


## Step 4: Route a Workflow to the Sandbox


To route requests to the sandbox, use the[Signadot Chrome Extension](https://chrome.google.com/webstore/detail/signadot-sandboxes/) :


- Install the extension and **log in to the Signadot dashboard via the extension.** This will enable the extension to show a list of available Sandboxes.
- In the client UI, select the sandbox you just created using the extension. This will enable requests to be routed to the new sandbox version of the worker.
- Submit a new money transfer.


Watch the Temporal UI and both worker logs:


You may see the task initially picked up by a baseline worker, but it will eventually be processed by the sandbox worker (retries are visible in the Temporal UI).


## Step 5: How It Works (Under the Hood)


### Context Propagation


- The client uses the Temporal Python SDK’s` TracingInterceptor` to propagate OpenTelemetry baggage (including the Signadot routing key,` sd-routing-key` ) into the workflow submission task. The serialized context is stored under the` _tracer-data` key in the workflow’s headers in Temporal’s persistent storage.
- The` _tracer-data` header format is the same across the Python, TypeScript, and Java SDKs, so clients and workers can mix SDKs freely.


### Selective Task Execution


- On the worker side, custom interceptors (see` temporal_worker/signadot/interceptors.py` ) read the routing key from the task headers and consult the Signadot routeserver (with a periodically refreshed cache) to determine if the worker should process the task.
- If the routing key matches the sandbox, the sandbox worker processes the task; otherwise, it is skipped and retried until the correct worker picks it up.
- There are interceptors for both Workflow and Activity tasks.


### Routing Key Propagation into Activities


Accepting or rejecting tasks by routing key is only half the story. Activity code often makes outbound HTTP calls to other services, and those calls must also carry the routing key so Signadot can route them to the right sandboxes downstream.


Here’s the subtle part: the Temporal SDK’s tracing interceptor attaches the OTel context from the task headers around workflow execution, but for activities it only uses that context to parent the activity span. The baggage that carries` sd-routing-key` is not active while your activity code runs. Without extra work, an outbound HTTP call made from an activity carries` traceparent` but no` baggage` , and downstream sandbox routing silently breaks.


The worker’s` SelectiveTaskInterceptor` closes that gap: after the routing check passes, it extracts the context from the` _tracer-data` task header and attaches the baggage to the current OTel context, scoped to each activity execution. The worker auto-instruments aiohttp, so outbound HTTP calls made from activities automatically carry` baggage: sd-routing-key=...` .


The Java SDK has the same behavior, and the same fix applies: bridge the baggage in an` ActivityInboundCallsInterceptor` using` Baggage.makeCurrent()` , scoped to the activity call. See` temporal_worker/README.md` in the example repository for details.


## The TypeScript Worker


The example includes a second worker (` ts_worker/` ) that implements the same routing pattern with the Temporal TypeScript SDK. From a tutorial standpoint, nothing new is required: the TypeScript worker has its own task queue (` money-transfer-ts` ), its own deployment manifest, and its own sandbox spec (` ts-worker-sandbox.yaml` ), and the steps above work the same way. All Temporal SDKs store the OTel context under the same` _tracer-data` header, so workflows started by the Python client are routed and processed correctly by the TypeScript worker, and vice versa.


What makes it worth a closer look is how the routing pattern is implemented, because the Python design does not port directly.


### Why the V8 Isolate Forces a Different Design


The TypeScript SDK runs workflow code, including workflow interceptors, inside a deterministic V8 isolate: no I/O, no Node.js APIs, and no access to the worker process’s memory. That is how the SDK guarantees deterministic replay, but it also means a workflow interceptor cannot query the routeserver or read a routing cache the way the Python interceptor does. The TypeScript worker splits the routing check across the two runtimes instead.


### Workflow Task Routing via a Local Activity


Inside the isolate, a workflow interceptor reads the routing key from the` _tracer-data` header using a pure string parse (no OTel runtime in the isolate). It then delegates the routing decision to a` signadotShouldProcess` local activity. Local activities are the sanctioned escape hatch here: they run on the Node.js side of the same worker process that is executing the workflow task, with full access to that worker’s routeserver-backed cache. A regular activity would be dispatched through the Temporal server and could be picked up by any worker, so the answer to “should I process this?” could come from the wrong worker’s cache.


The local activity’s result is recorded in workflow history as a marker event, which keeps replay deterministic. If the baseline worker later takes over a workflow that a sandbox worker started (for example, after the sandbox is deleted), replay sees the recorded result instead of re-running the check.


When the task is not for this worker, the interceptor throws a plain` Error` . In the TypeScript SDK, an error that is not a` TemporalFailure` fails the workflow task rather than the workflow, so the server retries the task until a matching worker picks it up: the same polling-based distribution the Python worker relies on. Commands from the failed task, including the marker, are discarded, so the next worker runs its own check.


### Header Propagation Without the OTel SDK


The` _tracer-data` header is copied verbatim onto the activities, child workflows, and everything else the workflow schedules, via a custom outbound interceptor. This is deliberately a deterministic payload copy rather than the OTel SDK’s propagator machinery: workflow isolates are reused across workflow runs (` reuseV8Context` ), and the OTel SDK holds mutable module state that would make runs and replays diverge.


### Activity Routing and Baggage Bridging


On the Node.js side, an activity interceptor runs with no determinism constraints. It performs the routing check for activity tasks (local activities are exempt, since they already run on the worker that passed the workflow-level check) and restores the OTel context, including baggage, around activity execution. This closes the same gap the Python worker closes, so outbound HTTP calls made from activities carry the routing key downstream.


## Platform vs. Application Code


All of the Signadot-specific machinery in both workers lives in a platform-owned layer: the` signadot/` package in the Python worker (` worker.py` ,` interceptors.py` ,` routing.py` ) and` src/signadot/` in the TypeScript worker. Application workflows, activities, and models contain no Signadot or OpenTelemetry code at all. The only integration point is constructing the platform’s` SandboxAwareWorker` in the entry point and handing it your workflows and activities.


## Summary


In this tutorial, you learned how to use Temporal and Signadot Sandboxes to rapidly test new worker code in an isolated Kubernetes environment. We demonstrated deploying baseline services, submitting and routing workflows, and leveraging sandboxes for safe, high-fidelity testing without disrupting the baseline environment. Under the hood, interceptors route each task to the right worker version, and OTel baggage bridging keeps the routing key on outbound calls made from activities. The same pattern is implemented in both Python and TypeScript workers, with all Signadot-specific machinery isolated in a platform-owned layer so application code stays untouched. This approach enables faster iteration and more reliable integration testing for microservices.


Ready to go further? Check out more tutorials and resources at[https://www.signadot.com/docs/overview](https://www.signadot.com/docs/overview) .
