---
schema_version: "1.0.0"
document_id: "caf9a6c9ab7009531e4162e67b665a7820a407f0fa6e3e9093d93a34903007a3"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/solutions/the-oracle-estate-you-can-control-openshift-virtualization/"
published_at: "2026-08-04T13:00:00+00:00"
first_seen_at: "2026-08-04T14:48:56.189571+00:00"
fetched_at: "2026-08-04T15:13:53.558824+00:00"
content_hash: "sha256:befeba35b90da7800dc66b4a9246d0884e56c0b4c2f5242015595358d4da7293"
---

# The Oracle Estate You Can Control

### Summary


Red Hat OpenShift Virtualization and Everpure help modernize Oracle databases with validated live migration, VMware migration, and a more flexible path for infrastructure upgrades.


Your Oracle estate is running on three “clocks” you don’t control: The hypervisor renewal arrived with a higher cost nobody budgeted for. The database support window has a date on it. And somewhere above you, someone has committed the company to AI that must rely on the critical data that lives in those databases. Each timeline belongs to a different vendor and is being used to force a decision on someone else’s schedule.


The work Red Hat and Everpure have done together exists to break that coupling. Every layer of the estate—hypervisor, operating system (OS), storage, and the database itself—should modernize on its own timeline, which you control. Over the past two years, we’ve built and published the validation to make that real for Oracle workloads specifically. This post covers what we tested, what the results were, and what they mean for how you sequence the next three years.


## Validating Oracle workloads on Red Hat OpenShift Virtualization and Everpure


Red Hat’s engineering team published a[complete reference architecture for Oracle Database 19c on Red Hat OpenShift Virtualization](https://www.redhat.com/en/blog/Oracle-Database-Workloads-On-OpenShift-Virtualization-Reference-Architecture) , covering single instance and Oracle Real Application Clusters (RAC), with the full test artifacts available on GitHub. The validation ran functionality, performance, scalability, and live migration testing. The storage under those tests included Everpure™[FlashArray](https://www.everpuredata.com/products/block-file-object-storage.html) ™ provisioned through the Portworx® Enterprise operator.


Two results matter most for anyone planning a migration:


1. Live migration of a virtual machine (VM) running a single-instance Oracle Database completed in 1-2 minutes on average, and the result held whether the database was idle or under production-like load, with virtual users generating sustained volumes of concurrent transactions. Live migration is the operation that makes a virtualization platform livable. Node maintenance, rebalancing, and hardware refresh all depend on it, and it works for Oracle on this platform the way you need it to.
2. The support posture is the same one you already operate under. Oracle certifies to the OS inside the VM, not to the hypervisor beneath it. Oracle Database on[Red Hat Enterprise Linux (RHEL)](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) remains a fully supported OS configuration, and Red Hat has published comprehensive, validated reference architectures for running these workloads—including RAC—on[OpenShift Virtualization](https://www.redhat.com/en/technologies/cloud-computing/openshift/virtualization) .


## 3 postures, 1 platform


Customers come to this work from different starting positions. The platform doesn’t force them into the same path.


- **Keeping the estate on premises and exiting the legacy virtualization platform.** This is the posture the renewal letter creates. Instead of being locked in to a prohibitive hypervisor renewal,[Red Hat’s migration toolkit for virtualization](https://docs.redhat.com/en/documentation/migration_toolkit_for_virtualization/2.6/html-single/installing_and_using_the_migration_toolkit_for_virtualization/index) moves Oracle VMs from VMware vSphere to OpenShift Virtualization without needing the administrator to modify the guest. The database administrator’s environment on Monday looks like it did on Friday. FlashArray sits underneath both the old platform and the new one, and ActiveCluster™ can move the storage between arrays without taking the database down, which means the storage layer never blocks the migration window. The database didn’t move. The platform around it did.
- **Modernizing on the database’s own clock.** Oracle’s upgrade cycle is its own forcing function, and an upgrade rehearsed badly is the most expensive kind of outage. Array snapshots give you instant, space-efficient clones of production for dev, test, and performance environments, so the rehearsal runs against real data without a second copy of the storage bill.[FlashBlade](https://www.everpuredata.com/products/unstructured-data-storage.html) ® restores at up to 65TB per hour, so the rollback path is measured in minutes rather than in a weekend. The platform migration and the database upgrade stop competing for the same change window because neither one depends on the other.
- **Building around the database.** New services land as containers now. The question is whether they land on the same platform as the estate or on a second one with its own storage, backup, and on-call rotation. Everpure serves persistent storage to both the VMs and the containers from one plane, on the same arrays. The team that runs the VM estate and the team that’s shipping containerized services stop maintaining parallel infrastructure. And the eventual AI workloads—including the vector and retrieval capabilities arriving in the current generation of Oracle Database—inherit the same substrate rather than demanding a new one.


## Value of the storage layer


A platform migration is a sequence of moments where data has to be in two places, move without stopping, or be recoverable faster than the business notices. Those are storage properties. The Red Hat validation results on RAC made the point empirically: External enterprise array storage outperformed hyperconverged alternatives for the most demanding configurations. The seams in a modernization—between hypervisors, between database versions, and between VM and container—are exactly where the storage platform either decouples the clocks or chains them back together.


Everpure[Evergreen® architecture](https://www.everpuredata.com/knowledge/what-is-evergreen-architecture.html) extends the same logic to the storage itself. The array under your Oracle estate upgrades nondisruptively across controller generations, which removes the one clock infrastructure teams traditionally imposed on everyone else: the forklift refresh.


## What to do with this


Start with the seam that has a date on it. If that date is a hypervisor renewal, run Red Hat’s migration toolkit for virtualization against a nonproduction Oracle VM and measure the result against the published reference architecture. If that date is a database support window, build the rehearsal environment from snapshots this quarter and time your restore path. Both exercises cost days, and both convert an external deadline into a measured plan.


The estate you run in 2029 will not look like the one you run today. The work above means you get to decide the order in which it changes.


**To learn more, explore these resources:**


- [Oracle Database Workloads On OpenShift Virtualization Reference Architecture](https://www.redhat.com/en/blog/Oracle-Database-Workloads-On-OpenShift-Virtualization-Reference-Architecture)
- [Modern virtualization infrastructure with Red Hat and Portworx](https://www.redhat.com/en/resources/openshift-virtualization-portworx-overview?sc_cid=RHCTN0260000481706&gad_source=1&gad_campaignid=23776920176&gbraid=0AAAAADsbVMTbuYymM8lhuWlS08K7vv4os&gclid=EAIaIQobChMIjuHP2cGRlQMV6xwIBR06VixEEAAYASAAEgI9iPD_BwE)
- [Oracle and Everpure partnership](https://www.everpuredata.com/partners/technology-alliance-partners/oracle.html)
- [Oracle Database solutions: Unlock the true value of your Oracle data with Everpure](https://www.everpuredata.com/solutions/databases/oracle.html)


## Build a More Flexible Foundation for Oracle


See how Red Hat OpenShift Virtualization and Everpure storage support Oracle environments with flexible modernization, resilient performance, and a smoother path to what’s next.


[Explore Oracle Solutions](https://www.everpuredata.com/solutions/cyber-resilience/ransomware/safemode.html)
