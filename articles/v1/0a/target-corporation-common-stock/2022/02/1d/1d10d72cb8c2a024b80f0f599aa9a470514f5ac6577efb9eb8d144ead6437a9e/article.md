---
schema_version: "1.0.0"
document_id: "1d10d72cb8c2a024b80f0f599aa9a470514f5ac6577efb9eb8d144ead6437a9e"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/journey-to-a-hybrid-multi-cloud"
published_at: "2022-02-17T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:781222b8d176bd2c7e0b3acf06cfd46f9648a3d1268b50580855a0be35ac7527"
---

# The Journey to a Hybrid-Multi-Cloud

This blog is cross posted from


[LinkedIn](https://www.linkedin.com/pulse/journey-hybrid-multi-cloud-hari-govind/) and has been edited for length.


Target's cloud journey started about a decade ago. First there was Amazon Web Services (AWS). Followed by IBM Cloud. Then Google Cloud Platform (GCP). We have since expanded to a Hybrid-Multi-Cloud architecture with GCP and Microsoft Azure.


Public Cloud


To understand why we chose public cloud, one should first understand that retail is a seasonal business. In particular, a lot happens in the six weeks between Thanksgiving and Christmas! While the pandemic has shifted shopping behaviors in many ways, the seasonality of our core business has not changed. Holiday seasons have and will remain the busiest time of year. This is where public cloud is invaluable for us. We use public cloud for on-demand scale. If we had to run all our seasonal workloads on a private cloud of our own, we would need to overcapitalize our compute and storage capacity by 100% or more! We would underutilize this capacity eight months a year because of the effects of seasonality. This is hugely inefficient.


Private Cloud


Public cloud has enabled significant agility and scale for our enterprise. However, there will always be a set of workloads that run in our private data centers for a multitude of reasons. With that in mind, we're in the midst of a multi-year effort of “Infrastructure as Code” to fully modernize our private cloud architecture to a cloud-native stack.


Our private cloud footprint between enterprise data centers and the edge is extensive. We run our private cloud on an elaborate network of enterprise data centers, combined with a near edge infrastructure in our regional distribution centers (DCs) and a far edge infrastructure in our stores. Enterprise DCs still serve all our data-sensitive needs such as our unified guest data platform. A massively scaled edge footprint enables low latency workloads in stores and distribution centers.


Multi-cloud


Multi-cloud enables us to leverage what's best about each cloud. Running enterprise services on Azure makes sense for us. Commodity Kubernetes clusters in GCP are easy to manage and scale. Distributing workloads between two public cloud providers does offer redundancy and has the potential to eliminate single points of failure. This does, however, impose additional burden in terms of understanding the dependency stack and actively managing it. Egress costs are exorbitant, so we generally avoid multi-cloud for geo-redundancy alone, and instead are exploring geo-latency as one possible trigger for leveraging multiple providers. Recently, we have also been experimenting with auto-scaling workloads between public clouds. Lastly, multi-cloud for us also serves as an arbitrage for technology readiness, because each public cloud provider has its own strengths.


Cluster Management


Managing service clusters across the hybrid cloud has historically been a hard problem. Google Anthos, AWS Outpost and Azure Arc only solve parts of the problem. When we started our cloud journey, Anthos/GCP, Outpost/AWS, and Arc/Azure were still hyper-focused on a single public cloud platform, and imposing varying constraints – from specific hardware needs to the level of interoperability that is feasible for workloads running on private cloud. We built our Target Application Platform (TAP) as our homegrown cluster management platform that enables us to manage compute workloads and data across the hybrid cloud. TAP makes it easy for app developers to configure a pipeline, and deploy a binary down to Target stores, distribution centers, data centers, or to the public cloud. Learn more


[in this video](https://iccon.target.com/videos.html#popup2) from Dan Woods and Haylie Helmold about the TAP origin story and where we're going!


Application stack: Cloud Native or Lift and Shift?


We embraced code ownership years ago as the way to compete. Consequently, over 4,000 engineers at Target collaborated to fully modernize our application stack on an event-driven, microservices architecture rather than lift and shift legacy apps to the cloud. There were three tenets to our approach:


- Re-write apps to a microservice based, asynchronous, event-driven architecture.


The majority of our systems have been modularized and rewritten. Java and Python are our top language runtimes.


- Leverage open source.


Much of our software has been built using open source software. Our engineers have contributed to almost 300 open source projects and Target has published more than two dozen open source projects in the last three years.


- Operate a homogenous Platform-as-a-Service (PaaS) on all environments


– both public and private – and if you can't, build a cluster management platform that helps abstract away the differences from app engineers. Avoid using any cloud provider's PaaS offering, and if you do use one, have an exit strategy. For example: we've built adapters in TAP to manage our Linux based containers and vSphere VMs in our private cloud; Kubernetes clusters in Stores and GCP; and container instances in Azure.


Service Management for Reliability & Efficiency Reliability


The complexity of managing the private cloud ecosystem of 1,926 stores, 48 distribution centers, and three data centers is only compounded as you consider newer application and data management patterns across a hybrid multi-cloud architecture. The discipline of site reliability engineering at Target is addressing this with a real-time service graph of the critical dependencies that power product experiences and driving changes to platform patterns and practices as well as engineering culture. Learn more about what's working for us in this


[exciting talk on reliability engineering at Target](https://iccon.target.com/videosday-2.html#popup4) by John Engelman and Kate Blanchard.


Efficiency


As we started to scale out our consumption of public cloud services, we've had to mature our practice for how we manage capacity, utilization, and data movement. We’ve built forensic tools to understand service consumption of raw compute, memory, disk, and network bandwidth. What we've also learned along the way is that efficiency is a function of entropy: things can regress without continuous focus on driving down our utilization of applications with a range of performance optimization approaches along with using fine-grained regression detection tools.


You know service management is working for you when you are achieving the same or better reliability for the end-to-end architecture stack and when you are utilizing more of your existing capacity rather than growing your overall capacity across the Hybrid-Multi-Cloud with lower overall utilization. The last few years have been an incredibly rewarding time for engineers at Target with faster product experiences, fewer service interrupts, growing capacity base, and a doubling of underlying compute utilization.


Several of these topics deserve their own stand-alone write-ups. As always, check out Target’s


[ICCON Talks](https://iccon.target.com/videos.html) to learn more on a variety of Hybrid-Multi-Cloud topics and join our


[ICCON LinkedIn Group](https://www.linkedin.com/groups/14046078/) to connect and discuss with peers!
