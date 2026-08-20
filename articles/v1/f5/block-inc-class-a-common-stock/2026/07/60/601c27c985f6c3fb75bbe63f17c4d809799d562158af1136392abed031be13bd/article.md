---
schema_version: "1.0.0"
document_id: "601c27c985f6c3fb75bbe63f17c4d809799d562158af1136392abed031be13bd"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/buzz-sharing-compute-powered-by-meshllm"
published_at: "2026-07-27T12:00:00+00:00"
first_seen_at: "2026-08-04T09:20:55.656727+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:115b6f426f98226eb26095d8516a1434c296773fd5af7480f5fe3af01d93efde"
---

# Buzz: Sharing compute powered by MeshLLM

Buzz is all about sharing: people share knowledge, and agents share spaces with people.


Buzz shipped with a (not very well) hidden feature for "sharing compute." This feature enables Buzz community members to contribute and use compute for AI and LLM inference without sharing the agents themselves.


The[MeshLLM project](https://github.com/Mesh-LLM/mesh-llm/) powers this feature. MeshLLM develops distribution technology and models that can work in a pool, allowing people to share idle compute for inference. The shared resource is inference compute rather than agents themselves; the privacy and security considerations depend on how the mesh is configured. The workloads are mostly floating-point operations, often quantized to integers in current model implementations, but that is a detail for another time.


The reality is that you often have a ton of idle compute in your room at any one time. If not in your room, house, or office, perhaps you have capacity that you aren’t using at night or on weekends. MeshLLM lets you pool that capacity privately across your own devices, with family, or, in this case, with members of your Buzz community or server. Buzz presents a natural use case because its communities bring together people with similar interests who have an incentive to share excess capacity.


It's a bit like friends or neighbors sharing power tools. Instead of everyone needing to own everything, the community gets more value by pooling tools that spend most of their time unused. And as with borrowed tools, it comes down to trust: prompts and any data sent to another machine are visible to its owner, and you are trusting the results that come back. Share compute with people you trust.


# Sharing models


MeshLLM puts that shared capacity to work in three main ways:


## Sharing is caring


The simplest approach is for someone to run a model and share it with other members of Buzz. If more people share the same model, the community gains extra capacity for the same intelligence.


## Mixture-of-Agents intelligence


When several models are available, the mesh can send a request to multiple models and combine their responses for robustness, consensus, and correction. This approach resembles the[Mixture-of-Agents](https://arxiv.org/abs/2406.04692) framework, in which multiple models propose responses and an aggregator synthesizes them. Combining models can improve answers on some tasks, but adding more models does not automatically produce a better result.


During prefill or decoding, MeshLLM can use model confidence signals to identify when a model may be going off track or about to hallucinate. The model can then request input from other models before continuing through the pipeline.


## Distributed inference


Distributed inference is the most aspirational mode of compute sharing. The MeshLLM project pre-splits a model into layers so that each machine downloads only the layers it needs, reducing the model’s on-disk footprint on each machine. The mesh allocates the layers to pipeline stages and connects the stages using the lowest-latency paths available between nodes. Distributed inference remains advanced and challenging to get right.


The payoff is the ability to run much larger models by distributing their layers across multiple machines. The trade-off is communication latency between stages, which can leave parts of the inference pipeline idle. Speculative techniques, including draft models, n-gram matching, and multi-token prediction (MTP), generate candidate tokens that later stages can verify. Verifying candidate tokens can keep the pipeline busy and potentially reduce response time. Multiple concurrent sessions can also be batched through the stages to improve overall throughput. Like a CPU pipeline, the system works best when every stage has useful work to do.


So far, the largest models we have run are around the 500-billion to 1-trillion parameter scale, including Qwen and Inkling. The effective throughput of distributed inference depends heavily on the network and on how effectively the predictors keep each stage busy. Improving that throughput remains an active area of research.


With Buzz, we are working to create an experience that matches people’s expectations. For example, do you start with many smaller models or patiently wait for enough peers to form a mesh for a frontier-scale model?


Distributed inference across multiple machines depends on mature networking technologies.[Iroh](https://github.com/n0-computer/iroh) and other libraries provide QUIC-native NAT traversal and encrypted relay fallback when a direct peer-to-peer connection cannot be established.


# MeshLLM


MeshLLM is available as a binary that can run as a daemon or as an SDK that applications can embed in various languages, as Buzz does. MeshLLM fetches the runtime that best matches the platform to make effective use of the available resources. One of the project’s goals is heterogeneous platform support, so we spend a lot of time working with different hardware combinations, especially around releases.


Like Buzz, MeshLLM is written in Rust. MeshLLM maintains a small set of patches to` llama.cpp` while those changes await inclusion upstream. MeshLLM also ships with an optional web console for monitoring usage or basic chat. At its core, MeshLLM exposes an inference endpoint that any agent harness can use.


MeshLLM also has a “public mesh,” where people share idle compute in a large global pool for fun and experimentation. The public mesh operates on a strictly best-effort basis, with no guarantees.


# Conclusion


It is an exciting time for open models. New models, incentives, and business models are emerging, pushing forward the development of models that people can run themselves.


Reusing idle hardware isn’t specific to model hosting. Google has discussed[using compute from retired phones to reduce waste](https://research.google/blog/a-low-carbon-computing-platform-from-your-retired-phones/) , which is food for thought at a moment when the hefty laptop I bought went up in value the week after I got it because of chip prices and demand. Has that ever happened in computing history?


There is a lot going on in this post (and in this space). I would encourage you to join the community and try it in your own applications! Like, star, and share it. The more platforms and models that we experiment with, the better the experience will get for everyone.


You can learn more at[meshllm.cloud](https://meshllm.cloud/) ,[join the MeshLLM project on GitHub](https://github.com/Mesh-LLM/mesh-llm/) , or watch[my deeper-dive presentation on MeshLLM](https://conffab.com/presentation/what-if-you-never-needed-an-api-key-again-building-a-mesh-llm-from-spare-compute/?gl=nlhvzUAbn1Ac) from the AI Engineer conference series in Melbourne.


Enjoy!
