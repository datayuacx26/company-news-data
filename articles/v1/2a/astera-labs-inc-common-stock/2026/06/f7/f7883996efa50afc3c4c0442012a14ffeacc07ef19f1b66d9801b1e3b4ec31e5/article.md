---
schema_version: "1.0.0"
document_id: "f7883996efa50afc3c4c0442012a14ffeacc07ef19f1b66d9801b1e3b4ec31e5"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/resources/blog/computex-2026-agentic-workloads-cpus-take-center-stage-for-ai-infrastructure/"
published_at: "2026-06-11T23:23:21+00:00"
first_seen_at: "2026-07-20T23:21:25.038635+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:4daba7b0f321692eaaf59549e7622ab5bc9f2e0c8dfb85741eb9bceaf60e1259"
---

# Computex 2026: Agentic Workloads, CPUs Take Center Stage for AI Infrastructure

Computex 2026 put agentic AI and the CPU on the same track.


They showed up together in the biggest keynotes of the week. Intel talked about CPUs as the orchestration layer for agentic systems. Arm argued that agents are driving a fresh wave of CPU demand in the data center. NVIDIA introduced Vera as a CPU designed for agentic workflows rather than for the older model of general-purpose compute.


The practical takeaway from Taipei for AI infrastructure is clear: more coordination around the model, more pressure on memory hierarchy, and more value assigned to the fabric that moves data between CPUs, accelerators, memory, and storage.


## Agentic AI Changes the Shape of the System — and Brings the CPU Back Into Focus


The CPU story at Computex was really a systems story.


[Agentic AI is creating a different kind of workload](https://www.asteralabs.com/scaling-agentic-ai-with-cxl-memory-expansion-and-extended-pcie-6-signal-reach/) than the prompt-and-response pattern that defined most of the last two years. Agentic workflows carry more context, invoke more tools, hold more state, and create more orchestration work around the model. The CPU kept returning to the center of the conversation in Taipei for this reason: The workflow around the accelerator is getting busier.


The interesting nuance to this conversation is that vendors at the show highlighted different CPU capabilities as solutions to the same challenges. Intel emphasized core density and agent throughput. NVIDIA emphasized low-latency orchestration inside tightly coupled agentic loops. Both point to the same broader change: Agentic inference gives the CPU more work to do again.


## Once CPUs Matter More, the Handoffs Matter More


Intel and[SambaNova’s disaggregated inference demo](https://sambanova.ai/blog/first-disaggregated-inference-demo-for-ai-agents-live) was one of the clearest illustrations of where the system is heading. Their framing split the workflow across CPU orchestration, RDU decode, and GPU prefill, making the point that more of the inference path is now coordinated across multiple components rather than concentrated in one place.


This was one of several conversations at Computex to highlight that the next infrastructure constraint increasingly sits in the handoffs. The faster the accelerators get and the more modular the workflow becomes, the less tolerance there is for wasted time between steps. Data movement starts to look less like a supporting function and more like the thing that determines whether expensive compute is actually productive.


## Memory Tiering Is No Longer Just a Memory Story


Computex also made it easier to see why memory connectivity is becoming part of inference performance.


Longer context windows, multi-turn sessions, shared state across agents, and KV cache growth all push pressure down the stack. HBM remains the hot tier as it is directly integrated into GPUs, but the interesting question now is what happens after it fills up. That is where[CXL-attached memory](https://www.asteralabs.com/breaking-through-the-memory-wall-how-cxl-transforms-rag-and-kv-cache-performance/) , local storage, rack-level storage, and shared appliances start to matter.


CXL keeps resurfacing in these conversations because once the workload extends beyond the fastest local memory, the problem is not just capacity. It is how efficiently the system can step outward without dragging down time to first token or tokens per second. We’re used to thinking about the impact latency and bandwidth have on what matters most for[token economics](https://www.asteralabs.com/inference-tokenomics-how-cxl-memory-expansion-improves-ai-economics/) — GPU utilization — but more and more, memory distance from the GPU plays a critical role in this story.


## The Optics Discussion Is Getting More Immediate


Computex sharpened the edges of the optics conversation. The show-floor evidence was already there: end-to-end optical PCIe 6 links, linear pluggable optics, AI-factory scale networking, and co-packaged optics all had real mindshare in Taipei.


It’s most useful to think about this as a continuum, not a binary decision. Astera Labs’ position has been consistent: Use copper where you can and optical where you must. Winning systems will likely place the boundary between copper and optics where physics and economics align. That came through clearly in three demos showing the commonality in the retimer technologies on the board, in AECs and in AOCs. Our posts on[how Astera Labs is approaching optics for scale-up](https://www.asteralabs.com/scaling-the-ai-rack-how-astera-labs-is-approaching-optics-for-scale-up/) , our analysis of[the 400G-per-lane inflection point](https://www.asteralabs.com/the-400g-per-lane-inflection-point-where-copper-and-optical-meet-in-ai-infrastructure/) where copper and optical meet, and our[linear optics Computex demo](https://www.asteralabs.com/computex-2026-demo-how-linear-optics-enables-longer-link-reach-lower-latency-for-scorpio-smart-fabric-switches/) make the case that copper still holds important ground for short reach, reliability, and cost, while optics expands where reach, bandwidth, and power efficiency start to force the issue.


That is also why the CPO discussion now feels more immediate. The timing debate is ongoing, but the sequencing is starting to come into view.[Tae Kim’s Computex interview with NVIDIA networking SVP Gilad Shainer](https://taekim.substack.com/p/nvidia-executive-is-bullish-on-cpo) suggested NVIDIA is ready to start shipping co-packaged optics, with a ramp in the second half of 2026 beginning in scale-out and scale-up to follow later.


That leaves the industry arguing less about whether optics matters and more about where it lands first and how fast it gets adopted in multi-rack architectures.


## The Market Is Widening, Not Narrowing


Another major takeaway is that the AI rack is getting more heterogeneous, not less.


PCIe, CXL, Ethernet, UALink, NVLink Fusion, copper, NPO, CPO, merchant GPUs, and custom ASICs all showed up in the broader conversation around the show. Intel’s rack-scale blueprints and NVIDIA’s NVL72 and DSX framing both reinforced the same point in different ways: The rack is becoming the real unit of design.


This diversity has an important implication for infrastructure builders. The challenge is not just to choose the accelerator, but to make the full system coherent when compute, memory, protocol, and media choices are increasingly mixed. Flexibility is becoming a more valuable infrastructure trait.


## Closing Thoughts


Computex 2026 gave the industry a clearer view of what comes next.


Agentic AI is bringing the CPU back into focus. That shift is making data movement more consequential, memory hierarchy more strategic, and optics more immediate. The accelerators will keep getting faster. The system around them is getting more complex.


If there was one practical lesson from Taipei, it is this: The next round of AI infrastructure competition will be shaped as much by orchestration, memory tiering, and connectivity as by the accelerator alone.
