---
schema_version: "1.0.0"
document_id: "8ec2aebc3ea3536828fbeb0f8fe7b5f00e82071c4d7d1f1c67672f0e664fb1ae"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods/"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:07164d3640f33cf8fecda095e3379e4b4ad15228da9a5b6edaa243080e96494b"
---

# What was killing our healthy Kubernetes pods

Last week one of our web pods restarted 11 times in 24 hours, and another restarted 9 times. The app worked, users were happy, and latency was fine. Our application logs had nothing to say about it, just gaps where a process used to be.


We spent a couple of days chasing the wrong thing. Then we found a Kubernetes default we’d never thought to override, and the crash loop stopped.


Here’s the debug, in roughly the order it happened.


## The first clue: silence


Pods restart sometimes and we’re used to that, but the weird part was how quiet these restarts were.


Every Node.js service we run has a` SIGTERM` handler that drains connections, flushes a few in-flight jobs, and writes a tidy goodbye line to the logs. None of those lines were there. The process was just gone, and a fresh one was in its place.


That ruled out a clean shutdown, and it also ruled out almost any kind of self-imposed exit. An unhandled rejection writes a stack trace, and a graceful shutdown writes a log line. Both would be in BetterStack, and there was nothing.


So whatever killed the pod didn’t let it run any more JavaScript on the way out.


## The convenient theory


The convenient theory was OOM. Node.js services hit the cgroup limit, the kernel reaps the process with SIGKILL, the dashboards spike, the pod restarts. We’ve seen it before, and it’s the first thing anyone reaches for.


We looked, and container memory stayed comfortably below the limit on every restart we examined. The in-app memory watchdog (which logs and shuts down at 80% RSS) hadn’t fired on any of the dead pods, the heap was healthy, and native memory looked normal.


It was a tidy theory, and it was wrong.


## The exit code that didn’t match the story


When OOM stopped fitting, we walked back to` kubectl` and looked at what it actually said about the dead containers.


```text
Last State:     Terminated
Reason:       Error
Exit Code:    137
```


Exit code 137 is` 128 + 9` , which is SIGKILL, so the kernel was definitely killing the process. That part of the OOM theory held, but the` Reason` field didn’t. When the OOM killer fires, Kubernetes reports` Reason: OOMKilled` , and we were seeing` Reason: Error` . So something else inside the cluster was signalling these pods to die.


The list of things that can SIGKILL a container from outside the process is short. Node selection pressure (no, no evictions in the events). A draining node (no, the nodes were stable). An admission controller (no, none configured for this). Or kubelet itself, in response to a failing probe.


That last one was a thread we hadn’t pulled on yet.


## The one pod that lived


Before pulling it, we noticed something we’d been overlooking. We had several web pods running the same image, with the same env, on similar nodes. Most of them were crash-looping, but one wasn’t, and that one pod had 0 restarts over the same 24 hours.


We compared metrics side by side. CPU, memory, and request volume were all similar. The only thing meaningfully different was the event-loop lag chart. Every crashing pod had multiple multi-second event-loop stalls per day (the longest we caught was around 8.9 seconds), and the healthy pod had no critical stalls at all.


So the pods that froze for several seconds were the ones being killed, and the pods that didn’t freeze were fine.


This is the part where you stop having theories and start having a suspect.


## The kubelet event we should have looked at sooner


` kubectl describe pod` on a crash-looping pod had been telling us the answer the whole time, in 2 lines we’d glossed over because they looked like noise:


```text
Warning  Unhealthy  worker-...  Liveness probe failed: Get "http://...:9091/": context deadline exceeded
Normal   Killing    worker-...  Container ... failed liveness probe, will be restarted
```


` context deadline exceeded` on a liveness probe is kubelet’s way of saying the HTTP request to` /health` didn’t return in time. We’d written it off as a broken` /health` handler and spent an hour proving the handler was fine.


What we’d failed to ask was: how much time, exactly, does kubelet give it?


## A default that shouldn’t be a default


The answer is in[the Kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/#configure-probes) , but it’s easy to miss because it’s in the table of defaults rather than the prose:


- ` timeoutSeconds` defaults to **1** .
- ` failureThreshold` defaults to 3.
- ` periodSeconds` defaults to 10.


Translated: kubelet calls` /health` , waits up to 1 second for a response, and if it doesn’t get one in that window the probe is failed. 3 failures in a row at a 10-second cadence, so roughly 21 to 30 seconds of unresponsiveness, and the container gets a SIGKILL and a restart.


That’s a perfectly reasonable default if` /health` runs on a dedicated thread, or a separate process, or any kind of isolated worker that can’t be blocked by application work. It’s a terrible default for Node.js, where` /health` runs on the same event loop as everything else.


Our` /health` handler is a single` res.send('ok')` call, which is the fastest endpoint we have. But “fastest endpoint” assumes the event loop is currently running. When the event loop is paused, mid-GC or mid-synchronous-work, every endpoint takes as long as the pause, including` /health` .


So a 5-second GC pause didn’t just make a request slow, it made` /health` slow, and 3 of those in a row got kubelet to kill the pod. The blocked event loop meant the` SIGTERM` handler that would have logged the kill couldn’t run, so the process exited silently, the restart count incremented, and we got another empty gap in the logs.


So the whole crash loop boiled down to kubelet correctly enforcing a probe timeout set too tight for the runtime underneath it, with no bugs, no OOMs, and no actually-unhealthy pods involved.


## Why the fix is what the fix is


Raising` timeoutSeconds` is the obvious move. We did that, but only after settling on a strong opinion about probes.


The opinion: **liveness and readiness probes are different things and should be tuned differently** .


- **Readiness** controls whether the pod receives traffic. When readiness fails, Kubernetes removes the pod from the service endpoints. When readiness passes again, it puts the pod back. Failing readiness is recoverable. There’s no real cost to being strict about it.
- **Liveness** controls whether the pod gets to keep running. When liveness fails, kubelet SIGKILLs the container. There’s no recovery. The process loses its connection pool, its in-flight work, anything in memory, and (because the event loop is blocked) its chance to run a` SIGTERM` handler that might have done any of that gracefully.


So readiness should fail fast on the smallest sign of trouble. Liveness should only fail when the process is genuinely wedged, ideally not when it’s mid-GC.


The new liveness config for both web and worker:


```text
livenessProbe  :
httpGet  :
path  :   /health
port  :   http
initialDelaySeconds  :   0
periodSeconds  :   10
timeoutSeconds  :   5
failureThreshold  :   6
```


A pod now has to be unresponsive for about 60 seconds before kubelet kills it. Readiness stays at` timeoutSeconds: 3` and` failureThreshold: 3` , so traffic still drains away within ~10 seconds of trouble. Users see a brief failover, and the pod gets a chance to finish its GC, complete its slow job, and serve the next probe.


In production this dropped web restarts from 9-11 per day per pod to effectively 0.


## The memory tuning that came with it


We rolled out 2 related changes alongside the probe fix, aimed at shrinking the stalls themselves so the new tolerance doesn’t have to do all the work.


First, we lowered` --max-old-space-size` from 75% of the container memory limit to 60%. Web went from 2688MB to 2150MB on a 3584MB container. Worker went from 3072MB to 2456MB on a 4096MB container. 2 reasons:


- ` --max-old-space-size` only bounds V8’s old generation. It doesn’t bound native memory used by Buffers, ICU data, worker-thread heaps, or native drivers. Leaving headroom under the cgroup limit reduces the chance of an OOM kill triggered by native allocations the GC can’t help with.
- Smaller heaps mean shorter GC pauses, which are exactly the stalls that were tripping the liveness probe.


A useful side note: on Node 22, dropping the flag entirely is also viable. Recent V8 auto-sizes the heap to roughly 51% of the cgroup limit when the flag is absent. We kept an explicit value to make the budget obvious in the chart.


Second, we made our in-app memory watchdog configurable via` MEMORY_WATCHDOG_THRESHOLD_PERCENT` and raised it from 80% to 90%. The watchdog triggers a graceful shutdown when RSS crosses the threshold. It’s useful as a safety net, but it was firing too eagerly during recoverable native-memory spikes. With the heap ceiling at 60% of the limit, V8 still hard-stops growth well below the watchdog, so the new threshold gives transient spikes more room to resolve before we preemptively cycle the pod.


## Confirming the fix against real kubelet


The change was small, but justifying the rollout took more work. The risk cut the other way too: a too-tolerant liveness probe could mask a genuinely stuck pod and delay a restart that would actually help.


We wrote a small reproduction harness that runs against real kubelet in minikube. It installs 2 identically-built pods, each running the production` /health` handler and the production memory watchdog. Both get the same self-induced event-loop freezes on a schedule. The only difference is the liveness probe.


Deployment Liveness probe Restarts in 2.5 minutes


` victim-oldprobe` 1s default timeout, threshold 3 2 (crash-looping)


` victim-newprobe` 5s timeout, threshold 6 0 (stable)


That was enough to ship, and the harness is also a useful artifact for the next time someone proposes touching probe values.


## What we still want to fix


Tolerant liveness is the right default, but the multi-second event-loop freezes are still a problem on their own. They make every other latency budget worse, even when they no longer cause kills. The follow-up work falls into a few buckets:


- Move CPU-heavy synchronous work (large JSON serialization, certain native driver paths) off the main event loop, either into worker threads or into separate processes.
- Reduce the number of synchronous blocking points in the request path. Most are fixable. Some require rewriting how a library is called.
- Track and alert on event-loop stalls directly, instead of inferring them from probe failures.


None of those are emergencies anymore. The crash loop was the emergency, and the stalls are a contained performance issue now.


## What we took away


A few things we’ll carry into the next stack of services we build.


1. **When a Node.js process dies silently, suspect something outside the process.** A blocked event loop can’t run your` SIGTERM` handler, your` process.on('exit')` , or your` unhandledRejection` handler. If the logs are empty, the cause was probably external.
2. **Always set` timeoutSeconds` and` failureThreshold` explicitly** on Kubernetes probes. The kubelet defaults assume a synchronous, thread-per-request world. They aren’t safe for Node.js out of the box.
3. **Treat liveness and readiness as fundamentally different.** Readiness should be strict and liveness should be tolerant, because failing readiness just drains traffic while failing liveness destroys process state.
4. **Confirm exit codes before you blame memory.** Exit 137 with reason` Error` is a SIGKILL from outside the process, which includes kubelet’s probe killer. OOM kills come with their own` Reason: OOMKilled` .
5. **Reproduce infra changes against real infra.** Mocking kubelet’s probe timing is easy to get wrong. A short minikube manifest gave us a definitive answer in 2.5 minutes.


If you want a tour of how we think about reliability and engineering trade-offs more broadly, we have a longer writeup on[how we sped up tables by 4 to 5x with virtualization](https://www.basedash.com/blog/how-virtualization-increased-our-table-performance-by-500) and another on[reworking our data fetching with React Query](https://www.basedash.com/blog/optimizing-rest-api-calls) . Both are the same shape as this one: a small change that paid off because we measured before and after.


And if you want to see what we actually build with all this,[Basedash](https://www.basedash.com/) is an AI-native BI platform that runs on the stack we’ve been hardening here.
