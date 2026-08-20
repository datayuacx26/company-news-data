---
schema_version: "1.0.0"
document_id: "f37fb8fcd8f8f97b8c87f694c6f8bbba41efdc52132549a254dd5f3750733be8"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-rss-7ea402701b4d"
canonical_url: "https://product.hubspot.com/blog/its-never-a-hardware-bug-until-it-is"
published_at: "2025-09-24T16:38:06+00:00"
first_seen_at: "2026-07-20T03:32:21.515438+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:046dcdf8135d908c6cc0a9ae66443398b3debcecd206644d8adbc83056f0b53e"
---

# It's Never a Hardware Bug, Until it is.

​​


Edit, April 30 2026: The OpenJDK now contains a


[mitigation](https://bugs.openjdk.org/browse/JDK-8370947) for the hardware bug described in this article, which will appear in OpenJDK 27. Thank you to


[Evgeny Astigeevich](https://github.com/eastig) at AWS for his work on the mitigation.


Most software developers have heard the maxim, “it’s never a compiler bug,” and its companion, “it’s never a hardware bug.” But if you investigate enough bugs, you’ll eventually find an exception to this rule, and that’s what we found recently at HubSpot. I discovered how a hardware bug was affecting our users’ experience.


A core part of HubSpot’s software offering is its CRM, which most of HubSpot’s other features are built on top of. The CRM stores contact information about HubSpot’s customers’ customers. This is a lot of data, and we need to store it somewhere that can handle that large data set reliably. Since 2012, the HubSpot CRM has been housed in HBase. HBase is a database designed for storing large amounts of data, and providing fast random access to it. Like most of our software at HubSpot, HBase is written in Java.


On August 1st, 2025, a technical lead on HubSpot’s CRM software team noticed slow-downs occurring occasionally in one of the HBase clusters that store CRM data. He observed that specific servers within the cluster went through periods when they were much busier than usual in certain ways. We track many metrics describing different aspects of an HBase server’s behavior. Through these metrics, he saw that request handler threads spent more time occupied, request latency increased, disk read latency increased, CPU usage increased, and time spent garbage-collecting increased. These changes were significant enough to affect our users’ experience: some users’ page loads would be noticeably slower. However, the differences were not significant enough to trigger our automatic failure monitoring, which is why they went unnoticed by the HBase administration team.


When HBase performance suddenly changes like this, it’s usually because request traffic to the database has changed, and the database is performing differently because of the new workload. I spent weeks combing through HubSpot’s extensive repositories of metrics and logs in an attempt to find some pattern in the HBase cluster’s workload that correlated with the server slowdowns. I found a few leads that I thought were promising: specific applications that were sending potentially expensive requests to the database. I put limits on the rate of requests that HBase would accept from those applications, but this did not make the slowdowns cease. Eventually I gave up looking for a pattern in the request traffic that would stand out to me.


When the issue happened, it tended to last for about an hour, and it wasn’t severe enough to raise alarm bells. For this reason, we usually found out about it after it was over, meaning there were some Java observability techniques that were not usable because they require live access to the problematic JVM. To work around this, I created a detector to identify servers when they experience the issue, and automatically profile their JVMs. After letting this run for a few days, I had many profiles available to me that theoretically caught the problem in action.


Here I’m showing a CPU-time profile of an HBase server in the middle of a mysterious slow-down. I am accustomed to reading flamegraphs of HBase servers, and what stood out to me here was just how ordinary it looked. All of the usual components of an HBase server are visible, with nothing new that’s out-of-the-ordinary. There is an especially large share of CPU time being used by garbage collection, but I knew that already from metrics. I also knew from our allocation-rate metric that there was no spike in allocation rates to explain why garbage collection suddenly got more time-consuming. CPU usage metrics said that the HBase JVM was occupying more CPU cycles than usual, but the flamegraphs said that all the extra CPU cycles were just being eaten up by normal HBase server workload.


One morning, on a whim, I decided it was finally time to upgrade to IntelliJ Ultimate. After doing that, I double-clicked on a server profile in Java Flight Recorder format, intending to look at it in YourKit for the dozenth time, hoping something new would stand out. Unbeknownst to me, IntelliJ was now the default application to open JFR files on my machine, so I was treated to a new view of my JFR file. I’m showing it in dark mode because it’s easier to read.


This revealed a level of detail that I hadn’t seen before. I’ve stitched together two screenshots above to show what I noticed when scrolling through the “timeline” view of my JFR file. The x-axis of this display represents the 10 seconds of wall-clock time during which this profile was taken. The green marks represent each time a sample was taken from a thread. Because this is a CPU-time profile, samples are only taken when a thread is running on a CPU. Therefore, we can infer from the sampling rate of a thread how often that was on the CPU. There are two bursts of samples in the ZWorkerYoung threads, part of the Z garbage collector, showing that those threads were busy doing CPU-bound work during those two periods. This is fine and not interesting on its own. However, what caught my attention is that HBase’s request handler threads experienced a rise, fall, and then rise in activity that mirrored the GC threads. This should not happen. There is no good reason why HBase’s request handlers would be busier when a garbage collection was running.


I looked at many of the individual samples in GC and user (non-GC) threads and found a major commonality.


# Instruction cache invalidation in the JVM


A huge percentage of the samples terminated at __aarch64_sync_cache_range.


This told me that I needed to understand more about what that function did, and why the JVM was calling it. I learned about an internal JVM concept called “nmethods,” short for “native methods,” which is how some code is represented in the JVM at runtime. nmethods are Java methods that have been compiled into machine code. Every nmethod has a boolean state associated with it, marking the nmethod as “armed” or “disarmed.” Some garbage collections transition the state of every nmethod into armed. The next time the nmethod is encountered by JVM internals, or by code needing to run the method, it must be disarmed.


Every nmethod has an “entry barrier,” a bit of code that runs before the method, to ensure that the method has been properly prepared by the runtime and is ready to run. HBase servers at HubSpot use Java 21 with the Z garbage collector, which includes[this bit of code](https://github.com/adoptium/jdk21u/blob/master/src/hotspot/cpu/aarch64/gc/z/zBarrierSetAssembler_aarch64.cpp#L840) in the method entry barrier:


This code is editing the machine code of the nmethod before it will be run in order to ensure it’s updated to conform to the latest state of the garbage collector. The final line ensures that the edited instruction is removed from CPU caches on the machine. All modern CPUs cache instructions in their per-CPU caches. If one edits an instruction in memory, one must consider that multiple cached copies of that instruction may need to be invalidated.


At HubSpot, we use Amazon Web Services servers to run HBase. In particular, we mostly use a mix of the i4i, i4g, is4gen, and i3en instance families. Some of these contain x86-64 CPUs, and some contain arm64 CPUs. The x86-64 architecture automatically keeps CPU caches synchronized with the contents of memory, so ICache::invalidate_word


doesn’t actually need to do anything when run on x86-64. The arm64 architecture does not automatically keep CPU caches synchronized with memory, so on arm64, ICache::invalidate_word


is implemented like so:


__builtin__clear_cache,


a built-in compiler function, delegates to __aarch64_sync_cache_range


inside the compiler-generated implementation. Now we’ve answered the question of why the JVM uses __aarch64_sync_cache_range


: it needs to disarm nmethods before using them.


I understood the context for the stack traces that dominated the CPU profiles during the HBase slowdown incidents: large amounts of time were spent invalidating CPU caches, or handling the resulting cache misses. Next I wanted to know why this happened in short bursts on specific servers. The OpenJDK code hinted that nmethods got armed on major ZGC garbage collections, but not minor collections.


We track the frequency and type of garbage collections in HBase servers, so it was easy to see that there were periods of more frequent major collections, and that these periods correlated with the server slowdowns.


# Instruction cache invalidation on Neoverse N1


I could have stopped at this point in my investigation and concluded that cache invalidations are just slow, therefore we should avoid major collections, but this didn’t sit right with me. Major collections on arm64 can’t universally be severely damaging to application performance, because somebody would have done something about this already, or at least filed a bug report. I wanted to know more about why exactly __aarch64_sync_cache_range


was so damaging to performance. Its implementation is part of the compiler, and my copy of Java was compiled with GCC, so I looked for the[relevant code](https://github.com/gcc-mirror/gcc/blob/6441eb6dc020faae0672ea724dfdb38c6a9bf6a1/libgcc/config/aarch64/sync-cache.c#L32) in GCC. Here I present it in simplified pseudo-assembly:


Now here is a version of it where each instruction, in bold, is translated into plain English, and I’ve added comments:


I used gdb


to check on the value of the ctr_el0


register on my machine, which would influence control flow of __aarch64_sync_cache_range.


I found that bit 28 was 1, and that bit 29 was 0. This meant that __aarch64_sync_cache_range


boiled down to ic ivau, dsb ish,


and isb


in my case. Which of these made it slow? To find out, I wrote a tiny C program like so:


I compiled this on one of our arm64 HBase servers, and then ran it under Linux’s perf


utility. This counted the CPU cycles spent on each instruction in the program. I got this result:


This is a very curious result. 83% of the CPU cycles are spent add


ing two registers together, and storing the result in a register! This instruction isn’t running any more frequently than the ones above and below it, and it’s inherently cheap, so what is going on? I suspected that the preceding instruction, ic ivau,


was doing more than just invalidating a cache line, and that the extra work that was generated was being attributed to the unlucky instruction that happened to come next.


I got this result on an AWS i4g machine, which uses Amazon’s own Graviton2 hardware. On a hunch, I tried my little C program on an i8g machine, which uses the Graviton4 hardware. It ran about 35 times faster, and the add


no longer dominated the cycle counting. I[reported](https://github.com/corretto/corretto-jdk/issues/136) this finding to AWS’s Corretto project, a fork of OpenJDK that is designed to run especially well on AWS’s hardware. I learned from the Corretto team that there are actually more layers underneath the assembly code I’ve shown.


Graviton2 is based on the Neoverse N1 CPU design from Arm. Arm reported a bug in some variants of Neoverse N1. The CPU is designed with automatic data-to-instruction cache coherency, but because of the bug, it doesn’t always actually work. In[this PDF](https://documentation-service.arm.com/static/63ed58d38e13d11599a93743?token=) , the bug is documented as errata number 1542419. Arm’s suggested workaround involves “trapping” the mrs


and ic


instructions. A trap overrides the attempted execution of a certain instruction, and redirects the CPU to instead run some other code. mrs


is trapped into the kernel, so it can return a value with bit 29 set to 0, which fools __aarch64_sync_cache_range


into thinking that the CPU does not provide automatic data-to-instruction cache coherency, thus encouraging it to use ic. ic


is trapped into firmware so that the firmware can “execute a TLB inner-shareable invalidation to an arbitrary address followed by a DSB,” as suggested by Arm. While only Amazon has access to their firmware to know for sure if they are using the suggested workaround, it’s possible to observe traps occurring, again using perf


:


perf


sampled over 32,000 traps, all of which it attributed to the add


instruction. If I give perf


a little grace and suggest that maybe the trap actually happened right before add


, and just got mis-attributed, then this is good evidence that Amazon is trapping our ic


instructions, to preserve the correctness of our instruction cache. Unfortunately, it seems that this workaround is expensive, too expensive for HBase to tolerate.


# Conclusion


It was a long journey, but I finally got to the bottom of the mysterious slowdowns in my HBase servers. At the beginning I never would have guessed that I would be consulting a CPU manual. A lot of things had to come together for the CPU bug to affect HBase:


- We're using a language with a just-in-time compiler, which edits machine code at runtime.
- We're using servers with a CPU architecture with a weak memory model, necessitating that the just-in-time compiler issue cache invalidations.
- The cache invalidations are far more expensive than the JVM authors could have anticipated, because of a workaround installed for a hardware bug.


None of the authors of the individual components could have anticipated how badly they would work when combined together in certain ways. I’m not blaming anybody involved in any layer of the stack. Bugs happen, and sometimes you get unlucky, and the bug has a big effect in a very specific context. That’s what makes software engineering interesting. If there were no more bugs, what fun would that be?


# Mitigation


HBase was the only use case for Graviton2 servers at HubSpot. We’re now migrating our most critical HBase servers away from the i4g and is4gen instance families to work around this bug. The instance families i4i, i7ie, and i8g offer similar compute resources without the same hardware bug.
