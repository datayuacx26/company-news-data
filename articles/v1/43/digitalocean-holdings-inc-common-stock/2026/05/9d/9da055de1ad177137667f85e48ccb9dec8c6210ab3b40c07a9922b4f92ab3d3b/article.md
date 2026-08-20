---
schema_version: "1.0.0"
document_id: "9da055de1ad177137667f85e48ccb9dec8c6210ab3b40c07a9922b4f92ab3d3b"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-news-import-6460f63f60af"
canonical_url: "https://investors.digitalocean.com/news/news-details/2026/Hippocratic-AI-Scales-to-10-Million-Patient-Calls-at-99-9-Clinical-Safety-on-DigitalOceans-AI-Native-Cloud-powered-by-NVIDIA-Blackwell-Ultra-GPUs/default.aspx"
published_at: "2026-05-27T08:00:00+00:00"
first_seen_at: "2026-07-21T16:29:05.431888+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:51a750c8d0e5605a916e0d51a836f4731edd2650283d4c7a6d3a87f6aeebd4a0"
---

# Hippocratic AI Scales to 10 Million Patient Calls at 99.9% Clinical Safety on DigitalOcean's AI-Native Cloud, powered by NVIDIA Blackwell Ultra GPUs

[View All News](https://investors.digitalocean.com/news/default.aspx)


### Hippocratic AI Scales to 10 Million Patient Calls at 99.9% Clinical Safety on DigitalOcean's AI-Native Cloud, powered by NVIDIA Blackwell Ultra GPUs


May 27, 2026


*DigitalOcean’s AI-Native Cloud, optimized from infrastructure to inference, delivers 2× prefill speedup and ~30% higher per-node throughput for Hippocratic AI's Polaris system, through close collaboration with NVIDIA, proving that a purpose-built platform is foundational to healthcare AI.*


BROOMFIELD, Colo.--(BUSINESS WIRE)--


[DigitalOcean](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2F&esheet=54542302&newsitemid=20260527711308&lan=en-US&anchor=DigitalOcean&index=1&md5=41794a6a8c596d24bd369e5d759d5941) (NYSE: DOCN) today announced that[Hippocratic AI](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fhippocraticai.com%2F&esheet=54542302&newsitemid=20260527711308&lan=en-US&anchor=Hippocratic+AI&index=2&md5=055b588cf19d0fd26fe15a323fd2b018) 's Polaris system has reached 10 million patient calls at a 99.9% clinical safety score, running on NVIDIA HGX™ B300 GPUs on DigitalOcean's AI-Native Cloud, a five-layer, integrated stack purpose built for production AI. This milestone is the result of DigitalOcean engineering its inference platform for the latency, reliability, and concurrency demands of safety-critical healthcare workloads, delivering 2× prefill speedup and ~30% higher per-node throughput, developed in close collaboration with both NVIDIA and Hippocratic AI. The results demonstrate why an increasing number of production AI workloads are choosing DigitalOcean's AI-Native Cloud as the purpose-built home for inference at scale.


[Hippocratic AI's Polaris system](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fhippocraticai.com%2Fpolaris%2F&esheet=54542302&newsitemid=20260527711308&lan=en-US&anchor=Hippocratic+AI%27s+Polaris+system&index=3&md5=103925f4804cef1a47eebc7b5e1dac7d) has reported a 99.9% clinical safety score and an average patient rating of 8.95 out of 10 across more than 10 million real patient calls, supported by human evaluation involving more than 7,500 clinical staff. With more than 180 million patient interactions to date across chronic disease management, medication adherence, care gap closure, and clinical scheduling, Hippocratic AI is operating at a scale where the line between infrastructure performance and patient safety disappears.


"Polaris is built for the realities of clinical care: long sessions, real human conversations, zero room for error. With DigitalOcean and NVIDIA, we have early access to NVIDIA HGX™ B300 and the optimization techniques it unlocks, including NVFP4 quantization,” said Debajyoti Datta, Co-Founder, Hippocratic AI. “That is what allows us to hold a 400-millisecond time-to-first-token at production scale, on the clinical conversations our patients depend on."


**Engineered to Support Safety-Critical Inference**


Production healthcare AI breaks the assumptions most inference stacks are built on. Sessions are long. Tokens are time-sensitive. A dropped connection in the middle of a care plan retrieval is not a UX bug. It is a clinical interruption. Meeting that bar requires deep platform engineering and reliability at scale, the kind that off-the-shelf GPU access cannot provide and that only a purpose-built inference cloud can deliver.


Over the past year, the engineering teams at DigitalOcean worked in close collaboration with Hippocratic AI to optimize every layer of the inference stack. DigitalOcean engineered its AI-Native Cloud with hardware-aware scheduling, optimized inference runtimes, and platform-level scaling tuned for sustained high-concurrency workloads. Hippocratic AI's model team contributed proprietary inference work, including FP8 and NVFP4 quantization, KV-cache optimization, custom MoE kernels, and a cache-aware routing architecture that maximizes KV-cache hit rate and context reuse across long-horizon clinical sessions. NVIDIA provided early access to next-generation HGX™ B300 hardware, alongside engineering collaboration on Hopper and Blackwell architecture.


The combined result, on long-context clinical sessions, is approximately 30% higher per-node throughput and a 2× reduction in prefill latency, compared to a prior-generation stateless serving configuration. These gains build on the production efficiency Hippocratic AI announced earlier this month at DigitalOcean Deploy, where the company reported 2× production inference throughput and a 40% reduction in end-to-end P99 latency on the AI-Native Cloud.


"What Hippocratic AI has built in healthcare AI is remarkable, hundreds of millions of real patient interactions across some of the most complex and sensitive moments in people's lives,” said Paddy Srinivasan, Chief Executive Officer, DigitalOcean. “Delivering that at 99.9% clinical safety is what production AI looks like when it matters most. This is what purpose-built inference delivers, and it's what our AI-Native Cloud makes possible. Hippocratic AI's results are the proof."


**Among the First Production Customers on NVIDIA HGX™ B300**


Having Hippocratic AI among the first production customers on NVIDIA HGX™ B300 GPUs, made available through DigitalOcean's early work with NVIDIA, means DigitalOcean is validating its inference platform against one of the most demanding real-world workloads, not synthetic benchmarks. For workloads where every token affects clinical experience, Blackwell Ultra unlocks a step-change in capacity per node, allowing Hippocratic AI to support more concurrent sessions at the same latency targets and to extend context windows on long-horizon clinical conversations.


"The demands of safety-critical AI workloads are fundamentally different from consumer applications,” said Dave Salvator, Director of Accelerated Computing Products, NVIDIA. “DigitalOcean and Hippocratic AI are demonstrating how tightly integrated infrastructure and inference optimization, built on NVIDIA Hopper and Blackwell architecture, can deliver both performance and reliability at scale."


**A Different Bar for Healthcare AI Infrastructure**


The infrastructure requirements of safety-critical AI are not the requirements of consumer or enterprise AI scaled up. They are different in kind. Latency translates directly into clinical workflow quality. Reliability is measured in successful patient interactions, not nine-fives uptime. Cost efficiency determines whether a healthcare AI workload can scale to serve a population, not just a pilot.


In healthcare AI, infrastructure is not just about performance. It is foundational to patient safety. The Hippocratic AI deployment on the DigitalOcean AI-Native Cloud reflects this shift, and the platform engineering behind it shows what production AI looks like when infrastructure, model optimization, and hardware are designed together for outcomes that matter.


Read the full customer case study, including a video interview with Hippocratic AI Co-Founder Debajyoti Datta, at[digitalocean.com/customers/hippocratic-ai](https://cts.businesswire.com/ct/CT?id=smartlink&url=http%3A%2F%2Fdigitalocean.com%2Fcustomers%2Fhippocratic-ai&esheet=54542302&newsitemid=20260527711308&lan=en-US&anchor=digitalocean.com%2Fcustomers%2Fhippocratic-ai&index=4&md5=024bcb50391b74c255d34ad577aff3fa) .


**About DigitalOcean**


DigitalOcean is the AI-Native Cloud purpose-built for the inference and agentic era. Its five-layer integrated platform - spanning infrastructure, core cloud, inference, data, and managed agents - is open throughout with no vendor lock-in, giving builders everything they need to start fast, scale production AI workloads, and improve unit economics. More than 650,000 customers globally trust DigitalOcean to build, ship, and scale their applications. Learn more at[digitalocean.com](https://cts.businesswire.com/ct/CT?id=smartlink&url=http%3A%2F%2Fdigitalocean.com&esheet=54542302&newsitemid=20260527711308&lan=en-US&anchor=digitalocean.com&index=5&md5=8b9d2a2d341dcb5d7a514eb50833493d) .


**About Hippocratic AI**


Hippocratic AI has developed the safest generative AI Agents for healthcare. The company believes that generative AI has the ability to bring healthcare abundance to every person in the world. The company focuses on building non-diagnostic patient-facing clinical AI agents and does not allow its agents to be used to prescribe or diagnose. Hippocratic AI has received a total of $404 million in funding and is backed by leading investors, including Andreessen Horowitz, General Catalyst, Kleiner Perkins, Avenir, NVIDIA's NVentures, Premji Invest, SV Angel, Google’s CapitalG, and numerous health systems. Learn more at[https://hippocraticai.com/](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fhippocraticai.com%2F&esheet=54542302&newsitemid=20260527711308&lan=en-US&anchor=https%3A%2F%2Fhippocraticai.com%2F&index=6&md5=19a4111cb7a5fbba7f915827d51fd634) .


**Media Relations**
Meghan Grady
press@digitalocean.com


**Investor Relations**
Radu Patrichi, CFA
investors@digitalocean.com


Source: DigitalOcean


[View All News](https://investors.digitalocean.com/news/default.aspx)
