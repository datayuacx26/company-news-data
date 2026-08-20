---
schema_version: "1.0.0"
document_id: "46a31bd8151fe5ae87cdcba0a29f0894ab2a41e9e1ecdb910ec10f6d5454ae39"
company_key: "yc-runanywhere"
company: "RunAnywhere"
source_id: "yc-runanywhere-news-import-450c102900e1"
canonical_url: "https://www.runanywhere.ai/blog/metalrt-vlm-fastest-vision-language-model-apple-silicon"
published_at: "2026-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:59.131595+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:f01dad0da56fde4ebc1f394d3e57962f2b7c47133e825cc3a32f53819d11b44b"
---

# MetalRT Now Runs Vision Language Models. Fastest on Apple Silicon.

First, we shipped the fastest LLM decode engine for Apple Silicon. Then, the fastest Speech-to-Text and Text-to-Speech.


Today, MetalRT adds vision. **The fastest VLM decode engine for Apple Silicon.**


We benchmarked Qwen3-VL-2B across four image resolutions against every major engine. MetalRT won every single configuration.


**279 tok/s average vision decode. 92ms time-to-output. 100% win rate.**


We tested against:


- **mlx-vlm** - Apple's MLX vision-language framework
- **llama.cpp** - the most widely-used open-source inference engine


## Setup


Engine Type Benchmark Method


**MetalRT** C++ Native binary


**mlx-vlm** Python + MLX C++ Python API


**llama.cpp** C/C++ Server API (server-reported)


- **Hardware** : Apple M4 Max, 64GB unified memory, macOS 26.3
- **Model** : Qwen3-VL-2B-Instruct (4-bit quantized)
- **Images** : Real photograph (COCO val2017) at 224×224, 320×320, 448×448, 448×896
- **Runs** : 5 per engine, averaged
- **Fairness** : Identical image, identical prompt, greedy decoding (temperature=0) across all engines


## Vision Decode Throughput


Decode speed is how fast tokens stream to the user after the image is processed. MetalRT won every resolution.


*Higher is better*


Image MetalRT mlx-vlm llama.cpp Winner


**224×224** **285.4** 239.1 231.0 **MetalRT**


**320×320** **286.6** 234.8 230.2 **MetalRT**


**448×448** **279.3** 226.7 226.8 **MetalRT**


**448×896** **264.7** 217.3 215.3 **MetalRT**


The speedups:


- **1.19-1.23x vs mlx-vlm**
- **1.23-1.25x vs llama.cpp**


## Time-to-Output


Time-to-output measures the full pipeline — vision encoding, prefill, and first tokens out. This is what the user actually feels.


*Lower is better*


Image MetalRT mlx-vlm llama.cpp Winner


**224×224** **92.4ms** 118.5 141.9 **MetalRT**


**320×320** **120.3ms** 142.8 161.5 **MetalRT**


**448×448** **171.8ms** 204.1 219.5 **MetalRT**


**448×896** **346.1ms** 334.7 362.7 **MetalRT**


92ms to process an image and start generating. That's faster than a single frame at 12fps.


## Text Decode Throughput


VLMs also handle text-only prompts. MetalRT wins every context length.


*Higher is better*


Context @ Decode MetalRT mlx-vlm llama.cpp Winner


**128 @ 50** **290.3** 244.9 230.6 **MetalRT**


**256 @ 100** **290.2** 232.0 221.0 **MetalRT**


**512 @ 100** **267.6** 219.8 203.4 **MetalRT**


**1024 @ 100** **245.2** 227.8 208.0 **MetalRT**


**2048 @ 100** **230.5** 213.8 197.9 **MetalRT**


## MetalRT vs mlx-vlm and llama.cpp


MetalRT is **1.19-1.23x faster than mlx-vlm** and **1.23-1.25x faster than llama.cpp** on vision decode across all resolutions.


## What MetalRT Is Built For


Use Case Why MetalRT


**Visual assistants** 92ms to process an image and start responding


**Document understanding** Fast vision decode for OCR and document QA pipelines


**Multimodal agents** Compound latency savings across vision + language calls


**Accessibility tools** Describe images instantly, entirely on-device


**Privacy-first apps** Process sensitive images without sending them to a cloud


**Edge deployment** Real-time visual inference in offline environments


## Summary


- **279 tok/s** average vision decode
- **92ms** time-to-output (224×224)
- **1.22x faster** than mlx-vlm on vision decode
- **1.24x faster** than llama.cpp on vision decode
- **100%** decode win rate (36 out of 36 configs)


Output quality is identical across all engines. The model is the same. The speed is not.


VLM support via MetalRT will be available through[RCLI](https://github.com/RunanywhereAI/RCLI) . Stay tuned.


---


*Benchmarked on Apple M4 Max, 64GB RAM, macOS 26.3. Model: Qwen3-VL-2B-Instruct (4-bit). Greedy decoding, 5 runs, averaged. Images: COCO val2017, resized to target resolution. KV cache cleared between every run on all engines.*
