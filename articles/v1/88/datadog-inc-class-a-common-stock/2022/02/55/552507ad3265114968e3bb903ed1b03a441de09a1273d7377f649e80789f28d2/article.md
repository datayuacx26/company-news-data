---
schema_version: "1.0.0"
document_id: "552507ad3265114968e3bb903ed1b03a441de09a1273d7377f649e80789f28d2"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/profiling-improvements-in-go-1-18/"
published_at: "2022-02-28T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:9c92e5a8c544d9e54330de0dfeb3a0532b94ab36a110128204c13bfae5a7a11b"
---

# Profiling improvements in Go 1.18

Felix Geisendörfer


Without a doubt, Go 1.18 is shaping up to be one of the most exciting releases since Go 1.0. You’ve probably heard about major features such as generics and fuzzing, but in this post, I’ll focus on profiling and highlight a few noteworthy improvements to look forward to.


A point of personal joy for me is that, as part of my job at Datadog, I was able to contribute to several of the improvements in Go 1.18, which will significantly improve our new functionality for[connecting Go profiling with tracing](https://felixge.de/2022/02/11/connecting-go-profiling-with-tracing/) .


If you’re new to Go profiling, you might also enjoy our[guide to Go profiling](https://github.com/DataDog/go-profiler-notes/blob/main/guide/README.md) before diving into this post.


## Better CPU profiling accuracy


Let’s start with the most significant profiling change in Go 1.18—the CPU profiler for Linux has become a lot more accurate. In particular, Linux no longer underestimates CPU bursts on multi-core systems.


This change goes back to late 2019 (Go 1.13), when[Rhys Hiltner](https://github.com/rhysh) opened[GH 35057](https://github.com/golang/go/issues/35057) to report that he noticed a Go service using 20 CPU cores according to` top` but only 2.4 cores according to Go’s CPU profiler. Digging deeper, Rhys found that this problem was caused by dropped POSIX signals in the kernel.


### Determining the root cause


To understand the problem, it’s worth noting that Go 1.17’s CPU profiler is built on top of the[setitimer(2)](https://man7.org/linux/man-pages/man2/setitimer.2.html) syscall. This allows the Go runtime to ask the Linux kernel to send a notification for every 10 ms the program spends running on a CPU. These notifications are delivered as SIGPROF signals; each delivery of a signal causes the kernel to stop the program and invoke the runtime’s signal handler routine on one of the stopped threads. The signal handler then takes a stack trace of the current goroutine and adds it to the CPU profile. All of this takes less than 10 µs, after which the kernel resumes the execution of the program.


So what was wrong? Well, given Rhys’s program reporting 20 CPU cores being used by` top` , the expected signal rate should have been 2,000 signals per second. However, the resulting profile only contained an average of 240 stack traces per second. Further investigation using[Linux Event Tracing](https://www.kernel.org/doc/html/v5.16/trace/events.html) revealed that the expected amount of` signal:signal_generate` events were observed but not the expected number of` signal:signal_deliver` events. The difference between signal generation and delivery might seem confusing at first, but it’s due to the fact that standard POSIX signals do not queue (see[signal(7)](https://man7.org/linux/man-pages/man7/signal.7.html) ). Only one signal can be pending delivery at a time; if another signal of the same kind is generated while one is already pending, the new signal gets dropped.


However, this still doesn’t explain why so many signals would be generated during the exact same roughly 10-µs signal-handler window so that they would end up on top of each other. The next piece to this puzzle is the fact that the resolution of syscalls measuring CPU time are limited to the kernel’s software clock, which measures time in *jiffies* (see[time(7)](https://man7.org/linux/man-pages/man7/time.7.html) ). The duration of a *jiffy* depends on[kernel configuration](https://github.com/torvalds/linux/blob/master/kernel/Kconfig.hz) , but the default time is 4 ms (250 Hz). What this means is that the kernel isn’t even able to honor Go’s wish of receiving a signal every 10 ms, and instead has to alternate between 8 ms and 12 ms as shown in the histogram below ([details](https://docs.google.com/spreadsheets/d/12zGfTIfHBivcYTrbbets-dDyg_OLmVP5x1VLbQeUezg/edit#gid=1108138742) ).


Since alternating between 8 ms and 12 ms still works out to 10 ms on average, it is not a problem in and of itself. What is a problem, however, is what happens when, for example, 10 threads are running on different CPU cores, causing them to use 40 ms of CPU time within a 4-ms *jiffy* window. When the kernel does its checking, it needs to generate four SIGPROF signals at once, causing all except one to be dropped instead of delivered. In other words, setitimer(2)—the kernel’s portable API advertised for profiling—turns out to be unsuitable for profiling busy multi-core systems.


Given that the man pages explain the *jiffy* limitations of CPU time-related syscalls and the semantics of process-directed signals, it’s not entirely clear if this would be recognized as a bug by the kernel maintainers. But even if setitimer(2) is fixed, it would take a long time for people to upgrade their kernels, so Rhys brought up the idea of using[timer_create(2)](https://man7.org/linux/man-pages/man2/timer_create.2.html) for per-thread accounting of pending signals.


### Collaborating on a fix


This idea didn’t receive feedback from the Go maintainers, so the issue sat dormant until Rhys and I started[discussing it again in May 2021](https://twitter.com/felixge/status/1397522130904965120) . We decided to collaborate, with Rhys working on the critical[CPU profiler changes](https://go-review.googlesource.com/c/go/+/324129/21) and me working on[better test coverage](https://go-review.googlesource.com/c/go/+/334769) for cgo and other edge cases shown in the diagrams below.


Another project of mine was to create a standalone C program, called[proftest](https://github.com/felixge/proftest) , to observe the signal generation and delivery behavior of setitimer(2) and timer_create(2). These observations confirmed another[bias issue](https://github.com/golang/go/issues/14434) from 2016: setitimer(2)’s process-directed signals are not fairly distributed among CPU-consuming threads. Instead,[one thread tends](https://twitter.com/felixge/status/1400356284285792262) to receive fewer signals than all other threads. To be fair,[signal(7)](https://man7.org/linux/man-pages/man7/signal.7.html) states that the kernel chooses an arbitrary thread to deliver process-directed signals when multiple threads are eligible. But on the other hand, macOS doesn’t show this kind of bias, and I think that’s the point where I need to stop making apologies on behalf of the Linux kernel.


The good news is that, except for jiffy resolution, timer_create(2) doesn’t appear to suffer from any of the ailments plaguing setitimer(2). Thanks to per-thread signal accounting, it reliably delivers the right number of signals and shows no bias toward any particular threads. The only issue is that timer_create(2) requires the CPU profiler to be aware of all threads. This is easy for threads created by the Go runtime but gets tricky when cgo code is spawning its own threads.


Rhys’s patch deals with this by combining timer_create(2) and setitimer(2). When a signal is received, the signal handler checks the origin of the signal and discards it if it’s not the best signal source available for the current thread. Additionally, the patch also has some[clever bits to deal with avoiding bias against short-lived threads](https://github.com/golang/go/blob/cb5a598d7f2ebd276686403d141a97c026d33458/src/runtime/os_linux.go#L630-L651) and is just a great piece of engineering.


Thanks to lots of review and feedback from the Go maintainers, Rhys’s patch fixing[GH 35057](https://github.com/golang/go/issues/35057) and[GH 14434](https://github.com/golang/go/issues/14434) was merged and will ship with Go 1.18. My test cases didn’t make the cut, partially because it’s hard to make non-flaky assertions on profiling data, but mostly because we didn’t want to risk those patches taking upstream’s attention away from the main changes themselves. Nevertheless, this open source community collaboration was one of my favorite computer experiences of 2021.


## Profiler label bug fixes


Another feature seeing improvements in Go 1.18 are[profiler labels](https://github.com/DataDog/go-profiler-notes/blob/main/guide/README.md#cpu-profiler-labels) (a.k.a.,` pprof` labels or tags), which allow users to associate arbitrary key/value pairs with the currently running goroutine, which are inherited by child-goroutines. These labels also end up in CPU and goroutine profiles, allowing you to slice and dice these profiles by label values.


At Datadog, we use profiler labels for[connecting Go profiling with tracing](https://felixge.de/2022/02/11/connecting-go-profiling-with-tracing/) , and as part of working on this feature, I did a lot of testing on them. I observed stack traces in my profiles that should have had labels attached but didn’t, leading me to report the issue as[GH 48577](https://github.com/golang/go/issues/48577) and take a closer look at the root cause.


The problem turned out to be really simple. The CPU profiler was occasionally looking at the wrong goroutine when adding the label and required essentially just a one-line fix:


```text
1   -cpuprof.add(gp, stk[  :n  ])
```


```text
1   +  cpuprof  .  add  (  gp  .  m  .  curg  ,   stk  [:  n  ])
```


This might be a bit confusing at first, since` gp` is the current goroutine and usually points to the same place as` gp.m.curg` (the current goroutine of the thread` gp` is running on). However, the two are not the same when Go has to switch stacks (e.g., during signal handling or resizing the stack of the current goroutine). If the signal arrives at one of these unfortunate moments,` gp` points to a purely internal goroutine that is executing on behalf of the user but lacks the labels belonging to the current stack trace` stk` . This is a common issue and documented in the[runtime/HACKING.md](https://github.com/golang/go/blob/master/src/runtime/HACKING.md#getg-and-getgmcurg) guide.


Considering the simplicity of the problem, I submitted a[one-line patch](https://go-review.googlesource.com/c/go/+/351751/1) for it. However, I was asked by[Michael Pratt](https://github.com/prattmic) to write a test case, the ROI of which I initially doubted—I couldn’t have been more wrong. It was very difficult to write a non-flaky test to demonstrate the issue, and the initial version of the patch had to be reverted because of this. However, the process of implementing and improving the test case helped Michael identify two additional issues related to labels, both of which he ended up fixing:


-


[CL 369741](https://go-review.googlesource.com/c/go/+/369741) fixed an off-by-one error in the encoding of the first batch of` pprof` samples causing a small number of samples to be tagged with the wrong labels.


-


[CL 369983](https://go-review.googlesource.com/c/go/+/369983) fixed an issue causing system goroutines (e.g., GC) to inherit labels when spawned from user goroutines.


With all three problems now fixed, profiler labels will become a lot more accurate in Go 1.18. The biggest impact will be for programs using a lot of cgo, since C calls run on a separate stack with` gp != gp.m.curg` . However, even regular programs experiencing 3–4 percent missing labels in Go 1.17 will benefit from this change and achieve full accuracy.


## Stack trace bug fix


Another profiling bug we discovered at Datadog is[GH 49171](https://github.com/golang/go/issues/49171) . The problem manifested itself in delta allocation profiles with negative allocation counts. Digging deeper, we encountered non-deterministic program counter (pc) symbolization as a root cause. This means that the exact same stack trace (same program counters) would contain different symbols (e.g., filenames) in profiles taken at a different time, which should be impossible.


The bug was also very hard to reproduce in a standalone fashion, and I spent almost a week before succeeding and reporting it upstream. In the end, it turned out to be a compiler regression involving inlined closures in Go 1.17 and was[fixed](https://go-review.googlesource.com/c/go/+/366494/) by[Than McIntosh](https://github.com/thanm) .


## Epilogue


As you can see, in addition to being packed with exciting new features, Go 1.18 also contains several important profiling improvements. Improved CPU profiler accuracy and important bug fixes for profiler labels and stack traces promise to have an immediate impact in this latest update. Please let me know if I missed anything or if you have any questions or feedback.


Thank you for reading, and thanks to my colleague[Nick Ripley](https://github.com/nsrip-dd) for reviewing this post.


-
-
-
