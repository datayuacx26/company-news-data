---
schema_version: "1.0.0"
document_id: "0d8ca3c51801fafaec60544e55ee3a4b9b28493022a97e1e8416190860bd2825"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/dropletplans-persecbilling-byoip-natgateway"
published_at: "2025-10-02T08:03:13.867+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:1c56b30e87a9307707f4493b20869b45827dd522a590ef55059caca961de8712"
---

# Announcing per-sec billing, new Droplet plans, BYOIP, and NAT gateway to reduce scaling costs

To help users cut down on cloud spending that’s wasted due to over-provisioning and inflexible billing models, we’re introducing tools that offer granular control without sacrificing simplicity or cost-effectiveness. We’re excited to announce new product updates that offer granular cost control, improved performance, and a clear path to savings.


**TL;DR**


DigitalOcean has transitioned to **per-second billing for Droplets.** Starting from January 1, 2026, you only pay for the exact compute time you use, perfect for slashing costs on ephemeral tasks and CI/CD pipelines.[Create Droplets](https://cloud.digitalocean.com/droplets/new) for short-lived workloads.


**New dedicated Droplet plans** are now generally available for a seamless performance upgrade. Visit your[DigitalOcean console](https://cloud.digitalocean.com/droplets/new?fleetUuid=2d108fad-6d02-4c95-af2f-4094f43eaed3&i=403c6d&region=sfo2&size=s-2vcpu-4gb-120gb-intel) to create these new Droplets.


**Bring your own IP** is now generally available to protect your IP reputation. Check out our[documentation](https://docs.digitalocean.com/products/networking/reserved-ips/details/features/#bring-your-own-ip) or visit your[DigitalOcean console](https://cloud.digitalocean.com/networking/reserved_ips?i=b1bbf4) to bring your own ip.


**VPC NAT gateway** is now generally available for centralized egress and static IPs. Join our[webinar: Stop the manual struggle of managing NAT instances and bastion hosts](https://streamyard.com/watch/xVrX94EnEFyK) or check out our[documentation](https://docs.digitalocean.com/products/networking/vpc/how-to/create-nat-gateway/) or visit your[DigitalOcean console](https://cloud.digitalocean.com/networking/vpc?i=b1bbf4) to set up your NAT gateway.


## Droplets per-second billing slashes costs for ephemeral workloads


Traditional hourly billing models for virtual machines have resulted in customers paying for idle time, even for short-lived workloads. Starting from Jan 1, 2026, our billing model transitioned to a per-second basis for[Droplets](https://www.digitalocean.com/products/droplets) , with a minimum charge of 60 seconds or $0.01, whichever is higher. This new approach aims to optimize cost control by charging for exact usage over a minute, making it simple to get precise billing for your actual usage. For sustained workloads, the monthly cap of 672 hours (24 hours x 28 days) of usage remains in place, ensuring your bill never exceeds the predictable monthly price.


Imagine your development team uses a CI/CD pipeline that uses $84/month CPU-Optimized Droplet to run automated tests taking 10 minutes. With hourly billing, each 10-minute task would charge you for the full hour, costing about $0.125. If you ran this job 20 times a day, your daily cost would have been around $2.50, even though the total active time was just over 3 hours. Now, with per-second billing, that same 10-minute task costs only about $0.02. By running the job 20 times a day, your total daily cost is just around $0.42.


Per-second billing can provide a similarly dramatic cost reduction by ensuring you only pay for the precise compute time you use. Effective Jan 1, 2026, this change will apply automatically to all Droplets. Customers will be able to access detailed usage information, including Droplet start and end times, through their billing history and downloadable CSV reports.


**Use cases:**


-


**CI/CD pipelines and automated testing:** Only pay for the duration of testing jobs (with a minimum charge of 60 seconds or $0.01, whichever is higher), not full hours.


-


**Auto-scaled applications:** Instances that briefly scale up will only be billed for active time (with a minimum charge of 60 seconds or $0.01, whichever is higher), reducing costs.


-


**Event-driven batch jobs:** Intermittent, event-triggered tasks will be billed precisely for compute time (with a minimum charge of 60 seconds or $0.01, whichever is higher), avoiding full-hour charges.


**Benefits:**


-


**Granular cost control:** Pay only for the seconds your Droplets are active, with a minimum charge of 60 seconds or $0.01, whichever is higher.


-


**Optimized auto-scaling:** Scale up and down to meet demand without incurring full hourly charges for brief usage, with a minimum charge of 60 seconds or $0.01, whichever is higher, per instance.


-


**Budget predictability:** Retain the monthly cap for continuous workloads.


Read[documentation](https://docs.digitalocean.com/products/droplets/details/pricing/) to learn more and[Create Droplets](https://cloud.digitalocean.com/droplets/new) for short-lived workloads.


## New dedicated Droplet plans for a seamless performance upgrade


Historically, upgrading from a shared CPU Droplet to a dedicated CPU Droplet for more consistent, sustained performance meant a large, costly leap in resources. This created a significant pain point for customers who needed the better performance of premium Droplets while retaining their static IP and avoiding a complex data migration.


To better address diverse needs, we are introducing new intermediate[Droplet](https://www.digitalocean.com/pricing/droplets#cpu-optimized) sizes for our dedicated CPU optimized and general-purpose plans. The new sizes include:


-


**Dedicated CPU Optimized Regular:** 5x SSD variant


-


**Dedicated CPU Optimized Premium:** 5x SSD variant


-


**Dedicated General Purpose Regular:** 6.5x SSD variant


-


**Dedicated General Purpose Premium:** 5.5x SSD variant


Additionally, the following sizes are exclusively available through our sales channel:


-


**Dedicated CPU Optimized Premium:** 96vCPU


-


**Dedicated Storage Optimized Premium:** 1.5x variant with 10TiB NVMe SSD


These new sizes provide a more balanced resource profile, offering a seamless in-place upgrade path without the need to recreate Droplets. This allows for easy migration to a dedicated environment, preserving your existing IP and application data, and avoiding payment for unneeded vCPUs or RAM.


To learn more about various Droplet plans and how to choose the right one, refer to[choose the right CPU Droplet](https://docs.digitalocean.com/products/droplets/concepts/choosing-a-plan/) .


**Use cases:**


-


**AI/ML:** A growing AI startup running a compute-intensive model on a shared CPU plan can seamlessly transition to a dedicated CPU plan to ensure consistent performance.


-


**SaaS:** A SaaS provider with a single-tenant architecture can easily upgrade a customer’s Droplet to a dedicated CPU plan as their needs grow, all while keeping their static IP and existing data.


**Benefits:**


-


**Precise resource allocation:** Get the right compute and memory for your workload without over-provisioning.


-


**Zero-downtime upgrades:** Transition from a shared to a dedicated CPU plan without the need for a full Droplet migration.


-


**Cost-effective performance:** Access consistent CPU performance without a steep increase in your monthly bill.


Visit your[DigitalOcean console](https://cloud.digitalocean.com/droplets/new?fleetUuid=2d108fad-6d02-4c95-af2f-4094f43eaed3&i=403c6d&region=sfo2&size=s-2vcpu-4gb-120gb-intel) to create these new Droplets or see[Droplet pricing page](https://www.digitalocean.com/pricing/droplets#cpu-optimized) for more details


## Bring your own IP (BYOIP) to protect your IP reputation


Migrating applications between cloud providers can disrupt business continuity due to IP address changes affecting client allow-lists, SEO, and IP reputation. DigitalOcean’s new Generally Available Bring-Your-Own-IP (BYOIP) feature for[Droplets](https://www.digitalocean.com/products/droplets) or[Kubernetes](https://www.digitalocean.com/products/kubernetes) nodes allows seamless IPv4 prefix transfer, helping to ensure business continuity, preserving IP reputation, and enabling true provider-agnosticism. Our BYOIP service is generally set up in just 7 business days, significantly faster than the 1-4 weeks often required by hyperscalers, allowing quicker and less disruptive workload migration.


Bring your own IP


**Use cases:**


-


**SaaS:** A company with a large B2B client base can use BYOIP to avoid having its clients’ allow-lists and firewalls break when migrating their app to DigitalOcean.


-


**Global businesses:** A company with global operations can maintain its established regional IPs to sell into another market.


**Benefits:**


-


**Business continuity:** Avoid downtime and service disruptions related to IP changes.


-


**IP reputation preservation:** Maintain your established reputation for services like email.


-


**Vendor independence:** Retain ownership and control of your IP addresses.


Learn more about BYOIP: Check out our[documentation](https://docs.digitalocean.com/products/networking/reserved-ips/details/features/#bring-your-own-ip) or visit your[DigitalOcean console](https://cloud.digitalocean.com/networking/reserved_ips?i=b1bbf4) to bring your own IP.


## VPC Network Address Translation (NAT) gateway for centralized egress and static IPs


Traditionally, enabling private resources to access the internet was done by manually configuring NAT instances, repurposing bastion hosts as outbound proxies, or assigning public IPs to every instance. These approaches were operationally heavy, insecure, or costly. DigitalOcean’s new VPC NAT gateway offers a fully managed, highly available, simple and scalable solution for SNAT and DNAT, designed to block external inbound connections and central egress traffic for consistent static IPs. NAT gateway is now generally available for both Droplets and DigitalOcean Kubernetes (DOKS). This enables pods and nodes within a Virtual Private Cloud (VPC) to initiate outbound connections to the public internet using a single, shared public IP address, while keeping the IP addresses of internal resources private.


Key features include easy setup, multiple NAT gateways per[VPC](https://www.digitalocean.com/products/vpc) for specific Droplet egress, and predictable pricing at $40 per size 1 NAT gateway (including 100 GB bandwidth, $0.01/GB overage), making it significantly cheaper than some market alternatives. It also supports up to 500,000 simultaneous connections, surpassing some market alternatives.


NAT gateway


**Use cases:**


-


**AdTech:** An ad-tech company can use a NAT gateway to help ensure all API calls to third-party ad networks originate from allowed static IP addresses.


-


**E-commerce:** An online store can use a NAT gateway to allow its backend services to fetch software updates from the internet without being exposed to public traffic.


-


**Streaming:** A video streaming service can use a NAT gateway to allow its content delivery network (CDN) edge servers to more securely access internal APIs for user authentication and content metadata without exposing these APIs to the public internet.


-


**Gaming:** An online gaming platform can use a NAT gateway to enable game servers to more securely connect to external APIs for player matchmaking, leaderboards, or anti-cheat services while keeping the game servers themselves better protected from direct internet exposure.


-


**Analytics:** A data analytics platform can use a NAT gateway to allow its data processing clusters to more securely pull data from external data sources or third-party APIs for analysis without exposing the clusters to public traffic.


**Benefits:**


-


**Enhanced security:** More securely access outbound internet for Droplets or DOKS that do not have public IPs, and centralize control of egress traffic for enhanced security, compliance, and operational simplicity.


-


**Simplified management:** Avoid the complexity, instability, or cost of self-managed NAT instances or hyperscaler NAT gateways, while getting inbuilt High Availability. Maintain static source IPs for allow-listing with third-party services


Join our[webinar: Stop the manual struggle of managing NAT instances and bastion hosts](https://streamyard.com/watch/xVrX94EnEFyK) or check out our[documentation](https://docs.digitalocean.com/products/networking/vpc/how-to/create-nat-gateway/) or visit your[DigitalOcean console](https://cloud.digitalocean.com/networking/vpc?i=b1bbf4) to set up your NAT gateway.


By building these new capabilities, we are providing a more robust, flexible, and cost-effective infrastructure to help you address the challenges of scaling your business and keeping costs under control.


Get started with these new tools today:


-


**Try these features** by heading to the[DigitalOcean console](https://cloud.digitalocean.com/login) .


-


**Learn more** by visiting our[product documentation](https://www.digitalocean.com/docs) .


-


**Join our**[Webinar: Stop the manual struggle of managing NAT instances and bastion hosts](https://docs.google.com/spreadsheets/d/1Rf7guWE0uCUyWMZ5-_cJ2tKhVLvM6Ksy4-9AJhu-zPU/edit?usp=sharing) to get more details on VPC NAT Gateway


-


[Get expert guidance for free](https://www.digitalocean.com/landing/get-expert-guidance) to strengthen your cloud architecture, optimize costs, scale your infrastructure, and improve backup and disaster recovery.
