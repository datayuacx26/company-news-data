---
schema_version: "1.0.0"
document_id: "de9f88db734787163a4d09bbd4c73e3283ad287cac180156181700e83e7759e8"
company_key: "dell-technologies-inc-class-c-common-stock"
company: "Dell Technologies Inc."
source_id: "dell-technologies-inc-class-c-common-stock-rss-5d64130a27f7"
canonical_url: "https://www.dell.com/en-us/blog/under-the-hood-data-protection-platform/"
published_at: "2023-01-11T13:00:29+00:00"
first_seen_at: "2026-07-20T04:35:28.920829+00:00"
fetched_at: "2026-07-28T22:00:21.221344+00:00"
content_hash: "sha256:3e397b2b9c3cbf34886990e59990033506e1c81560ff52ac6f4431f6165abb9c"
---

# Under the Hood: Data Protection Platform

*This blog is co-authored by Naveen Rastogi, Architect for PowerProtect Data Manager, Distinguished Engineer.*


Dell PowerProtect Data Manager software provides a software-defined data protection solution, built from the ground up for proven and modern workloads.


In a previous **[blog post](https://www.dell.com/en-us/blog/under-the-hood-dell-powerprotect-data-manager/)** , we examined the high-level architecture of the Policy Engine and Catalog Service of the Dell PowerProtect Data Manager protection platform. We will now examine other core services of PowerProtect Data Manager.


Figure 1: PowerProtect Data Manager – Data Protection Platform


#### **Data Services**


In data protection software, the ability to replicate copies of data is not a “nice to have” but a must-have functionality. While every customer has his or her own set of unique needs and circumstances to maintain multiple copies of production data, replication of data is an essential component for recovery in the event of a modern or natural disaster. Some common types include:


**Operational Recovery Copy.** This type of replication entails creating a protection copy in the same data center or location, enabling you to quickly recover and instantly access data without pulling the data remotely. To reduce costs on production storage and tolerate against corruption or defects in production storage, users create a protected copy of their production data and keep the copy in deduplicated protection storage (i.e., different media). This enables users to keep operational copies for a longer duration because of deduplication.


**Disaster Recovery Copy** . In this situation, the user replicates a copy to a remote data center to protect against disaster. This replicated protection copy could be stored in the user’s own data center or in public cloud. When replicating, users want the WAN bandwidth to be optimally utilized, replicate only the changes, tolerate higher latency and throttle the replication bandwidth. Users also want the ability to restore in the replicated site or restore back into the production site. Depending on the business need, these replicated copies could have longer or shorter retention than the operation recovery copy.


**Cloud Disaster Recovery Copy** . For virtual machines, users want the ability to replicate copy to public cloud object storage (with or without requiring compute in public cloud) and restore the copy into public cloud.


**Long-term Retention Copy** . Users want to keep the long-term retention copy for long duration (e.g., seven years) with low cost. Object Storage and deduplication can reduce costs for long-term retention.


Figure 2: PowerProtect Data Manager – Data Services


In PowerProtect Data Manager, we have added a set of data services to address the above requirements. PowerProtect Data Manager policies are flexible and allow users to choose the protection level needed for their organization.


PowerProtect Data Manager configures these services in protection stages in the policy engine at the platform layer, and they work consistently across all protection workloads. The protection applications handle the initial protection copy creation, and data services agnostic to the workload handle subsequent copies. These data services provide finer control to operate on the protection copies (e.g., which copies to replicate, how long to retain, when to operate on the copy, etc.)


**Replication Service** . Replication Data Service (see Figure 2) coordinates with PowerProtect DD series appliances to replicate all the files associated with a protection copy to the remote site. PowerProtect DD series appliances use the replication networks link very efficiently by sending only the unique data over the network. Replication service orchestrates replication of backup copies between DD series efficiently without overloading the target appliance.


To meet replication network bandwidth and RPO requirements, the DD series provides various scheduling options, including “Replication upon completion,” by automatically replicating the backup copy on completion of backup. Replication service works with catalog service to track the replicated backup copies and retention. It also ensures the catalog on the remote site is updated after replication. This enables the remote system Backup Admin to quickly recover copies in case of disasters.


PowerProtect Data Manager has tight integration with PowerProtect DD series for replication management, leveraging years of PowerProtect DD series’ strengths in eliminating tapes for disaster recovery (DR).


#### **Cloud Disaster Recovery Service**


Cloud DR provides a disaster recovery site in the cloud (AWS, Google Cloud). PowerProtect Data Manager takes copies of protected VMs from DD series and copies it efficiently to become object storage in the public cloud by replicating only the modified data. No additional compute resources are required in the public cloud. It provides failback, failover and test DR capability. In case of a site disaster, Cloud Disaster Recovery Server (a recovery service in the cloud) orchestrates the recovery of VM in the cloud and uses the public cloud compute/storage/network resources to run the workload. Cloud Disaster Recovery Server can restore data from Object Storage to the public cloud block storage and orchestrates the necessary format conversions to power up the VM in the public cloud.


PowerProtect Data Manager and Cloud DR not only significantly reduce DR data center costs, but they also allow customers to move to an OpEx model for storing/recovery VM protection copies in public cloud. In addition, they simplify auto orchestration of recovery in the public cloud.


#### **Cloud Tiering Service**


Cloud Tier enables the option to tier protection copies to Object Storage (on-prem or public cloud) for long-term retention. PowerProtect Data Manager leverages the PowerProtect DD Cloud Tiering capabilities for this and orchestrates the Client Tiering. The DD series keeps the data deduplicated in the Object Storage, thereby reducing further the Object Storage costs. PowerProtect Data Manager catalog is designed to keep these long-term copies and orchestrates the recovery/recall from the Object Storage (only changes are recalled). In addition, no compute is required in the public cloud.


PowerProtect Data Manager and PowerProtect DD series appliances have the best integrations for tiering data to the public cloud, significantly reduce the OPEX costs by minimizing bandwidth to public cloud and reduce object storage costs by deduplicating.


PowerProtect Data Manager empowers you to start your journey towards modern data protection with efficient performance, secure operational and cyber resilience and a simple, unified user management experience.


Click here to learn more about **[PowerProtect Data Manager](https://www.dell.com/powerprotectdatamanager)** .
