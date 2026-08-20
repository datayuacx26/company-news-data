---
schema_version: "1.0.0"
document_id: "252869d9404e4fa2d848bff57865e371323da9c3492de413228b3eb2448ccc3b"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/maximize-frontier-models"
published_at: "2026-06-10T14:27:49.137+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:a06bbddd3231f044b92c86a80eb4ad87d394f50fb98f908b9536c3edfd7b7726"
---

# The Inference Alpha: Maximizing Frontier Models on AMD

At DigitalOcean, we’re committed to providing high-performance infrastructure for the next generation of AI, which is why we’ve been focused on hosting frontier Large Language Models (LLMs) on frontier GPUs—including **AMD GPUs** .


We see inference performance as an intricate systems-level challenge. For frontier open-weight models, achieving peak output speed is not just about the raw hardware. It also depends on a complex interaction between model architecture, runtime execution, memory systems, scheduling, and decoding strategy.


We believe there’s a significant **“performance alpha”** found in specialized inference engineering. Optimizing for both speed and cost-efficiency requires a much deeper approach than standard configuration sweeps. By taking a custom approach to the software stack, we can demonstrate that achieving performance parity with more expensive hardware is entirely possible.


While the current software ecosystem often presents non-obvious hurdles, deep engineering allows us to deliver stronger inference economics on high-performance AMD infrastructure relative to conventional flagship deployments.


## The Proof is in the Throughput


To ground our “Performance Alpha” theory in reality, DO worked with[Wafer](https://www.wafer.ai/) to achieve high performance on specific frontier models on AMD GPUs through various optimizations. By utilizing **Wafer’s Agent** to identify inefficiencies and apply appropriate fixes, we were able to move beyond marginal gains toward order-of-magnitude improvements that change how these models are used in production.*


- **Kimi 2.5 (High-Speed Single Stream)**


- On a standard 10k input / 1.5k output workload, a stock configuration on 8x MI350X/MI355x hardware delivered a baseline of **22.5 tok/s** . Through deep kernel optimization and a customized inference framework, we increased this to **255.2 tok/s** - representing an **11.33x speedup** with zero trade-offs in accuracy.


- **DeepSeek V3.2 (Full-Stack Scaling)**


- While stock frameworks achieved **38.5 tok/s** for single-request output speed, our optimized stack pushed this to **200.8 tok/s** . More importantly, at a concurrency of 64, we saw a **7.32x improvement** in per-request output speed and boosted aggregate throughput from **548 tok/s** to **2,165 tok/s** .


- **GLM-5 (Flagship Efficiency)**


- GLM-5 is a massive 774B parameter flagship model. By optimizing the deployment topology and specializing the decode path, we enabled a single 8-GPU MI350X node to serve this model with a mean throughput of **151.1 tok/s** and an inter-token latency (ITL) of just **17.8 ms** .


## The Economic Thesis


Beyond the technical achievement, these results represent a fundamental shift in the economics of frontier inference. Our work demonstrates that fully optimized AMD infrastructure can achieve elite performance levels while remaining more cost-effective than traditional flagship hardware deployments.


The takeaway is straightforward: inference performance is increasingly a systems problem. Delivering both high performance and sustainable economics requires a deep, custom approach to the software stack that maximizes every cycle of the underlying silicon.


***** *Performance results are based on internal testing using the configurations described. Results in customer environments may vary depending on hardware, specific workload characteristics, implementation, and utilization.*


## Why “Out-of-the-Box” Software Leaves Performance on the Table


In our research, we define “stock” frameworks as unmodified, out-of-the-box versions of inference engines or standard kernel libraries. While these tools are the fastest way to get a model running, they carry several architectural taxes that can hinder frontier model performance.


- **The Generality Trade-off**


- Stock kernels are generically written to support many model shapes, which often leaves them unoptimized for the specific dimensions of frontier architectures.


- **Prefill Bias**


- Standard kernels are sized for large prefill batches and carry large-scale tile dimensions, register pressure and launch machinery calibrated for that regime. At single-stream decode, this is a fundamental mismatch—workload is memory-bandwidth-bound, yet the kernel continues to schedule compute resources at prefill scale, leaving the bulk of matrix cores idle.


- **The “Launch Tax”**


- Stock setups often dispatch operations like all-reduce, residual add, and RMSNorm as three separate kernels. This leads to microsecond-level administrative overhead for each call and forces unnecessary data “round-trips” to High Bandwidth Memory (HBM).


- **Rigid Software Constraints**


- Standard libraries frequently contain hard-coded assertions—such as requiring head counts to be multiples of 16 - that can cause immediate incompatibilities with the unique configurations of new frontier models.


## Problem Primer: Defining the Levers of High-Speed Inference


To understand how these gains were achieved, we must understand the systems-level concepts that govern frontier model performance. Inference engineering at its core is about mastering the interactions between hardware execution, memory hierarchy, the software dispatch layer, and knowing precisely where each lever sits.


### MXFP4 (Microscaling Formats)


MXFP4 is an open-standard 4-bit floating point format jointly developed by AMD, NVIDIA, Microsoft, and others under the OCP Microscaling specification[\[1\]](https://www.opencompute.org/documents/ocp-microscaling-formats-mx-v1-0-spec-final-pdf) . Unlike per-tensor or per-channel quantization schemes, MXFP4 operates at the *block* level: a group of 16 or 32 values shares a single 8-bit scaling exponent, giving an effective storage cost of approximately 4.25 bits per weight rather than a clean 4.0.


This shared-exponent design is the key insight - it preserves the dynamic range needed for numerically sensitive operations like expert routing and attention projection, while still achieving the memory footprint of a 4-bit format.


- **Compression**


- BF16 costs 16 bits per weight. MXFP4 at 4.25 bits is a ~3.8× reduction. For a model with hundreds of billions of parameters distributed across routed expert FFN layers, this is the difference between requiring multi-node serving and fitting comfortably on a single 8×GPU node. For GLM-5 at 774B parameters, the majority of parameters reside in expert weights that are sparsely activated - MXFP4 compresses precisely the weights that are memory-resident but rarely hot, making this an exceptionally well-targeted optimization.


- **KV Cache Headroom**


- The compression benefit is not purely about model weights. By reducing the static weight footprint, the GPU’s HBM budget is freed for a larger KV cache allocation. This directly improves throughput on long-context requests, where KV cache eviction is otherwise the binding constraint.


### MLA (Multi-Head Latent Attention)


Standard Multi-Head Attention (MHA) caches full K and V tensors for every layer, every head, and every token in the sequence. At long context lengths with large batch sizes, this KV cache becomes the dominant consumer of HBM - often exceeding the model weights themselves. MLA, introduced in DeepSeek-V2[\[2\]](https://arxiv.org/abs/2405.04434) and carried forward through DeepSeek-V3, Kimi K2.5, and others, addresses this by changing what is stored.


- **Low-Rank Compression**


- Rather than caching the full K and V matrices, MLA projects the attention input down to a low-rank latent vector **` c_KV`** of dimension **` d_c << d_model`** . At inference time, the full K and V heads are *reconstructed* from this latent vector on-the-fly via learned up-projection matrices. The KV cache now stores only **` c_KV`** per token which is a reduction of roughly 5 to 13x depending on the model’s head configuration at the cost of additional GEMM operations during decode.


- **Decoupled RoPE**


- MLA pairs the compressed KV cache with a decoupled rotary positional embedding scheme. Standard RoPE is applied to a separate **` d_r`** -dimensional key component, which is cached alongside **` c_KV`** . This avoids the numerical issue of applying position-dependent transformations to vectors that will later be linearly projected, preserving attention correctness.


- **The Kernel Challenge**


- The reconstruction path introduces a non-trivial compute pattern. During decode, each step must: (1) load **` c_KV`** from cache, (2) apply the up-projection GEMM to materialize K and V, (3) run attention across all cached latent vectors. This cannot be naively fused into a standard FlashAttention kernel. Efficient MLA execution requires a fused kernel that absorbs the up-projection into the attention computation itself — merging what would otherwise be a separate GEMM + attention dispatch into a single pass. Without this fusion, the projection GEMMs are too small to be compute-efficient at batch-1, and latency suffers significantly[\[3\]](https://arxiv.org/abs/2506.02523) .


### MoE (Mixture of Experts)


A standard dense transformer activates every parameter for every token. An MoE layer replaces the FFN block with a collection of parallel expert FFNs and a learned router[\[4\]](https://arxiv.org/abs/1701.06538) . For each token, the router computes a softmax over all experts and selects the top-K highest-scoring ones. Only those K experts execute; the rest remain dormant[\[5\]](https://arxiv.org/abs/1701.06538) . The result is a model with a very large *parameter count* but a modest *activated parameter count* per token—GLM-5 has 774B total parameters but activates roughly 50–60B per token[\[6\]](https://arxiv.org/abs/2602.15763) .


- **Routing Mechanics**


- In GLM-5’s 256-expert configuration with top-K routing, each token activates a fixed number of experts (typically K=8 or K=16 depending on the layer). The router output is a sparse selection vector; the token embedding is then dispatched to the selected expert FFNs, processed, and the outputs are weighted and summed. At scale across a batch, this produces a highly irregular all-to-all communication pattern — tokens from the same batch are routed to different experts, potentially on different GPUs in a tensor-parallel deployment.


- **Expert Parallelism**


- In multi-GPU serving, expert FFN weights are sharded across devices. Each GPU hosts a subset of experts. The routing step triggers an all-to-all dispatch where tokens are physically moved to the GPU that holds their assigned expert, processed, and returned. This all-to-all is the dominant latency bottleneck in MoE serving and does not diminish with tensor parallelism — it is irreducible given the routing structure.


- **The Empty Work Problem**


- Stock MoE kernels are written for batched operation. They sort and permute tokens into expert-ordered buffers before dispatching to expert FFNs, then reverse-permute the outputs. At batch-1 single-stream decode, most experts receive zero tokens. The kernel still executes the sort, allocates the permutation buffers, and iterates over the expert list — pure overhead on empty bins. Well-engineered MoE kernels for decode add an occupancy check to skip empty experts entirely, converting O(num_experts) overhead to O(K) where K is the active expert count.


### Kernel Fusion


Every GPU kernel launch carries a fixed administrative cost. The driver must validate arguments, schedule the kernel onto a Streaming Multiprocessor and synchronize before the next kernel can begin. On ROCm/HIP, this overhead is approximately 2 to 5µs per launch and for a transformer layer executing at batch-1, the total compute time for a small operation like RMSNorm may itself be only 5 to 10µs. This means launch overhead is the same order of magnitude as the useful work.


- **The Round-Trip Problem:**


- Beyond launch overhead, unfused kernels force intermediate results to be written to HBM and read back.
- After an all-reduce across TP ranks, the sequence is-


- Reduced result is written to global memory
- Residual add reads it back
- RMSNorm result is written to global memory again
- The next operation reads it back again


- Each round-trip traverses HBM at 6 TB/s — but even at that bandwidth, repeated materialization of intermediate tensors adds latency proportional to **tensor size × number of round-trips**
- For a hidden dimension of 7168 (DeepSeek-V3/Kimi K2.5) at BF16 across a batch, this is measurable at decode latency timescales.


- **Fused Kernels in Practice**


- A fused` all_reduce + residual_add + rms_norm` kernel keeps the reduced tensor in registers or L1/L2 cache throughout[\[7\]](https://arxiv.org/abs/2508.07071) . The all-reduce result never touches HBM — it flows directly into the add and normalization logic within the same warp execution. AMD’s AITER library provides` fused_add_rms_norm` as a primitive that targets exactly this pattern; additional fusion opportunities include fused QKV projection + rotary embedding and fused gating + activation in MoE FFN layers.


- **CUDA Graph Capture**


- An orthogonal but complementary approach is CUDA/HIP graph capture, which records the full sequence of kernel launches for a decode step and replays them as a single driver submission, eliminating per-kernel launch overhead entirely. This is particularly effective at batch-1 where the graph structure is static and the individual kernels are short.


### Speculative Decoding


Autoregressive decode is inherently serial: each token depends on all previous tokens, and the full model must execute once per generated token. The wall-clock cost per token is dominated by the memory bandwidth required to load all activated weights at batch-1, this is a pure bandwidth-bound problem regardless of how fast the matrix cores are[\[8\]](https://blog.google/innovation-and-ai/technology/developers-tools/multi-token-prediction-gemma-4/) . Speculative decoding breaks this serialization by parallelizing verification[\[9\]](https://arxiv.org/abs/2412.19437) .


- **The Drafter-Verifier Contract**


- A small, fast draft model (or a lightweight MTP head attached to the main model) proposes a sequence of K candidate tokens in K fast forward passes. The large target model then processes all K candidates simultaneously in a *single* forward pass, verifying each against its own distribution using a rejection sampling criterion. If the drafter’s token matches the target’s distribution, it is accepted at zero additional cost. If it diverges, the sequence is truncated at the first mismatch and the target’s correction is used.


- **MTP (Multi-Token Prediction) Heads**


- Rather than a separate draft model, MTP attaches parallel prediction heads directly to the main model’s hidden states. Each head predicts the token N steps ahead. This eliminates the KV cache mismatch and memory overhead of maintaining a separate draft model and keeps the drafter tightly coupled to the target’s representations, improving acceptance rates[\[10\]](https://sebastianraschka.com/llm-architecture-gallery/mtp/) .


- **Acceptance Rate Sensitivity**


- The 1.6x throughput figure is highly conditional. It assumes an acceptance rate above roughly 80% on typical user prompts. For code completion or highly predictable continuations, acceptance rates can reach 90%+ and the speedup exceeds 2x. For open-ended generation with high entropy, acceptance rates drop to 50–60% and the overhead of running the drafter erodes the gain. In production, speculative decoding should be gated on request type or dynamically disabled when measured acceptance rate falls below a threshold.


### Tensor Parallelism (TP)


Tensor Parallelism distributes the weight matrices of each layer across multiple GPUs. For a linear projection` Y = XW` , the weight matrix` W` is column-split across N GPUs; each GPU computes a partial result, and an all-reduce synchronizes the outputs before the next layer[\[11\]](https://www.digitalocean.com/blog/llm-inference-tradeoffs) . This allows a model that would not fit on a single GPU to be served across a node, and reduces the per-GPU memory requirement proportionally.


- **The All-Reduce Cost**


- Every layer boundary requires an all-reduce across all TP ranks. On an 8-GPU node over NVLink or AMD Infinity Fabric, a single all-reduce for a hidden dimension of 7168 at BF16 costs approximately 5 to 15µs depending on utilization and collective implementation. A 60-layer model executing at TP=8 therefore incurs 60 all-reduces per decode step — on the order of 300 to 900µs of pure synchronization overhead per token, independent of compute. This is the irreducible “TP tax.”


- **The TP=4 × 2 Replica Insight**


- The all-reduce cost scales with the number of participating ranks. At TP=4, each all-reduce involves 4 GPUs instead of 8 - roughly halving synchronization latency. If the model fits within the memory budget of 4 GPUs (feasible for 70 to 130B parameter models with aggressive quantization), running two independent TP=4 replicas on a single 8-GPU node doubles request throughput while maintaining lower per-token latency than a single TP=8 instance. This is not a universally applicable strategy - for 400B+ models that require TP=8 to fit, there is no choice - but it is a significant architectural decision for mid-size frontier models[\[12\]](https://arxiv.org/abs/2503.06433) .


- **TP vs. Pipeline Parallelism**


- For very large models that exceed the memory capacity of a single node, pipeline parallelism (PP) partitions layers across nodes. PP introduces bubble overhead from the pipeline fill/drain cycle but avoids the all-reduce cost of TP. In single-node inference, TP is almost always preferred; PP becomes relevant only when the model cannot fit on one node even with quantization.


## Reclaiming the Hardware Potential


The performance gaps we’ve identified - “Launch Tax”, prefill bias and rigid software constraints are not limitations of the underlying silicon. Rather, they are symptoms of a software ecosystem that has prioritized generality over peak efficiency.


By identifying these bottlenecks and mastering the “levers” of the modern inference stack from **MXFP4 quantization** to **custom kernel fusion** - our team has shown that it is possible to achieve significant performance gains on high-performance AMD infrastructure. These optimizations don’t just result in faster tokens, they rewrite the economic reality of hosting frontier models at scale.


### The Roadmap Ahead


This is only the beginning of our deep dive into inference engineering. In the coming weeks, we will release three technical “surgeries,” each focusing on a different frontier model and the specific optimizations used to unlock its potential:


- **Part 2: The Kimi 2.5 Deep-Dive**


- How we achieved an 11x speedup by bypassing kernel incompatibilities and engineering custom MLA and MoE kernels.


- **Part 3: Scaling DeepSeek V3.2**


- A look at full-stack serving optimizations, FP8 KV cache support, and high-concurrency throughput gains.


- **Part 4: Optimizing the 774B GLM-5**


- A breakdown of TP=4 deployment topologies, specialized batched GEMV kernels and fine-tuning speculative decoding for maximum efficiency.


Stay tuned as we move from the high-level anatomy of these bottlenecks to the low-level code that helps solve them. Keep in mind that results will depend on your specific configuration, hardware, and usage patterns.
