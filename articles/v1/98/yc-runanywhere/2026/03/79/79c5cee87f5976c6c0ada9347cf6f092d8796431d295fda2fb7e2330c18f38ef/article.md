---
schema_version: "1.0.0"
document_id: "79c5cee87f5976c6c0ada9347cf6f092d8796431d295fda2fb7e2330c18f38ef"
company_key: "yc-runanywhere"
company: "RunAnywhere"
source_id: "yc-runanywhere-news-import-450c102900e1"
canonical_url: "https://www.runanywhere.ai/blog/metalrt-s2s-fastest-speech-to-speech-apple-silicon"
published_at: "2026-03-15T00:00:00+00:00"
first_seen_at: "2026-07-22T12:25:05.894272+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:70c0608ef0ca788c42595f9b5df6599c27e21b8060f2bd083f8fa9aa7fb5209f"
---

# MetalRT Now Does Speech-to-Speech. 1.52x Faster Than mlx-audio.

We've shipped the fastest LLM decode, the fastest STT and TTS, and the fastest VLM engine for Apple Silicon.


Today, MetalRT adds speech-to-speech. **Audio in, audio out. 1.68 seconds end-to-end.**


We benchmarked LFM2.5-Audio against mlx-audio across three audio lengths. MetalRT won every single one.


**123 tok/s generation throughput. 1.52x faster than mlx-audio.**


We tested against:


- **mlx-audio** - Apple's MLX audio framework (pip install mlx-audio)


## Setup


Engine Compute Benchmark Method


**MetalRT** Metal GPU Native binary


**mlx-audio** Metal GPU Python pipeline


- **Hardware** : Apple M4 Max, 64GB unified memory, macOS 26.3
- **Model** : LFM2.5-Audio-1.5B (8-bit)
- **Audio** : Real speech inputs at 3.8s, 11.2s, and 34.7s
- **Fairness** : Identical audio inputs, same model across both engines


## End-to-End Latency


Full pipeline: audio input, encode, generate text and audio tokens, detokenize to output audio. MetalRT won every length.


*Lower is better*


Audio MetalRT mlx-audio Winner


**Short (3.8s)** **1,785ms** 2,466ms **MetalRT**


**Medium (11.2s)** **1,684ms** 2,558ms **MetalRT**


**Long (34.7s)** **1,771ms** 2,603ms **MetalRT**


1.68 seconds to hear a spoken response to spoken input. Entirely on-device.


## Generation Throughput


Generation is the core of the pipeline — producing both text and audio tokens from the encoded input.


*Higher is better*


Audio MetalRT mlx-audio Winner


**Short (3.8s)** **116 tok/s** 82 tok/s **MetalRT**


**Medium (11.2s)** **123 tok/s** 79 tok/s **MetalRT**


**Long (34.7s)** **118 tok/s** 78 tok/s **MetalRT**


The speedups:


- **1.42x** on short audio
- **1.56x** on medium audio
- **1.50x** on long audio


## MetalRT vs mlx-audio


MetalRT is **1.38-1.52x faster than mlx-audio** on total end-to-end latency across all audio lengths.


## What MetalRT Is Built For


Use Case Why MetalRT


**Voice assistants** Sub-2s spoken response to spoken input, entirely on-device


**Real-time interpretation** Fast enough for conversational back-and-forth


**Accessibility tools** Instant voice interaction without cloud dependencies


**Privacy-first voice apps** Audio never leaves the device


**Edge deployment** Voice AI in offline and air-gapped environments


## Summary


- **1,684ms** best end-to-end latency (Medium)
- **123 tok/s** peak generation throughput
- **1.52x faster** than mlx-audio (Medium)
- **100%** win rate across all audio lengths


Output quality is comparable across both engines. The model is the same. The speed is not.


S2S support via MetalRT will be available through[RCLI](https://github.com/RunanywhereAI/RCLI) . Stay tuned.


---


*Benchmarked on Apple M4 Max, 64GB RAM, macOS 26.3. Model: LFM2.5-Audio-1.5B (8-bit). Full pipeline: audio encode, text+audio token generation, detokenize to output WAV. Identical audio inputs across both engines.*
