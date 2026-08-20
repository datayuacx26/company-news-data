---
schema_version: "1.0.0"
document_id: "ed43b2e476e286b199bd8d3707cfc3dd42926227d93143858396a86abf9b52e2"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/calibrated-sparse-attention"
published_at: null
first_seen_at: "2026-07-21T19:16:24.438497+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:74cda47e6d87432a91670b6a066fcc84e35c5d91eca7a969e281b76e142e23a1"
---

# Accelerating Text-to-Video Generation with Calibrated Sparse Attention

[View publication](https://arxiv.org/abs/2603.05503)


Recent diffusion models enable high-quality video generation, but suffer from slow runtimes. The large transformer-based backbones used in these models are bottlenecked by spatiotemporal attention. In this paper, we identify that a significant fraction of token-to-token connections consistently yield negligible scores across various inputs, and their patterns often repeat across queries. Thus, the attention computation in these cases can be skipped with little to no effect on the result. This observation continues to hold for connections among local token blocks. Motivated by this, we introduce CalibAtt, a training-free method that accelerates video generation via calibrated sparse attention. CalibAtt performs an offline calibration pass that identifies block-level sparsity and repetition patterns that are stable across inputs, and compiles these patterns into optimized attention operations for each layer, head, and diffusion timestep. At inference time, we compute the selected input-dependent connections densely, and skip the unselected ones in a hardware-efficient manner. Extensive experiments on Wan 2.1 14B, Mochi 1, and few-step distilled models at various resolutions show that CalibAtt achieves up to 1.58× end-to-end speedup, outperforming existing training-free methods while maintaining video generation quality and text-video alignment.


- † Tel Aviv University
- ** Work done while at Apple
