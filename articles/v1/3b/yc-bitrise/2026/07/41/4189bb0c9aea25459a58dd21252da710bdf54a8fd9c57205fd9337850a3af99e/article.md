---
schema_version: "1.0.0"
document_id: "4189bb0c9aea25459a58dd21252da710bdf54a8fd9c57205fd9337850a3af99e"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/wwdc26-the-virtualization-framework-updates-that-matter-for-large-mac-fleets"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-24T03:43:06.304736+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:01c5eccd4e9a4e04350e4e41f6539e777d71db573a4bb9a8e3816a87d7c9549d"
---

# WWDC26: the Virtualization framework updates that matter for large Mac fleets

WWDC 2026 delivered plenty for app developers to talk about, but some of the most consequential announcements were aimed squarely at the people running the infrastructure underneath. We've already covered the[under-the-radar announcements for iOS developers](https://bitrise.io/blog/post/wwdc26-under-the-radar) and what Xcode 27's[Device Hub](https://bitrise.io/blog/post/wwdc-2026-device-hub-and-what-it-means-for-ci-cd) means for CI/CD. This one is for the fleet operators.


If you run Mac infrastructure at any scale, the Virtualization framework is already part of your life. On Apple Silicon, it's the only way to run a macOS guest. VMs give CI jobs two important characteristics: reproducibility and isolation. It's why the Virtualization framework quietly underpins large Mac fleets like ours.


WWDC 2026 brought a lot of gifts to the Virtualization framework. They were shared in session 224, “[Extend the capabilities of your virtualization app](https://developer.apple.com/videos/play/wwdc2026/224/) ”, and we want to share specific items that are exciting us from an infrastructure point of view.


Please note that many of these features require macOS 27 hosts (and some require macOS 27 guests as well). These will phase in as you upgrade your fleet rather than being available today.


## **Programmatic guest provisioning: cloud-init for Macs**


The first-boot experience has always been the awkward part of automating macOS guests. A fresh macOS install drops you into Setup Assistant, and getting past it to a machine you can actually reach over SSH has historically meant either scripting the UI or baking a pre-configured image.


The new[VZMacGuestProvisioningOptions](https://developer.apple.com/documentation/virtualization/vzmacguestprovisioningoptions?changes=l_5) addresses this directly. You can pass a full name, username, and password into Setup Assistant at first boot, and optionally enable auto-login and remote login (SSH), without anyone clicking through the setup screens or driving them with a UI script. It's the closest macOS has come to cloud-init: declarative first-boot configuration that gets you straight to a reachable machine.


Something to keep in mind is that this needs both the guest and the host on macOS 27, so the benefit only lands once both sides of your fleet have upgraded.


## **Real disk image management with DiskImageKit**


This is the announcement we're most excited about.[DiskImageKit](https://developer.apple.com/documentation/DiskImageKit) is a dedicated framework built on the Apple Sparse Image Format (ASIF, which shipped in macOS 26). The key capability is *stacked* disk images: a base layer, which can be raw or ASIF, plus any number of ASIF cache and overlay layers on top.


Why do we care? VM snapshots and clones become cheap. ASIF overlays are copy-on-write and store only the modified blocks, so cloning a VM is just adding a thin overlay. A read-only base layer can be shared across many VMs at once, with each VM's changes captured in its own overlay.


### **A brief explanation**


**Mental model for readers:** compare this with qcow2. These are layered virtual disks operating copy-on-write at the block level. Components:


- The **base layer** is your golden image. It's read-only and shareable across many concurrent VMs, so one base can back a whole pool of machines.
- The **cache layer** sits over slow or remote storage. When a read misses the cache, it falls through to the lower layers and the result is written into the cache locally, so subsequent reads of the same blocks are served from local disk. This cache layer could make network-backed base images practical.
- The **overlay layer** is copy-on-write and stores only the blocks a VM modifies. Stack multiple overlays and you get snapshot- and fork-like behavior, with each layer holding just its own changes.


### **What this might mean for us at Bitrise**


An immediate opportunity is to retire custom tooling. To support restartable VMs in[Bitrise RDE](https://bitrise.io/platform/remote-dev-environments) , we built our own network block device (NBD) and overlay-snapshotting layer. DiskImageKit covers the same ground natively, which means we could hand that responsibility back to the platform and shed the engineering maintenance of our homegrown NBD.


The next opportunity relates to image size and distribution. Today, Bitrise CI images are distributed in full to every builder node and live locally on each node's filesystem. That gives us fast image starts, but it means each node carries the full image on disk, and distributing an image update means moving large files across the fleet. Layered images could reduce the amount of data stored on each Mac Mini. A large base image on remote storage paired with a local cache layer gives us persistent base images without keeping the full image on every node, and the cache stays small as long as the base is rarely read in full.


That smaller on-node footprint also helps with distribution. When distributing images, our tooling is careful to download new images on a node only when no jobs are running on it. This avoids network and file I/O contention slowing down customer builds. Smaller image payloads mean we can roll out more images more quickly. There's a deduplication angle too: two images built on the same macOS version could share a single base layer instead of each carrying their own copy.


There are concerns. Network-backed stacks trade some performance for portability and storage efficiency, so first-access latency is something we'd measure carefully. And we'd keep a close eye on layer growth: we've observed that idle macOS guests write a surprising amount, so overlays and diffs can grow faster than one would expect. The storage savings may turn out smaller than hoped for.


## **vmnet: custom network topologies without the hacks**


The vmnet framework is now usable from Virtualization for network topologies that go beyond basic NAT or bridge networking. You get configurable DHCP settings, TCP and UDP host-port forwarding to specific VMs, and clean network separation between VMs.


These features can replace some in-house tools that achieved the same ends.


Take network isolation between tenants. On a multi-tenant host, two VMs belonging to different customers can share the same physical hardware, so keeping them apart is essential. Today we enforce that with a hand-built combination of separate virtual networks, firewalling between VMs, and a networking mode that blocks VM-to-VM traffic at the L2 level. Per-VM NAT networks from vmnet provide that out of the box.


The same goes for DHCP and port forwarding. First-class DHCP configuration and host-port forwarding can replace the custom DHCP lease management we maintain today. As with the disk-image work, we'd treat this as something to revisit and confirm against our actual setup rather than assume a drop-in swap, but the direction is clearly toward less custom tooling.


## **Conclusion**


The throughline is that WWDC 2026 will let us replace home-rolled solutions with first-class framework features. This is the kind of infrastructure plumbing we think about constantly when running Macs at scale, so it's good to see Apple investing directly in the parts that make large fleets manageable.
