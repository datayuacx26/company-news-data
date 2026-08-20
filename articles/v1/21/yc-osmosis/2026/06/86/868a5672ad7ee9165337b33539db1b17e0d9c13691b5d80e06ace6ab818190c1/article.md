---
schema_version: "1.0.0"
document_id: "868a5672ad7ee9165337b33539db1b17e0d9c13691b5d80e06ace6ab818190c1"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/training-thousands-of-lora-adapters-at-once"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:a3769c4b15c5726ac2135383672a55b57569539bc1904a910b841db8762da445"
---

# Training Thousands of LoRA Adapters at Once

**TL;DR: We extended[Miles](https://github.com/radixark/miles) with a multi-adapter LoRA training path that lets us train thousands of LoRA adapters concurrently and asynchronously. The core change is a modification to Megatron-Bridge + Miles which allows us to load multiple LoRA adapters as a single matrix. On our Qwen3.6-35B-A3B + GSM8K stress test, we ran 1,536 LoRA adapter instances concurrently with step time under 3 minutes.**


## Overview


LoRA-based post-training decomposes a model into two components: a shared base model and a lightweight low-rank adapter that captures task-specific updates. This setup works well when training a single policy, since only the adapter parameters need to be optimized. However, it underutilizes resources when many policies are fine-tuned in parallel. Each LoRA training run still requires deployment of the full base model alongside its adapter, causing the same underlying model weights to be replicated across concurrent runs. As the number of policies scales, this duplication introduces substantial wasted VRAM.


What if we could share the same base model between policies, and just fine-tune different LoRA adapters in a single batch? This is cleaner and improves scalability: we can keep one base model, route tokens to different LoRA adapters, and have the training/inference stack treat LoRA adapters as cheap concurrent policies rather than separate model replicas.


We built on our prior work of[supporting LoRA RL for the Qwen3.5 model family](https://osmosis.ai/blogs/unlocking-lora-moe-rl-for-qwen3-5) , with the goal of extending our training stack from "one LoRA policy" to "many LoRA policies, in the same training step."


The motivation is simple: base models are large, LoRA adapters are small. If we want to run thousands of RL experiments (i.e. prompt/harness, reward design, and curriculum ablations), we can't replicate the full base model for every individual training run.


We built our multi-LoRA framework on top of[Miles](https://github.com/radixark/miles) , RadixArk's continuously evolving open source RL post-training framework. Miles already provides us the pieces we require for large scale RL, such as:


- Megatron-based training with support for flexible modifications
- SGLang-based rollout with support for scaling to thousands of LoRA adapters
- Unified FP8 training support


We added multi-LoRA training by:


- Deploying one shared Qwen3.6-35B-A3B base model
- Supporting multiple LoRA adapter slots in Megatron-Bridge
- Implementing multi-LoRA rollouts and training with Miles
- Online loading and unloading adapters without restarting the RL trainer
- Serving multiple LoRA adapters using SGLang's native multi-LoRA interface
- Keeping experts adapter-free to unlock additional memory savings


### LoRA with GRPO


GRPO-style RL objectives already separate the trainable policy from a frozen reference model. In the[DeepSeekMath/GRPO](https://arxiv.org/abs/2402.03300) objective, the policy update is regularized by a KL term against \\(\\pi_{\\text{ref}}\\)


:


\\\[\\mathcal{J}_{\\text{GRPO}}(\\theta) = \\mathbb{E} \\left\[ \\frac{1}{G} \\sum_{i=1}^{G} \\frac{1}{|o_i|} \\sum_{t=1}^{|o_i|} \\left( \\min(\\cdots) - \\beta D_{\\text{KL}} \\left\[ \\pi_\\theta \\Vert \\pi_{\\text{ref}} \\right\] \\right) \\right\]\\\]


For LoRA policy \\(a\\)


, the model parameters are:


\\\[\\theta_a = \\theta_0 + \\Delta_a\\\]


where \\(\\theta_0\\)


is the shared base model and \\(\\Delta_a\\)


is the adapter-specific LoRA delta. If every LoRA adapter starts from the same base model, then the correct frozen reference for every one is the same model:


\\\[\\pi_{\\text{ref}, a} = \\pi_{\\theta_0}\\\]


The policy logprob depends on the adapter:


\\\[\\log \\pi_a(y_t \\mid s_t) = \\log \\pi_{\\theta_0 + \\Delta_a}(y_t \\mid s_t)\\\]


But the reference logprob does not:


\\\[\\log \\pi_{\\text{ref}}(y_t \\mid s_t) = \\log \\pi_{\\theta_0}(y_t \\mid s_t)\\\]


Using the common sampled KL estimator from GRPO implementations, define:


\\\[r_{a,t} = \\frac{ \\pi_{\\text{ref}}(y_t \\mid s_t) }{ \\pi_a(y_t \\mid s_t) } = \\exp\\left( \\log \\pi_{\\text{ref}}(y_t \\mid s_t) - \\log \\pi_a(y_t \\mid s_t) \\right)\\\]


Then the token-level KL penalty is:


\\\[\\widehat{D}_{\\text{KL}, a,t} = r_{a,t} - \\log r_{a,t} - 1\\\]


Only the policy logprob changes with adapter \\(a\\)


. The reference logprob is shared across all adapters for the same token state. Therefore, one frozen reference model can score the packed batch once, and each adapter's policy loss can reuse those reference logprobs.


Crucially, this policy only holds when all adapters share the same reference checkpoint. If adapter slots are warm-started from different base models or finetunes, this theory breaks down and we are forced to use multiple reference models.


## Multi-LoRA Implementation


### Multiple Adapters in Megatron-Bridge


We began by adding a new multi-LoRA transform within Megatron-Bridge's PEFT module. The new transform,` MultiLoRA` , behaves similar to a normal PEFT transform by attaching the LoRA weights and freezing the layer. However, instead of wrapping a target module with one adapter, it wraps it with N adapters, which we refer to as "slots."


The` MultiLoRA` transform targets the most transformer linear modules (` linear_qkv` ,` linear_proj` ,` linear_fc1` , and` linear_fc2` ) but deliberately skips MoE expert linear and router layers.


> The expert layer limitation is caused by the nature of MoE layers and routing. While training with multiple LoRAs, we keep sequences grouped by LoRA within a batch. However, when we encounter an expert activation, we must break this grouping to route the tokens to its expert and then reconstruct it for future layers. The reconstruction of the grouped sequences requires extra patching and bookkeeping in Megatron-Bridge that we plan to work on in the future.


This keeps the first version focused on dense transformer projections, where LoRA has the cleanest tensor-parallel semantics.


At each matched module, we replace the base linear with a` MultiLoRALinear` . Internally, each wrapped layer owns:


- The original Megatron parallel linear
- An` nn.ModuleList` of LoRA adapter slots
- Per-slot rank and alpha metadata
- A` tokens_per_adapter_slot` routing tensor
- Compatibility helpers for exposing a single slot as` .adapter` for checkpoint export/load


The key idea is that sequences generated from an adapter must be processed by the same during training. Before the forward pass, we can pre-process the incoming micro-batch by grouping by adapter slot, and then sorting it by slot. Then, each` MultiLoRALinear` receives a` tokens_per_adapter_slot` vector indicating how many contiguous tokens belong to each slot:


```text
x   = [slot0 tokens | slot1 tokens | slot2 tokens | ... | slotN tokens ]
tokens_per_adapter_slot   = [n0, n1, n2, ..., nN]


```


Since the tokens are grouped together with their offsets known, we can use a grouped matrix multiply for both the LoRA A and LoRA B matrices. Our modified forward pass looks like this:


### Rank Masking and Slot Lifecycle


Our multi-LoRA trainer also supports training on different ranks up to a maximum rank (R) which is set at initialization time of` MultiLoRA` . For ranks that are lower than the maximum, we zero out the unused rows and columns. This simple implementation plays well with other moving parts of the framework such as the optimizer, but likely has room for further optimization.


Slot lifecycle is controlled with small model-wide helpers:


- ` init_adapter_slot(model, idx, rank, alpha)`
- ` clear_adapter_slot(model, idx)`
- ` load_adapter(model, idx, state_dict)`
- ` set_tokens_per_adapter_slot(model, tokens_per_adapter)`


The goal of the slot lifecycle helpers is to provide slot orchestration as a user API while keeping the layer responsible for the local invariants.


### Grouped GEMM Forward


The base model runs normally for each target linear. We add the LoRA path on top:


\\\[y = W x + \\Delta_a x\\\]


Where each LoRA adapter still computes:


\\\[\\Delta_a = \\frac{\\alpha_a}{r_a} B_a A_a\\\]


A naive way to compute the per adapter output is to iterate over each adapter:


```text
for   adapter  in   adapters:
out[adapter_tokens] = B_a(A_a(x[adapter_tokens]))


```


However, we can't process the LoRA adapters sequentially, as invoking kernel launches for small GEMMs leads to poor scalability.


Instead,` MultiLoRALinear` stacks the raw LoRA weights:


```text
stacked_A = torch.stack([
a.linear_in.weight
for   a  in   adapters
])
stacked_B = torch.stack([
a.linear_out.weight
for   a  in   adapters
])


```


And uses` torch._grouped_mm` with offsets derived from` tokens_per_adapter_slot.cumsum()` , like so:


\\\[m_a = x_a A_a^\\top\\\]


\\\[o_a = m_a B_a^\\top\\\]


That being said, all adapters are processed as one grouped operation over packed token ranges.


The tensor-parallel and sequence-parallel collectives are issued once for the grouped GEMMs, matching the base linear layout:


- Column-parallel layers gather sequence-parallel input before LoRA matmul
- Row-parallel layers reduce partial LoRA hidden states between A and B
- Output layout is gathered or scattered to match the wrapped Megatron linear


This matters because we need to preserve Megatron's sharding semantics in order to properly use multi-LoRA. The wrapper should be invisible to the rest of the model: the caller still sees the same (linear_output, bias) structure.


### Checkpoint Compatibility and Weight Sync


One awkward systems detail is that existing Megatron-Bridge LoRA parameter export paths expect a single adapter named` .adapter` , while our multi-adapter layer stores multiple adapters under` .adapters` . Miles uses Megatron-Bridge's export functionality to access the weights for saving checkpoints and weight sync.


Rather than implement our own export logic, we added a context manager` expose_adapter_slot(model, idx)` that temporarily exposes a target slot as` .adapter` . While not the most performant approach, it allows us to reuse the existing save, load, and export code paths for now, simplifying implementation.


```text
module  .adapters[idx] ->  module  .adapter


```


We also added` hide_adapters(model)` for base checkpoint loading, so Megatron-Bridge does not try to map adapter parameters to Hugging Face base model weights.


### Inference Path


[SGLang's LoRA serving](https://docs.sglang.ai/advanced_features/lora.html) design already allows for optimized multi-LoRA serving by implementing concepts from[Punica](https://arxiv.org/abs/2310.18547) and[S-LoRA](https://arxiv.org/abs/2311.03285) . Punica batches multi-tenant LoRA requests against a shared base model using segmented matrix-vector kernels. S-LoRA extends this with unified paging for adapter weights and KV cache, along with tensor-parallel strategies that enables scaling to thousands of adapters with low overhead.


For the most part, multi-LoRA serving works out of the box for RL post-training. SGLang already supports:


- Multiple adapters in the same batch
- Adapters per batch using` max_loras_per_batch`
- CPU-side loaded adapter limits with` enable_lora_overlap_loading`
- Both` triton` and` csgmv` LoRA back-ends


So the inference-side work became mostly scaling and integration by:


- Running more SGLang replicas
- Sharding LoRA adapters across replicas
- Routing rollout requests to the replica that owns the LoRA adapter
- Keeping replica size at four GPUs per inference engine
- Keeping trainer size at four GPUs per trainer
- SGLang flag golf (mtp,` --enable-chunked-prefill` ,` --enable-mixed-chunk` , etc)


### Challenges


The first challenge was lining up Megatron tensor-parallel semantics cleanly. LoRA is simple in single-GPU PyTorch, but Megatron linears are not all the same. Column-parallel and row-parallel layers need different collectives between the A and B projections. Sequence parallelism also changes whether the LoRA path sees full sequence tokens or a shard. The wrapper has to match the base linear exactly, or the residual addition becomes shape-correct but semantically wrong.


The second challenge was checkpoint compatibility. By default, Megatron-Bridge will access the` .adapter` field of the module for exporting weights. Our multi-LoRA implementation contains a list of adapters indexed by` .adapters\[idx\]` . To avoid having to write a custom exporter for Megatron-Bridge, we implemented a simple Python context manager that would hide the` .adapters` field and expose an individual adapter as` .adapter` , which let us reuse the built-in export implementation in Megatron-Bridge.


The final challenge was deciding what we shouldn't build. Punica and S-LoRA already solved a large part of the multi-LoRA adapter inference problem. Our leverage was higher on the training side: i.e. supporting the ability to train many LoRA adapters concurrently on Megatron, then using SGLang's existing multi-LoRA serving design at a larger replica count.


### Results


To stress test our implementation, we fine-tuned Qwen3.6-35B-A3B on GSM8K with 128 adapters on a single H200 node. We also tested up to 1,536 concurrent LoRA adapter instances (12 replicas * 128 adapters) live in our cluster. Each training step time was under 3 minutes, with inference taking ~2 minutes and training taking ~1 minute.


Our reference model and policy model show minimal KL divergence in this stress test. That is consistent with the structure of the experiment: every policy is a small LoRA delta off the same base model, and the shared reference remains the zero-adapter/base policy.


While small, a batch size of 2 and 4 samples per adapter does work well for seeing how many adapters we can fit into a single node.


Another test with 32 adapters with Qwen3-4B, also on a single node, trained on Prime Intellect's[Reverse-Text-RL](https://huggingface.co/datasets/PrimeIntellect/Reverse-Text-RL) dataset shows that each adapter does indeed learn the signal. With 32 adapters, a fixed batch size of 2048, and a fixed samples per rollout of 8, the per adapter batch size is 8.


The tradeoff with more adapters is that the per adapter batch size is smaller, so the reward signal is learned more slowly and more stochastically. We discuss this in greater detail in a later section! Below is another run using 4 adapters, which allows us to use a per adapter batch size of 64. Compared to 32 adapters at the same train step, the reward is significantly higher and the learning curve is much more stable.


In our sweep across the number of adapters, we found the final reward to be roughly the same across all experiments. The total batch size (2,048) was held constant on the text reversal dataset, so training and rollout step times remained consistent. However, as the number of adapters increases, the per adapter batch size decreases, which means the total time required to train them also increases. This implies that changing the number of concurrent adapters primarily affects training time and time to first training run completion, representing a classic latency-versus-throughput systems tradeoff.


This tradeoff is precisely why multi-LoRA training matters. Training a single LoRA adapter underutilizes available resources, while training too many concurrently increases the time required for any individual adapter to make progress. The goal is to find a balance that maximizes overall throughput without pushing per-adapter latency beyond what is practical for RL training. We want to ensure that this balance can be struck even when scaling to thousands of policies as first-class RL actors in the future, all without multiplying the base model footprint.


### Trade-Offs & Insights


The tradeoffs in our current setup are relatively simple. Given a fixed compute budget and a maximum trainer batch size, there are compromises between convergence rate, sample efficiency, and the number of LoRA adapters that can be trained simultaneously.


In single-LoRA training, the trainer batch size is:


```text
bsz = n_training_rows × n_samples
```


Where a *training row* is a single row from the dataset, and` n_samples` is the number of rollouts generated for that row.


In multi-LoRA training, the batch size becomes:


```text
bsz = n_loras × n_training_rows × n_samples
```


This is because multiple LoRA adapters are processed within the same training batch. Assuming the total batch size is fixed, increasing the number of LoRA adapters reduces the effective per-LoRA batch size:


```text
n_samples × n_training_rows
```


For example, with a batch size of 2048, the following configurations are all valid:


- 16 LoRAs, each training on 16 rows per step with 8 samples per row
- 32 LoRAs, each training on 8 rows per step with 8 samples per row
- 32 LoRAs, each training on 16 rows per step with 4 samples per row
- 128 LoRAs, each training on 4 rows per step with 4 samples per row


Although training 128 LoRAs simultaneously may seem attractive, the per-LoRA batch size becomes quite small, as we have seen with our stress test. As a result, each optimization step is more stochastic, which can negatively impact convergence. In addition, because each LoRA adapter only processes 2 training rows per step, the time required for any individual adapter to complete training increases substantially.


In the following diagram, we can see that different numbers of adapters directly affect how quickly it takes to learn the reward signal in our fixed batch experiments.


Multi-LoRA training may become much more compelling when the compute budget is more flexible. In large-scale training systems, batch size is often treated as a constrained resource. From an algorithmic perspective, there is typically a critical batch size where larger batch sizes yield diminishing returns. From a systems perspective, the amount of data parallelism is ultimately bounded by the available batch size.


Multi-LoRA training offers an additional scaling dimension. Assuming all multi-LoRA slots can be kept occupied, training can scale horizontally across adapters. Modern rollout engines can already support hundreds of LoRA adapters at once and can be replicated. On the trainer side, data parallelism may be able to scale further, since each adapter has its own independent batch size, increasing the total batch size per optimization step.


To avoid excessive communication overhead on the trainer side, LoRAs can also be partitioned across trainer groups and scaled separately. There is still significant systems and optimization work to be done here, but multi-LoRA training presents a viable path toward scaling reinforcement learning workloads beyond the constraints of traditional single-adapter training.


### Conclusion


Multi-LoRA training turns large-scale RL from "train many separate models" into "train many deltas around one base model." Miles gives us the distributed RL skeleton: Megatron for training, SGLang for rollout, and synchronization between them. By adding a grouped-GEMM multi-LoRA path to Megatron-Bridge and leaning on SGLang's Punica/S-LoRA-inspired serving support, we can run thousands of Qwen3.6-35B-A3B LoRA policies concurrently.


We also see additional benefits from multi-LoRA training aside from the resource efficiency gains:


- Online loading and unloading with multi-LoRA bypasses the cold-start time for spinning up large base models for fine-tuning on a cluster, allowing for faster iteration
- Using multi-LoRA training can quickly scale up post-trained expert models for[on-policy distillation](https://thinkingmachines.ai/blog/on-policy-distillation/) , an increasingly popular post-training technique


At Osmosis, we enable developers to build task-specific models with RL that beat foundation models. Our multi-LoRA trainer is the backbone of our post-training platform. If you're exploring post-training, reach out for access to our research preview.
