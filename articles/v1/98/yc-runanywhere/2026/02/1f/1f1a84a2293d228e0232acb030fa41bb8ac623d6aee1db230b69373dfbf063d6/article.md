---
schema_version: "1.0.0"
document_id: "1f1a84a2293d228e0232acb030fa41bb8ac623d6aee1db230b69373dfbf063d6"
company_key: "yc-runanywhere"
company: "RunAnywhere"
source_id: "yc-runanywhere-news-import-450c102900e1"
canonical_url: "https://www.runanywhere.ai/blog/fastvoice-on-device-voice-ai-pipeline-apple-silicon"
published_at: "2026-02-22T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:59.131595+00:00"
fetched_at: "2026-07-28T22:19:30.840186+00:00"
content_hash: "sha256:a046e75557558a454ee121b792c10d1a50ca3400b48322565654af59640e064c"
---

# FastVoice: 63ms First-Audio Latency for On-Device Voice AI on Apple Silicon

How fast can a voice AI respond if it never leaves your device?


**63 milliseconds.** From the moment you stop speaking to the moment you hear the first word back. That's faster than a human blink.


We built FastVoice — a fully on-device voice AI pipeline in C++ that composes speech recognition, LLM inference, and text-to-speech on Apple Silicon. No cloud APIs. No network round-trips. No privacy compromises.


This post walks through the system architecture, the four optimizations that got us there, and the ablation study that proves each one matters.


## Why First-Audio Latency?


Most LLM benchmarks measure **throughput** — tokens per second. That metric is wrong for voice.


In a voice conversation, what the user perceives is **how long they wait before hearing a response** . Research shows the human perceptual threshold for conversational responsiveness is **200–500ms** . Anything under 200ms feels instant.


We call this metric **first-audio latency** : the time from end of user speech to first audible word of the response.


What makes up first-audio latency


The formal decomposition:


text


```text
1  L_first = T_vad + T_asr + T_ttft + T_gen(k) + T_tts(c₁)
```


The key lever is **k** — the number of tokens generated before TTS has enough text to speak. At 205 tok/s, each extra token adds ~4.9ms. Minimizing k is the game.


## The Pipeline


FastVoice is a multi-stage streaming pipeline. Each stage runs on a dedicated OS thread, connected by lock-free ring buffers.


STT → LLM → TTS streaming pipeline Stage Component Details


**VAD** Silero VAD Voice activity detection, speech segmentation


**STT** Whisper / Moonshine Via sherpa-onnx, CoreAudio integration


**LLM** Qwen3 0.6B (Q4_K_M) llama.cpp, Metal GPU, KV cache Q8_0


**TTS** Piper VITS (Amy medium) 1.1x speed scaling, word-level flushing


**Audio** CoreAudio Zero-allocation real-time output


The entire system uses a pre-allocated **64MB memory pool** backed by OS-level 2MB superpages with bump allocation and 64-byte cache-line alignment. No malloc in the hot path. No GC pauses. No jitter.


## Baseline: Where Does the Time Go?


Before any optimizations, here's where first-audio latency comes from:


Baseline latency decomposition by stage Stage Latency Share


VAD 4.8ms 6%


STT (ASR) 37.4ms 46%


LLM TTFT 13.1ms 16%


LLM gen → first sentence ~25ms 31%


TTS (first chunk) ~151ms —


**First-audio** **80.6ms** —


STT dominates at 46%. But the real opportunity is in **how we bridge LLM generation and TTS** — the streaming granularity.


## The Four Optimizations


We applied four optimizations, each ablated independently.


### 1. KV Cache Quantization (FP16 → Q8_0)


Standard KV caches store attention states at FP16 (2 bytes per element). We quantize to Q8_0 (1.0625 bytes per element).


KV cache memory: FP16 vs Q8_0 Metric FP16 Q8_0 Change


KV cache memory 224 MB 119 MB **-47%**


Long-tier first-audio 82.9ms 69.7ms **-16%**


Decode throughput 205 tok/s 194 tok/s -5.6%


The 5.6% throughput regression is worth it. On Apple Silicon's unified memory, reducing KV cache size **directly reduces GPU memory pressure** , which improves latency across the entire pipeline. This cross-component interaction is invisible if you benchmark the LLM in isolation.


### 2. Word-Level Streaming Flush


This is the big one.


Standard TTS pipelines wait for a complete sentence before synthesizing speech. That means the LLM must generate enough tokens to form an entire sentence — potentially 15–30 tokens — before the user hears anything.


Word-level flushing changes the rule: **emit text to TTS after N=7 words** , even without a sentence boundary.


First-audio latency: sentence flush vs word flush (N=7) Response Tier Sentence Flush Word Flush (N=7) Change


Short 54.2ms 55.0ms +1.5%


Medium 63.5ms 62.8ms -1.1%


Long 82.9ms 63.1ms **-24%**


Longer 76.2ms 66.4ms **-13%**


Longest 77.1ms 67.7ms **-12%**


The critical insight: word-level flushing makes first-audio latency **nearly constant** (~55–77ms) regardless of how long the LLM's response is. Without it, longer responses mean longer waits.


### Word Count Threshold Sweep


We swept N from 1 to ∞ to find the optimal threshold:


First-audio latency across word flush thresholds Threshold (N) First-Audio (Long) Notes


1 ~50ms Fragmented, robotic prosody


3 ~55ms Better, still choppy


**7** **63.1ms** **Sweet spot for VITS models**


10 ~70ms Marginal benefit over sentence


∞ (sentence) 82.9ms Standard approach


N=7 hits the quality-latency Pareto frontier: low enough to start TTS early, high enough that Piper produces natural prosody.


### 3. System Prompt KV Cache


The system prompt is identical across all user queries. We cache its KV state at initialization and restore it per query instead of re-evaluating.


Metric Without Cache With Cache Change


TTFT 15.6ms 13.1ms **-16%**


Small but free. Every millisecond matters in voice.


### 4. Asynchronous LLM–TTS Pipeline


In the baseline, TTS synthesis **blocks** the LLM decode thread. While Piper synthesizes one sentence (~147ms), the LLM sits idle.


We refactored to a producer-consumer queue: the LLM pushes text chunks to a queue, and a dedicated TTS worker thread consumes them independently.


Synchronous vs asynchronous LLM-TTS pipeline


For a 6-sentence response, this recovers **~882ms of blocked LLM generation time** . The LLM never waits for TTS again.


## Cumulative Results


All four optimizations combined:


First-audio latency: baseline vs fully optimized Metric Baseline Optimized Change


First-audio (Long) 80.6ms 63.1ms **-22%**


KV cache memory 224 MB 119 MB **-47%**


First-audio range 54–83ms 55–77ms Near-constant


Tool-call accuracy 100% 100% Preserved


**63ms first-audio.** Well under the 200ms perceptual threshold. The user hears a response before they consciously register that they've stopped speaking.


## Model Scale: What's Fast Enough?


We tested across three Qwen3 model sizes:


Model First-Audio Decode Speed Meets <200ms?


**0.6B** **63.1ms** 194 tok/s **Yes**


1.7B 148ms 89 tok/s Yes (barely)


4B 326ms 42 tok/s No


On current Apple Silicon, only sub-1B models achieve truly instant voice interaction. As hardware improves, larger models will cross the threshold.


## What This Means


FastVoice proves that **cloud-competitive voice AI is possible entirely on-device** :


- **Privacy** : Audio and text never leave the device
- **Offline** : Works without any network connection
- **Cost** : Zero API calls, zero per-query charges
- **Latency** : 63ms — faster than any cloud round-trip


The pipeline handles tool calling with 100% correctness, enabling on-device voice agents that can check weather, control smart home devices, or query databases — all without touching a server.


## Summary


- **63ms** first-audio latency (22% reduction from baseline)
- **47%** KV cache memory reduction (224MB → 119MB)
- **Near-constant** first-audio (~55–77ms) regardless of response length
- **100%** tool-call correctness across all configurations
- **Zero** cloud dependencies


The bottleneck in voice AI isn't the model. It's the pipeline. FastVoice shows what happens when you optimize the full stack — from VAD to audio output — as a single, streaming C++ system on Apple Silicon.


---


*Evaluated on Apple Silicon M3 Max, 36GB unified memory. Model: Qwen3 0.6B Q4_K_M with Q8_0 KV cache. STT: Whisper tiny.en via sherpa-onnx. TTS: Piper Amy medium at 1.1x speed. File-mode benchmarks, 5 runs, median reported. Word flush threshold N=7.*
