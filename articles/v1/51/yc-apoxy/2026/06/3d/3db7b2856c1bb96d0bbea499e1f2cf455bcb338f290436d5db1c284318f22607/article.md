---
schema_version: "1.0.0"
document_id: "3db7b2856c1bb96d0bbea499e1f2cf455bcb338f290436d5db1c284318f22607"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/gvisor-cold-start-where-the-time-goes"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:fa7398b2bb111e23a6dc01f46db4fa77f2fdd2bc8e04b4d0e50bcef5e6396340"
---

# Where gVisor cold-boot time goes - and why a bigger instance won't fix it

For our[CLRK](https://apoxy.dev/enter-clrk) runtime we run agents inside[gVisor](https://gvisor.dev/docs/) sandboxes because we believe its isolation properties are[the right ones](https://apoxy.dev/enter-clrk#gvisor-as-an-isolation-primitive) for untrusted code that touches LLM tools and the customer's network. So we wanted to know exactly what that isolation costs at boot, and which of the available knobs can reduce that number.


Across eight hosts ranging from an M1 Mac to an AWS bare-metal instance, the same workload took anywhere from **81 ms to 280 ms** of` runsc create` plus` runsc start` . The fastest host was the M1 laptop, which beat every cloud instance - including the biggest-core-count boxes, which landed dead last. Most of the obvious tuning knobs turned out to be either neutral or slightly negative. The one trick that worked the best recovered a` 4.5x` reduction, but we had to cheat a little bit.


## Why we care about gVisor cold start


CLRK agent sandboxes are launched on a worker pod with one of two purposes. For a long-lived` DaemonAgent` , boot latency is amortized over the agent's lifetime and barely matters. For a short-lived` TaskAgent` , boot latency is added to user-perceived response time on every dispatch. That second case is the one we wanted to bound.


Existing gVisor[benchmarks](https://gvisor.dev/docs/architecture_guide/performance/#start-up-time) tend to measure either steady-state syscall throughput (workload runs inside the sandbox) or whole-container boot including image pull. Neither is what we cared about. We wanted the wall time from "controller decides to dispatch this task" to "the init process inside the sandbox has been scheduled," holding the image pull and worker overhead aside as separate concerns.


So we built a measurement spike that ran the exact same` runsc` argv as the CLRK worker, with the same OCI spec, the same` sentrystack` plugin network, and the same` --platform=systrap` flag. We measured each phase end to end. Then we replicated the measurement on eight different hosts to see which axes of variance matter.


## How we ran the benchmark


We tried to reuse existing CLRK components as much as possible - the image store and rootfs cache, the plugin network the sandbox attaches to, and the` runsc` (the worker binary doubles as` runsc` , so the spike forks and measures the actual thing rather than a reimplementation that could drift from it). Same goes for other bits - capabilities, mounts, namespades, and cgroup - Claude was entrusted to rip out/import all of that out byte-for-byte to make the bench as close to the real thing as possible. A human ensured no funny business was going on with the benchmark setup.


Per-phase boundaries are split into two complementary streams:


- **Outer wall times** measured externally around` runsc create` ,` start` ,` wait` , and` delete` .
- **Internal phase markers** parsed from the per-sandbox` --debug-log` by matching known Sentry log strings (` Create container` ,` Gofer started, PID` ,` Installing seccomp filters` ,` loader.go:519\] CPUs:` ,` Process should have started` , and others).


Each run was 5 warmup iterations (discarded) plus 50 measured cold iterations using` alpine:3.20` as the rootfs and` /bin/true` as the init process - the cheapest possible boot. Image extraction is timed separately, since the worker's` ImageStore` caches the extracted rootfs across sandboxes and the per-sandbox cost is a near-zero digest cache hit.


We re-ran the same spike across:


- Apple M1 Pro inside an OrbStack arm64 Ubuntu VM (10 vCPU)
- AWS` c7g.xlarge` (Graviton 3, arm64)
- AWS` c7g.metal` (Graviton 3, arm64, 64 vCPU)
- AWS` c7i.xlarge` (Sapphire Rapids, amd64)
- AWS` c7i.metal-24xl` (Sapphire Rapids, amd64, 96 vCPU)
- GCP` c4a-standard-4` (Google Axion, arm64)
- GCP` c3-standard-4` (Sapphire Rapids, amd64)


Each host ran Ubuntu 24.04. The cloud guests ran the stock Canonical` 6.17.0-aws` /` 6.17.0-gcp` kernels; the OrbStack VM ran the custom` 6.19.13-orbstack` kernel. The spike was compiled fresh on each host with` go-1.23` .


## Hard numbers


CORE


is the sum of` runsc create` +` runsc start` . OVERALL


adds` runsc wait` (` /bin/true` exit + reap) and` runsc delete` . All values are over 50 measured iterations, systrap,` /bin/true` init.


gVisor cold boot


FIG. 01


Hover a row for p50 / p95 / max


Apple M1 Pro


OrbStack


** **78**


** **103**


Apple M1 Pro *OrbStack*


** CORE


**78** *p50* **83** *p95* **84** *max*


** OVERALL


**103** *p50* **110** *p95* **111** *max*


GCP c4a-standard-4


Google Axion


** **129**


** **174**


GCP c4a-standard-4 *Google Axion*


** CORE


**129** *p50* **139** *p95* **144** *max*


** OVERALL


**174** *p50* **186** *p95* **195** *max*


AWS c7i.xlarge


Intel


** **134**


** **183**


AWS c7i.xlarge *Intel*


** CORE


**134** *p50* **145** *p95* **154** *max*


** OVERALL


**183** *p50* **195** *p95* **201** *max*


AWS c7a.xlarge


AMD EPYC 9R14


** **137**


** **185**


AWS c7a.xlarge *AMD EPYC 9R14*


** CORE


**137** *p50* **146** *p95* **148** *max*


** OVERALL


**185** *p50* **195** *p95* **199** *max*


GCP c3-standard-4


Sapphire Rapids


** **143**


** **188**


GCP c3-standard-4 *Sapphire Rapids*


** CORE


**143** *p50* **152** *p95* **267** *max*


** OVERALL


**188** *p50* **200** *p95* **312** *max*


AWS c7g.xlarge


Graviton3


** **156**


** **208**


AWS c7g.xlarge *Graviton3*


** CORE


**156** *p50* **168** *p95* **184** *max*


** OVERALL


**208** *p50* **224** *p95* **236** *max*


AWS c7i.metal-24xl


Intel · metal


** **191**


** **263**


AWS c7i.metal-24xl *Intel · metal*


** CORE


**191** *p50* **201** *p95* **230** *max*


** OVERALL


**263** *p50* **275** *p95* **312** *max*


AWS c7g.metal


Graviton · metal


** **203**


** **276**


AWS c7g.metal *Graviton · metal*


** CORE


**203** *p50* **218** *p95* **220** *max*


** OVERALL


**276** *p50* **293** *p95* **299** *max*


0


80


160


240


320 ms


** Core boot


** Overall


** bar = p50, whisker → p95 / max


Lower is better


Two things to note in the tails. First, every host is well-behaved except GCP` c3-standard-4` , whose max blew past 250 ms while p95 was only 152 ms - a single outlier iteration on what we believe is GCP host-side noisy-neighbor scheduling. Second, the AWS hosts with the most cores have noticeably worse medians (and tails) - caused by the membarrier/RCU grace-period tax that scales with core count (broken down in the` hostmm` section below), which a one-line gVisor patch removes.


### Mac supremacy


My M1 Pro is a 3.2 GHz part; the Intel Xeon in the AWS` c7i.xlarge` boosts to 3.8 GHz and Graviton 3 is fixed at 2.6 GHz, so by clock alone we'd expect the Intel guest fastest, not last by a factor of two behind an older laptop. Our first guess why that is was memory-level parallelism - boot chases pointers through cache-cold Go objects, and the cores even rank-match by reorder-buffer size. A top-down PMU profile of the boot falsified this theory: the core is stalled on a DRAM miss just **0.6% of cycles** , and disabling every hardware prefetcher moves boot time by **0.2%** - so neither the instruction window nor memory latency is the bottleneck.


PMU


**Performance Monitoring Unit (PMU).** Hardware counters in every CPU core that tally microarchitectural events - cache/TLB misses, mispredicts, and why each cycle stalled - at near-zero overhead. The *top-down* method buckets every issue slot the core had into *frontend-bound* (no instruction ready), *backend-bound* (ready, but no cache/DRAM/port resource), *retiring* , or *bad speculation* - so you see where the stalls went. Reading it needs the counters exposed to the OS, which is why this ran on bare metal.


What the counters show instead is a CPU front end starved for instructions: the biggest bucket is **frontend-bound, ~28% of issue slots** , lost to a cold instruction cache, iTLB misses and branch resteers as gVisor's enormous code footprint pages in. That also explains the laptop supermacy - the M1's Firestorm core has an 8-wide decoder and a ~192 KB L1 instruction cache, roughly 3x the Graviton's, so on an instruction-supply-bound workload it simply feeds itself faster. Full disclosure: we couldn't measure M1's instruction-side counters because Apple's PMU is not exposed in a VM gVisor runs in, so this remains only an educated guess.


## Throwing a kitchen sink of optimizations


We chased a number of threads with most not making much of a dent, here are the most interesting ones:


### KVM does not help cold boot


We tested` --platform=kvm` on both metal instances. The result was within noise of systrap on both arches. On amd64 KVM, the` runsc wait` phase showed a curious tail blowup (p50 of 134 ms vs 35 ms on systrap), apparently from KVM's exit-handling for trivial-exit processes; we did not chase this further.


The reason KVM was neutral is that cold boot is dominated by Go-runtime initialization, MemoryFile preallocation, and the Sentry's urpc control-server handshake - none of which are syscall-heavy from the host's perspective. KVM's win lives in steady-state syscall throughput for code running *inside* the sandbox, which` /bin/true` does not exercise. To measure the KVM benefit you need a syscall-heavy guest workload which doesn't match our benchmark setup.


### Nested-paging can be holding us back on cloud, THP to the rescue


Cloud guests pay a tax that doesn't appear on bare metal and is much smaller on a Mac under Apple's Virtualization.framework: every Sentry memory access walks **two** page-table hierarchies. First the guest's stage-1 paging, then the host's EPT (Intel) or NPT (AMD). A guest TLB miss that would have cost one walk on unvirtualized hardware costs ~150 cycles on a nested guest. gVisor's cold boot is heap-allocation-heavy and page-fault-heavy, so every fresh page paid this tax twice.


THP


**Transparent Huge Pages (THP).** x86-64 pages are 4 KB by default; a huge page is 2 MB, and THP backs eligible memory with them automatically. Two wins: one TLB entry now covers 2 MB instead of 4 KB (512× the reach, so fewer TLB misses), and each page-table walk ends one level early - the page-directory entry maps the 2 MB region directly, so the PT level disappears. Under virtualization that second win compounds: a shorter guest walk fires fewer nested (EPT/NPT) walks along the way.


We flipped the host THP policy on a fresh` c7a.xlarge` (AMD EPYC 9R14) and measured both the wall time and the per-iter PMU counters:


Counter (20-iter run) THP=madvise **THP=always** Change


**CORE p50** **137 ms** **127 ms** **-10.5 ms (-7.6%)**


OVERALL


p50 185 ms 170 ms -15 ms (-8%)


cycles 20.9 B 18.0 B -14%


page-faults 656 k 339 k **-48%**


data page walks 8.6 M 5.0 M -42%


instruction page walks 4.8 M 3.7 M -23%


L1 DTLB misses total 66.9 M 36.7 M -45%


The 48% drop in page faults is the bigger effect than the walk-cost reduction itself - fewer faults means the kernel's fault handler runs fewer times during boot, which compounds across the Sentry's many fresh allocations. We confirmed the mechanism applies to the Sentry's own mappings by snapshotting` /proc/<sentry-pid>/smaps_rollup` of a long-lived sandbox:` AnonHugePages: 0 KB` with the default` madvise` policy,` AnonHugePages: 4096 KB` with` enabled=always` . The Go runtime cooperates with the system policy and the kernel collapses heap arenas into 2 MB pages at fault time.


The Mac doesn't enjoy this win. Running the same` THP=madvise` →` always` experiment on the OrbStack VM bought only **-2.4 ms (-2.9%)** of CORE


- vs the cloud's **-10 ms (-7.6%)** . That asymmetry is likely the result of Apple's Stage-2 translating on a 16 KB granule (M1 page size), and Firestorm having enormous TLBs. Bigger pages help cloud guests more because cloud hosts have far larger translation costs.


gVisor already ships the deterministic version of this, with a catch: the` MemoryFile` gets` MADV_HUGEPAGE`[automatically](https://github.com/google/gvisor/blob/cfb7c0629521099eb14d7bd86e9fbfa47287a640/runsc/boot/loader.go#L953) - but only when the host's` transparent_hugepage/defrag` is` always` ,` defer` , or` never` . Under the default` defrag=madvise` gVisor *disables* huge pages on the MemoryFile on purpose, because a madvised shmem allocation would synchronously compact where the equivalent native anonymous mapping would not - so` shmem_enabled=advise` is inert until you move` defrag` off` madvise` . The Sentry's own Go heap is anonymous and never madvised, and it boots and exits in ~130 ms - long before khugepaged (a ~10 s scan) would promote it - so backing it inside that window needs` enabled=always` with` defrag=always` , where first-touch faults allocate 2 MB synchronously. That` defrag=always` is what backed both mappings in the measurement above, for about 10-15% more RSS per Sentry process.


The whole interaction, holding` shmem_enabled=advise` and` enabled=always` fixed and varying only` defrag` , during the ~130 ms boot:


` defrag` MemoryFile Sentry Go heap Fault-path stall


` madvise` *(distro default)* 4K - gVisor refuses to advise 2M only if unfragmented † none


` defer+madvise` 4K - gVisor refuses to advise 2M only if unfragmented † none


` never` 2M only if unfragmented † 2M only if unfragmented † none


` defer` 2M only if unfragmented † 2M only if unfragmented † none


` always` **2M, guaranteed** **2M, guaranteed** **yes**


† The fault-time THP allocation takes a huge page when contiguous free memory is available - a freshly-cycled worker node - and silently falls back to 4K as the host fragments; no compaction runs in the fault path.` defrag=always` is the only value that forces synchronous compaction to guarantee 2M, and the only one that adds a fault-path stall. The two default rows are the trap: on a stock node` shmem_enabled=advise` leaves the MemoryFile at 4K no matter what, because the advise is gated off.


### Where pre-loader is spending its time


Parsing the Sentry's` --debug-log` timestamps gave us a phase-by-phase breakdown of an 84 ms boot on the Mac:


Boot timeline


FIG. 03


### Where the 84 ms goes


0


20


40


60


84 ms


1


2


3


4


5


6


7


8


1


runsc create CLI 0–3 ms


spec read · gofer fork + sync · sandbox fork


**3** *ms*


2


Sentry pre-loader (~25 ms) 3–28 ms


Go runtime init · argv + spec read from donated FD · config dump to debug log · pre-chroot reads (DMI / THP / nvidia) · applyCaps re-exec


**25** *ms*


3


Pre-loader seccomp 28–32 ms


seccomp filter install


**4** *ms*


4


boot.New - early phases 32–37 ms


CPUs · platform · MemoryFile · VDSO


**5** *ms*


5


boot.New - remainder 37–49 ms


VFS register · Kernel.Init · control server StartServing · sync byte → runsc create returns


**12** *ms*


6


runsc start CLI 49–52 ms


dial control socket · send StartRoot uRPC


**3** *ms*


7


Sentry start path 52–71 ms


setupNetwork · sentrystack.PreInit · l.run() · seccomp install · createContainerProcess · k.Start


**19** *ms*


8


/bin/true runs & exits 71–84 ms


workload runs to completion · exit reported via runsc wait


**13** *ms*


** Everything before **71 ms** is gVisor boot overhead - the workload runs only in the final 13 ms.


We initially attributed the 25 ms Sentry pre-loader latency to "Go runtime init + config dump + pre-chroot probes" which was quickly ruled out. Running the spike binary under` GODEBUG=inittrace=1` (Go's built-in package-init profiler) dumped per-package wall times that sum to **18.95 ms across 512 packages, with last-init elapsed at 27 ms** - matching the slowdown almost exactly. The 25 ms pre-loader delay is just Go package` init()` functions running serially before main.


Top offenders by clock time (this dump is from the 10-vCPU OrbStack VM; on the AWS hosts` hostmm` runs larger - it scales with online CPU count, broken down just below):


$


inittrace


TXT


` 5.000 ms gvisor.dev/gvisor/pkg/sentry/hostmm


1.500 ms k8s.io/client-go/kubernetes/scheme


0.930 ms sigs.k8s.io/controller-runtime/pkg/client/apiutil


0.800 ms gvisor.dev/gvisor/pkg/sentry/syscalls/linux


0.690 ms k8s.io/component-base/metrics/legacyregistry


0.670 ms github.com/prometheus/client_golang/prometheus


0.640 ms sigs.k8s.io/controller-runtime/pkg/internal/controller/metrics


0.620 ms k8s.io/component-base/metrics


0.500 ms k8s.io/apimachinery/pkg/runtime


...


`


Two things to notice.


**First** , the worker binary's k8s and controller-runtime imports bleed ~5 ms of init into the Sentry that the Sentry never uses. CLRK ships one binary that doubles as both worker and` runsc` via a self-execution shim - when` runsc create` forks` /proc/self/exe` to start the Sentry, all those imports' package inits run again because Go's loader doesn't know they're dead code on this code path. Splitting the binary (or build-tag-gating the k8s imports out of the runsc path) recovers this directly.


**Second** , the single biggest init line -` pkg/sentry/hostmm` - which we traced to a kernel RCU grace period. Its init eagerly issues` membarrier(MEMBARRIER_CMD_REGISTER_PRIVATE_EXPEDITED)` , which walks the kernel through` sync_runqueues_membarrier_state` , whose decisive path is a single` synchronize_rcu()` :


$


kernel/sched/membarrier.c


C


` static


int


sync_runqueues_membarrier_state


(


struct


mm_struct


*


mm)


{


int


membarrier_state


=


atomic_read


(


&


mm->membarrier_state);


/* ... */


if


(


atomic_read


(


&


mm->mm_users)


==


1


||


num_online_cpus


()


==


1


) {


this_cpu_write


(runqueues.membarrier_state, membarrier_state);


smp_mb


();


/* single-user fast path */


return


0


;


}


/* ... */


synchronize_rcu


();


/* wait one full grace period */


/* ... then IPI only the CPUs whose rq->curr->mm == mm ... */


return


0


;


}


`


*(kernel/sched/membarrier.c,[v6.17](https://github.com/torvalds/linux/blob/v6.17/kernel/sched/membarrier.c#L438) )*


RCU


RCU (read-copy-update) lets kernel readers run lock-free while a writer swaps in new data. The writer can't free the old copy until it is sure no CPU is still looking at it, so it waits for a *grace period* - the moment every online CPU has passed through a quiescent state (a context switch, or a timer tick taken in user or idle mode).` synchronize_rcu()` blocks the caller until that happens. The wait is bounded by the *slowest* CPU to check in, and the kernel only nudges stragglers along on a timer, so a grace period costs whole milliseconds - and the more online CPUs there are to wait on, the longer it runs. That last part is why a 64-core box pays this tax harder than a 4-core one.


For any multi-user` mm` on a multi-CPU host the fast path is skipped and the call blocks on a full` synchronize_rcu()` , whose latency tracks` num_online_cpus` (per-CPU-count): roughly 5-7 ms on a 4-vCPU host and 12-17 ms on a 64-vCPU host. A single cold boot fires the` REGISTER` call nine times (bpftrace-counted) - once per process in the self-execing` runsc` fork chain - so the grace period lands on the create+start critical path again and again, charging the full cost to CORE


over and over.


On systrap and ptrace nothing ever consumes the registration: both embed` UseHostGlobalMemoryBarrier` (` MEMBARRIER_CMD_GLOBAL` , a plain` synchronize_rcu` with no registration). Only KVM's` UseHostProcessMemoryBarrier` needs the private-expedited registration. So we gate it behind a` sync.Once` fired from the getters - init keeps only the cheap` QUERY` , and the registration happens lazily on first real use.` hostmm.init` drops from ~5-17 ms to ~0.001 ms, and CORE


p50 falls across the board:


Host GOMAXPROCS Baseline p50 Patched p50 Delta


` c7g.xlarge` (4 vCPU) 4 158.8 ms 135.5 ms -23.3 ms


` c7i.xlarge` (4 vCPU) 4 145.2 ms 118.1 ms -27.1 ms


` c7g.16xlarge` (64 vCPU) 64 203.8 ms 137.4 ms -66.4 ms


` c7a.16xlarge` (64 vCPU) 64 204.7 ms 144.9 ms -59.8 ms


### The one trick that helped the most


What if we avoided paying the cold boot cost for *every* request? Instead of always cold-booting a fresh gVisor sandbox we keep a pool of already-booted sandboxes - warm slots - idling on` sleep infinity` and dispatch incoming work into a free one with` runsc exec` . The expensive part of a cold boot -` runsc create` and` start` , the self-execing` runsc` fork chain, and every package init inside the Sentry - has already happened by the time a request arrives, so it pays only to spawn one more task in a sandbox that is already running. We pre-pay the cold boot once, off the request path, and amortize it across every task the slot serves before it recycles.


The warm-pool exec path skips every cost in that breakdown except "task creation inside an existing Sentry." It pays only the runsc exec CLI dispatch (about 5 ms) and the Sentry's` ExecuteAsync` handler (about 15 ms).


A separate run validated this design: a single long-lived sandbox booted with` sleep infinity` as init, with` runsc exec --cwd=/ <id> /bin/true` invoked 10 times in succession. p50 / p95 / max in ms:


gVisor warm exec


FIG. 02


Hover a row for p50 / p95 / max


Apple M1 Pro


OrbStack


** **20**


Apple M1 Pro *OrbStack*


** WARM


**20** *p50* **22** *p95* **23** *max*


AWS c7i.xlarge


Intel


** **28**


AWS c7i.xlarge *Intel*


** WARM


**28** *p50* **32** *p95* **36** *max*


AWS c7g.xlarge


Graviton3


** **21**


AWS c7g.xlarge *Graviton3*


** WARM


**21** *p50* **23** *p95* **23** *max*


0


10


20


30


40 ms


** Warm exec


** bar = p50, whisker → p95 / max


Lower is better


The narrow stdev (` ~1 ms` on the Mac,` ~3 ms` on the AWS amd64) is significant - it means we can make tight SLA promises against the warm path.


The are some tradeoffs though. Reusing an existing sentry means processes dispatched into the same warm slot share the sandbox's PID namespace and` /tmp` tmpfs. They do not share memory or file descriptors, and the host-kernel boundary is unchanged. The practical implication is that **per-tenant pool partitioning is mandatory** - a slot can serve many tasks from one tenant but never cross tenants. Memory cost is about 50 MB RSS per warm slot, so total worker memory grows linearly with` slots-per-tenant × active-tenants` .


If a warm slot serves K tasks before recycling:


$


amortized latency


TXT


` amortized_latency ≈ (cold_boot + slot_idle_overhead) / K + exec_cost


≈ 110 / K + 20 ms


`


For K=10, you get ~31 ms p50 latency; for K=100 you approach the warm floor. Pool replenishment happens off the request path in a background goroutine, so the cold-boot cost is shifted from per-request latency to worker memory pressure.


## What stuck in the end


Four insights fall out of the data:


**Pick instance types tuned for single-thread, not core count.** AWS` c7i.xlarge` and` c7g.xlarge` were the best general AWS choices - the high-core-count SKUs (metal and large VMs alike) cold-boot slower - even with the below gVisor patch Go's GC/scheduler overhead on high core counts takes a toll. If you have the choice on Google Cloud, **` c4a` (Axion) is the best mainstream cloud cold-start latency we measured** , beating both AWS Graviton 3 SKUs by` 15-20%` on the same workload.


**Tune THP for the boot path, but change defrag too.** gVisor only applies MADV_HUGEPAGE to its MemoryFile when defrag is always, defer, or never. With the common default,` defrag=madvise` , it disables huge pages because synchronous compaction can make shmem slower than native anonymous memory. So shmem_enabled=advise alone does nothing.


Use` defrag=defer` . It lets kcompactd compact in the background, giving fresh nodes 2 MB pages without blocking faults. With` enabled=always` , this saves about 10 ms from CORE startup; fragmented nodes simply fall back to 4 KB pages.


Avoid` defrag=always` : it adds host-wide fault stalls precisely when memory is fragmented and the risk is highest. Expect roughly 10–15% more RSS per Sentry.


**Patch gVisor's` hostmm` init to be lazy.** The single biggest package init in the Sentry is` gvisor.dev/.../hostmm.init` , which eagerly issues` membarrier(REGISTER_PRIVATE_EXPEDITED)` . That call costs a single` synchronize_rcu` grace period - ~5-7 ms on a 4-vCPU host, ~12-17 ms on a 64-vCPU one - and a full cold boot pays it several times over, once per process in the self-execing` runsc` fork chain.


In systrap and ptrace modes that registration has no consumer at all (only the KVM platform's` UseHostProcessMemoryBarrier` uses it), so wrapping the init body in a` sync.Once` triggered from the getters removes it from the cold path entirely. We measured the patch:` hostmm.init` falls to ~0.001 ms and CORE


drops ~23-27 ms on 4-vCPU hosts and ~56-66 ms on 64-vCPU hosts, with no steady-state regression for workloads that actually call` membarrier(2)` .


**Worker warm pool optimization.** This one worked the best by amortizing fixed cold boot costs over many executions. A warm-pool exec path drops p50 latency from` ~108 ms` (our best fully-patched cloud cold CORE


p50 -` c7i.xlarge` 's 118 ms after the` hostmm` patch, less the ~10 ms THP win) to` ~24 ms` (cloud warm) with a four-and-a-half-times reduction and a` ~1-3 ms` standard deviation. The implementation requires extra-complexity - per-tenant partitioning and an idle-eviction policy but, for our usecase, the tradeoff is worth it.


Beyond that, there are some long-term wins on the horizon that would land us in the` ~20-35 ms` band without the per-tenant pool partitioning constraint that complicates the exec path which we'll talk about in a future post. Stay tuned!


Dmitry Ilyevsky


Co-founder & CTO


Dmitry is co-founder and CTO of Apoxy. He previously built and operated infrastructure at Google, Cruise, and Mux (where he and Matt met).


[GitHub ↗](https://github.com/dilyevsky)


[← Back to all posts](https://apoxy.dev/blog)
