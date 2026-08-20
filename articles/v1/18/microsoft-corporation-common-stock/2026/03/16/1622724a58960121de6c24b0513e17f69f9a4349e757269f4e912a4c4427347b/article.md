---
schema_version: "1.0.0"
document_id: "1622724a58960121de6c24b0513e17f69f9a4349e757269f4e912a4c4427347b"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0db2ae40c128"
canonical_url: "https://opensource.microsoft.com/blog/2026/03/24/whats-new-with-microsoft-in-open-source-and-kubernetes-at-kubecon-cloudnativecon-europe-2026/"
published_at: "2026-03-24T07:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.025355+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:ee5491d1abb61e90da2be3042e10689fbf5d423fb9225e28c1db452a5d78c122"
---

# What’s new with Microsoft in open-source and Kubernetes at KubeCon + CloudNativeCon Europe 2026

There’s a pattern in how complex technology matures. Early on, teams make their own choices: different tools, different abstractions, different ways of reasoning about failure. It looks like flexibility but at scale it reveals itself as fragmentation.


The fix is never just more capability; it’s shared operational philosophy. Kubernetes proved this. It didn’t just answer “how do we run containers?” It answered “how do we change running systems safely?” The community built those patterns, hardened them, and made them the baseline.


AI infrastructure is still in the chaotic phase. The shift from “working versus broken” to “good answers versus bad answers” is a fundamentally different operational problem, and it won’t get solved with more tooling. It gets solved the way cloud-native did: open source creating the shared interfaces and community pressure that replace individual judgment with documented, reproducible practice.


That’s what we’re building toward. Since my[last update at KubeCon + CloudNativeCon North America 2025](https://opensource.microsoft.com/blog/2025/11/10/whats-new-with-microsoft-in-open-source-and-kubernetes-at-kubecon-north-america-2025/) , our teams have continued investing across open-source AI infrastructure, multi-cluster operations, networking, observability, storage, and cluster lifecycle. At KubeCon + CloudNativeCon Europe 2026 in Amsterdam, we’re sharing several announcements that reflect that same goal: bring the operational maturity of Kubernetes to the workloads and demands of today.


[Learn more about Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service)


## Building the open source foundation for AI on Kubernetes


The convergence of AI and Kubernetes infrastructure means that gaps in AI infrastructure and gaps in Kubernetes infrastructure are increasingly the same gaps. A significant part of our upstream work this cycle has been building the primitives that make GPU-backed workloads first-class citizens in the cloud-native ecosystem.


On the scheduling side, Microsoft has been collaborating with industry partners to advance open standards for hardware resource management. Key milestones include:


- **Dynamic Resource Allocation (DRA)** has graduated to general availability, with the DRA example driver and DRA Admin Access also shipping as part of that work.
- **Workload Aware Scheduling for Kubernetes 1.36** adds[DRA support in the Workload API](https://github.com/kubernetes/enhancements/blob/master/keps/sig-scheduling/5729-resourceclaim-support-for-workloads/README.md) and drives integration into KubeRay, making it more straightforward for developers to request and manage high-performance infrastructure for training and inference.
- **DRANet** now includes upstream compatibility for Azure RDMA Network Interface Cards (NICs), extending DRA-based network resource management to high-performance hardware where GPU-to-NIC topology alignment directly affects training performance.


Beyond scheduling, we’ve continued investing in the tooling needed to deploy, operate, and secure AI workloads on Kubernetes:


- [AI Runway](https://github.com/kaito-project/kubeairunway) is a new open-source project that introduces a common Kubernetes API for inference workloads, giving platform teams a centralized way to manage model deployments and adopt new serving technologies as the ecosystem evolves. It ships with a web interface for users who shouldn’t need to know Kubernetes to deploy a model, along with built-in HuggingFace model discovery, GPU memory fit indicators, real-time cost estimates, and support for runtimes including NVIDIA Dynamo, KubeRay, llm-d, and KAITO.
- [HolmesGPT](https://www.cncf.io/blog/2026/01/07/holmesgpt-agentic-troubleshooting-built-for-the-cloud-native-era/) has joined the Cloud Native Computing Foundation (CNCF) as a Sandbox project, bringing agentic troubleshooting capabilities into the shared cloud-native tooling ecosystem.
- [Dalec](https://github.com/project-dalec/dalec) , a newly onboarded CNCF project, defines declarative specifications for building system packages and producing minimal container images, with support for SBOM generation and provenance attestations at build time. Reducing attack surface and common vulnerabilities and exposures at the build stage matters for any organization trying to run AI workloads responsibly at scale.
- [Cilium](https://github.com/cilium/) also received a broad set of Microsoft contributions this cycle, including native mTLS ztunnel support for sidecarless encrypted workload communication, Hubble metrics cardinality controls for managing observability costs at scale, flow log aggregation to reduce storage volume, and two merged Cluster Mesh Cilium Feature Proposals (CFPs) advancing cross-cluster networking.


## What’s new in Azure Kubernetes Service


In addition to our upstream contributions, I’m happy to share new capabilities in[Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service) (AKS) across networking and security, observability, multi-cluster operations, storage, and cluster lifecycle management.


### From IP-based controls to identity-aware networking


As Kubernetes deployments grow more distributed, IP-based networking becomes harder to reason about: visibility degrades, security policies grow difficult to audit, and encrypting workload communication has historically required either a full-service mesh or a significant amount of custom work. Our networking updates this cycle close that gap by moving security and traffic intelligence to the application layer, where it’s both more meaningful and easier to operate.


[Azure Kubernetes Application Network](https://aka.ms/aks/application-network) gives teams mutual TLS, application-aware authorization, and detailed traffic telemetry across ingress and in-cluster communication, with built-in multi-region connectivity. The result is identity-aware security and real traffic insight without the overhead of running a full-service mesh. For teams managing the deprecation of ingress-nginx,[Application Routing with Meshless Istio](https://aka.ms/aks/app-routing/gateway-api) provides a standards-based path forward: Kubernetes Gateway API support without sidecars, continued support for existing ingress-nginx configurations, and contributions to ingress2gateway for teams moving incrementally.


At the data plane level,[WireGuard encryption](http://aka.ms/acns/wg) with the Cilium data plane secures node-to-node traffic efficiently and without application changes.[Cilium mTLS in Advanced Container Networking Services](https://aka.ms/acns/cilium-mtls) extends that to pod-to-pod communication using X.509 certificates and SPIRE for identity management: authenticated, encrypted workload traffic without sidecars. Rounding this out,[Pod CIDR expansion](https://aka.ms/aks/pod-cidr-expansion) removes a long-standing operational constraint by allowing clusters to grow their pod IP ranges in place rather than requiring a rebuild, and administrators can now[disable HTTP proxy](https://aka.ms/aks/http-proxy) variables for nodes and pods without touching control plane configuration.


### Visibility that matches the complexity of modern clusters


Operating Kubernetes at scale is only manageable with clear, consistent visibility into infrastructure, networking, and workloads. Two persistent gaps we’ve been closing are GPU telemetry and network traffic observability, both of which become more critical as AI workloads move into production.


Teams running GPU workloads have often had a significant monitoring blind spot: GPU utilization simply wasn’t visible alongside standard Kubernetes metrics without manual exporter configuration.[AKS now surfaces GPU performance and utilization](https://aka.ms/aks/managed-gpu-metrics) directly into managed Prometheus and Grafana, putting GPU telemetry into the same stack teams are already using for capacity planning and alerting. On the network side, per-flow L3/L4 and supported L7 visibility across HTTP, gRPC, and Kafka traffic is now available, including IPs, ports, workloads, flow direction, and policy decisions, with a[new Azure Monitor experience](https://learn.microsoft.com/en-us/azure/aks/container-network-observability-logs) that brings built-in dashboards and one-click onboarding. For teams dealing with the inverse problem (metric volume rather than metric gaps) operators can now dynamically control which container-level metrics are collected[using Kubernetes custom resources](https://learn.microsoft.com/en-us/azure/aks/container-network-observability-metrics?tabs=Cilium#container-network-metrics-filtering-preview) , keeping dashboards focused on actionable signals.[Agentic container networking](https://learn.microsoft.com/en-us/azure/aks/advanced-container-networking-services-overview) adds a web-based interface that translates natural-language queries into read-only diagnostics using live telemetry, shortening the path from “something’s wrong” to “here’s what to do about it.”


### Simpler operations across clusters and workloads


For organizations running workloads across multiple clusters, cross-cluster networking has historically meant custom plumbing, inconsistent service discovery, and limited visibility across cluster boundaries. Azure Kubernetes Fleet Manager now addresses this with[cross-cluster networking](https://aka.ms/kubernetes-fleet/networking/cross-cluster) through a managed Cilium cluster mesh, providing unified connectivity across AKS clusters, a global service registry for cross-cluster service discovery, and intelligent routing with configuration managed centrally rather than repeated per cluster.


On the storage side, clusters can now[consume storage from a shared Elastic SAN pool](https://learn.microsoft.com/en-us/azure/storage/container-storage/use-container-storage-with-elastic-san) rather than provisioning and managing individual disks per workload. This simplifies capacity planning for stateful workloads with variable demands and reduces provisioning overhead at scale.


For teams that need a more accessible entry point to Kubernetes itself,[AKS desktop](https://learn.microsoft.com/en-us/azure/aks/aks-desktop-overview) is now generally available. It brings a full AKS experience to your desktop, making it straightforward for developers to run, test, and iterate on Kubernetes workloads locally with the same configuration they’ll use in production.


### Safer upgrades and faster recovery


The cost of a bad upgrade compounds quickly in production, and recovery from one has historically been time-consuming and stressful. Several updates this cycle focus specifically on making cluster changes safer, more observable, and more reversible.


Blue-green agent pool upgrades create a parallel pool with the new configuration rather than applying changes in place, so teams can validate behavior before shifting traffic and maintain a clear rollback path if something looks wrong. Agent pool rollback complements this by allowing teams to revert a node pool to its previous Kubernetes version and node image when problems surface after an upgrade (without a full rebuild). Together, these give operators meaningful control over the upgrade lifecycle rather than a choice between “upgrade and hope” or “stay behind.” For faster provisioning during scale-out events,[prepared image specification](https://aka.ms/preparedimagespecification) lets teams define custom node images with preloaded containers, operating system settings, and initialization scripts, reducing startup time and improving consistency for environments that need rapid, repeatable provisioning.


## Connect with the Microsoft Azure team in Amsterdam


The Azure team are excited to be at KubeCon + CloudNativeCon Europe 2026. A few highlights of where to connect with the Azure team on the ground:


- [Rules of the Road for Shared GPUs: AI Inference Scheduling at Wayve](https://sched.co/2HgE0) —Customer keynote, Tuesday, March 24, 2026, 9:37 AM CET.
- [Scaling Platform Ops with AI Agents: Troubleshooting to Remediation](https://sched.co/2HgFq) —Tuesday, March 24, 2026, 10:13 AM CET with Jorge Palma, Principal PDM Manager, Microsoft.
- [Building cross-cloud AI inference on Kubernetes with OSS](https://sched.co/2IEHs) —Wednesday, March 25, 2026, 1:15 PM CET with Jorge Palma, Principal PDM Manager, Microsoft and Anson Qian, Principal Software Engineer, Microsoft.
- Visit our booth #200 for live demos and conversations with the Azure and AKS team.
- Or browse the[full schedule of sessions](https://aka.ms/KubeConEU2026) by Microsoft speakers throughout the week.


Happy KubeCon + CloudNativeCon!


[Get started with Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service)
