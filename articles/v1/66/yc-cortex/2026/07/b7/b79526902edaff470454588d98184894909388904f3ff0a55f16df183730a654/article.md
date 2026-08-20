---
schema_version: "1.0.0"
document_id: "b79526902edaff470454588d98184894909388904f3ff0a55f16df183730a654"
company_key: "yc-cortex"
company: "Cortex"
source_id: "yc-cortex-news-import-d57bc7656b70"
canonical_url: "https://builders.cortex.io/blog/sandboxing-agents-part-2/"
published_at: "2026-07-23T14:30:00+00:00"
first_seen_at: "2026-07-23T15:08:04.869939+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:6c272b140a0b9e929f221fd7bdde6000fa7d6462c27cda48efffb5d17fcae8b2"
---

# Snapshots, copy-on-write, and the economics of agent sandboxes

In[Part 1](https://builders.cortex.io/blog/sandboxing-agents-part-1/) , we built one Linux machine for one agent. But how do we launch a thousand of them? Or a million of them?


As we built out our software factory,[engrams](https://builders.cortex.io/blog/software-factories-are-operating-models/#devlogs-to-come) , we realized that the economics of running thousands of isolated agents gets out of control pretty quickly.


We landed on some pretty clever techniques to minimize cost which we’ll cover in this next part.


Let’s get into it!


## How “micro” VMs become expensive


At first, Firecracker, our VMM of choice, made this look cheap. It has very little overhead, boots quickly, and exposes only a minimal set of virtual devices.


…Until we ran real agents inside it!


Once an agent started a compiler, language server, browser, and build daemon, the guest stopped looking so micro. A 4 GiB guest could pull several GiB of pages into host RAM regardless of how small the Firecracker process was at startup.


We hit the same problem with disk. Copying a 40 GiB ext4 image for every agent would be absurd when most changed only a few files.


A minimal microVM ≠ a minimal *workload* .


## North Star


With` engrams` we have two competing goals in tension with one another:


1. We want to minimize cost
2. We want to maximize the number of possible concurrent sandboxes


Let’s talk about cost first.


As a simplification, let’s think of fleet cost as a linear combination of CPU, memory, and disk hours for the number of fleet nodes (` n` ):


```text
cost = t(α * vCPU + β * RAM + γ * Disk) * n
```


Sandbox placement onto host nodes is similarly simple: each host has finite vCPU, RAM, and disk, and each sandbox requests some amount of all three.


I’ve whipped up an interactive visualization to illustrate the problem: schedule “workloads” on the fleet and see how quickly we exhaust the fleet and start to incur costs:


**node_packing** / autoscale ≤ 3


** cost clock running


autoscaler


**1 / 3 nodes active**


host capacity


**16 · 64 · 400**


vCPU


**0 / 16**


0% allocated


Memory


**0 GiB / 64 GiB**


0% allocated


Disk


**0 GiB / 400 GiB**


0% allocated


running workloads


**00 active**


No agents running in this fleet.


fleet cost


**$0.00**


1 node · $0.80/hour


00h 00m · 1 sec = 1 hour


add workload


Every active node adds $0.80/hour to the fleet cost, whether it is empty or full.


We get anywhere between 9 and 24 parallel agents running until we saturate the fleet! You can of course autoscale to oblivion, but cost explodes very quickly.


Not great, but we can do better.


## Why don’t you just kill them when they’re done?


Our first obvious option was to treat these sandboxes as ephemeral.


We could give them a maximum lifetime, choose a harness or prompt engineer agents to one-shot tasks, and destroy the sandbox when the task completed.


That would give us the same contract as CI runners like GitHub Actions: finite tasks with hard timeouts.


But it’s not always clear what “done” even means: if Claude Code stops to ask me a question would the session be reaped? Or if I want to[migrate the Bun codebase to Rust over the course of 11 days](https://bun.com/blog/bun-in-rust) should that task die after 24 hours?


This approach worked for[Stripe](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents) , because they explicitly designed their agents for one-shot tasks.


With` engrams` we wanted to be completely harness agnostic and mirror the experience of running your favorite harness on your own computer, where you can return to a session and pick up exactly where you left off months later.


Even better than just recovering the conversation, why can’t we recover the entire state of the agent, including its memory, disk, and running processes?


## Snapshotting sandbox state


We figured that agents spent most of their time sitting around for the user. We wanted to only pay for *parallel* sandbox resources while still maximizing the number of possible *concurrent* sandboxes.


So why not just snapshot and tear it down to free host resources? We could capture a sandbox, tear it down to free the host, then restore it when the user returned.


To make that work, we needed to capture three parts of a Firecracker microVM:


1. CPU and device state
2. Guest memory
3. Guest disk


Firecracker already captures the first two. It serializes the vCPU, KVM, and emulated-device state into a small opaque file (` state.bin` ) and writes guest memory into a separate file (` mem.bin` ). And disk is already represented as a file, so we could just copy it.


That gave us a capture flow that looked like this:


Reclaim host resources by snapshotting sandbox state


To resume a sandbox, we reverse the process:


Turn a stored snapshot back into a running sandbox


We started with this approach which worked reasonably well for a proof of concept, but fell flat pretty quickly… The combined snapshots were MASSIVE!


Just take one sandbox that has 16 GiB of requested RAM and 128 GiB of requested disk. In the worst case we’re shuttling ~144 GiB of data in and out of blob storage, which is a non-starter in terms of latency and cost of keeping long-lived sandboxes.


## Content-addressable storage / copy-on-write


We quickly realized that a 128 GiB virtual disk doesn’t necessarily contain 128 GiB of unique data.


Most of it comes from the original image: the operating system, compilers, package caches, and whatever else was installed before the sandbox booted. Every sandbox created from that image starts with exactly the same bytes. Even after an agent has been working for a while, it usually changes only a small fraction of the disk.


We were duplicating all this effort and paying for it in storage and bandwidth! We needed a better way to store and transfer disk state.


We settled on splitting the disk into fixed-size 16 MiB chunks.


We hash each chunk with SHA-256 and use the hash as its address in storage. Identical chunks that produce the same hash are *stored once* . We skip completely empty (zeroed) chunks.


Then we record how the chunks fit together in a small manifest:


```text
offset        chunk
0 MiB         sha256:a8f1...
16 MiB        sha256:4c2e...
32 MiB        sha256:91bd...
...           ...
```


Now a 128 GiB disk is only a list of offsets and hashes. The chunks are immutable, so every sandbox from the same image can share them.


When we create a sandbox, we give it a manifest pointing at the base image’s chunks. We copy a metadata object, not 128 GiB of disk.


We bridged our content-addressed storage with Firecracker using Linux’s *Network Block Device (NBD)* protocol. NBD lets userspace processes serve reads and writes for a Linux block device.


We wrote a small NBD server that translates reads and writes into chunk fetches and uploads. We attached it to Firecracker, and the guest still saw an ordinary ext4 disk, but was secretly our server doing the translation behind the scenes:


On a read, our NBD server translates the byte offset into a manifest chunk. It serves the chunk from local NVMe when possible and fetches it from blob storage on a miss. We take advantage of fast NVMe by treating it as an LRU cache for hot chunks, never exceeding host capacity, and falling back to slow blob storage instead of outright failure.


We no longer assemble the whole disk before boot — we fetch only what the sandbox reads.


Reading a chunk through NBD


The guest issues a normal ext4 read to an NBD block device. The NBD server resolves the byte offset through the session manifest, checks the host NVMe cache, and fetches the immutable chunk from blob storage only on a cache miss.


READ REQUEST


` offset 48–64 MiB`


GUEST


**ext4 read** ordinary disk I/O


BLOCK DEVICE


**/dev/nbdN** same disk the VM sees


MANIFEST


**offset → hash** 48 MiB → sha256:B


HOST NVME


**chunk cache** **hit** → return immediately


ON CACHE MISS


**blob storage** fetch immutable chunk B


←


` chunk B` Only the requested 16 MiB region returns to the guest


A virtual-disk read resolves through metadata first. Only the requested chunk is fetched, and a cache hit never reaches blob storage.


For writes, we added copy-on-write. The first write to an immutable base chunk creates a private, dirty copy for that sandbox, leaving every other sandbox untouched.


That means two sessions can begin with the same three chunks, and a write to chunk` B` in session 1 would diverge only that chunk:


Chunk-level copy-on-write


The base image and two sessions initially reference shared immutable chunks A, B, and C. After session 01 writes to B, only its middle manifest entry points to a new private chunk B prime. The base image and session 02 continue to reference the original B.


Three manifests, four stored chunks. Only session_01's write creates new data.


When it’s time to snapshot, we hash and upload the dirty chunks, then publish a new version of the session’s manifest. Untouched offsets keep referencing the shared base chunks. A session that dirties twenty chunks adds at most 320 MiB of new disk data, even if the guest sees a 128 GiB disk.


That improved both cost and latency:


- On suspend, we store the base disk once plus each session’s unique changes
- On resume, we load a small manifest and fetch chunks on demand, so startup work is tied to the sandbox’s working set rather than its internally advertised disk size


That takes the 128 GiB disk out of our worst-case 144 GiB snapshot.


But we still have a 16 GiB` mem.bin` to deal with! Fortunately, memory has the same useful property: most sandboxes begin with the same bytes, and only some of those bytes diverge over time.


## Copy-on-write memory with` MAP_PRIVATE` and` userfaultfd`


At this point we had taken the 128 GiB disk out of the snapshot. Memory was harder: it had no NBD boundary where a manifest could resolve each read. Every instruction could touch it.


We started by content-addressing memory in 512 KiB chunks. That deduplicated storage, but rebuilding a 16 GiB file on every resume kept most of the latency and RAM cost. We needed the bytes to stay shared while VMs ran.


So we gave each host one sparse shared-memory file for an image’s canonical memory: the *base shm* . We mapped it into each sandbox with[MAP_PRIVATE](https://man7.org/linux/man-pages/man2/mmap.2.html) , letting clean pages share physical page-cache backing while writes got private pages through kernel copy-on-write.


We keep the base shm sparse by filling it on demand with Linux[userfaultfd](https://www.kernel.org/doc/html/latest/admin-guide/mm/userfaultfd.html) . On a fault, we compare the session and image manifests:


1. For a canonical chunk, we populate the base shm and map it with[UFFDIO_CONTINUE](https://man7.org/linux/man-pages/man2/UFFDIO_CONTINUE.2const.html) , preserving sharing across sandboxes.
2. For a changed chunk, we fetch it from the session snapshot and install it privately with[UFFDIO_COPY](https://man7.org/linux/man-pages/man2/uffdio_copy.2const.html) .


GUEST MEMORY BACKING


` UFFDIO_CONTINUE`


session_01


session_02


PHYSICAL BACKING


base shm


private


A


A


A


·


B


B


B


·


C


C


C


·


D


D


D


·


UFFDIO_CONTINUE


Both sandboxes map page B from the same base-shm page-cache entry.


**35%** ΣPSS / ΣRSS


three guests · 33% is ideal sharing


**49 MiB** physically allocated


4 GiB apparent base shm · first canary


Canonical pages share one physical backing; only writes and restored divergence become private.


We could have used` UFFDIO_COPY` everywhere, but every restored page would be private.` UFFDIO_CONTINUE` preserved the sharing we built the base shm for.


We fetch the surrounding 512 KiB chunk on each fault to amortize I/O, while copy-on-write stays page-granular. On capture, we keep the hashes of unchanged chunks and created new ones for changed chunks. An inactive sandbox becomes a manifest plus unique chunks, not another 16 GiB file.


Through all this effort, we’re paying for, both in active sessions, as well as snapshotted sessions at rest, only the *base snapshots* of workloads, plus memory + disk *divergences* only.


## From resource density to session density


Once this worked, we could revisit our original capacity model. It applied while an agent worked, but agents spent much of their time waiting for people, network calls, or their next task.


We had given one retained session two physical shapes:


- Running: vCPU, private memory pages, and an active disk working set
- Inactive: a small CPU/device-state file, two manifests, and unique memory and disk chunks


18 RETAINED · 6 ACTIVE


**3 NODES BILLING**


retained


**18** sessions


running


**18** microVMs


host cost


**$2.39** per hour


node_01


$0.80/h


01


02


03


04


05


06


node_02


$0.80/h


07


08


09


10


11


12


node_03


$0.80/h


13


14


15


16


17


18


DURABLE SNAPSHOTS


idle VMs still occupy hosts


·


·


·


·


·


·


·


·


·


·


·


·


** active


** idle but resident


** snapshotted


Total session count stays constant. Snapshotting lets host count follow active concurrency instead.


Now we could capture and evict idle sandboxes, then let the autoscaler remove empty hosts. We could size for active work, not every retained session.


And the economics of the fleet cost improved significantly! Thinking through where we started, we had:


```text
cost ∝ α(requested vCPU) + β(requested RAM) + γ(requested disk)
```


Now it looks more like:


```text
cost ∝ α(requested vCPU) + β(base shm + dirty RAM) + γ(base disk + dirty disk)
```
