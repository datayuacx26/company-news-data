---
schema_version: "1.0.0"
document_id: "73a88808ce014393aabbd53168d13f6f580f577a3041846375421d56e0ef4361"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/purely-technical/new-features-expand-everpure-and-nutanix-joint-solution/"
published_at: "2026-07-31T13:00:00+00:00"
first_seen_at: "2026-07-31T16:56:51.776562+00:00"
fetched_at: "2026-07-31T16:56:53.056692+00:00"
content_hash: "sha256:77a682fb36ad9493eb10ef0a54c4d82671d273109e2cba62352f8f3a101ba095"
---

# New Features Expand Everpure and Nutanix Joint Solution

### Summary


Everpure and Nutanix have expanded their joint solution with new features that improve backup performance, double virtual disk scalability, strengthen cyber resilience, and simplify management.


If you’ve been following our journey, you know that Everpure and Nutanix have co-engineered a deeply integrated offering to give our customers a flexible, high-performance solution. The latest release adds significant new features that increase scale, enhance cyber resilience, and simplify management. Most importantly, these updates reinforce our long-term commitment to the joint partnership and our continued roadmap investment. Both organizations share an active pipeline of innovations still to come.


When we officially announced general availability for our Nutanix partnership in December 2025, we set out to build far more than just a validated storage target for Nutanix. We envisioned a deeply co-engineered ecosystem designed to push performance, efficiency, and enterprise scalability forward. Over the past eight months, that vision has rapidly materialized–beyond what we initially released. The teams have been very productive. From extending support for FlashArray//C, to supporting Nutanix Kubernetes Platform (NKP) and more recently extending to unstructured data solutioning for Nutanix AI workloads from FlashArray and FlashBlade with PX-CSI, our joint engineering teams have proven that this partnership is anything but a “V1 and done” milestone.


At the core of our technical momentum is a simple principle: **intelligent offloading** . Storage platforms perform best when tasks are handled where the context natively lives. In our initial release, we demonstrated this by offloading VM creation, cloning, snapshotting, and restores directly to the storage array, eliminating compute overhead and enabling near-instantaneous operations. What came at GA was a significant step in that vision.


But we were not done.


There are two generalized workflow types when it comes to storage and a hypervisor:


- Managed: storage workflows that are directly integrated into the virtualization control plane (virtual disk creation in Prism for example)
- Unmanaged: storage workflows that occur with no knowledge by the hypervisor


The former was heavily activated by our first release. Nutanix 7.6.0 opens up the latter.


So features like:


- Snapshot protection policies
- Safemode
- Asynchronous snapshot replication
- ActiveDR
- And more…


Are now opened up for customer usage in this release.


Let’s dig in.


# **Change Block Tracking Offload**


The Nutanix 7.6.0 release, along with Purity 6.12.0+ or 6.11.8+, adds a major architectural enhancement born from our deep collaboration: Change Block Tracking (CBT) offload


Today, we are taking the next logical step forward in co-engineering by tackling one of the most persistent efficiency bottlenecks in virtualized environments: Change Block Tracking (CBT).


Historically, tracking modified data blocks for backup and replication required Nutanix Controller VMs (CVMs) to maintain dedicated tracking metadata volumes for every provisioned vDisk. This approach adds storage management overhead, inflates array volume counts, and introduces brittleness—if a storage admin executes a direct array-side volume copy or refresh outside of Prism, the hypervisor’s change tracking breaks.


To solve this, Nutanix can now offload change tracking directly to Everpure’s native vol-diff API. Instead of forcing the hypervisor to manage extra tracking volumes and compute cycles, Nutanix queries our array on demand to identify changed blocks instantly. The result is a lighter compute footprint, significantly reduced volume bloat, and full support for array-native workflows—bringing unprecedented flexibility and efficiency to database admins, storage architects, and enterprise operations.


*Figure 1: The new release fundamentally changes how changed blocks are tracked, freeing up resources.*


Instead of the Nutanix CVM burning resources, the FlashArray now natively understands the block-level differences between storage objects using the Volume Differential (VolDiff) API. When your backup provider needs to know what changed, it simply queries VolDiff, and FlashArray reports the exact differences instantly. The Nutanix CVM is completely freed from the burden of tracking and storing these block changes.


This follows a larger industry trend of developing CBT-type solutions to increase efficiency, as seen recently in both[Kubernetes](https://kubernetes.io/blog/2025/09/25/csi-changed-block-tracking/) and[OpenStack](https://review.opendev.org/c/openstack/cinder-specs/+/985089) development. This fits into growing use cases around agentic AI and containerized workloads, which Everpure is actively supporting by offering multiple connectivity options between Nutanix and Everpure arrays. These options—such as a CSI driver, object storage, and file data—are detailed in our[connectivity blog](https://blog.everpuredata.com/products/more-than-block-storage-3-ways-everpure-connects-your-nutanix-infrastructure/) .


## **Double the scale, better performance, and enhanced cyber resilience**


What does this architectural shift actually mean for your bottom line? Let’s look at the real-world impact in two dimensions.


**Infrastructure and performance benefits**


- **Hardware native change tracking:** The Nutanix CVM is completely freed from the burden of tracking and storing block changes, which is now handled efficiently by the FlashArray. This frees up processing, which allows you to run more workloads per server, reducing management overhead and total solution cost.
- **Doubled virtual disk scalability:** We’ve created a zero-clutter architecture. By eliminating the need for a secondary metadata volume for every virtual disk, you instantly double the number of virtual disks a single array can support, letting you run more VMs per array without spending anything on new gear. This lets you delay capital expenditures, which is especially valuable in a time of[supply constraints](https://blog.everpuredata.com/perspectives/everpure-protecting-customers-through-supply-uncertainty/) and long lead times.
- **Peak performance during backups:** By moving change tracking natively to the hardware, you can say goodbye to application slowdowns during backups. Incremental backup providers get faster data differentiation directly from the storage layer.
- **Seamless backup software compatibility:** Backup performance gains are completely transparent to your backup provider. It will just work, with no changes needed to your backup software, such as Commvault, HYCU, Rubrik, Veeam, and others.


**Secondary workflow and disaster recovery benefits**


Historically, managing snapshots strictly through Nutanix Prism limited what storage administrators could do. Doing an out-of-band restore directly on the FlashArray would break the internal CBT records held by Nutanix.


Because the FlashArray is now the “source of truth” for change tracking, a wave of advanced workflows are now supported:


- **Safe out-of-band restores:** Administrators can log directly into the FlashArray and restore a Nutanix virtual disk from another volume. This includes using[SafeMode™ Snapshots](https://www.everpuredata.com/solutions/cyber-resilience/ransomware/safemode.html) , which provide indelible copies that cannot be deleted and offer a way to recover from a ransomware attack. If Nutanix or a backup provider requests a delta afterward, VolDiff automatically adjusts and provides an accurate map of the changes, keeping the backup chain intact.
- **Frictionless DevOps and data portability:** Administrators can now perform safe out-of-band restores and easily share volumes between VMs. This allows your DevOps teams, DBAs, application owners, and others to instantly copy production data to test environments with just one click, dramatically accelerating your development cycles and speeding time to market.
- **Built-In business continuity:** Because the array now serves as the source of truth for change tracking, you can safely leverage advanced features like asynchronous replication and[ActiveDR](https://www.everpuredata.com/solutions/cyber-resilience/business-continuity-data-recovery.html) ™ to protect Nutanix VMs. If disaster strikes, this unlocks fast, flawless data recovery because target sites can be restored directly from these array-based snapshots with the storage layer seamlessly handling the underlying block tracking. By speeding up recovery time, organizations return to operations faster, avoiding potential financial and reputational loss.


## **The bottom line**


At Everpure, we believe your infrastructure should work for you, not the other way around. By decoupling compute and storage scaling, you can avoid synchronized procurement cycles and only buy what you need, when you need it.


These latest solution updates resonate across organizations. CIOs like it because it simplifies IT infrastructure and increases capacity and data security. CFOs are happy because it delays new hardware spend and improves return on invested capital. IT teams enjoy a solution that is simple, scalable, and intuitive.


This unique co-engineering between Nutanix and Everpure delivers a seamless, flexible experience right inside your familiar Nutanix Prism environment. You get the best of both worlds: hardware-level performance with hypervisor-level control. And much more to come.


Are you ready to see what Everpure can do for your virtualized workloads? Reach out to your Everpure or Nutanix account team today to learn more.


**Learn more about**[Everpure solutions for Nutanix](https://www.everpuredata.com/solutions/virtualization/nutanix.html) **.**


## See What Everpure + Nutanix Can Do for Your Virtualized Workloads


Get hardware-level performance with hypervisor-level control — scale further, recover faster with SafeMode, and manage it all inside Nutanix Prism.


[Explore Everpure for Nutanix](https://www.everpuredata.com/solutions/cyber-resilience/ransomware/safemode.html)
