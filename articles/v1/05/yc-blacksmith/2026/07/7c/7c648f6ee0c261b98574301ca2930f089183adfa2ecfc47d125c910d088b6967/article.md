---
schema_version: "1.0.0"
document_id: "7c648f6ee0c261b98574301ca2930f089183adfa2ecfc47d125c910d088b6967"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/why-i-joined-blacksmith-to-work-on-storage-again"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T16:03:31.504310+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:fb8858bf5d9244adce285cf9b834e55e2563921140f28a168b59c3818470c8c8"
---

# Why I joined Blacksmith to work on storage again

I want to tell you about the storage problems hiding inside continuous integration: why they're more interesting and more unusual than they look, and why they're the reason I joined Blacksmith. To explain that, I should start with how I ended up here.


What I look for in a job, more than anything, is to be challenged. I've spent my career chasing the kind of systems problems that don't come with ready answers: storage at Google, on Colossus, the largest multi-purpose storage foundation; an eBPF-based debugger at a startup; and later Datadog, most recently on[production-grounded code optimization](https://www.datadoghq.com/blog/ai/production-grounded-code-optimization/) . The common thread isn't a particular layer of the stack; it's the pull of a problem nobody has a clean answer to yet.


So when I kept hearing that the people at Blacksmith were taking on exactly that kind of problem, it got my attention. I dug in, and found that Blacksmith (which runs continuous integration, the builds and tests that fire on every push and pull request, on its own fleet of machines) is, under the hood, a storage problem at least as much as a compute one. Standing up and tearing down enormous numbers of short-lived virtual machines, and feeding each one the data it needs (source, dependencies, build caches, the results it produces) turns out not to fit the tools you'd normally reach for: high scale, high churn, tight latency budgets. I didn't come for "managed CI." I came for that workload.


I've also observed that most of the industry's storage energy right now goes up the stack: new databases and data layers, increasingly AI-oriented, built on top of the same primitives we've had for years. What pulled me to Blacksmith was the rarer chance to work a layer down: to rethink the storage foundations themselves. Not as a greenfield indulgence, but because the workload here gives a concrete, well-grounded reason to question those primitives. Getting to build new foundations because the workload genuinely demands it is, to me, the most exciting kind of systems work there is.


So let me show you the workload. Here's what a CI job actually does to storage, at the scale we run it, and why the obvious answers don't drop in.


‍


## What a CI job actually does, at scale


Forget, for a moment, any picture of "servers with disks." The unit of work at Blacksmith is a fresh, ephemeral microVM that lives for a handful of minutes. Every CI job boots a clean VM, does its work, and is destroyed. There is no long-lived host quietly accumulating state the way a normal server does. And we run these at an enormous rate: every day we create and destroy on the order of two and a half million per-job volumes, with peaks around a thousand new VMs a minute.


That single fact is the root of everything below: state has to come from somewhere, fresh, every single time, fast. It starts before the job even runs: the machine has to read its own root filesystem to boot, and that same base image is demanded by huge numbers of VMs at once.


Once a VM is up, the work it does is a collection of distinct storage workloads, each with its own character:


- **Fetching the repo.** Almost every job begins by pulling source. At our rate that's a wall of repetitive transfer against the same handful of repositories: the same bytes paid for over and over, often from a distant origin. What each job wants differs, too: a shallow tip here, full history there. The pressure is network throughput and locality, not bytes sitting on a disk.
- **Environment setup, repeated endlessly.** Much like fetching the repo, nearly every run of a given repo redoes the same expensive setup (install the toolchain, the system packages, the dependencies) before any real work starts. The bytes that setup produces are essentially identical from run to run, and yet they're regenerated from scratch each time.
- **Integration tests that stand up dependencies.** Databases, services, fixtures, seed data spun up per run: heavy, transient I/O that is almost entirely discarded when the job ends.
- **Builds that need incremental caching.** Docker layer caches, compiler and dependency caches, build-tool state. These are write-heavy and only valuable if they survive the VM that produced them: the cache has to outlive the machine. And the caching semantics (what to keep, how to key it) live inside the build tool, not in any storage knob.
- **Persisting run artifacts.** The flip side of all that throwaway: the things that genuinely must survive: build outputs, test reports, coverage, logs, images pushed downstream. A minority of the writes, with the opposite requirements: durable, addressable long after the VM is gone, sometimes large, governed by retention rules. The same system has to do "store almost nothing" and "never lose this" at the same time.


Every one of these sits on the critical path of a developer waiting on CI. Each step adds seconds or minutes to every run, thousands of times a day: speed here isn't a nicety, it's the product. That pressure applies to everything below.


## Churn is the overarching challenge


The thread running through all of those workloads is churn. Every job creates its world, writes furiously into it, and throws almost all of it away: an integration-test job spins up databases, services, fixtures, seed data, does its work, and almost none of it should survive. Across the fleet, jobs write around` two petabytes a day` into their per-job volumes and about` 95% of it is discarded` , across ~2.5M volume lifecycles. The sheer rate of create → write → discard is itself the load. Most storage systems are sized for data that accumulates; this evaporates within minutes.


## Scale is also concurrency on shared data


Rate isn't the only axis. A lot of the difficulty is concurrency on data we want to share.


On the read side, picture a popular repository's mirror. At peak, a single cached snapshot can have thousands of jobs reading it simultaneously. Serving that fan-out from one shared copy, fast, is its own problem.


On the write side, the hard part is the incremental updates: state that lives across runs, with every job folding a small delta into it. That state is a large, mostly-static shared base (built dependencies, warmed layers, populated caches), and we want the deltas to land, because the next run is faster if they do. But many jobs produce deltas against the same base at once (a workflow's parallel jobs, or many runs of a hot repo), each an ephemeral VM that vanishes when it's done, with no long-lived owner to coordinate or compact. They collide: a delta often can't cleanly land on a base that several other writers are mutating in the same window, so it's contended, reconciled, or simply lost. The shape resembles one that might be familiar, a mutation log with background compaction, but the economics break: the deltas are tiny and each writer is short-lived, so the log never grows big enough for compaction to pay for itself, and yet the chain depth still accumulates and has to be compacted away.


Concurrency adds a second danger on top of who wins, what's lost: correctness. With many writers on a shared cache, a reader must never observe a half-updated state, no matter how the writes land. So it isn't enough to resolve the collisions; every reader has to be guaranteed a coherent, valid view at all times.


## Sharing is the whole game, and the jobs don't trust each other


Almost every efficiency win above comes from sharing: one base image and one repo mirror served to thousands of VMs, environments deduplicated across runs, caches reused run after run within a tenant. But each job is arbitrary code, alive for only minutes, and it can fail, get cancelled, or misbehave halfway through anything. So the very mechanism that buys the efficiency is the one with the blast radius: corrupt a single shared cache entry and it can silently propagate into thousands of later builds. A store serving one careful writer never has to think about this; shared CI state has to stay correct no matter what any individual job does to it.


## The pooling effect


One more property, and it's the one that turns the difficulty into an asset. A single customer's CI traffic is spiky and bursty (quiet for stretches, then a flood when a big push lands), and the churn only makes that worse. If you had to provision for one tenant's peak, you'd carry a lot of idle capacity. But pool the whole fleet together and those peaks stop lining up: one tenant's burst fills another's lull, and the aggregate load smooths out. Provisioning for peak stops hurting, because the fleet rarely peaks all at once. The same churn and burstiness that make a single workload painful become, at fleet scale, a statistical advantage, and that's a huge opportunity. We've written about the economics of this pooling before, so[I'll just point at it here rather than re-derive it](https://www.blacksmith.sh/blog/the-economics-of-operating-a-ci-cloud) .


Spiky tenants, smooth pool — Blacksmith


# Spiky tenants, smooth pool


Individual CI traffic is violently spiky; summed across the fleet, peaks stop lining up.


1


100


20 tenants


· 0%


of capacity used


The more tenants share the pool, the closer mean use climbs to the capacity you pay for.


## An unusual reliability and durability profile


Two things are true at once here, and they pull in opposite directions.


**It must not fail during a run.** A storage hiccup that fails a job is very bad: CI jobs aren't safely, blindly retryable. They cost real wall-clock time, they can have side effects, and there's a developer blocked on the result. While a job is live, the storage under it has to be genuinely reliable.


But durability comes in tiers. Artifacts must genuinely survive. Caches are worth keeping but reconstructible: losing one costs a slower rebuild, not a lost result. And the vast transient remainder has no durability obligation at all; once the run is done, it can simply cease to exist.


That asymmetry (reliable in the moment, durable only where it counts) is a relaxed constraint, and a relaxed constraint is design space. General-purpose storage is built to never lose a byte, ever. We get to trade some of that permanence for speed and cost, as long as we're solid for the minutes a job is running.


None of these properties is exotic on its own; the difficulty is that they all hold at once. A thin layer of must-keep data riding on mountains of throwaway, a shared base that thousands read and many mutate concurrently, and a must-not-fail-now-but-need-not-last-forever reliability profile: all on a developer's critical path, all at fleet scale. General-purpose storage isn't tuned for that shape, which is why the off-the-shelf answers, natural as they are, fall short of it.


## Why off-the-shelf storage falls short


GitHub Actions has been around for years, and somewhere along the way it became accepted folklore that CI jobs, especially storage-heavy ones, are "just slow." That resignation is exactly the opening. When you actually look, there's enormous headroom to make them fast. A big part of why I'm here is that the problems aren't even fully mapped yet: the work is as much finding the opportunities as building the fixes.


So why don't the obvious answers close that gap? Walk through them.


**A virtual block device per job.** The natural mental model: hand every VM a disk. But that collides head-on with the churn problem. A generic block device assumes a single owner accumulating durable state over time. CI hands you the opposite: a flood of transient writes, plus concurrent mutations to shared cached data from many short-lived writers. A plain block device doesn't, on its own, have an answer for that.


**The tools' own caching.** These tools and CI platforms do ship caches aimed at ephemeral runners: Bazel has a remote cache, BuildKit has registry and` gha` cache exporters, GitHub offers both native and compatible cache backends. They're real, and they help. What they tend to do, though, is hand you the storage problem rather than solve it.


Their native, local caches all assume a machine that sticks around (a git clone kept on disk, BuildKit's layers retained on the daemon, Bazel's on-host cache), which is exactly what an ephemeral VM isn't. The speedup only appears if something reconstitutes that state every run. And where the remote caches exist, they relocate the problem onto you. Here's a concrete observation from production: wiring a tool's content-addressed cache straight to an object store generates a lot of network traffic: every run pulls the cache down and pushes it back up wholesale, even when very little actually changed. That points right at what's missing: more targeted, lazy reads (fetch only what this run touches) and incremental writes (push back only the delta). You still own the backing store, the throughput, the locality, the concurrency, and the eviction. The caching is good and the hooks exist, but using them well is the storage problem, and each tool's cache model is its own.


Incremental builds — Blacksmith


layer objects


build scratch


intermediate FS — discarded with the builder; cache metadata kept


build time


paid this build


Which instruction changed:


reused from layer cache


rebuilt this run


scratch, thrown away


Only the changed layer and everything after it gets rebuilt — the rest ships free from cache.


**A blobstore, for the workloads it's made for** . An object store has a specific purpose, and where that purpose applies (durable, scalable, content-addressable "keep this blob" workloads), it's naturally easy to integrate. But not without caveats: the primitive still has to be wired into CI's semantics, and that wiring is the hard part. You still have to solve concurrent access (parallel jobs racing to write the same logical cache), eviction (the policy that actually matters lives inside the build tool, not in a size knob; set a limit and usage can still run away), keys and addressing, and lifecycle. And granularity bites specifically here: an object store's natural unit is the whole object, so treating a cache as one opaque blob re-incurs that same wholesale-traffic problem when only a fraction changed. The blobstore gives you durable bytes; it gives you none of the CI-shaped behavior around them.


The pattern: even the primitives that fit don't drop in. Each CI workload (source fetch, build cache, test scratch, artifacts) comes with its own retain rate, its own concurrency pattern, and its own native caching model. The work is the integration, not the choice of backend. There's no off-the-shelf answer because the workload itself is the unusual part.


## The problems I keep coming back to


This is the part I'd actually want to talk through over a whiteboard. Each of these is an open problem with just enough texture to show why it's interesting; I'm deliberately not walking through how we solve them. They follow the arc of what a job does: fetch in, assemble the environment, run and write, hand off what matters, and then one problem that cuts across all of it.


### 1. Getting the machine, source, and dependencies onto the host, fast, everywhere


It starts before the job does. The first inbound-delivery problem is getting the VM's root filesystem onto the host so the machine can boot, and that same base image is requested by enormous numbers of VMs concurrently. It's the purest form of the read-fan-out problem: one large, mostly-identical artifact, demanded everywhere at once, on the path before any useful work can begin.


Then the job pulls in everything else. Source is the obvious fetch, but the bulk of inbound bytes is dependencies, gigabytes of them on a typical run, in whatever form they arrive: dependency source, package archives, container images, prebuilt binaries, toolchains pulled on demand. Same underlying shape: large, repetitive, externally-hosted content fetched at the start of nearly every run.


The hard parts:


- **Throughput** . Hundreds of thousands of pulls a day, concentrated on a small set of hot repos and common dependencies. Getting each from its origin every time is a wall of redundant bytes.
- **Locality, at two scales** . The cache has to live near the compute, and within a cluster it still matters whether a copy is on the same host or a hop away. Where do copies live, how do they stay warm, and what happens to a job whose nearest copy is cold?
- **Cheap versus cacheable** . The cheapest pull for one job (only the piece it needs right now) and the most reusable one for a thousand (a complete copy you can serve to everyone) are not the same pull, and they pull in opposite directions.
- **A working set larger than any cache** . Access is heavily power-law: a few repos and dependencies are most of the traffic, with a long cold tail. What to keep warm is a live, ongoing decision, not a static size knob.


‍


### 2. The environment that's rebuilt a thousand times


Once a job has pulled in its source and dependencies, it assembles them into a working environment, and that assembly is the same work nearly every run. Same toolchain, same packages, same dependencies, producing essentially the same bytes over and over, at fleet scale, on the developer's critical path.


The moment you try to keep the warmed-up state instead of recomputing it, you hit a combinatorial blow-up: a distinct warmed environment for every combination of repo, toolchain, version, and config, meaning thousands of large, near-identical environments that overlap by most of their bytes. Storing each as an independent copy is hopeless; the whole point is the overlap. So the open question is: how do you give every run a warm, ready-to-go environment while storing the overlap once, exploiting the massive shared substructure across thousands of near-duplicate environments, and resolving the right one per job, fast?


### 3. Knowing which writes matter, and how durable they need to be


This is the one I spend most of my time thinking about, and it sits right where the job does its real work, between assembling the environment and handing off results.


A job emits a flood of writes, and they are wildly unequal. Most are pure scratch that will be discarded in minutes. A few are gold that must survive. Some matter only until the job ends; others must outlive it. And you often can't tell which is which until the job is nearly done. The problem is that the storage layer is asked to treat all of it identically, paying full durability cost for every byte, when the data's actual value and required lifetime span a huge range.


So: how do you even classify a write's worth and its required durability, ideally online, without the job telling you? Recall the reliability profile from earlier: it must not fail mid-run, yet most of it needn't be durable past the run. How much durability cost should a given write actually incur, and when? What's the right contract for data that's important now but disposable later? Getting that distinction right is the difference between paying to durably store garbage and paying only for what matters.


### 4. Keeping the things that must not be lost: run artifacts


At the end, a job produces the minority of data that is the exact opposite of all that throwaway: it must survive the VM and stay durably retrievable long after the job that made it is gone: build outputs, test reports, logs, images pushed downstream. The same storage stack optimized to forget almost everything has to also guarantee it never loses these.


The interesting questions: how do you move artifacts off an ephemeral VM before it's destroyed, reliably, without slowing the job down? How do you make them durable, addressable, and fast to retrieve from anywhere (locality again) at the volume a busy fleet produces? What are the retention rules, and who pays for the long tail? It's the durability counterweight to the churn story: the bytes where "good enough" isn't.


### 5. Sharing aggressively without letting one job break the rest


This one isn't a stage in the job's life; it cuts across all four above, which is why it comes last. Every efficiency win in this post comes from sharing: one base image and one repo mirror across thousands of VMs, environments deduped across runs, caches reused run after run. But each job is arbitrary code, alive for minutes, and anything can go wrong mid-write. So the shared bytes that make CI fast are also a shared blast radius.


How do you share aggressively for speed while guaranteeing no job's failure can corrupt what the others read? How do you validate that a shared entry is exactly what it claims to be before thousands of jobs build on it? The obvious answer, "just don't share," is the one thing the economics won't let us do.


## What this work actually feels like


A few things make the day-to-day genuinely fun, beyond the problems themselves. Every decision is grounded in production telemetry at fleet scale. We can measure exactly how much each job writes and keeps, so the workload tells us where the problem is instead of us guessing at it.


It's full-stack systems work. One problem stretches from the VM-and-host boundary down through durable storage and up through the product a developer actually uses. You don't get to stay in one layer.


And it's green-field but constrained: real users, real bills, real correctness stakes, with the design space genuinely open. The day-to-day is measuring a real workload, finding where it doesn't fit, and getting to build the thing that does.


## Come work on this


These are the problems I joined to work on: unusual, challenging, and genuinely alluring. If they're the kind of thing you'd want to spend your time on, come find us.[We're hiring storage and systems engineers who want to own a problem end to end.](https://www.blacksmith.sh/careers)


And if you want to see how we're actually solving these, that's a story for another post. Stay tuned.


‍
