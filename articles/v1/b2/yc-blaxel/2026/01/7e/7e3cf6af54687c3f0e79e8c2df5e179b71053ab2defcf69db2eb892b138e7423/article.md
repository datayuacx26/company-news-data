---
schema_version: "1.0.0"
document_id: "7e3cf6af54687c3f0e79e8c2df5e179b71053ab2defcf69db2eb892b138e7423"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/anatomy-of-a-runtime"
published_at: "2026-01-27T18:09:55+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:4032dea5ab41a675a030fd7c05ac0cae32524ba3710d47e7118c23fcccf64f0b"
---

# What if Lambda and EC2 had a baby?

As an infrastructure provider, there's one hard truth we've learned. No one really cares about infrastructure...until it breaks! Or gets too slow.


That's why we're constantly evaluating, benchmarking and optimizing our platform. You can see these footprints in our docs and code samples - mysterious names like "Mark 2.0" and "Mark 3.0".


If you've ever wondered what these mean, it's actually quite simple. Each "Mark" is an[infrastructure generation](https://docs.blaxel.ai/Infrastructure/Gens) that we designed, tested, rolled out to production and - eventually - deprecated in favor of something better.


AI agents are a new kind of workload. They demand the instant cold starts of a serverless function, but require the persistence of a full server. Standard cloud infrastructure forces you to pick one. We decided to build both.


This blog post looks into the anatomy of our current sandbox runtime, in the context of how our infrastructure has evolved over the past 16 months towards "Mark 3.1", our next-generation, "Lambda meets EC2" runtime built on bare metal and microVMs.


## Mark 1.0 - October 2024: Self-Managed Kubernetes with Knative Serving


When we first started Blaxel, our goal was to offer serverless infrastructure for AI models. We were primarily focused on providing a better experience for AI inference workloads, by enabling users to offload model requests to remote compute through our[Global Agentic Gateway](https://docs.blaxel.ai/Infrastructure/Global-Inference-Network) . This first-generation infrastructure (Mark 1.0) was built on[Kubernetes](https://kubernetes.io/) with[Knative](https://knative.dev/) Custom Resource Definitions (CRDs).


Knative is a Kubernetes extension for serverless workloads. It provides Custom Resource Definitions (CRDs) for all the key components of a serverless platform, such as services, routes, configurations and revisions. It consists of three primary components:[Knative Serving](https://knative.dev/docs/#knative-serving) for auto-scaling,[Knative Eventing](https://knative.dev/docs/#knative-eventing) for managing events, and[Knative Functions](https://knative.dev/docs/#knative-functions) for serverless function development. Within clusters, Mark 1.0 relied on Knative Serving's auto-scaling capabilities: if there was a sudden spike in requests for a model, we used Knative Serving to scale up additional pods to meet the increased demand, and scale back down later.


This took care of scaling within a cluster, but we also needed to manage traffic between clusters and ensure high availability. So we built a custom Blaxel Inference gateway to offload inference requests from the customer's cluster to ours, based on inference metrics, HTTP status and health checks. Our gateway was located in front of the cluster load balancer, automatically rerouting requests to our closest managed clusters if thresholds were exceeded. Our integrated heartbeat monitoring enabled near-instant redirection, with the gateway also able to dynamically spin up new instances of the customer's model on the new target cluster.


But serving inference workloads alone wasn't enough, and the economics of being "just" an overflow option were difficult to sustain (but this is another story). Interviewing customers, we observed that infrastructure was still being built for the SaaS era and there were very few dedicated and specialized non-inference infrastructure options, such as infrastructure for agents, MCP servers or batch jobs.


Mark 1.0 demonstrated reasonable stability, and the cold starts were somewhat acceptable for models, since the loading time for models is often much higher than just the normal operating system boot time. However, these cold starts were not acceptable for more lightweight workloads: tool calls, sub-agent orchestration, and external model routing.


## Mark 2.0 - November 2024: Managed Kubernetes with Managed Knative aka Google Cloud Run


In January 2025, we decommissioned Mark 1.0 in favour of Mark 2.0, which used containers with a managed cloud for workload execution. After running benchmarks on Google Cloud Run, Cloudflare Workers and AWS Lambda, we selected[Google Cloud Run](https://cloud.google.com/run) because (a) it was compatible with Knative APIs, enabling us to quickly migrate without investing significant time and effort, and (b) it gave us the most flexibility in term of workload type and run time (e.g. AWS Lambda functions are[limited to 15 minutes](https://docs.aws.amazon.com/lambda/latest/dg/configuration-timeout.html) ), even if it wasn't the best in term of cold starts. Cloudflare Workers were interesting, but the limitations of WebAssembly were too numerous to support AI use cases


Another benefit was Google Cloud's built-in support for multi-region infrastructure automation in their APIs and Terraform provider, which made it easy to replicate our infrastructure in 16 global regions. We created our own orchestrator to decide how to deploy the agents and MCPs, apply deployment policies (for example, no deployment in US or EU), manage autoscaling, and support intelligent routing to the closest region (discussed in more detail in[this post about our networking layer](https://blaxel.ai/blog/we-slashed-our-network-latency-to-under-50-ms) ).


Although Mark 2.0 also suffered from slow cold starts, it still offered some important benefits: containers could use the host kernel for system calls and, once warmed up, deployments had near-instant response speeds for subsequent requests.


Mark 2.0 infrastructure was well suited for models and batch jobs, but didn't really work well for agents, MCP servers, and sandboxes. There were two main issues:


- The slow cold starts - between 3-5 seconds minimum and up to 15 seconds - of containers were slowing down agent responses and affecting the end-user experience.
- We believed that agentic workloads needed more isolation and security than was possible with containers. For example, malicious users could[inject prompts](https://owasp.org/www-community/attacks/PromptInjection) to retrieve the agent's environment variables and credentials, or to directly use them in the agent container. Similarly,[container escape](https://blaxel.ai/blog/container-escape) exploits could allow attackers to directly access host resources and data.


Enter Mark 3.0.


## Mark 3.0 - March 2025: Unikernels and microVMs


We needed a way to enable fast, sandboxed execution of arbitrary code while also scaling horizontally. So, in March 2025, we started prototyping[unikernel](https://en.wikipedia.org/wiki/Unikernel) -based machines using the[Unikraft](https://unikraft.org/) open source project.


Unikernels are fast, single-purpose kernels. They contain only the minimum libraries required for their purpose and can run directly under a hypervisor. Their small footprint and fast boot times make them well suited for agentic workloads, which typically involve many short-lived agents requiring isolated environments and small bursts of compute.


Unikernels gave us the fast boot times and isolation we needed, but we hit a speed bump due to missing shared libraries (arising from unikernels' static linking approach). For example, we needed` libssl` in our microVMs, which in turn depends on cryptographic modules in the kernel for random number generation. These modules and shared libraries didn't exist by default in the unikernel; we had to manually import the required libraries during build, but this approach didn't scale across hundreds or millions of applications with different dependencies. We eventually solved this by using a stripped-down and highly-optimized full kernel instead of a unikernel. As a nice side bonus, this optimized kernel also reduced our cold start times.


This was the beginning of our work on Mark 3.0, defined by our transition from pure containers to a microVM based hypervisor. In April 2025, during Y Combinator, we launched perpetual sandboxes running on this new infrastructure. Our stack consisted of Unikraft nodes that we managed ourselves with[Firecracker microVMs](https://firecracker-microvm.github.io/) , a custom[Virtual Kubelet](https://virtual-kubelet.io/) that we built specifically for sandboxes to reduce the latency introduced by Kubernetes for cluster orchestration, and a custom cluster gateway based on the[Pingora](https://github.com/cloudflare/pingora) framework for routing. We developed our own build process consisting of downloading and copying a custom kernel image, a custom EROFS image, a boot wrapper for customized boot behavior, and a metadata file.


This infrastructure was highly performant and optimized for agentic workloads: sandboxes had a boot/resume time of under 20ms (we even recorded a complete resume in 9ms!), offered complete isolation and a persistent filesystem across shutdowns, and supported both REST and MCP protocols for in-sandbox operations. The entire lifecycle of the sandbox was automatically managed, including snapshotting and resume-from-standby.


In terms of node density, this infrastructure gave us both spatial and temporal advantages. Spatially, we were able to run more sandboxes per node with our optimized images while maintaining strong microVM isolation. And temporally, since our sandboxes resumed in under 25ms, we were able to aggressively turn them off when idle, freeing up capacity to serve more customers. Both density improvements reduced the number of physical nodes needed to serve the same number of sandboxes. It was also a win for customers: they only paid compute costs while the sandbox was actively processing a request.


The downside of this approach was that it required running on bare-metal servers to avoid the overhead and compatibility issues associated with nested virtualization (VMs running inside VMs). This added a new set of constraints (but also opportunities to optimize the software with the hardware).


Although this infrastructure worked well for our initial launch and is currently in production, we wanted to improve it further:


- We wanted native multi-tenancy management and cluster scheduling, both of which were critical to us for horizontal scaling.
- We wanted our users to have a complete VPC-like experience, with sandboxes on different hosts being able to talk to each other. We wanted faster and more scalable networking without needing to rely on standard TAP devices. And we also wanted to support per-sandbox dedicated IPs and granular network policies between workloads.
- We wanted to improve filesystem performance for package managers like` npm` /` pnpm` . We also wanted more flexibility in capacity allocation, with the ability to transfer even storage-dependent workloads (like databases) between nodes.


In June 2025, we started work on our next-generation Mark 3.1 infrastructure while retaining Mark 3.0 in production for sandboxes, agents, models and MCP servers. We deprecated Mark 2.0 in Q4 2025.


## Mark 3.1 - Currently in production rollout: MicroVMs with cluster orchestration and VPPs


Mark 3.1 represents a major architectural shift, from a unikernel-based solution to a custom Firecracker implementation. We have designed and implemented it from the ground up as a secure and scalable platform that's purpose-built for agentic workloads.


Here are some of the key technical improvements in Mark 3.1.


### Storage and filesystem


We added the ability to move snapshots between hosts, using CPU templates to ensure that a snapshot can be resumed in the future on another host with the same CPU architecture, even if the feature set differs (for example, AMD EPYC 9654 (Zen 4) to AMD EPYC 9754 (Zen 5)).


The default filesystem is still stored in RAM and we snapshot both the memory state and the filesystem simultaneously. This offers the best performance and full consistency for resume-from-standby operations.


In the future, we also plan to implement block storage and we are currently evaluating several options, including NVMe-oFabric as the transport with a custom disk controller.


### Networking


Mark 3.1 moves entirely to IPv6 with[VPP (Vector Packet Processing)](https://en.wikipedia.org/wiki/Vector_Packet_Processing) . Each sandbox appears as a distinct compute instance with a real IPv6 address; VPP takes care of rerouting packets correctly. We have changed the virtual interface type to make it much faster to create a VM and configure the network.


With this, users can create private networks between sandboxes - for example, an application running in a sandbox could communicate with API servers and databases running in other sandboxes within a user's private network. This feature also eliminates rate limiting arising from shared IP addresses and provides the foundation for a true VPC experience.


Forking and cloning of sandboxes was not possible with Mark 3.0, but is possible with Mark 3.1.


### Scalability


Mark 3.1 has a new virtual machine monitor (VMM) to help the customized Kubernetes scheduler spread workloads across multiple nodes more efficiently. The VMM handles Firecracker deployment on nodes, while Virtual Kubelet acts as the translator between the Kubernetes control plane and the VMM. To optimize performance, we also built a custom scheduler and inspected and removed all components that slow down Kubernetes' deployment controllers.


### Build process


We improved the build process, switching to a kernel image already directly available on the host (no download needed), a custom EROFS image, a shared boot library also on the host, and a metadata file.


## Mark 3.1 - Performance


In early testing, Mark 3.1 has displayed better performance than Mark 3.0. Resumes are down to 20ms from 25ms. Network bandwidth is at 12-15 Gbps per instance, and we hope to eventually achieve 50-100 Gbps with specialized hardware and specialized providers.


It's not only software and networking - Mark 3.1 also brings better hardware to the table. Thanks to this, CPU performance is better by 30%. And we also have more latitude in how we can optimize our software in combination with the hardware we select - for example, adding CPU features for cryptography, enabling hardware-accelerated networking by offloading network packet processing using Data Plane Development Kit (DPDK) to SmartNICs, and more.


Our new Mark 3.1 infrastructure is currently in production for batch jobs. We are continuing to improve it, and plan to roll it out for general availability later this year. If you'd like to know more, we'd be happy to answer questions -[join our Discord](https://discord.gg/enAfyZFWHW) and drop us a comment! And if hacking on cutting-edge AI-native infrastructure is your jam...[we're hiring](https://www.ycombinator.com/companies/blaxel/jobs) !
