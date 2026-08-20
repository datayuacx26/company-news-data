---
schema_version: "1.0.0"
document_id: "0e824009564324aec618412edf3fa833259d5711a8c34645d4b2f94ceb80839e"
company_key: "yc-cedana"
company: "Cedana"
source_id: "yc-cedana-news-import-d2b6a3d3a0df"
canonical_url: "https://cedana.com/blog/roofline-analysis-and-the-inference-value-chain/"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-08-18T05:08:30.961906+00:00"
fetched_at: "2026-08-18T05:08:32.098954+00:00"
content_hash: "sha256:25629d79f5613d5cc89de143876e6f7e73d860b07eb484a8210269ffefcb6eea"
---

# Roofline Analysis and the Inference Value Chain

OpenAI doubled token prices from GPT-5.4 to GPT-5.5, and Anthropic continues to tune its pricing as demand increases. It’s becoming clear to anyone integrating intelligence into their systems that the proprietary model providers are moving towards usage-based pricing.


This makes sense, given IPO ambitions. It results in pressures on organizations to either continue to allocate extra $/tokens or avoid frontier lab pricing altogether and bring the task of serving intelligence home.


Increasing capabilities of open source models, questions about privacy and a maturing ecosystem are all tailwinds pushing groups to actively consider deploying full systems of their own. Model capabilities have kind of supplanted the early infrastructure around AI, negating the need for complex RAG or training pipelines. Larger context windows now allow people to drop troves of data into a prompt and maintain multi-turn sessions against them (another security angle pushing towards self-hosting).


AI compute mix · 2022–2030


Training vs inference share of AI compute


Data: JP Morgan Equity Research, Paul Kedrosky, JPMAM, 2026.


> From the JPM Eye on the Market: Microsoft raised Copilot prices starting June 1st, and also cut its internal Claude Code licenses. Some token price increase examples: 3x – 9x for Opus variants, up to 9x for Sonnet, up to 6x for Gemini and 6x for GPT. Some users even reported 100x price hikes with these changes Anthropic’s Team Plan tops out at 150 seats. Larger customers are pushed to Enterprise where the public self-serve pricing is $20 per seat plus metered usage at standard API token rates with no included token allowance, meaning that the $20 seat fee only buys access and every token used is billed separately. The reason Anthropic is so focused on this: SemiAnalysis estimates the revenue opportunity loss associated with subscriptions vs API calls. For the Claude Max 20x plan, a subscription user would pay $200 per month but if these same tasks were executed at API pricing, they could spend as much as $8,000 instead.


US business paid adoption


Share of US businesses paying for AI models


Data: JP Morgan Asset Management, 2026.


So what do the economics of self hosting look like? Vector recently published this informative graph on the buy vs build story to self-host Kimi K3.


Buy vs build · Kimi K3


Annual cost to run Kimi K3: API vs self-hosted


Vector modeling · K3 API $5.40/M blended (80/20); self-host ~48 H100s at ~$2.50/GPU-hr, kept lit.


This is great for a birds-eye view on the story, told through token volume. There’s some interesting dynamics at play here however that are worth parsing out. Central to this is that once you stop renting intelligence, you own performance, and so it’s important to understand the mechanics intuitively.


Spinning up an 8xB200 node and sshing into it to deploy a running inference server is easy, but the path towards a productionized system for intelligence is further away from just that. What connects the disparate pieces is what governs the design of the hardware, the balance between compute throughput (FLOPs) and memory bandwidth - also known as roofline analysis.


The same analysis governs decisions at every layer of the stack, from the GPUs -> model -> engine -> use cases. We’ll walk up the stack, and pick apart where that tension rears its head every time.


The stack


Where the roofline shows up in the stack


To set the stage, it’s worth talking about the compute profile of inference, then start at hardware and move ourselves up the stack. Quick note that this is by no means comprehensive - the space is both large and moves incredibly fast.


## Inference


Breaking down the transformer architecture is out of scope here, the following resources will do it better justice:


Token generation splits into two phases with very different performance characters. Prefill ingests the input prompt in a single parallel pass (every token processed at once) populating the KV cache (see above primers on what the KV cache holds) and emitting the first output token as a byproduct.


Because many tokens go through together, the core operations are large matrix-matrix multiplies (GEMMs): lots of math per byte of weights loaded, high arithmetic intensity, squarely compute-bound: the right side of the roofline.


Decode then generates one token at a time. Each new token attends to the whole cached context, and the weight matmuls degrade from matrix-matrix (GEMM) to matrix-vector (GEMV): the full weight matrix is streamed from HBM to do only a vector's worth of work. Little math per byte, low arithmetic intensity: therefore memory bandwidth bound, the left side of the roofline. The token produced is appended to the context, and the process repeats, one HBM sweep of the weights per token.


Prefill vs decode


Prefill and decode as matrix operations


The tension is immediately clear here. Compute scaled faster than memory bandwidth when we jumped from A100s to H100s, so the ALUs are spending more time idle on larger, more powerful GPUs as decode is processed one at a time. The simple fix for this is batching, or processing multiple requests/tokens simultaneously. The entire optimization game now becomes managing this tension.


Batching


GPU utilization under three batching strategies


## Hardware


Roofline analysis falls out of two characteristics of the hardware, peak FLOPs (floating point operations per sec) and peak memory bandwidth (speed of onchip memory). You can plot this on a graph, commonly referred to as a roofline graph.


The roofline model


Attainable performance vs arithmetic intensity


Peak FLOPs/sec and peak memory bandwidth are fundamental characteristics of the hardware, fixed in the silicon and determined during chip design. To saturate the hardware and ensure maximum utilization, you want your system operating as close to the ridge point as possible - peak flops/peak bandwidth.


Compute has historically scaled much faster than memory bandwidth. Take the jump from A100s to H100s, where we went from ~312 TFLOP/s dense BF16/FP16, ~2.0 TB/s HBM2e (a ridge point of ~156 FLOP/byte.) -> ~990 TFLOP/s dense BF16, ~3.35 TB/s HBM3 (a ridge point of ~295 FLOP/byte). So compute ~3xed, while bandwidth ~1.7xed.


A100 → H100


Peak compute and memory bandwidth scaling


Notice the caveat - your FLOPs are dependent on the precision of the numbers you’re operating against, something that will come up in a later discussion. Precision is driving hardware design though as we’ll later see - newer generations optimize for operating with lower precision (or mixed precision) numbers.


Number formats


FP16, BF16, and FP8 bit layouts


This is all within a single GPU, once you start operating outside it (again as we’ll see later with systems like Mixture of Experts), you introduce a tertiary element to the system: you could now be waiting on networking between GPUs and nodes in addition to compute and memory-bandwidth.


## Model Architecture


The optimization space inside models themselves is wide. Let’s pull on a few that have direct relevance to what we walked through earlier.


MoE (Mixture of Experts) attacks both axes at once on the roofline plot. A Mixture-of-Experts model replaces the dense feed-forward block with many parallel "expert" blocks and a router that, per token, activates only a few of them. Take Qwen3.6-35B-A3B as an example - 35B is the total count of the parameters, while 3B represents the size of each expert. The result is a model with a huge total parameter count but a small active count: you pay the FLOPs of a small model while owning the weights of an enormous one.


Mixture of Experts


Parameter activation in dense and MoE models


That split is a roofline story, and it cuts against you in decode. Fewer active parameters means fewer FLOPs per token, the numerator of arithmetic intensity drops. But the weights still have to be somewhere, and during decode the router can send successive tokens to different experts, so in practice you stream far more weight than the "active" figure suggests to serve a batch. Low math, high memory traffic: MoE pushes decode further left on the roofline, deeper into the bandwidth-bound regime the phase already lives in. You've made the model cheaper to compute and more expensive to feed.


And it introduces a cost the single-GPU roofline can't even see. A 235B-parameter model doesn't fit on one GPU, so the experts are sharded across the node (` --tp 8` and, when attention and experts are split differently,` --dp 8 --enable-dp-attention` ). Now every token's router decision may point at an expert living on another GPU, so each decode step triggers an all-to-all exchange across the interconnect. This is the third axis we flagged at the hardware layer made concrete: you can be network-bound (NVLink or InfiniBand saturated) while compute units and HBM sit idle waiting for tokens to finish their round trip. It's why serving MoE has its own dedicated machinery (` --moe-a2a-backend deepep` ,` megamoe` ), which is an engine concern we'll pick up in the next section.


If MoE changes how many weights you touch, quantization changes how many bytes each one costs. It's the most direct lever on arithmetic intensity in the stack: same math, fewer bytes moved, so every operation's intensity rises and the whole workload slides right, toward the compute-bound side of the ridge.


The reason that matters follows straight from the decode picture. Decode is bandwidth-bound with one HBM sweep of the weights per token, so its cost is set by bytes streamed, not math done. Halve the bytes per parameter (BF16 → FP8, or further to FP4, as in` --quantization modelopt_fp4` ) and you roughly halve the traffic. You also collect a second win the hardware layer set up: low-precision tensor cores have a higher FLOP ceiling (B200 FP4 peak is 9 PFLOPS, 18 PFLOPs with structural sparsity (in effect skipping 0s)) so quantization simultaneously lowers the bytes and raises the roof.


But "quantization" is really two levers aimed at two different ceilings, and conflating them is a common mistake. Quantizing the weights attacks the bandwidth bound: less to stream per token. Quantizing the KV cache (` --kv-cache-dtype mxfp8` ) attacks a different wall entirely: memory capacity. The KV cache grows with every token of context and every concurrent request, and on long-context or high-concurrency workloads it, not the weights, is what exhausts HBM.


The third lever is subtler. Everything above assumes full attention, every token attends to every prior token, which is why the KV cache grows linearly with context and, on long sessions, becomes the dominant claim on memory. That growth means a full-attention model drifts further into memory-bound territory the longer a conversation runs. Newer architectures attack this directly: sliding-window and state-space (mamba-style) variants cap or flatten how memory scales with context, trading a measure of modeling reach for a memory cost that stops climbing. The details we’ll save for a future post on model specific optimizations, getting into optimizations that some of the larger frontier-class models (like GLM5.2 or Kimi K3) employ.


To wrap up, this roofline graph from the Kimi K3 tech blog ([https://www.kimi.com/blog/kimi-k3](https://www.kimi.com/blog/kimi-k3) ) is a great example of an optimization, demonstrating a generated compact compiler for CUDA kernels - MiniTriton. The graph is useful in understanding how kernel level optimizations help move us along the roofline.


MiniTriton · NVIDIA L20 (sm_89), fp32


Kernel performance against the L20 roofline


Recreated from the Kimi K3 tech blog — MiniTriton CUDA-core roofline (NVIDIA L20, sm_89, fp32).


## Engine/Serving


Finally, said models need to be served on said GPU. There’s some complexity baked in here, because kernels can be compiled for specific architectures, taking advantage of features that are extant in some but not others.


For the sake of simplicity, we’ll look at sglang, although vLLM and TensorRT are popular and performant engines in their own rights.


Let’s start with batching. The primer left us with idle compute: decode streams the full weights from HBM to produce a single token, so the ALUs starve waiting on memory. Batching works to alleviate this.. Instead of serving one request's decode step at a time, the engine groups many requests together and runs their decode steps as one pass, the weights are swept from HBM once and reused across every request in the batch. You can think of the memory load (or throughput penalty) being amortized across multiple requests.


The naive version of collecting N requests -> running them together -> waiting for all to finish wastes the hardware, because requests finish at different lengths and the batch stalls on the slowest. Modern engines use continuous (or in-flight) batching: requests join and leave the running batch token-by-token, so a finished request is immediately evicted and a waiting one takes its slot. The GPU stays saturated instead of draining and refilling.` --max-running-requests 256` is the ceiling on how many the engine will hold in flight at once, a direct dial on how far up the roofline you push. The tradeoff here is non-weights memory use, or the KV cache, which grows with the context.


From earlier, we know that MoE models can be network-bound, because every token's router decision may point at an expert living on a different GPU, and each decode step triggers an all-to-all exchange across the interconnect. That's a property of the model's shape. What the engine does about it is expert parallelism, which is why it has dedicated backends (` --moe-a2a-backend deepep` ,` megamoe` ). These we’ll also get into in a later post!


The engine's job is to keep the interconnect from becoming the bound. Two moves matter. First, placement: how experts are distributed across GPUs (` --dp 8 --enable-dp-attention` splits attention and experts differently, because they have different parallelism sweet spots: attention likes data parallelism, experts like expert parallelism). Second, overlap: the all-to-all is communication, and communication can be hidden behind computation, while one layer's tokens are in flight across the network, the GPU works on math it already has, so the network latency doesn't stall the pipeline. A good MoE backend is mostly a scheduler for hiding the network behind the compute. Get it right and the interconnect roofline stops binding; get it wrong and you've bought a giant model that spends its time waiting on NVLink.


The KV Cache grows with every token of context and every concurrent request, and on long or high-concurrency workloads it is what exhausts HBM (not the weights, as that’s fixed). Architecture decides how the cache grows; the engine decides how it's stored and reused, and two techniques do most of the work.


Paged attention borrows the idea of virtual memory: instead of reserving one big contiguous block per request (which fragments badly as requests come and go at different lengths), the cache is split into fixed-size pages allocated on demand. Fragmentation collapses, so you fit more concurrent requests in the same HBM, which increases how large a batch you can run. PagedAttention is the paper that vLLM was built around.


Prefix caching exploits reusing when many requests share a prompt prefix, a common system prompt, a shared document or a common codebase. SGLang, for example, is built around RadixAttention. Rather than caching a single prefix, it organizes all active caches into a radix tree. A shared system prompt sits near the root; a shared document branches below it; each session's divergent tail hangs off as its own leaf. Any new request walks as far down the tree as its prefix matches and only computes prefill for the part that's new, valuable for agents or multi-turn sessions.
