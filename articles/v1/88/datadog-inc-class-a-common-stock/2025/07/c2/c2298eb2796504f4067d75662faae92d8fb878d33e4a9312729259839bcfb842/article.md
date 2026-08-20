---
schema_version: "1.0.0"
document_id: "c2298eb2796504f4067d75662faae92d8fb878d33e4a9312729259839bcfb842"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/go-memory-regression/"
published_at: "2025-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:ba7e978d120ebbfbd4fceeafd594874eec9c3fa2821219649a8d7938676a8a96"
---

# How we tracked down a Go 1.24 memory regression across hundreds of pods

Nayef Ghattas


When Go 1.24 was released in early 2025, we were eager to roll it out across our services. The headline feature—the new[Swiss Tables](https://go.dev/blog/swisstable) map implementation—promised reduced CPU and memory overhead.


Our story begins while the new version was being rolled out internally. Shortly after deploying it to one of our data-processing services, we noticed an unexpected memory usage increase:


We observed the same pattern, a **~20% increase in memory usage** , across multiple environments before pausing the rollout. To confirm our suspicions, we conducted a bisect in the staging environment, which pointed directly to the Go 1.24 upgrade as the culprit.


But here’s where things got truly puzzling: The increased memory usage wasn’t showing up in Go runtime metrics and live heap profiles, which meant that, from the Go runtime’s perspective, the service wasn’t using more memory. This caught our attention immediately. After all, Go 1.24 was supposed to reduce memory usage thanks to Swiss Tables, not increase it.


In this two-part series, we’ll share how we investigated this surprising increase and how the same release ultimately helped us **reduce** our memory footprint.


In this first post, we’ll walk through how we diagnosed a subtle memory allocator regression introduced by a runtime refactor and how we worked with the Go team to identify and confirm the root cause. In[Part 2: How Go 1.24’s Swiss Tables saved us hundreds of gigabytes](https://www.datadoghq.com/blog/engineering/go-swiss-tables/) , we’ll show how Go 1.24’s new Swiss Tables implementation dramatically reduced the memory usage of a large in-memory map—yielding a net win across our highest traffic services.


## Ruling out major runtime changes in Go 1.24


Before diving deeper, we needed to eliminate the most likely suspects. Go 1.24 introduced a couple of major changes that could potentially impact memory usage, so we systematically tested each one:


-


**Swiss Tables** : This feature was supposed to reduce memory usage, but we needed to verify it wasn’t somehow causing our problem. We created a test build with Swiss Tables disabled by setting the` GOEXPERIMENT=noswissmap`[flag](https://tip.golang.org/doc/go1.24#runtime) . However, **this did not show any improvement in memory usage** .


-


**Spin bit mutex** : This modified the internal implementation of the runtime mutexes. We also tested reverting the spin bit mutex implementation by creating a build with the` GOEXPERIMENT=nospinbitmutex`[flag](https://tip.golang.org/doc/go1.24#runtime) . However, **we still observed the increased memory usage** with this Go experiment flag set.


## Why system metrics disagreed with Go’s runtime accounting


After ruling out the most likely culprits, we decided to deep dive into Go’s runtime metrics to understand what was happening under the hood. These metrics provide valuable insights into Go’s runtime, including internal memory management—heap allocations, garbage collector (GC) cycles, and so on. Since Go 1.16, these metrics are exposed by the[runtime/metrics package](https://pkg.go.dev/runtime/metrics) . For more details on Go runtime memory metrics, check out[Go memory metrics demystified](https://www.datadoghq.com/blog/go-memory-metrics/) .


Despite the visible increase in overall memory usage, Go’s runtime metrics showed almost no change after upgrading to version 1.24:


This contradiction was our first significant clue. If Go’s internal accounting showed stable memory usage, why were system metrics telling a different story? System components—for example, the Linux OOM Killer—will use variants of these system metrics to control resource usage, such as Kubernetes memory limits, so it’s important to understand why we see this discrepancy.


Digging deeper into system-level metrics revealed a substantial increase in **resident set size (RSS)** :


RSS measures **actual physical memory usage** in RAM, while Go’s runtime metrics primarily track **virtual memory** —the address space allocated to the process, which can be larger than physical RAM usage.


## Our theory: More virtual memory committed to physical RAM


Go 1.24 wasn’t requesting additional memory from the system, but something in the new version was causing previously uncommitted virtual memory—memory that was allocated but not yet physically used—to be committed to physical RAM. This would explain why Go’s internal memory accounting remained stable, while system-level RSS measurements showed increased memory consumption.


Was this affecting all memory regions allocated by Go, or just specific ones?


To answer this question, we turned to Linux’s` /proc` filesystem—specifically` /proc/\[pid\]/smaps` , which[provides detailed memory mapping information](https://man7.org/linux/man-pages/man5/proc_pid_smaps.5.html) for each process. This file shows exactly how much virtual memory is allocated and how much physical memory is used across different regions of a process’s address space, giving us the microscopic view we needed.


When examining the` smaps` data from a Go 1.24 process, we found something interesting:


```text
1   c000000000-c054800000 rw-p 00000000 00:00 0 ← Memory region details    2   Size:            1384448 kB ← 1.28 GiB virtual memory allocated    3   KernelPageSize:        4 kB    4   MMUPageSize:           4 kB    5   Rss:             1360000 kB ← 1.26 GiB physical memory used (very close to virtual memory size)
```


Since this particular memory region is mapped relatively early in the address space—close to the executable—and it’s a read/write mapping with ~1.2 GiB virtual memory allocated, we can assume it’s the Go heap.


**Note** : Lénaïc Huard on the Datadog Container Integrations team recently implemented[a change upstream to label memory regions allocated by Go](https://go-review.googlesource.com/c/go/+/646095) , which should make identifying memory regions in` maps` and` smaps` easier!


Comparatively, in Go 1.23, the` smaps` data shows that the Go heap mapping’s RSS is roughly 300 MiB lower than its virtual memory size (` Size` ):


```text
1   c000000000-c057400000 rw-p 00000000 00:00 0 ← Memory region details    2   Size:            1429504 kB ← 1.33 GiB virtual memory allocated    3   KernelPageSize:        4 kB    4   MMUPageSize:           4 kB    5   Rss:             1117232 kB ← 1.04 GiB physical memory used (roughly 300 MiB lower)
```


After going through all the other memory usage, all signs were clear: **the Go heap is the only memory region impacted by the increase in RSS** .


At this point, we were also diving deep into the Go 1.24 changelog, searching for any clues that might explain our observations, such as changes impacting channels or maps besides the Swiss Tables implementation.


One particular change caught our attention: a significant[refactoring of the mallocgc function](https://go-review.googlesource.com/c/go/+/614257) in the Go runtime. This seemed like a promising lead, as changes to memory allocation could certainly impact how virtual memory gets committed to physical RAM.


To recap our findings so far:


-


**Go 1.24 is likely committing more virtual memory to physical RAM** than Go 1.23, causing increased RSS usage, while Go’s internal memory accounting remains stable.


-


**The Go heap is the only memory region affected.**


-


We had a hunch that this might be due to a[mallocgc refactor in the Go runtime](https://go.dev/cl/614257) .


## Pinpointing the root cause with the Go community


Armed with all the preceding knowledge, we started a thread in the Gophers Slack, hoping to get insights from the Go community and runtime team for a quick validation of our findings:


PJ Malloy (thepudds), an active contributor in the Go community, had previously developed[heapbench](https://github.com/thepudds/heapbench/tree/dev-go124-rss-regression) , a GC benchmarking tool that had proven valuable in the past for identifying runtime issues.


To help zero down on the regression, thepudds asked us what data was in the heap for our impacted service. To do that, we looked at live heap profiles, which enable us to get a snapshot of the data present within the Go heap:


As the profile above shows, most of the memory is used by:


-


**Buffered channels (~50%)**


-


**Maps (** **` map\[string\]someStruct`** **) (~20%)**


With that information, thepudds used heapbench to explore different memory allocation patterns, examining factors like:


-


Different data structures, starting with buffered channels and maps—as observed with our data-processing service—as well as slices and more


-


Small allocations ( **≤32 KiB** ) versus large allocations ( **>32 KiB** )


-


Allocations containing pointers versus non-pointer data


## Large pointer-heavy allocations increase RSS in Go 1.24


The results revealed a striking pattern: **Large allocations (over 32 KiB) containing pointers showed dramatically higher RSS usage in Go 1.24 compared to Go 1.23.**


For example, **large channel buffers with pointer-containing structs** used nearly twice as much RSS in Go 1.24. However, similarly sized allocations of simple integers or smaller allocations showed no significant difference between versions.


To continue tracking the root cause, thepudds used the reproducer to perform a` git bisect` that pointed to the[mallocgc refactoring change](https://go.dev/cl/614257) we had suspected. The RSS measurements clearly showed a dramatic increase after this commit. With this compelling evidence, they filed an[issue in the Go repository](https://github.com/golang/go/issues/72991) .


Michael Knyszek from the Go team quickly identified the specific problem—the refactoring of the memory allocator (` mallocgc` ) inadvertently removed an important optimization:


-


Previously, when allocating large objects (>32 KiB) that contain pointers, Go would avoid re-zeroing memory that was freshly obtained from the operating system (since OS-provided memory is already zeroed).


-


The refactoring in Go 1.24 lost this optimization, causing all large objects with pointers to be unconditionally zeroed, even when unnecessary.


This perfectly aligned with our observations—the unnecessary zeroing caused Go to commit more virtual memory pages to physical RAM, **increasing RSS without changing Go’s internal memory accounting** . Our data-processing service uses large channel buffers with pointer-containing structs, which also checks out.


[The fix](https://go.dev/cl/659956) restored the previous optimization while also addressing subtle memory ordering issues to ensure memory safety during garbage collection.


To validate the fix, we built a custom Go version with the fix cherry-picked and confirmed the results:


We reported this back upstream and **the fix will be included in Go 1.25** . You can follow[Go issue #73800](https://github.com/golang/go/issues/73800) for updates on the backport to Go 1.24.


## Rolling out our data-processing service with a better understanding of RSS impact


Returning to our data-processing service, we now have a good understanding of the issue and can predict the impact on RSS during roll outs to new production environments. In the worst-case scenario, RSS usage should roughly match the total virtual memory reported by the Go runtime.


After confirming that we had sufficient headroom within the services’ existing memory limits, we proceeded with deploying the data-processing service across all production environments. As predicted, RSS usage and Go-managed memory converged in lower-traffic environments. However, **we saw a surprising improvement in our highest-traffic environment** :


In our highest-traffic environment, we actually observed a **drop** in memory usage: virtual memory decreased by approximately **1 GiB per pod (~20%)** , and RSS dropped by around **600 MiB per pod (~12%)** . These improvements contrast with the regression seen in lower-traffic environments: **What was driving that improvement? And why didn’t we see the same benefit in lower-traffic environments?**


In[Part 2: How Go 1.24’s Swiss Tables saved us hundreds of gigabytes](https://www.datadoghq.com/blog/engineering/go-swiss-tables) , we’ll dive into the new Swiss Tables map implementation, show how it reduced memory usage for one of our largest in-memory maps, and share the struct-level optimizations that led to even bigger gains across our fleet.


Our investigation helped uncover and validate a memory allocation regression caused by a lost optimization. With the help of the Go community, we traced the issue to a runtime refactor, confirmed the root cause, and tested a fix that will ship in Go 1.25.


If collaborating upstream and solving tough performance issues sounds exciting,[check out our open roles](https://careers.datadoghq.com/all-jobs/?s=Go&parent_department_Engineering%5B0%5D=Engineering) .


-
-
-
