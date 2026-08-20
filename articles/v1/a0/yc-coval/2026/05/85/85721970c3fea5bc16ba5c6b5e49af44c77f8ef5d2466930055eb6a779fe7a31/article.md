---
schema_version: "1.0.0"
document_id: "85721970c3fea5bc16ba5c6b5e49af44c77f8ef5d2466930055eb6a779fe7a31"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/benchmarks/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:342a2805f829fa83d4404c4662db73ca92fc25416c5cb72f4e177724c56f38c2"
---

# Benchmarks

In natural conversation, the gap between one person finishing a sentence and the other starting to respond averages around 200 milliseconds.


For voice agents built on cascaded pipelines (STT, LLM, tool calls, TTS), TTS is the last stage before the user hears anything, and it inherits every upstream delay.


The latency budget for TTS itself is typically 200-300ms Time to First Audio (TTFA).


As of May 14, 2026, Gradium ranks first on[Coval’s TTS latency benchmarks](https://benchmarks.coval.ai/tts) . This is not at the expense of quality metrics: Gradium provides state of the art Word Error Rate (WER).


Source:[Coval TTS benchmark](https://benchmarks.coval.ai/tts) , retrieved on May 13, 2026 at 10:30 CET.


Coval is an independent voice AI evaluation platform (YC S24); its TTS benchmark results are published at[benchmarks.coval.ai/tts](https://benchmarks.coval.ai/tts) . Gradium develops audio language models that power text-to-speech, speech-to-text, and voice cloning through a single API. This post covers the methodology and the results of Gradium, ElevenLabs flash_v2.5 and Multilingual v2, Cartesia sonic-3, Rime arcana, and OpenAI.


## What Coval is and why its TTS benchmarks matter


Coval was founded in 2024 by Brooke Hopkins, who previously built evaluation job infrastructure for self-driving at Waymo. The platform applies the testing rigor self-driving teams developed over a decade (simulation-driven testing, continuous benchmarking, trajectory-based analysis) to voice and chat AI evaluation.


Most TTS benchmarks come from the vendors themselves. The conditions get picked to flatter the system being measured: studio-quality reference text, simple inputs, controlled load, P50 numbers without spread. The measurements are real, but they are not comparable across providers, and they do not necessarily reflect production conditions.


Coval’s approach is the opposite: standardized conditions across providers, continuously updated, with methodology, and code made public. For voice agent teams, this is the closest thing the industry has to an apples-to-apples TTS comparison.


## What Coval measures today: TTFA, latency range, and TTS WER


The TTS suite measures three properties that determine whether a voice agent works in production.


**Time to First Audio (TTFA)** is the time from when a request is sent to until the first streamed chunk containing audio samples is received from the API. TTFA is reported at P50, P75, P95, and P99.


**Latency range** is the P25-P75 spread of TTFA across requests. Conversational latency is governed by both the worst case and the median. A model with 200ms P50 and 600ms P75 will feel worse than one with 250ms P50 and 300ms P75.


**Word Error Rate (WER)** for TTS is an intelligibility metric. The synthesized audio is transcribed with a reference STT model and the resulting transcript is compared to the input text. Low WER means the model articulates correctly, handles complex pronunciations cleanly, and does not hallucinate or drop words. WER on TTS is distinct from WER on STT, which measures transcription accuracy on real human speech.


## TTS benchmark results: Gradium vs. ElevenLabs, Cartesia, Rime, and OpenAI


As of May 14, 2026, Gradium ranks first on latency (P50 TTFA and P25-P75 spread) while remaining competitive with the SOTA on WER. This is consistent with their mission to deliver low-latency voice models that scale.


**Methodology:** Coval’s benchmark methodology and code are open source at[github.com/coval-ai/benchmarks](https://github.com/coval-ai/benchmarks) . The full input dataset is[published](https://github.com/coval-ai/benchmarks/blob/main/runner/src/coval_bench/datasets/manifests/tts-v1.json) and covers the kinds of utterances voice agents handle in production. Two examples:


“There’s a slight delay with your $347.89 order, but we expect it to ship by Friday afternoon.”


“Hi Ms. Garcia, your appointment with Dr. Peterson is scheduled for Tuesday, March 5th at 10:30 AM.”


## How Gradium achieves SOTA latency without trading off quality


Gradium develops audio language models (ALMs), an audio-native counterpart to LLMs. Rather than training a TTS model and stitching it into a pipeline with bespoke tuning, an ALM is trained on paired audio and text and performs multiple voice tasks within a single architecture. This paradigm was originally introduced by Gradium’s founders at Google and Meta and has since become the dominant approach across the industry.


For more details, see[Optimizing Quality vs. Latency in Real-Time TTS](https://www.coval.ai/blog/voice-ai-latency) .


## How to reproduce Coval’s TTS benchmarks


Coval’s benchmark code is open source:[github.com/coval-ai/benchmarks](https://github.com/coval-ai/benchmarks) . The suite can be run against any combination of providers, with custom audio data and test scenarios.


For voice agent teams, this is more useful than relying on either vendor-reported numbers or generic public results. Production conditions are specific to your text inputs, your latency budget, your concurrent load.


## Get started with Gradium TTS and Coval


If you’re building a voice agent and want to talk through TTS evaluation, benchmarking methodology, or how Gradium fits into your stack, we’d love to hear from you.


For partnership and enterprise enquiries, contact us atsales@coval.dev .


Reach out at **contact@gradium.ai** or visit[gradium.ai](https://gradium.ai/) .


You can also start integrating Gradium right away:[Get started](https://studio.gradium.ai/) .


To run your own TTS evaluations:[benchmarks.coval.ai/tts](https://benchmarks.coval.ai/tts) for live results,[github.com/coval-ai/benchmarks](https://github.com/coval-ai/benchmarks) for the code.


## Frequently Asked Questions


What is Coval?


Coval is an independent voice and chat AI evaluation platform, founded in 2024 by Brooke Hopkins (formerly Waymo). It provides public benchmarks for TTS and STT providers at[benchmarks.coval.ai](https://benchmarks.coval.ai/) , as well as a commercial platform for simulation, continuous monitoring, and CI/CD evaluation of voice agents in production.


Which TTS API ranks first on Coval’s benchmarks?


As of May 13, 2026, Gradium ranks first on Coval’s TTS benchmarks across two metrics: (1) P50 Time to First Audio (TTFA), (2) latency range (P25-P75 spread), and is SOTA on Word Error Rate (WER). Results are public at[benchmarks.coval.ai/tts](https://benchmarks.coval.ai/tts) .


What is Time to First Audio (TTFA)?


TTFA is the elapsed time from when a TTS request is sent to until the first streamed chunk containing audio samples is received from the API. It differs from TTFB (Time to First Byte) because the initial bytes from a TTS API are often container headers rather than audible audio. For voice agents, TTFA is the conversational-latency metric that matters.


How is WER measured for TTS?


WER for TTS measures intelligibility. The synthesized audio is transcribed with a reference STT model and the transcript is compared to the input text. The error rate captures dropped words, mispronunciations, and hallucinations. WER on TTS is distinct from WER on STT, which measures transcription accuracy on real human speech.


Where can I run my own TTS evaluations?


The Coval benchmark code is open source at[github.com/coval-ai/benchmarks](https://github.com/coval-ai/benchmarks) . It can be run against any TTS provider with custom inputs and test scenarios.


### References


Coval voice AI benchmarks:[benchmarks.coval.ai](https://benchmarks.coval.ai/) . Methodology and code:[github.com/coval-ai/benchmarks](https://github.com/coval-ai/benchmarks) .
