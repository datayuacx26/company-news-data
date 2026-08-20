---
schema_version: "1.0.0"
document_id: "744572aa6bf6a421619f8751800f73726918c4ea8644644967ea3fc3aaa3f8fa"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/voice-ai-tts-stt-benchmark/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:c836cc04740ddd5784ece39ac2a0e84a234501346fd0065093cba0165b80c0f5"
---

# Voice AI Benchmark: TTS and STT Results via API

Voice AI teams pick TTS and STT vendors based on demos and documentation. Both are bad inputs: demo audio is cherry-picked and provider benchmarks are run under conditions that flatter their own systems. The gap between benchmark performance and production performance is where voice agents fail.


Coval runs independent TTS and STT benchmarks across 55+ models under production-realistic conditions. Because Coval doesn’t sell TTS or STT models, every provider gets measured by the same dataset, the same infrastructure, and the same methodology. Results refresh continuously, and the full dataset is accessible via API.


## Key takeaways


- Coval’s TTS benchmark covers 24 models on Time to First Audio (TTFA) and Word Error Rate (WER); the STT benchmark covers 31 models on Time to Final Segment (TTFS) and WER
- The live leaderboards show rolling 1-day, 7-day, and 30-day results, so rankings and values change as new runs arrive
- Both benchmarks run approximately every 30 minutes; results aggregate across rolling 7-day windows
- Methodology is open-source (Apache-2.0); full results are accessible through the[Coval benchmark site](https://benchmarks.coval.ai/)


## Benchmark at a glance


TTS Benchmark STT Benchmark


**Models tested** 24 31


**Metrics** TTFA + WER TTFS + WER


**Live results**[TTS leaderboard](https://benchmarks.coval.ai/tts)[STT leaderboard](https://benchmarks.coval.ai/stt)


**Refresh cadence** Every ~30 minutes Every ~30 minutes


**Methodology** Open-source, Apache-2.0 Open-source, Apache-2.0


## Why Vendor Benchmarks Fail Voice AI Teams


Provider documentation cites accuracy numbers measured under ideal conditions like clean audio, controlled load, and often, the provider’s own test set. Those conditions have nothing to do with a production call center, a healthcare intake flow, or a financial services IVR.


Three specific problems recur when teams rely on vendor benchmarks:


1.


**Studio audio vs. production audio.** Benchmark test sets use clean studio recordings, but production calls arrive with background noise, codec compression, varied accents, and telephony artifacts. WER measured on studio audio is consistently lower than WER on production traffic, often by a wide margin.


2.


**Controlled load vs. real load.** Latency benchmarks run in isolation. TTFA spikes under concurrent load, and providers don’t publish p95 numbers that would reveal tail latency at production scale.


3.


**Point-in-time snapshots.** Providers ship silent model updates. A benchmark from last quarter may be measuring a version that no longer ships.


The practical consequence: the difference between the fastest and slowest TTS models in Coval’s benchmark is more than 1,000ms. That gap is invisible in provider documentation.


## The Coval TTS Benchmark


Coval benchmarks 24 TTS models across two metrics.


### Time to First Audio (TTFA)


The latency from sending a synthesis request until the first audio byte returns. For a voice agent, TTFA is the pause before it starts speaking. Latency above 400ms registers as noticeable to most users. TTFA predicts perceived conversational responsiveness better than throughput.


### Word Error Rate (WER)


The normalized edit distance between the original text and a transcript of the model’s audio output. It counts substitutions, deletions, and insertions, capturing garbled pronunciation, skipped words, and hallucinated content.


The[live TTS leaderboard](https://benchmarks.coval.ai/tts) shows the current latency and accuracy rankings across 1-day, 7-day, and 30-day windows. Use the window that best matches how quickly you need to detect provider drift.


No single model leads on both dimensions. Teams choosing a TTS provider make a tradeoff between latency and accuracy calibrated to their use case. A voice agent handling healthcare triage weights accuracy differently than one handling order confirmations.


For full results, see the[live TTS benchmark](https://benchmarks.coval.ai/tts) . For a deeper look at 2026 TTS providers including pricing, voice cloning, and multilingual support, see the[Coval guide to TTS providers](https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)) .


## The Coval STT Benchmark


Coval benchmarks 31 STT models across two metrics.


### Time to Final Segment (TTFS)


The latency from when a speaker finishes talking until the final transcript is returned. Tracked at both median and p95 percentiles. The median reflects typical performance; p95 reveals how often the tail affects users. For real-time voice agents where turn-taking matters, p95 is often the more important number.


### Word Error Rate (WER)


The normalized edit distance between the model transcript and the reference transcript, measured across a production-realistic audio dataset covering varied accents, noise conditions, and speaking rates.


The[live STT leaderboard](https://benchmarks.coval.ai/stt) shows the current latency and accuracy rankings across 1-day, 7-day, and 30-day windows. Teams building real-time voice agents where end-of-turn detection matters should look at p95 alongside medians rather than relying on one headline value.


For a full breakdown of 2026 STT providers including streaming support, multilingual accuracy, and pricing, see the[Coval guide to STT providers](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) .


## How the Benchmarks Work


Both benchmarks run against the same pinned dataset under the same conditions across all models. No provider gets a custom test set or controlled environment. Every model is measured the same way.


The methodology and code are open-source under[Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0) . The full test suite is reproducible by any team that wants to run their own version.


**Refresh cadence:** Both benchmarks run approximately every 30 minutes. Results aggregate across rolling 7-day windows to smooth transient spikes while keeping data current. Results are also syndicated daily to Openfunnel’s Openbenchmarks platform, where they power the independent[TTS](https://openbenchmarks.com/text-to-speech-benchmark-by-coval) and[STT](https://openbenchmarks.com/speech-to-text-benchmark-by-coval) leaderboards.


**Why continuous refresh matters:** Model performance isn’t static. Providers ship silent updates and servers experience different loads throughout the day. A model call at 9am PST may be much slower than at 11pm PST. When a provider’s latency increases after an infrastructure change or server slowdown, the benchmark reflects it within the same day.


**Why neutrality matters:** Coval builds testing and evaluation infrastructure for voice AI teams. It doesn’t sell TTS or STT models, which is what makes neutral benchmarking possible. There’s no commercial relationship with any provider that changes the numbers.


## Access via API


Benchmark results are available via the Coval API. Teams can pull current TTS and STT performance data directly into their own tooling rather than checking the benchmark dashboard manually.


1.


**Model selection pipelines.** Query current benchmark results as part of a vendor evaluation. Pass or fail a candidate provider based on whether it meets your TTFA and WER thresholds, automated rather than manual.


2.


**CI/CD integration.** Gate deployments on whether your chosen model still ranks within acceptable performance bounds. If a provider’s latency increases after a silent infrastructure change, catch it before it reaches production traffic.


3.


**Production monitoring.** Track how your vendor’s benchmark performance changes over time alongside your own quality metrics from Coval’s[Observe feature](https://docs.coval.ai/concepts/observe) . Benchmark drift and production drift together give a more complete picture than either alone.


Get access through the[Coval benchmark site](https://benchmarks.coval.ai/) .


## What These Benchmarks Don’t Measure


Being precise about scope matters.


**Voice naturalness and quality.** Perceived naturalness is real and important for TTS. It’s also subjective and can’t be automated at scale without human listening panels. Until that’s measured independently, naturalness claims from providers are their own assertion. The most reliable way to evaluate naturalness for your use case is to run structured listening tests on your own audio with a representative sample of users.


**Pricing.** Provider pricing changes frequently and varies by volume tier. These benchmarks don’t track it. Compare pricing directly with providers.


Results are also point-in-time relative to the model version running during each benchmark execution. A model that leads today may rank differently after a provider update. The 30-minute refresh cadence reduces this risk but doesn’t eliminate it.


## Frequently Asked Questions


How often do the Coval TTS and STT benchmarks update?


Both benchmarks run approximately every 30 minutes. Results aggregate across rolling 7-day windows to smooth transient spikes while staying current. When a provider ships an update that changes performance, the benchmark reflects it within the same day. There’s no quarterly refresh cycle or snapshot schedule.


What is Time to First Audio (TTFA) and why does it matter?


TTFA is the latency from sending a text-to-speech synthesis request until the first audible moment in the returned audio. In a voice agent conversation, it’s the pause before the agent speaks. Most users register latency above 400ms as noticeable. TTFA predicts user experience better than throughput metrics because it measures the waiting moment in a conversation directly.


What is the difference between TTFA and TTFS?


TTFA (Time to First Audio) is a TTS metric: the latency until the first audio byte returns from a synthesis request. TTFS (Time to Final Segment) is an STT metric: the latency from when a speaker finishes talking until the final transcript is returned. Both measure the waiting pause in a voice conversation, from opposite sides: one before the agent speaks, one before the agent processes what a human said.


How does Coval measure Word Error Rate (WER) for TTS?


For the TTS benchmark, WER is measured by sending text to a TTS model, capturing the audio output, transcribing that audio through a reference STT model, and comparing the resulting transcript to the original input text. This catches pronunciation errors, skipped words, and hallucinated content in synthesized speech. The full methodology is open-source under Apache-2.0.


Which speech-to-text model is most accurate in current benchmarks?


Rankings shift as new runs arrive and providers update models. See the[live STT leaderboard](https://benchmarks.coval.ai/stt) for the current accuracy ranking and choose the 1-day, 7-day, or 30-day window that matches your evaluation horizon.


Can I access benchmark results via API?


Yes. Benchmark results are available via the Coval API. Teams use the API to pull current TTS and STT performance data into model selection pipelines, CI/CD quality gates, and production monitoring dashboards. See the[Coval benchmark site](https://benchmarks.coval.ai/) for API access details.


Why doesn’t the benchmark measure voice naturalness or pricing?


Naturalness requires human listening panels to measure reliably. Automating it at scale introduces subjective variation that makes cross-provider comparison unreliable. For naturalness evaluation, the most defensible approach is a structured listening test with real users on your actual audio. Pricing is excluded for a different reason: it changes too frequently and varies too much by volume tier to track usefully in an automated benchmark. Get quotes directly from providers and negotiate based on your projected call volume.


*Coval builds testing and evaluation infrastructure for voice AI. Independent benchmarks, continuous production monitoring, and human review, for the teams accountable when agents go live.[See the Coval platform](https://coval.ai/) .*
