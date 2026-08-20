---
schema_version: "1.0.0"
document_id: "13b460a666bf3f22286f19d9e1d7aeffcecdf3dd832eddc98a7c19b76e1ae0b4"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/audio-synthesis-diffusion-transformers"
published_at: null
first_seen_at: "2026-07-29T03:22:23.076826+00:00"
fetched_at: "2026-07-29T03:22:24.586686+00:00"
content_hash: "sha256:d082704b7f786187f29d1eea3de5528ac62ae5749e9586a872eda1d2047f8d34"
---

# Memory Efficient Audio Synthesis with Decoupled Temporal Depth Diffusion Transformers

[View publication](https://arxiv.org/abs/2607.23811)


Siri Expressive Voices synthesize rich, configurable speech in real time and entirely on device, powered by AFM 3 Core Advanced, Apple’s most powerful on-device foundation model. This work presents the memory-efficient audio synthesis architecture behind that capability: a detokenizer that converts the semantic audio tokens emitted by the foundation model into high-fidelity audio within the tight compute and memory budget of the Apple Matrix Coprocessor (AMX). We convert semantic audio tokens to a residual vector quantization (RVQ) representation with a three-component design—a streaming encoder, a temporal decoder, and a depth decoder—that systematically decouples temporal and depth processing. A single reusable depth decoder with Diffusion Transformer (DiT)-style stage conditioning generates all RVQ levels autoregressively, replacing the dedicated per-level decoders of prior multi-decoder architectures, while causal sliding window attention with fixed-window key-value caching yields constant memory complexity independent of sequence length. Deployed on the AMX, the detokenizer sustains roughly 10ms per generation step—about 16x faster than real time—with a peak runtime memory of only ∼21MB and 329MB of on-device assets, enabling continuous streaming synthesis of 20–320 seconds of audio alongside the on-device foundation model. This constant, small footprint replaces the linear and quadratic memory scaling of conventional transformer- and GAN-based approaches. Comprehensive ablation studies validate the effectiveness of key architectural components, including DiT conditioning mechanisms, temporal lookahead processing, and unified depth decoding strategies. Audio quality assessment through phonetic discriminability analysis, perceptual quality metrics, and neural quality estimation confirms that the proposed architecture maintains synthesis fidelity while achieving computational efficiency gains over existing methodologies. The proposed architecture is deployed in production as part of Siri Expressive Voices, powering a voice overhaul with Pace and Expressivity customizations sliders in Apple Devices and support for custom assistant voices. Operating at a 1-billion-parameter activation size within AFM 3 Core Advanced, it improves Mean Opinion Score (MOS) by +0.28 overall (4.15 vs. 3.87) and by +0.42 on conversational speech (4.24 vs. 3.82) over the prior on-device text-to-speech system.


- * Equal contribution
