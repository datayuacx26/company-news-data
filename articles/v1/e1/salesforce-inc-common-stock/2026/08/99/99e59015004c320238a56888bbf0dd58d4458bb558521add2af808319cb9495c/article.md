---
schema_version: "1.0.0"
document_id: "99e59015004c320238a56888bbf0dd58d4458bb558521add2af808319cb9495c"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/removing-the-security-barrier-to-agentforce-adoption/"
published_at: "2026-08-03T20:46:54+00:00"
first_seen_at: "2026-08-03T20:54:33.083982+00:00"
fetched_at: "2026-08-03T21:33:45.218976+00:00"
content_hash: "sha256:369607510365171c1760045ea431ab4b622a8c5a49d8868f6115a79d17b232f3"
---

# Removing the Security Barrier to Agentforce Adoption

As enterprises accelerate secure AI adoption with[Agentforce](https://engineering.salesforce.com/how-agentforce-prevents-language-drift-in-600k-daily-multilingual-ai-workflows/) and[Data 360](https://engineering.salesforce.com/how-data-360-segmentation-processes-a-quadrillion-records-across-arbitrary-customer-data-models/) , organizations across healthcare, banking, government, and critical infrastructure face a sobering reality. The bottleneck to enterprise AI adoption is no longer model capability, rather, it is governance. Agentforce can only reason on the data available to it, but patient records, financial transactions, taxpayer information, and operational systems often sit behind firewalls and strict security controls. For many organizations, their internal security policies require that sensitive data travel exclusively over private, dedicated paths, which is a bar higher than standard TLS encryption. Without a connectivity layer that meets that bar, AI initiatives worth millions of dollars can stall during security reviews before they ever reach production.


Trusted outcomes require trusted context, trusted context requires trusted data, and trusted data requires a trusted path to get there. If the connectivity layer isn’t private and auditable, nothing above it can be trusted either. Private Connect solves that problem by providing a private, dedicated network path that securely connects customer environments to Salesforce services running on[Hyperforce](https://engineering.salesforce.com/aws-cost-reduction-through-hyperforce-optimization-re-routing-traffic-slashing-20m/) , including Data 360, Agentforce, Tableau, CRM Analytics, and GovCloud, without traversing the public internet. Rather than optimizing for throughput alone, Private Connect focuses on delivering a secure, audited, and encrypted private networking layer that satisfies the stringent requirements of regulated industries. What began as an AWS-only capability has since evolved into a multi-cloud platform that now processes **approximately 120 TB of data and 683 million requests each month** , removing a major barrier to Agentforce adoption while scaling to support enterprise AI workloads at massive scale. Private Connect is the foundation of that entire trust chain.


*Evolution of Private Connect footprint through the years.*


##### Simplifying Secure Connectivity Reduced Provisioning from Weeks to Minutes


Early customer deployments required organizations to build VPN tunnels, configure firewall rules, maintain allow lists, and manage cross-cloud infrastructure. Even experienced teams could spend weeks or months stitching together secure connectivity.


The Private Connect team redesigned the experience around shared infrastructure, automation, and a managed control plane, moving much of that complexity behind the scenes. Processes that once took significant time can now be completed **in less than 30 minutes** , and customers can establish secure connections without needing to become networking experts.


As enterprise AI adoption accelerated, simplifying the customer experience became just as important as building the underlying technology.


*Private Connect provides a fully managed, end to end solution, freeing customers from maintenance burdens with DIY solutions.*


##### Rebuilding the Private Connectivity Architecture for Scale


The original challenge emerged before AWS supported native cross-region connectivity. Core customers needed cross-region traffic from day one, forcing engineers to solve a problem the underlying cloud platform had not yet addressed. To bridge the gap, the team built site to site VPNs and proxy layers to route traffic across regions. Supporting PPV2 headers, maintaining virtual machines, patching AMIs, and managing multiple layers of infrastructure created growing operational overhead.


Then[Data 360](https://engineering.salesforce.com/building-data-360-clean-rooms-zero-copy-architecture-for-privacy-safe-data-collaboration/) arrived, introducing throughput and latency requirements the original architecture wasn’t designed for. Rather than adding more layers, the team chose to redesign Private Connect entirely. The result was Private Connect v2.0, a lightweight architecture built around private links, transit gateways, and direct endpoint connectivity. Traffic could flow directly between Salesforce endpoints and customer environments without unnecessary hops, reducing operational complexity, improving throughput, and accelerating engineering velocity. Teams spent less time maintaining infrastructure and gained faster patching and automation cycles. Private Connect v2.0 became a reusable foundation instead of another one-off solution.


Another challenge emerged as Hyperforce expanded. Earlier releases involved manual coordination across teams for every new region, which couldn’t keep pace with Hyperforce’s rapid global expansion. The team redesigned the control plane so new[Hyperforce](https://engineering.salesforce.com/hyperforce-migration-at-scale-how-deterministic-automation-replaced-manual-spreadsheets-across-95000-organizations/) regions could be onboarded through automated pipelines. Instead of coordinating patch releases across teams, downstream services could simply consume APIs, accelerating expansion while reducing operational risk. T **oday, Private Connect processes approximately 120 TB of data and roughly 683 million requests each month across 15 AWS regions.**


*How Private Connect privately routes data from customer data lakes into Data 360.*


##### Scaling Private Connectivity Across Multi-Cloud Environments


Customer environments rarely conform to simple assumptions. Some organizations wanted compute traffic private and storage traffic public. Others wanted everything private. AWS and Azure behaved differently, DNS mechanisms varied, and features arrived on different schedules. Supporting those combinations became one of the hardest engineering challenges.


The team responded by decoupling endpoint management and allowing customers to configure connectivity independently rather than forcing them into all-or-nothing architectures. Scaling required similar flexibility. Engineers designed the platform for automatic expansion as endpoint limits increased, and instead of building one-off integrations for every platform, the team began developing a generic connector framework capable of supporting less common data stores without bespoke engineering. The lesson was simple: customers shouldn’t have to adapt to the architecture. The architecture should adapt to customers.


For Azure, PrivateConnect leverages industry-standard, cloud-specific cross-substrate interconnects. This further demonstrates the flexibility of the plug-and-play model, allowing PrivateConnect to integrate with different cloud-specific connectivity solutions while adapting quickly to the underlying infrastructure.


*Cross-substrate connectivity: How Private Connect routes data between Salesforce to customer data lakes on Azure.*


##### Preparing Private Connectivity for the Next Wave of AI


Private Connect started as an AWS-only capability. Today it supports Data 360, Tableau, CRM Analytics, GovCloud,[Sales Cloud](https://engineering.salesforce.com/sales-clouds-ai-transformation-welcome-to-the-autonomous-selling-era/) , and Service Cloud across 12+ connectors including Snowflake, Databricks, Redshift, Athena, and Kafka. Headless 360 and MCP endpoints are expanding the number of services that require secure access, multi-cloud deployments are becoming the norm, and agents are generating entirely new traffic patterns. Meeting those demands requires a platform-wide connectivity layer capable of securely exposing services at massive scale while maintaining the controls regulated enterprises depend on.


The team is intentionally designing for growth. Their philosophy is straightforward: when assumptions break, redesign. Do not add band-aids. Do not pile on more layers. With Private Connect v2.0, the team remains confident it can support workloads an order of magnitude larger than those seen today while continuing to expand to new clouds, protocols, and services.


As[Agentforce](https://engineering.salesforce.com/inside-unified-planner-the-ai-brain-behind-agentforce/) and Data 360 continue to evolve, and as Data 360 becomes the trusted business context for agents, secure connectivity is becoming just as foundational to enterprise AI as the models themselves. Enterprises already want AI. In regulated industries, the challenge isn’t convincing organizations to adopt it. It’s giving security teams an architecture they can approve. Given how fast AI is evolving, it is no longer a security versus adoption conversation. Security must develop ways that enable people to move forward safely. That is exactly what Private Connect does. It does not slow adoption down, it makes adoption *approvable* .


##### **Learn more**


- Stay connected — join our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) !
- Check out our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to learn how you can get involved.
