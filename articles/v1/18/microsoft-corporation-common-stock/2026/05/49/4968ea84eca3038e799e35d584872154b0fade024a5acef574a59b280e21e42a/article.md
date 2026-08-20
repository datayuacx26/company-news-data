---
schema_version: "1.0.0"
document_id: "4968ea84eca3038e799e35d584872154b0fade024a5acef574a59b280e21e42a"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0db2ae40c128"
canonical_url: "https://opensource.microsoft.com/blog/2026/05/18/from-open-source-to-agentic-systems-microsoft-at-open-source-summit-north-america-2026/"
published_at: "2026-05-18T15:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.025355+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:3e7885fc5f520d8ef677c55bda046b06cce1f0e6a16de004f3f96d25af890b3a"
---

# From open source to agentic systems: Microsoft at Open Source Summit North America 2026

Open source is the foundation for AI and, as AI workloads scale, developers need that foundation to be more secure, more predictable, and easier to build apps and agents.


At[Open Source Summit North America 2026](https://events.linuxfoundation.org/open-source-summit-north-america/) , we’re announcing two updates that strengthen exactly that: the upcoming public preview of **[Azure Linux 4.0](https://aka.ms/AzureLinuxForm) on Azure Virtual Machines** and the **general availability of Azure Container Linux** , our immutable container-optimized operating system (OS), with the broader rollout at[Microsoft Build on June 2](https://build.microsoft.com/en-US/home) . Together, they give developers and organizations a hardened Linux distribution purpose-built for cloud native and AI workloads.


That foundation is also what makes the next chapter possible. The move from cloud native to AI native is the next evolution of open source, **** and it’s the focus of my keynote this week, *From Open Source to Agentic Systems: Building the AI Native Era.* Open source is already at the core of AI today, and AI in turn is reshaping how open source itself gets built, from how we collaborate, to how we test, to how the developer experience comes together. We’ve done this before. We know what it takes to build an open ecosystem at scale, and we know that openness is what makes it work.


[Be the first to try Azure Linux 4.0](https://aka.ms/AzureLinuxForm)


## How open source built the modern cloud: Linux, Kubernetes, and containers


Linux,[Kubernetes](https://azure.microsoft.com/en-us/products/kubernetes-service) , and containers have made the modern cloud possible. Every hyperscaler, every AI training cluster, every inference endpoint serving millions of tokens a second is built on open source. Open standards, shared governance, and community innovation have been the way an ecosystem of this scale comes together, allowing the best ideas, from anywhere, to compound.


For Microsoft, this story started in earnest in 2009,[when we contributed more than 20,000 lines of Hyper-V driver code to the Linux kernel](https://azure.microsoft.com/en-us/blog/microsofts-open-source-journey-from-20000-lines-of-linux-code-to-ai-at-global-scale/?msockid=26a0ddfb5425612d0982cbb655a260cf) . A small patch then, but a clear signal of where we were headed.


Today, **more than two-thirds of customer cores in Azure run Linux** , and the platforms running Microsoft 365, GitHub, and OpenAI’s ChatGPT all sit on Linux foundations. When ChatGPT scales across more than 10 million compute cores worldwide and serves a billion queries a day, Linux and Kubernetes are what make that possible.


## Azure Linux 4.0 and Azure Container Linux: A secure Linux foundation for cloud native and AI workloads


For developers running modern workloads on Azure, the OS layer should be invisible: secure by default, consistent across hosts and containers, and out of your way. That’s what[Azure Linux](http://aka.ms/AzureLinuxForm) and Azure Container Linux are designed to do.


Both are hardened, with a reduced package footprint, transparent supply chain, and consistent performance characteristics from the host all the way up to the container. Teams running regulated or security-sensitive workloads get a smaller attack surface and a Linux distribution maintained by the same team that operates the cloud it runs on. Azure Linux 4.0 is a Fedora-derived, RPM-based Linux distribution built and maintained by Microsoft. It’s open source, free to use, and optimized specifically for Azure infrastructure. Azure Container Linux is based on the[Flatcar](https://github.com/flatcar/Flatcar) project. And because we develop in the open and contribute upstream first, the work that hardens Azure Linux benefits the broader ecosystem too.


## How AI is reshaping open source development


AI isn’t just a new workload sitting on top of open source; it’s changing how open source itself gets built.


- Maintainers are using coding agents to triage issues, generate tests, and review PRs.
- Agentic tooling is starting to handle the toil associated with dependency updates and security patches.
- And the contribution loop is opening to more developers, in more languages, at a faster cadence than we have ever seen.


That’s a good thing for the ecosystem, but it raises the bar on the fundamentals: provenance, review discipline, supply chain integrity, and clear standards. The communities that figure out how to fold AI into their workflows while keeping the trust model intact are the ones who will define the next decade of open source.


## Building an open agentic stack: Frameworks, protocols, and governance for AI agents


Delivering agentic systems at global scale takes collaboration across the open source ecosystem. Agents need to work everywhere developers build—across frameworks, clouds, languages, and runtimes. That kind of portability only happens when the foundations are open.


That’s why we are working alongside the open source community on the building blocks of an open agentic stack:


- **[Microsoft Agent Framework](https://learn.microsoft.com/en-us/agent-framework/overview/?pivots=programming-language-csharp)** : Our open source SDK and runtime for building, deploying, and managing multi-agent systems. It carries forward the lessons of Semantic Kernel and AutoGen into a single foundation that maps cleanly from local development to cloud deployment, with the observability, evaluation, and lifecycle primitives production systems need.
- [Ray](http://ray.io/) **and[NVIDIA Dynamo](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/nvidia-dynamo-on-aks---autoscaling-llm-inference/4499545)** : Partnerships and contributions that let agents and AI workloads compose across the most widely adopted open frameworks in the ecosystem.
- **A2A (agent-to-agent) protocols** : Open interfaces so agents from different vendors, frameworks, and clouds can communicate, delegate, and coordinate.
- **[Agent Governance Toolkit](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/)** : The control-plane primitives (identity, policy, audit, access boundaries) that let organizations deploy agents responsibly. Just as Kubernetes needed RBAC and admission controllers to be enterprise-ready, agentic systems need governance primitives and those primitives belong in the open.


Those building blocks need a shared standards body to keep them interoperable. That is where the Agentic AI Foundation comes in.


## The Agentic AI Foundation: Open standards for agent interoperability


The **[Agentic AI Foundation (AAIF)](https://aaif.io/)** is already the **fastest-growing project in Linux Foundation history** . Microsoft is a founding member, and we believe deeply in its mission: establishing open standards for agent-to-agent communication, agent runtimes, and agent orchestration.


The AAIF builds on and complements what the[Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) has done for cloud native—the two are designed to work together. The reason this is happening so quickly is straightforward: customers and the broader community are asking for interoperability. They don’t want to bet their agentic future on a single vendor’s stack and open standards are how we make sure they do not have to.


The early momentum across industry and academia tells you how much the ecosystem wants this to be open. As customers scale multi-agent systems composed of custom built and third party agents, interoperability becomes essential to truly deliver on the business transformation goals. The agentic future cannot be proprietary, and the AAIF is how we make sure it isn’t.


## Securing the open source supply chain for AI


None of this works if the underlying ecosystem isn’t trustworthy. The same projects that power the cloud and AI also power critical infrastructure, and the people maintaining them are often a handful of volunteers in their spare time. As agents become more autonomous, every dependency they touch becomes part of their trust boundary. Securing open source isn’t just hygiene anymore. It’s a prerequisite for letting AI agents do real work.


That’s why Microsoft has made a sustained, multi-phase investment in **OpenSSF and Alpha-Omega** :


- A **kick-start investment** to seed Alpha-Omega’s mission of improving the security posture of critical open source software through expert engagement and automated security testing.
- A **second round of funding** to Alpha-Omega and OpenSSF to scale sustainable **, AI-powered open source security solutions** , using the same agentic capabilities we’re building elsewhere to harden the supply chain itself.


We’re also a founding partner in the **[GitHub Secure Open Source Fund](https://github.blog/news-insights/company-news/announcing-github-secure-open-source-fund/)** , which pairs direct financial support ($10,000 per project) with a three-week program of security education, mentorship from GitHub Security Lab, tooling, and ongoing check-ins. The model is designed to scale; invest in maintainers as people, not just packages, and the security improvements compound across the dependency graph.


## Kubernetes and CNCF: Where Microsoft contributes upstream


For three years running, **Microsoft Azure has been the**[largest public cloud contributor and the second-largest contributor overall to CNCF projects](https://all.devstats.cncf.io/d/5/companies-table?orgId=1&var-period_name=Last%203%20years&var-metric=contributions) **.**


That includes core upstream work in[Kubernetes](https://github.com/kubernetes/kubernetes) ,[Helm](https://github.com/helm/helm) ,[containerd](https://github.com/containerd/containerd) ,[Istio](https://github.com/istio/istio) , and[Envoy](https://github.com/envoyproxy/envoy) —the runtime layer the entire cloud-native ecosystem depends on, alongside community projects we contribute to heavily, like[OpenTelemetry](https://github.com/open-telemetry) ,[ArgoCD](https://github.com/argoproj/argo-cd) ,[HolmesGPT](https://github.com/HolmesGPT/holmesgpt) ,[OPA Gatekeeper](https://github.com/open-policy-agent/gatekeeper) , and[Cilium](https://github.com/cilium/cilium) as well as projects we’ve launched and donated:


- [Dapr](https://github.com/dapr/dapr) : Graduated CNCF project for cloud-agnostic distributed applications.
- [KAITO](https://github.com/kaito-project/kaito) : Kubernetes AI Toolchain Operator for deploying and fine-tuning models on AKS and at the edge.
- [KubeFleet](https://github.com/kubefleet-dev/kubefleet) : Multi-cluster orchestration with smart scheduling and progressive rollouts.
- [Radius](https://github.com/radius-project/radius) : Application-centric platform spanning Azure, AWS, and private clouds.
- [Drasi](https://github.com/drasi-project/drasi-platform) : Change-driven workflows over real-time data.
- [Copacetic](https://github.com/project-copacetic/copacetic) : Supply-chain-focused container patching.
- [Dalec](https://github.com/project-dalec/dalec) : Declarative format for building system packages and containers in a secure way.
- [Flatcar](https://github.com/flatcar/Flatcar) : Container-optimized Linux, accepted into CNCF at the incubating level.
- [Headlamp](https://github.com/kubernetes-sigs/headlamp) : **** Kubernetes dashboard UI for managing and visualizing clusters for workloads.
- [Inspektor Gadget](https://github.com/inspektor-gadget) : **** eBPF-powered observability toolkit for deep Kubernetes and container runtime insights.


Every one of these projects started with a problem we hit running Kubernetes on Azure at scale. When we do work in the open, we get better solutions, and the broader community strengthens and benefits from the work too.


## From cloud-native to AI-native: The open source principles that carry over


The takeaway from a decade of cloud native is that the principles still apply:


- **Open interfaces** , so workloads and agents are portable.
- **Shared governance** , so no single vendor controls the runway.
- **Distributed innovation** , so the best ideas can come from anywhere.
- **Collective security** , so the foundation everyone depends on stays trustworthy.


Kubernetes and Linux fueled the cloud era as the foundational layers. We believe they will be foundational for the agentic era too, alongside the new open standards the community is building right now.


## Come find us in Minneapolis


If you’re at the summit this week, please come say hello. The Microsoft team is at the booth with live demos across:


- Azure Linux
- Azure Container Linux
- [Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service)
- KAITO
- Dapr
- Radius
- Our security work


We have engineers, maintainers, and product managers ready to dig into whatever’s on your mind—whether that’s a thorny Kubernetes question, an idea for a new CNCF sandbox project, an AAIF contribution, or how to get your first agent into production.


The cloud era was built by this community. The AI native era will be too. I can’t wait to see what we build together.


*See you in Minneapolis.*


— *Brendan*


## Ready to try Azure Linux 4.0?


Sign up to express your interest in the upcoming public preview and be among the first to get access.


[Register now](https://aka.ms/AzureLinuxForm)
