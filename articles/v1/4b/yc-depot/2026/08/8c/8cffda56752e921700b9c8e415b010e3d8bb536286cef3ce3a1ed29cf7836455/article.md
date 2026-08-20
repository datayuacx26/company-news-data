---
schema_version: "1.0.0"
document_id: "8cffda56752e921700b9c8e415b010e3d8bb536286cef3ce3a1ed29cf7836455"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/why-i-reimplemented-lvm"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T18:26:53.553735+00:00"
fetched_at: "2026-08-13T18:26:56.978435+00:00"
content_hash: "sha256:99fbad7dcf0570b6d0dc03f8551fffc222e1a6aea01b7e4072dd599ee6f94fb4"
---

# Why I reimplemented LVM (with worse guarantees)

> Wait, you rewrote LVM, and made it worse?!


This is the question I heard in my head when I started reimplementing LVM, which has been rock-solid for two decades. But I couldn't use vanilla LVM2 for our hypervisors running microVMs: it simply does too much and assumes too little about our workload.


## Running microVMs is weird


The whole story starts with us moving towards microVMs on bare-metal hypervisors to run the Sandboxes that host our customer workloads. This means we are running expensive bare-metal machines, where we should not waste a single second between customer workloads - so we went down to sub-second microVM launches.


Doing this sub-second on local SSD is already a nice challenge, but we do this with networked storage with pretty good performance, so we had to (re)invent a couple of tools for ourselves.


**Note** : This move also means that we are auto-scaling expensive bare-metal machines, so every second we spend on booting our hypervisors and getting them prepared to launch microVMs counts.


## What is LVM even doing?


What we love about LVM is that it is super simple and, because the data-plane is the Linux kernel's device-mapper infrastructure, it uses as little magic as possible with good performance.


However, LVM is meant to be rather safe and give guarantees, such as your data not vanishing if you crash while creating and deleting volumes. Even more, if you accidentally delete, you have a really good chance of recovering it!


This is at the cost of global locking (well, volume group-wide), which is acceptable at one operation a week. But an operation holding the VG-wide lock for roughly 100ms on an operation you want to do up to 200 times a second in parallel is hardly doable. It makes you wonder if you are even using the right tool.


### LVM guarantees vs guarantees we actually need (and want to afford)


Locking and data-safety are really good features, but if your workload fails performance targets because of it, you cannot use the solution. This was the case for us with LVM.


One place where we really want a fast solution is assembling block devices to be consumed by our hypervisor to launch microVMs. Basically, it has to take network block devices from a remote host, add dynamically sized caches on top of them, and produce a single block device.


Zooming into this post's most important bit: we need a very quick way to allocate variable amounts of RAM from a big pool, then create a cached block device via device-mapper.


Crash safety does not give us a lot in this specific scenario. A hypervisor crash results in both the failure of an ephemeral microVM workload (therefore it is discarded) and the loss of volatile memory (which results in data loss anyway), so there is either no value or no data to protect from crashes.


We need pretty good performance and high reliability, so we use the same device-mapper infrastructure that is behind LVM. We just assemble it differently.


## What the solution looks like


As the first layer, we have networked storage, that provides us various block devices. This is currently out of scope for this post, but we have our own storage agent and storage daemon which materialize variable sized block devices.


The second layer is the memory block device, which the Linux kernel provides. There are multiple mechanisms and patterns on how to achieve this, but the end result is essentially the same: a pool of memory, managed by our storage agent.


Once we have this memory pool, an in-process allocator in the storage agent manages it. The allocator holds the mappings, the slices, and everything else in memory, and can handle allocations of custom sizes and shapes.


Because this critical path is in-process, and we don't have to open a disk or do any "real" IO operation, the allocation is incredibly fast. This allocation is the only point where we need any kind of locking, and it is only for an in-memory mapping.


Since the mapping ensures that there is no race on using the memory block device, we can enjoy the kernel's asynchronous device-mapper creations, resulting in roughly 100x speedup compared to using LVM for the same use case.


Even though the first prototype still used` dmsetup` , it was already fast enough for our boot-time target. It just wasn't exactly elegant.


### What if storage agents crash?


This is the cool bit: we do the smart part of our control plane (allocation, etc.), but we don't directly "program the data plane". The kernel has the important part of our mappings. Our agent saves this mapping to the disk periodically, but we can rebuild it and validate it by reading the device-mapper mapping in the kernel. This means that our agent can crash at any time without impacting running workloads. Even if recovery fails, the existing device-mapper devices keep working.


### Why not use (insert other storage tech here)?


Before this whole thing, we evaluated multiple storage solutions (which are all amazing in their own way!), but none were a good fit. This blog post is focusing on one part of our storage stack, but I want to zoom out a bit further.


Our desired architecture and our performance targets made ZFS a bad choice for us, even with tuning. This was true for Btrfs and Stratis as well, and neither can provide block devices. While loop devices exist, we just didn't want to go in the filesystem direction.


A better alternative is for us to own even more of our storage stack, but that is a topic for another post.


## What did it cost?


Development was much faster than I personally anticipated, but what it cost us was debugging some very quirky scenarios related to locking, asynchronous work (us, not the kernel), orchestrating with network block devices (which might randomly take longer to materialize locally), and just all-around weird behavior. However, it became rather stable rather quickly.


## Was it worth it?


Yes! And I think it is sometimes worthwhile to reinvent foundational technologies, especially when it opens up new avenues.


For example, we would otherwise need to spend multiple times this 100ms lock, just to spin up a microVM in barely under a second. The cool thing here is that we didn't reinvent the whole stack, we just replaced the very slow control-plane in one place, where we handle extremely scarce resources.


This wasn't where we stopped. Doing fewer things is *faster* , but doing nothing is *fastest* . We applied that idea to a few other parts of the launch path, but those deserve their own posts.


The next step is to own even more of our storage stack.


## FAQ


Why is LVM too slow for launching microVMs?


We have very tight boot time targets launching our microVMs, so we have to weigh every millisecond. LVM takes a volume group-wide lock for volume operations, and in our environment that lock is held for roughly 100ms. At one operation a week that is invisible. When you want to do up to 200 operations a second in parallel to launch microVMs, the serialization on that single lock is what breaks your latency target, not the data path underneath it.


What happens to running microVMs if the storage agent crashes?


They keep running. The kernel holds the part that matters, which is the actual device-mapper mapping, so devices that are already assembled keep serving IO regardless of what our agent is doing. What you do lose while the agent is down is the control plane, so nothing new gets allocated until it comes back. The agent writes its mapping to disk periodically and can also rebuild and validate it by reading the device-mapper state back out of the kernel. Even if that recovery fails, the devices that are already assembled keep working.


Can you use device-mapper without LVM?


Yes. Device-mapper is a kernel facility you can program directly through its ioctl interface, either with a tool like` dmsetup` or from your own process. LVM is a control plane on top of it that adds metadata, locking, and recovery. That split is what made this possible for us: we kept the same kernel data plane LVM uses and only replaced the control plane above it. Our first prototype drove device-mapper with` dmsetup` and was already fast enough for our boot-time target, just not elegant.


When would giving up LVM's crash safety be the wrong call?


Whenever the data is supposed to survive the crash. Our tradeoff works because a hypervisor crash takes out both the ephemeral microVM workload and the volatile memory backing its device, so there is nothing left worth protecting. If your volumes hold persistent data, or you would want to recover a volume you deleted by accident, LVM's metadata and locking are doing real work for you and are worth the 100ms.


Héja Péter (Vau)


Staff Infrastructure Engineer at Depot
