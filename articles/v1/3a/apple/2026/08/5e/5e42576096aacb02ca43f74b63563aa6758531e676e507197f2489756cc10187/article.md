---
schema_version: "1.0.0"
document_id: "5e42576096aacb02ca43f74b63563aa6758531e676e507197f2489756cc10187"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/locking-pretrained-weights"
published_at: null
first_seen_at: "2026-08-06T23:08:34.959267+00:00"
fetched_at: "2026-08-06T23:08:36.751844+00:00"
content_hash: "sha256:13980f043f53980a43a65e97dbed3f88636598c054d86a43fb0d201fddc640e9"
---

# Locking Pretrained Weights via Deep Low-Rank Residual Distillation

[View publication](https://arxiv.org/abs/2605.10777)


The quality of open-weight language models has dramatically improved in recent years. Sharing weights greatly facilitates model adoption by enabling their use across diverse hardware and software platforms. They also allow for more open research and testing, to the extent that users can use them as checkpoints, fine-tune them according to their needs, and potentially redistribute them. In some cases, however, concerns on modifying these weights towards unauthorized uses may outweigh the pros of giving users such a freedom. Defending against such adaptation is non-trivial: since an adaptive attacker can observe all weights and architectures by definition, they can reverse simple structural defenses, and use optimization to defeat the simplest locking mechanisms. In this work, we exploit the inference–training asymmetry of automatic differentiation as a novel defense axis. We propose DLR-Lock, a method where the purveyor of the model purposely replaces each pretrained MLP in their model with a deep low-rank residual network (DLR-Net) of comparable parameter count, forcing activation memory that grows linearly with depth during backpropagation. DLR-Nets are efficiently trained via module-wise distillation. We show that, beyond this memory overhead, DLR-Lock results in architectural mismatches that complicate the optimization landscape of standard fine-tuning, and a backward pass that incurs disproportionately more overhead than the forward pass. Our defense succeeds in withstanding adaptive attackers with full knowledge of the defense strategy while preserving the original model’s capabilities. Experiments on LLM validate these claims.


- † University of Tokyo
- ** Work done while at Apple
