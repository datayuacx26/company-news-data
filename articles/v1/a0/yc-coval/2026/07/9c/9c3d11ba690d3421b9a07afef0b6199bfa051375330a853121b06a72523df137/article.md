---
schema_version: "1.0.0"
document_id: "9c3d11ba690d3421b9a07afef0b6199bfa051375330a853121b06a72523df137"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/tts-latency-benchmark-2026/"
published_at: null
first_seen_at: "2026-07-23T15:13:38.581486+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:f30c49a5d187acbc9326fe7e6282f9e3d8e1ef11e4de7ff6390d151e95f8584a"
---

# TTS Latency 2026: The Production Speed Metric for Voice AI

TTS latency is the gap between when a voice agent finishes generating a response and when the user starts hearing it. Humans respond to each other in about 400 milliseconds. That is not a soft guideline. It is the threshold below which a conversation feels natural and above which something starts to feel wrong, even if you cannot name why. A pause that is slightly too long, a response that takes just a beat too much to arrive: users register it as hesitation, confusion, or incompetence, regardless of how accurate the answer is.


In a voice agent pipeline, TTS is the last model before the user hears a response. By the time the text-to-speech model gets its input, the STT model has already transcribed the user’s speech and the LLM has already generated a response. Whatever time TTS takes comes on top of all of that. Across the models in Coval’s TTS benchmark, the median Time to First Audio is 519ms, well past the 400ms threshold. As of July 23, 2026, Palabra TTS v1 leads Coval’s[benchmark across TTS models](https://benchmarks.coval.ai/) on latency, with a median TTFA of 103ms. Here is what that number means, how we measure it, and why it matters.


> **Key Takeaways**
>
>
> - Humans respond in around 400ms. TTS latency is the last thing that eats into that budget before the user hears a response.
> - Most latency benchmarks measure throughput, not streaming. Coval measures streaming only, because that is what a call center actually runs on.
> - TTFA (Time to First Audio) is the primary TTS latency metric: how long from when the TTS model receives text to when the first audio byte arrives.
> - As of July 23, 2026, Palabra TTS v1 leads Coval’s TTS benchmark at 103ms median TTFA. Live rankings at[benchmarks.coval.ai](https://benchmarks.coval.ai/) .


---


## Table of Contents


1. Why Latency Is the Metric Nobody Gets Right
2. Why TTS Latency Matters More Than People Think
3. Throughput vs. Streaming: Why Most Benchmarks Measure the Wrong Thing
4. How Coval Measures TTS Latency: TTFA
5. July 2026 Benchmark Results: Palabra Leads on Latency
6. How to Evaluate TTS Latency for Your Use Case
7. FAQ


---


## Why Latency Is the Metric Nobody Gets Right


Ask a vendor for their latency numbers and you will get a figure. Ask two vendors for their latency numbers and you will get two figures measured in completely different ways under completely different conditions.


Throughput, first-byte latency, end-to-end latency, real-time factor: each measures something real, but none of them are the same thing. The number that matters for a production voice agent is specific: how long does a user wait between the agent finishing its reasoning and the agent starting to speak? That number is what Coval benchmarks. Everything else is a proxy.


---


## Why TTS Latency Matters More Than People Think


TTS is the last model in the pipeline before the user hears a response. That position matters.


A voice agent pipeline typically looks like this: the user speaks, the STT model returns a transcript, the LLM interprets the intent and generates a response, the TTS model converts that response into audio, and the user hears it. Each of these steps takes time. The 400ms budget for a natural-feeling conversation has to be split across all of them.


TTS latency comes last. By the time the TTS model receives its input, every other step has already consumed part of the budget. A slow TTS model arrives at the end of a chain that may already be close to the threshold, tipping the whole interaction over it.


The inverse is also true. A fast TTS model frees time across the pipeline. When TTS returns audio quickly, the total response time stays inside the 400ms window even when earlier steps have a slower moment. The buffer that keeps conversations feeling natural is largely built by fast TTS.


For a complete breakdown of end-to-end voice pipeline latency (including STT and LLM steps), see[Coval’s complete voice AI latency guide](https://www.coval.ai/blog/how-to-measure-voice-ai-latency-the-complete-guide) .


> **What low TTS latency buys you:** Not just faster audio. More slack in the pipeline for every upstream model.


---


## Throughput vs. Streaming: Why Most Benchmarks Measure the Wrong Thing


There are two distinct use cases for text-to-speech: batch generation and real-time streaming.


Batch generation is what you use to produce a completed audio file: a podcast episode, a dubbed video, an audiobook chapter. You send the full text and wait for the full audio file. Speed matters, but the user is not waiting live. According to[Artificial Analysis](https://artificialanalysis.ai/) , which measures throughput as a real-time factor (seconds of audio generated per second of compute), this is the right metric for batch work.


Real-time streaming is what a call center voice agent runs on. The LLM has finished generating a response and the agent needs to start speaking as quickly as possible so the conversation does not stall. Throughput tells you almost nothing useful here because the user is not waiting for a file. They are waiting to hear anything at all, and every millisecond before that first audio byte is a millisecond of silence in a live call.


Coval benchmarks streaming latency only, because that is the scenario that matters for voice agents. In Coval’s benchmark, the models that lead on streaming TTFA are not consistently the same models that lead on throughput rankings. The two lists diverge. A model that achieves excellent throughput on batch generation can still feel slow and unresponsive in a live call, and using a throughput benchmark to make a streaming decision is a common and costly mistake.


---


## How Coval Measures TTS Latency: TTFA


Since[Coval expanded its benchmark coverage from TTS into full STT evaluation](https://www.coval.ai/blog/new-insights-expanding-our-voice-ai-stack-benchmarks-beyond-tts) , latency has been measured alongside accuracy as a parallel evaluation dimension. For TTS, Coval uses one primary latency metric: TTFA.


**Time to First Audio (TTFA)**


TTFA measures the latency from when the TTS model receives its text input to when the first audio byte is returned. The timer starts the moment text is sent. It stops when audio begins arriving.


This is the number that directly corresponds to the gap a user experiences between the end of the LLM’s processing and the start of the agent’s voice. Low TTFA means the agent starts speaking quickly. High TTFA means there is a noticeable pause before anything is heard, regardless of how good the audio sounds once it arrives.


Coval reports TTFA at both median and p95. The median tells you typical behavior. The p95 tells you how bad the worst 5% of interactions are. For a call center running thousands of calls a day, a high p95 means a meaningful share of your users are experiencing that delay on every single call.


> **What TTFA measures:** How long between when the agent has something to say and when the user hears the first sound.


---


## July 2026 Benchmark Results: Palabra Leads on Latency


As of July 23, 2026,[Palabra TTS v1](https://palabra.ai/) leads Coval’s[TTS benchmark](https://benchmarks.coval.ai/) on TTFA. Benchmarks refresh every 30 minutes and the live ranking is always at benchmarks.coval.ai.


Model Median TTFA


Palabra TTS v1 103ms


TTS-2 (Inworld AI) 196ms


Flash v2.5 (ElevenLabs) 202ms


**What the result means:** At 103ms median TTFA, Palabra TTS v1 is producing its first audio byte in roughly a quarter of the 400ms human response window. That leaves substantial time for the upstream pipeline (STT and LLM) before the total response time becomes noticeable.


**What the gap means:** The difference between first and second place is not marginal. Palabra’s 103ms is approximately half the latency of the next closest model. At that delta, users do not need a benchmark to notice. The conversation either feels natural or it does not.


**What the result does not mean:** Latency and audio quality are separate dimensions. A model that leads on TTFA may not lead on naturalness, accuracy, or voice cloning fidelity. Before making a vendor decision on latency alone, evaluate audio quality under your specific use case conditions. For a full provider comparison, see the Coval TTS provider guide at[coval.ai/blog](https://coval.ai/blog) .


---


## How to Evaluate TTS Latency for Your Use Case


**Step 1: Establish your latency budget.** Work backwards from the 400ms target. How much time does your STT model take? How much does the LLM need? What is left for TTS? That number is your TTFA ceiling.


**Step 2: Look at p95, not just median.** Median performance is what you see in demos. p95 is what your users experience in production. If your call center runs 10,000 calls a day, a p95 of 600ms means 500 users per day are experiencing that delay every call. In Coval’s benchmark, the spread between median and p95 TTFA varies widely across providers. Some models hold close to their median at p95; others diverge by a large margin.


**Step 3: Test under your actual conditions.** Text length, punctuation patterns, and LLM output style all affect TTFA. A model that is fast on short clean sentences may slow down on the verbose, structured outputs common in enterprise LLM pipelines. Benchmark under conditions that match your deployment.


**Step 4: Test latency and quality together.** The fastest model is not useful if the audio sounds unnatural or the voice does not match your brand. Both have pass/fail thresholds. For a step-by-step evaluation framework, see[how to evaluate voice agents](https://www.coval.ai/blog/how-to-evaluate-voice-agents-a-practical-guide-to-testing-and-quality-assurance) .


Track your chosen model over time with[Coval’s Observe](https://docs.coval.ai/concepts/observe) . Providers ship silent infrastructure updates that affect latency. A model that leads now may not lead next month. For a framework on embedding latency checks into a continuous evaluation workflow, see[Coval’s voice AI regression testing guide](https://www.coval.ai/blog/voice-ai-regression-testing) .


---


## FAQ


What is TTFA?


TTFA stands for Time to First Audio: how long from when a TTS model receives its text input to when the first audio byte is returned. It is the primary latency metric for production voice agents because it directly corresponds to the pause a user experiences between the agent finishing its response and the agent beginning to speak. Unlike latency metrics that measure full audio generation, TTFA captures only the delay before streaming audio begins, which is the number that matters in real-time conversation.


Why does 400ms matter?


Humans respond to each other in roughly 400 milliseconds in natural conversation. Systems that consistently exceed this threshold feel noticeably slow to users, even when those users cannot articulate why. TTS latency is the last contributor to that total: by the time the TTS model receives input, STT and LLM processing have already consumed part of the budget. A high TTFA at the end of that chain tips the whole interaction over the threshold. The 400ms figure is not a hard cutoff, but it is the practical target for a voice agent that feels responsive rather than robotic.


Why does Coval benchmark streaming latency instead of throughput?


Throughput measures how many seconds of audio a model can generate per second of compute (a real-time factor). A model scoring 8x throughput generates 8 seconds of audio per second of compute. That sounds fast, but a model with strong throughput can still deliver a TTFA of 900ms in a live call; the two numbers are not correlated. Throughput captures batch generation efficiency; streaming latency captures real-time responsiveness. In a live call, the user is not waiting for an audio file; they are waiting for the agent to start speaking. Coval benchmarks streaming latency because that is the scenario contact center voice agents actually run in.


How often does Coval update its TTS benchmark?


Every 30 minutes. Results are surfaced across 1-day, 7-day, and 30-day rolling windows. Providers ship silent infrastructure changes that shift latency rankings, sometimes overnight. Coval’s continuous refresh means a performance change is reflected in the benchmark within the same day rather than waiting for a quarterly update.


Should I choose my TTS model based on latency alone?


No. Latency and audio quality are separate dimensions and both have pass/fail thresholds for production. A model that is fast but sounds unnatural, clips words, or fails on your voice cloning requirements is not production-ready regardless of its TTFA. Evaluate latency and quality together. See the Coval TTS provider guide at[coval.ai/blog](https://coval.ai/blog) for a cross-provider comparison on both dimensions.


What is the difference between TTS latency and STT latency?


TTS (text-to-speech) latency measures how quickly a model converts text into audio. STT (speech-to-text) latency measures how quickly a model converts audio into text. In a voice agent pipeline, STT runs first (transcribing the user’s speech), then LLM processing, then TTS (generating the agent’s spoken response). Both contribute to total response time, but they run at different points in the pipeline and are measured by different metrics. STT latency is measured by TTFS (Time to Final Segment); TTS latency is measured by TTFA (Time to First Audio).


---


*Coval builds testing and evaluation infrastructure for voice AI. Independent benchmarks, continuous production monitoring, and human review, for the teams accountable when agents go live. See the platform:[https://coval.ai](https://coval.ai/)*
