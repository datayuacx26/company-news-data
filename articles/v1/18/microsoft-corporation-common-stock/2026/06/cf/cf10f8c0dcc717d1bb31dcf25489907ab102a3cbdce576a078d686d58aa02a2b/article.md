---
schema_version: "1.0.0"
document_id: "cf10f8c0dcc717d1bb31dcf25489907ab102a3cbdce576a078d686d58aa02a2b"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-bfb1f574a3a6"
canonical_url: "https://azure.microsoft.com/en-us/blog/accelerate-modern-linux-workloads-with-azure-files/"
published_at: "2026-06-29T15:00:00+00:00"
first_seen_at: "2026-07-25T02:59:23.665583+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:ea1a3216020c43fd82a271df03bc6318a860ffb8c89385335968a08730c361ad"
---

# Accelerate modern Linux workloads with Azure Files

Linux workloads are evolving quickly as organizations modernize on-premises Linux applications, adopt cloud-native architectures, and build new AI and data-intensive pipelines in the cloud. As this shift accelerates, teams need a managed file platform that delivers the performance, resilience, security, and flexibility required to support these workloads in Azure.


[Azure Files](https://azure.microsoft.com/en-us/products/storage/files) helps teams make that transition with fully managed file storage for Linux workloads. It combines familiar file access with built-in performance, data protection, security, and Azure service integration, making it easier to run shared file storage in the cloud without managing file infrastructure directly.


In this blog, we explore how[Azure Files NFS](https://learn.microsoft.com/en-us/azure/storage/files/files-nfs-protocol) supports modern Linux workloads across AI, cloud-native, enterprise modernization, and partner or ISV scenarios.


[What’s new with Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/files-whats-new)


## Speed up AI and data-intensive workloads


AI inferencing is where a trained model is loaded and serves predictions. Before any request is answered, the model’s weights, which capture what the model learned during training, must be read from storage into the serving instance. As models have grown to tens or hundreds of gigabytes, that loading step heavily influences how quickly an endpoint starts and scales. It happens every time a replica spins up, and because GPUs are among the most expensive resources in the stack, every minute they sit idle waiting on weights is money wasted.


Traditionally, teams embed model weights directly in the container image or have each replica download its own copy—producing bloated images, duplicated data, and cold starts that stretch to minutes just when an inference fleet needs to scale fast. With Azure Files, the weights live once on a file share that every replica mounts and reads simultaneously: images stay lean, there’s a single copy to manage, and new replicas come online in seconds for small and mid-sized models. Support for the Linux NFS[nconnect mount option](https://learn.microsoft.com/azure/storage/files/nfs-performance#nfs-nconnect) allows a client to establish multiple parallel TCP connections to the same file share, which can improve throughput at scale.


The **new**[Zonal placement](https://learn.microsoft.com/azure/storage/files/zonal-placement?tabs=azure-portal) lets you co-locate file shares in the same availability zone as your GPU virtual machines, reducing round-trip latency for the data-intensive reads these workloads depend on. The[provisioned v2 billing model](https://learn.microsoft.com/azure/storage/files/understanding-billing#provisioned-v2-model) lets you size IOPS and throughput independently to keep GPUs fed. The result is faster scaling, higher GPU utilization, lower cost per inference, and a more responsive experience for your customers.


## Scale cloud-native applications with shared storage


Cloud-native applications often need shared storage that works directly with Kubernetes-based deployment models. This is especially important for workloads that need ReadWriteMany (RWX) access, persistent shared state, or storage that evolves with the application.


Azure Files integrates with Azure Kubernetes Service through the[Azure Files CSI driver](https://learn.microsoft.com/azure/aks/create-volume-azure-files) , enabling Kubernetes-native workflows such as dynamic provisioning through StorageClasses, expandable persistent volumes, and shared storage across multiple pods with ReadWriteMany access.


The cloud-native story is also about scale and speed. The[new file share experience](https://learn.microsoft.com/azure/storage/files/storage-files-planning#management-concepts) supports up to 10,000 file shares per subscription per region, helping teams support large multi-tenant or high-density environments. It also provisions about 2.5 times faster, with even greater gains in batch deployments, making it easier to scale shared storage quickly as application demand grows. The provisioned v2 model also helps teams manage total cost of ownership as they scale: you can start with smaller shares, grow capacity incrementally as demand rises, and use percentage-based metrics to monitor utilization and right-size shares over time.


Together, these capabilities make Azure Files well suited for cloud-native scenarios such as shared content repositories, configuration and artifact stores, CI/CD workflows, and containerized services that need persistent file storage across replicas or nodes.


[Zooniverse](https://www.microsoft.com/en/customers/story/1376275735613758956-zooniverse-azure-non-profit) , the world’s largest platform for people-powered research, puts this into practice. After migrating from AWS to a fully managed Azure Kubernetes Service environment, the team runs self-hosted Redis instances backed by Azure Files to provide caching and persisted data storage. The platform supports roughly 100 active projects and 10,000 to 15,000 daily users, with traffic that can spike to tens of thousands of API calls per minute. Deployments that once took an hour now run in three to 10 minutes, and the move to managed services on Azure reduced the team’s operational workload by about one full-time engineer.


## Modernize enterprise applications with less disruption


Many enterprise applications built on NFS still depend on familiar Linux and POSIX-style behaviors, stable file semantics, and operational continuity. That makes wholesale refactoring expensive and risky.


Azure Files helps organizations modernize these applications by providing managed NFS file shares for Linux workloads while preserving familiar access patterns. That makes it easier to move existing Linux line-of-business applications like SAP and other workloads that depend on POSIX-compliant file shares, case sensitivity, or Unix-style permissions without forcing teams to refactor them first.


Organizations can use the **new**[Azure Storage Mover](https://learn.microsoft.com/azure/storage/files/migrate-files-storage-mover) and[Azure Migrate support for NFS](https://learn.microsoft.com/azure/storage/files/storage-files-migration-nfs?tabs=ubuntu) to assess, plan, and move Linux-based file workloads into Azure with less disruption to existing operations. Teams that prefer their existing tooling can also run third-party migrations from on-premises NAS using partners such as Komprise, giving Linux file estates more than one path into Azure. The result is a practical, end-to-end path for bringing existing Linux and NFS environments into Azure, where assessment, migration, and modernization happen as one motion rather than a series of disconnected steps.


Once applications are running in Azure, organizations can also strengthen business continuity and security. Features such as[snapshots](https://learn.microsoft.com/azure/storage/files/storage-snapshots-files?tabs=portal#nfs-file-share-snapshots) and[soft delete](https://learn.microsoft.com/azure/storage/files/storage-files-prevent-file-share-deletion?tabs=azure-portal) help improve recovery options and protect business-critical data, while[encryption in transit for NFS](https://learn.microsoft.com/azure/storage/files/encryption-in-transit-for-nfs-shares?tabs=azure-portal%2CUbuntu) helps secure data moving between clients and shares. Combined with Azure networking options such as[VPN and ExpressRoute](https://learn.microsoft.com/azure/storage/files/storage-files-configure-s2s-vpn?tabs=azure-portal) , Azure Files provides resilience, and operational foundation enterprises expect for long-running Linux applications.


The[new file share experience](https://learn.microsoft.com/azure/storage/files/create-file-share?tabs=azure-portal) also simplifies governance, improves cost tracking at the share level, and enables more granular network configuration and role-based access control for different teams and applications.


[Medline](https://www.microsoft.com/en/customers/story/25243-medline-azure) , an over $25 billion leader in medical-surgical supply manufacturing and distribution, shows this modernization path in action. As part of its migration from an on-premises SAP ECC on HANA environment to Azure, Medline built a cloud-native SAP solution leveraging fully managed Azure Files shares to provide zonally resilient shared storage. The re-architecture improved transaction times for key SAP workloads by more than 80 percent, cut response times for the most essential transactions by nearly 60 percent, and accelerated IDoc processing by more than 50 percent.


## Build more with partners and ISVs


A key reason Azure Files fits partner ecosystems is that it speaks standard protocols rather than a proprietary interface. By exposing industry-standard SMB 3.x and NFS 4.1 endpoints, Azure Files works with existing applications, tools, and frameworks without code changes.


Azure Files NFS also expands what partners, ISVs, and platform teams can build on top of the service. Capabilities such as the NFS Snapshot, Soft Delete, and[REST API](https://learn.microsoft.com/rest/api/storageservices/file-service-rest-api) for NFS shares help ecosystem partners integrate Azure Files into broader solutions for BCDR, data movement, management, and application enablement.


In the SaaS and ISV space, Azure Files can also serve as shared storage behind partner applications that need familiar POSIX-style access patterns without forcing customers to manage file infrastructure themselves. These same protocol and data-protection primitives also make Azure Files a target for database backup workloads—including SAP and Oracle backups—that partners and ISVs increasingly run against NFS or REST endpoints.


Azure Files also fits into adjacent developer and platform scenarios. For example, **new**[GitHub-based workflows on Azure Kubernetes Service](https://learn.microsoft.com/azure/aks/github-actions-azure-files-overview) may use shared storage for runners, caching, artifacts, or job state. These scenarios show how Azure Files can bring together application platforms, developer tooling, and shared file storage.


Across AI, cloud-native, enterprise, and ecosystem scenarios, Azure Files NFS gives organizations a single managed file platform for modern Linux workloads. It helps teams support shared storage needs in Azure without stitching together separate solutions for each use case. To get started, explore[Azure Files documentation](https://learn.microsoft.com/azure/storage/files/) . If you would like to learn more or discuss your scenario, contact us atazurefiles@microsoft.com .


## Azure Files


Run shared file storage in the cloud without managing file infrastructure directly.


[Get started](https://azure.microsoft.com/en-us/products/storage/files)
