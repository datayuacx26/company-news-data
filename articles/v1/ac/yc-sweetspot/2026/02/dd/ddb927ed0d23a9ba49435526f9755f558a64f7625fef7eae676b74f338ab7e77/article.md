---
schema_version: "1.0.0"
document_id: "ddb927ed0d23a9ba49435526f9755f558a64f7625fef7eae676b74f338ab7e77"
company_key: "yc-sweetspot"
company: "Sweetspot"
source_id: "yc-sweetspot-news-import-95ae5efddaf0"
canonical_url: "https://www.sweetspot.so/articles/hardening-kubernetes-for-cmmc/"
published_at: "2026-02-20T00:00:00+00:00"
first_seen_at: "2026-07-24T03:25:11.786592+00:00"
fetched_at: "2026-07-28T22:19:36.583779+00:00"
content_hash: "sha256:a402b9fdbd59df3db453dcc26ec8f700b925ce16d954db39b27d721f1ed5055a"
---

# Hardening Kubernetes for CMMC: The Configurations That Matter

Kubernetes out of the box is not CMMC-ready.


The defaults are designed for developer productivity, not for protecting Controlled Unclassified Information. Mutable container tags, broad node-level IAM roles, accessible instance metadata, general-purpose operating systems with full shell access. Every one of these defaults is a gap that must be closed before you’re anywhere near NIST SP 800-171 compliance.


The following configurations are roughly in order of how often we see them missed.


Configuration CMMC Control Why It Matters


Immutable node OS CM.L2-3.4.7, CM.L2-3.4.8 No shell, no package manager, eliminates post-exploitation persistence


FIPS-validated node images SC.L2-3.13.11 Every crypto operation on the node uses validated modules


IMDSv2 enforcement AC.L2-3.1.1, SC.L2-3.13.1 Prevents SSRF-based credential theft from containers


Workload identity (IRSA) AC.L2-3.1.5, AC.L2-3.1.6 Pod-level IAM instead of node-level, true least privilege


Image digest pinning SI.L2-3.14.1, CM.L2-3.4.1 Immutable image references prevent supply chain substitution


Encrypted storage MP.L2-3.8.1 All volumes encrypted: OS, data, and ephemeral


Non-root containers AC.L2-3.1.5, CM.L2-3.4.7 Minimal base images, no build tools in production


Control plane logging AU.L2-3.3.1 Every K8s API call captured for audit and incident response


## Immutable Node Operating Systems


Most Kubernetes clusters run on general-purpose Linux distributions like Amazon Linux, Ubuntu, or RHEL. These are full operating systems with package managers, shells, system utilities, cron, and everything else an attacker needs to establish persistence after gaining access to a node.


Purpose-built container operating systems take a different approach. They ship with only what’s needed to run containers: a kernel, a container runtime, and a kubelet. No shell, no package manager, no SSH daemon, no mechanism for runtime modification. If the OS needs to change, you build a new image and replace the node.


This eliminates entire categories of post-exploitation techniques. An attacker who gains access to a node finds nothing useful. No tools to download additional payloads, no package manager to install them with, no cron to schedule persistence. The only way to modify the OS is through the node image build pipeline, which is controlled, auditable, and automated.


For CMMC, this maps directly to CM.L2-3.4.7 (restrict nonessential programs) and CM.L2-3.4.8 (apply deny-by-exception policies).


## FIPS-Validated Node Images


Running an immutable OS isn’t enough if its cryptographic modules aren’t FIPS-validated. The node must boot with FIPS mode enforced, meaning the kernel’s cryptographic self-tests run at boot and all system cryptographic operations route through FIPS-validated modules.


This extends to everything running on the node: the container runtime, the kubelet, and any cloud SDK calls should be configured to use FIPS endpoints. The node image build pipeline should verify FIPS mode is active as a gate before promoting the image.


Rebuilding these images weekly ensures OS-level security patches are applied on a known cadence. Each rebuild goes through the same FIPS verification gate, so you never ship a node image that lost FIPS enforcement.


## Instance Metadata Lockdown


Cloud instance metadata services (IMDS) are a common target for SSRF attacks. If a containerized application has an SSRF vulnerability, an attacker can reach the metadata service to steal the node’s IAM credentials, which in many configurations grant broad access to cloud resources.


IMDSv2 (on AWS) requires a token for metadata access, which raises the bar for SSRF-based credential theft. The token request uses a PUT with a TTL header, and the response hop limit controls how many network hops the token can traverse.


For containerized workloads, the hop limit must be set correctly. Too low and your pods can’t reach the metadata service legitimately. Too high and you’ve defeated the purpose. This is a configuration that needs to be set on every node class in your cluster.


## Workload Identity Over Node-Level IAM


Traditional cloud IAM assigns roles at the node level. Every pod running on that node inherits those permissions. This violates least privilege. Your logging sidecar doesn’t need the same S3 permissions as your document processing service.


Workload identity federation (IRSA on AWS, Workload Identity on GCP, Pod Identity on Azure) assigns IAM roles at the pod level via OIDC federation. Each service gets exactly the permissions it needs, scoped to its own identity. A compromised pod can only access the resources its specific role allows, not everything the node can reach.


This isn’t optional for CMMC. AC.L2-3.1.5 (least privilege) and AC.L2-3.1.6 (use non-privileged accounts) require it. Node-level IAM roles are, by definition, more privileged than any individual workload needs.


## Image Digest Pinning


Container image tags are mutable pointers.` myapp:v1.2.3` points to a specific image today, but nothing prevents it from being overwritten to point to different content tomorrow. In a supply chain attack, an attacker who gains write access to your registry can replace a tagged image with a compromised one, and every subsequent pod deployment pulls the malicious version.


Image digests (SHA256 content hashes) are immutable.` myapp@sha256:abc123...` will always reference exactly the same image content. It cannot be overwritten, redirected, or tampered with. Your deployment manifests should reference digests, not tags.


Combined with immutable tag policies on your container registry (preventing tag overwrites) and scan-on-push (scanning every image for vulnerabilities before it enters the registry), this creates a supply chain integrity guarantee: the image that was scanned is the image that runs. No substitution possible.


## Encrypted Storage on Every Volume


Every block device attached to every node (OS volumes and data volumes) must be encrypted at rest. This must be applied across every node class: general-purpose, compute-optimized, GPU, spot, on-demand. It’s easy to configure encryption on your primary node group and forget to apply it to a specialty node class added later.


This maps to CMMC MP.L2-3.8.1 (protect CUI on system media). If CUI touches a node, even transiently in a container’s ephemeral storage, the underlying volume must be encrypted.


## Hardened Application Containers


The container runtime is only as secure as the containers it runs. Application images should be built on minimal base images with a reduced attack surface, not full OS distributions with compilers, debuggers, and system utilities.


Containers must run as a dedicated application user with no login shell and no supplementary group memberships. Never run as root in production. Use multi-stage builds where the build stage includes compilers, package managers, and build tooling, and the runtime stage copies only the built artifacts. No build tools ship to production.


Choose base images from organizations with established security advisory pipelines. RHEL UBI Minimal, Google’s distroless images, and Chainguard’s Wolfi-based images are all viable options. The fewer packages in the base image, the smaller the CVE surface area.


## Control Plane Logging


Kubernetes audit logs capture every API call to the control plane: who did what, when, and from where. These logs are essential for incident response and for demonstrating compliance with CMMC AU.L2-3.3.1 (system auditing).


All control plane log types should be enabled: API server, audit, authenticator, controller manager, and scheduler. These should flow to a durable log store with a retention period that meets your compliance requirements. 365 days is a reasonable baseline for CMMC.


Control plane logs only capture Kubernetes API calls. Application-level audit logging (who accessed which documents, which AI models were invoked, what data was returned) requires separate instrumentation at the application layer.


---


None of these configurations are individually difficult. The challenge is getting all of them right across every node class, every container image, every deployment manifest, and every new service, and keeping them right as your cluster evolves. A new node class gets added without FIPS enforcement. A new image gets built on a base image with a shell. A deployment manifest references a mutable tag instead of a digest.


At Sweetspot, these configurations are enforced through infrastructure-as-code and automated pipelines. Every node boots from a verified FIPS image. Every container runs as non-root on a minimal base. Every image is pinned by digest and scanned on push. Every volume is encrypted. When a new node class or service gets added, the same automation applies the same posture, with no manual checklists.


You shouldn’t need a platform engineering team to run compliant AI infrastructure. We already built one, so you can use frontier AI models on government contracts without thinking about IMDSv2 hop limits or image digest pinning.
