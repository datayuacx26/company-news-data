---
schema_version: "1.0.0"
document_id: "67f9913e416c4b1089748246c0fb6ebdead4981593bf310390243b773d86ee15"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/paranoia-is-baseline-now"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:59312e3d0122b15bbd31d471a56a1aa14fa3454162bae93896102d363504d79e"
---

# Paranoia is baseline now: Security in the AI era

## Paranoia used to be optional


There's a certain attitude people have toward the extremes of hardening. Disable Hyper-Threading? Sure, if performance doesn't matter! Lock kernel module loading after boot? Cool, until it breaks. I've had that attitude myself, and for the longest time, it was the right one.


The kernel had earned a lot of trust. Bugs got reported, embargoes held (mostly...), and the really nasty bugs usually got patched before exploitation in the wild. So I never even really thought about obscure kernel features - if it has a CVE against it *right now* , it's usually already fixed. Many hardening steps felt like a mix between an insurance policy that won't ever pay out and security theater with performative hardening as the main star.


Then, this spring, something genuinely changed. AI increased productivity for all - even attackers, who could look for vulnerabilities faster and in more places than ever before. And once such a bombardment of vulnerabilities starts, you stop asking "is this feature vulnerable today" and start asking "isn't it reckless to let the kernel auto-load whatever". Unused kernel surface is no longer just debt.


## The wave


Five Linux local privilege escalation (LPE) vulnerabilities got disclosed and immediately abused in a couple of weeks. Copy Fail started the party. It was the result of an hour-long AI scan of the kernel's` crypto/` subsystem. The published exploit for it was a short Python script that worked on almost everybody's favorite Linux distributions. The vulnerability had been sitting in` algif_aead` , introduced by an optimization that everybody had in their kernel, and nobody really needed.


Here's the run:


Date Name Class Surface abused


Apr 29[Copy Fail](https://en.wikipedia.org/wiki/Copy_Fail)
([CVE-2026-31431](https://www.cve.org/CVERecord?id=CVE-2026-31431) ) page-cache write → root AF_ALG +` splice()` ,
` algif_aead` module


May 7 Dirty Frag
([CVE-2026-43284](https://www.cve.org/CVERecord?id=CVE-2026-43284) +[CVE-2026-43500](https://www.cve.org/CVERecord?id=CVE-2026-43500) ) page-cache write → root IPsec ESP,
` esp4` /` esp6` ,` rxrpc` modules


May 13 Fragnesia
([CVE-2026-46300](https://www.cve.org/CVERecord?id=CVE-2026-46300) ) page-cache write → root XFRM ESP-in-TCP coalescing,
` esp4` /` esp6` ,` rxrpc` modules


May 20 ssh-keysign-pwn
([CVE-2026-46333](https://www.cve.org/CVERecord?id=CVE-2026-46333) ) FD theft → secrets/root` ptrace` exit race +` pidfd_getfd`


May 21 PinTheft
([CVE-2026-43494](https://www.cve.org/CVERecord?id=CVE-2026-43494) ) page-cache write → root` io_uring` + RDS zerocopy double-free,
` rds_tcp` ,` rds` modules


Four of the five are almost the same bug: some optimized crypto or zerocopy path writes into a buffer the kernel doesn't actually own, and you end up with a controlled write into the page cache of a Set User ID (SUID) binary you're allowed to read. They don't actually use the network, and none of them are extremely privileged. (I'm not a kernel hacker, so I rely on the write-ups, reading the patches and a bit of guesswork, but the pattern is real.)


These kinds of things have happened before. We had embargoes violated or not-quite-responsible disclosures regarding vulnerabilities. We always had some people looking at recent Linux patches, trying to reverse engineer vulnerabilities from patches - we also had hasty fixes introducing new bugs. But we never had this scale or speed.


Patches are still a must - but that's reactive. The patching is now a race against something that can read 100x more code than you, even the boring bits. But we can give them less attack surface and a harder time.


## Paranoia keeps becoming the default


Just to preface this: nothing listed here is really new. These hardenings started their lifetime as paranoid measures, then moved into normalcy, best practice or even defaults.


For example, OpenBSD has always been quick and an early adopter of these ideas and has often been called paranoid due to its choices. When OpenBSD devs separated the privileges in OpenSSH, to separate the unprivileged process talking on the network (risky), it felt like over-engineering, but today it's common. Their W^X (memory is writeable or executable, not both) and a stack-smashing protector (ProPolice) were adopted in OpenBSD system-wide in 2003, but it took Linuxes one more year for NX and multiple years to turn stack protector on (Ubuntu and RHEL ~2006, Debian ~2013).


I have to also tip my hat to PaX and grsecurity. They have maintained a huge set of out-of-tree patches to harden the Linux kernel, with innovative and early adoption of hardening ideas. Just listing what they had patches for early on would often sound like an advertisement for their paid products.


Sadly, these paranoid hardenings often came with a cost (performance, ease of use, etc.) up front, but very abstract protection against a bug (or bug class) that might not exist yet, so they were usually adopted after the fact.


## Sorting hardening by cost


Hardening is often a balancing game, and it's not just one thing or one idea. Each choice can have wildly different trade-offs, and they all depend on what you are trying to achieve. Some of them are trivial choices, some of them may make a service 30% more expensive to run. To make choices easier, I feel it's easier to think about categories:


- **A - Free.** Simple housekeeping, won't harm anything.
- **B - Nearly free.** Turning off genuinely useful things, but not things we use.
- **C - Costs you visibility.** Makes operations harder (debug, monitor), but closes doors.
- **D - Costs you some performance.** Can have a negative effect on performance, but very tiny. Might hurt on high node count.
- **E - Costs you real money.** You lose throughput, you have to run more nodes or more expensive hardware. Danger zone.


This is neither a definitive guide for hardening, nor a todo list! The scope here is mostly about the Linux kernel, but these ideas can be applied to other pieces of software and infrastructure. If you don't "check all the boxes" and apply all hardening, you are not necessarily reckless. If you are conscious about your trade-offs, then you will be able to decide what is worth it and what is not. Also, do note that in this list, only the cost of running servers and services is considered - things like spending engineering hours for security, maintaining customizations, and making decisions, are not part of it.


It's important to note that I'm writing about generic advice and I'm not defining a concrete threat model here - you will need to consider your own requirements and threat model to weigh any advice in this post. I also want to highlight that the goal of the hardening measures and ideas I'm talking about is to minimize the chance an attack is successful or useful for information extraction, denial of service or similar malicious goals. Exploitation often involves multiple vulnerabilities, where attackers might have to orient themselves and might have to pivot from lesser to privileged access.


### A - Free: the housekeeping


The Linux kernel's support for protocols, hardware and others is extensive, but this also means a bigger attack surface. While maintaining your own custom kernel build per use-case might be good, the kernel distributed with common distributions can also be locked down. For example, you may blocklist rarely used kernel modules, which[Ubuntu already does](https://documentation.ubuntu.com/security/security-features/kernel-protections/#denylist-rare-protocols) for you. This meant that PinTheft ([CVE-2026-43494](https://www.cve.org/CVERecord?id=CVE-2026-43494) ) didn't affect stock Ubuntu installations. The good thing about this is that you may also blocklist and unload kernel modules on running machines, like the workarounds for these vulnerabilities, which reduces the chance of being affected by faulty hotfixes, such as Fragnesia ([CVE-2026-46300](https://www.cve.org/CVERecord?id=CVE-2026-46300) ).


Some hardening can also just make attackers' lives harder, like reducing information leaks on the system. For example, don't let any user read` dmesg` (sysctl:` kernel.dmesg_restrict=1` ), which can contain sensitive information that regular users really shouldn't read. Another example is to not let` setuid` /` setgid` programs coredump (sysctl:` fs.suid_dumpable=0` ), which will hold sensitive information in case of, say,` sudo` . Two more concrete hardenings catching real bugs are making NULL pointer dereference exploitation much harder by making low memory unmappable (sysctl:` vm.mmap_min_addr=65536` ), and enforcing stricter symlink/hardlink checks (sysctl:` fs.protected_symlinks=1` ,` fs.protected_hardlinks=1` ), so TOCTOU (time of check, time of use) symlink attacks are much harder.


Another one to consider is to reduce` setuid` binaries to a minimum, and make them unreadable by anybody outside of root - which could block attacks relying on poisoning the page cache of these` setuid` files. Be careful with persistence here, though.


These are pretty easy to implement, free in terms of performance, not too intrusive and won't reduce observability for operators. Many of these hardenings became baseline for some popular Linux distributions.


### B - Nearly free: purge unused modules


The basic hygiene step could be to enforce signature checking on kernel modules (set` /sys/module/module/parameters/sig_enforce` to 1), to block loading unsigned modules. While` lockdown=integrity` (` /sys/kernel/security/lockdown` set to` integrity` ) also allows this, that value also blocks other functionality, such as` kexec` and some debugging opportunities - more on that later. This might be enabled based on your distribution (Ubuntu enabled it by default in 20.04 LTS) and your UEFI secure boot setting.


A standard Debian or Ubuntu image for example ships multiple thousands of kernel modules. What used to be paranoid but I think is a good practice now is to prune your kernel modules and block kernel module loading after boot (via` /proc/sys/kernel/modules_disabled` ), which also prevents an attacker from loading or unloading any kernel modules. (While kernel module signing exists, vulnerable kernel modules are also signed.) Doing this on fixed-purpose nodes can be pretty easy (like hypervisors, simple nginx web servers), but it can be really painful for generic hosts (like kubernetes nodes) or generic images.


### C - Costs visibility: ptrace, eBPF


I think hardening measures impacting observability are pretty divisive: observability helps you, but it can also help attackers. I remember calling someone paranoid for disabling eBPF altogether, but I do now agree that limiting it at least is a minimum (sysctl:` kernel.unprivileged_bpf_disabled=1` - 1 is disabled until reboot, 2 is disabled, but reversible), so only` root` can load BPF programs.


There is also Yama, which can help harden certain parts of the kernel. For example, you can enforce that only` CAP_SYS_PTRACE` privileged processes can do ptrace (sysctl:` kernel.yama.ptrace_scope=2` ), but it's also sensible to turn this off completely (sysctl:` kernel.yama.ptrace_scope=3` ) until a reboot. The ssh-keysign-pwn ([CVE-2026-46333](https://www.cve.org/CVERecord?id=CVE-2026-46333) ) vulnerability could've been prevented with this. However, completely disabling ptrace means no` gdb -p` or` strace -p` - so don't do this on your dev machine, but it's fine on a server.


You can also reduce visibility into the kernel by hiding kernel pointers from userspace (sysctl:` kernel.kptr_restrict=2` ), which helps prevent information leaks.


### D - Costs performance: pinning, libc


In this category, there are a couple of hardening options, which will likely reduce performance, but can be worth it. One example is if you run workloads for multiple tenants, and want to provide better isolation on your hypervisor node. In this case, it will be necessary to do CPU core pinning at least, so no two tenants are on the same CPU core at the same time, so fewer resources are shared - more on this in the last point. Depending on your business, you might even have to separate tenants between NUMA nodes, and might have to consult your CPU documentation.


One thing that is pretty divisive is using glibc or musl libc. On one hand, musl libc's code is incredibly easy to understand, and small enough to reason about its correctness, giving a "safer feeling" codebase. On the other hand, glibc has had more eyeballs on it for years, and the performance is also on its side.


The math here is pretty interesting: 3% performance may not seem like much, but these loses stack up, and a couple of percentages can silently bleed a lot of money.


### E - Costs real money: turn off SMT


Let's get back to OpenBSD and paranoia a bit. 20 years ago, CPUs were considered reliable, then came the hardware bugs (Spectre and its siblings), which were fixable by microcode updates to some degree, but might require compiler patches, kernel support, and it can cause performance loss. The choice is clear: be vulnerable, or be slower by some percentages.


However, there is also the case of SMT (Simultaneous multithreading, or Hyper-Threading in the case of Intel), which essentially lets 2 threads independently execute on a single CPU core, albeit slower. The upside is that instead of 1 physical CPU core's performance you get 2 times roughly 65% performance, resulting in up to 30% overall performance increase, depending on workload. The problem is that this means that 2 threads are executing in the same CPU, and they share a physical, classically indivisible unit of computing. Good performance, potential problems.


As a reaction to CPU hardware bugs, OpenBSD disabled SMT altogether in June 2018 (sysctl` hw.smt` default off), calling everyone else to follow suit. Then, still in 2018 and later in 2019, multiple SMT hardware bugs were disclosed, such as L1TF (e.g.[CVE-2018-3615](https://www.cve.org/CVERecord?id=CVE-2018-3615) ,[CVE-2018-3620](https://www.cve.org/CVERecord?id=CVE-2018-3620) ,[CVE-2018-3646](https://www.cve.org/CVERecord?id=CVE-2018-3646) ), MDS (e.g.[CVE-2018-12126](https://www.cve.org/CVERecord?id=CVE-2018-12126) ,[CVE-2018-12127](https://www.cve.org/CVERecord?id=CVE-2018-12127) ,[CVE-2018-12130](https://www.cve.org/CVERecord?id=CVE-2018-12130) ,[CVE-2019-11091](https://www.cve.org/CVERecord?id=CVE-2019-11091) ) and TAA ([CVE-2019-11135](https://www.cve.org/CVERecord?id=CVE-2019-11135) ), where these vulnerabilities were either caused by SMT or were amplified by SMT.


So, OpenBSD was right. But Linux wasn't wrong either, because 30% of performance loss is extreme - leaving that on the table means you have to pay for much more resources, and the business is hurt. So, while OpenBSD disabled it by default, Linux decided to keep it enabled by default, because in a plethora of server use-cases the potential vulnerabilities don't matter (big data clusters, storages, etc.) or can be mitigated (core isolation between tenants, hardened databases, etc.), and if you keep your personal machine up-to-date, keeping up to 30% of performance gain is not that reckless.


There is also io_uring, which for example Google decided to get rid of on production servers and ChromeOS and blocked for Android, because in the kCTF program (where Google pays for kernel vulnerability reports)[60% of their successful vulnerability submissions were io_uring](https://security.googleblog.com/2023/06/learnings-from-kctf-vrps-42-linux.html#:~:text=in%2E-,Learnings%20and%20Statistics) , meaning it has a poor track record as a Linux feature, amassing more vulnerability reports in this program that year, than the rest of the Linux kernel. So, it is straightforward to disable it (sysctl:` kernel.io_uring_disabled=2` ). However, io_uring can provide dramatic performance increases in certain IO-heavy workloads - some PostgreSQL synthetic benchmarks claim even 2-3x increases, but real-world numbers can easily be in the 10-30% range, depending on many factors. I would argue that in a number of use-cases (e.g. big data clusters, etc.), it makes a lot of sense to enable io_uring if it makes your workload faster or more efficient, so you can reduce the required number of hosts. The PinTheft ([CVE-2026-43494](https://www.cve.org/CVERecord?id=CVE-2026-43494) ) vulnerability also required io_uring.


## Doing this without overdoing it


The hardest part of hardening is finding balance in everything - performance, ease of use, and also, very importantly, the culture. It can easily become a *Paranoiafest* , because there is always something that can be locked down further. This can fatigue people, to the point that they just start ignoring the warnings after the hundredth "this is extremely important" unimportant tunable that catches nothing.


I recommend documenting hardening steps, especially:


- why you do it (threat model, historical bug count, or if it's an immediate fix for a vulnerability)
- what are the costs (resources, operational, complexity)
- operational notes (why it was made, or how it changes operations), if necessary


This documentation makes it easier for everyone to digest decisions and review them later.


While paranoia in hardening used to be optional, in a race with AI-assisted bug hunting it is reckless to leave unnecessary attack surface. It is also very important to remind ourselves that hardening is supposed to help ensure things are *working* safely - a turned-off computer buried in the forest is extremely secure, but won't serve customers.


## FAQ


Which Linux kernel hardening steps are basically free to turn on?


The category A housekeeping: it costs nothing in performance and doesn't reduce observability for operators. Restrict` dmesg` to root (` kernel.dmesg_restrict=1` ), stop` setuid` /` setgid` programs from coredumping (` fs.suid_dumpable=0` ), make low memory unmappable so NULL pointer dereferences are harder to exploit (` vm.mmap_min_addr=65536` ), and enforce stricter symlink/hardlink checks (` fs.protected_symlinks=1` ,` fs.protected_hardlinks=1` ) to make TOCTOU symlink attacks much harder. Blocklisting rarely used kernel modules is in the same bucket, and it's why stock Ubuntu wasn't affected by PinTheft.


Should I disable SMT (Hyper-Threading) for security?


Only if your threat model justifies it, because the bill is real. SMT gets you up to 30% more performance, and turning it off means paying for that much more hardware. OpenBSD disabled it by default in 2018 and was vindicated by L1TF, MDS, and TAA, but Linux keeping it on wasn't wrong either: for big data clusters, storage, or a personal machine you keep patched, the shared-core risk often doesn't matter or can be mitigated with core isolation between tenants. If you run untrusted multi-tenant workloads on shared cores, that's when disabling it earns its cost.


Can I apply these hardening changes to a running server without rebooting?


A lot of them, yes. Most of the sysctls take effect at runtime, and you can blocklist and unload kernel modules live, which is exactly how the workarounds for these LPEs worked and why you don't have to trust a hasty hotfix. A couple are one-way latches: once you set` kernel.yama.ptrace_scope=3` or` kernel.unprivileged_bpf_disabled=1` , you can't loosen them again until you reboot. That irreversibility is the point. The thing to actually watch is the reverse problem: runtime sysctl changes don't survive a restart unless you write them into your persistent config (` sysctl.d` ), so a reboot can quietly hand your attack surface back to attackers.


If I patch fast, do I still need all this hardening?


Patching is mandatory but it's reactive, and now it's a race against tooling that can read 100x more of the kernel than you can, including the boring bits nobody audits. Reducing attack surface is what changes the odds before a patch exists: four of the five LPEs in the wave abused optimized crypto or zerocopy paths that most machines had enabled and didn't need. Patch and prune the surface. Doing only the first leaves you running every race at full speed.


## Related posts


- [Container security at scale: Building untrusted images safely](https://depot.dev/blog/container-security-at-scale-building-untrusted-images-safely)
- [How we got microVMs booting in under a second](https://depot.dev/blog/optimizing-microvm-boot-times)
- [Now available: Egress filtering for GitHub Actions Runners](https://depot.dev/blog/now-available-egress-filtering-for-github-actions-runners)


Héja Péter (Vau)


Staff Infrastructure Engineer at Depot
