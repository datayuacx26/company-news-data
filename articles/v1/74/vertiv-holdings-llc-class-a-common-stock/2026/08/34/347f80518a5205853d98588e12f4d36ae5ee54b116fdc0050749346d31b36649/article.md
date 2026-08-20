---
schema_version: "1.0.0"
document_id: "347f80518a5205853d98588e12f4d36ae5ee54b116fdc0050749346d31b36649"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/blog-posts/from-pilot-to-production-three-enterprise-ai-infrastructure-decisions-that-matter/"
published_at: null
first_seen_at: "2026-08-13T04:02:26.871985+00:00"
fetched_at: "2026-08-13T04:02:28.763826+00:00"
content_hash: "sha256:2421663cfe564efbb4a826065eede9cdae9a3b8200fd19f9a5690ea2bd9e0917"
---

# From pilot to production: Three enterprise AI infrastructure decisions that matter

Enterprise AI is entering a more consequential phase.


For the past several years, many organizations have focused on experimentation: testing copilots, evaluating frontier models, automating selected tasks, and proving that AI could create value. Those pilots were typically optimized for speed, accessibility, and flexibility. Public cloud platforms made it possible to move quickly without making long-term infrastructure commitments.


Production changes the equation.


Once AI becomes embedded in business processes, the organization must support more users, more applications, more data, and more sustained consumption. The workload may become operationally critical. Costs become material. Governance becomes more complex. Latency and data movement begin to matter. The consequences of failure become greater.


In a recent DCD AI Week discussion with Diana Kearns-Manolatos and Matt Jacobs from Deloitte, we explored how enterprises should move from pilots to production without allowing infrastructure to become the next constraint.


Three takeaways stood out.


### 1. The workload should determine where AI runs


The cloud-versus-on-premises debate is too simplistic.


Enterprise AI is becoming a portfolio of workloads, each with different operating characteristics. A general employee copilot may be well suited to a cloud-centric model. An engineering simulation using proprietary product data may require greater control. A manufacturing vision application may need to run near the equipment generating the data. A real-time control system may require local, predictable response.


The right starting point is therefore not the technology platform. It is the workload.


*Source: Vertiv*


The workload behavior can be understood through a small number of repeatable archetypes:


- Model development and experimentation
- Operational retraining and recalibration
- Generative and interactive applications
- Pattern recognition and anomaly detection
- Predictive applications
- Real-time control and autonomous systems


Those archetypes help describe what the workload does. Four additional constraints help determine how much control the enterprise requires:


**Criticality** : What happens if the workload is unavailable, delayed, or wrong?


**Data sensitivity** : Does it use customer information, financial data, employee data, engineering intellectual property, or regulated information?


**Latency** : How quickly must the result reach the user, business process, machine, or control system?


**Data gravity** : Where does the data already reside, and how practical or economical is it to move?


Scale and utilization then influence the economics.


This logic leads naturally to a hybrid estate. A complex enterprise may use public cloud, private infrastructure, dedicated colocation, NeoClouds, and edge capacity at the same time.


That should not automatically be viewed as unnecessary complexity. It can be the result of disciplined workload placement.


The important question is not, “Where should enterprise AI run?” It is:


What does this workload require, and which environment provides the right combination of economics, performance, governance, and control?


### 2. Token economics will increasingly shape infrastructure strategy


AI cost is different from conventional software cost.


Consumption can grow non-linearly as more users adopt an application, as agents make repeated calls to models, and as AI becomes embedded in increasingly complex workflows. Enterprises may understand the direct cost of individual cloud applications while still lacking a consolidated view of AI consumption across business functions, software platforms, SaaS contracts, and internally developed applications.


This makes transparency essential.


Organizations need to understand:


- Which users and applications are consuming tokens
- How many tokens are required to complete a task
- Which models are being used
- Whether the selected model is appropriate for the task
- What business result is being created
- How utilization changes over time
- Whether costs are predictable enough to support planning


Total token volume alone is not a sufficient measure. Enterprises need to distinguish productive consumption from inefficient or poorly governed consumption.


A large model may be unnecessary for a relatively simple task. Separate prompts may repeatedly rebuild the same context. Agentic systems may make multiple calls that are not visible to the end user. Different business units may acquire overlapping AI capabilities without an enterprise-wide view of consumption.


The more useful management question may eventually become:


**How much business value is created for every token consumed?**


Infrastructure placement becomes part of this discussion when a workload develops a sustained and predictable base demand.


Cloud elasticity remains valuable for experimentation, variability, and access to leading models. But once utilization becomes steady, cost predictability becomes important, data becomes more sensitive, or latency becomes restrictive, private or dedicated infrastructure may become economically and operationally rational.


This is not an automatic migration away from cloud. It is a shift toward a more intentional hybrid model.


Workloads should be reassessed as they mature. A workload might begin in public cloud, move to a hybrid model as demand becomes predictable, and later use dedicated infrastructure or edge capacity for specific components.


Placement should therefore be treated as a lifecycle decision, not a permanent decision made during the pilot.


### 3. The most common enterprise journey may be brownfield to GPU production


Most enterprises will not begin with a greenfield AI factory.


They are more likely to start with an existing enterprise facility, a conventional server room, an established colocation footprint, or space originally designed for lower-density CPU infrastructure.


At the same time, many enterprises have spent years reducing internal data center engineering and operations capability. They may have strong teams in applications, data science, cybersecurity, and cloud architecture, but relatively few people with experience designing, commissioning, and operating high-density, liquid-cooled GPU environments.


This creates what I describe as the brownfield-to-GPU production challenge.


The existing facility may have power available on paper, but that does not mean the power is usable for AI. Distribution, protection, redundancy, cooling, heat rejection, controls, or network layout may prevent the facility from supporting the intended compute.


Enterprises considering an on-premises or dedicated-colocation deployment need to evaluate:


- Available power versus usable AI power
- Power distribution and protection capacity
- Rack density and floor-space constraints
- Air, liquid, or hybrid cooling requirements
- Heat rejection capacity
- CDU and secondary-fluid-network ownership
- Network topology and rack proximity
- Controls and monitoring
- Commissioning requirements
- Operating and maintenance responsibility
- Expansion to future compute generations


The encouraging point is that the transition is very possible.


Vertiv has supported multiple deployments in which an existing environment was upgraded to support modern GPU infrastructure. Success depends on treating the deployment as a coordinated system transition rather than a server installation.


**The enterprise does not need to become a hyperscaler. It needs the right combination of:**


- A clear workload strategy
- A validated reference architecture
- Partners aligned across the technology stack
- Defined responsibilities and demarcations
- System-level commissioning
- An operating model for the full lifecycle


**This is where the partner ecosystem becomes critical.**


Deloitte can help an enterprise establish the business case, economics, governance, and operating model. Silicon providers and OEMs define the compute architecture. Cloud and colocation providers supply different capacity models. System integrators connect applications and technology platforms. Vertiv translates compute requirements into the power, thermal, controls, deployment, commissioning, and lifecycle infrastructure required to operate the system.


The customer ultimately needs one operating outcome, even though many organizations contribute to it.


**The seams may determine whether the strategy succeeds**


The final theme from our discussion was the importance of the seams and interfaces.


Those seams do not begin in the data center.


They begin between the desired business outcome and the selected AI use case. They continue between the workload and the placement decision, between strategy and execution, between the cloud and private portions of a hybrid estate, and between the organizations responsible for governance, finance, technology, facilities, and operations.


Within the physical infrastructure, the seams appear between compute, networking, power, cooling, controls, commissioning, and service.


The individual elements may all be technically sound, while the overall deployment still underperforms because ownership, assumptions, and interfaces were not defined.


The enterprises that scale AI most successfully will be those that manage these seams deliberately.


They will understand the workload before selecting the infrastructure. They will create transparency around token economics. They will design a growth path before installing the first production cluster. And they will align partners around one operating outcome rather than a collection of individual deliverables.


### Two questions are worth leaving enterprise leaders with:


Is your AI strategy designed around where workloads run today, or around how they may need to move as scale, economics, and risk change?


And if your most successful AI workload doubled tomorrow, would the next bottleneck be the model, or would it be the facility, the power, the cooling, the operating model, or the interfaces between them?


Watch the full DCD discussion on demand:[From pilot to production - Matching enterprise AI workloads to infrastructure models](https://www.datacenterdynamics.com/en/dcd-broadcasts/ai-week-2026/from-pilot-to-production-matching-enterprise-ai-workloads-to-infrastructure-models/)
