---
schema_version: "1.0.0"
document_id: "e28504111bc4732968812eec639989e1f06486a0c4bfd82f2998fdb6efadab24"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0db2ae40c128"
canonical_url: "https://opensource.microsoft.com/blog/2026/06/17/whats-new-with-microsoft-in-open-source-and-kubernetes-at-open-source-summit-and-kubecon-india/"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.025355+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:7957b915a72dc754935bd251cc0cc8b5bf150062684e2de7d700d9d94b774095"
---

# What’s new with Microsoft in open source and Kubernetes at Open Source Summit and KubeCon India

When building with AI on Kubernetes, getting a model running is the starting line. The engineering shows up in the months after: serving traffic you didn’t forecast, rolling out model and platform changes without downtime, and holding costs down while the system keeps shifting underneath you.


That work raises the bar on infrastructure. Training and inference put real money on every idle accelerator, so utilization stops being a vanity metric. Estates grow from a handful of clusters to hundreds, so governance has to scale without a matching increase in headcount. And the cost of a bad upgrade rises, because a stalled training run or a reloaded inference server is far more painful to recover than restarting a stateless web service.


The updates we’re sharing this week at Open Source Summit and KubeCon + CloudNativeCon India target exactly that operational layer, across the open source foundation and[Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service) . Several of them build on work first shared at KubeCon + CloudNativeCon Europe 2026 and Open Source Summit North America 2026 earlier this year.


[Learn what you can do with Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service)


## Building the open source foundation for agentic systems


Agents have to run somewhere, and most of them land on a Kubernetes cluster on top of a Linux host. The more autonomous they become, the more that foundation has to be predictable and secure by default. We made two announcements at Open Source Summit North America 2026, with broader rollout at Microsoft Build 2026, that are worth repeating:


- **Azure Container Linux** is generally available (GA). It is a minimal, immutable, container-optimized operating system maintained by Microsoft, with a smaller package footprint that cuts patching overhead and limits drift as fleets grow.
- **Azure Linux 4.0** is in public preview on Azure Virtual Machines, giving teams a hardened base for cloud-native and AI workloads.


We have also continued working in the open on the building blocks an agent stack needs to stay portable across frameworks, clouds, and runtimes: the **Microsoft Agent Framework** (an open-source SDK and runtime for multi-agent systems), the **Agent Governance Toolkit** for identity, policy, and audit, and the **Agentic AI Foundation** , where Microsoft is a founding member. The pattern mirrors how Kubernetes got enterprise ready: the runtime came first, then the governance primitives that made it safe to operate. The full set is in the[Open Source Summit North America 2026 recap](https://opensource.microsoft.com/blog/2026/05/18/from-open-source-to-agentic-systems-microsoft-at-open-source-summit-north-america-2026/) .


## What’s new in Azure Kubernetes Service


Alongside the open-source work, we continue to ship new capabilities in Azure Kubernetes Service (AKS) across cluster lifecycle, multi-cluster operations, GPU efficiency, faster provisioning, and agent-assisted operations:


### Run upgrades you can reverse


The cost of a bad upgrade compounds in production, and recovery has historically meant a manual reprovision under pressure.[Agent pool rollback](https://learn.microsoft.com/en-us/azure/aks/roll-back-node-pool-version) is now generally available: one command reverts both the Kubernetes version and the node image to their previous state, across all node pool types and OS SKUs, with no snapshots to manage. For drains that stall,[max blocked nodes](https://learn.microsoft.com/en-us/azure/aks/upgrade-options) lets you set how many nodes can fail to drain before an upgrade stops, cordoning the stuck ones while the rest of the pool keeps moving, so drain failures are no longer all-or-nothing.


To stay ahead of support cutoffs, AKS now publishes[end-of-support notifications](https://learn.microsoft.com/en-us/azure/aks/aks-eol-notifications) to Azure Resource Graph with no per-cluster setup, giving you a fleet-wide view of exposure ready to wire into Azure Monitor alerts. And cluster extension auto-upgrades now honor maintenance windows, with an optional patch-only mode that applies security fixes without advancing minor versions.


### Operate large fleets as one system


Most teams run many clusters, often across regions and clouds, and the differences tend to show up as operational inconsistency. Azure Kubernetes Fleet Manager closes several of those gaps this cycle.[Managed Fleet Namespaces](https://aka.ms/kubernetes-fleet/managed-namespaces) are now generally available: define a namespace once as an Azure Resource Manager resource, with optional quotas, network policies, and Microsoft Entra ID access, then place it across clusters as an immutable unit. A single fleet now supports[up to 1,000 member clusters](https://aka.ms/kubernetes-fleet/) , up from 200, so teams that split clusters to work around the old limit can consolidate.


[Fleet Manager for Arc-enabled clusters](https://aka.ms/kubernetes-fleet/arc-enabled) , announced in GA at Microsoft Build 2026, extends that control to any CNCF-compliant distribution running through Azure Arc, including workload placement across hybrid and multi-cloud estates. On the update path:


- [Approval gates](https://aka.ms/kubernetes-fleet/safe-updates/approval-gates) add required sign-off before and after stages.
- A new[target Kubernetes version channel](https://aka.ms/kubernetes-fleet/auto-upgrade/target-minor) holds clusters on a chosen minor while still applying patches (useful for long-term support clusters).
- Preview[update-run improvements](https://aka.ms/kubernetes-fleet/safe-updates) add a security-patch channel along with finer control over concurrency and scheduling.


### Get more out of your GPUs


GPU capacity is expensive and frequently underused, with accelerators sitting idle between requests and workloads spread thinly across nodes.[Configurable scheduler profiles](https://learn.microsoft.com/en-us/azure/aks/concepts-scheduler-configuration) expose the upstream Kubernetes scheduling framework through an AKS-managed custom resource to pack pods more densely without running your own scheduler.[GPU memory profiling](https://aka.ms/aks/gpuprofiling) , in public preview, adds function-level visibility into GPU memory through Prometheus and Grafana, to tune allocation and catch leaks before an out-of-memory crash. Several Build 2026 announcements reinforce this layer:


- [Managed system node pools in AKS Automatic](https://learn.microsoft.com/en-us/azure/aks/automatic/aks-automatic-managed-system-node-pools-about) take core system components off your workload nodes, reducing resource contention when capacity is constrained.
- [Anyscale on Azure](https://azure.microsoft.com/en-us/solutions/anyscale?msockid=19ea681fff4368ed14267f45fe326933) brings managed Ray to AKS for distributed training and inference with fractional and heterogeneous GPU allocation.
- [AKS on bare metal](https://learn.microsoft.com/en-us/azure/aks/aksarc/aks-bare-metal-overview) , in preview, runs without a hypervisor for direct NVLink and RDMA access on the most demanding jobs.


### Provision faster, and rebuild less


When pods wait on multi-gigabyte image pulls, scale out that should take seconds stretches into minutes.[Artifact streaming](https://aka.ms/artifactstreaming) means AKS streams only the layers needed for startup directly from Azure Container Registry, making pod startup concurrent rather than serial. In our testing, images under 10 GB dropped from minutes to seconds to start.[Node auto-provisioning](https://learn.microsoft.com/en-us/azure/aks/node-auto-provisioning) adds two preview capabilities, support for fixed-size node pools alongside autoscaling ones and custom OS configurations, and you can now apply a[Capacity Reservation Group to an existing node pool](https://learn.microsoft.com/en-us/azure/aks/use-capacity-reservation-groups) in place rather than recreating it.


For Windows workloads,[Windows Server 2025](https://aka.ms/aks/upgrade-windows-os-version) is generally available: run it alongside Windows Server 2022 in the same cluster, migrate incrementally, and run existing 2022 containers on the newer host without image rebuilds, with native GPU acceleration for CUDA workloads in Windows containers.


### Let agents take the first pass


When an AKS incident fires, operators often spend the first hour issuing discovery commands and correlating evidence before they can even form a hypothesis.[Azure SRE Agent](https://aka.ms/aks-sre-agent) now covers AKS scenarios in preview: it gathers the evidence, attributes the failing layer (workload, cluster, network, or Azure dependency), and proposes a concrete next step, across the highest-volume incident families and the upgrade lifecycle. Writes are approval-gated and audited, and read-only diagnostics work on private clusters today through the az aks invoke bridge. The point is not to replace the operator, but to hand them one ranked finding instead of three hypotheses to chase.


The thread running through all of these announcements is open foundations, change you can make safely and observe, and operations that scale past the limits of any single expert, making the day-to-day of running AI on Kubernetes more predictable, more efficient, and safer to change.


## Where to get started


- Spin up a cluster and explore the latest capabilities in the[Azure Kubernetes Service documentation](https://learn.microsoft.com/en-us/azure/aks/) .
- Manage multi-cluster operations from one place with[Azure Kubernetes Fleet Manager](https://aka.ms/kubernetes-fleet/) .
- Read the broader announcements from[Open Source Summit North America 2026](https://opensource.microsoft.com/blog/2026/05/18/from-open-source-to-agentic-systems-microsoft-at-open-source-summit-north-america-2026/) and[Microsoft Build 2026](https://techcommunity.microsoft.com/blog/appsonazureblog/whats-new-in-azure-kubernetes-service-at-microsoft-build-2026/4524862) .


## Azure Kubernetes Service


Build and run AI predictably and efficiently on AKS


[Get started](https://azure.microsoft.com/en-us/products/kubernetes-service)
