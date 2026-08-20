---
schema_version: "1.0.0"
document_id: "50e54e7164baad17ad47744845f28a9131ce37791de0b479ad1cef7e1a40aa1b"
company_key: "yc-camfer"
company: "camfer"
source_id: "yc-camfer-news-import-eadafe2b34d8"
canonical_url: "https://camfer.dev/blog/flare/"
published_at: "2025-08-21T00:00:00+00:00"
first_seen_at: "2026-07-24T23:30:43.881527+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:21c80804452829855c50340c40d5534cdb934308376b2cf176ecc3f191be8b0d"
---

# FLARE: Fast Low-rank Attention Routing Engine

Wanted to highlight some awesome work done by[Vedant Puri](https://vpuri3.github.io/) et al. in collaboration with camfer!


[FLARE](https://huggingface.co/papers/2508.12594) is a novel self-attention mechanism that learns a low-rank token mixing mechanism by routing attention through fixed-length latent sequences. This reduces the attention-block time complexity to linear scale, meaning we can learn on long sequences like point clouds!


FLARE can be implemented with standard SDPA kernels and scales to meshes with one million points (over 200× faster than vanilla attention!). In torch, this is as simple as:


```text
import   torch.nn.functional   as   F
def   flare_multihead_mixer  (q, k, v):
"""
Arguments:
q: Query tensor [H, M, D]
k: Key tensor [B, H, N, D]
v: Value tensor [B, H, N, D]
Returns:
y: Output tensor [B, H, N, D]
"""
z   =   F.scaled_dot_product_attention(q, k, v,   scale  =  1.0  )
y   =   F.scaled_dot_product_attention(k, q, z,   scale  =  1.0  )
return   y
```


By projecting the input sequence of length` N` , then unprojecting back up, we get a low-rank form of attention with rank at most` M` . And since FLARE allocates a distinct slice of the latent tokens to each head, we get` num_heads` distinct projection matrices that can specialize in their routing patterns.


We’re excited to see how models built on FLARE perform when applied to downstream tasks, such as generating embeddings for LLM point-cloud understanding.


Code can be found at[https://github.com/vpuri3/FLARE.py](https://github.com/vpuri3/FLARE.py) .


If this line of work is interesting, please reach out directly tohiring@camfer.dev ! Or see open roles[here](https://camfer.dev/careers/) !
