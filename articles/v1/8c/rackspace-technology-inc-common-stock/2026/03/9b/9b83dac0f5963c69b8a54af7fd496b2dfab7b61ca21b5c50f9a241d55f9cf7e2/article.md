---
schema_version: "1.0.0"
document_id: "9b83dac0f5963c69b8a54af7fd496b2dfab7b61ca21b5c50f9a241d55f9cf7e2"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/cinder-csi-vs-ceph-rbd-csi-in-kubernetes-an-analysis-of-persistent-volume-lifecycle-performance-on-rackspace-spot"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T10:47:01.830546+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:604727de5ccbffc07b079de30d54e3a92665b4389cb3d7d75c4ccd300be605e3"
---

# Cinder CSI vs Ceph RBD CSI in Kubernetes: An Analysis of Persistent Volume Lifecycle Performance on Rackspace Spot

[Rackspace Spot](https://spot.rackspace.com/) recently[introduced a new storage class](https://spot.rackspace.com/blogs/why-kubernetes-ceph-csi-for-rackspace-spot?utm_source=chatgpt.com) , *spot-ceph* , a[Ceph](https://ceph.io/en/) -backed distributed storage option for Kubernetes workloads.[Early feedback from users has been positive](https://github.com/rackerlabs/spot/discussions/283#discussioncomment-15905447) , particularly around faster pod startup times and more predictable volume behavior.


*spot-ceph* was introduced in response to operational issues observed in older storage classes backed by[OpenStack Cinder](https://docs.openstack.org/cinder/latest/?utm_source=chatgpt.com) where volume lifecycle operations did not complete as expected. Some of the issues reported were “[Pods stuck in pending due to PVC](https://github.com/rackerlabs/spot/discussions/251) ” and “[Persistent Volume stuck in ‘attaching’ status](https://github.com/rackerlabs/spot/discussions/214) ”.


To understand the root cause, this article examines the architectural and operational differences between Cinder (OpenStack block storage **)** and Ceph RBD (Ceph block storage **)** as Kubernetes storage classes on Rackspace Spot.


The analysis reproduces volume attachment and detachment flows and traces state transitions across Kubernetes, the Container Storage Interface(CSI) layer, and the underlying storage backend. The objective is to identify where state drift or transition latency occurs.


To be clear, this is not a throughput benchmark. Instead, the focus is on lifecycle reliability: *how quickly volumes attach, how consistently they detach, and how these behaviors impact pod startup time. T* he results show that Ceph RBD attaches and detaches volumes significantly faster than Cinder under identical conditions. This article breaks down the architectural reasons behind this difference.


‍


## Storage concepts and architecture in Kubernetes


Before diving into the analysis, it's worth going through some foundational concepts. To meaningfully compare Cinder and Ceph CSI, understanding what the Container Storage Interface (CSI) is, what a CSI driver does, and how Kubernetes orchestrates volume provisioning and attachment across its different layers is essential. With that in place, the architectural differences and test observations will be much easier to follow.


‍


### What are volumes in Kubernetes?


Local storage inside a Pod is ephemeral. When the Pod is deleted, the data disappears with it. However, some workloads require data that outlives the Pod and survives rescheduling events. This is where volumes come in.


A volume allows a Pod to access storage that exists outside the container's writable layer, providing persistence beyond the lifecycle of the container.


‍


### Why do volumes need a storage backend?


A Kubernetes volume is an abstraction. It defines how a Pod should access storage, but does not store data by itself. Every volume must be backed by a real storage system that handles the actual persistence. How Kubernetes connects to that storage system is what the rest of this section covers.


‍


### What are Persistent Volumes (PVs)?


A[PersistentVolume (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) is a cluster-level resource that represents a real storage asset provisioned for use in Kubernetes. It is the bridge between the abstract volume a Pod needs and the actual storage backend providing it, whether that is a block device, a network filesystem, or a distributed storage system.


A PV exists independently of any specific Pod. It defines properties such as capacity, access mode, and reclaim policy, and remains available until it is released or deleted according to its lifecycle rules.


‍


### What is a Persistent Volume Claim (PVC)?


A PersistentVolumeClaim (PVC) is a request for storage from the cluster. It specifies the required storage size and access mode. When a PVC is created, Kubernetes either binds it to an existing matching PV or, if a StorageClass is specified, dynamically provisions a new one. Once bound, the PVC acts as a stable reference that Pods use to mount storage, and that binding persists even if the Pod is deleted and recreated.


In short:


- A PV represents the storage.
- A PVC requests and claims that storage.
- The Pod consumes the PVC.


With PVCs establishing how storage is requested, the next question is: *how does Kubernetes communicate with the underlying storage backend to fulfill a PVC request?* That is where the Container Storage Interface comes in.


‍


### What is the Container Storage Interface (CSI)?


The[Container Storage Interface](https://kubernetes-csi.github.io/docs/) (CSI) is a universal standard that defines how container orchestrators like Kubernetes communicate with storage systems. Rather than building storage integrations directly into Kubernetes, CSI defines a set of Remote Procedure Calls (RPCs) that Kubernetes calls and that storage vendors must implement. This separation means any storage system with a CSI-compliant driver can work with any CSI-compliant orchestrator (in this case, Kubernetes) without either side needing to know the internal details of the other.


The specification defines exactly what parameters are sent by the caller, what responses are expected, and what error codes are valid for each operation. These operations include:


- **CreateVolume** — Kubernetes calls this RPC when a Pod requires a volume, passing details such as the volume name and required capacity. The storage driver implements this call, provisions the volume on its backend, and returns the result.
- **DeleteVolume** — Kubernetes calls this RPC when a volume is to be removed. The storage driver implements the code to decommission the volume from the backend when the call is made.
- **ControllerPublishVolume** — called when a volume needs to be attached to a node.
- **ControllerUnpublishVolume** — called when a volume needs to be detached from a node.


‍


### What are CSI drivers?


A[CSI driver](https://kubernetes-csi.github.io/docs/drivers.html) is the implementation of the CSI standard for a specific storage system. It is the component that receives RPC calls from Kubernetes and translates them into operations against a particular storage backend, whether that is Ceph, OpenStack Cinder, AWS EBS, or any other system. Kubernetes uses the` CSIDriver` resource to register and interact with these drivers within the cluster.


Different CSI drivers manage the lifecycle of volume attachments differently. When a pod requests a persistent volume, Kubernetes orchestrates a multi-step process involving volume provisioning, attachment to a node, and finally mounting into the container. If any step in this lifecycle encounters delays, partial failures, or state inconsistencies, the entire pod startup process stalls.


‍


### What are StorageClasses?


A StorageClass is how Kubernetes exposes a CSI driver to users. It references a provisioner (the CSI driver) and defines configuration parameters such as performance characteristics and reclaim behavior. When a PVC specifies a StorageClass, Kubernetes uses the associated CSI driver to dynamically provision a PV by making the appropriate RPC calls against the underlying storage backend. By defining multiple StorageClasses, a cluster can support different storage profiles with varying performance and cost characteristics.


‍


### The managed storage layer in Rackspace Spot


In Rackspace Spot, CSI drivers are installed and managed by the platform. Users do not deploy or configure them directly. Instead, a StorageClass is specified in the PVC, and Kubernetes uses the corresponding CSI driver to provision and mount storage from the underlying backend.


Cinder-backed StorageClasses provision volumes through OpenStack Cinder against region-specific storage backends. The spot-ceph StorageClass provisions volumes directly from platform-managed Ceph clusters. In both cases, the user only selects a StorageClass and the platform handles the rest.


‍


## How CSI drivers work in Kubernetes


With the core concepts in place, we can now look at how a CSI driver is actually structured and how it coordinates volume operations across Kubernetes and the storage backend. Understanding these internals is important because when a volume fails to attach or detach, the failure can originate at any one of these layers.


A CSI driver consists of two main components:


- **Controller component** — runs as a Deployment and handles cluster-wide storage operations such as provisioning and attaching volumes. It communicates with the storage backend API directly.
- **Node plugin** — runs as a DaemonSet on every node and handles node-local operations such as mounting and unmounting volumes on the host.


These components are accompanied by sidecar containers that act as the bridge between Kubernetes and the CSI driver:


- **External provisioner** — watches for new PVCs and calls the` CreateVolume` RPC on the CSI driver when dynamic provisioning is required
- **External attacher** — watches for new VolumeAttachment objects and calls` ControllerPublishVolume` and` ControllerUnpublishVolume` on the CSI driver to attach and detach volumes


The CSI driver sits between Kubernetes and the storage backend, translating Kubernetes storage operations into backend API calls over gRPC.


**Kubernetes → CSI Driver → Storage Backend**


## What happens when a volume is created?


When a PVC is created and requires dynamic provisioning, the following sequence occurs:


1. The external-provisioner sidecar detects the PVC
2. It calls the CSI driver's` CreateVolume` RPC over gRPC
3. The CSI driver executes its internal implementation of` CreateVolume`
4. The driver calls the storage backend API (Cinder, Ceph, or otherwise)
5. The backend provisions the volume
6. The CSI driver returns a` CreateVolumeResponse`
7. The external-provisioner updates the Kubernetes API and creates the PV
8. Kubernetes binds the PV to the PVC


This sequence forms the baseline for understanding what follows. The experiment in this article focuses specifically on steps 3 through 7, where the behavior of Cinder and Ceph diverge significantly.


‍


## OpenStack Cinder CSI architecture


Before the analysis, it is worth understanding how Cinder is structured, because its architecture directly explains the behavior observed during volume attachment and detachment.


Cinder is OpenStack's block storage service, responsible for provisioning and managing volumes. It is not the CSI driver itself. The CSI driver,` cinder.csi.openstack.org` , sits between Kubernetes and Cinder, translating Kubernetes storage calls into Cinder API operations. Cinder then coordinates with its configured backend storage systems, which may vary by region.


What this diagram shows is that attaching a single volume in Cinder is not a single operation. It requires coordination across five independent control plane layers.


1. **Kubernetes layer** — creates the VolumeAttachment object and calls` ControllerPublishVolume()` via the external-attacher
2. **CSI driver layer** — receives the gRPC call and sends the` os-attach` REST API request to Cinder
3. **Cinder layer** — validates volume state, sets it to` attaching` , creates an attachment record, and calls Nova
4. **Nova layer** — creates a Block Device Mapping (BDM) and instructs the compute service to attach the device
5. **Hypervisor layer** — hotplugs the block device, updates the VM configuration, and exposes the device inside the instance.


Each layer must complete its operation and propagate state to the next before the attachment can finalize.


‍


## Ceph CSI (RBD) architecture


The Ceph CSI driver,` rbd.csi.ceph.com` , takes a fundamentally different path. Rather than coordinating across multiple independent services, it communicates directly with the Ceph cluster. The CSI driver maps the RBD image directly on the node and returns success once the mapping is confirmed.


Now that the architectural differences between Cinder and Ceph CSI are established, the analysis traces how Cinder's multi-layer control plane coordination and Ceph's direct attachment path affect volume attachment latency, detachment reliability, and overall pod lifecycle behavior in a cluster.


## An analysis of persistent volume lifecycle performance: Cinder CSI vs Ceph RBD


The analysis is centered on the following observations:


1. **Initial attachment behavior** : When a newly created pod mounts a freshly provisioned volume for the first time, how do the CSI drivers perform?
2. **Reattachment behavior** : When a newly created pod requests a volume that was recently released by a deleted pod, how do retry attempts, error handling, and state synchronization affect pod time-to-ready?
3. **Detachment behavior** : When a pod is deleted, how quickly and reliably does each CSI driver release the volume and clean up Kubernetes attachment objects?
4. **Tracing state transitions across Kubernetes and the storage backend:** How do Kubernetes and the CSI driver coordinate state with the underlying storage backend, and where do inconsistencies introduce delays or failures?
5. **Architectural differences** : What fundamental design choices in each CSI implementation lead to the observed performance characteristics?


To capture these observations, a delete-and-recreate cycle is followed:


1. Observe the pod in a running state with both volumes successfully attached
2. Force-delete the pod and monitor detachment behavior across both CSI drivers
3. Recreate the pod and monitor reattachment behavior across both CSI drivers
4. Capture detailed state at each phase: Kubernetes events, VolumeAttachment objects, CSI driver logs, and API error responses


Timestamps are recorded at each state transition, errors are classified by their origin (Kubernetes, CSI driver, or storage backend API), and state propagation is traced across the control plane layers involved in each volume operation.


### Setting up the test environment


The cluster is running, so the available StorageClasses are confirmed first:


```text
kubectl get storageclasses
```


Next, the PVCs are deployed:


```text
apiVersion: v1
kind  : PersistentVolumeClaim
metadata  :
name: cinder-gen1-pvc
namespace  : pv-dual-test
spec  :
accessModes:
- ReadWriteOnce
storageClassName  : ssd
resources  :
requests:
storage: 10Gi
---
apiVersion: v1
kind  : PersistentVolumeClaim
metadata  :
name: ceph-pvc
namespace  : pv-dual-test
spec  :
accessModes:
- ReadWriteOnce
storageClassName  : spot-ceph
resources  :
requests:
storage: 10Gi
```


Next, the pod manifest is deployed:


```text
apiVersion: v1
kind  : Pod
metadata  :
name: dual-writer
namespace  : pv-dual-test
labels  :
app: dual-writer
spec  :
terminationGracePeriodSeconds:   5
containers  :
- name: writer
image  : busybox:  1.36
command  :
- sh
- -c
- |
set -eux


mkdir -p /mnt/cinder /mnt/ceph
echo   "starting dual writes: $(date -Iseconds)"   | tee -a /mnt/cinder/log.txt /mnt/ceph/log.txt


while     true  ;   do
# Write small chunks frequently to keep both mounts active
dd   if  =  /dev/u  random   of  =  /mnt/  cinder/blob.bin bs=4k count=  128   conv=fsync   2  >  /dev/  null   ||   true
dd   if  =  /dev/u  random   of  =  /mnt/  ceph/blob.bin   bs=4k count=  128   conv=fsync   2  >  /dev/  null   ||   true


date -Iseconds | tee -a /mnt/cinder/log.txt /mnt/ceph/log.txt
sleep   1
done
volumeMounts  :
- name: cinder-vol
mountPath  :   /mnt/  cinder
- name: ceph-vol
mountPath  :   /mnt/  ceph


volumes  :
- name: cinder-vol
persistentVolumeClaim  :
claimName: cinder-gen1-pvc
- name: ceph-vol
persistentVolumeClaim  :
claimName: ceph-pvc
```


The manifest file above defines two Persistent Volume Claims (PVCs): one backed by a gen-1 Cinder storage class (cinder-gen1-pvc) and one backed by spot-ceph (ceph-pvc), and a Pod that mounts both volumes simultaneously. This allows us to observe how each storage class behaves under identical scheduling and node conditions, particularly during Pod creation and deletion, when attachment and detachment operations are triggered.


Next, check the VolumeAttachment objects for both PVCs:


```text
kubectl get volumeattachments -o wide
```


The Ceph-backed volume shows` attached: true` , while the Cinder-backed volume still reports` attached: false` .


The Pod remains in` ContainerCreating` because all volumes are not fully attached and this delay is coming directly from Cinder.


‍


Describing the Pod reveals the following events:


```text
kubectl -n pv-dual-test describe pod dual-writer | head -  80
```


```text
Events:
TYPE      REASON                   MESSAGE
Normal    Scheduled                Successfully assigned pv-dual-test/dual-writer to prod-instance-  17724290682921461
Normal    SuccessfulAttachVolume   AttachVolume.Attach succeeded   for   volume   "pvc-0ec55d46-dff8-4e46-bb15-f9d36e1789ca"
Warning   FailedAttachVolume       AttachVolume.Attach failed   for   volume   "pvc-ee80f713-4675-4d79-b495-f36fa0ffc37e"   : rpc error: code = Internal desc = [ControllerPublishVolume] failed to attach volume: [  2026  -  03  -02T14:  29  :21Z] [attempt-  1  ] Volume 1927ee12-2f13-4d9d-800e-ca76e1718dc6 is not yet attached to instance 9f270f30-e77a-  4798  -a3a1-c3e6d0ff8ffd
Warning   FailedAttachVolume       AttachVolume.Attach failed   for   volume   "pvc-ee80f713-4675-4d79-b495-f36fa0ffc37e"   : rpc error: code = Internal desc = [ControllerPublishVolume] Attach Volume failed   with   error failed to attach 1927ee12-2f13-4d9d-800e-ca76e1718dc6 volume to 9f270f30-e77a-  4798  -a3a1-c3e6d0ff8ffd compute: Bad request   with  : [POST https:  //csi-proxy-prod.spot.rackspace.com/ord.servers/v2/1339069/servers/9f270f30-e77a-4798-a3a1-c3e6d0ff8ffd/os-volume_attachments], error message: {"badRequest": {"message": "Invalid volume: volume '1927ee12-2f13-4d9d-800e-ca76e1718dc6' status must be 'available'. Currently in 'attaching'", "code": 400}}
Warning   FailedAttachVolume       AttachVolume.Attach failed   for   volume   "pvc-ee80f713-4675-4d79-b495-f36fa0ffc37e"   : rpc error: code = Internal desc = [ControllerPublishVolume] Attach Volume failed   with   error failed to attach 1927ee12-2f13-4d9d-800e-ca76e1718dc6 volume to 9f270f30-e77a-  4798  -a3a1-c3e6d0ff8ffd compute: Bad request   with  : [POST https:  //csi-proxy-prod.spot.rackspace.com/ord.servers/v2/1339069/servers/9f270f30-e77a-4798-a3a1-c3e6d0ff8ffd/os-volume_attachments], error message: {"badRequest": {"message": "Invalid volume: volume '1927ee12-2f13-4d9d-800e-ca76e1718dc6' status must be 'available'. Currently in 'in-use'", "code": 400}}
```


Under the` Events` section, the Ceph-backed PVC attaches successfully, while the Cinder-backed PVC repeatedly fails during attachment with the following errors:


` Volume is not yet attached to instance`


` status must be 'available'. Currently in 'attaching'", "code": 400`


` status must be 'available'. Currently in 'in-use'", "code": 400`


‍


***What is causing these errors?***


The diagram below traces the full attachment sequence across all five control plane layers involved in a Cinder volume operation. Each error maps to a specific point in this sequence where state has not yet propagated to the next layer. Referencing this diagram while reading through each error will make it clear exactly where the breakdown is occurring.


**Attempt 1:**` Volume is not yet attached to instance`


- Kubernetes created the` VolumeAttachment` object and called` ControllerPublishVolume()` .
- The Cinder CSI driver received the gRPC call and sent the` os-attach` REST API request to Cinder.
- Cinder accepted the request and began the attachment workflow:


- Validated the volume was` available`
- Set the volume state to` attaching`
- Created an attachment record in its database
- Called Nova to begin block device mapping


At this point, the backend work has started but the attachment has not yet completed at the compute layer. Nova had not yet finalized the block device mapping, so when the CSI driver polled Cinder for the attachment status, it found the operation incomplete. The CSI driver returned the failure to the external-attacher, which triggered Kubernetes to retry.


‍


**Attempt 2:**` status must be 'available'. Currently in 'attaching'`


Between Attempt 1 and Attempt 2, something important has changed:


- The first attach request moved the volume state from` available` to` attaching`
- The attach workflow is still in progress
- Nova and the hypervisor are still completing device mapping


At this stage, the multi-layer orchestration is progressing in the background:


- Cinder has created an attachment record in its database
- Cinder has called the Nova API to attach the volume to the target instance
- Nova is creating a Block Device Mapping (BDM) entry
- Nova Compute is instructing the hypervisor via libvirt to hotplug the block device
- The hypervisor is updating the VM configuration and exposing the device inside the instance


Kubernetes retries the attach request on its periodic reconciliation loop before this workflow completes. The CSI driver sends another attach request to Cinder, but this time:


- Cinder checks the volume state
- The volume is no longer` available` , it is in` attaching`
- Cinder rejects the request and will not allow a second attach while the first is still processing
- The CSI driver returns the failure to the external-attacher, which triggers another Kubernetes retry


This is the Kubernetes reconciliation loop colliding with a backend whose transitional states last longer than one retry interval.


Until all of these layers complete successfully and report back, Cinder keeps the volume in the` attaching` state. Because Kubernetes retries attachment on a reconciliation loop, a new attach request while the last request is still in progress. Cinder then rejects the retry because the volume is no longer` available` , but not yet fully finalized either.


‍


**Attempt 3:**` status must be 'available'. Currently in 'in-use'`


Between Attempt 2 and Attempt 3, the volume state has progressed again:


- The backend attachment completed
- Nova created the Block Device Mapping
- The hypervisor hotplugged the disk
- Cinder updated the volume state to` in-use`


At this point, the backend believes the volume is successfully attached. However, Kubernetes has not yet observed a successful attach response and retries again.


‍


***Why did Kubernetes retry if Cinder already transitioned the volume to` in-use` ?***


For Kubernetes to mark the volume as attached, all of the following must occur in sequence:


1. The CSI driver must return success from` ControllerPublishVolume()`
2. The external-attacher sidecar must receive that success
3. The external-attacher must update the` VolumeAttachment` object in the Kubernetes API
4. Kubernetes must observe that update and mark the volume as` attached: true`


If any of these steps do not complete during a retry attempt, Kubernetes does not update state and the external-attacher retries again. So when the CSI driver sends another` os-attach` request, Cinder checks the volume state and rejects it because the volume is already` in-use` and no longer` available` . The CSI driver returns the failure to the external-attacher, triggering yet another retry.


‍
The error progression tells the full story:


- ` attaching` — the backend accepted the request and the workflow was in progress
- ` in-use` — the backend completed the attachment, but Kubernetes had not yet converged


This is backend state advancing faster than Kubernetes reconciliation. The attachment completed at the infrastructure layer, but the Cinder control plane did not report the operation as fully finalized until roughly 32 seconds later. The final` attached=true` state was only set at the next successful retry around the 70-second mark, once Cinder returned a clean success response.


‍


Reviewing the events shows that the attachment failed three times before eventually succeeding, indicating that the Cinder-backed volume required multiple retries before reaching a stable attached state.


**Summary of delays**


- **Backend execution delay** — Cinder, Nova, and the hypervisor completing the actual attachment across all infrastructure layers
- **Control plane convergence delay** — Cinder waiting for all layers to finalize before marking the volume stable and returning success to the CSI driver
- **Kubernetes reconciliation delay** — the external-attacher waiting for the next successful` ControllerPublishVolume()` response before updating` VolumeAttachment.status.attached = true`


‍


### Volume Cleanup and Detachment Behavior on Pod Deletion


With the attachment behavior analyzed, the next observation is how each CSI driver handles cleanup when the Pod is deleted.


When the Pod is deleted, Kubernetes triggers the` ControllerUnpublishVolume()` call through the external-attacher, initiating the detachment workflow across the same control plane layers involved during attachment.


Checking the` VolumeAttachment` objects after pod deletion is the first indicator of how differently each CSI driver handles this process.


**Ceph CSI:**


- ` VolumeAttachment` completely removed immediately after pod deletion
- No lingering` VolumeAttachment` object


**Cinder CSI:**


- ` VolumeAttachment` still exists with` ATTACHED=true` after pod deletion
- Checking the event logs shows:` failed to detach volume... Resource not found: volume_id not found: 8526f8d3-9b8e-41f0-9394-a3533e681c7d (404)`


**Ceph detachment (~10 seconds)**


In the Ceph case, detachment completed in approximately 10 seconds with no retries and no conflicting intermediate states. The CSI driver communicated directly with the Ceph cluster, and once the RBD mapping was removed from the node, Ceph acknowledged the detach immediately. The external-attacher then cleared the finalizer. Kubernetes observed the finalized state and deleted the` VolumeAttachment` object. The entire operation is self-contained within the Ceph cluster and the CSI driver.


**Cinder detachment (~75 seconds)**


In contrast, Cinder detachment took approximately 75 seconds to clean up the volume.


When the Pod is deleted, Kubernetes sets a` deletionTimestamp` on the` VolumeAttachment` object, marking it for deletion. However, the external-attacher has placed a finalizer on the object that prevents Kubernetes from deleting it until the CSI driver confirms the volume has been fully detached on the storage backend. Until that confirmation arrives, the` VolumeAttachment` object remains in a terminating state.


For the CSI driver to confirm that detachment is complete, the following sequence must first finish at the backend:


1. Cinder marks the volume as transitioning
2. Nova removes the block device mapping
3. The hypervisor detaches the device
4. Cinder finalizes the volume state
5. The CSI driver returns a successful` ControllerUnpublishVolume()`
6. The external-attacher removes the finalizer


Only after all of this does Kubernetes delete the` VolumeAttachment` object.


The 75-second delay reflects the time Kubernetes had to wait for the Cinder layers to fully complete their detach workflow before the VolumeAttachment object could be finally removed.


‍


### Volume reattachment behavior on pod rescheduling


When the Pod was recreated, both VolumeAttachment objects were created simultaneously. The Ceph-backed volume attached immediately with no retries or errors. In contrast, the Cinder-backed volume entered the same retry pattern observed during the initial attachment. The first attempt failed with “volume not yet attached,” followed by additional failures while the volume transitioned through` attaching` and` in-use` states, before finally succeeding. The Pod started running only after the full attach workflow completed, taking roughly 76 seconds.


‍


### Why Ceph does not experience these delays?


Unlike Cinder, Ceph does not rely on Nova, hypervisor orchestration, or cross-service state reconciliation. The attach and detach workflows are handled directly between the CSI driver and the Ceph cluster. Specifically, Ceph requires no involvement from:


- Nova API
- Block device mapping orchestration
- Hypervisor attach workflows
- Cross-service state reconciliation


This results in fewer transitional states and a single control plane to coordinate, which is why Ceph attachments complete in under a second with no retries, while Cinder must wait for multiple independent layers to fully complete before Kubernetes can proceed.


‍


## Performance summary


- **Detach phase** : Cinder requires **75 seconds** ; Ceph completes in **10 seconds** with clean removal
- **Attach phase (initial)** : Cinder requires **70 seconds** with 3 retry failures due to state conflicts; Ceph completes in **<1 second** with a single successful attempt
- **Attach phase (reattachment)** : Cinder requires **71 seconds** with 3 retry failures (identical pattern); Ceph completes in **<1 second** with a single successful attempt
- **End-to-end pod rescheduling** : **151 seconds** (Cinder: 75s detach + 76s reattach) versus **11 seconds** (Ceph: 10s detach + 1s reattach) - a **13.7x performance improvement**


Scenario Operation Cinder (seconds) Ceph (seconds) Difference Comparative Result


**Initial Pod Startup** First Attach 78s ~1s ~77s Ceph completed attachment ~78× faster


**Pod Deletion** Detach 75s 10s 65s Ceph completed detachment ~7.5× faster


**Pod Rescheduling** Reattach 76s ~1s ~75s Ceph completed reattachment ~76× faster


**PVC Provisioning** CreateVolume ~2s ~1s ~1s Comparable


‍


## From single volume to hundreds: how Cinder CSI behaves at scale


A single Cinder attachment already introduced ~70 seconds of delay and multiple retries. Now consider what happens when this pattern is multiplied across hundreds of volumes during rolling deployments, autoscaling events, node drains, or failure recovery.


Each volume must independently traverse the same sequence: Cinder state validation, Nova coordination, hypervisor attachment, and final state propagation back to Kubernetes. With hundreds of volumes, these sequences run concurrently, multiplying the API calls and database writes that already caused retries and transitional state collisions in our single-volume observation.


The key insight is not just that Cinder was slower, but that its architecture introduces more synchronization boundaries, each of which adds propagation delay. In high-churn environments, these effects amplify. Ceph's shorter control path keeps convergence time low making its behavior significantly more predictable at scale.


‍


## Conclusion


And this is precisely why Rackspace introduced **spot-ceph** as a new storage class.


The goal was not simply to offer another storage option, but to reduce control-plane complexity, minimize state transition latency, and provide more predictable lifecycle behavior for Kubernetes workloads. By leveraging managed Ceph clusters with a shorter and more direct attachment path, spot-ceph eliminates several coordination layers present in the Cinder architecture. Fewer synchronization boundaries mean faster convergence, fewer retry collisions, and more consistent time-to-ready during attach, detach, and rescheduling operations.


For high-churn Kubernetes environments, especially those running stateful workloads, this architectural simplification translates directly into operational reliability. Faster attachments improve startup times. Faster detachments improve recovery and node recycling. And more deterministic state reconciliation reduces the likelihood of volumes becoming stuck in transitional states. In essence, spot-ceph was introduced to align the storage control plane more closely with the speed and concurrency model that Kubernetes demands.


Get started with[Rackspace Spot](https://spot.rackspace.com/) and experience faster, more predictable volume operations at any scale.
