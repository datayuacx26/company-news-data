---
schema_version: "1.0.0"
document_id: "d575894036b57bf52a0d8f93f7ee019c439a525dc38954cfa171f2db35cffbdd"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/unlocking-lora-moe-rl-for-qwen3-5"
published_at: "2026-04-03T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:90be6fbf0555d3e0927ace30958bcd76175dc58b54197bcd0533ad5e92aaafe5"
---

# Unlocking LoRA MoE RL for Qwen3.5

**TL;DR: We merged three separate Megatron-Bridge forks, resolved Docker dependency conflicts, and fixed a CUDA IPC race condition. We ported an end-to-end pipeline that enables LoRA RL training for MoE Qwen3.5 models like Qwen3.5-122B-A10B.**


### Overview


In order to train large MoE models with RL, you need two systems: a training cluster and an inference cluster. Our stack pairs Megatron-LM with SGLang for this purpose. We wanted to share the process of how we added support for LoRA within our training stack - specifically for the Qwen3.5 MoE models.


Existing LoRA + RL implementations for Qwen3.5 exist (e.g. Unsloth), but they operate in a single-process regime: i.e. they lack model sharding and expert replay, don’t use expert or tensor parallelism during training, and don’t have online weight sync to a dedicated serving engine. In order to integrate LoRA into a system designed for larger scale training, we solved the following challenges:


- Merge conflicts ([SGLang](https://github.com/sgl-project/sglang) ,[Miles](https://github.com/radixark/miles) ,[slime](https://github.com/THUDM/slime) ,[Megatron-Bridge](https://github.com/NVIDIA-NeMo/Megatron-Bridge) )
- Race conditions on LoRA weight sync
- Dependency issues (Docker, CUDA,[Transformers](https://github.com/huggingface/transformers) )
- Architecture integration (Qwen3.5 bridge registration, MoE parallelism validation)


### Merge Conflicts


The first issue was that no codebase had everything we needed. The official Megatron-Bridge repository has general LoRA support, but lacks an MoE bridge for Qwen3.5 as well as support for LoRA export to SGLang. We found three separate Megatron-Bridge forks - all with only one piece of the puzzle:


- [fzyzcjy/Megatron-Bridge@dev_rl](https://github.com/fzyzcjy/Megatron-Bridge/tree/dev_rl) - used by slime, but has no LoRA export
- [yushengsu-thu/Megatron-Bridge](https://github.com/yushengsu-thu/Megatron-Bridge/tree/merged-megatron-0.16.0rc0-miles) - has LoRA export, but doesn’t have Qwen3.5 support
- [coding-farmer/Megatron-Bridge-slime@qwen35](https://github.com/coding-famer/Megatron-Bridge-slime/tree/qwen35) - has Qwen3.5 support, but has no LoRA export


We merged the latter two into a separate branch ([Osmosis-AI/Megatron-Bridge](https://github.com/Osmosis-AI/Megatron-Bridge/tree/main) ), combining Qwen3.5 support with LoRA export support. In the process, we also resolved conflicts related to bridge registration decorators, weight tuple formats, and configuration validation.


On the Megatron-LM side, the initial setup (slime-provided Megatron-LM setup for Qwen3.5) applied a 500+ line patch file to NVIDIA's upstream repository at Docker build time. We built patches for multi-token prediction (MTP) training, multi-head latent attention (MLA) fixes, and MoE routing replay (more about routing replay[here](https://arxiv.org/pdf/2510.11370) ) natively into our implementation to avoid fragile patch-apply steps that break on upstream changes.


LoRA Export to SGLang Qwen3.5 MoE Bridge Support


[Megatron-Bridge](https://github.com/NVIDIA-NeMo/Megatron-Bridge)


[fzyzcjy/Megatron-Bridge@dev_rl](https://github.com/fzyzcjy/Megatron-Bridge/tree/dev_rl)


[yushengsu-thu/Megatron-Bridge](https://github.com/yushengsu-thu/Megatron-Bridge/tree/merged-megatron-0.16.0rc0-miles)


[coding-farmer/Megatron-Bridge-slime@qwen35](https://github.com/coding-famer/Megatron-Bridge-slime/tree/qwen35)


[Osmosis-AI/Megatron-Bridge](https://github.com/Osmosis-AI/Megatron-Bridge/tree/main)


### Race Conditions


With the merge conflicts resolved, the next issue was getting LoRA weight sync working between Megatron-LM and SGLang during inference/trainer colocated scenarios.


At a high level, what happens is:


1. To prepare for syncing weights, the rank 0 trainer worker will create a flattened tensor representation copy ([slime](https://github.com/THUDM/slime/blob/dbd4e738d6b9b505fb90ec6a4c0a6a0410c82119/slime/backends/megatron_utils/update_weight/update_weight_from_tensor.py#L236) ,[SGLang](https://github.com/sgl-project/sglang/blob/3c91ebdf55261ce412fb15aaba52dcb3f52f3b8e/python/sglang/srt/weight_sync/tensor_bucket.py#L19) ) of the LoRA weights and create a CUDA IPC handle pointing to it along with some other metadata. A PyTorch internal reference counter for the flattened tensor memory will increment and will decrement on deletion of the tensor.
2. The rank 0 trainer worker serializes the metadata and the CUDA IPC handle and makes a request to the SGLang HTTP server via` POST /load_lora_adapter_from_tensors` ([miles 1](https://github.com/radixark/miles/blob/c09647012038a11b54eb42e3b31e2013c99484d7/miles/backends/megatron_utils/update_weight/update_weight_from_tensor.py#L339) ,[miles 2](https://github.com/radixark/miles/blob/c09647012038a11b54eb42e3b31e2013c99484d7/miles/backends/sglang_utils/sglang_engine.py#L305) ). This allows SGLang to access the new weights with zero copying.
3. SGLang still needs to update the LoRAs per TP worker though, so it dispatches the request to the rank 0 TP worker via ZMQ then which broadcasts the request to all TP workers (note: technically request handling is at the scheduler level which are 1:1 with TP workers).
4. Critically, SGLang’s TokenizerManager is configured to wait for` server_args.dp_size` results ([SGLang](https://github.com/sgl-project/sglang/blob/3c91ebdf55261ce412fb15aaba52dcb3f52f3b8e/python/sglang/srt/managers/tokenizer_communicator_mixin.py#L234) ) from the schedulers (via its` _Communicator` which acts like a barrier) until it returns an HTTP response. Since` server_args.dp_size` is always equal to 1 for dynamic LoRA loading as of now ([SGLang](https://github.com/sgl-project/sglang/blob/3c91ebdf55261ce412fb15aaba52dcb3f52f3b8e/python/sglang/srt/managers/tokenizer_communicator_mixin.py#L676) ), it immediately returns after a single scheduler/TP worker completes, despite broadcasting the update to all schedulers. This only happens when loading LoRAs and does not happen when performing a full weight sync.
5. Once the rank 0 trainer worker receives the response from the SGLang HTTP server, it assumes that all LoRAs have been loaded on each SGLang TP worker and deletes the flattened tensor data ([miles](https://github.com/radixark/miles/blob/c09647012038a11b54eb42e3b31e2013c99484d7/miles/backends/megatron_utils/update_weight/update_weight_from_tensor.py#L195) ). However, this is not necessarily true: we only know that at least one TP worker has loaded the LoRA.
6. It is possible that not all TP workers have not opened the CUDA IPC handle pointing to the flattened tensor data yet, which means that PyTorch memory reference count drops to zero, allowing for it to be deleted. When something goes wrong with CUDA IPC, we get` torch.AcceleratorError: CUDA error: operation not permitted in _new_shared_cuda` .


In Miles’ example scripts for LoRA, they always run with` --rollout-num-gpus-per-engine` set to 1 ([miles](https://github.com/radixark/miles/tree/c09647012038a11b54eb42e3b31e2013c99484d7/examples/lora) ), which directly maps to a single TP worker. In this case, this race condition will not occur because there is only a single TP worker.


There are a few workarounds for this race condition:


1. Like Miles, we can set` --rollout-num-gpus-per-engine` to 1, which is effectively TP=1. This is not practical for larger models in the Qwen3.5 family.
2. We can update SGLang to wait for all TP workers to give an result before returning. However, only the rank 0 TP worker/scheduler communicates with the tokenizer manager, so there would have to be a separate level of synchronization.
3. We can hold onto the tensors in on our training worker for a bit longer before freeing them - long enough for us to ensure that the LoRAs are loaded and the flattened tensors can be freed.


We opted for (3) because it was the simplest method to solve our problem. (1) is not as practical and (2) would take longer to implement correctly. In our code, we use a little bit of extra storage on the trainer side to hold the LoRA flattened tensors until the next training iteration before letting them go out of scope.


Our implementation is available[here](https://github.com/Osmosis-AI/Megatron-Bridge/tree/main) .


### Dependency Issues


Our Docker image contains:


- slime
- SGLang
- FlashAttention 2.8.3 with FlashAttention3 Hopper kernels
- TransformerEngine 2.12
- NVIDIA Apex
- Megatron-LM
- Megatron-Bridge with merged changes from` yushengsu-thu` and` coding-farmer` branches


Getting all the packages to agree on CUDA, cuDNN, and torch versions was a non-trivial effort, but the biggest issue was the transformers version. Our container had` transformers==4.57.1` , which doesn't natively recognize` qwen3_5_moe` as a model type. With` trust_remote_code=True` , it pulled the remote config class from HuggingFace Hub at runtime and synthesized default values (` intermediate_size=5632` ,` rope_theta=10000.0` ) that silently conflicted with our training configuration. This results in a training run that starts normally, but produces corrupted rollouts:


We fixed this by upgrading to` transformers>=5.2.0` . The native` Qwen3_5MoeConfig` uses different attribute names (` moe_intermediate_size` instead of` intermediate_size` ,` rope_parameters.rope_theta` instead of` rope_theta` ), so the validator skips the conflicting checks entirely. This is a safe change for LoRA runs, and non-LoRA runs bypass` AutoConfig` validation via the` --spec` path regardless.


### Architecture Integration


Finally, the Qwen3.5 bridge registration required a fallback strategy. The LoRA bridge helper tries native Qwen3.5 bridges first, auto-registered via` @register_bridge` decorators. If those aren't available, it will alias Qwen3.5 to Qwen3-VL with patches for` rope_theta` and` intermediate_size` . This is important since if the bridge was implemented incorrectly, it would produce incorrect weight conversions that only show as degraded training performance.


It’s also worth mentioning that MoE expert parallelism added a hard constraint on the parallelism layout:` world_size % (EP * ETP * PP) == 0` . (hard-coded in SGLang[here](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/server_args.py) ) Our testing setup was EP=8 and TP=2 on 8×H200, and required mutual consistency between the LoRA config, parallelism dimensions, and expert distribution.


While configuration errors generally produce clean failure messages, issues here would be harder to diagnose as they’d deceptive surface as wrong gradients, a hang during the first all-reduce, or silently incorrect expert routing.


**Single Node (8xH200)**


Config TP PP EP ETP DP Notes


A (ours) 2 1 8 1 1 Best MoE config; max EP keeps 32 experts/rank. TP=2 shards attention. Works for 122B and 35B.


B 1 1 8 1 1 No TP overhead. Works for 35B, but triggers attention unsharded OOM on 122B (3072 hidden × 32 heads full per rank).


C 4 1 4 1 1 35B: heads=16, GQA=2 → 16%4=0, 2%4≠0 — invalid for 35B. 122B: heads=32, GQA=2 → valid. Halves EP → 64 experts/rank


D 2 1 4 1 1 DP=1 (8/2/1/4=1). Half EP means 64 experts/rank — more memory per rank, worse load balance.


E 2 2 4 1 1 PP=2 splits layers across pipeline stages. Hurts RL throughput due to bubble overhead. Avoid using.


F 2 1 8 2 1 ETP=2 splits each expert's FFN across 2 ranks reduces per-expert memory but adds communication overhead. Niche.


G 8 1 1 1 1 Dense-style sharding. All 256 experts on every rank 256× memory duplication. Only for tiny models (4-8B)


**Two Nodes (8xH200)**


Config TP PP EP ETP DP Notes


H (ours) 2 1 8 1 2 DP=2 doubles effective batch throughput. EP=8 stays intra-node (no cross-node all-to-all). Best.


I 2 1 16 1 1 EP=16 spans both nodes. Cross-node all-to-all is 5-10X slower. Avoid.


J 4 1 8 1 1 TP=4, 122B only (GQA=2 constraint). More TP communication, but no cross-node EP.


K 2 2 8 1 1 PP=2 cross-node. Avoids DP gradient all-reduce but adds pipeline bubbles.


L 2 1 8 2 1 ETP=2 plus DP=1. Each expert split across 2 ranks. Only useful if a single expert is too large.


### Testing


To validate our implementation, we confirmed consistent loss curves between a LoRA configuration and a full fine-tuning baseline on DAPO-Math-17K under the same parallelism layout. We ran validation tests with the following configuration:


- Model: Qwen3.5-122B-A10B
- LoRA: rank=32, alpha=32, target qkv_proj,o_proj,gate_up_proj,down_proj, EP=8, TP=2
- Full Fine-Tuning (FFT): Same configuration without LoRA (baseline)
- Context Budget: 16K tokens
- Algorithm: GRPO
- Dataset: DAPO-Math-17K
- Evaluation: AIME 2024
- Resources: 16 H200s for LoRA, 64 H200s for Full Fine-Tuning


### Results


The LoRA run achieved 44% higher token throughput than the FFT run, primarily driven by reduced gradient computation and smaller weight updates. We confirmed no routing divergence between training and inference, and there was stable expert utilization across all expert parallelism ranks. LoRA preserved the same training dynamics as FFT, given that loss curves converged to ~3% of the baseline by step 150.


Qwen3.5 is, from our experience, the most powerful open source model family currently available. By increasing the scale of parallelism and unlocking LoRA RL training, we’re excited to improve training efficiency for Pareto optimal models like Qwen3.5-122B-A10B - making it easier to iterate on RL use cases that require more performance than the small, dense models that fit on a single GPU or node.
