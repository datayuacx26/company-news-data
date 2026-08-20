---
schema_version: "1.0.0"
document_id: "aa21a39003d547df1d9e97e9ccd6569a635fade08ad5ac57a24cfc67245d3526"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/purely-technical/vvols-deprecation-migration-options-for-everpure-customers/"
published_at: "2026-08-18T19:34:43+00:00"
first_seen_at: "2026-08-18T22:42:18.699344+00:00"
fetched_at: "2026-08-18T22:42:20.080117+00:00"
content_hash: "sha256:bce6468fb3a498d8983ae21437c08eb126657de0b7a10573703e9d5a44a88cfb"
---

# Planning Beyond vVols: A Concise Guide to VMware Storage Decisions

### Summary


Broadcom has deprecated VMware vVols in VCF 9.0. Everpure customers can migrate workloads to supported FlashArray-backed VMFS or NFS datastores without replacing their existing storage infrastructure.


Broadcom has deprecated vSphere Virtual Volumes (vVols) beginning with VMware Cloud Foundation (VCF) 9.0 and VMware vSphere Foundation (VVF) 9.0. While Broadcom states that vVols will be removed in a future release, their[knowledge base article 401070](https://knowledge.broadcom.com/external/article/401070) does not specify a final release version or date.


For customers running VMware on Everpure™[FlashArray](https://www.everpuredata.com/products/block-file-object-storage.html) ™, the practical response is to plan a move from vVols to a supported VMFS or NFS datastore while preserving the FlashArray platform and its storage services.


## **What has changed**


Broadcom has shared that:


- vVols is deprecated in VCF/VVF 9.0.
- vVols will be removed in a future VCF/VVF release.
- Customers should validate support for their current and target VMware releases before upgrading.


Because vVols is deprecated, it should not be the target storage model for a new VCF deployment. Existing customers should use their upgrade, renewal, and application-lifecycle milestones to plan the transition.


### **Where should Everpure customers move their workloads?**


The best target depends on the connectivity already deployed, the VCF deployment path, and the direction of your next infrastructure refresh.


**Recommended Target** **Best Fit** **Key Considerations**


VMFS on SCSI-FC Customers with an established Fibre Channel fabric or greenfield VCF deployment Supported as principal storage for greenfield VCF 9 domains and as supplemental storage


VMFS on NVMe-FC Customers refreshing FC fabrics, hosts, or adapters A forward-looking block storage option when the required ESXi, adapter, fabric, FlashArray, and VCF combinations are supported


VMFS on NVMe-TCP Customers standardizing on Ethernet and looking for a modern block protocol Use for imported or supplemental storage where the environment meets the current interoperability requirements


VMFS on iSCSI Customers that want to preserve an existing iSCSI design Supported as supplemental storage; for principal storage in VCF 9, use the supported import or convergence path rather than the greenfield installer


NFSv3 Customers that prefer file-based datastores or already use NFS operationally Supported as principal storage for greenfield VCF 9 domains and as supplemental storage


NFSv4.1 Customers with an existing NFSv4.1 design or an imported VCF environment Supported for imported or supplemental storage; validate the target deployment model before selecting it as principal storage


The current[FlashArray VMware Cloud Foundation User Guide](https://support.everpuredata.com/r/user-guides-for-vmware-solutions/vmware-cloud-foundation-user-guide) provides the detailed support matrix. Protocol support can vary between greenfield, imported or brownfield, principal, and supplemental storage, so confirm the exact combination before designing the migration.


## **Recommended vVols migration paths**


### **Path 1: Preserve the existing fabric and move to VMFS**


This is often the simplest path for customers already using Fibre Channel or iSCSI with FlashArray.


- Create a new VMFS datastore on FlashArray using the supported block protocol.
- Use Storage vMotion to move vVols-based VMs to the new VMFS datastore.
- Recreate the required protection and operational workflows using FlashArray and VMware tools.
- Validate application behavior, performance, backup, replication, and recovery.


Choose SCSI-FC VMFS when FC is already well established and supported in the target design. Select iSCSI VMFS when preserving the existing iSCSI network reduces migration effort; remember that iSCSI principal storage in VCF 9 requires the supported import or convergence workflow rather than greenfield deployment.


### **Path 2: Move to NFS on FlashArray**


NFSv3 is a strong option for customers that prefer file-based datastores, already operate NFS, or want to avoid introducing a new block fabric.


- Create an[NFS datastore](https://www.everpuredata.com/knowledge/what-is-nfs-data-store.html) on FlashArray using the supported NFS version.
- Use Storage vMotion to move workloads from vVols to NFS.
- Validate NFS networking, datastore behavior, backup, replication, and recovery workflows.
- Confirm whether NFSv3 or NFSv4.1 is supported for the specific VCF deployment model.


For greenfield VCF 9 deployments, NFSv3 is the relevant principal-storage option. NFSv4.1 requires an imported or supplemental-storage scenario according to the current FlashArray VCF support matrix.


### **Path 3: Modernize the storage protocol during the migration**


A vVols transition can also be an opportunity to move to[NVMe-oF](https://www.everpuredata.com/knowledge/what-is-nvme-over-fabrics-nvme-of.html) . Consider NVMe-FC when the environment already uses Fibre Channel and the required host and fabric components support NVMe. Consider NVMe-TCP when the organization is standardizing on Ethernet and wants to use an IP-based storage fabric.


Treat a protocol change as a datastore migration rather than an in-place conversion:


- Build the new NVMe-oF-backed VMFS datastore.
- Validate host, adapter, fabric, FlashArray, Purity, and VCF interoperability.
- Use Storage vMotion to move workloads from vVols to the new datastore.
- Test performance, multipathing, failure behavior, backup, replication, and recovery before broad deployment.


NVMe-RoCEv2 is also listed in the current FlashArray VCF support matrix for imported and supplemental storage. It requires careful end-to-end validation of the network and host design.


## **How FlashArray capabilities carry forward**


Moving away from vVols changes the VMware storage presentation; however, it does not require customers to replace FlashArray. FlashArray continues to provide the underlying storage platform for VMFS and NFS datastores, along with FlashArray data services and VMware integrations supported by the target design.


The[Everpure vSphere Plugin](https://support.everpuredata.com/r/user-guides-for-vmware-solutions/using-the-everpure-plugin-for-the-vsphere-client) can be used with supported FlashArray and VCF configurations to simplify datastore provisioning and storage administration from the vSphere environment.


For customers using replication or stretched-storage designs, review the support requirements for[ActiveCluster](https://support.everpuredata.com/r/user-guides-for-vmware-solutions/activecluster-with-vmware-user-guide) ™ and[ActiveDR](https://support.everpuredata.com/r/user-guides-for-vmware-solutions/activedr-with-vmware-user-guide) ™ against the selected protocol and target VCF release. The current VCF guide identifies protocol-specific support considerations, including ActiveCluster support limitations by release and protocol.


## **A practical vVols transition plan**


### **1. Inventory the vVols environment**


Identify the vVols datastores, virtual machines, capacity, storage policies, replication relationships, snapshot schedules, backup workflows, and automation that depend on vVols.


### **2. Choose the target based on the environment**


Use this decision pattern as a starting point:


- **Existing FC fabric:** VMFS on SCSI-FC, or NVMe-FC if the environment is being modernized
- **Existing iSCSI network:** VMFS on iSCSI, using supplemental storage or the supported import/convergence path for principal storage
- **Existing NFS operations:** NFSv3 for greenfield principal storage or NFS for supplemental storage, subject to the version and deployment model
- **Ethernet storage refresh:** VMFS on NVMe-TCP where the complete solution is supported
- **Existing or planned RoCE infrastructure:** NVMe-RoCEv2 where the end-to-end design is validated


### **3. Create the target datastore**


Provision the target datastore on FlashArray using the appropriate protocol and follow the current VMware and Everpure implementation guidance for host connectivity, multipathing, networking, and datastore configuration.


### **4. Migrate with Storage vMotion**


Move workloads in controlled groups. Start with a representative, low-risk workload and validate the complete operating model before scaling the migration.


### **5. Recreate protection and operational intent**


Map vVols-based policies and workflows to the target design. Validate FlashArray snapshots, replication, backup integration, monitoring, VM provisioning, restores, and day-two administration.


### **6. Confirm upgrade readiness**


Before upgrading to a VMware release that removes or no longer supports vVols, confirm that no in-scope workloads, datastores, policies, or automation still depend on vVols.


## **Frequently asked questions**


**Does moving away from vVols require replacing FlashArray?**


No, the recommended transition is to move workloads from vVols datastores to supported FlashArray-backed VMFS or NFS datastores. The array platform remains in place; the VMware storage presentation changes.


**Which protocol should I choose?**


Choose the protocol that best fits your existing infrastructure and target VCF deployment model. SCSI-FC is a strong choice for established FC environments. NFSv3 is a practical choice for file-based datastore operations and greenfield VCF principal storage. iSCSI can preserve an existing Ethernet block design, but principal-storage use in VCF 9 requires the supported import or convergence path. NVMe-FC and NVMe-TCP are modernization options when the full host, fabric, FlashArray, and VCF combination is supported.


**Can I move directly from vVols to NVMe-oF?**


Yes, when the target configuration is supported. Create a new NVMe-oF-backed VMFS datastore and use Storage vMotion to move workloads; do not treat the change as an in-place protocol conversion.


**Is iSCSI supported in VCF 9?**


Yes, but the role matters. The current FlashArray VCF guidance lists iSCSI as supplemental storage for greenfield and imported domains, and as principal storage for imported or brownfield domains. It is not a greenfield principal-storage selection in the VCF Installer.


**Is NFSv4.1 supported in VCF 9?**


Yes, in the supported deployment roles identified in the current FlashArray VCF support matrix. NFSv4.1 is listed for imported or supplemental storage; NFSv3 is the relevant NFS principal-storage option for greenfield VCF 9 deployments.


**Where can I verify the support matrix?**


Review[Broadcom KB 401070](https://knowledge.broadcom.com/external/article/401070) ,[Broadcom KB 416270](https://knowledge.broadcom.com/external/article/416270) , and the current[FlashArray VMware Cloud Foundation User Guide](https://support.everpuredata.com/r/user-guides-for-vmware-solutions/vmware-cloud-foundation-user-guide) before making a production design or upgrade decision.


You can also learn more in the blog “[How to Get Complete Storage Flexibility in VMware Cloud Foundation](https://blog.everpuredata.com/purely-technical/storage-considerations-for-vmware-cloud-foundation-with-flasharray/) .”


## Plan Your Next VMware Move


Join Everpure at VMware Explore 2026, August 31–September 3 in Las Vegas, to discuss vVols transition options and build a more flexible VMware storage strategy.


[Register Now](https://www.everpuredata.com/events/vmware-explore-las-vegas-2026.html)
