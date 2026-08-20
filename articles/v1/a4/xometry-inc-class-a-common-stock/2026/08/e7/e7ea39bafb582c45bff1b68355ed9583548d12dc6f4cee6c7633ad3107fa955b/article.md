---
schema_version: "1.0.0"
document_id: "e7ea39bafb582c45bff1b68355ed9583548d12dc6f4cee6c7633ad3107fa955b"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/data-center/hyperscale/"
published_at: "2026-08-13T03:00:18+00:00"
first_seen_at: "2026-08-13T17:41:42.989942+00:00"
fetched_at: "2026-08-13T17:41:52.800977+00:00"
content_hash: "sha256:2d6964705b2221df58d084743bc5ebe37b56201764ee8d354e7d069d77a2ce96"
---

# Hyperscale Data Center: Definition and Scale

## What Are the Sizes of a Hyperscale Data Center?


The sizes of a hyperscale data center are measured across 4 primary dimensions that collectively define the facility's operational scale and infrastructure capacity.


The sizes of a hyperscale data center are listed below.


- **Server Capacity** : Server capacity measures the total number of active compute and storage nodes the facility houses, ranging from 5,000 servers at the minimum qualifying threshold to over 1 million servers in the largest campus deployments.
- **Physical Footprint** : Physical footprint quantifies the raised floor area and total campus size, starting from 10,000 square feet for a minimum qualifying facility and exceeding 1 million square feet for large-scale multi-building campuses.
- **Power Capacity** : Power capacity measures the total electrical load the facility delivers to IT equipment, ranging from 20 megawatts at the lower hyperscale threshold to over 1 gigawatt across the largest campus deployments.
- **Network Throughput** : Network throughput measures the total internal switching and external connectivity bandwidth, ranging from hundreds of terabits per second in mid-scale facilities to 1 petabit per second or more in the largest deployments.


### 1. Server Capacity


Server capacity defines the scale of a hyperscale data center's compute and storage infrastructure, directly determining the volume of workloads, concurrent users, and data processing operations the facility sustains. The Uptime Institute and major research firms set the minimum qualifying server count at 5,000 units, a threshold that distinguishes hyperscale from large enterprise facilities operating 500 to 2,000 servers.


Mid-scale hyperscale deployments operate from 50,000 to 200,000 servers, supporting regional cloud service delivery, large-scale content streaming, and AI inference workloads for tens of millions of users. The largest hyperscale campuses, operated by major cloud and technology companies, maintain server counts exceeding 1 million units distributed across multiple buildings on a single campus. At that scale, the facility processes exabytes of data per day across distributed computing frameworks (Apache Spark, MapReduce, and proprietary equivalents) that partition workloads into parallel tasks executed simultaneously across thousands of nodes.


Server hardware in a hyperscale environment uses a commodity open-rack model where individual servers are stripped of non-essential components to reduce power consumption per unit. A standard hyperscale server draws 200 to 500 watts, meaning a 100,000-server facility consumes 20 to 50 megawatts of IT power from compute alone, before accounting for storage and networking equipment. The density and count of servers define every other sizing dimension of the facility, from power capacity and cooling load to physical floor space and network switching requirements.


### 2. Physical Footprint


Physical footprint measures the total raised floor area, building count, and campus land area of a hyperscale data center, reflecting the spatial requirements of housing tens of thousands to millions of servers with their supporting power and cooling infrastructure. The minimum qualifying footprint for a hyperscale facility is 10,000 square feet of raised floor space, distinguishing the category from large enterprise data centers that typically occupy 2,000 to 8,000 square feet.


Mid-scale hyperscale facilities range from 100,000 to 500,000 square feet of raised floor across one or more buildings, while the largest campuses exceed 1 million square feet distributed across 4 to 10 or more buildings on a contiguous land parcel. Microsoft's data center campus in Boydton, Virginia, occupies over 500,000 square feet of raised floor across multiple buildings, with campus land area exceeding 100 acres to accommodate cooling infrastructure, generator yards, and future expansion phases. Land requirements for hyperscale campuses range from 50 to 500+ acres, depending on building height, cooling tower placement, and planned expansion capacity.


Floor-to-ceiling heights in hyperscale facilities range from 3 to 5 meters of clear structural height, providing sufficient airflow volume for hot-aisle and cold-aisle containment systems at row lengths of 20 to 60 meters. Structural loading requirements for hyperscale server rows reach 1,500 to 2,500 kilograms per square meter, significantly exceeding standard commercial building floor load ratings of 500 to 750 kilograms per square meter, requiring purpose-engineered structural slab designs.


### 3. Power Capacity


Power capacity measures the total electrical infrastructure a hyperscale data center delivers to IT equipment, support systems, and cooling, representing one of the most capital-intensive dimensions of facility sizing. The minimum power threshold for hyperscale classification is 20 megawatts of IT load capacity, a level that requires utility-grade electrical infrastructure and dedicated high-voltage transmission connections.


Mid-scale hyperscale deployments operate from 50 to 200 megawatts of total facility power, requiring 115 kilovolts (kV) to 230 kV utility feeds and on-site substation infrastructure rated for the full load plus N+1 redundancy. The largest hyperscale campuses draw over 1 gigawatt of total power, a consumption level equivalent to powering a city of 750,000 residents. Capital expenditure for electrical infrastructure at that scale, including transformers, switchgear, UPS systems, and generators, ranges from \[$500 million to over $2 billion\], depending on utility tariff structures and redundancy requirements.


Power usage effectiveness (PUE) measures how efficiently the facility converts total power input into IT equipment power, with hyperscale operators achieving PUE ratings from 1.10 to 1.40, compared to the industry average of 1.58. A PUE of 1.10 means that for every 1 watt delivered to IT equipment, the facility consumes an additional 0.10 watts on cooling, lighting, and power conversion overhead. On-site renewable energy generation through solar arrays and wind power purchase agreements covers 50 to 100% of hyperscale campus energy consumption for major operators committed to carbon neutrality targets.


#### 4. Network Throughput


Network throughput measures the total internal switching capacity and external connectivity bandwidth of a hyperscale data center, reflecting the volume of data the facility moves from server to server, from storage to compute, and from the facility to the public internet per unit of time. Internal switching fabric in a hyperscale environment uses a spine-and-leaf topology with 100-gigabit to 400-gigabit Ethernet links connecting every server to the switching layer.


Internal switching capacity in a mid-scale hyperscale facility (50,000 servers) reaches 500 terabits per second to 1 petabit per second of aggregate switching bandwidth, ensuring non-blocking data transfer across all active nodes simultaneously. The largest hyperscale campuses operate internal fabrics exceeding 1 petabit per second, with next-generation deployments targeting 10 petabits per second as AI training workloads generate inter-node traffic at rates of 400 to 800 gigabits per second per GPU cluster. East-west traffic, meaning server-to-server data movement within the facility, accounts for 70 to 80% of total network throughput, as distributed computing frameworks continuously exchange intermediate data across nodes.


External connectivity to the public internet is delivered through redundant 100-gigabit to 400-gigabit uplinks to multiple internet service providers (ISPs) and direct peering connections at internet exchange points (IXPs). A major hyperscale facility maintains 1 to 10 terabits per second of external connectivity, distributed across 4 to 8 diverse fiber paths entering the building from different physical directions to eliminate single-path failure risk. Content delivery traffic from hyperscale facilities accounts for a significant portion of global internet backbone utilization, with individual operators transferring 10 to 100 exabytes of data monthly across their network infrastructure.


## How does a Hyperscale Data Center Work?


A hyperscale data center works by combining physically distributed server infrastructure, software-defined resource management, and automated operational systems into a unified platform that allocates compute, storage, and network resources dynamically in response to real-time workload demand. The facility does not function as a collection of independent servers; instead, orchestration software abstracts all physical hardware into shared resource pools that workloads draw from as needed, without awareness of which specific physical machine executes each task.


Workloads enter the facility through external network connections at speeds from 100 gigabits to 400 gigabits per second, passing through edge routers and load balancers that distribute incoming requests across server clusters based on current utilization rates and geographic proximity rules. The orchestration layer (Kubernetes, Apache Mesos, or proprietary platform equivalents) schedules each computational task on a server node with available CPU, memory, and local storage capacity, completing the scheduling decision in under 100 milliseconds. Storage requests route through a distributed storage network where data is fragmented across multiple nodes using erasure coding or replication, ensuring redundancy without requiring a dedicated storage server for each dataset.


Cooling systems maintain server inlet temperatures from 18°C to 27°C through a combination of precision air handling, hot-aisle containment, and direct liquid cooling for high-density GPU rows, with thermal sensors adjusting cooling output every 30 to 60 seconds based on real-time heat load measurements. Power management systems monitor per-rack energy consumption through intelligent PDUs and apply dynamic power capping to prevent overloads while sustaining maximum utilization across all active nodes. Fault detection operates continuously at the hardware level, with baseboard management controllers (BMCs) reporting disk failures, memory errors, and CPU faults to the orchestration system within 5 to 30 seconds, triggering automatic workload migration to healthy nodes before service impact occurs. The complete operational cycle from workload ingestion through resource allocation, processing, fault recovery, and output delivery executes without human intervention at the hyperscale level.


## Who Uses Hyperscale Data Centers?


The users of hyperscale data centers are organizations whose operational requirements for compute scale, data volume, and global reach exceed what conventional enterprise or colocation infrastructure delivers.


The organizations using data centers are listed below.


- **Cloud Service Providers** : Cloud providers are the primary builders and operators of hyperscale facilities, using the infrastructure to deliver on-demand compute, storage, and networking services to millions of business and individual customers globally.
- **Artificial Intelligence and Machine Learning Companies** : AI and ML organizations require hyperscale infrastructure for model training pipelines that consume hundreds of megawatts of GPU compute power and process petabyte-scale training datasets.
- **Streaming and Content Delivery Platforms** : Streaming platforms use hyperscale infrastructure to encode, store, and deliver video content at resolutions from 1080p to 8K to hundreds of millions of concurrent viewers across global networks.
- **Enterprise Technology Companies** : Large technology companies with global software platforms, SaaS products, and enterprise application stacks require hyperscale infrastructure to maintain the performance and availability levels their customer contracts require.


### 1. Cloud Service Providers


Cloud service providers are the dominant builders and operators of hyperscale data centers, as the on-demand infrastructure model they sell requires physical facilities capable of serving millions of customers simultaneously across globally distributed networks. A cloud provider's product catalog, covering virtual machines, managed databases, object storage, and AI inference APIs, is only deliverable at commercial scale if the underlying physical infrastructure operates at the hyperscale level.


AWS, Microsoft Azure, and Google Cloud collectively operate over 300 hyperscale data center facilities across 30+ geographic regions globally, with each facility containing tens of thousands to hundreds of thousands of servers. The capital expenditure for hyperscale data center construction by the 3 major cloud providers exceeded \[$150 billion\] collectively in 2023, reflecting the infrastructure investment required to support projected cloud workload growth through 2030. AWS alone operates facilities in 33 regions and 105 availability zones, with each availability zone representing a discrete hyperscale or near-hyperscale facility connected to others within the region through redundant fiber links delivering inter-zone latency under 2 milliseconds.


The hyperscale architecture is essential for cloud providers because elasticity, the ability to provision and release resources within seconds, requires a physical server and storage pool large enough to absorb sudden demand spikes without exhausting capacity. A single Black Friday e-commerce event generates compute demand spikes of 300 to 500% above baseline for cloud-hosted retail workloads, a variability range that only hyperscale resource pools can absorb without service degradation. Revenue per hyperscale facility for major cloud providers ranges from \[$500 million to $2 billion+ annually\], depending on utilization rates and regional pricing structures.


### 2. Artificial Intelligence and Machine Learning Companies


Artificial intelligence and machine learning companies depend on hyperscale data centers for the GPU cluster density, network interconnect bandwidth, and storage throughput that large-scale model training and inference workloads require. Training a large language model (LLM) at the scale of GPT-4 or equivalent architectures requires thousands of GPUs operating in parallel for periods of 30 to 90 days, consuming 10 to 30 megawatts of power per training run.


GPU clusters in hyperscale AI facilities are interconnected through high-speed fabric networks (NVIDIA InfiniBand or proprietary RoCE equivalents) delivering 400 to 800 gigabits per second of inter-GPU bandwidth, allowing gradient exchange across thousands of GPUs with latency under 1 microsecond. A single AI training cluster in a hyperscale facility contains 1,000 to 16,000 or more GPUs, with rack power densities reaching 40 to 100 kilowatts, necessitating direct liquid cooling rather than air-based thermal management. NVIDIA's DGX SuperPOD reference architecture, deployed within hyperscale AI facilities, delivers up to 1 ExaFLOP of AI training performance per pod from 32 DGX H100 systems consuming 640 kilowatts of power.


Hyperscale architecture is essential for AI companies because the compute requirements for frontier model training double approximately every 6 months, a rate that requires infrastructure capable of expanding GPU cluster capacity by thousands of units per quarter without facility redesign. Inference workloads for deployed AI models generate millions of API requests per day, requiring the same horizontal scaling capability that hyperscale compute pools provide for cloud workloads. The storage requirements for AI training datasets range from 10 petabytes to over 1 exabyte per training corpus, demanding the distributed object storage architectures that only hyperscale facilities implement at the required throughput levels.


### 3. Streaming and Content Delivery Platforms


Streaming and content delivery platforms rely on hyperscale data centers to store, transcode, and distribute video content at the quality levels and global reach that hundreds of millions of simultaneous viewers demand. A single streaming platform serving 200 million active subscribers generates peak traffic loads exceeding 100 terabits per second during simultaneous content release events, a throughput level that only hyperscale infrastructure sustains without buffering or quality degradation.


Video transcoding at hyperscale requires dedicated CPU and GPU compute clusters that convert raw video uploads into 8 to 15 output formats per title, ranging from 360p mobile streams to 4K HDR files, simultaneously across thousands of parallel encoding jobs. Netflix's hyperscale infrastructure processes over 1 million hours of new content per year through transcoding pipelines that run on AWS hyperscale facilities across multiple regions. Storage requirements for a major streaming platform's content library range from 50 petabytes to over 1 exabyte of encoded video files, stored across geographically distributed hyperscale object storage systems with 11 nines (99.999999999%) durability guarantees.


Hyperscale architecture is essential for content platforms because global audience distribution requires content origin servers and caching infrastructure positioned within 50 milliseconds of viewers in every major market. A streaming platform uses hyperscale origin facilities in 4 to 6 core regions, combined with edge caching nodes in 50 to 100+ metropolitan markets, to deliver start times under 2 seconds and rebuffering rates below 0.5% across all network conditions. The content delivery network (CDN) layer that hyperscale facilities anchor handles 10 to 100 exabytes of monthly data transfer for the largest platforms, requiring network uplink capacity and peering agreements that only hyperscale-grade facilities negotiate and maintain.


### 4. Enterprise Technology Companies


Enterprise technology companies with globally distributed SaaS platforms, productivity software, and enterprise application stacks operate hyperscale data centers to deliver the performance, availability, and geographic reach their business customer contracts require. A SaaS platform serving 100,000 enterprise customers across 50 countries requires infrastructure in multiple geographic regions with guaranteed uptime from 99.9% to 99.99% annually, translating to maximum annual downtime from 8.7 hours to 52 minutes.


Microsoft operates hyperscale facilities globally to support Office 365, Teams, Dynamics 365, and LinkedIn, collectively serving over 300 million commercial users. Salesforce maintains hyperscale-grade infrastructure across multiple AWS and self-operated data center regions to support CRM, marketing automation, and analytics workloads for 150,000+ enterprise customers generating billions of daily API transactions. SAP runs HANA Enterprise Cloud on hyperscale infrastructure to deliver in-memory ERP processing at throughput rates of 100,000 to 1 million transactions per second for global manufacturing and retail customers.


Hyperscale architecture is essential for enterprise technology companies because the multi-tenancy model of SaaS delivery requires resource pools large enough to isolate each customer's workload while maintaining shared infrastructure efficiency ratios of 60 to 80% average utilization. A SaaS platform built on hyperscale infrastructure reduces per-customer infrastructure cost by 40 to 65% compared to dedicated single-tenant hosting, as resource pooling eliminates idle capacity waste across the customer base. The geographic redundancy that hyperscale multi-region deployments provide satisfies enterprise customer requirements for data residency compliance (GDPR, CCPA, and sector-specific regulations) across the jurisdictions where enterprise customers operate.


## What Qualifies as a Hyperscale Data Center?


A hyperscale data center qualifies under the category when the facility simultaneously meets defined thresholds across server count, physical floor area, and power capacity, with architectural characteristics including horizontal scalability, software-defined infrastructure management, and automated fault recovery. No single metric alone qualifies a facility; the classification requires the convergence of scale, design, architecture, and operational model.


The Uptime Institute and major research firms define the minimum quantitative thresholds at 5,000 servers, 10,000 square feet of raised floor space, and 20 megawatts of IT power capacity. A facility meeting 2 of the 3 thresholds but not the third does not qualify, as each dimension reflects a different aspect of the operational scale the hyperscale classification represents. Power density per rack must reach 10 kilowatts or above across the majority of installed racks, distinguishing hyperscale deployments from large-but-low-density colocation facilities that occupy equivalent floor space at lower compute intensity.


Architectural qualification criteria include the implementation of a spine-and-leaf network topology with non-blocking internal switching, a modular physical design that supports incremental expansion without facility shutdown, and a software-defined orchestration layer that manages resource allocation without manual provisioning. Operational qualification criteria include 24/7 automated monitoring with fault detection response times under 30 seconds, redundancy configurations at Tier III or Tier IV equivalent standards, and a documented capacity expansion roadmap. A facility that meets quantitative thresholds but operates with manual provisioning, non-modular architecture, or single-path power distribution does not qualify as a hyperscale data center under the architectural definition.


### What Is the difference between Hyperscale and Cloud Data Centers?


A hyperscale data center and a cloud data center are related but distinct concepts, with the hyperscale category describing a physical infrastructure classification and the cloud category describing a service delivery model that may or may not operate from hyperscale facilities. The distinction is architectural and operational rather than mutually exclusive, as many cloud data centers are physically hyperscale facilities, but not all hyperscale facilities deliver public cloud services.


A hyperscale data center is defined by measurable physical and architectural thresholds: minimum 5,000 servers, 10,000 square feet of raised floor, 20 megawatts of IT power, modular design, and automated management systems. A Cloud Data Center is defined by its service model: virtualized, provider-managed infrastructure delivered to tenants over the internet on a pay-per-use basis. A private enterprise could operate a hyperscale facility for internal workloads without offering any cloud services, and a small cloud provider could deliver cloud services from a non-hyperscale colocation environment.


The relationship between the categories becomes explicit in public cloud infrastructure, where providers such as AWS, Microsoft Azure, and Google Cloud build hyperscale facilities specifically to host the physical layer of their cloud service delivery. In that context, the hyperscale facility is the physical substrate, and the[cloud data centers](http://root-domain/data-center/types/cloud/) are the logical service layer running on top of it. The operational difference is that hyperscale refers to how the facility is built and managed, while cloud refers to how resources are packaged and sold to customers accessing the infrastructure over the internet.


#### Are Hyperscale Data Centers More Scalable Than Enterprise Data Centers?


Yes, hyperscale data centers are significantly more scalable than enterprise data centers across every measurable dimension, including server count, power capacity, storage volume, and network throughput. The architectural distinction between the two models makes the scalability gap a structural characteristic rather than a matter of degree.


An Enterprise Data Center is designed to serve a single organization's defined IT requirements, with capacity planned around projected internal demand over a 3 to 5-year horizon. Expansion requires capital expenditure approval, construction lead times of 18 to 36 months for new building capacity, and procurement cycles for new hardware that extend 6 to 12 months. Power capacity in an enterprise facility ranges from 1 to 20 megawatts, with expansion constrained by available utility capacity at the facility's location and the physical space within the existing building footprint.


A hyperscale data center is engineered from the ground up for continuous, rapid expansion through modular design, pre-provisioned power and network infrastructure, and automated orchestration that absorbs new server capacity into production workloads within hours of installation. Hyperscale operators add capacity in increments of thousands of servers per month, with campus-level power capacity expanding from 20 megawatts to 1 gigawatt through phased building additions that do not interrupt existing operations. The software-defined management layer in a hyperscale facility integrates new hardware automatically, while an enterprise data centers require manual configuration of each new server, storage array, and network port before the equipment enters production service.


#### Is a Hyperscale Data Center important for AI?


Yes, a hyperscale data center is critically important for AI, as the compute density, network interconnect bandwidth, storage throughput, and power capacity required for large-scale AI model training and inference are only achievable within hyperscale infrastructure. No other facility category provides the GPU cluster sizes, inter-node communication speeds, and sustained power delivery that frontier AI workloads demand.


Training a large-scale AI model at the level of current frontier systems requires 1,000 to 100,000+ GPUs operating in parallel for 30 to 90 continuous days, consuming 10 to 30 megawatts of power per training run. Hyperscale facilities are the only data center category that delivers sustained power at that level while maintaining the cooling infrastructure to manage the thermal output of GPU clusters operating at 40 to 100 kilowatts per rack. Enterprise and colocation data centers cannot accommodate GPU cluster power densities at that scale without a fundamental redesign of their power and cooling infrastructure.


Network interconnect within a hyperscale AI cluster delivers 400 to 800 gigabits per second of inter-GPU bandwidth through InfiniBand or RoCE fabric, a connectivity requirement that the standard Ethernet infrastructure of enterprise data centers does not support. The storage systems feeding AI training pipelines ingest datasets at rates of 1 to 10 terabytes per hour per training job, requiring distributed storage architectures with aggregate throughput from 100 to 1,000 gigabytes per second that only hyperscale object storage deployments achieve. As AI model sizes and training dataset volumes grow at rates that double compute requirements every 6 months, the hyperscale data center remains the only infrastructure category that scales at the pace AI development demands.


#### Disclaimer


*The content appearing on this webpage is for informational purposes only. Xometry makes no representation or warranty of any kind, be it expressed or implied, as to the accuracy, completeness, or validity of the information. Any performance parameters, geometric tolerances, specific design features, quality and types of materials, or processes should not be inferred to represent what will be delivered by third-party suppliers or manufacturers through Xometry’s network. Buyers seeking quotes for parts are responsible for defining the specific requirements for those parts. Please refer to our[terms and conditions](https://www.xometry.com/legal/) for more information.*
