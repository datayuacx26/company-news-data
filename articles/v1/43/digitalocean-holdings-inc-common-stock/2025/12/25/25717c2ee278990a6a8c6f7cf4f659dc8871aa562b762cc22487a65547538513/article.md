---
schema_version: "1.0.0"
document_id: "25717c2ee278990a6a8c6f7cf4f659dc8871aa562b762cc22487a65547538513"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/kubernetes-updates-h2"
published_at: "2025-12-15T18:30:03.533+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:a166d8fc6709b0120dc05e334eb6bd15f69839de521534368fc4e9af3efc84e3"
---

# Leveling Up Kubernetes: Key DigitalOcean Managed Kubernetes Releases in 2025

2025 was a busy and transformative year for DigitalOcean Managed Kubernetes, marked by a series of releases that make DigitalOcean Kubernetes simpler, more secure, and more scalable for developers and growing businesses. Across engine upgrades, networking and security enhancements, autoscaling improvements, and new ecosystem integrations, this year’s updates aimed to give teams more power with less operational overhead. Whether users are running production workloads, experimenting with microservices, or scaling customer-facing applications, the enhancements launched throughout 2025 make it easier than ever to deploy, manage, and optimize Kubernetes on DigitalOcean. In this recap, we’ll walk through the major releases that shaped the platform over the past year and how they help developers move faster with confidence.


## Next Evolution of DigitalOcean Kubernetes \[March\]


In March, we rolled out four major upgrades to DigitalOcean Kubernetes (DOKS) that make it easier to run larger and more efficient workloads: increased cluster capacity, VPC-native networking, eBPF-powered routing, and Managed Cilium. Here’s a look at each one:


-


**Cluster capacity has doubled from 500 to 1,000 worker nodes** , allowing bigger applications to run on a single cluster without the overhead of managing multiple environments.


-


**VPC-native Kubernetes** now assigns IPs directly from your VPC, improving performance and simplifying communication with other cloud resources.


-


Replacing kube-proxy with **eBPF-based networking and routing** delivers faster packet processing and lower latency, benefiting high-traffic and real-time workloads.


-


**Managed Cilium with Hubble** for observability adds stronger security, modern networking, and easier troubleshooting.


Together, these features significantly boost scalability, performance, and reliability while reducing operational complexity of DOKS, so developers gain clearer visibility and a simpler networking stack, while businesses benefit from lower overhead and the ability to scale applications more efficiently.


## Q3 DigitalOcean Kubernetes Features/Releases \[July\]


In July, we introduced four powerful new features to help you build and deploy more efficient applications on DigitalOcean Kubernetes—especially AI and machine-learning workloads.


-


We introduced four new **GPU Droplet types** :


-


[NVIDIA RTX 4000 Ada Generation GPU](https://www.nvidia.com/en-us/products/workstations/rtx-4000/) , with use cases including content creation, 3D modeling, rendering, video, and inference workflows with exceptional performance and efficiency.


-


[NVIDIA RTX 6000 Ada Generation GPU](https://www.nvidia.com/en-us/products/workstations/rtx-6000/) , perfect for rendering, virtual workstations, AI, graphic, and compute performance.


-


[NVIDIA L40s GPU](https://www.nvidia.com/en-us/data-center/l40s/) with use cases including graphics, rendering, and video streaming.


-


[AMD MI300X GPU](https://www.amd.com/en/products/accelerators/instinct/mi300/mi300x.html) , a high-performance GPU built for advanced AI inference and HPC workloads that combines powerful compute cores with high memory bandwidth.


-


The second feature we added was **Nodepool Scale-to-zero** . This capability allows a node pool to automatically scale down to zero nodes when no active workloads require them, eliminating compute charges during periods of inactivity. This is especially useful for development/testing environments, applications with business-hour usage, or workloads relying on specialized node pools for intermittent jobs.


-


We opened a new, AI-optimized **Atlanta datacenter (ATL1)** , offering fully managed Kubernetes clusters in the southeast United States. ATL1 is currently our largest and most advanced facility, purpose-built for high-density GPU infrastructure that supports demanding AI and machine learning workloads. For latency-sensitive applications, this means faster response times, lower transfer delays, and improved overall performance.


-


**DOKS Routing Agent** is a fully-managed solution that simplifies static route configuration in your Kubernetes clusters. With support for Kubernetes custom resources, this tool makes it easy to define custom routes, use Equal-Cost Multi-Path (ECMP) routing across multiple gateways, and override default routes without disrupting connectivity.


## DOKS Support for DigitalOcean MCP Server \[August\]


The DigitalOcean MCP (Model Context Protocol) Server lets users manage cloud resources using simple natural-language commands through AI-powered tools like Cursor, Claude, or their own custom LLMs. Running locally, it streamlines tasks such as provisioning Managed Databases, making cloud operations faster, easier, and more intuitive for developers. DOKS Support for DigitalOcean MCP Server marks a major step forward in bringing AI directly into containerized applications. It allows users to integrate natural-language automation into their workflows, reduce operational overhead, and build smarter, more responsive developer experiences.


## DOKS Managed Gateway API \[September\]


Kubernetes Gateway API, as a managed service, is pre-installed in all DOKS clusters and ready to use at no additional cost. This next-generation traffic management solution is more expressive, extensible, and powerful than Ingress. Powered by Cilium’s high-performance eBPF implementation, it offers superior performance and advanced routing capabilities without the overhead of traditional proxy-based solutions.*


*[More about Cilium (eBPF)](https://www.tigera.io/learn/guides/cilium-vs-calico/cilium-service-mesh/)


## Prioritize Node Pool Selection with Priority Expander Managed Gateway API \[September\]


We also introduced Priority Expander for the DigitalOcean Kubernetes (DOKS) Cluster Autoscaler. This feature lets user workloads automatically scale across multiple node pools in a defined priority order, automating fallback and eliminating the need for manual intervention to add capacity.


## VPC NAT Gateway and Network File Storage (NFS) are now available on DOKS and NFS \[November\]


VPC NAT Gateway enables Kubernetes workloads running in private subnets to securely access the internet for outbound operations, such as pulling container images, fetching updates, or calling external APIs, all without exposing those workloads to inbound traffic. By routing traffic through a managed NAT gateway with its own public IP, DOKS simplifies network architecture, strengthens security, and makes it easier to run production-grade private clusters.


Network File Storage (NFS) provides a scalable, high-availability shared file system that can be mounted across multiple pods and nodes in Kubernetes clusters. This makes it easy to persist and share data for workloads like content management systems, ML pipelines, and collaborative applications. With managed performance and automatic durability, it simplifies stateful application deployment on DOKS.


## Support for Multi-node GPU \[December\]


DOKS also supports multi-node GPU configurations, enabling users to deploy scalable, GPU-powered workloads across multiple nodes seamlessly. This makes it easier than ever to run high-performance applications like machine learning training, data processing, or GPU-intensive containerized workloads on DigitalOcean’s infrastructure. Examples of use cases include distributed model training, large-scale inference services, and real-time data processing pipelines.


## What’s to come in 2026


We’ve had a busy year making our managed Kubernetes offering simpler, easier to scale, and more performant—but we’re not done yet. We’ve got a lot coming in 2026, so stay in touch with us:


-


[Sail to Success Webinar series:](https://www.digitalocean.com/try/sailtosuccess) Sign up to be notified about our weekly webinars that we host with external guest speakers and some of DigitalOcean’s very own cloud experts.


-


[Sign up for our customer newsletter](https://www.digitalocean.com/blog) to get the monthly scoop on new releases, developer and community meetups, tutorials, and more.


-


[Visit our Managed Kubernetes homepage](https://www.digitalocean.com/products/kubernetes) to learn more about use cases, case studies, and more.


-


Thinking of migrating?[Learn more about our Migrations Program](https://www.digitalocean.com/migrate) , where you can migrate to DigitalOcean for free.
