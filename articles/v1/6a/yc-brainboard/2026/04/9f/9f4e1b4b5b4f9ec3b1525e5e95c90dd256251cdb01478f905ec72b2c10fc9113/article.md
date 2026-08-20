---
schema_version: "1.0.0"
document_id: "9f4e1b4b5b4f9ec3b1525e5e95c90dd256251cdb01478f905ec72b2c10fc9113"
company_key: "yc-brainboard"
company: "Brainboard"
source_id: "yc-brainboard-news-import-c1cb64b4ba70"
canonical_url: "https://www.brainboard.co/blog/working-with-kubernetes-persistent-volumes-without-the-confusion"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-23T04:03:09.323194+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:bdc547b22553f9bf03436bff40de5426f1046843e9990e99546e71983bb4f373"
---

# Working with Kubernetes Persistent Volumes without the Confusion

Containers are ephemeral by nature: when a pod dies, all the data within it is lost. This is fine for stateless applications, but a database that forgets everything every time it restarts is clearly not a viable scenario.


This is exactly why Kubernetes persistent storage exists. Kubernetes persistent volumes and related abstractions allow you to store data independently of pod lifecycles: the information remains even if a pod crashes, restarts, or moves to another cluster node. At the same time, developers don’t need to understand the technical details of how the storage works under the hood.


## What a Persistent Volume Actually Is


Persistent Volume (PV) is a cluster-level resource that represents a block of storage allocated by an administrator or dynamically created by the system. The key term here is “cluster-level”: a PV exists independently of any specific pod and is not tied to its lifecycle.


Kubernetes persistent volumes can be backed by a wide variety of storage types: cloud disks (AWS EBS, Google Persistent Disk, Azure Disk), network storage (NFS, iSCSI), or local node disks. Kubernetes abstracts these differences - to the application, everything looks the same, regardless of what lies beneath.


A PV has its own lifecycle and set of parameters: size, access mode, and reclamation policy. The cluster administrator creates the PV and makes it available - and then PVCs come into play.


## How Persistent Volume Claims Work


If a PV is a storage offer from the cluster, then a Persistent Volume Claim (PVC) is a request for that storage from the application. The developer specifies how much space is needed and what access mode is required, and Kubernetes finds a suitable PV and links them together.


This abstraction is fundamentally important: the developer works with a Kubernetes Persistent Volume Claim without knowing which specific disk will be used - SSD or HDD, cloud-based or local. Infrastructure details are hidden behind a simple interface.


Kubernetes PVC, once bound to a PV, is mounted into a pod as a regular directory. The application writes files, reads data, and all of this is preserved regardless of what happens to the pod itself. It is the Kubernetes PVC that serves as the developer’s primary tool when working with persistent storage.


## Static vs Dynamic Provisioning


There are two ways to create a PV, and understanding the difference between them helps choose the right approach.


**Static provisioning** - the administrator manually creates a set of Persistent Volumes in advance. The developer creates a PVC, and Kubernetes searches among existing PVs for one that matches the request. The downside is obvious: you need to predict in advance what storage will be needed and create it before it is requested.


**Dynamic provisioning** solves this problem. Using a StorageClass, Kubernetes automatically creates a new Persistent Volume the moment a persistent volume claim appears. There is no longer a need to plan - the system allocates exactly the requested storage. This is the approach used in most modern production environments.


## Creating a PV and PVC: YAML Walkthrough


Creating storage in Kubernetes starts with YAML manifests. When describing a PV, the key fields are capacity (disk volume), accessModes (who can mount it and how), storageClassName (which storage class it belongs to), and persistentVolumeReclaimPolicy (what to do with the data after the PVC is deleted).


For Kubernetes create a PVC, the manifest is simpler: we specify the storageClassName, the desired capacity in resources.requests.storage, and accessModes. When you apply this manifest, Kubernetes searches for a suitable PV or creates a new one via the StorageClass.


A Kubernetes PVC is specified in the pod’s spec, volumes section, and mounted inside the container via volumeMounts. This is exactly how the application accesses the persistent directory - it looks like a regular file system to it.


Designing infrastructure with Kubernetes, including storage, becomes significantly more intuitive with[Brainboard](https://www.brainboard.co/) - a platform that allows you to build architecture and automatically generate IaC code visually.


## Access Modes and Reclaim Policies Explained


Kubernetes Persistent Volume supports three access modes, and choosing the right one is critical.


ReadWriteOnce (RWO) - the volume can be mounted with write access by only one node. Suitable for most databases. ReadOnlyMany (ROX) - the volume is mounted read-only by multiple nodes simultaneously. Convenient for static files and configurations. ReadWriteMany (RWX) - the volume is mounted with write access by multiple nodes. Required for shared file systems, but not supported by all storage types.


Reclaim policies determine the fate of data after a PVC is deleted. Retain - data is retained, the PV transitions to Released status, and requires manual cleanup. Delete - the PV and data are automatically deleted. Recycle - data is cleaned up, and the PV becomes available again (an outdated approach, rarely used).


## StorageClasses and Why They Matter


A StorageClass is a template that defines exactly how storage is created during dynamic provisioning. It specifies the provisioner, disk parameters, and reclamation policy.


Different StorageClasses can correspond to different performance levels. For example, fast-ssd with NVMe disks for databases, and standard-hdd for archive data. The developer specifies the desired class in the PVC and receives storage with the required characteristics, without delving into implementation details.


StorageClass also allows you to set the volumeBindingMode policy: immediate PV creation or deferred - until the pod is actually assigned to a node. The second option is useful in multi-zone clusters, where it is important to create the disk in the same zone as the pod.


## Common Mistakes and How to Avoid Them


Several typical issues encountered when working with persistent storage:


- **The PVC is stuck in the Pending status.** Most often, there is no suitable PV or a non-existent StorageClass is specified. Check ‘kubectl describe pvc’ - the exact reason will be listed there.
- **Incorrect access mode.** The application requires ReadWriteMany, but ReadWriteOnce is specified - the pod will not start. Always verify the access mode against the application’s requirements and the storage class’s capabilities.
- **Data loss after PVC deletion.** If the reclamation policy is Delete and you expect to preserve the data - it will be deleted along with the PVC. For production, always use Retain or perform backups.
- **Size mismatch.** The PVC requests 10Gi, but the available PV has 8Gi - the binding will fail. With static provisioning, ensure the sizes match.


[Brainboard](https://www.brainboard.co/use-cases) helps systematically establish proper infrastructure management processes - including storage - with built-in checks and visual monitoring that reduce the likelihood of such errors.


## FAQ


### **What happens to data when a PVC is deleted?**


It depends on the PV reclamation policy. With Retain, the data is preserved, but the PV transitions to Released status and requires manual intervention. With Delete, the PV and data are automatically deleted. Always check the policy before deleting a PVC in production.


### **Can multiple pods use the same Persistent Volume?**


Yes, if the access mode is ReadWriteMany or ReadOnlyMany. With ReadWriteOnce, the volume is mounted with write permissions for only one node - multiple pods on the same node can use it, but not on different nodes.


### **What is the difference between a volume and a Persistent Volume in Kubernetes?**


A regular volume in Kubernetes is tied to the pod’s lifecycle - when the pod is deleted, the volume is deleted as well. A Persistent Volume exists independently of pods and survives their restarts and deletions.


### **Do I need a Persistent Volume for every stateful app?**


Not necessarily for everyone, but for any application that stores important data - yes. Databases, file storage, and message queues with persistence all require a PV. For temporary cache or session data, a regular volume or even an emptyDir is sufficient.


### **How do I resize a Persistent Volume claim?**


You need to make sure that the StorageClass supports allowVolumeExpansion: true. Then simply edit the PVC by increasing the storage value in the resources.requests section. Kubernetes will automatically expand the volume - usually without restarting the pod, though this depends on the storage type.
