---
schema_version: "1.0.0"
document_id: "4e3dd382e939dff7b4c8455d8f5e7653e343674db61681daa90fb8e73be3b379"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/cutting-memory-in-long-context-rl-with-fused-logprobs"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:0cd0cb2447fa3a0433bf2cbf99d87f48cb553b9c68b4542062d0281ac55f4415"
---

# Cutting Memory in Long-Context RL with Fused Logprobs

**TL;DR: We built a fused logprob kernel that reduces RL training time by 20% and increases maximum trainable context length by 33% (12K → 16K) on Qwen3.5-122B-A10B (single 8xH200 node + Miles).**


## Overview


Long-context RL training has become increasingly popular as open source models have continued improving, and companies have adopted more agentic use cases that require multi-turn tool use. There are a few main components that lead to training inefficiencies in long-context scenarios: attention, activation memory across the transformer stack, and per-token logprob computation at the LM head (i.e. the final linear layer that projects hidden states into vocabulary-sized logits).


There have been multiple solutions that combat the first two. FlashAttention computes attention in tiled blocks so the \[L, L\] score matrix doesn't need to allocate. This, along with other optimizations, turns attention's scaling from quadratic to linear in context length. And activation memory can be addressed with gradient checkpointing (i.e. recompute some activations instead of storing them) and Megatron-style sequence parallelism (i.e. split a sequence across GPUs).


On the other hand, there's been far less focus towards logprob computation. But it's becoming the bottleneck at the training scale we care about: i.e. long-context training on medium-to-large size models. Specifically, the memory size of the logits tensor (i.e. the array of logits - output scores from the linear layer - being generated) grows significantly at long-context. For Qwen3.5-122B-A10B with a 248K vocabulary, the \[L, V\] (tokens * vocabulary entry) logits tensor immediately before loss is ~7.8 GB per tensor-parallel (TP) rank at a context size of 16K.


Our observation is that GRPO (and GRPO-style policies, like GSPO) doesn't actually require the full \[L, V\] logits tensor. This is possible due to two properties of the LM head computation. First, the loss only requires a single scalar per token (i.e. the selected token's logprob), so we can reduce the vocabulary dimension on the fly via a log-sum-exp and a gather rather than needing to fully materialize the full \[L, V\] logits tensor. Second, each token's logprob is computed row-wise, independent of every other token's logprob.


These two properties combined enable us to process the sequence dimension in chunks, which means we never need to store the entire ~7.8 GB tile in memory. Instead, we can stream chunks of the sequence dimension, and collapse the vocabulary dimension in each one.


## Implementation


### Baseline Chunking


We used[Miles](https://github.com/radixark/miles) as our starting point for the kernel, which was convenient as Miles already chunks the[sequence dimension](https://github.com/radixark/miles/pull/1068) . Miles' chunked-TP logprob path processes the loss in fixed-size sequence chunks. For each chunk of N tokens, it:


- Gathers hidden states across sequence-parallel ranks (if needed)
- Projects hidden states to logits via the TP LM head
- Computes the log-softmax
- Gathers the selected-token scores
- Discards the logits before proceeding to the next chunk


Chunking alone drops peak logits memory from O(L * V) to O(N * V). This ensures that the maximum context length is no longer directly bottlenecked by head memory.


The chunking introduces a trade-off: reducing chunk size will continue reducing peak memory, but it also multiplies the per-chunk overhead (i.e. the Python loop and op-dispatch cost, separate matmul / log-softmax / gather kernels per chunk, and a TP all-reduce per chunk). At 12K context,` chunk_size=128` requires 96 loop iterations while` chunk_size=512` requires 24 loop iterations.


We ablated using Qwen3.5-35B-A3B with GRPO on[DAPO-Math-17K](https://huggingface.co/datasets/BytedTsinghua-SIA/DAPO-Math-17k) with 12K context size and found` chunk_size=256` to be the sweet spot. It was small enough to reduce memory usage at long context, while being large enough that per-chunk overhead didn't negate the performance gains.


Metric Baseline chunked_seq256 Reduction


log_probs_time 22.45 s 17.11 s -23.8%


actor_train_time 32.59 s 21.14 s -35.1%


train_time 55.25 s 38.52 s -30.3%


step_time 179.29 s 134.46 s -25.0%


peak_gb 49.30 GB 41.16 GB -16.5%


Our sweep across multiple chunk sizes revealed that the chunking itself wasn't the bottleneck, but more specifically, it was the per-chunk cost. Reducing the per-chunk cost makes smaller chunk sizes viable, and smaller chunk sizes unlock more context per node.


### Fused Triton Kernel


Our fused kernel targets per-chunk cost in three ways:


1. Eliminates the \[N, V_local\] logits tile
2. Collapses per-chunk kernel launches into a single Triton pass
3. Shrinks the TP all-reduce traffic by ~200MB per TP rank


For the baseline, each chunk launches separate kernels for the matmul (\\(h \\, W^\\top\\)), log-softmax, and selected-token gather processes - along with a TP all-reduce. The matmul materializes an \[N, V_local\] logits tile in HBM (high bandwidth memory), and the subsequent kernels then read it back.


In contrast, our fused kernel combines the matmul, a streaming log-softmax, and the selected-token gather into a single pass. The \[N, V_local\] tile lives only in the on-chip SRAM, and is consumed by the same kernel that produces it.


TP semantics are preserved exactly, with per-token scalar reductions rather than tensor-sized ones. For each token row \\(t\\) with hidden state \\(h_t\\) and target id \\(y_t\\):


- Local logits on each TP rank:


- \\(z_{\\text{local}} = h_t \\, W_{\\text{local}}^\\top + b_{\\text{local}}\\)


- Global row max:


- \\(m_t = \\mathrm{AllReduce}_{\\max}(\\max(z_{\\text{local}}))\\)


- Global log-sum-exp denominator:


- \\(d_t = \\mathrm{AllReduce}_{\\sum}\\!\\bigl(\\sum \\exp(z_{\\text{local}} - m_t)\\bigr)\\)


- Selected target logit (only the owner rank contributes a non-zero):


- \\(z_{\\text{local},\\,t,\\,y_t} = \\mathrm{AllReduce}_{\\sum}(z_{\\text{local},\\,t,\\,y_t})\\)


- Final logprob:


- \\(\\log p(y_t) = z_{t,\\,y_t} - (m_t + \\log d_t)\\)


Mathematically, this computes the same selected-token logprob as the full vocabulary path. It decreases the per-chunk all-reduce traffic from \[N, V_local\] (up to gigabytes) to just three \[N\] vectors (kilobytes), and also decreases per-chunk kernel launches from four to one.


These changes combat the per-chunk overhead, allowing us to drive down chunk size and unlock longer context lengths & more memory headroom for training.


### LoRA Backward Pass Fix


While the fused kernel targets the forward pass, we found a related opportunity to improve efficiency in the backward pass. We use[LoRA](https://thinkingmachines.ai/blog/lora/) in our training infrastructure and noticed in the baseline that every chunk would:


- Allocate \\(\\nabla h\\), \\(\\nabla W\\), \\(\\nabla b\\)
- Recompute logits over vocab tiles
- Accumulate into \\(\\nabla h\\), \\(\\nabla W\\), \\(\\nabla b\\)


We fixed this by having the fused kernel respect[ctx.needs_input_grad](https://github.com/radixark/miles/pull/1068) , so it only computes the gradients that autograd actually asks for:


- Compute \\(\\nabla h\\) only if hidden needs grad
- Compute \\(\\nabla W\\) only if weights need grad
- Compute \\(\\nabla b\\) only if biases need grad
- Only perform TP all-reduce for hidden if \\(\\nabla h\\) exists


We leave the core gradient unchanged (with temperature \\(T\\)):


\\\[\\nabla z = \\nabla\\ell \\cdot \\bigl(\\text{softmax}(z) - \\text{onehot}(y)\\bigr) \\cdot \\frac{1}{T}\\\]


And the chain rule gives:


\\\[\\nabla h \\mathrel{+}= \\nabla z \\; W_{\\text{tile}}\\\]


\\\[\\nabla W \\mathrel{+}= (\\nabla z)^\\top \\; h\\\]


\\\[\\nabla b \\mathrel{+}= \\sum \\nabla z\\\]


With these changes, each accumulation only runs if` needs_input_grad` is` True` for that input. With a frozen output layer, we can skip \\(\\nabla W\\) & \\(\\nabla b\\) completely. The TP all-reduce for \\(\\nabla h\\) is also skipped if the hidden gradient isn't requested. For our LoRA RL use case (our default for the Osmosis training platform), this streamlines the backward pass through the head so the kernel only performs the work required by the active parameter set.


## Results


We set the following parameters in our training experiments to ensure that our results would be representative of actual training costs:


- We enabled entropy with a nonzero entropy coefficient, which forces the loss to compute and backpropagate through entropy. As a result, we can measure the full logprob + entropy path rather than a 'cheaper' logprob-only path.
- We used the normal loss function for the actor. Specifically,` policy_loss_function` calls` get_log_probs_and_entropy(..., with_entropy=True)` - with chunking and fusion enabled, this skips a redundant forward pass by reusing cached hidden states. From there, it runs one of two kernels: either a chunked output-layer pass, or a fused kernel (i.e. to go straight from hidden states to a token's entropy and logprob).
- We used random weights and disabled stopping (i.e.` ignore_eos=True` with no stop tokens) to ensure generations always filled the context window for maximum context stress testing.


We found that on Qwen3.5-35B-A3B with 12K context, chunking reduced step time by 22.0% and peak memory by 16.2%.


Then with Qwen3.5-122B-A10B at 12K context, we found that chunking cut logprob time by 59.1%, training time by 20.0%, and reduced peak memory by ~10 GB.


On top of chunking, we observed that the fused logprob kernel reduced step time by a further 8.7% (fused 128 compared to chunked 256), which was in large part driven by a 40.5% reduction in logprob time due to the fused logprob kernel compared to chunking alone.


With chunking and the fused logprob kernel, we were also able to increase the total context length supported on a single node from 12K to 16K.


For Qwen3.5-122B-A10B at 16K context length on a single node we observe the following behavior from each set up, considering each GPU of an H200 has 141 GB of RAM:


Configuration Peak Memory (GB) Memory Overflow (GB)


Baseline 150.00 +9.00


Fused 128 136.13 -4.87


Each H200 has 141 GB of RAM, which meant the baseline training configuration with Qwen3.5-122B-A10B at 16K context length would encounter out-of-memory (OOM) errors, while our fused 128 configuration did not.


## Next Steps


We're continuing to expand the corpus of training-specific kernels that we use for our post-training platform. We also plan to continue evaluating & iterating on the fused kernel - e.g. testing it with other model families (Gemma 4), more RL algorithms (DAPO, PPO, etc.), and greater context lengths (100K+ tokens).


At Osmosis, we enable developers to build task-specific models with RL that beat foundation models. If you're exploring post-training,[reach out](https://osmosis.ai/cdn-cgi/l/email-protection#6f040e1c0a162f001c02001c061c410e06) for access to our research preview.
