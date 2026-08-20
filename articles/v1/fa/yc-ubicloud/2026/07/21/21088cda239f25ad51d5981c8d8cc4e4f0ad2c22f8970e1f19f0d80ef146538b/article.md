---
schema_version: "1.0.0"
document_id: "21088cda239f25ad51d5981c8d8cc4e4f0ad2c22f8970e1f19f0d80ef146538b"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/cloud-virtualization-red-hat-aws-firecracker-and-ubicloud-internals"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:5e9496f3abe0115238229b27360d04f2c658d31e46e37f324f3964cde6bc0eae"
---

# Cloud virtualization: Red Hat, AWS Firecracker, and Ubicloud internals

## Cloud virtualization: Red Hat, AWS Firecracker, and Ubicloud internals


January 24, 2025 · 8 min read


Ozgun Erdogan


Co-founder / Co-CEO


VMs are much harder to understand today than they were back in 2009. Linux provides many building blocks for virtualization, but only a select few kernel engineers know how to stitch them together.


Further, finding a good design document that describes VM tech in 2025, in depth and breadth, is even harder. We think this is because you need to:


- Know about dozens of complex Linux projects that interact with each other.
- Combine these projects to achieve a multi-dimensional goal - operational and security isolation across CPU, memory, disk, and network.
- Compare tradeoffs between different projects and VM solutions. This is hard given the problem space’s combinatorial complexity.


So, we sifted through dozens of manual pages, Linux projects, blog posts, and papers. We then compiled our understanding into this blog that describes four reference architectures - Red Hat, AWS Firecracker, Ubicloud, and AWS Nitro. This document helped our team understand in a “concise” way how people put together decades of work in the virtualization space. Hopefully, it will help you too.
‍


Some Terminology ‍Red Hat Reference Architecture ‍AWS Firecracker ‍Ubicloud Compute ‍AWS EC2 Nitro ‍Conclusion ‍Key References


### Some Terminology


There’s quite a bit of confusion around the terminology used for virtualization solutions. So, we’ll share *our* terminology first. We consider a virtualization solution to include a hypervisor kernel, virtual machine monitor (VMM), and device drivers. For brevity, we may refer to the hypervisor kernel just as the hypervisor in this blog post.


The bare-metal hypervisor runs as part of the host OS in privileged mode and has access to the underlying hardware. The VMM also runs on the host OS, but typically in non-privileged mode. The combination of the hypervisor and VMM allocate resources to VMs (guests). Guest OS is the OS installed inside a VM.


VMs are used to isolate workloads from one another; and workload isolation comes in two types.


‍ *Operational isolation* ensures that one VM can’t cause another one to run more slowly. For example, if one VM uses too much CPU, this shouldn’t impact the performance of other VMs running on the same host. This problem is also known as the noisy neighbor effect.


‍ *Security isolation* ensures that one VM can’t access, or infer, data belonging to another VM. This includes preventing privilege escalations, so that a customer can’t access any information outside of their VM boundary.


Security isolation also prevents information disclosure side channels, where sensitive information leaks through unintended pathways. For example, hyperthreading allows two threads to execute on the same physical core. The L1 and L2 caches are often shared between threads running on the same core; and an attacker can exploit this by observing cache access patterns.


### Red Hat Reference Architecture


Red Hat’s “[All you need to know about KVM userspace](https://www.redhat.com/en/blog/all-you-need-know-about-kvm-userspace) ” is one of the most referenced articles on the internet about open source virtualization. The article also describes RedHat’s reference VM architecture.


Linux KVM, QEMU, and libvirt form the backend of Red Hat’s virtualization stack.


‍ **KVM (Kernel-based VM)** is the core virtualization infrastructure in the kernel. KVM also acts as an interface between the hardware and QEMU. It’s responsible for:


- Hardware virtualization interface: Leverages CPU features (Intel VT-x or AMD-V) for hardware assisted full virtualization. Provides the mechanism to create and manage VMs through an interface to QEMU
- CPU and memory virtualization: Ensures isolation and efficient use of these resources
- Other kernel level features: These include handling VM exits, VM scheduling, translating and routing interrupts to virtual devices. They also include exposing certain kernel level features to QEMU, such as memory management and process scheduling


**QEMU (Quick EMUlator)** operates in user space. It acts both as a device emulator and virtual machine monitor (VMM).


- Device emulation: Emulates the hardware that the guest OS will interact with. This includes disk drives, network interfaces, and GPUs
- VM creation and management: Initializes and runs VMs. Set up the environment for guest OS to run
- Performance optimizations: When used with KVM, QEMU can pass through hardware calls directly to KVM


Using a hypervisor kernel (KVM) along with a VMM (QEMU) in user space is quite typical. For QEMU, the Red Hat architecture diagram doesn’t specify how block devices are emulated.[Subsequent blog posts](https://www.redhat.com/en/blog/virtio-devices-and-drivers-overview-headjack-and-phone) from Red Hat indicate the use of **virtio** for this purpose.


The **libvirt** (not to be confused with virtio) part of this architecture is more Red Hat specific. Libvirt is traditionally used to help with managing VMs across various virtualization technologies, including KVM, Xen, VMware ESXi, and Hyper-V.


Red Hat expands on that use and uses libvirt as a *jailer* . In this architecture, the QEMU process handles input and commands from the guest and it’s exposed to potentially malicious activity. Libvirt isn’t visible to the guest, so it’s the best place to confine QEMU processes. SELinux and file system permissions further help in restricting access to processes and files. Together, these technologies seek to ensure that QEMU can’t access resources from other VMs.


KVM, QEMU, and libvirt from the backbone of Red Hat’s reference architecture. cgroups and nftables are popular Linux kernel projects that are used for workload isolation, so it’s also worth explaining them here.


- **cgroups (control groups)** limits resource usage for a group of processes. These resources include CPU, memory, disk I/O, and network bandwidth. As such, cgroups helps with operational isolation.
- **nftables** filters and classifies network packets. This helps in providing firewall-like functionality and restricting network access to the host. You can think of nftables as a way to achieve security isolation, but from external resources rather than internal VMs running on the host.
- For security isolation across VMs on the same host, Linux network namespaces and seccomp-bpf are typically used. These aren’t called out in Red Hat’s architecture, but we’ll talk more about them below.


### AWS Firecracker


[AWS Firecracker](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf) is an open source VMM specialized for serverless workloads. It’s arguably Amazon’s most influential open source contribution.


Firecracker aims to provide VM-level isolation guarantees *and* solve three challenges associated with virtualization. These are: (a) VMM and the kernel have high CPU and memory overhead for VMs, (b) VM startup takes seconds, and (c) hypervisors and VMMs can be large and complex, with a significant attack surface. They are also typically written in memory unsafe programming languages.


AWS solves the challenges by keeping Linux *KVM* , but swapping *QEMU* with a super lightweight alternative called Firecracker, written in Rust. In particular, Firecracker provides:


- Device emulation for disk, networking, and serial console (keyboard)
- REST based configuration API to configure, manage, start and stop MicroVMs. This replaces some of the functionality offered by *libvirt*
- Rate limiting for network and disk. Can configure throughput and request rates. For this Firecracker implements its own solution for simplicity, rather than using *cgroups*


For network and block devices, Firecracker uses *virtio* . Virtio provides an open API for exposing emulated devices from hypervisors. Virtio is simple, scalable, and offers good performance through its use of paravirtualization.


On the networking side, Firecracker uses a TAP virtual network interface and encapsulates the guest OS (and the TAP device) inside their own *network namespace. ‍* For storage, AWS chooses to support block devices, rather than filesystem passthrough, as a security consideration. Filesystems are large and complex code bases, and providing only block IO to the guest protects a substantial part of the host kernel surface area.


Finally, Firecracker also has a jailer around it to provide an additional level of protection against unwanted VMM behavior (such as a bug). The jailer implements a restrictive sandbox around the guest by using a set of Linux primitives. These include chroot, pid, and network namespaces. The jailer also uses *seccomp-bpf* to whitelist the set of system calls that can drop to the host. This, rather than using *libvirt* as the jailer, also diverges from Red Hat’s architecture.


In summary, Firecracker’s architecture seems to be guided by two principles: (a) reuse Linux components where possible and (b) where resource utilization or the attack surface area (code size) matters, opt for super lightweight alternatives. That makes Firecracker a secure and solid solution for serverless workloads.


### Ubicloud Compute


[Ubicloud](https://www.ubicloud.com/docs/architecture/control-and-data-plane) VMs share a lot of similarities with AWS Firecracker. From a security isolation perspective, we have a strong bias towards simple solutions that minimize the attack surface. We also believe in defense in depth.


Ubicloud also uses KVM, but swaps out Firecracker with the Cloud Hypervisor (CH). A large part of the CH code is based on Firecracker. Both projects are also written in Rust.


CH however offers a more general purpose VMM. This provided us with the following benefits at the time of us picking CH:


- Can run both Linux and Windows distros (the Windows theory currently remains untested)
- Supports a more diverse set of devices, including PCI passthrough for GPUs
- Supports vhost-user devices: virtio backends in guest user space as separate processes
- Supports guest backing by hugepages


The control plane starts, stops, and manages VMs. Rather than REST APIs, we use SSH as the communication interface between the control plane and data plane. For reasons about our choice of SSH, you can watch PVH’s[Heroku architecture talk](https://www.youtube.com/watch?v=twTWnZb0Ib4) .


For network and storage, Ubicloud follows a similar model to that of Firecracker. On the networking side, we create tap devices. We then encapsulate the tap device and VM inside a network namespace to provide an extra layer of security. We also use *nftables* similar to Red Hat to provide firewalls. (AWS also has firewalls, but the Firecracker paper doesn’t talk about this since this isn’t related to isolation on the host.)


For storage, we use[block storage](https://www.ubicloud.com/blog/building-block-storage-for-cloud-with-spdk-non-replicated) instead of relying on the file system. For block storage, we use the open source Storage Performance Development Kit (SPDK). The guest and the host communicate through virtio devices.


Finally, we take an additional set of steps to “sandbox” the Cloud Hypervisor (CH). We run CH as a regular Linux user on the host. We manipulate CH’s permissions in systemd and restrict its file system access through permissions. As mentioned earlier, we use network namespaces. CH also has seccomp-bpf built-in for syscall filtering. The Cloud Hypervisor project is already sensitive about its sycall footprint, so we don’t yet enable seccomp-bpf filtering.


### AWS EC2 Nitro


This blog post wouldn’t be complete without AWS EC2.


In short, EC2 started out with a basic architecture that used the Xen hypervisor and software devices for network and block storage. The challenge with this architecture, at least in 2013, was that device emulation required CPU resources on the instance. You also needed to carve out additional CPU resources on the host for encryption at rest and in-transit.


To optimize network and disk I/O on EC2 instances, AWS incrementally moved towards a model that[offloaded these software devices to hardware](https://www.youtube.com/watch?v=LabltEXk0VQ) . This included first adding a network accelerator card to the host, next moving all network processing to a networking card, and then offloading calls to EBS to a remote storage card (2013 - 2016). Next, AWS offloaded the local storage device to a storage card (2017). These shifts to specialized hardware helped improve performance on the hosts, particularly for I/O bound workloads.


Today, AWS Nitro hosts offload all device management to specialized cards. These accelerator cards provide higher performance than virtio devices. Further, AWS also offloads the task of managing and monitoring VMs to hardware. This offloading removes the need for a VMM like QEMU. (It however introduces the complexity of managing specialized cards.) So, AWS only runs a hypervisor that’s based on KVM on the host.


### Conclusion


AWS opened a new era with cloud computing. Then, as cloud virtualization became commonplace, customers needed more performance and better isolation guarantees. AWS Nitro delivered on that demand by offloading logic to specialized hardware. Other public cloud providers followed suit.


In parallel, we saw progress in commodity hardware and open source software. In 2006, commodity servers had dual-core processors. Today, you can find commodity servers with 64+ cores. In 2006, Intel and AMD CPUs didn’t even have support for virtualization. Today, Linux KVM leverages hardware assisted full virtualization across both chipsets.


In 2012, we didn’t even have a standard for virtualized device drivers. Today, virtio is widely used to virtualize network and storage devices.


The culmination of this progress led to an open source ecosystem that’s now competitive enough to offer an alternative. This blog post summarized that rich ecosystem and how these projects work together.


In closing thoughts, we think open source can mean more secure and offer better price / performance. True, we’re biased. Then again, this entire era opened with a little known open source project called Xen.


### Key References


[Red Hat: All you need to know about KVM userspace](https://www.redhat.com/en/blog/all-you-need-know-about-kvm-userspace) (2019)
‍[Firecracker: Lightweight Virtualization for Serverless Applications](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf) (2020)
‍[AWS re:Invent: C5 Instances and the Evolution of Amazon EC2 Virtualization](https://www.youtube.com/watch?v=LabltEXk0VQ) (2017)


**Footnote** : In this blog post, we focused on AWS and open source technologies; and our terminology followed that. If you've worked with Hyper-V or VMware ESXi, you would have heard them referred to as Type 1 hypervisors. These hypervisors run directly on the host’s physical hardware without relying on a separate OS. The equivalent of them in our terminology would be the combination of the hypervisor kernel, virtual machine monitor (VMM), and device drivers, all bundled together in kernel space.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)
