---
schema_version: "1.0.0"
document_id: "86d7b85030bcd7b028327b6f5d9c88e3417f4ab967d4c40605a96ea01c8449c8"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/ebpf-workload-protection-lessons/"
published_at: "2026-01-07T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:b63a241223f1831ae008481572063c0f0744585aeee31c92dea78936bd0946ae"
---

# Hardening eBPF for runtime security: Lessons from Datadog Workload Protection

Guillaume Fournier


eBPF has opened up new capabilities for observability, networking, and security. But when you run it in production across thousands of environments and kernel versions, things get complicated fast.


At Datadog, we’ve spent the past 5 years building and operating[Workload Protection](https://www.datadoghq.com/product/workload-protection/) , a runtime security product powered by eBPF. In that time, we’ve hit just about every edge case you can imagine: hooks that silently fail, programs that work on one kernel and break on another, and rule logic that looks solid—until it misses something critical in production.


This post distills six lessons we’ve learned the hard way about making eBPF work reliably at scale: getting programs to load, attach, and keep firing across kernels; capturing and enriching data correctly; monitoring and auditing eBPF usage to reduce attack surface; operating alongside other eBPF users on the same host; measuring and controlling performance cost; and shipping changes safely with strong rollout practices.


If you’re building with eBPF—whether you’re just getting started or deploying across fleets—these lessons might save you from a few late nights. In this post, we’ll walk through:


-


Why we chose eBPF for Workload Protection


-


What we evaluated before choosing eBPF


-


Why eBPF stood out


-


Six lessons from running eBPF in production


## Why we chose eBPF for Workload Protection


The goal of Workload Protection is to detect active threats at runtime without compromising performance, system stability, or detection accuracy. While it is widely accepted in the security community that “shifting left” (giving developers better tools to prevent vulnerabilities early) is essential, we also know that no static analysis or scanning tool is perfect.


Zero-days are discovered constantly. Third-party dependency updates are tedious to manage at scale. Even when patches are available, they can take time to roll out, leaving a window where vulnerable services are running in production.


Workload Protection is designed to give security teams peace of mind in two ways. First, it allows teams to deploy detection rules and monitor known-vulnerable workloads closely until patches are deployed. Second, it continuously monitors all workloads, so even zero days—vulnerabilities unknown to developers—can be caught and mitigated quickly during incident response.


## What we evaluated before choosing eBPF


To meet these goals, we evaluated nearly every monitoring capability the Linux kernel had to offer. Some of the options we considered included:


-


**Linux kernel modules** : These allow deep instrumentation with almost no restrictions. You can hook or even replace nearly any kernel function. But they come with risks—modifying the kernel directly is invasive, and infrastructure teams are understandably hesitant to run custom modules in production.


-


**Traditional tracing interfaces** : Tools like` inotify` ,` fanotify` ,` kprobes` , tracepoints, and perf events provide detailed visibility without injecting new code into the kernel. Used together, they offer a reasonably holistic view of system activity.


-


**` ptrace`** **and** **` seccomp-bpf`** : When focused solely on user space, attaching to processes with` ptrace` (optionally combined with a seccomp profile) offers full visibility into what a process is accessing.


-


**The Linux Audit framework** : This well-known solution is used by many security tools today, including[go-audit](https://github.com/slackhq/go-audit) . It provides configurable event streams for process execution, file access, and network activity.


-


**Other options** : We also considered lesser-known interfaces like Netlink,` LD_PRELOAD` , and` binfmt_misc` , each with their own tradeoffs in terms of reliability, visibility, and system impact.


After evaluating all of these, we chose to build on eBPF. More than 5 years later, that decision still holds up, and here’s why.


## Why eBPF stood out


eBPF offers a unique combination of performance, flexibility, and safety that few other kernel technologies can match.


First, before accepting an eBPF program, the Linux kernel performs an in-depth static analysis of its bytecode to ensure that it won’t compromise the system—catching things like infinite loops, or unsafe memory writes. This safety check is a major benefit over kernel modules, since security and infrastructure teams are (rightfully) cautious about altering the kernel in production.


Although eBPF can cause harm or slow down a machine (subtle foreshadowing), its performance impact is generally lower than that of solutions like the Linux Audit framework or` ptrace` .


eBPF is also a “multiple birds with one stone” type of technology. Unlike tracing tools like` inotify` ,` fanotify` , or Netlink, which need to be combined to get full visibility into processes, file systems, and network activity, eBPF gives us all three through a single, unified mechanism. That versatility has been a key strength for our agent.


Another major advantage is that eBPF provides consistent visibility across namespaces, cgroups, and containers. Compared to techniques like` LD_PRELOAD` or Netlink-based approaches, it’s easier to set up and works reliably across a wide range of Linux distributions, especially since[CO-RE (Compile Once – Run Everywhere)](https://nakryiko.com/posts/bpf-core-reference-guide/) was introduced.


Finally, eBPF lets us implement mandatory access controls via BPF LSM program types. Apart from kernel modules and` fanotify` , no other Linux tracing mechanism offers this kind of enforcement power, which is critical for a security product like Workload Protection.


## Six lessons from running eBPF in production


eBPF remains an incredible piece of technology that has revolutionized networking, performance monitoring, and security in the Linux kernel. The eBPF community is extremely knowledgeable and always learning, sharing, and contributing new instrumentation ideas back upstream.


But eBPF also has its quirks. After building and running a complex eBPF-powered agent that hooks into process scheduling, file system, and networking kernel internals for the past 5 years, we’ve learned many of its lessons the hard way.


Simply put, despite numerous claims that eBPF is safe, secure, and comes with negligible performance impact, the reality—especially at scale—is nuanced. Your experience will depend heavily on how you use eBPF and what kind of workloads you’re monitoring.


To share our story, we’ve distilled our experience into six core lessons. These reflect the problems we’ve hit most often and the patterns we’ve built to avoid hitting them again. Each lesson is grounded in real production failures we’ve encountered while running eBPF at scale, along with the safeguards and engineering strategies we’ve developed to mitigate them.


## 1. Navigate the edge cases of eBPF program loading and kernel hook points


Like any fast-growing Linux kernel subsystem, eBPF’s program requirements and execution model have changed over time. With Workload Protection, we currently support all kernels down to version 4.15 (the[latest version](https://www.kernel.org/) at the time of this writing is 6.18). Over just these past two major releases, eBPF has evolved drastically, and what might seem like a subtle change can have serious consequences for a security product, including full-blown bypasses or degraded detection coverage.


### Pitfall 1: Kernel version and distribution compatibility challenges


An eBPF program may load and attach seamlessly on your development machine, yet fail entirely when deployed to a different kernel version or Linux distribution. These discrepancies are common, and many of them stem from how quickly the kernel and eBPF ecosystem have evolved. Some of the most critical factors we’ve encountered include:


**Program type, helper, and map compatibility** : The capabilities and constraints of eBPF program types, helper functions, and maps have changed significantly over time. For example, starting with[kernel 6.10](https://github.com/torvalds/linux/commit/eb166e522c77699fc19bfa705652327a1e51a117) , it became possible to use` bpf_get_current_pid_tgid` in network tracing programs like TC (traffic control) classifiers and cgroup SKB programs. If your program depends on newer features like these, it will be rejected on older kernels that don’t support them.


**Hook point availability and naming differences** : Hook points that exist in your development environment may be missing or renamed in production. This can be due to differences in compiler options, optimization flags, and distribution-specific kernel patches. Functions might be renamed with suffixes like` isra.*` due to optimizations, or they might be missing altogether as a result of kernel refactors or backports—even when the version numbers appear similar.


**Function inlining inconsistencies** : Kernel configurations and compiler flags can result in different inlining behavior for non-exported functions. If your instrumentation depends on hooking a non-exported function, you may find that the function has been inlined in certain builds, introducing blind spots and reducing observability.


**eBPF verifier sensitivity and evolution** : The eBPF verifier, which ensures that programs are safe to run in kernel space, has undergone substantial changes. Some of the challenges we’ve encountered include:


-


**Map operation restrictions** : Older kernels limit certain map usage patterns. For example, using a map value as a key in another map was not supported[before kernel 4.18](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=d71962f3e627b5941804036755c844fabfb65ff5) .


-


**Instruction count limitations** : The maximum number of instructions allowed per program has gradually increased—from 4,096 in early versions to 1 million[in kernel 5.2](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=c04c0d2b968ac45d6ef020316808ef6c82325a82) . Programs that exceed the limit will be rejected.


-


**Memory bounds checks** : Modern kernels perform stricter and more sophisticated bounds checking. To remain compatible with older versions, you may need to add explicit bounds checks. Even then, using stack variables smaller than 64 bits can lead to flaky verifier failures because the verifier’s analysis relies on 8-byte-aligned “stack slots” and can become confused when multiple smaller variables are packed into the same slot.


-


**Evolving feature conventions** : Even existing features can change. For example, tail calls originally only required that programs in a program array shared the same type. Since[kernel 6.11](https://github.com/torvalds/linux/commit/28ead3eaabc16ecc907cfb71876da028080f6356) , however, the verifier also enforces that the context type (i.e., attached hook point) matches.


-


**Kernel configuration dependencies** : The availability of certain helpers can depend on kernel security settings. For example, when the kernel is running in Lockdown mode, helpers like` bpf_probe_write_user` and` bpf_probe_read` may be disabled entirely.


-


**Dead code elimination** : Beginning with[kernel 4.15](https://github.com/torvalds/linux/commit/c131187db2d3fa2f8bf32fdf4e9a4ef805168467) , dead code elimination allows program logic to adapt at load time by patching constants before the program is handed off to the kernel. If your code relies on this feature and must run on earlier kernels, you’ll have to implement alternative logic paths.


The list above isn’t exhaustive—for example, we haven’t touched on bounded loops or the use of global variables in the` .rodata` (read-only) ELF section—but it illustrates just how many subtle, version-dependent factors can prevent an eBPF program from loading or attaching, even when it works perfectly in a controlled environment.


### Consequences


The primary consequence of these issues is straightforward but severe: Your eBPF program may be rejected at the verifier stage or during attachment. That means lost telemetry, incomplete monitoring coverage, and potentially undetected attacks or performance regressions.


### How Datadog handles it


We’ve developed several strategies to mitigate these issues effectively:


**Comprehensive testing matrix** : We maintain an extensive matrix of kernel versions and Linux distributions in our CI pipelines. We don’t consider a kernel version or distribution supported unless it is actively tested in CI.


**Centralized expertise and shared libraries** : We share knowledge across teams and centralize our eBPF logic by consolidating all eBPF-based products onto a single,[open source library: ebpf-manager](https://github.com/DataDog/ebpf-manager) . This Golang library abstracts much of the program lifecycle management to reduce error-proneness.


**Strict runtime safety checks** : One key feature in` ebpf-manager` is the ability to define a minimal required set of eBPF programs that must load and attach before critical features, such as Workload Protection, are allowed to start. If this minimum isn’t met, the product refuses to start and provides a clear, actionable error explaining why it can’t run on the current kernel.


**Developer-friendly abstractions** : We make extensive use of wrappers and macros within our codebase to abstract away kernel-version-specific verifier quirks. This helps shield developers from low-level compatibility details, while still providing clear and explicit errors when something doesn’t meet requirements.


### Pitfall 2: Incomplete coverage when hooking at the syscall layer


Hooking at the syscall layer is often one of the most effective ways to comprehensively monitor an application’s behavior and its interactions with the kernel. But this approach comes with a number of subtle—but critical—edge cases that can easily lead to incomplete coverage if not carefully handled. A few important considerations:


**Compatibility syscall functions** : When instrumenting syscalls with kprobes, it is crucial to also attach your programs to the “compatibility” syscall functions. These are used to support 32-bit binaries (x86) running on 64-bit (x86_64) systems. Failing to cover these compatibility layers can result in missed events for legacy or cross-compiled applications.


**Tracepoint selection** : If using tracepoints to hook syscalls, prefer` raw_tracepoints/sys_enter` and` raw_tracepoints/sys_exit` over dedicated per-syscall tracepoints (for example,` tracepoints/syscalls/sys_enter_open` ). The per-syscall tracepoints don’t fire for 32-bit binaries on 64-bit kernels, leading to blind spots in your monitoring.


**Syscall number interpretation** : When using` raw_tracepoints/sys_enter` or` sys_exit` , the syscall number you retrieve may differ depending on whether the originating user-space process is a 32-bit or 64-bit binary. To interpret it correctly, you need to check the[architecture flag](https://elixir.bootlin.com/linux/v6.15.5/source/arch/x86/include/asm/thread_info.h#L225) in the task structure—specifically, fields such as` status` or` flags` in the` thread info` structure, depending on the kernel version. Getting this wrong can cause your logic to silently fail or misclassify calls.


**Handling** **` io_uring`** : The` io_uring` syscall is a flexible interface that allows user space to asynchronously perform a wide range of kernel operations, including file-related syscalls like` openat` . If you’re only monitoring traditional syscall paths, you’ll have significant gaps in visibility.


**New syscall introduction** : Although relatively rare, new syscalls are introduced in newer kernel versions (for example,[openat2 in Linux 5.6](https://man7.org/linux/man-pages/man2/openat2.2.html) ). To maintain full coverage, it is important to continuously monitor kernel development and explicitly add hooks for newly added syscalls that matter to your use case.


**Interpreters and exoteric process execution techniques** : Beyond the classic` execve` syscall, the Linux kernel exposes several lesser-known—and sometimes surprising—ways to execute binaries. These include:


-


**` binfmt_misc`** lets you register custom binary format handlers, allowing arbitrary interpreters to run non-native binaries or script-like files transparently. Attackers can abuse this to execute unexpected payloads.


-


**` call_usermodehelper`** (the` usermod` helper mechanism) is a kernel mechanism used to launch user-space helper programs—for example, during module loading or network configuration). If misconfigured, it can be hijacked to execute arbitrary code.


-


**Cgroup release agents** are user-defined scripts that run when the last process in a cgroup exits. They can be set to arbitrary binaries and triggered stealthily.


-


**Shebang-based Interpreters** (for example,` #!/usr/bin/python` ) cause the kernel to execute the interpreter specified in the script header rather than the script itself. If your monitoring only tracks the script path or` execve` , you’ll miss the actual command being executed.


### Consequences


While the first pitfall focused on missing hook points due to inconsistencies across kernel versions, this one relates to incomplete syscall coverage caused by overlooking certain syscall variants and interfaces. The impact, however, is the same: missing hook points lead to reduced telemetry, blind spots in observability, and potentially open avenues for security bypasses.


### How Datadog handles it


We’ve added extensive helper functions within our` ebpf-manager` library to dynamically identify and attach to all required syscall hook points, rather than relying solely on static symbol names in upstream kernel source code. We also maintain a rigorous testing strategy and continuously track kernel patches and new releases. This ensures we can proactively adapt to changes and maintain robust syscall coverage across kernel versions and system configurations.


### Pitfall 3: Hooks not triggering consistently despite best practices


Even when you’ve avoided the common compatibility pitfalls, selected the right hook points, and ensured coverage across the relevant call paths, your eBPF hooks might still fail to trigger reliably in production. Several low-level factors contribute to this non-deterministic behavior, and they can be especially difficult to diagnose and work around. Some of the main factors we’ve encountered include:


**Program type limitations** : Some eBPF program types come with intrinsic constraints. For example,` kretprobe` has a` maxactive` parameter, which caps the number of concurrent active instances. If too many functions return at once—for example, under heavy load— additional probe activations may be silently dropped.


**Hardware interrupts and re-entrancy constraints** : Hardware interrupts can interfere with` kprobe` execution on a given CPU. If a` kprobe` is preempted by a hardware interrupt, no additional` kprobe` program will run on that CPU until the first one resumes and completes. This is a safety mechanism that protects against stack overflows and crashes, but it can also cause missing events under high interrupt pressure or low-latency workloads.


**Kernel module lifecycle challenges** : Attaching hooks to functions inside kernel modules introduces extra operational complexity. If a module is unloaded and reloaded—during a system update or reconfiguration—any hooks attached to its functions are lost. Without an explicit mechanism to reattach them, this can lead to silent failures and gaps in telemetry.


### Consequences


When hooks fail to trigger as intended, you risk losing telemetry and observability, leading to incomplete data collection and blind spots in security or performance monitoring. In the context of security products, this may even create potential avenues for evasion or exploitation.


### How Datadog handles it


We’ve implemented several mitigation strategies to reduce the impact of these challenges:


**Careful usage of return probes** : We strive to minimize reliance on function “exit” hooks (like` kretprobe` or` fexit` ) whenever possible. When we do use them, we explicitly increase the` maxactive` parameter as much as is safe to accommodate higher concurrency. We also ensure that exit hooks are only used to enrich security events with supplemental metadata—not for business-critical logic like sending security signals to user space.


**Favoring exported functions** : We avoid hooking non-exported functions and instead prefer those explicitly marked with` EXPORT_SYMBOL(*)` in the kernel. This reduces our dependency on internal implementation details, improves hook stability, and lowers the risk of probes being silently bypassed due to compiler optimizations or inlining.


**Dynamic re-hooking on module lifecycle events** : We actively monitor kernel module loading and unloading activities. When a module is reloaded, we automatically detect it and dynamically re-attach our eBPF programs to the appropriate symbols. This preserves observability without requiring manual intervention or agent restarts.


## 2. Safely capturing and enriching kernel data reliably is harder than it looks


Even when your programs are triggering as expected, you’re not out of the woods. Reading kernel structures, user-space memory pointers, or even network packets can still go wrong. From a security product perspective, failing to properly capture a security-relevant event can be exploited by attackers to evade detection.


### Pitfall 1: Retrieving consistent and reliable data is harder than it looks


Reading consistent and correct data in production environments is far from straightforward. While data access might appear to work reliably in controlled development or test setups, production often exposes edge cases that can compromise both observability and security coverage:


**Kernel structure volatility** : The layout of these structures—including field offsets, sizes, and even the presence of certain fields—can vary significantly between kernel versions and across different build configurations. Fields may be renamed, moved into nested substructures, or excluded entirely based on compilation flags. A prime example is the[task_struct](https://elixir.bootlin.com/linux/v6.15.5/source/include/linux/sched.h#L813) , which has undergone numerous modifications over the years. As a result, fields like` pid` and` tgid` don’t have fixed offsets and can’t be reliably accessed without accounting for those variations. Blindly reading them without a robust offset-handling mechanism can result in garbage data and serious coverage gaps.


**User space memory** : Another frequent pitfall involves accessing user space memory directly from eBPF programs. If the target memory has been paged out—for example, to swap or a backing file—the kernel would typically handle this transparently by triggering a page fault and reloading the data. However, eBPF programs execute with page faults strictly disabled to maintain system safety and performance guarantees—a behavior discussed in detail on the[IOVisor developer mailing list](https://lists.iovisor.org/g/iovisor-dev/topic/accessing_user_memory_and/21386221) . Consequently, if your eBPF program attempts to read a paged-out user-space buffer, the read will fail silently, potentially resulting in missing or incorrect data.


**Time-of-Check to Time-of-Use** : To mitigate the previous issue, you might wait until after the kernel has already copied user-space data into kernel-space memory before reading the content of the user-space pointer. This can be done by reading data only from the exit probe of a syscall. However, this approach introduces a new risk: a Time-of-Check to Time-of-Use (TOCTOU) window between the time the kernel reads the data and the time your eBPF program does. During this gap, an attacker could race to modify the memory content before your inspection, effectively misleading your instrumentation and potentially allowing bypasses.


**Path resolution** : Even if you successfully obtain syscall arguments, reconstructing accurate file paths introduces its own set of challenges. The kernel accepts relative paths (relative to the process’s current working directory) as well as symbolic links. Resolving these paths correctly requires maintaining accurate state about each process’s working directory and properly resolving link targets. While technically feasible, this process is error-prone and highly sensitive to subtle race conditions.


**Reading network packets** : A similar class of issues arises when reading network packets through direct packet access in eBPF programs such as TC classifiers. When a socket buffer (` skb` ) is non-linear, only part of its data resides in the linear section, with additional data stored in fragment buffers. Reading directly from the linear section without first pulling in the necessary data using` bpf_skb_pull_data` can result in accessing empty or uninitialized memory regions, leading to corrupted or partial packet data.


### Consequences


When eBPF programs cannot reliably access accurate data, the direct result is a loss of telemetry and security coverage. This can lead to silent blind spots in observability pipelines and introduce opportunities for attackers to evade detection, undermining the integrity of security products built on top of eBPF.


### How Datadog handles it


We’ve addressed these challenges with a multi-layered strategy:


**Kernel structure handling** : Our preferred approach is to use CO-RE (Compile Once – Run Everywhere), which automatically adjusts structure offsets at program load time. However, because CO-RE support is limited on certain kernel releases and distributions, we maintain fallback mechanisms that rely on runtime offset guessing or, as a last resort, hardcoded offsets derived from kernel-version-specific analysis. These layered fallbacks give us strong kernel support all the way back to version 4.14 (and even earlier for some CentOS releases).


**Extensive testing and self-validation** : As mentioned previously, our CI system includes an extensive matrix of kernel versions and distributions, ensuring that changes are validated in realistic environments. Our agent also runs self-tests at startup and reports results to our backend, providing early warnings of regressions or unsupported configurations and giving customers visibility into their exact state of security coverage.


**Avoiding problematic user-space memory access** : To sidestep the complexities and risks of user-space memory reads and path reconstruction, we resolve paths entirely from kernel data structures. By traversing the kernel’s internal file tree, we reconstruct absolute paths to their mount points, eliminating ambiguities caused by relative paths and symbolic links. In the rare cases where capturing raw user-provided data is unavoidable, we wait until the kernel has copied the content of the user-space pointer and read only that kernel copy.


**Reliable packet inspection** : For deep packet inspection, we ensure that all our TC entry points start with a call to` bpf_skb_pull_data` and we also use` bpf_skb_load_bytes` for good measure.


### Pitfall 2: Maintaining consistent caches in kernel and user space is treacherous


A common technique used in eBPF-based observability and security tools involves aggregating information from multiple kernel hook points, storing intermediate context in eBPF maps, and finally emitting rich events to user space that consolidate this context. When additional metadata or processing is required—for example, for process trees or detailed process attributes—it’s also common to maintain duplicate caches in user space.


While conceptually straightforward, maintaining these caches—both in kernel and user space—introduces numerous pitfalls that can severely compromise the reliability and security of your detections. These issues are particularly insidious because their impact isn’t always immediately evident and can lead to subtle but critical security bypasses.


Here are several cache-related traps we’ve encountered while building secure detections with eBPF:


**eBPF hashmap** : One fundamental building block of eBPF is the eBPF hashmap, introduced in[Linux 3.19](https://github.com/torvalds/linux/commit/0f8e4bd8a1fc8c4185f1630061d0a1f2d197a475) . When using a hashmap—for example, to store process context indexed by PID—you must specify its maximum size upfront. Determining an appropriate size is non-trivial: Sizing it too conservatively limits the number of processes you can track concurrently, while oversizing it risks unnecessary memory consumption and potential resource exhaustion. Improper lifecycle management of map entries can also lead to “entry leaks”—stale entries that prevent tracking new processes—effectively bricking your tool’s monitoring capabilities.


**eBPF LRU hashmap** : To mitigate these risks, many practitioners turn to eBPF’s LRU (Least Recently Used) hashmaps. However, the[LRU implementation in eBPF](https://lpc.events/event/16/contributions/1368/attachments/1102/2113/LPC2022%20-%20Pressure%20feedback%20for%20LRU%20maps.pdf) doesn’t strictly adhere to traditional LRU semantics. In some cases, entries can be evicted even if the map isn’t full—a trade-off made for performance reasons. This can have critical consequences if your detection logic relies on persistent context stored in these maps.


**Preallocation tradeoffs** : Another potential approach to fix the hashmap sizing issue is to set a high maximum entry count while disabling preallocation using the` BPF_F_NO_PREALLOC` flag. Although this allows memory to be allocated on demand, it introduces a new risk: the total memory footprint becomes unbounded and, more importantly, unpredictable because it depends on workload behavior. In environments such as Kubernetes—where strict memory limits are often enforced (especially on sidecars)—this can result in out-of-memory (OOM) kills that affect system stability and availability. Similar issues arise with “object-attached storage” map types—for example,` BPF_MAP_TYPE_INODE_STORAGE` ,` BPF_MAP_TYPE_TASK_STORAGE` —which attach map entries directly to kernel objects that can grow unexpectedly.


**Blocking syscalls** : Another subtle but dangerous trap involves blocking syscalls. Consider the` connect` syscall: If you store context in a map at syscall entry and remove it on exit, an attacker could exploit this by issuing multiple blocking` connect` calls that never return. This causes your maps to accumulate stale entries, consuming resources and possibly allowing the attacker to poison or evade your detection logic. Once a hashmap is full, no new entries can be added until you make space for them. Switching to an LRU map doesn’t fully solve this problem—it introduces additional unpredictability by randomly evicting entries, potentially creating new coverage gaps.


**Lost and out-of-order events** : Transferring data to user space adds another layer of complexity. eBPF tools commonly use perf buffers or ring buffers to stream events from kernel to user space. While these buffers allow multiple kernel writers, only a single CPU core can read from them in user space. Under high system load, this can lead to dropped or out-of-order events. Lost events directly degrade telemetry coverage, while misordered events can cause incorrect context attribution. For example, receiving an` open` event before an` exec` event could result in attributing file access to the wrong process, leading to missed detections or false negatives.


**Cache synchronization** : Finally, correctly indexing and synchronizing caches is critical. Cache keys must be carefully chosen to ensure uniqueness, avoid collisions, and remain robust against namespace-related issues. For example, if a process context cache is indexed solely by PID, a missed` exec` event under load could cause the new process to inherit outdated metadata from its predecessor. Since PIDs can be rapidly reused on busy systems, missing exit events exacerbates the risk of context confusion. Similar challenges apply to file system caches indexed only by inode: hard links share inodes despite different paths, and inode reuse can further undermine consistency. Additional complications arise when dealing with mount namespaces, which require careful resolution to host-level identifiers.


### Consequences


Inconsistent or unreliable caches can create critical blind spots, enabling attackers to bypass detection mechanisms in subtle ways. As detection logic and event-enrichment pipelines grow more sophisticated, the risk of introducing vulnerabilities through subtle cache inconsistencies also increases. Ultimately, while context-rich detections promise deeper insights, they also demand robust cache design to prevent catastrophic security failures.


### How Datadog handles it


We’ve addressed these challenges through a combination of design choices and operational safeguards:


**Extensive observability of internal state** : We instrument our kernel maps, ring buffers, and user-space caches with detailed metrics to monitor their usage and health. This allows us to proactively detect and fix sizing issues, entry leaks, or unexpected memory growth.


**Rigorous map design and lifecycle management** : We carefully choose map types and enforce strict entry-lifecycle policies to prevent potential abuses. Special care is taken to avoid scenarios where map behavior could be exploited for stealthy bypasses. Overall, we avoid non-preallocated maps because we prefer maintaining a predictable memory footprint on a host, rather than requesting more memory when the machine is already under pressure.


**Dynamic in-kernel filtering** : We mitigate event loss by dynamically computing and applying in-kernel filters tailored to the active ruleset and workload behavior. This reduces the volume of events sent to user space, minimizing both coverage loss and user-space CPU and memory pressure. Events discarded in the kernel don’t consume downstream resources, enabling more efficient and resilient processing pipelines.


**Robust cache synchronization mechanisms** : We maintain mechanisms to reconcile discrepancies between kernel and user-space caches. For instance, if an` exec` event is missed, we can detect inconsistencies using metadata such as the inode of the executed binary. When a mismatch is found, we trigger an explicit resynchronization of the process context via` /proc` data. This ensures that even in high-load scenarios or during partial event loss, we can restore accurate context attribution without manual intervention.


**Event reordering** : Instead of streaming events directly from the perf buffer to our processing pipeline in user space, we reorder events at runtime using a sliding window of a few milliseconds. Although not perfect, this approach has significantly improved event-order consistency. Migrating to ring buffers of type` BPF_MAP_TYPE_RINGBUF` has also helped, but since this feature was introduced in[kernel 5.8](https://github.com/torvalds/linux/commit/457f44363a8894135c85b7a9afd2bd8196db24ab) , it’s not always available on every platform we support.


### Pitfall 3: Writing rules can be error prone


Even when security events are consistent and reliable, another source of error shouldn’t be overlooked: Writing good detection rules can be tricky. It’s easy to make mistakes if you don’t have a good understanding of the data you’re working with and how it was captured. Following are three examples of the most common issues we’ve encountered over the past few years:


**Symlinks** : One of the biggest sources of rule-writing errors we’ve seen comes from symbolic links. A symbolic link is an actual file that simply points to another one, while a hard link is a different path that leads to the same file. While writing rules, you need to know whether your monitoring tool resolves links by default and whether the process you’re targeting will use symlinks or hard links. For example, if you write a rule using the soft link path but your underlying tool automatically resolves symlinks, that rule will never trigger. Similarly, if you write a rule for only one of several hard-link paths that lead to the same file, you’ll get only partial coverage.


**Interpreters** : Another common mistake is forgetting about interpreters. Interpreters are called by the kernel to execute a file with a shebang (` #!` ) at the start of the script, such as Python or Bash scripts. For example, you can call` execve(“/tmp/my_python_script.py”)` , and the script will get executed correctly if it starts with a shebang pointing to Python. The problem is that most eBPF-powered monitoring solutions only capture the path to the script as context and ignore the interpreter. Because collecting interpreter data requires deeper kernel hooks, it’s often omitted, but that can allow hidden or unexpected processes to slip through. You should always write rules that consider both the file provided to the` execve` syscall and the interpreter command itself.


**Syscall arguments aren’t shell commands** : It’s also important not to confuse syscall arguments with shell commands. They differ in at least two key ways:


-


First, if an environment variable is used in a command, its value is resolved by the shell before being passed to the syscall. Writing a rule that matches on a dollar-sign expression (for example,` $MYVAR` ) will never match.


-


Second, process binary paths are resolved by the shell using the current` $PATH` environment variable. This means that when you run` curl”` , the kernel captures` /usr/bin/curl` as the path—not just` curl` .


### Consequences


Writing rules is hard, and even small mistakes can severely affect your detection coverage. Incomplete or incorrect rules can be exploited by attackers to avoid detection, even if the underlying monitoring system captures the event correctly.


### How Datadog handles it


With Workload Protection, we’ve implemented several safeguards to make our rule-writing experience more consistent:


**Symlinks** : We always resolve paths, meaning soft links are ignored. You only need to account for hard links; if your target file has multiple hard links, you’ll need to list all possible paths in your rules.


**Interpreters** : We’ve interfaced with the kernel to collect interpreter data (when present), allowing detection engineers to write rules that reference interpreters.[These are available](https://docs.datadoghq.com/security/workload_protection/linux_expressions) for both the current process (` process.interpreter.*` ) and ancestor processes (` process.ancestors.interpreter.*` ).


**CI tests** : We built a dedicated CI system that lets detection engineers test their rules before releasing them to customers.


## 3. eBPF introduces an attack surface that should be monitored and audited


With its numerous program types and deep kernel integration, eBPF has become a prime target for kernel exploits as well as for building rootkits. Because some eBPF programs can be loaded by non-privileged users, the eBPF verifier is the last line of defence protecting the kernel from exploitation attempts (see[CVE-2023-2163](https://bughunters.google.com/blog/6303226026131456/a-deep-dive-into-cve-2023-2163-how-we-found-and-fixed-an-ebpf-linux-kernel-vulnerability) ,[CVE-2024-41003](https://github.com/google/security-research/security/advisories/GHSA-hfqc-63c7-rj9f) , and many others).


### Pitfall 1: eBPF can be abused to build powerful rootkits


In 2021, our team explored using eBPF to implement a full-fledged rootkit as part of a[hackathon project](https://github.com/Gui774ume/ebpfkit) . What started as an experiment quickly turned into a serious proof of concept: We realized that eBPF’s flexibility and deep kernel hooks—typically celebrated for observability and performance—could just as easily be weaponized.


We demonstrated this by creating **ebpfkit** , presented at[Black Hat 2021](https://www.blackhat.com/us-21/briefings/schedule/#with-friends-like-ebpf-who-needs-enemies-23619) and[DEF CON 29](https://defcon.org/html/defcon-29/dc-29-speakers.html#fournier) . It included capabilities such as process hiding, network scanning, data exfiltration, command-and-control channels, and persistence—all built entirely with eBPF.


While mitigations have been introduced since then (for example, blocking` bpf_probe_write_user` when Kernel Lockdown is set to integrity mode, now default on most distributions), eBPF remains a highly privileged and potentially dangerous mechanism if left unrestricted.


### Consequences


Contrary to its reputation as a safe observability tool, eBPF is a powerful and intrusive kernel feature that can be exploited for stealthy malicious activities. This doesn’t mean it shouldn’t be used in production but it does mean eBPF usage must be strictly controlled and continuously audited.


### How Datadog handles it


To keep this attack surface visible and under control in production, we do the following:


**Dedicated** **` bpf`** **event visibility** : We introduced a dedicated` bpf`[event type that captures all BPF-related activity](https://docs.datadoghq.com/security/workload_protection/linux_expressions#event-bpf) , including program loading, map operations, and attachments.


**Inspection of helpers and maps** : We collect details such as which maps and helpers are used by each loaded program. This enables detection rules for suspicious behavior—for example, identifying network programs interacting with maps shared by the file system or process-discovery probes, or flagging the use of helpers like` bpf_override_return` and` bpf_probe_write_user` , which can alter kernel execution paths.


**Hardening against eBPF tampering** : We’ve also invested research time in how attackers might disable or interfere with eBPF-based detections. This led to a[Black Hat 2022 talk](https://www.blackhat.com/us-22/briefings/schedule/#return-to-sender---detecting-kernel-exploits-with-ebpf-27127) proposing strategies to protect eBPF from malicious actors, several of which are now implemented in our agent.


## 4. Kernel resources are shared—account for other eBPF-based tools


New eBPF program types are continually added to the upstream Linux kernel, each introducing new capabilities but also new dependencies on internal kernel resources, resources that may need to be shared with other eBPF-based tools. For example, certain program types attach to cgroups, which are often shared among multiple services and vendors on the same system.[Strict rules](https://elixir.bootlin.com/linux/v6.13.7/source/include/uapi/linux/bpf.h#L1159) govern how cgroup programs are ordered, scheduled, and whether they can override each other. Similar challenges exist for other program types,[such as legacy TC](https://docs.ebpf.io/linux/program-type/BPF_PROG_TYPE_SCHED_CLS/) classifier programs, where conflicts can arise if multiple tools attempt to attach to the same hook points. The risk here is that if your tool—or any third-party vendor’s tool—does not account for other eBPF programs on the host, race conditions and conflicts can appear, reducing visibility, degrading coverage, or even creating full detection bypasses.


### Pitfall 1: Beware of conflicts when multiple eBPF-based tools share kernel resources


When deploying eBPF-powered security solutions, it is easy to forget you may not be the only one instrumenting kernel resources on a host. In 2022, our team encountered a critical incident involving our solution and Cilium, a popular CNI (Container Network Interface) for Kubernetes. Workload Protection uses eBPF TC classifiers (BPF program type` SCHED_CLS` ) to inspect and potentially block network packets at the interface level. Cilium also attaches eBPF programs to control pod networking, using a hardcoded priority (priority 1) and hardcoded handle (` handle: 0:1` , a unique per network classifier identifier). Due to timing and race conditions, our agent sometimes loaded its TC filters before Cilium, unintentionally taking handle` 0:1` under Cilium. When Cilium later loaded its filters and replaced ours, it disrupted our internal resource tracking logic. Our cleanup mechanism—designed to prevent network namespace leaks—interpreted this change as a signal to delete resources, and we inadvertently removed Cilium’s filters, breaking pod connectivity.


This conflict illustrates how critical it is to account for third-party eBPF tools and shared kernel resources. Even when both solutions are independently correct and stable, unexpected interactions can cause severe service disruptions. (You can learn more about this incident and how we debugged it by watching[our presentation at CiliumCon 2023](https://www.youtube.com/watch?v=YK7GyEJdJGo) .)


### Consequences


Conflicting kernel resource usage between eBPF tools can result in severe network outages, loss of connectivity, or unpredictable failures in production. In our case, affected pods could lose connectivity entirely or suffer degraded service until a manual restart.


### How Datadog handles it


After debugging and resolving the incident, we implemented mitigations in our agent to prevent it from happening again:


**Safer defaults for TC priorities** : We use a higher default priority (priority 10) so infrastructure-related classifiers can run first.


**More conservative cleanup** : We hardened cleanup logic to avoid race conditions and default to never deleting queuing disciplines, minimizing the risk of disrupting other agents.


**Vendor coordination and detection** : We proactively collaborate with vendors like Cilium on priority conventions and hardcoded handles. We also improved documentation and added detections that warn when another process may disable or interfere with our network monitoring.


## 5. Measuring performance impact is a necessary evil and a two-step process


eBPF-powered tools introduce two levels of overhead. One is visible: the memory and CPU consumed by the user-space agent you deploy. The other is more hidden, harder to benchmark, and often more important to get right: the performance impact on services introduced by your eBPF programs, plus workload-dependent memory consumption from non-preallocated eBPF maps.


### Pitfall 1: Always monitor and benchmark CPU and memory usage under real load


This might sound obvious, but rigorously measuring the performance of your security tools—especially under heavy load—is critical. This is even more true in Kubernetes, where strict CPU and memory limits are often enforced on sidecars to ensure they don’t interfere with business-critical workloads. With eBPF-based tools, stress testing is even more important, since some kernel objects (like attached maps or buffers) can spike in memory usage if not properly sized or managed.


### Consequences


When a security sidecar consumes excessive CPU or memory, it can exhaust host resources, degrade critical services, or render them unavailable. In severe cases, the kernel may kill the process (OOM) to reclaim memory, creating immediate gaps in security coverage and monitoring.


### How Datadog handles it


To mitigate this risk, Datadog has taken several steps to protect customer infrastructure:


**Resource limits** : We set strict memory and CPU limits on our Workload Protection agent so it can’t starve a host. By designing with these constraints in mind, we continuously optimize our resource usage to maintain both operational reliability and strong security guarantees.


**Deployment at Datadog’s scale** : Workload Protection runs at scale internally—including on Datadog’s edge infrastructure—which is a strong testament to its efficiency and resilience under pressure.


### Pitfall 2: Always measure the performance impact of kernel instrumentations


Like any kernel-level instrumentation, eBPF introduces runtime overhead. While eBPF was designed to minimize its footprint—for example, limiting program instruction counts—the impact on production workloads can still be significant if not carefully managed. Before deploying at scale, it’s critical to understand and control this cost. Two main factors drive overhead:


**Hook points and attachment strategy** : Where you attach matters. Hooking user-space functions with uprobes is typically more expensive than hooking kernel-space functions, since uprobes require two extra context switches. Some program types have lower overhead; for instance, entry and tracepoints are much more efficient than kprobes. (See Cloudflare’s[benchmark for detailed comparisons](https://github.com/cloudflare/ebpf_exporter/blob/master/benchmark/README.md) .)


**Map operations and program complexity** : Program complexity and map usage heavily influence kernel cost. Some map types, like` BPF_MAP_TYPE_LRU_HASH` , are slower because they require cross-CPU synchronization, while others, such as` BPF_MAP_TYPE_PERCPU_ARRAY` , offer faster CPU-local access. In practice, program complexity is often the main driver of kernel overhead.


### Consequences


Neglecting kernel impact can cause severe production issues. Services may degrade under load (for example, reduced throughput) or require costly horizontal scaling just to handle the same traffic—raising infrastructure costs and frustrating customers.


### How Datadog handles it


At Datadog scale, we’ve seen firsthand how certain hooks—like` raw_syscalls` tracepoints—can affect production systems if not handled carefully, from slowing connection acceptance to consuming host resources. Over time, we refined our approach to ensure we could run even at our edge endpoints. What’s worked for us:


**Filter aggressively in the kernel** : Drop events as early as possible to reduce both user-space processing and kernel time. With Workload Protection’s default ruleset, we carefully drop—based on the loaded rules—up to 95% of captured events before they reach user space. This keeps the agent lean and preserves kernel performance.


**Expose internal observability** : We maintain dashboards, metrics, SLOs, and runbooks so SRE can quickly identify—or rule out—our agent as a source of kernel-related performance issues. This helps catch regressions early and avoids wasted time in incident investigations.


**Test at scale on diverse environments** : Every environment behaves differently. A setup that runs smoothly in one context can cause unexpected issues in another. By treating each new environment as a potential failure source and testing broadly, we improve reliability and add safeguards for unpredictable loads.


## 6. Best practices before rolling out to production—and acknowledging the risks


As we’ve seen, running eBPF in production can have real consequences—from consuming CPU or memory to disrupting business-critical operations like networking—and even introduce a new attack surface. That doesn’t mean you shouldn’t use eBPF; it means you should use it with care, especially when rolling out changes. This is particularly true for security, where eBPF programs can make access-control decisions based on dynamically loaded rules. Although the specific issue[that hit Crowdstrike in 2024](https://www.crowdstrike.com/en-us/blog/falcon-content-update-preliminary-post-incident-report/) might have been avoided with eBPF, there are many other ways for eBPF could trigger a similar incident. eBPF isn’t the “one solution” that will prevent all incidents from happening.


### Pitfall 1: Maintaining and deploying security tools at scale is risky business


Writing detection rules and operating security tools at scale is inherently challenging,and by making kernel instrumentation more accessible than ever, eBPF can amplify those risks:


**Detection engineering risks** : It’s easy to miss edge cases in customer environments and accidentally push faulty rules into production, which is especially dangerous when rules take active responses, such as killing processes or blocking access to critical resources.


**Engineering and agent risks** : Attaching an eBPF program to the wrong kernel hook can throttle connection-accept rates on edge nodes. Large or poorly optimized programs on syscall entry points can slow the kernel and force unnecessary scaling.


**Environment variability** : The behavior and performance impact of any eBPF tool depends heavily on the workload. Treat each customer environment as unique and potentially fragile—even small changes can have unexpected consequences.


### Consequences


Faulty updates can cause anything from degraded coverage to severe outages or business-critical disruptions.


### How Datadog handles it


Human error is inevitable, but it can be mitigated. Our customers trust us to keep their most critical workloads secure and operational. At Datadog, we take this duty seriously and have implemented safeguards:


**Test comprehensively** : We validate across a wide range of kernel versions and Linux distributions to ensure stability and minimal performance impact. New agent and rule versions are deployed to our own infrastructure first. We dogfood and we don’t ship something that doesn’t work for us.


**Roll out gradually** : We use slow, controlled deployments for new features and detection content. This helps catch regressions early and prevents widespread incidents before they reach production.


## What’s next ?


Despite its limitations and challenges, eBPF has a bright future. From performance monitoring to networking, security, and debugging, it’s become a must-have for systems engineers. As the tech evolves, we expect more features that smooth out today’s pitfalls, similar to what CO-RE, BPF links, and the newer TC classifier programs have already delivered.


What’s less certain is long-term access for third-party software on managed and serverless platforms. While most cloud providers seem to embrace this new technology and integrate into their own services (see[GKE Dataplane V2](https://cloud.google.com/blog/products/containers-kubernetes/bringing-ebpf-and-cilium-to-google-kubernetes-engine) , Google’s contributions to the eBPF ecosystem including[KRSI](https://lwn.net/Articles/798918/) , and the[Azure x Isovalent partnership](https://azure.microsoft.com/en-us/blog/microsoft-and-isovalent-partner-to-bring-next-generation-ebpf-dataplane-for-cloudnative-applications-in-azure/) to bring next generation eBPF dataplane), they usually block access to eBPF for managed or serverless compute, citing security or operational concerns. For example, after multiple back and forths,[it’s still unclear whether AWS Fargate](https://github.com/aws/containers-roadmap/issues/1027) will allow third-party software to use eBPF, despite requests from the community.


One way forward is to consolidate the eBPF ecosystem, making it safer to use and harder to abuse, like Microsoft’s proposed the[Hornet Linux security module](https://lwn.net/Articles/1015105/) , which introduces a signature verification scheme for eBPF programs, similar to what exists today for kernel modules. What is certain is that more open platforms will benefit end users, as third-party tools won’t have to fall back to heavier monitoring techniques on locked-down platforms (see[our workaround](https://docs.datadoghq.com/security/workload_protection/guide/ebpf-free-agent/?tab=docker) for platforms without eBPF support).


At Datadog, we recognize the huge potential of eBPF, and will continue to harvest its power for multiple use cases—security with Workload Protection, network monitoring with[Cloud Network Monitoring](https://docs.datadoghq.com/network_monitoring/cloud_network_monitoring/) , service monitoring with[Universal Service Monitoring](https://www.datadoghq.com/product/universal-service-monitoring/) , and more to come.


## Closing thoughts


Running eBPF in production demands constant learning, careful rollout, and fine-grained observability. At Datadog, we’ve tackled these challenges head-on, refining our approach to ensure reliability and performance. By understanding these lessons, you can avoid common traps and make informed decisions when building with this technology. As the ecosystem evolves, staying vigilant and learning from real-world experience is key to harnessing eBPF’s full potential.


The Datadog Security Engineering team is hiring. Come help us push eBPF forward in production—at scale.[Explore open security roles](https://careers.datadoghq.com/all-jobs/?s=security&parent_department_Engineering%5B0%5D=Engineering&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025-workloadprotection-ebpf&gh_src=hm4uekgj1us) .


-
-
-
