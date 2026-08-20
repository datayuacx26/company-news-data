---
schema_version: "1.0.0"
document_id: "8cc7a89d22b8aef692937a2c02c6dce1a75902854d425ff203dba2c0b0cdfa8b"
company_key: "yc-sf-tensor"
company: "SF Tensor"
source_id: "yc-sf-tensor-news-import-73b0de6f3d55"
canonical_url: "https://sf-tensor.com/engineering/multi-tenant-lora"
published_at: "2026-05-17T00:00:00+00:00"
first_seen_at: "2026-07-27T05:05:23.755525+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:448df2d246b3bb26a4d89e18f6500be30f2454e151287880aa5118f1e31fd912"
---

# One Model Per Customer Shouldn't Mean One GPU Per Customer

Ship a foundation model as a product and you'll get the same request from every customer: they want *their* model. Tuned to their data, serving only their traffic, their data walled off from everyone else's.


The obvious move is to fine-tune a separate copy of the model per customer. It works. And it's miserable to serve: the bill scales linearly with your customer count and most of that bill is idle GPU.


There's a better pattern, and most teams converge on it: one shared base model, one tiny per-customer adapter, served together. It's well-trodden, it's productized, and the serving economics are good. Not just tolerable, good. This post covers why the naive approach hurts, how LoRA fixes the math, how good LoRA really is next to a full fine-tune, and how to batch requests from *different* customers on the same GPU.


## The naive way: one model per customer


Picture a model with ~12GB of weights. Fully fine-tune a separate copy per customer and every time you switch which customer you're serving, you drag a fresh 12GB onto the GPU. That I/O dominates everything else.


So you can't co-locate customers on the same hardware at all. You'd think you could just pack a few onto one box and call it a day. Turns out that's bullshit the moment you do the I/O math: you end up paying for a dedicated, mostly-idle replica per customer. Cost scales linearly with headcount, and most of that cost is a GPU doing nothing.


A GPU should serve customers, not babysit one. What you actually want is simple: keep the expensive, shared part of the model resident on the GPU once, and swap only the small, customer-specific part. That's exactly what LoRA gives you.


## One base, many tiny adapters


LoRA fine-tunes a model by **freezing the original weights** and learning a small low-rank "correction" on top. Instead of updating a weight matrix` W` directly, you learn two small matrices` A` and` B` and add their product as a delta:


```text
output = x·Wᵀ  +  (x·Aᵀ)·Bᵀ · (α/r)
└─────┘   └──────────────┘
shared base   per-customer delta


```


` A` is` \[r × d\]` ,` B` is` \[d × r\]` , the **rank**` r` is small, typically 8-64, and the hidden dimension` d` runs into the thousands. The adapter is therefore *tiny* next to the full weights: tens of MB against a base measured in gigabytes, depending on rank and which layers you target.


That changes the serving economics completely. Keep the base resident on the GPU **once** and stream in only the per-customer adapter. Loading an adapter is a millisecond operation, not a multi-second, multi-gigabyte transfer.


One detail matters for multi-tenant serving. You *can* merge an adapter into the base (` W' = W + BA` ) for zero inference overhead, but then that GPU is locked to a single customer, which defeats the entire point. For shared serving you keep the base and the adapter **separate** , compute the two terms independently, and add them. You pay a small overhead on the delta. In return, you serve many customers from one resident base.


Bonus: clean data isolation falls out for free. Each customer's adaptation lives entirely inside their own adapter weights and never touches the shared base.


## Is anyone actually doing this? Yes.


This is an established serving pattern, not research speculation. The work that matters:


- **LoRA** : Hu et al., 2021 ([arXiv:2106.09685](https://arxiv.org/abs/2106.09685) , ICLR 2022). The original method.
- **S-LoRA: Serving Thousands of Concurrent LoRA Adapters** : Sheng et al., 2023 ([arXiv:2311.03285](https://arxiv.org/abs/2311.03285) , MLSys 2024). The paper most directly about multi-tenant serving. One shared base, thousands of adapters kept in main memory and paged onto the GPU on demand, custom kernels that batch adapters of *different ranks* together. It reports roughly 2,000 adapters on a single A100 and up to about 4× the throughput of naive LoRA support in vLLM/PEFT.
- **Punica: Multi-Tenant LoRA Serving** : Chen et al., 2023 ([arXiv:2310.18547](https://arxiv.org/abs/2310.18547) , MLSys 2024). Introduces the **SGMV** kernel that makes cross-customer batching efficient (more below), reporting around 12× throughput over prior systems while adding only ~2ms latency per token.
- **dLoRA: Dynamically Orchestrating Requests and Adapters** : Wu et al., OSDI 2024. Automates the merged-vs-unmerged call with a credit-based batching algorithm, and migrates requests and adapters across replicas to balance load.


On the framework side, it's already shipped. **vLLM** has multi-LoRA support, **LoRAX** (Predibase) is purpose-built for exactly this pattern, and **TensorRT-LLM** supports it too. The takeaway: don't write your own serving kernels. Start from a stack that already exists.


## How good is LoRA versus a "real" fine-tune?


Honest answer: it depends on how far the customer sits from the base. And there's now solid literature mapping that out.


The original LoRA paper showed parity with, sometimes beating, full fine-tuning on adaptation-style tasks. The essential counterweight is **"LoRA Learns Less and Forgets Less"** (Biderman et al., TMLR 2024,[arXiv:2405.09673](https://arxiv.org/abs/2405.09673) ). In one sentence: in standard low-rank settings, LoRA *underperforms* full fine-tuning most when the target demands learning **substantial new knowledge far from pretraining** , their hard cases were code and math, but it *forgets the base capabilities much less* , acts as a strong regularizer, and keeps generations more diverse. They also clocked full fine-tuning learning weight changes with 10–100× higher rank than a typical LoRA, which explains a lot of the gap.


The practical reading: if each customer is mostly a *shift within* a domain your base already knows, rather than a leap into genuinely new territory, you're in the regime where LoRA does well. Sensible default, **LoRA is good enough** , with full fine-tuning kept in your back pocket as a per-customer escape hatch for the rare customer that proves hard.


For customers in between, reach for these before a full fine-tune: raise the rank, target more layers, or use **DoRA** (Liu et al., ICML 2024 Oral,[arXiv:2402.09353](https://arxiv.org/abs/2402.09353) ), which splits each weight into a magnitude and a direction and applies LoRA to the direction. It closes much of the remaining gap at similar parameter cost, and **adds no inference overhead** .


**One caveat, flagged loudly.** Nearly all the published quality evidence is on language models. Apply this to another modality, time-series, vision, audio, and the *mechanics* transfer fine (LoRA attaches to any linear layer), but the *quality numbers* above are borrowed from the LLM world and may not carry over. In a less-studied modality, run your own LoRA-versus-full-fine-tune comparison early, instead of assuming the gap holds.


## Can you batch across different customers' adapters?


Yes, and this is the part that makes the economics good instead of just acceptable.


The key observation: the **expensive, shared** work, attention and the big feed-forward matmuls on the base weights` W` , is **identical for every request in the batch** , no matter which customer sent it. So it runs as one large, efficient matrix multiply and gets the full payoff of big batch sizes. The *only* thing that differs per request is the small low-rank delta` (x·Aᵀ)·Bᵀ` , where each request may use a different` A, B` .


The naive way to handle that divergence, loop over requests, do each adapter's little multiply on its own, is exactly what kills throughput. Those tiny per-request ops go memory-bound and waste the GPU. The fix is custom kernels that batch *across* adapters:


- **Punica's SGMV** (Segmented Gather Matrix-Vector multiplication) groups requests by which adapter they use, gathers the right` A` /` B` per group, and runs the low-rank multiply as a single batched operation. Overhead stays small against the shared base pass. (Punica's original SGMV assumed all adapters share a rank, padding otherwise.)
- **S-LoRA's batched kernels** do the same thing, but also handle adapters of **different ranks** in one batch, plus the paged memory management for swapping adapters in and out of GPU memory.


So the cost structure lands where you'd want: the expensive base path is fully shared and batch-efficient, and the per-customer cost is just a small rank-` r` projection done inside a grouped kernel.


### "Isn't this just MoE routing?"


Tempting comparison, and as a *systems* pattern the instinct is right, group requests by which weights they need, then do segmented matmuls. But one distinction matters. In MoE, routing is **learned** and the experts are part of the model; any token can be sent to any expert. In multi-tenant LoRA, the routing is **trivial and deterministic** : you already know which customer sent each request, so there's no router and no load-balancing loss to train. It's a gather by customer ID, nothing more. (The two *have* been combined: "Mixture of LoRA Experts", but for one-adapter-per-customer serving you want the plain deterministic version. Simpler, and exactly what SGMV implements.)


## Practical notes


- **Adapter cold-start is cheap.** Loading a customer's adapter is an MB-scale transfer, not a multi-gigabyte base load, so on-demand loading from CPU or disk is fine. S-LoRA-style paging handles eviction when you have more adapters than fit in GPU memory at once.
- **Merged vs. unmerged is a latency knob, not a one-time decision.** A customer with steady, heavy traffic earns a dedicated *merged* replica (lowest possible latency). The long tail of low-traffic customers shares the *unmerged* multi-tenant pool. dLoRA automates the tradeoff at runtime.
- **Don't build the serving layer yourself.** Start from vLLM, LoRAX, or TensorRT-LLM. The paging and the batched kernels are already written.


## A path that works


Building this from scratch? A reasonable default: adopt an existing multi-LoRA serving framework; default every customer to a LoRA adapter against the shared base; validate the LoRA-versus-full-fine-tune quality gap on your own data as an early milestone (especially outside the LLM modality); and reserve higher rank, more target layers, DoRA, or a dedicated full fine-tune for the few customers that actually need it.


The headline: "one model per customer" doesn't have to mean "one GPU per customer." One shared base, per-customer adapters, and you collapse the long tail onto shared hardware, keep every customer's data cleanly isolated, and still hand each of them a model that behaves like it's theirs.


Serving infrastructure should be boring. This is how you make it boring.


---


The serving story is one half. The other half is training the adapters: one big base run, then a long tail of small per-customer fine-tunes that show up whenever a customer does. Most infra makes you choose how to size for that. You shouldn't have to. We built[SF Tensor](https://sf-tensor.com/) so pre-training and a few hundred on-demand LoRA jobs run on the same stack, on whatever accelerator is cheapest at the time, without you babysitting any of it. If you're doing this kind of thing,[talk to us](https://cal.com/team/sf-tensor/first-meeting) .


---


## References


1. Hu et al. *LoRA: Low-Rank Adaptation of Large Language Models.* arXiv:2106.09685 (2021); ICLR 2022.
2. Sheng et al. *S-LoRA: Serving Thousands of Concurrent LoRA Adapters.* arXiv:2311.03285 (2023); MLSys 2024.
3. Chen et al. *Punica: Multi-Tenant LoRA Serving.* arXiv:2310.18547 (2023); MLSys 2024.
4. Wu et al. *dLoRA: Dynamically Orchestrating Requests and Adapters for LoRA LLM Serving.* USENIX OSDI 2024.
5. Biderman et al. *LoRA Learns Less and Forgets Less.* arXiv:2405.09673; TMLR 2024.
6. Liu et al. *DoRA: Weight-Decomposed Low-Rank Adaptation.* arXiv:2402.09353; ICML 2024 (Oral).
