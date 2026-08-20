---
schema_version: "1.0.0"
document_id: "95ce617d344234f4fb2230a5cf315392cca58ec374a27be5c1a06688a58b020d"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:92b78fba04dbdc428f6b7823a5759283d28cbd06a28db89121349de65b0f59e0"
---

# Best STT Providers 2026: Independent Benchmarks & How to Choose

Word error rate on clean English audio has plateaued. The top providers (Deepgram Nova-3, AssemblyAI Universal-3 Pro, OpenAI gpt-4o-transcribe, ElevenLabs Scribe v2, Microsoft MAI-Transcribe-1) sit within 1-2 percentage points of each other on LibriSpeech and FLEURS. The competitive surface has shifted to streaming latency, end-of-turn detection, multilingual depth, code-switching, and cost at production scale. Vendor benchmarks still don’t tell you which one will work on your traffic.


This Coval guide covers the 2026 speech-to-text landscape across 14 providers (Deepgram, AssemblyAI, OpenAI Whisper, ElevenLabs Scribe, Microsoft Azure, Google Cloud, Amazon Transcribe, Speechmatics, Gladia, Rev.ai, NVIDIA Parakeet, Cartesia Ink-Whisper, Krisp VIVA, plus open-source Whisper hosts). It explains the metrics that actually predict production performance, walks through the 2026 model lineup with WER and latency claims, and ends with how teams run apples-to-apples comparisons against their own audio using independent measurement.


> **Key takeaways**
>
>
> - **WER plateau on clean English** (~2-3% across the top tier). Real differentiation now happens on Earnings22-style real-world audio, code-switching, low-resource languages, and end-of-turn detection latency.
> - **Microsoft launched MAI-Transcribe-1 on April 2, 2026.** Microsoft’s first proprietary STT model claims 3.8% avg WER on FLEURS across 25 languages, beats Whisper Large v3 on all 25, ships in Azure AI Foundry at ~50% lower GPU cost.
> - **OpenAI shipped GPT-Realtime-Whisper on May 7, 2026** at $0.017/min streaming, alongside refreshed Realtime-2 voice models. First time OpenAI separated streaming-optimized STT from the batch Whisper line.
> - **[Deepgram Flux Multilingual](https://deepgram.com/learn/deepgram-launches-flux-multilingual-press-release) (April 29, 2026)** is the first conversational STT with integrated end-of-turn detection. No external VAD needed. Median EOT under 300ms, saves 200-600ms on agent response time vs. STT + VAD pipelines.
> - **AssemblyAI Universal-3 Pro (Feb 3, 2026)** introduced “speech language model” architecture, with natural-language keyterm prompting up to 1,500 words, 5.6% mean WER, ranked 3rd on Artificial Analysis AA-WER v2.0 AgentTalk subset.
> - **NVIDIA Parakeet-TDT-0.6B-v3** extended Parakeet to 25 EU languages at 6.34% avg WER on the HuggingFace Open ASR Leaderboard, the strongest open-source self-host option available.
> - **Cascaded STT → LLM → TTS still wins** for transcript-required workloads (compliance, audit logging, observability, post-call analytics). Native speech-to-speech (OpenAI Realtime-2, Amazon Nova Sonic, Gemini Live) wins on latency and naturalness when the transcript isn’t load-bearing.


## What’s actually changed in STT in 2026


The STT landscape most teams remember from mid-2025 doesn’t exist anymore. Headline shifts:


- **[Microsoft MAI-Transcribe-1 (April 2, 2026)](https://microsoft.ai/news/state-of-the-art-speech-recognition-with-mai-transcribe-1/) .** First proprietary speech model from Microsoft’s MAI family (Mustafa Suleyman’s Superintelligence team). 3.8% avg WER on FLEURS across 25 languages. Shipped in Azure AI Foundry. Strategic shot at OpenAI inside Microsoft’s own cloud.
- **OpenAI GPT-Realtime-Whisper (May 7, 2026).** Dedicated low-latency streaming transcription at $0.017/min, alongside the refreshed[Realtime-2 voice models](https://openai.com/index/introducing-gpt-realtime/) . First explicit split between streaming-optimized and batch Whisper.
- **[Deepgram Flux Multilingual (April 29, 2026)](https://deepgram.com/learn/deepgram-launches-flux-multilingual-press-release) .** Conversational STT with integrated end-of-turn detection. Replaces external VAD. Median EOT detection under 300ms.
- **[AssemblyAI Universal-3 Pro (Feb 3, 2026)](https://www.assemblyai.com/blog/introducing-universal-3-pro) .** Speech language model with natural-language keyterm prompting up to 1,500 words. Streaming variant followed in March 2026.
- **ElevenLabs Scribe v2 Realtime (Nov 11, 2025).** Sub-150ms latency, 90+ languages, “negative latency” predictive streaming. Scribe v1 is deprecated.
- **Speechmatics Ursa 2.** 18% WER reduction across 55 languages vs. Ursa 1, code-switching 35% better than the nearest competitor. Powers their Flow voice-agent API at $0.0537/min.
- **NVIDIA Parakeet-TDT-0.6B-v3.** Parakeet from English-only to 25 EU languages, 6.34% avg WER on the HuggingFace Open ASR Leaderboard. The open-source self-host story is more credible than ever.
- **Cartesia Ink-Whisper (May 2025, expanded 2026).** $0.13/hr streaming-only, integrated in Vapi / LiveKit / Pipecat for the full Cartesia voice stack.
- **Krisp VIVA 2.0 (May 6, 2026).** Voice isolation in front of any STT, processing >12B minutes/year across Daily, Vapi, LiveKit, Ultravox, Telnyx. Cuts WER 10-30% on noisy audio.
- **Open-source Whisper economics keep falling.** Groq Whisper-v3 at $0.04/hr, Lemonfox at $0.17/hr, fal.ai Wizper, Replicate. 10-30× cheaper than OpenAI’s first-party Whisper endpoint for cost-sensitive workloads.


If your mental model of “best STT provider” was set before late 2025, the entire model lineup, latency frontier, and pricing curve has moved.


## Why vendor-reported STT benchmarks are unreliable


Vendor benchmarks are marketing copy with measurements attached. The numbers are real. The conditions are picked to flatter the system being measured.


**Under what conditions was the benchmark run?**


- Clean studio audio, not the real-world Earnings22 / TED-LIUM / AMI variants that surface microphone variation, accents, and conversational overlap
- Single language at a time, masking code-switching weaknesses
- Long-form audio averaged over minutes, masking streaming latency variance
- The vendor’s preferred benchmark suite (e.g., Deepgram uses its own 2,703-file proprietary benchmark; AssemblyAI publishes its own English-only set)


**Measured how?**


- “Median WER” hides P95 outliers, the worst-case experience callers actually have
- “Sub-300ms latency” without specifying TTFB vs. final-transcript vs. end-of-turn detection
- Word Error Rate alone doesn’t capture entity preservation (proper nouns, alphanumeric IDs), capitalization, or punctuation, all of which matter for downstream LLM reasoning


**Compared to what?**


- Whisper Large v3 (a 2-year-old open-source baseline) is the most common comparison, convenient because every vendor beats it
- Older competitor versions, not current flagship models
- Internal previous-version comparisons (easy wins against models the vendor controls)


Two failure modes show up most often in production. The first is **entity preservation collapse** : vendor demos transcribe general English at 95%+ accuracy, but accuracy on alphanumeric IDs (order numbers, prescription codes) and proper nouns (drug names, brand names) drops to 50-70% in many providers’ production output. The second is **code-switching degradation** : a multilingual model that posts 5% WER on monolingual benchmarks routinely posts 15-20% WER on Spanish-English or Hindi-English code-switched calls.


## The six metrics that actually matter


When buyers evaluate STT providers for production voice agents, six dimensions consistently predict whether the deployment survives contact with real traffic.


**1. Streaming latency under load.** Time-to-first-token (TTFT) and time-to-complete-transcript (TTCT) at P95, measured against your actual audio profile. P50 latency under ideal conditions is a marketing number. Production traffic at concurrent load is what your callers experience.


**2. End-of-turn detection latency.** How fast the system detects that the caller stopped talking. Pre-Flux, this required external VAD adding 200-600ms. Deepgram Flux, AssemblyAI Universal-3 Pro Streaming, and ElevenLabs Scribe v2 Realtime now ship integrated EOT, collapsing this layer.


**3. WER on your domain audio.** Standard benchmarks (LibriSpeech, FLEURS, Common Voice) don’t reflect your scripts, your callers, your accents, or your domain vocabulary. WER on your actual production audio is the only number that predicts production behavior.


**4. Entity preservation.** Alphanumeric IDs (order numbers, account numbers, prescription codes), proper nouns (drug names, brand names, street names), and code-switching (Spanish-English in healthcare, Hindi-English in financial services). WER averages don’t surface these failure modes; entity-level accuracy does. Deepgram claims 90%+ alphanumeric accuracy vs. 43-58% across competitors.


**5. Cost at production scale.** Per-hour pricing varies by ~30× across the market: Cartesia Ink-Whisper at $0.13/hr at the low end, AssemblyAI Universal-3 Pro Streaming at $0.45/hr at the upper-mid tier, premium voice-agent bundles (Deepgram Voice Agent API, AssemblyAI Voice Agent API) at $0.075/min ($4.50/hr). At scale, this is the line item that moves TCO by an order of magnitude.


**6. Multilingual depth.** Number of supported languages is one dimension; quality across those languages is another. Universal-2 supports 99 languages; Universal-3 Pro supports 6 with code-switching. Pick the model that fits your actual language mix, not the highest count.


## The 2026 STT provider lineup


Provider Flagship model (2026) Streaming latency Languages Pricing Best fit


**Deepgram** Nova-3 + Flux Multilingual (Apr 29) sub-300ms; Flux EOT ~260ms Nova-3: 30+; Flux: 10 with code-switching $0.0048–0.0078/min streaming; Voice Agent API $0.075/min Voice agents needing integrated EOT


**AssemblyAI** Universal-3 Pro + Streaming (Feb 3) 300-600ms median Universal-3 Pro: 6 with code-switching; Universal-2: 99 Async $0.21/hr; Streaming $0.45/hr NLU + transcription combined


**OpenAI** GPT-Realtime-Whisper (May 7) + gpt-4o-transcribe + Whisper Turbo sub-150ms (Realtime); Turbo 8× faster than Large v3 50+ Realtime-Whisper $0.017/min; gpt-4o-transcribe ~$0.006/min Native S2S workflows, agentic tool calling


**ElevenLabs** Scribe v2 + Scribe v2 Realtime (Nov 11 2025) sub-150ms (Realtime) 90+ Tier-based, post-May 7 reset cut STT 45% End-to-end ElevenLabs stack


**Microsoft Azure** MAI-Transcribe-1 (Apr 2) + Conversational Transcription Foundry GPU-hour billed 25 (MAI-Transcribe-1); 100+ legacy MAI: Foundry per-GPU-hour; Standard $0.36–1/hr Azure-native + global enterprise


**Google Cloud** Chirp 3 + Chirp 2 ~250-400ms 125+ $0.36/hr batch; $1/hr standard real-time GCP-native + multilingual breadth


**Amazon Transcribe** Base + Medical + Call Analytics ~250ms streaming 100+ $0.024/min ($1.44/hr) AWS-native + medical/legal SKUs


**Speechmatics** Ursa 2 + Flow sub-1s real-time 55 Flow $0.0537/min ($3.22/hr) Code-switching + on-prem


**Gladia** Solaria ~300ms 100+ $0.61/hr async, $0.75/hr real-time Real-time multilingual at moderate cost


**Rev.ai** Reverb (open-source) ~250ms 57+ (Rev API); Reverb English-focused Reverb $0.20/hr async, Turbo $0.10/hr, Whisper Fusion $0.30/hr Long-form + open-source self-host


**NVIDIA Parakeet** Parakeet-TDT-0.6B-v3 Hardware-dependent 25 EU Self-host GPU cost only Open-source self-host, cost-sensitive


**Cartesia** Ink-Whisper Streaming-optimized English + select $0.13/hr Vapi / LiveKit / Pipecat stacks


**Krisp** VIVA 2.0 (May 6) n/a (front-of-STT) n/a (audio enhancement) Enterprise contact-sales Noisy audio + voice-agent infra layer


**Open-source Whisper hosts** Whisper Large v3 / Turbo Provider-dependent 99 (Whisper) $0.04/hr (Groq) - $0.17/hr (Lemonfox) Cost-sensitive batch workloads


## Provider deep-dives


For head-to-head comparison data on your specific use case, the[Coval STT benchmark dashboard](https://benchmarks.coval.ai/stt) runs continuous independent measurement across providers.


### Deepgram


Nova-3 (Feb 11, 2025) is Deepgram’s monolingual + multilingual STT flagship; Flux Multilingual (April 29, 2026) is the conversational-speech model with integrated end-of-turn detection. Nova-3 Multilingual covers 10 languages with code-switching; the monolingual variant has expanded throughout 2025-2026 to 30+ languages with monthly additions (Thai, Cantonese, Mandarin, Indic, Turkish, Norwegian, Indonesian, etc.). Pricing: Nova-3 Monolingual $0.0048/min streaming, Multilingual $0.0058/min; Flux English $0.0065/min, Flux Multilingual $0.0078/min. Voice Agent API at $0.075/min. The differentiator: Flux’s median EOT detection under 300ms saves 200-600ms of agent response time vs. STT + external VAD. Strong choice when voice-agent turn-taking is the latency bottleneck.


### AssemblyAI


Universal-3 Pro (Feb 3, 2026) introduced “speech language model” architecture, with natural-language keyterm prompting up to 1,500 words to bias vocabulary, capitalization, and formatting. 5.6% mean WER on English benchmarks; ranked 3rd on Artificial Analysis AA-WER v2.0 AgentTalk subset at 2.3% WER. Pricing: $0.21/hr async, $0.45/hr Universal-3 Pro Streaming, Voice Agent API at $0.075/min. The audio intelligence add-ons (diarization, sentiment, entities, PII redaction, content moderation) under one API make this the right pick for teams that need ASR + structured data extraction in one call. Universal-2 (99 languages) remains in the catalog for high-language-count workloads.


### OpenAI (Whisper, Realtime-Whisper, gpt-4o-transcribe)


OpenAI’s STT story split into three tiers in 2026: **Whisper Large v3** + **Whisper Turbo** (offline batch, ~8× faster than Large v3, lower accuracy); **gpt-4o-transcribe** and **gpt-4o-mini-transcribe** (March 2025, GPT-4o-class accuracy with streaming, ~$0.006/min); and **GPT-Realtime-Whisper** (May 7, 2026, $0.017/min streaming, paired with Realtime-2 voice models). The Realtime-Whisper launch is the first time OpenAI explicitly separated low-latency streaming STT from the batch Whisper line. Trade-off: Realtime models are tied to the OpenAI stack, with no custom voice cloning and no knowledge-base support in pure Realtime mode.


### ElevenLabs (Scribe v2 / Scribe v2 Realtime)


[Scribe v2 Realtime](https://elevenlabs.io/blog/introducing-scribe-v2-realtime) (Nov 11, 2025) brought sub-150ms latency, 90+ languages, automatic language detection, and “negative latency” predictive streaming. 93.5% accuracy across 30 European + Asian languages. Scribe v1 (Feb 2025, 96.7% English / 98.7% Italian on FLEURS) is now deprecated. May 7, 2026 pricing reset cut STT pricing up to 45%. Best fit when paired with the rest of the ElevenLabs stack (Flash v2.5, Eleven v3, ElevenAgents). Detailed coverage in the[ElevenLabs review](https://www.coval.ai/blog/elevenlabs-review-2026-voice-cloning-and-synthesis-capabilities-explained) .


### Microsoft Azure (MAI-Transcribe-1)


MAI-Transcribe-1 launched April 2, 2026 in Azure AI Foundry. It’s the first proprietary speech model from Microsoft’s MAI family (Mustafa Suleyman’s Superintelligence team). Claims 3.8% average WER on FLEURS across 25 languages, beating Whisper Large v3 on all 25 and ElevenLabs Scribe v2 / gpt-4o-transcribe on 15 of 25. ~50% lower GPU cost vs. leading alternatives. Pricing follows Azure AI Foundry’s per-GPU-hour model. Strategic significance: Microsoft’s first STT model not built on OpenAI infrastructure. Strong fit for Azure-native enterprises and procurement environments with deep Microsoft commitments. Standard Conversational Transcription remains in the catalog at $0.36-1/hr for legacy workloads.


### Google Cloud Speech-to-Text


Chirp 3 (and Chirp 2 still maintained) covers 125+ languages with Google’s typical breadth. Latency in the 250-400ms band, competitive but not best-in-class. Pricing: $0.36/hr batch, $1/hr standard real-time, additional tier for medical / call-analytics variants. The right fit for teams already deep in GCP, those needing extensive language coverage, or workloads where Google’s broader speech-and-translation portfolio matters more than raw latency.


### Amazon Transcribe


AWS-native STT with three variants: Base, Medical (HIPAA-eligible), Call Analytics (sentiment + speaker labels for contact center). 100+ languages. ~250ms streaming latency. Pricing at $0.024/min ($1.44/hr) for base; Medical and Call Analytics priced higher. Closes the gap with mid-tier vendors on accuracy in 2025-2026 but remains best fit for AWS-native deployments rather than as a standalone choice on merit.


### Speechmatics


Ursa 2 delivers 18% WER reduction across 55 languages vs. Ursa 1, with sub-1s real-time latency and code-switching 35% better than the nearest competitor (per Speechmatics’ own benchmark). Powers their Flow voice-agent API at $0.0537/min ($3.22/hr). Strong on-prem deployment story for regulated industries that can’t use US-hosted services. The right pick when code-switching, on-prem, or true multilingual breadth (not just count) dominates the decision.


### Gladia


Solaria is Gladia’s current real-time STT API. 100+ languages, ~300ms latency, $0.61/hr async and $0.75/hr real-time. Strong choice for teams that need real-time multilingual at a moderate cost without committing to a hyperscaler. Developer-friendly API and pricing transparency are the brand differentiators.


### Rev.ai (Reverb)


Reverb (early 2025) is Rev’s open-source speech model, a meaningful contribution to the open-weight ecosystem from a long-standing transcription vendor. Self-hostable for cost-sensitive workloads; hosted Reverb at $0.20/hr async, Turbo at $0.10/hr, and Whisper Fusion streaming at $0.30/hr for managed simplicity. The broader Rev API covers 57+ languages; the Reverb model itself is English-focused. Particularly strong on long-form audio (podcasts, meetings, depositions) given Rev’s heritage in that domain.


### NVIDIA Parakeet


Parakeet-TDT-0.6B-v3 extended Parakeet from English-only to 25 EU languages with 6.34% average WER on the HuggingFace Open ASR Leaderboard, top of the open-source rankings. Open-weight model under permissive license, self-hosted on NVIDIA hardware. The cost story is straightforward: GPU time only, no per-minute API fees. The trade-off is operational lift. Running production-grade ASR infrastructure (WebSocket handling, VAD, hallucination mitigation, scaling) is more work than calling an API. Best fit for teams with strong ML infrastructure and high audio volume.


### Cartesia (Ink-Whisper)


Ink-Whisper is Cartesia’s streaming-optimized Whisper variant, pairing with their Sonic-3 TTS for the full Cartesia voice stack. $0.13/hr ($0.002/min), among the lowest published prices on the market. Integrated in Vapi, LiveKit, and Pipecat for one-click adoption inside those orchestration platforms. Best fit for teams already on Cartesia TTS who want a single-vendor stack, or for cost-sensitive English-first workloads.


### Krisp (VIVA 2.0)


VIVA 2.0 (May 6, 2026) is not a standalone STT model. It’s the voice isolation and audio-cleanup layer that sits in front of any STT. Processes >12B minutes/year of voice-agent traffic across Daily, Vapi, LiveKit, Ultravox, Telnyx. Cuts WER 10-30% on noisy audio (call centers, mobile, background music, multi-speaker bleed). Enterprise pricing via contact-sales. The right addition when noisy audio is the dominant source of WER degradation in production.


### Open-source Whisper hosts


For teams that want Whisper economics without operating GPU infrastructure: Groq Whisper-v3 at $0.04/hr, Lemonfox at $0.17/hr, fal.ai Wizper, Replicate. 10-30× cheaper than OpenAI’s first-party Whisper endpoint. Trade-off: less control over latency consistency, fewer enterprise compliance assurances, and dependency on the host’s uptime.


## Cascaded vs. native speech-to-speech


The 2026 architectural debate: cascaded STT → LLM → TTS, or native speech-to-speech models (OpenAI Realtime-2, Amazon Nova Sonic, Gemini Live, xAI Grok Voice Agent)?


**Cascaded wins on:**


- **Transcript availability.** Compliance, audit logging, post-call analytics, transcription-based search, redaction workflows: all require a literal transcript. S2S models don’t natively produce one (though some can be coerced into emitting transcript fragments).
- **Vendor swap-ability.** Cascaded lets you swap any layer (STT, LLM, TTS) independently. S2S locks you to one vendor’s stack.
- **Observability.** Coval and other[voice observability](https://www.coval.ai/blog/voice-observability-guide) platforms evaluate STT, LLM, and TTS separately. Cascaded makes each layer’s quality measurable; S2S makes it harder.
- **Tool calling and structured outputs.** More mature in cascaded LLM workflows than in S2S models that mix audio and tool semantics.


**Native S2S wins on:**


- **End-to-end latency.** April 2026 TTFT measurements cluster between 0.78s (Grok) and 2.98s (Gemini 3.1 Flash Live) vs. 1.5-3s for cascaded. The gap is closing as Realtime-Whisper and Flux EOT reduce cascaded latency.
- **Emotional preservation.** S2S models carry tone, prosody, and paralinguistic signal from input to output. Cascaded loses these in the STT→text→TTS handoff.
- **Naturalness on interruptions and overlaps.** Real conversation has interruptions, overlaps, hesitations. S2S handles these more gracefully than cascaded pipelines that have to coordinate three sub-components.


Most production teams in 2026 run cascaded by default, with S2S options live for use cases where latency or naturalness dominates. The choice is per-use-case, not per-platform.


## The multi-provider strategy


Production STT in voice AI typically isn’t single-vendor. The patterns that work:


**Primary + fallback.** Default traffic flows to one STT provider; a secondary takes over on latency spikes, accuracy regressions, or vendor outages. Most orchestration platforms (Vapi, Pipecat, LiveKit) support fallback chains natively.


**Traffic splitting for continuous evaluation.** Route 95% of production audio to the primary, 5% to a candidate provider. Score both against the same rubric on the same input. After a statistically meaningful sample, decide.


**Best-of-breed routing.** Premium STT (Deepgram Flux, ElevenLabs Scribe v2 Realtime, AssemblyAI Universal-3 Pro Streaming) for the conversational layer; cheap batch transcription (Whisper Turbo, Rev.ai, open-source hosts) for post-call analytics; specialized variants (Amazon Transcribe Medical, AssemblyAI Medical Mode) for domain-specific traffic.


**Voice-isolation layer.** Krisp VIVA or equivalent in front of any STT, particularly for call-center deployments where background noise, multi-speaker bleed, or mobile audio is the dominant WER driver. 10-30% WER reduction on noisy audio without changing the STT.


The orchestration cost is real. The payoff at scale is outage resilience, ongoing quality monitoring, and cost optimization across traffic segments.


## How to compare STT providers honestly


The pattern that works at production scale is independent measurement against your actual audio. Three layers, in order:


1. **A test set drawn from your real audio.** Speakerphone, regional accents, frustrated tones, code-switching, your specific scripts and vocabulary. Not the vendor’s demo voice or the LibriSpeech clean set.
2. **Multiple grading dimensions, not just WER.** WER is one input. Entity preservation (proper nouns, alphanumeric IDs), punctuation, code-switching handling, end-of-turn timing, and PII redaction quality all matter for downstream LLM reasoning. Use[language-model graders](https://docs.coval.dev/concepts/metrics/overview) that score behavioral outcomes, not just transcript fidelity.
3. **Continuous regression testing.** Every provider update (including vendor-pushed updates that don’t change the version string) runs against the same test set before it ships to production. Silent regressions on Deepgram, AssemblyAI, or Whisper turn updates are a recurring 2026 failure mode. Catch them with a[simulation suite](https://docs.coval.dev/concepts/simulations/overview) in CI/CD, not after a customer complaint.


Coval is the evaluation infrastructure layer for that pattern. Vendor-agnostic by design: the same test set runs unchanged across Deepgram Nova-3, AssemblyAI Universal-3 Pro, OpenAI Realtime-Whisper, ElevenLabs Scribe v2 Realtime, Cartesia Ink-Whisper, and the rest of the lineup, producing apples-to-apples scoring on your audio. Public head-to-head benchmarks live at[benchmarks.coval.ai/stt](https://benchmarks.coval.ai/stt) ; the methodology is documented in our[voice observability guide](https://www.coval.ai/blog/voice-observability-guide) .


Teams that build this discipline early ship faster downstream. Provider swaps move from quarter-long re-evaluation projects to overnight regression diffs, which means agents can incorporate new STT capabilities — a new Deepgram model, a new Whisper variant, a new Microsoft MAI release — as soon as they ship rather than waiting on a bake-off cycle.


## Frequently asked questions


**What’s the most accurate STT provider in 2026?**


On clean English audio, the top tier sits within ~1-2 percentage points of each other: Deepgram Nova-3, AssemblyAI Universal-3 Pro, OpenAI gpt-4o-transcribe, ElevenLabs Scribe v2, and Microsoft MAI-Transcribe-1 all cluster at 2-5% WER on standard benchmarks. Differentiation moves to real-world conditions: Earnings22-style audio, code-switching, low-resource languages, and end-of-turn detection. The honest answer to “most accurate” is “most accurate on your audio,” which requires measuring directly.


**What’s the lowest-latency STT API in 2026?**


ElevenLabs Scribe v2 Realtime publishes sub-150ms streaming latency with predictive token emission. Deepgram Flux Multilingual targets sub-300ms end-of-turn detection with integrated EOT (replacing external VAD). OpenAI Realtime-Whisper at $0.017/min targets sub-150ms TTFT. AssemblyAI Universal-3 Pro Streaming reports 300-600ms median. For voice agents, end-of-turn detection latency often matters more than raw TTFT. Flux’s integrated EOT can save 200-600ms vs. STT + external VAD.


**Should I use Whisper open-source or a hosted STT API in 2026?**


It depends on volume, ops capacity, and accuracy needs. Self-hosted Parakeet-TDT or Whisper Large v3 on NVIDIA hardware can transcribe at <$0.01/hr at sufficient scale, 10-30× cheaper than commercial APIs. But operating production-grade ASR infrastructure (WebSocket handling, VAD, fallback, scaling, hallucination mitigation) is real engineering work. For most production voice agents, hosted APIs (Deepgram, AssemblyAI, ElevenLabs, OpenAI) cost more but eliminate operational lift. The break-even depends on your audio volume and team capacity.


**What’s the difference between cascaded STT and speech-to-speech models?**


Cascaded STT → LLM → TTS processes audio through three distinct stages, producing a transcript at the STT step. Native speech-to-speech models (OpenAI Realtime-2, Amazon Nova Sonic, Gemini Live, xAI Grok Voice Agent) process audio directly into audio without a literal transcript step. S2S wins on latency and emotional preservation; cascaded wins on transcript availability, vendor swap-ability, and observability. Most production teams run both.


**How much does STT cost at production scale in 2026?**


Three buckets to model your blended per-hour rate. (1) **Premium streaming** for voice-agent traffic where latency and accuracy both matter — Deepgram Flux Multilingual, AssemblyAI Universal-3 Pro Streaming, ElevenLabs Scribe v2 Realtime, OpenAI Realtime-Whisper in the $0.30–$0.50/hr range. (2) **Batch and async transcription** for post-call analytics, call recordings, and compliance review — Deepgram Nova-3 batch, AssemblyAI Universal-2, OpenAI Whisper Turbo in the $0.10–$0.25/hr range. (3) **Self-host or aggressive-budget hosts** for high-volume, latency-tolerant workloads — NVIDIA Parakeet self-hosted or Groq Whisper-v3 at $0.04/hr. The blended rate is what to model; the headline numbers don’t predict TCO until you know the traffic mix.


**Are vendor-published STT benchmarks trustworthy for procurement?**


Three practical checks before any vendor-reported WER influences a decision. (1) **Confirm the benchmark suite.** Deepgram uses a proprietary 2,703-file set; AssemblyAI publishes English-only numbers; OpenAI tends to benchmark against older Whisper versions. Numbers on different suites aren’t directly comparable. (2) **Confirm the audio profile.** LibriSpeech and FLEURS are clean studio audio — useful for academic comparison, not for predicting how a model handles speakerphone calls with hold-music bleed. (3) **Confirm the latency tier.** Streaming WER is typically 1–3 percentage points worse than batch WER on the same model; a vendor quoting only batch numbers is comparing apples to oranges with a streaming competitor. Independent leaderboards (HuggingFace Open ASR for open-source models, Coval’s continuous[STT benchmarks](https://benchmarks.coval.ai/stt) for hosted providers) close the suite-comparability gap. Your own production-audio test set closes the rest.


**Which STT providers support speaker diarization, sentiment, and entity detection?**


AssemblyAI (built-in across the Universal lineup), Deepgram (Audio Intelligence add-on), ElevenLabs Scribe v2, Microsoft Azure (Conversational Transcription + MAI-Transcribe-1 entity extraction), Amazon Transcribe (Call Analytics), Speechmatics, and Rev.ai. The depth varies: AssemblyAI is the most feature-complete on the NLU/audio-intelligence side; Deepgram is leanest. For pure ASR without extras, OpenAI Whisper variants, Cartesia Ink-Whisper, and self-hosted Parakeet keep the surface narrow.


**How often should I re-evaluate my STT provider in 2026?**


Continuously. Vendor-pushed model updates can change accuracy or latency without changing version strings; new vendor launches arrive every quarter; pricing resets reshape unit economics overnight (ElevenLabs cut STT 45% in the May 7 reset). The May 7 OpenAI Realtime-Whisper launch and the April 29 Deepgram Flux Multilingual launch both materially shifted competitive positions in the same month. Teams running voice AI at scale build continuous evaluation into their CI/CD pipeline rather than treating STT choice as a one-time decision.


## Where to go from here


For provider-specific depth on the ElevenLabs side, the[ElevenLabs review](https://www.coval.ai/blog/elevenlabs-review-2026-voice-cloning-and-synthesis-capabilities-explained) covers Scribe v2 alongside the rest of the ElevenLabs stack. For the TTS-side companion piece, see[best TTS providers in 2026](https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)) . For Vapi as the orchestration layer that sits on top of these STT models, see the[Vapi review](https://www.coval.ai/blog/vapi-review-2026-is-this-voice-ai-platform-right-for-your-project) . For the broader voice AI model landscape, see[voice AI models in 2026](https://www.coval.ai/blog/voice-ai-models-2026) . For agent platforms that orchestrate STT and TTS, see[voice AI agent architecture](https://www.coval.ai/blog/voice-ai-agents-architecture-deployment-evaluation) .


If you want to talk through how to measure STT providers against your specific deployment,[book a call with the Coval team](https://cal.com/forms/b7126bad-a5c3-4ed0-af4b-fdfd4b268381) .
