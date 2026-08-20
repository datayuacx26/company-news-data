---
schema_version: "1.0.0"
document_id: "08543a22985d611923b6ba9e89e4104d380f4b6ff95bdb02730ba123fa90dddd"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/why-your-mixture-of-experts-model-is-only-as-good-as-your-switch/"
published_at: "2026-05-05T20:00:00+00:00"
first_seen_at: "2026-07-20T23:21:25.038635+00:00"
fetched_at: "2026-07-28T22:12:47.698407+00:00"
content_hash: "sha256:f26d7f6109c0cfc2616ca83294c2381545ca96312e06e22772b955f30f1e1890"
---

# Why Your Mixture-of-Experts Model Is Only as Good as Your Fabric

*Introducing Hypercast™ for Improved Intelligence Benchmarks and Tokens-Per-Watt Performance*


The more capable you make a frontier AI model, the harder it becomes to run. More parameters, more experts, more sophisticated routing: every architectural improvement that lifts benchmark scores also lifts the communication demands on the hardware connecting compute accelerators such as GPUs. The legacy switches at the center of today’s AI clusters were never built for this, and the gap is widening.


Today’s leading models use a technique called mixture-of-experts (MoE), which delivers more capable, more efficient inference by routing each piece of data through a small subset of hundreds of specialized sub-networks. Instead of running every token through the full weight of the model, MoE activates only a small fraction of its experts for each token, which means more total capacity without proportionally more compute. Larger, more capable models become practical to run. But every routing decision is also a communication event: a demand placed on the switches and links that connect accelerators. More experts, more routing decisions, more communication load.


Engineers building these systems are facing design constraints imposed by the legacy interconnect. One significant source of pain is multicast – the crucial ability of the interconnect to take a single data packet from one GPU and send copies to many other GPUs. System designers are forced into architectural compromises by legacy switches that can’t configure multicast groups fast enough to keep pace with dynamic expert routing, and don’t support enough multicast groups to cover the collective operations a frontier model demands. These two problems show up either as unpredictable latency or in model capabilities deliberately hobbled during training to stay within what the interconnect can tolerate.


Two specific problems, one urgent solution. Both of these problems demand a form of multicast that legacy switches were never designed to provide: a new multicast purpose-built for AI that we’re calling Hypercast™.


## **Hypercast™ critical need #1: Mixture of Experts**


Mixture-of-experts is an LLM architecture that has exploded in popularity in the past year. MoE brings pronounced benefits—more capable LLMs, with much lower inference time and power because fewer weights are active per token—but at the cost of placing unprecedented demands on the interconnect. To understand why, let’s peek under the hood at what’s going on in these models. An MoE model trains a router to recruit different sets of experts – alternate paths through one layer of the model – for each token. As each token comes in, the router will evaluate it and choose some of the experts to activate – not all of them. That’s important for the design of the interconnect, because the chosen experts may live in different GPUs in the cluster; we’ll talk more about that in a moment.


*Diagram 1: MoE model schematic showing a row of many experts in parallel within an MoE layer that is one part of a model containing many such layers*


How many experts are we talking about here? Among open-weights models, Qwen3.5 has 128 to 256 experts, while DeepSeek V2 had 162 and DeepSeek V3 cranked it up to 256. Llama 4 Maverick has 128. There are some open-weights MoE models that make do with smaller numbers of experts (Grok-1 and Mixtral come to mind), but on the whole the trend is toward hundreds of experts. This trend makes sense: The models with higher expert counts have been racking up impressive scores on performance benchmarks while in some cases costing less to run.


#### Table 1: A survey of popular mixture-of-experts model architectures


Model Total layers Dense layers MoE layers Experts/layer Active/token


DeepSeek-V3\[1\] 61 3 58 256 routed + 1 shared 8 routed + 1 shared


Kimi K2\[2\] 61 1 60 384 routed + 1 shared 8 routed + 1 shared


Qwen3-235B-A22B\[3\] 94 0 94 128 routed 8


Mixtral 8x7B\[4\] 32 0 32 8 2


Grok-1\[5\] 64 0 64 8 2


To execute each MoE layer, the activations – the output of the previous layer – must be transferred to each GPU that implements one or more of the experts recruited by the learned router.


*Diagram 2: Activation in MoE layer, showing each token evaluated by the router and sent to a different group of experts*


That calls for duplication: copies of all the data must be sent from each member of one group of GPUs to each member of another group.


*Diagram 3: Schematic of GPUs sending duplicate data to multiple recipients through a switch*


But be careful! Just because we have, say, 256 experts does not mean that we have 256 GPUs that need copies of the data. Rather, we’ll have some smaller number of GPUs, with multiple experts residing in each GPU. How many experts should we put in each compute GPU? It depends on the capacity of the memory in the GPU, but beyond that, it’s an efficiency tradeoff. Cramming more experts into one compute GPU means less communication overhead in some phases of inference, but more overhead in others. System designers work hard to find the optimal point on that curve, and they depend on the interconnect to provide the correct feature set to enable them to make that optimization. We’ll talk more about this in a moment.


*Diagram 4: Expanded view of Diagram 3, showing multiple Layer N experts living in each of several GPUs, multiple Layer N+1 experts living in each of several other GPUs, and the Layer N router multicasting to the Layer N+1 experts*


Imagine a group of GPUs or other accelerators all connected to the same switch – perhaps 16, 32, or 64 GPUs depending on the radix and bandwidth supported by available switches. For a single layer of our model, the system designers will make every effort to keep it within this “tight cluster” of GPUs on the same switch, so that the most latency-sensitive communications only need to cross one switch “hop”. The goal is to build a cluster structure that matches real-world LLM communication patterns. Research shows that during LLM processing, “GPUs exchange data in sparse yet high-volume bursts within specific groups”\[6\] that are participating in tightly-coupled operations – those are the groups that system designers will aim to connect to a single switch if at all possible.


Now that our router has decided to recruit, say, eight experts out of the 256 total, that means there are between one and eight GPUs that need copies of the data. It’ll be one GPU if we got very lucky and all eight experts live in the same GPU, or eight GPUs if we got maximally unlucky and each expert in our selection lives in a different GPU; but usually somewhere in between.


*Diagram 5: Three cases — lucky (all active experts in one GPU), unlucky (active experts spread out over eight GPUs) and somewhere in between (active experts spread out over about four GPUs)*


We’d love to configure the interconnect with a multicast group in advance to handle this operation. However, the number of possible combinations of between one and eight GPUs chosen from a set of 32 is…big:


*Equation 1: Binomial coefficient 32 over N, N varying from 1 to 8*


Because the system doesn’t know which GPUs need a copy of the data until the router finishes evaluating the activations from the previous stage, system designers are faced with a choice between two painful options:


1. Let each GPU send out many copies of its data, which leaves the system bandwidth-constrained and the GPUs sitting idle for precious microseconds (not to mention burning extra power transmitting the same data over and over), or
2. Configure the interconnect dynamically to accommodate this last-minute multicast request – a slow process with traditional switches.


*Diagram 6: Two painful choices. In option 1, there is a bandwidth pinch point at the output of the sender GPU. In option 2, the configuration time is long and unpredictable.*


Let’s zoom in on option two – dynamically configuring the switch with a new multicast group for the expert activation step. The critical question here is – how long will it take? After the router tells the switch which GPUs are sending data and which GPUs need to get copies, how long do the sender GPUs have to wait until they can send their data?


System designers are faced with a bewildering landscape of configuration possibilities, with some switches requiring non-standard or proprietary – in some cases even out-of-band – configuration commands. Configuration times vary widely and are rarely specified. Experiments show legacy open protocol solutions delivering an average configuration time running to the hundreds of microseconds — but the most painful part, from a system design perspective, is the variability. There’s no telling how long a particular multicast setup operation will take with legacy open protocol solutions; the histogram has a long tail, with setup times up to tens of milliseconds!


*Diagram 7: Detailed view of option 2 from Diagram 6, with the configure-then-send flow shown step by step*


Recall that there are dozens of these expert layers — as many as sixty — in a frontier MoE model. That’s unacceptable latency from the user’s perspective. If the user gets a fast response 200 times and a slow response once, guess which one they post about? Which one do they remember when it’s time to renew the service? Even more impactful from a business perspective: Latency with a multi-order-of-magnitude variability is painful to system designers because it can give rise to hard-to-predict race conditions and other system faults, which translate into system downtime and lost revenue. For cloud service providers, unpredictable latency makes it harder to meet the metrics for consistent quality of service required by the agreements they’ve signed with their most important customers.


What we need here is an interconnect that is purpose-built for AI. Such an interconnect must be architected from the ground up to address the critical needs of real production AI model structures such as MoE. This is Hypercast: composable AI infrastructure from a single unified management software stack.


## Hypercast critical need #2: AllGather and Other Collective Operations


In contrast to the expert layer, with its separate parallel paths, a dense layer requires the GPUs to cooperate to calculate a very large matrix product. All LLMs in production today require many of these dense layers. In MoE models, don’t forget that every expert layer also includes a densely-connected (all-to-all) stage to ensure that the layer can learn to project the signals coming in from the previous layer into a space that works for its experts. In addition to the densely-connected stages in each expert layer, most MoE models include at least a few densely-connected layers; it all adds up to a significant number of all-to-all operations for each token, regardless of whether the LLM uses MoE architecture or not.


*Diagram 8: A schematic of a MoE model. Here, the dense stage in each MoE layer and the pure-dense layers are highlighted in green.*


These operations impose a difficult challenge on the system designer: Both for performance and also due to memory capacity constraints, the dense-matrix projection will be spread across many GPUs in the same cluster as a single operation. This type of parallel processing is usually described as a “tensor parallel” because all the GPUs are cooperating to calculate a single tensor (matrix) product. When we want to multiply large matrices spread across multiple GPUs, we can use collective operations such as AllGather to assemble the results.


*Diagram 9: GPUs connected to a switch performing the type of all-to-all multicast required for the dense layers in both MoE and other model types*


In the best case, it will be possible to keep the operation within the “tight cluster” of GPUs connected to a single switch. Regardless of how the GPUs are connected to each other, partial products must be multicast from each GPU to all the other participating GPUs.


*Diagram 10: Breakdown of an AllGather collective operation showing GPUs sending out their partial products all-to-all and then concatenating*


This is a far more frequent operation than the expert activation we discussed above, with a wide range of transfer sizes that runs from small snacks to massive feasts. For the little snacks, it’s inefficient to make the GPUs wait while we set up a new multicast group in the interconnect. It’d be much better to set it up in advance, so that when the data is ready to go, the GPUs can immediately send it on the multicast path with confidence that it will go to the correct group of destination GPUs. Recall that above when we were talking about activating a group of experts that we don’t know in advance, it wasn’t feasible to configure all possible multicast groups in advance, because the number of possible combinations is astronomical. Contrast that with this distributed matrix multiplication situation: Here, the system designer knows in advance which combinations of GPUs will be multicasting data to each other, because they’ve already decided how to partition the model across the GPUs that will run it in parallel. The number of combinations is large, but tractable.


*Diagram 11: GPUs sending duplicate data as part of a tensor parallel operation*


The critical resource in this case is the number of multicast groups supported by the interconnect. A traditional enterprise or datacenter switch will support a small number of these destination groups; likely too few to cover the combinations that frontier AI applications demand. That leaves system designers with another painful choice. Which of the collective operations should they expend their precious few multicast groups to accelerate? Which operations will happen most frequently? Which operations can be left to languish in unicast “bandwidth jail” without passing frustrating latency on to users, or worse — introducing hard-to-catch timing bugs that only make themselves felt after the system is in production, causing lost revenue and late-night calls from major customers when their quality-of-service metrics flag a violation?


*Diagram 12: Here, some operations are accelerated with traditional multicast, but some others are not because the old interconnect doesn’t support enough multicast groups; the bandwidth pinch site is highlighted*


What we need here is an interconnect that is purpose-built for AI, architected from the ground up to address the critical needs of real production AI model structures such as MoE. This is Hypercast: composable AI infrastructure for performance-sensitive collective operations, all from a single unified management software stack.


*Diagram 13: The problems are resolved because Hypercast provides a sufficient number of multicast groups so all operations can now be accelerated*


## Wrapping Up: The Solution Must Live in the Interconnect


Hidden within the technical papers describing all these frontier models are quiet sounds of pain: the sound of system developers working around the limitations of a traditional interconnect that was designed for the pre-AI world. Consider this phrase from the DeepSeek V3 paper: “Among the routed experts, 8 experts will be activated for each token, ***and each token will be ensured to be sent to at most 4 nodes.*** ”\[7\] Is that the sound a headache makes? The designer would like to allow the router to choose the best set of 8 experts for every token, but that would increase the maximum transfer latency too much. Instead, during training the model is forced to learn to live with the constraint. The resulting model isn’t just slower to run in production; its core capabilities have been undermined. When the interconnect is not architected for AI, then something else has to bend. System designers are forced into architectural compromises that present a bill later on down the line. We must stop forcing engineers to work around interconnect weakness by watering down AI model capabilities. The correct place to solve these problems is in the interconnect, with solutions born in the AI era.


---


**Sources:**


\[1\] arXiv:2412.19437
\[2\] arXiv:2507.20534
\[3\] arXiv:2505.09388
\[4\] arXiv:2401.04088
\[5\] GitHub: xai-org/grok-1
\[6\] arXiv:2509.15940v1
\[7\] arXiv:2412.19437
