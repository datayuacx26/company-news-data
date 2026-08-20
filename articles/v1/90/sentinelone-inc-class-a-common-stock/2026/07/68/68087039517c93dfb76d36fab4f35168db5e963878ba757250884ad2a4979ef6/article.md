---
schema_version: "1.0.0"
document_id: "68087039517c93dfb76d36fab4f35168db5e963878ba757250884ad2a4979ef6"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/mount-here-read-there-twin-path-traversal-cves-in-kubernetes-storage/"
published_at: "2026-07-23T13:00:50+00:00"
first_seen_at: "2026-07-23T13:41:30.567190+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:7fa442c5ea4061d18de8533b0ff4cbfd802ad0ba5d4abeb1152ff9cb6c264218"
---

# Mount Here, Read There: Twin Path Traversal CVEs in Kubernetes Storage

` filepath.Join` was never designed to be a security boundary. We found two CSI drivers that shipped on the assumption it was, and the result was cross-tenant data access with optional node destruction, using nothing more than a valid Kubernetes manifest. The vulnerable drivers are the Kubernetes CSI Driver for NFS (csi-driver-nfs) and the Kubernetes CSI Driver for SMB (csi-driver-smb), both maintained by the upstream kubernetes-csi organization. An attacker who can create a` PersistentVolume` can craft a volume identifier that escapes the subdirectory boundary and reaches another tenant’s data on the shared export.


The impact varies by deployment. In the default configuration, an attacker can read, modify, and delete files belonging to other tenants sharing the same export. In production deployments where the CSI controller is configured with broader hostPath mounts (a common pattern for log collection and operational tooling), the same primitive enables arbitrary directory deletion on the Kubernetes worker node, including paths like` /var/lib/kubelet` ,` /etc/kubernetes` , and` /etc/cni` . Remove those and the node is dead.


The Kubernetes Security Response Committee published advisories and assigned CVE-2026-3864 to the NFS driver issue and CVE-2026-3865 to the SMB driver issue.Fixes shipped in csi-driver-nfs v4.13.1 and csi-driver-smb v1.20.1. We reported both vulnerabilities in January 2026 and worked with the maintainers through coordinated disclosure.


## Kubernetes Storage Concepts


First, some background on how Kubernetes does storage. This section covers the four building blocks an attacker manipulates in this attack chain.


### Container Storage Interface


Kubernetes does not implement storage directly. It delegates that responsibility to the Container Storage Interface, a gRPC contract between the Kubernetes kubelet and a vendor-supplied driver.


Figure 1. The CSI specification sits between Kubernetes and the underlying storage drivers, with each layer owned by a different party.


When a workload requests storage, Kubernetes issues calls such as CreateVolume, NodePublishVolume, and DeleteVolume to the CSI driver, which in turn provisions, mounts, and tears down the actual storage on the underlying system, whether that system is an NFS export, an SMB share, an Amazon Elastic File System filesystem, a block device, or anything else.


Figure 2. The CSI driver mediates between Kubernetes pods and the underlying storage backend.


The CSI specification is intentionally minimal about what drivers must validate. It specifies the gRPC interface and the lifecycle but leaves input validation, authorization, and isolation to each driver implementation. This design decision is the underlying reason the same class of bug recurs across multiple drivers.


### PersistentVolume and the volumeHandle


A` PersistentVolume` is a Kubernetes API object that represents a unit of storage in the cluster. When a` PersistentVolume` references a CSI driver, it carries a string field called volumeHandle. This field is opaque to Kubernetes: the API server stores it but does not parse, validate, or interpret it. The driver alone is responsible for understanding the format of volumeHandle and using its contents safely.


Each driver defines its own format. The NFS CSI driver parses volumeHandle as` {server}#{share}#{subDir}#{uuid}#{onDelete}`


The SMB CSI driver parses it as` //{server}/{share}#{subDir}#{uuid}#{secretNs}#{secretName}#{pvName}`


In both drivers, the subDir component is the security boundary. It’s also the one that breaks.


### Subdirectory-Based Multi-Tenancy


Some clusters share a single NFS or SMB export across tenants by giving each one a dedicated subdirectory. Both drivers’ deployment guides document this pattern. It’s common wherever provisioning a separate export per tenant is expensive: on-premises NAS appliances, cloud file services that bill per share, shared storage systems that are slow to reconfigure.


In this model, the security boundary between tenants is the subdirectory path. Team A’s` PersistentVolume` is scoped to subDir: team-a. Team B’s is scoped to subDir: team-b. The driver mounts each tenant into their own directory, and the assumption is that neither tenant can reach the other’s data, even though both share the same underlying export.


### Role-Based Access Control and PersistentVolume Authorization


Kubernetes uses RBAC to control who does what. Creating a` PersistentVolume` (PV) is cluster-scoped, and the documented threat model assumes only cluster admins hold this permission. In practice? It’s everywhere. CI/CD pipelines, ArgoCD, Helm automation, operator controllers. They all need` persistentvolumes:create` to provision storage for applications. Compromise any of those service accounts and you’re in.


That gap between the documented threat model and how clusters actually run is what makes these bugs practically exploitable.


## Trusting the Wrong Function


There’s a misconception in the Go ecosystem that keeps burning people: the idea that` filepath.Join` prevents path traversal. It doesn’t.` filepath.Join` is a normalizer. It collapses ., .., and redundant separators into a canonical form. It has no concept of “base directory” or “boundary” and can’t tell whether the result landed somewhere the caller never intended.


Try it yourself: give it “` /var/lib/csi/team-a` ” and “` ../../../etc/passwd` “. You get` /etc/passwd` . Clean path, totally canonical, pointing straight at a location outside the intended base directory. The function did what it was designed to do. It just didn’t do what the caller assumed. A safe-path function would take the base directory as a parameter and refuse to return anything outside it. A safe option exists in Go, but these drivers never adopted it. The community workaround is filepath-securejoin, which does the symlink-aware join and returns an error instead of letting you escape.


Both vulnerable drivers shipped this misconception. They pulled the subdirectory string out of volumeHandle, passed it through` filepath.Join` , and trusted the result.


## CVE-2026-3864 — Kubernetes CSI Driver for NFS


We found the bug in the controller-server implementation of csi-driver-nfs. The driver parsed the volume identifier into its parts, pulled out the subdirectory string, and built an internal mount path by joining that subdirectory to a per-volume working directory.


Figure 3 (NFS code): The NFS CSI driver extracts subDir from the volume ID and passes it to filepath.Join without validation, then calls os.RemoveAll on the result.


Three things matter here. No validation on subDir between extraction and use.` filepath.Join` won’t refuse a result that escapes its working directory. And the resulting path gets handed to` os.RemoveAll` . That last part is key: this isn’t just a read primitive. It deletes.


The exploit is a one-line modification to a` PersistentVolume` manifest:


Figure 4 (Exploit YAML): A malicious` PersistentVolume` manifest. The subDir component team-a/../../team-b escapes the legitimate tenant directory.


When a pod mounts this` PersistentVolume` , the path that actually gets bound is 10.0.0.50:/exports/team-b. Not team-a. The attacker’s pod now sees team B’s files, can read them, modify them, and (if the reclaim policy is set to delete) trigger their recursive removal when the` PersistentVolumeClaim` (PVC) is deleted.


In production deployments where the CSI controller has been granted broader hostPath mounts of the worker node (common in clusters that integrate the controller with kubelet log collection or other operational tooling), the traversal can reach beyond the export and into the host filesystem itself. A volumeHandle containing the subdirectory string` ../../../../../../var/lib/kubelet` provides the controller with a recursive-delete primitive against the kubelet’s own state directory, rendering the node permanently non-functional.


Two things make this harder to fix than it looks. First, NFS servers from major vendors expose hidden .snapshot directories. Point-in-time copies, invisible to ls but reachable by path. A subdirectory of team-a/../.snapshot mounts the snapshot tree and surfaces data that admins thought was gone. Second, even after the driver patch, an attacker with write access inside their own tenant directory can plant symlinks that cross the tenant boundary through the NFS data plane. The control-plane fix is necessary but not sufficient. Data-plane mitigations require mounting with` nosymfollow` (Linux 5.10+) or enabling subtree_check on the export.


## CVE-2026-3865 — Kubernetes CSI Driver for SMB


The SMB CSI driver is maintained by the same upstream organization and follows the same architectural shape. It contained the same vulnerability:


Figure 5 (SMB code): The SMB CSI driver follows the same pattern as NFS: subDir extracted at index 1, joined without validation.


The exploit is structurally identical. The volumeHandle for SMB is` //{server}/{share}#{subDir}#` …, and a payload of` //smb.internal/shared#dept-engineering/../../dept-finance#uuid` causes the same escape into a sibling tenant’s directory.


Honestly, the recurrence is the more interesting finding than either CVE on its own. Same org, same misconception, years apart. When a standard-library function looks like a sanitizer but only normalizes, people keep reaching for it. Every codebase that did has to be audited now. Every callsite patched.


One note for other researchers on this bug class. The Kubernetes triage team initially could not reproduce the SMB report because their reproduction harness ran the proof-of-concept against an SMB-server pod inside the cluster, and SMB exports are sandboxed by the underlying storage system. The vulnerability is in the CSI controller’s internal file operations on its working-mount directory, not in protocol traffic to the SMB server. Researchers reporting CSI path traversal should lead with a host-mount reproducer and only attach protocol-level demonstrations as supplementary material. This pattern recurs because the server-side of NFS and SMB is usually well-defended by the storage vendor, while the driver-side code path between the Kubernetes API and the host filesystem is where the bug actually lives.


## Multi-Tenant Storage as Attack Surface


These two CVEs are instances of something bigger. Kubernetes storage drivers sit on a trust boundary that the CSI spec doesn’t enforce. Nobody validates volumeHandle. The kubelet assumes the driver does it. The driver assumes whoever wrote the PV knew what they were doing. That was probably a CI/CD pipeline that assumed the API server would catch anything dangerous. The API server treats the field as opaque. So nobody checks. The string just flows through.


This isn’t just NFS and SMB. There are dozens of CSI drivers in production, each with its own volumeHandle format, each parsing user input with varying degrees of care, each forwarding strings into mount commands and host file operations. We haven’t audited all of them. But the pattern is there.


The bigger question is where input validation should live. The driver? Implemented inconsistently. The API server? Treats volumeHandle as opaque. An admission controller? Only works if someone remembers to deploy one. The CSI spec? Mandates none of the above. Until one of these layers owns the boundary, this bug class will keep producing CVEs.


## The Full Chain — From PersistentVolume-Create to Cross-Tenant Compromise


The full attack chain:


Prerequisite: the attacker holds` persistentvolumes:create` (typically a CI/CD pipeline, GitOps controller, or operator service account) and identifies the cluster as using the NFS or SMB CSI driver with shared multi-tenant exports.


1. Craft a` PersistentVolume` whose` volumeHandle` contains a traversal payload in the subdirectory component.
2. Create a` PersistentVolumeClaim` bound to the malicious PV. The scheduler treats it as normal.
3. Schedule a pod mounting the claim. The CSI driver parses the malicious` volumeHandle` , joins the traversal string against its working directory, and mounts the victim tenant’s directory.
4. Read, modify, or delete the victim’s files. With` onDelete=delete` , deleting the PVC triggers recursive deletion.
5. (Optional) If the CSI controller has broader hostPath mounts, traverse into the host filesystem and destroy node-critical paths like` /var/lib/kubelet` .


The entire chain executes against the Kubernetes API server using only the` persistentvolumes:create` permission. No exploit code. Just YAML.


## Fixes and Mitigations


The Kubernetes Security Response Committee released fixes for both vulnerabilities. The NFS driver fix shipped in csi-driver-nfs v4.13.1 and rejects subdirectory strings containing .. components outright. The SMB driver fix shipped in csi-driver-smb v1.20.1 and applies the same validation pattern.


Beyond the upstream patches, cluster operators have several additional mitigations that should be applied as defense-in-depth:


- Restrict` persistentvolumes:create` authorization. Audit which service accounts in the cluster currently hold this permission. CI/CD pipelines, GitOps controllers, and operator service accounts should be reviewed, and the permission should be scoped down or wrapped behind admission-controller approval for any account that is not strictly an administrative identity.


- Deploy admission-time validation. A` ValidatingAdmissionPolicy` (in Kubernetes 1.30 and later) or a` ValidatingWebhookConfiguration` (in earlier versions) can reject any` PersistentVolume` whose` spec.csi.volumeHandle` field contains .. or other suspicious patterns. This control survives any future zero-day in the same class. The next CSI driver to ship the same misconception is automatically defended.


- Address the data-plane symlink amplification. The driver patches don’t cover the symlink variant. A tenant with write access to their own directory can still plant a relative symlink that crosses into someone else’s. Mount NFS exports with the` nosymfollow` option (Linux 5.10 and later) where supported, enable subtree_check on the NFS server export configuration, or audit symlink targets on the storage server.


- Treat the bug class as recurring. The pattern of` filepath.Join` on attacker-controlled input is present in many storage drivers, container network interface plugins, operators, and webhook servers throughout the cloud-native ecosystem. Any Go code that takes an untrusted string and joins it to a path should be audited and converted to use either strict validation or the filepath-securejoin library.


## Conclusion


Path traversal is one of the oldest bug classes in computing. The fact that it keeps showing up in 2026, in actively maintained infrastructure running multi-tenant production clusters, says something. The safe path-handling already exists, but these drivers kept trusting` filepath.Join` instead of using it.


The CSI specification doesn’t mandate input validation on volume identifiers. Individual drivers implement it inconsistently, or not at all. The protocols underneath (NFS and SMB here, but the pattern generalizes) were designed for trusted networks decades before multi-tenant container orchestration existed. Until one of these layers takes ownership of the boundary, the same misunderstanding will keep shipping.


For defenders: patch the drivers, restrict who can create` PersistentVolumes` , deploy admission-time validation, and treat any .. sequence in a` volumeHandle` as a credible alert. Validators reject. Normalizers don’t. Know which one you’re using.


## Disclosure Timeline


- January 15, 2026 — Vulnerabilities identified during CSI driver code review by SentinelOne Researchers.
- January 16, 2026 — NFS and SMB reports submitted to the Kubernetes SecurityResponse Committee.
- January 19, 2026 — NFS report triaged after proof-of-concept harness was clarified.
- January 19, 2026 — SMB report triaged.
- March 9, 2026 — NFS driver fix released in csi-driver-nfs v4.13.1.
- March 17, 2026 — Public disclosure of the NFS finding; CVE-2026-3864 assigned.
- March 21, 2026 — SMB driver fix released in csi-driver-smb v1.20.1.
- April 11, 2026 — Public disclosure of the SMB finding; CVE-2026-3865 assigned.


## Additional Resources


- [CVE-2026-3864 — Kubernetes NFS CSI Driver path traversal](https://github.com/kubernetes/kubernetes/issues/137797) (Kubernetes Security Advisory)
- [CVE-2026-3865 — Kubernetes SMB CSI Driver path traversal](https://github.com/kubernetes/kubernetes/issues/138319) (Kubernetes Security Advisory)
- [filepath-securejoin](https://github.com/cyphar/filepath-securejoin) — Secure path-joining library for Go
