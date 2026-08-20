---
schema_version: "1.0.0"
document_id: "4b65eefe299593e3d62d5035904e2e36512de7c6e2f7aefc805166108109a807"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/moving-a-jobsystem-to-kubernetes/"
published_at: "2021-02-22T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:b5c99c797f2bfa74fcdfd1c8fe131f975c0f6c50dad87fb80fba173f170cffd5"
---

# How we minimized the overhead of Kubernetes in our job system

Lally Singh


Ashwin Venkatesan


If you’re running a lot of machines, you probably know that Kubernetes can offer significant management and scalability benefits. But these benefits aren’t free: Kubernetes and container runtime overhead can be significant. Compounding the issue, a poorly-configured or naively-deployed Kubernetes can lead to underutilization of *all* of your machines. At Datadog, we moved our existing job system to Kubernetes. It took substantially more CPU time than before, yet completed jobs at a 40-50% slower rate.


This post describes how we solved this performance regression. The solution involved some performance experiment design, light performance tuning, and timing analysis to get back to parity. We also answered a key deployment question: what does the per-pod overhead look like?


## Experiment setup


Our initial Kubernetes experiment was mainly about deployment, not about performance. Our job system used a worker-coordinator pattern with coordinators called “parents”, which dispatched work to each node and monitored their progress.


VM Kubernetes


Node Count 100


Instance Types` c5.4xlarge`


Kernel Version` 3.13.0-141`


Parent Processes / Node 16


Children / Parent 5


Percent Jobs Enqueued 75%


Avg CPU Idle 52%


Jobs Completed 12,830


In this deployed setup, we have half as many nodes and half as many worker-parent clusters in Kubernetes. Accordingly, the throughput of roughly a quarter does make sense.


We did notice something strange: lower CPU idle time in Kubernetes. But maybe that matched up with the lower job enqueue rate. There wasn’t much we could do at that stage: both the deployment setup and the workloads were quite different. We couldn’t tell when performance differences were justified by the nodes doing different work. That made performance analysis really hard.


The best experiment setup is to have no known differences at all, so we can treat every difference as part of the problem. The job system team came back with a wonderful setup:


VM Kubernetes


Node Count 3


Instance Types` c5.2xlarge`


Kernel Version` 3.13.0-141`


Parent Processes / Node 12


Children / Parent 10


Percent Jobs Enqueued 100%


Avg CPU Idle 76%


Jobs Completed 998


It was a very close comparison. Both clusters were repeatedly running a simple Python script as their jobs, and we could see what was going on.


In the Kubernetes configuration, each pod had one parent process and its children together. So, the parent process count on a node was the same as the job system pod count on that node. Note that the old kernel version in the VM configuration does not have any CPU mitigations.


## Work metrics


Now that we had comparable work for VMs and Kubernetes, it was time to decide on metrics. Performance metrics are subtle and can be misleading. This makes metric selection critical.


We looked at two key metrics: effort and performance.


### The effort metric


How much effort is each node expending? Compared with the full capacity of the node, this tells us how much more the node can take.


The first effort metric we looked at was CPU load average. Each CPU core has a *run queue* of its tasks that it can run right now. The load average is an exponentially weighted average of the length of that run queue, plus one if the CPU’s actually busy. Without knowing anything else about what it’s running, that’s a reasonable way to tell how busy a machine is.


But on this Kubernetes setup, this metric was misleading. On Kubernetes, you’ll see an increase in load average for processes doing background work: polling the cluster, checking the state of pods, etc. These processes wake up periodically, do a bit of work, and fall back asleep. They don’t actually use the CPU very much.


But when you have a small number of processes doing the real work, the CPU blips from Kubernetes can really bump up the load average. This is a matter of counting the processes versus how much CPU they actually need—a process needing a microsecond of CPU time before it’s done adds 1.0 to the load, just like a process needing hours of CPU before it’s done.


So, we stopped looking at what the CPU was supposed to do (its *run queue* ), and instead, we looked at how much it was working. Or rather, how much it wasn’t working. Idle CPU is a wonderful metric for how much work the CPU is doing. It’s


for all process CPU usage


, telling us how much CPU we have remaining on the machine. It counts CPU time, not processes.


### The performance metric


How well is the system doing its job? This is what we generally optimize for.


Each system eventually has to choose between a latency bias or a throughput bias. How much of one can you sacrifice for the other?


Generally you watch both, but their roles differ. You can optimize for throughput and watch latency for problems. That makes a lot of sense for batch processing, where latency proxies for your work queue length. Or you can optimize for latency and look at throughput as a proxy for used capacity.


The job system team’s concerns are based on throughput. The team’s key measurements are based on how many jobs it can complete in 30 seconds.


## Kubernetes configuration


The primary differences between raw VMs and Kubernetes came in making the latter schedule correctly. The job system’s workers share a parent that coordinates them. The cluster of parent and its attached workers is deployed together, and the set of clusters is scaled to match the work. Each cluster is a Kubernetes pod.


Most of our performance improvements came from tuning the resource requests to make pods schedule better. The goal was to get six pods per node.


Each pod was originally set to` request` a full CPU core.` c5.2xlarge` has 8 cores and 16 GiB of RAM, with 1.5 GiB taken up by Kubernetes, system services, and the kernel. Between the CPU and memory` request` for the pod, only four pods were getting scheduled on a node. So, we made our first adjustments: reducing the memory` request` down to` 500mb` and reducing the CPU down to` 100m` —that’s 100 millicores.


Adjusting the CPU requests mostly got us to six pods per node, but some nodes still only scheduled five.


The memory was set similarly, but was still too high. There are other daemons we run on all pods, in addition to Kubernetes itself. They took up enough memory that the memory` request` value was allowing only five out of the six desired pods on some hosts.


Generally, if we’re trying to size` request` values to get a certain number of pods on a node, we should only choose one resource metric to use. The others should accurately represent how much of each resource the pod needs.


Those adjustments had no negative effect on runtime. The` request` amounts are only used as minimums for scheduling pods to nodes. As long as the pod requests enough resources to operate, they’ll schedule fine. The` limit` amounts actually constrain the pods once they’re running.` limit` values don’t affect pod scheduling.


### Why just one parent in a pod?


Should there be more parent processes per pod? A single parent and its workers make a natural unit for the application, but is that unit too small?


Behind that question was one of overhead. How much overhead are we paying for a pod? If the overhead is high, we should aim to minimize the pod count and shove more stuff in each one. But if it’s low, it’s substantially easier to orchestrate one parent per pod than many.


#### Understanding the pods


We used` pstree` to look at all the processes on a node, and found six` jobsystem` instances (with all their child processes and threads) with lines like:` containerd-shim─┬─tini───/opt/app/bin─┬─3*\[jobsystem - dumm\]` . After some finagling with the output, we figured out which processes belonged to each jobsystem pod. We then started counting overhead.


Per-pod, it’s three containers’ overhead, which looks to be the overhead of` containerd-shim` . We then looked at CPU and memory.


#### CPU overhead per pod


For the CPU, we had a look at` perf sched` :


```text
1          $ sudo perf sched record -- sleep 10    2          $ sudo perf sched latency > k8s-jobsystem-lat-x2.txt
```


For 10 seconds of time on eight CPUs, we have 80 possible CPU seconds usable. After doing some tabulations on the file` k8s-jobsystem-lat-x2.txt` we got:


Process Total CPU Time


TOTAL: 27,914.6580


jobsystem - dumm:(779) 11,823.1570


/opt/app/bin:(52) 5,214.8220


runc:(1939) 2,623.0330


containerd:(195) 1,684.0350


agent:(123) 1,608.1120


psql:(24) 819.7830


trace-agent:(19) 800.7580


containerd-shim:(299) 601.6430


systemd:(3) 516.8040


The CPU was only active for \\~28 seconds out of the 80 seconds available across all cores. In that time, it spent 600ms on the only` containerd-shim` process active. Averaging that out, **it’s 100ms per pod per 10 seconds, and thus 10ms/pod/second.**


#### Memory overhead per pod


Memory overhead for a process is complicated. Processes share memory, and cause the kernel to use more memory (via the page cache) that isn’t directly attributed to a process. We don’t have to worry about the page cache issue for` containerd-shim` , but we do have many instances that share a lot of memory.


So we asked the kernel about them, and found that they were all in the 1-5MB range.


Terminal window


```text
1   $     for     i     in   $(  pgrep     containerd-shim  ) ;   do     echo     -n     $i  ;   sudo     cat     /proc/  $i  /smaps   |   grep     '^Private'   |   awk     'BEGIN{ f=0 } { f += $2; } END { print ": ",  f, " kB" }'  ;   done    2   436:      1404      kB    3   ..     snip     ..    4   23796:      4688      kB    5   24722:      1380      kB    6   26633:      1332      kB    7   26734:      4536      kB    8   ..     snip     ..    9   32355:      1080      kB
```


#### Stuff we didn’t count


10ms/pod/second, and 1-5MB of memory/pod is quite small. But it’s only part of the picture. Duplicate pods will mount the same image, will require a few additional routing table rules, and generally be present in some data structures in Kubernetes processes.


We don’t think that those overheads would be any larger than those measured for` containerd-shim` . They’re also more difficult to measure, and the values we get out will be less accurate. For example, one of our Kubernetes nodes holds ~110 pods, but some of these overheads may be


. Runtime and memory overhead in that class can be hard to separate from the noise with such low values of


.


## Work execution


Secondly, we tried to look at what each machine was doing. Looking at one of these production hosts, we ran` mpstat 1` to see how busy the CPU was. The` %usr` column (equivalent to “User CPU (%)” in the host dashboard) oscillated high and low in a regular cadence.


The CPUs were often doing nothing. Compared to jobsystem baseline data from four months ago in all-VMs, this was quite unusual. Digging through the source, there was a block like this:


```text
1   for   w   in   workers:    2          w.check()    3          time.sleep(w.sleep_delay)
```


` w.check()` checks if the worker is done, and then sleeps. The total time interval between checks on the same worker is` len(workers) * sleep_delay` . Any time after the worker completes, and before it’s checked again, it’s idle. If the ratio of the check interval to average work time is low, this is fine. **But** that wasn’t the case here. Most jobs were completed in three seconds, and each worker was checked once every two seconds. That leaves


seconds of idle time per job, which is an overhead of


of the worker’s CPU time idle. That’s more than one core per instance left idle.


After reducing the delay to check a single worker once a second, the node count went down by 15. There may yet be more tuning work here to improve performance.


## Results


The Kubernetes transition originally had 100% overhead versus raw VMs. We got that overhead down to 38%, with further optimization possible.


Initially the Kubernetes cluster had 200 nodes, replacing a 100 node non-Kubernetes cluster on AWS. After the` requests` and delay adjustments, the node count came down to 138. The AWS cluster used a much older (` 3.13` ) kernel than the Kubernetes cluster (` 5.0` ), and listed no CPU mitigations. Those mitigations, plus Kubernetes overhead is roughly 15-20%, so we expect that further tuning can bring the cluster down to roughly 120 nodes.


## Summary


We ended up with two questions to solve: why is Kubernetes’ node count so much worse, and having answered that, how should our job system map its processes to pods? While experimenting, we also found a simple optimization.


Experiment design is often very involved. After sorting that out, we could answer our questions. The answer for the first question came down to the fact that deployment setup is complicated. We chose a better set of` request` values in the pod specification to fit enough pods on each node.


The second question made us dig into how Kubernetes pods show up in the process tree. We used our friend` perf sched` to get CPU overhead, and went into` /proc/$pid/smaps` to get memory. The results were “very little.” The memory overhead is in the single-digit megabytes, and the CPU overhead is a few more` kubelet` status queries every couple of seconds—about 10ms of CPU time per second.


Finally we noticed that the CPU usage had a sawtooth pattern on each node. The workers spent much of their time idle. We tracked that down to the parent’s interval between worker status checks. Reducing the interval reduced the time between a worker finishing its job and the parent giving it a new one.


Now the service is as easily managed and scaled just like any other Kubernetes service, with all the benefits of a single containerized workflow that operates across cloud providers.


## A look to the future: bin-packing pods


With all this said, we also realized there’s a good chance that we can squeeze out even more efficiency by optimizing the pod specification and node size. But this is its own story, for next time.


If you’re interested in solving challenges like this,[Datadog is hiring](https://careers.datadoghq.com/engineering/) .


-
-
-
