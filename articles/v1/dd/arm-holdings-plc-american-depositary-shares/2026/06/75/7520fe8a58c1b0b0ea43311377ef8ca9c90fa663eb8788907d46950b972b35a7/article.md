---
schema_version: "1.0.0"
document_id: "7520fe8a58c1b0b0ea43311377ef8ca9c90fa663eb8788907d46950b972b35a7"
company_key: "arm-holdings-plc-american-depositary-shares"
company: "Arm Holdings plc"
source_id: "arm-holdings-plc-american-depositary-shares-news-import-5227abe14b68"
canonical_url: "https://newsroom.arm.com/blog/agentic-ai-rack-scale-infrastructure"
published_at: "2026-06-25T16:00:00+00:00"
first_seen_at: "2026-07-21T07:43:11.542330+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:47dbcb9be2b1f083fd33bd44c000a057012845d4ae5c27a904f68eb0e8d88b4c"
---

# From host node to heterogeneous rack: Rethinking the AI CPU

AI infrastructure is entering a crucial new phase. The first phase of generative AI infrastructure was defined by accelerator scale: how many GPUs, NPUs or custom AI accelerators could be deployed, powered, cooled and connected. That phase is not over, but it is no longer sufficient.


The next phase is about rack-scale system composition: heterogeneous AI racks where different compute resources are optimized for different phases of the agentic AI workflow. Specialized racks of CPUs, accelerators, memory and networking are being assembled into gigawatt-scale AI superclusters, with each rack operating as a dense compute engine for a specific part of the workflow.


As AI inference evolves from single-pass model calls to multi-step agentic pipelines, infrastructure design is shifting from a narrow focus on accelerator scale to a broader question of system composition: how to assemble the right compute for each phase of the agentic AI workflow. Accelerators remain critical for prefill, decode and model execution. CPUs become critical in the surrounding orchestration layer, where agents fan out across tools, APIs, retrieval systems, cloud-native services and multiple model calls.


That shift matters because inference is changing structurally. Traditional inference was often a relatively linear process: a request enters the system, a model generates a response and the transaction ends.[Agentic AI](https://newsroom.arm.com/blog/what-is-agentic-ai) behaves differently. Agents plan, retrieve data, call tools, run code, invoke APIs, reason over intermediate results and loop through multiple model calls before producing an answer.


This changes the shape of the data center. More of the workload sits outside the neural network forward pass. More time is spent coordinating work across accelerators, memory, storage, networking and software services. More infrastructure decisions are made at rack scale rather than server scale.


As[Austin Lyons recently argued in Chipstrat](https://www.chipstrat.com/p/are-agentic-cpus-a-commodity-its) ** “The CPU is not a commodity, but it is not a single prize either.” That framing is important because heterogeneous AI racks do not need one generic CPU. They need CPUs optimized for the specific work being performed: keeping accelerators fed, managing memory-intensive decode and executing the agentic work that happens between model calls.


## The rack is becoming the AI system


AI infrastructure is becoming more specialized at each stage of the inference pipeline. One of the clearest examples is the growing separation between prefill and decode. Prefill is the phase where the system processes the input prompt and builds the KV cache; it is typically compute-intensive and accelerator-heavy. Decode is the phase where the system generates the response token by token; as context grows, it becomes increasingly constrained by memory bandwidth, memory capacity and KV cache movement.


A heterogeneous AI rack must make many specialized components behave like one coherent system. It must route requests to the right compute tier, move data efficiently between prefill and decode, manage KV cache transfers, maintain session state, execute non-model work and enforce service-level objectives across the full pipeline.


That is why the industry is moving toward a more system-level view of AI infrastructure. The amount of useful agentic work a data center can perform is not determined by accelerator capacity alone. It also depends on CPU capacity, memory bandwidth, network performance, software orchestration and the ability to balance the whole system under real workload pressure. The architectural question is shifting from “how many accelerators can we deploy?” to “how do we specialize and coordinate each stage of the agentic AI workflow?”


In this architecture, there is not one CPU use case; there are multiple.


## Three CPU roles in the heterogeneous AI rack


- The first role is the **prefill host CPU** (coordinates the compute-intensive phase of inference).
- The second role is the **decode host CPU** (manages the latency-sensitive, memory-intensive phase of inference).
- The third role is the **agent or worker CPU** (the compute tier where the agent lives between model calls).


These three roles are related, but they should not be evaluated with the same checklist.


This reframing is important. The industry has often talked about the “host CPU” as a single supporting component. In heterogeneous rack-scale AI, that framing is too narrow. Infrastructure teams need to evaluate CPU capacity according to the distinct work being done at each layer of the inference pipeline.


## Why this changes infrastructure design


With this new system-level approach, infrastructure teams can right-size each tier based on actual workload behavior. For example, a workload with long prompts and short outputs may require more prefill capacity. A chatbot with long-running conversations may require more decode and memory bandwidth. A multi-agent enterprise workflow may require significantly more worker CPU capacity for tool execution, retrieval, policy checks and application orchestration.


At the same time, one of the historical barriers to heterogeneous infrastructure is beginning to fall. Multi-architecture deployments have traditionally carried real software friction: porting applications, validating software stacks, tuning performance and adapting DevOps workflows across different compute platforms. But agentic software development is compressing that work. AI-assisted porting, testing, validation and operations are reducing the time and risk required to support large-scale multi-architecture environments.


That matters because the architectural question is changing. Infrastructure teams no longer need to force every workload onto a single default architecture simply to avoid operational complexity. They can increasingly choose the right architecture for the right tier of the AI system and use modern software automation to manage the complexity behind the scenes.


This also changes the economics of AI infrastructure. The key question is no longer just which accelerator delivers the highest model throughput. It is how efficiently the full rack converts power, memory, I/O and software coordination into useful AI work.


That is a system question, not a component question.


It is also where Arm’s architectural strengths become especially relevant. AI infrastructure is moving toward heterogeneous compute, high concurrency, power-constrained scaling and software-defined orchestration. These are precisely the conditions where performance per watt, ecosystem breadth and architectural flexibility matter most.


Mohamed Awad, EVP, Cloud AI Business Unit, Arm, on why performance per watt is the new AI data center benchmark


## Arm AGI CPU and the agentic infrastructure era


The[Arm AGI CPU](https://newsroom.arm.com/blog/introducing-arm-agi-cpu) was designed for this shift. It extends Arm’s cloud infrastructure roadmap into a new class of agentic AI systems where CPUs play a central role in orchestration, memory coordination, accelerator management and rack-scale efficiency.


For heterogeneous AI racks, the value is not just one specification. It is the combination of high core density, memory bandwidth, high-speed I/O, CXL readiness, software maturity and efficiency within modern datacenter power constraints. Those attributes map directly to the three CPU roles emerging in rack-scale AI I referenced earlier: prefill host, decode host and agent worker.


Arm’s broader ecosystem gives infrastructure builders choice. Arm-based CPUs already sit across cloud, networking, edge and AI systems. Partners can build with Arm IP, Arm Compute Subsystems or production silicon depending on the level of customization and time-to-market they need. That flexibility is becoming more important as AI infrastructure fragments into specialized but interconnected tiers.


The industry is moving beyond a simple accelerator-first view of AI infrastructure. Accelerators remain critical, but they are only one part of the system. The differentiator will increasingly be how well the full rack is orchestrated, how efficiently resources are used and how predictably the system performs under real agentic workloads.


In the agentic AI era, the rack becomes the system. Heterogeneity becomes the norm. And the CPU becomes one of the most important architectural decisions in the AI data center.
