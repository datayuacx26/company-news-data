---
schema_version: "1.0.0"
document_id: "201b81e12226bd875fdf33c3d940ba74d1b58f3872b2b87182e5437ccdf9da55"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/products/flash-array-simplifies-vmware-migration/"
published_at: "2026-07-02T13:00:00+00:00"
first_seen_at: "2026-07-25T03:30:11.662383+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:3ad1478b9c708fe06c0f68e352f29d0b85234888e4960412ab679ca68e674fe9"
---

# FlashArray Simplifies VMware to Red Hat OpenShift Virtualization Migration

### Summary


Everpure FlashArray accelerates VMware to Red Hat OpenShift Virtualization migration by offloading VM data movement to the array, reducing downtime, network load, and migration complexity.


As organizations look for a practical path away from traditional VMware-centric infrastructure, they want more than a workable migration plan. They want a transition that feels faster, simpler, and lower risk from day one. The challenge is not whether a move is possible. It’s whether that move can happen fast enough, predictably enough, and with low enough operational risk to make the transition worthwhile.


That’s where Everpure™[FlashArray](https://www.everpuredata.com/products/block-file-object-storage.html) ™ changes the equation.


The Red Hat Migration Toolkit for Virtualization (MTV) provides the framework to move virtual machines from vSphere into OpenShift Virtualization, while Everpure FlashArray adds the storage-side acceleration and consistency that can make migrations faster and less disruptive.


## **The problem with traditional VM migration paths**


In a standard MTV migration path, VMware Virtual Disk Development Kit (VDDK) is used to read and move VM disk data from the source datastore into OpenShift. That works, but it pushes the migration burden onto the host, the CPU, and the network.


The cost of this approach is real: The host CPU utilization rises with read throughput, full disk contents move over the network, worker nodes spend CPU cycles on conversion and writes, and migration time scales linearly with data volume.


Storage copy offload is designed to migrate VMware VMs and offload the copy operations to the storage network (SAN) efficiently instead of pushing full disk copies over the network. For VMFS-based migrations, that means XCOPY offload. For RDM and vVol migrations, Everpure can go further by leveraging the FlashArray Volume Copy API to offload the full data migration directly to the array in the fastest way possible.


## **What Everpure FlashArray does differently**


FlashArray shifts bulk data movement off the hosts and into the storage layer. To offload the migration to the array, both VMware and OpenShift share the same FlashArray, allowing migration data movement to happen inside the storage path rather than through the host network stack. So the array is doing all of the heavy lifting rather than just accelerating part of the operation.


The high-level architecture looks like this for VMFS-based XCOPY offload:


*Figure 1: High-level overview on how the XCOPY migration path maps from vSphere to OpenShift.*


And for vVol- or RDM-based volume copy, the model is even more direct:


*Figure 2: High-level overview on how the volume copy migration path maps from vSphere to OpenShift.*


At a glance, these diagrams show the core value proposition: FlashArray sits beneath both the source VMware environment and the target OpenShift Virtualization environment, enabling the migration engine to use array-native operations instead of forcing full host-based network copies. In the XCOPY path, FlashArray offloads VMFS-based data movement from the VMDK to the persistent volume by simply updating metadata pointers. In the volume copy path, vVol- or RDM-based migrations are offloaded with a simple REST operation with a volume copy, which is why the data copy itself can happen in seconds rather than minutes for large virtual disks.


That delivers several customer advantages:


- Reduced dependence on host-based transfer paths and VDDK bottlenecks
- Lower CPU, memory, and network utilization during migration
- More predictable migration throughput because performance is less tied to host load and network conditions
- A common storage foundation across source and target environments that simplifies the migration architecture


Just as important, the same platform continues to add value after cutover and migration, and the same storage platform supports Kubernetes-native operational models such as CSI-based provisioning and CSI snapshot workflows for PVC-backed workloads.


## **Differentiation that customers can actually feel**


For customers, differentiation only matters if it shows up in outcomes. In this case, it does.


The validated lab results from real workload testing by Everpure show that offloaded migration consistently reduced total migration time and total downtime. It also dramatically reduced network traffic compared with VDDK-based paths.


For a large Linux VM with about 305GB of used data, a VMFS migration over VDDK took 1 hour 25 minutes 31 seconds, while XCOPY offload completed in 5 minutes 10 seconds. The same pattern holds at scale with offloaded migration improving scalability for VM migrations and reduced infrastructure overhead during execution. That matters because migration success at enterprise scale is rarely about one VM. It’s about repeating the process across dozens or hundreds of workloads without creating a new bottleneck in the process.


And the value becomes even clearer for more complex estate types. In the shared-disk scenario (both with RDMs and vVols), the key is not just that the migration is faster but rather that the FlashArray system is taking over the full data migration through volume copy, offloading all of the migration directly to the array rather than relying on a partial host-assisted path. That’s why large shared-disk migrations that would otherwise take 8 hours 20 minutes 27 seconds for the first stage and 6 hours 48 minutes 45 seconds for the remaining clustered VMs can be compressed into roughly 5 minutes for the first VM and 5 minutes 28 seconds for the second VM respectively. For vVols and RDMs, the data copy itself can happen in seconds on the array, with the remaining elapsed time driven by workflow orchestration, attachment, and cutover steps rather than the data move itself.


## **Why this matters beyond speed**


Speed is the headline, but it is not the whole story. FlashArray helps reduce migration risk and gives teams a better chance to shorten migration windows, simplify cutovers, and phase modernization in a controlled way.


The architecture is also well-suited to phased adoption. Everpure guidance recommends starting with a controlled pilot of non-production VMs, then moving to foundational services, and then progressing in dependency-aware waves for application migrations.


That makes the FlashArray advantage more strategic than tactical. Customers are not just getting a faster copy engine. They’re getting a migration model that supports a cleaner transition from VMware to OpenShift.


## **A better operational landing zone after migration**


An important part of the total value is what happens after the migration is complete.


Everpure and OpenShift deliver optionality by defining storage classes that leverage multiple array-native features. For mission-critical workloads, you can provide synchronous zero-RPO protection via[ActiveCluster](https://www.everpuredata.com/demos/platform/protecting-your-data/activecluster-configuration-and-validation/6366915074112.html) ™, while for disaster recovery over distance, you can utilize[ActiveDR](https://www.everpuredata.com/demos/platform/protecting-your-data/activedr/6367043313112.html) ™ for near-zero RPO. For all other workloads, volume snapshots provide a robust and space-efficient default protection tier.


That gives customers a more complete modernization story: migrate faster, operate more consistently, and protect workloads using data services already aligned to the OpenShift model.


## **The bottom line**


OpenShift Virtualization gives customers a modern destination for VM workloads. Red Hat MTV provides the migration framework. And Everpure FlashArray turns that framework into a faster, lower-impact, more predictable migration experience.


For customers migrating to OpenShift Virtualization, the differentiation is clear:


- Move data inside the array instead of through overloaded hosts and networks
- Use XCOPY for VMFS and full FlashArray volume copy for vVols and RDMs, matching the fastest path for each storage type
- Compress migration windows from hours to minutes in validated scenarios, with data copy on vVols and RDMs occurring in seconds on the array
- Reduce infrastructure overhead during migration
- Preserve a common storage platform before, during, and after the move
- Land on an operational model that supports Kubernetes-native provisioning, snapshots, backup, and DR


For organizations trying to modernize without accepting long migration windows, network strain, or unnecessary operational complexity, that’s a meaningful advantage.


To learn more, see the following documentation:


- [Official Everpure documentation](https://support.purestorage.com/bundle/m_red_hat/page/Red_Hat/topics/openshift_migration_tool_for_virtualization_with_flasharray.html)
- [Official Red Hat Migration Toolkit for Virtualization documentation](https://docs.redhat.com/en/documentation/migration_toolkit_for_virtualization/2.11/html/migrating_your_virtual_machines_to_red_hat_openshift_virtualization/index)
- [Official Forklift documentation](https://kubev2v.github.io/forklift-documentation/)
- [How to migrate from Portworx® FlashArray Direct Access volumes to Portworx Kube Datastore](https://docs.portworx.com/portworx-enterprise/provision-storage/kubevirt-vms/rapid-vm-migration)


## Plan Your OpenShift Virtualization Migration


Get the official Red Hat guidance for migrating virtual machines to OpenShift Virtualization with Migration Toolkit for Virtualization.


[Red Hat Migration Toolkit for Virtualization documentation](https://docs.redhat.com/en/documentation/migration_toolkit_for_virtualization/2.11/html/migrating_your_virtual_machines_to_red_hat_openshift_virtualization/index)
