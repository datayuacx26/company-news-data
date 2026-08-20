---
schema_version: "1.0.0"
document_id: "7c58f1408ce65e3b767b030945472f08612d5c798dfa684d6fba97d50b00a29e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/differences-between-qemu-and-cloud-hypervisor"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:54:15.153981+00:00"
content_hash: "sha256:717f24c269b4855558c507fecbfcb6f5e564d0df4bf263709cd27ac155877a14"
---

# The differences between QEMU microvm and Cloud Hypervisor

## Prologue


I bet you've heard the term microVM recently—it goes hand in hand with CI and sandboxes, both hot topics™ of 2026. Say you want to build a cool CI project with super fast-booting VMs. How do you choose from the myriad of available technologies? We're not here to tell you which one to choose, but we can at least try to explain some differences between QEMU microvm and cloud-hypervisor. As the fundamentals and goals are quite similar, we'll concentrate on the most notable differences that might be interesting from the perspective of building a platform.


## What is a microVM?


(If you're already familiar with microVMs, feel free to skip to the next section.)


microVMs are lightweight virtual machines with strong isolation guarantees, a reduced attack surface, and usually pretty fast boot times. In Linux-land, the most notable players are all KVM-based. The VMs are typically managed by a VMM (Virtual Machine Monitor) such as Firecracker or cloud-hypervisor. There are a few performance tricks that make microVMs snappier compared to full virtualization:


- **Paravirtualization** , where the guest OS is aware of being virtualized and communicates with the hypervisor via optimized API calls instead of emulation.
- **Minimalist kernels** , compiled only with the necessary components so they don't need to load extra modules at boot.
- **Lightweight init** , employing a specialized, low-footprint init script instead of something like systemd.


Due to the lower resource overhead, you can achieve higher density by packing more microVMs on a single host compared to traditional VMs, especially with CPU overcommit. These properties make microVMs a good fit for short-lived, ephemeral workloads.


## Comparison


### Background


cloud-hypervisor is a modern VMM written in Rust. It's inspired by Firecracker and even shares some of its components. As a relatively new project focused on cloud workloads, it has no business supporting legacy CPU architectures and devices. cloud-hypervisor runs on KVM (Linux) and Microsoft Hypervisor, but has no macOS support.


The QEMU microvm machine type is also relatively new and also inspired by Firecracker. It's powered by KVM and runs exclusively on Linux. Notably, it doesn't support PCI-only devices, preferring virtio-mmio as its transport instead. More on the drawbacks later.


### API support


cloud-hypervisor has a rich, user-facing[REST HTTP API](https://raw.githubusercontent.com/cloud-hypervisor/cloud-hypervisor/master/vmm/src/api/openapi/cloud-hypervisor.yaml) . In fact, this is the primary way to manage VMs and interact with the VMM. When a cloud-hypervisor instance is started, the API is made available via either a UNIX socket or a file descriptor. As you may have noticed, there isn't a higher-level abstraction or control plane that lets you list running VMs, observe their statuses, etc. Each invoked binary corresponds to a single cloud-hypervisor process.


See the[cloud-hypervisor API docs](https://github.com/cloud-hypervisor/cloud-hypervisor/blob/main/docs/api.md#rest-api-endpoints) for the full endpoint list.


The QEMU microvm machine type doesn't expose a REST API, but it does offer several other ways to interact with the VMM. One notable mention is[QMP](https://wiki.qemu.org/Documentation/QMP) , a JSON-based protocol over a UNIX socket. As an alternative, you might use libvirt, an open-source VM management tool and API that supports various hypervisors, including QEMU. The bad news is that microvm compatibility varies greatly depending on the Linux distribution you're using, so you'll probably end up compiling your own libvirt version. Unfortunately, libvirt uses its own RPC-based binary format instead of REST and relies heavily on XML for configuration, which might discourage some.


For more information, see the[API reference for libvirt](https://libvirt.org/html/index.html) .


### GPU passthrough


GPU passthrough is a way to make a physical GPU attached to the host available inside the VM. The VM should see the device as if it were native.


cloud-hypervisor uses the VFIO (Virtual Function I/O) framework, implementing the vfio-pci device to allow attaching compatible devices to the VM. (It doesn't have to be a GPU: it could be an SD card or a network card as well.) TL;DR: cloud-hypervisor supports GPU passthrough.


2026 is all about GPUs so all VMMs must support GPU passthrough, right? Well, not quite. As mentioned earlier, QEMU microvm doesn't support PCI devices, which means direct GPU passthrough isn't possible out of the box. (However, since QEMU 5.2, PCIe is configurable for microVMs as well.)


### Snapshotting


The essence of snapshotting is capturing a VM's state and dumping it to disk for later use. The saved data typically includes CPU register state, disk contents, device states, and dirty (in-use) memory pages. This state can then be restored at a later point.


cloud-hypervisor supports snapshotting via its API. First, the VM needs to be paused, then the snapshot can be taken. When restoring a VM from a snapshot, it needs to be explicitly resumed. The only caveat is that VFIO device states are not included in the snapshot. cloud-hypervisor allows snapshots to be taken regardless of the disk format, including raw disks. Watch out though, snapshotting isn't guaranteed to work across different versions.


Snapshotting should work with QEMU microvms as well, using the` savevm` and` loadvm` commands. These are exposed to the user via the QEMU monitor or QMP. At least one writable qcow2 disk is needed, as the entire VM state is dumped into the disk image. Alternatively, QEMU's` migrate` command can dump the full VM state (CPU, RAM, devices) to an external file, which works regardless of disk format, though the disk image must be managed separately in that case.


### Live migration


Live migration is the act of moving a running VM from one process to another on the same host, or from one host to another entirely. All processes and connections within the VM should remain intact, and the source VM should gracefully stop once the migration completes. For this to work, both environments need to be identical. The migration is typically performed by iteratively copying dirty memory pages to the destination and switching over once the delta is small enough. Live migration is useful when a host needs to be decommissioned but the workloads must keep running (like for mission-critical services).


cloud-hypervisor supports live migration. As usual, the functionality is exposed via its API.


Nothing in the documentation explicitly says that QEMU microvm doesn't support live migration, so there's reason to believe it does. However, live migration with ongoing external IO isn't supported for the microvm machine type.


For both cloud-hypervisor and QEMU, live migration isn't guaranteed to work across different versions, which is a small price to pay for such a useful feature.


### Hotplug


Hotplugging is the act of adding, removing, or modifying VM resources and devices without requiring the VM to be stopped or rebooted. Just as plugging in a USB device makes it instantly appear on your machine, VMs should be no different, right?


Well, cloud-hypervisor supports hotplugging x86 vCPUs, PCI devices, and memory resizing. When adding extra vCPUs to a running VM, they need to be "onlined" on the guest side. Multiple memory hotplugging methods are supported, some of them semi-manual (see the[documentation](https://github.com/cloud-hypervisor/cloud-hypervisor/blob/main/docs/hotplug.md#memory-hot-plug) for details). Adding or removing PCI devices (including GPUs) is handled by the guest with no manual work required.


Unfortunately, QEMU microvm doesn't support hotplugging at all.


## Conclusion


To make everything we've covered more digestible, let me summarize it in a table.


cloud‑hypervisor QEMU microvm Why care?


API support ✅ ✅ ⚠️ A good API can reduce the amount of "glue" needed to wrap the VMM for your platform.


GPU passthrough ✅ ⛔ Having GPU access from a VM is how you make customers happy.


Snapshotting ✅ ⚠️ ✅ ⚠️ Providing a way for customers to save CI workloads for later reuse sounds like a cool feature!


Live migration ✅ ⚠️ ✅ ⚠️ One of your hosts now only runs a handful of workloads, otherwise it could be scaled down. You could save margin by migrating those workloads to denser machines.


Hotplug ✅ ⛔ Dynamically changing VM resources? Reclaiming host memory? Attaching devices on-the-go? Deal me in!


There's no intention to declare a winner here. You might have a very good reason to choose either of these amazing technologies. For example, if you run virtualization across multiple platforms and guest OSes, and you don't need GPU passthrough or hotplugging, it might make sense to go with QEMU, which has the same API and shenanigans as the rest of your stack. As with everything in life, collect information, capture your requirements, and make an educated decision.


If you're interested in which VMM we chose, stay tuned! Join our[Discord Community](https://discord.gg/MMPqYSgDCg) to get all the latest.


## FAQ


Which VMM should I choose for a CI platform?


It depends on your specific requirements. If you need GPU passthrough or dynamic resource hotplugging, cloud-hypervisor is the better choice. However, if you don't need those features and you're already knee deep in the QEMU ecosystem, microvm fits into that existing workflow. Consider your must-have features first, then evaluate the trade-offs from there.


What's the difference between QEMU microvm and regular QEMU?


QEMU microvm is a stripped-down machine type inspired by Firecracker. It removes legacy device support, uses virtio-mmio instead of PCI, and focuses on fast boot times for cloud workloads. Regular QEMU supports a much wider range of hardware and use cases, but microvm trades that flexibility for simplicity and speed.


Can I migrate a running VM from cloud-hypervisor to QEMU or vice versa?


No, you can't live migrate between different VMMs. Live migration requires identical environments on both source and destination, which means the same VMM version. If the VM parameters are supported by both VMMs, you could snapshot a VM with one VMM and restore it with another, but that's not a live migration and would require careful handling of the disk image and state formats.


## Related posts


- [How we launch GitHub Actions runners in 5 seconds](https://depot.dev/blog/github-actions-breaking-five-second-barrier)
- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [GitHub Actions Runner architecture: The listener](https://depot.dev/blog/github-actions-runner-architecture-part-1-the-listener)


Peter Hasko-Nagy


Staff Engineer at Depot
