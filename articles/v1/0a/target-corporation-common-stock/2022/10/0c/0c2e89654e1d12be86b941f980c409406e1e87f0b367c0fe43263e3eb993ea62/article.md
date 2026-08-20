---
schema_version: "1.0.0"
document_id: "0c2e89654e1d12be86b941f980c409406e1e87f0b367c0fe43263e3eb993ea62"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/targets-cloud-journey"
published_at: "2022-10-21T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:c640f724b0b1c64f5de8be57554ad2722f6976505c27d1dd291159642d3ecc52"
---

# Target’s Cloud Journey

Target began its cloud journey nearly a decade ago. Since then, Target tech teams have expanded our technology footprint to a hybrid-multi-cloud architecture. As we shared at our recent


[Infra Cloud Conference](https://iccon.target.com/index.html) (ICCON), I wanted to look back at our journey so far in the hope that it may help other teams embarking on a similar journey to a hybrid cloud.


Why Migrate?


Many aspects of the retail business are seasonal. Target’s “peak season” – the six weeks between Thanksgiving and Christmas Eve – is by far the busiest time of year for Target. Digital traffic on Black Friday – Cyber Monday, for instance are orders of magnitude higher by 3X or 4X than during the rest of the year! This is where public cloud is invaluable to help us scale up or down as needed based on demand. If we ran our seasonal workloads entirely on a private cloud, we would need to overcapitalize our compute and storage capacity at peak times, and then underutilize this capacity many weeks throughout the year. This would be hugely inefficient for us.


While public cloud enables significant agility and scale for our enterprise, there will always be a set of workloads that run in our private data centers for a multitude of reasons that include privacy, latency, data locality etc. Our private cloud footprint includes enterprise data centers and an extensive edge computing footprint that spans each of our nearly 2,000 stores and 52 distribution centers, powering everything from point-of-sale and IoT devices to robotics and more.


The Journey to Cloud


We began our journey to public cloud a decade ago, with shifts along the way to various public cloud providers. Each of these shifts was a massive undertaking, with significant rewrite. As we finally settled on a hybrid-multi-cloud architecture, our approach veered away from lift and shift to fully modernizing our application stack – so workloads would be fully portable. Over 4,000 Target engineers collaborated to transform applications to an event-driven, microservices architecture.


There were three tenets to our approach:


- Re-write apps to a microservice-based, asynchronous, event-driven architecture


. Most of our systems have been modularized and rewritten. Java and Python are two of our top language runtimes.


- Leverage open source


. Target is an activate participant in the open source community. Much of our software has been built using open source software. Our engineers have also contributed to more than 300 open source projects/repos and originated more than


[60 open source projects](https://github.com/target) .


- Single pane of glass


. We built Target Application Platform (TAP) as our homegrown cluster management platform to simplify how we manage cloud and on-prem resources – both public and private. We built adapters in TAP to manage Linux based containers for modern apps and VMs for legacy apps in our private cloud; Kubernetes clusters in Stores; GKE on Google Cloud; and Container Instances in Azure. TAP became the single pane of glass enabling Target tech engineers to manage workloads across our hybrid-multi-cloud.


Sticker Shock


Public cloud adoption and its associated cost were initially unexpectedly high in 2018 and 2019 as app engineers across Target began adapting workloads. As we started to scale up our use of public cloud, we invested in new performance and efficiency engineering to better manage capacity and utilization and developed sophisticated tools to manage workload placement and data movement across our hybrid-multi-cloud platform. This included building forensic tools to understand service consumption of raw compute, memory, disk, and network bandwidth. All this together helped us slow down our public cloud growth to single digits in 2020 and 2021 despite Target’s unprecedented digital growth during this period.


Long-Term Evolution


Target’s long-term architecture will remain hybrid for the foreseeable future – with public cloud for scale and cost optimization combined with private cloud for data-intensive workloads, line of business applications, enterprise systems, and edge computing. We have learned that the long-term cost implications of public cloud make sense with either committed spend or on-demand consumption for transient workloads with spiky growth and extended, deep troughs where on-prem capitalization is inefficient.


We are also acutely aware that, purely from a unit of compute perspective, a private cloud can offer significant cost benefits. As one of the first retailers to join the


[Open Compute Project](https://www.opencompute.org/) and with open source projects like


[SONiC](https://github.com/sonic-net) , we are continuing to drive down the cost of running our own private cloud with disaggregated, commodity hardware that offers more efficient designs and is more scalable, at lower cost.


Advice for Leaders Eyeing Similar Processes


As you embark upon your own migration projects, it is important to follow a few tenets to maximize the opportunities for success.


- First, intentionally invest in architecture to modernize applications that will future-proof the enterprise in terms of workload and data portability, as cloud economics shift.


- Second, what we call performance and efficiency engineering is now emerging into the practice of


[FinOps](https://www.finops.org/introduction/what-is-finops/) – as CFOs are increasingly paying attention to escalating public cloud costs with strong incentives to optimize infrastructure and app consumption patterns. FinOps or similar practices dedicated to efficient utilization of cloud capacity will go a long way to managing costs even in the medium-term.


- Third, large enterprises with a substantial private cloud footprint will benefit from developing norms for workload management across the hybrid cloud and building agility into platforms that enable workload placement.


Interested in learning more about this and other topics like site reliability engineering, network engineering, and more? Check out video from our recent


[ICCON](https://iccon.target.com/) flagship conference and other recent meetups where senior leaders across the industry, and a community of engineers came together to explore the next infrastructure mindset.
