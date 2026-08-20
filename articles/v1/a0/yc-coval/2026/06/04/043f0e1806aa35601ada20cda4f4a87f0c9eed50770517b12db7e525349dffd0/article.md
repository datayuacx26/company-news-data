---
schema_version: "1.0.0"
document_id: "043f0e1806aa35601ada20cda4f4a87f0c9eed50770517b12db7e525349dffd0"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:183e9aef56f59e6771adc4caab2e354766a60701b25ee600f25ec6f1041f3d02"
---

# Best TTS Providers 2026: Why Vendor Benchmarks Lie

Every text-to-speech provider claims to be the most natural, the fastest, and the most affordable. The benchmarks they publish are real measurements under conditions that flatter the system being measured. Production traffic looks nothing like those conditions, which is why teams that rely on vendor-reported numbers end up with agents that sound great in a demo and fall apart on hold-music bleed-through, regional accents, and frustrated callers.


This Coval guide covers the 2026 TTS landscape across 14 providers — ElevenLabs, Cartesia, OpenAI, Deepgram, Microsoft Azure, Google Cloud, Amazon Polly, PlayAI, LMNT, Rime, MiniMax, Inworld, Hume, and Sesame — plus low-cost newcomers like Vapi Voices Beta. It explains why vendor-reported benchmarks are unreliable for production planning, the six metrics that actually predict whether an agent will hold up at scale, and how to run apples-to-apples comparisons against your own traffic using independent measurement.


> **Key takeaways**
>
>
> - The 2026 TTS market split into three tiers: expressive offline models (ElevenLabs Eleven v3, Inworld Realtime TTS, Hume Octave), real-time agent models (Flash v2.5, Cartesia Sonic 3, Deepgram Aura-2, Rime Coda), and high-volume cheap models ([Vapi Voices Beta at $0.0025/min](https://vapi.ai/blog/vapi-voices-beta) , Polly Standard, Azure Neural).
> - **Latency is no longer the differentiator at the top of the market.** Cartesia, Deepgram, Rime, and ElevenLabs Flash v2.5 all publish sub-100ms TTFB. The competitive surface has shifted to emotional control, prosody, multilingual fidelity, and cost.
> - **OpenAI shipped Realtime-2 + Realtime-Translate + Realtime-Whisper on May 7, 2026** , collapsing STT and TTS into one speech-to-speech model with GPT-5-class reasoning. New voices Cedar and Marin are exclusive to Realtime-2.
> - **PlayHT became PlayAI after Meta’s July 2025 acquisition** and is being wound down by Meta. Treat existing PlayHT integrations as deprecation-risk.
> - **Vendor-reported benchmarks aren’t usable for procurement decisions.** Independent measurement against your own use case is the only reliable comparator. Coval publishes head-to-head[TTS benchmarks](https://benchmarks.coval.ai/tts) and[STT benchmarks](https://benchmarks.coval.ai/stt) that update continuously.
> - Multi-provider strategies (primary + fallback, traffic splitting, best-of-breed routing) are the production-grade pattern for teams that can’t tolerate a single-vendor regression.


## What’s actually changed in TTS in 2026


The TTS landscape that most teams remember from mid-2025 doesn’t exist anymore. The headline shifts:


- **[ElevenLabs Eleven v3 went GA on Feb 2, 2026](https://elevenlabs.io/blog/eleven-v3-is-now-generally-available)** with audio tags, multi-speaker dialogue, and 74 languages. ElevenLabs crossed[$500M ARR in May 2026](https://elevenlabs.io/blog/500m-arr-and-new-investors) after a[$500M Series D in February](https://elevenlabs.io/blog/series-d) .
- **Cartesia raised $100M in late 2025** (Kleiner Perkins, Index, Lightspeed, NVIDIA) and shipped Sonic-3 in April 2026 with a Sonic-3.5 update rolling out in May.
- **OpenAI shipped Realtime-2 on May 7, 2026** — native speech-to-speech with GPT-5-class reasoning. The earlier` gpt-realtime-2025-08-28` model is now superseded.
- **Vapi Voices Beta** launched December 2025 at $0.0025/min, collapsing the TTS line item to near-zero for base-case use cases (status updates, appointment reminders, IVR routing).
- **Microsoft launched MAI-Voice-1 in April 2026** — the first TTS model from Microsoft’s Superintelligence team (Mustafa Suleyman), marking a strategic move beyond OpenAI dependency.
- **PlayHT was acquired by Meta in July 2025** , rebranded PlayAI, and is reportedly being wound down. Production teams should plan migrations.
- **Sesame open-sourced CSM-1B under Apache 2.0** in April 2026, then pivoted toward voice-companion + eyewear hardware rather than competing as a pure TTS API.
- **The May 7 ElevenLabs pricing reset** cut TTS prices up to 55% and added pay-as-you-go for self-serve developers, triggering a broader market price compression.
- **AIUC-1 certification** (Feb 2026, ElevenAgents was the first to achieve it) introduced enterprise-insurable AI voice agents, which is now a procurement unlock for regulated industries.


If your mental model of “best TTS provider” was set before late 2025, almost every data point that drives the decision has changed.


## Why vendor-reported TTS benchmarks are unreliable


Vendor benchmarks are marketing copy with measurements attached. The numbers are real. The conditions are picked to flatter the system being measured.


**Under what conditions was the benchmark run?**


- Shortest possible text samples (which masks streaming latency on long generations)
- Ideal network conditions (which masks regional latency variance)
- A single voice model that the vendor optimized hardest for (which masks degradation on the long tail of voices)
- Lab inference (which masks load behavior at production concurrency)


**Measured how?**


- “Latency” often means time-to-first-byte rather than total voice-to-voice. The two can differ by 300–500ms on cascaded stacks.
- Naturalness scores rely on MOS panels that vary in cultural background, sample selection, and rubric design.
- “Sub-150ms” without a percentile (P50? P95? P99?) hides the tail. P99 latency is what callers experience on a bad day.


**Compared to what?**


- Previous in-house version (easy win against a prior model the vendor controls)
- Strategically chosen competitor models (often older or simpler)
- Human speech (a subjective comparison without a ground truth)


Two specific gaps surface most often in production. The first is **prosody and naturalness on domain vocabulary** : vendor demos sound great on general English but degrade meaningfully on medication names, alphanumeric IDs, or industry jargon. The second is **latency tail behavior** : P50 numbers look identical across providers, but P95 and P99 (the experience callers actually have on a bad day) can vary by 200ms or more between vendors that publish the same headline figure.


## The six metrics that actually matter


When buyers evaluate TTS providers for production voice agents, six dimensions consistently predict whether the deployment will survive contact with real traffic.


**1. Latency under load.** Time-to-first-byte (TTFB) at P95 and P99, measured against the audio profile you actually serve. Vendor-claimed P50 latency is a marketing number. What matters is the worst-case experience under concurrent load.


**2. Naturalness on your content.** MOS scores generalize poorly. The voice that sounds perfect reading a vendor’s demo script can sound robotic reading your insurance scripts, medication names, or alphanumeric order IDs. Naturalness measurement must use your text.


**3. Prosody and emotional control.** Whether the voice carries appropriate inflection on questions, urgency on alerts, empathy on complaints. Eleven v3 introduced audio tags (` \[whispers\]` ,` \[sighs\]` ) for explicit control; Hume Octave models emotion natively; OpenAI Realtime adjusts emotional register from conversational context. Buyer guides that ignore this dimension overweight raw audio quality.


**4. Reliability and consistency.** Voice drift across long sessions (60+ seconds), pronunciation consistency on domain vocabulary, and uptime across regions. Cartesia Sonic-3.5 specifically addressed long-session voice drift, which had been a recurring complaint on the earlier model.


**5. Cost at production scale.** Per-million-character pricing varies by 25x across the market (Polly Standard at $4 vs. MiniMax Speech-02 HD at ~$100). At scale, the choice of provider can move TCO by an order of magnitude. The trade-off is naturalness and feature coverage.


**6. Voice quality consistency over time.** Vendor-pushed model updates that don’t change the version string create silent regressions. Continuous monitoring catches these; one-shot benchmarks don’t.


## The 2026 TTS provider lineup


The fourteen providers below cover the production-grade TTS market in 2026. Pricing reflects the most recent published numbers as of May 2026; per-minute rates assume conversational pacing of ~150 characters/sec.


Provider Flagship model (2026) TTFB Languages Pricing / 1M chars Voice cloning Best fit


**ElevenLabs** Eleven v3 (Feb 2 GA) + Flash v2.5 ~75ms (Flash) 74 (v3) / 32 (Flash) $50 (Flash, post-May 7 reset) IVC + PVC Cinematic content + production agents


**Cartesia** Sonic-3 / Sonic-3.5 ~90ms (40ms Turbo) 42 Pro $5/mo, Scale $299/mo (8M credits) Pro cloning Real-time agents with sub-100ms budget


**OpenAI** Realtime-2 + tts-1-hd <300ms (S2S) 50+ $32/1M audio in, $64/1M out (~$0.18–$0.46/min) Preset voices only Agentic workflows with tool calling


**Deepgram** Aura-2 ~200ms streaming 7 majors + extended $30 Pre-set voices Call center IVR + alphanumeric-heavy stacks


**Microsoft Azure** Neural + Neural HD + MAI-Voice-1 ~300–500ms 140+ $16 Neural / $22 HD Custom Neural Voice (gated) Global enterprise + data sovereignty


**Google Cloud** Chirp 3 HD + Studio + WaveNet ~250–400ms 380+ voices, 50+ langs $30 (Chirp 3 HD) / $160 (Studio) Custom voices (limited) Multilingual + GCP-native stacks


**Amazon Polly** Neural + Generative ~250–400ms 60+ $4 (Standard) / $16 (Neural) / $30 (Gen) Limited AWS-native + cost-sensitive workloads


**PlayAI** (was PlayHT) Play 3.0 + Play 3.0 Mini ~150ms (Mini) 50+ $39/mo Creator, $99/mo Pro; Enterprise contact sales Yes Migration-risk — Meta acquired, winding down


**LMNT** Aurora ~140ms 30+ $35 Yes (instant) English-first real-time agents


**Rime** Coda (May 2026) + Mistv2 sub-100ms English-focused, expanding Tier-based Yes Conversational US English at speed


**MiniMax** Speech-02 family (HD + Turbo) ~150ms 30+ (Chinese-strong) ~$100 (Speech-02 HD) Yes China-region deployments + multilingual


**Inworld** Realtime TTS 1.5 Max ~180ms 28 Per-tier Yes (gamedev focus) Interactive game/companion voices


**Hume** Octave 2 + EVI 3 ~150–200ms English + select $50–150 per tier Yes Emotion-critical interactions (support, mental health)


**Sesame** CSM-1B (open-source) + closed CSM-3B/8B Variable English (20+ planned) Self-host (open) Limited Open-source + voice companion hardware


Plus **Vapi Voices Beta** as a high-volume budget option at **$0.0025/min** , accessible only inside the Vapi orchestration platform (not a standalone API).


## Provider deep-dives


The provider entries below cover the trade-offs that decide which is the right fit. For head-to-head comparison data on your specific use case, the[Coval TTS benchmark dashboard](https://benchmarks.coval.ai/tts) runs continuous independent measurement across providers.


### ElevenLabs


Eleven v3 (Feb 2, 2026 GA) is the most expressive model ElevenLabs has shipped. Audio tags, multi-speaker dialogue, and 70+ languages, but it’s not real-time, and the 5,000-character cap per request limits use to offline content. Flash v2.5 is the real-time variant (~75ms TTFB, 32 languages, 40,000-character cap) that voice agents actually run. The May 7 pricing reset cut TTS prices up to 55%; Business tier offers TTS at roughly 5¢/minute. ElevenLabs is also the first AI voice company to earn AIUC-1 certification, which matters for regulated procurement. Detailed coverage in the[ElevenLabs review](https://www.coval.ai/blog/elevenlabs-review-2026-voice-cloning-and-synthesis-capabilities-explained) .


### Cartesia


Cartesia’s State Space Model architecture gives it the lowest published TTFB in the field at ~90ms on Sonic-3, ~40ms on the Turbo variant. Sonic-3 went GA April 2026; Sonic-3.5 began rolling out in May with fixes for long-session voice drift and step-change multilingual improvements (Hebrew, Japanese, Spanish, Hindi, German, Korean, French). 42 languages, professional cloning supported. $100M raised in late 2025 led by Kleiner Perkins with Index, Lightspeed, and NVIDIA. Strong choice when latency budget is the hard constraint. Comparison details in[ElevenLabs vs. Cartesia](https://www.coval.ai/blog/elevenlabs-vs-cartesia-which-tts-provider-is-right-for-your-voice-ai-project) .


### OpenAI


The May 7, 2026 launch of Realtime-2, Realtime-Translate, and Realtime-Whisper supersedes the previous` gpt-realtime-2025-08-28` model. Realtime-2 collapses STT and TTS into native speech-to-speech with GPT-5-class reasoning. Two exclusive voices, Cedar and Marin, joined the existing alloy / echo / shimmer / marin / cedar lineup. Trade-off: no knowledge-base support with Realtime, no custom voice cloning, preset voices only. For offline content, tts-1-hd and gpt-4o-mini-tts remain available (gpt-4o-mini-tts at ~$0.015/min is one of the cheapest premium options on the market). Realtime is the right choice for agentic workflows where tool calling, mid-sentence interruption, and emotional adaptation matter more than brand-specific voice.


### Deepgram


Aura-2 (April 2025) pairs natively with Deepgram’s Nova-3 STT and Flux STT on the same enterprise runtime, which reduces pipeline hops and total voice-to-voice latency. Aura-2 publishes ~200ms streaming TTFB at $30/1M chars. Deepgram’s edge is alphanumeric and domain-vocabulary accuracy (Aura-2 claims 90%+ on alphanumeric content versus 43–58% across competitors), which makes it the right fit for IVR, call center, and any workflow where order IDs, phone numbers, and medication names dominate the script. Naturalness on cinematic content trails ElevenLabs and Inworld.


### Microsoft Azure AI Speech


Unmatched language breadth (140+ languages, 500+ voices, expanding to 700+ with Dragon HD Omni) and the strongest enterprise compliance posture (SOC 2, ISO, data sovereignty, HIPAA on Azure subscription). Standard Neural Voice at $16/1M chars, Neural HD at $22 (cut from $30 in March 2026). MAI-Voice-1 launched April 2026 from Microsoft’s Superintelligence team. It’s the first Microsoft TTS model not built on OpenAI infrastructure. Custom Neural Voice (Professional) and Personal Voice (Instant) are gated behind Microsoft’s Responsible AI review. The right fit when global language coverage, data residency, or Fortune 500 compliance dominates the decision.


### Google Cloud Text-to-Speech


Chirp 3 HD voices launched in 2025 with stronger naturalness than the older WaveNet lineup. 380+ voices across 50+ languages. Studio voices (premium) at $160/1M chars; Chirp 3 HD at $30; standard WaveNet/Neural2 at $16. Custom voice creation supported but gated. Best fit for teams already deeply integrated with GCP or whose multilingual coverage requirements outstrip ElevenLabs / Azure.


### Amazon Polly


The cheapest path to TTS at scale. Polly Standard at $4/1M chars (~3% of OpenAI Realtime’s effective per-minute cost), Neural at $16, Generative at $30, Long-Form at $100. Speech quality on Generative voices closed the gap with mid-tier premium providers in 2025. Strong for AWS-native deployments and high-volume / cost-sensitive applications. Less competitive at the top of the naturalness or emotional-control market.


### PlayAI (formerly PlayHT)


Play 3.0 and Play 3.0 Mini remain in production for existing PlayHT customers. Meta acquired the company in July 2025 and rebranded it PlayAI; reports indicate Meta is winding down the standalone API in favor of integrating the technology into Meta’s own products. **Treat any new PlayHT integration as deprecation-risk** and plan migration to Cartesia, LMNT, or ElevenLabs Flash v2.5 for similar latency / language profiles.


### LMNT


LMNT Aurora targets the real-time agent space with ~140ms TTFB and a clean instant-cloning workflow. 30+ languages, $35/1M chars. Strong English performance; multilingual breadth is narrower than ElevenLabs or Azure. Best fit for English-first conversational agents where simple pricing and a tight API beat the larger vendors’ configurability.


### Rime


Rime shipped Coda in May 2026 (the company’s flagship for the year) alongside the existing Mistv2 model. Both publish sub-100ms TTFB. The positioning is conversational US English at speed: accents, dialects, and informal speech patterns that traditional TTS handles poorly. The right pick for US-market voice agents where authentic conversational delivery matters more than international language coverage.


### MiniMax


Speech-02 (HD and Turbo) is the flagship family from MiniMax, with particularly strong Chinese performance and aggressive global expansion through 2025–2026. Speech-02 HD at ~$100/1M chars sits in the premium tier; Turbo is cheaper but trades naturalness. Voice cloning supported. The right choice for China-region deployments, multilingual products targeting Asian markets, or workloads where Chinese phonetic accuracy is a hard requirement.


### Inworld


Inworld’s Realtime TTS 1.5 Max consistently tops the Artificial Analysis TTS leaderboard on Elo scores. ~180ms TTFB, 28 languages. The product is built around interactive game and companion-AI use cases, so the developer ergonomics favor character voices, emotional range, and consistent persona across long sessions. The right fit when the agent is a persona (game character, AI companion, branded virtual host) rather than a utility-routing IVR.


### Hume


Octave 2 and EVI 3 (Empathic Voice Interface) ship native emotional understanding: the model adjusts delivery based on the caller’s detected affect. 150–200ms TTFB. $50–150/1M chars depending on tier. Best fit for high-stakes interactions where misjudging emotional register has real cost: mental-health triage, customer recovery on complaint calls, healthcare empathy scripts.


### Sesame


Sesame open-sourced CSM-1B under Apache 2.0 in April 2026, with CSM-3B and CSM-8B powering the closed commercial offering. English-only with 20+ languages planned. Sesame’s strategic pivot is toward voice-companion software + eyewear hardware rather than competing as a pure TTS API for voice agents. The right interest level for teams that want a self-hostable open-source baseline or are watching the consumer companion-AI category. Not a primary choice for production voice-agent TTS in 2026.


### Vapi Voices Beta


Not a standalone API; accessible only inside the Vapi orchestration platform. $0.0025/min collapses the TTS cost line item to near-zero for base-case use cases: appointment reminders, verification calls, IVR routing, status notifications. Naturalness is intentionally below premium ElevenLabs / Cartesia / Inworld; teams use Vapi Voices for high-volume / low-stakes interactions and route to premium providers for naturalness-critical conversations. Coverage in the[Vapi review](https://www.coval.ai/blog/vapi-review-2026-is-this-voice-ai-platform-right-for-your-project) .


## The multi-provider strategy


Production voice AI teams that operate at scale rarely run a single TTS provider. The patterns that work:


**Primary + fallback.** One vendor handles default traffic, a second takes over on latency spikes or error rates above threshold. Fallback chains protect against vendor outages and silent regressions from model updates. Most platform-level orchestrators (Vapi, Pipecat, LiveKit) support fallback configuration natively.


**Traffic splitting for continuous comparison.** Route 95% of production traffic to the primary provider and 5% to a candidate provider. Score both against the same rubric. After a statistically meaningful sample, decide whether to switch, expand the test, or roll back. This is the only way to catch quality regressions that don’t show up in synthetic benchmarks.


**Best-of-breed routing by use case.** Premium provider (ElevenLabs v3 / Inworld) for high-stakes branded interactions; cheap provider (Vapi Voices, Polly Standard) for routing menus and verification; Realtime / Hume for emotionally complex moments inside a longer conversation. Routing logic adds engineering cost but optimizes both quality and unit economics.


The cost of a multi-provider stack is the orchestration and observability infrastructure underneath. Without continuous evaluation, traffic splitting becomes noise. With it, the multi-provider strategy meaningfully outperforms single-vendor commitments at scale.


## How to compare TTS providers honestly


The pattern that works at production scale is independent measurement against your actual audio. Three layers, in order:


1. **A test set drawn from your real use case.** Real callers, speakerphone audio, regional accents, frustrated tones, your specific scripts and vocabulary. Not the vendor’s demo voice reading the vendor’s demo script.
2. **Behavioral graders, not just MOS panels.** Language-model[graders](https://docs.coval.dev/concepts/metrics/overview) that score whether the synthesized speech preserved the meaning, hit the right emotional register, pronounced domain vocabulary correctly, and held consistent voice across the session. MOS is one input; “did the listener get the right information and feel respected” is the outcome.
3. **Continuous regression testing.** Every provider update, including vendor-pushed updates that don’t change the version string, runs against the same[simulation suite](https://docs.coval.dev/concepts/simulations/overview) before it ships to production. Silent regressions are the most expensive failure mode in voice AI.


Coval is the evaluation infrastructure layer for that pattern. Vendor-agnostic by design: the same test set runs unchanged across ElevenLabs Flash v2.5, Cartesia Sonic-3, OpenAI Realtime-2, Deepgram Aura-2, Vapi Voices, Azure Neural, and the rest of the lineup, producing apples-to-apples scoring on your audio. Public head-to-head benchmarks live at[benchmarks.coval.ai/tts](https://benchmarks.coval.ai/tts) ; the methodology is documented in our[voice observability guide](https://www.coval.ai/blog/voice-observability-guide) .


Teams that build this discipline early ship faster downstream. Provider swaps move from quarter-long re-evaluation projects to overnight regression diffs, which means agents can incorporate new vendor capabilities as they ship rather than after a delayed bake-off.


## Frequently asked questions


**What’s the most natural-sounding TTS provider in 2026?**


Independent benchmarks (Artificial Analysis Elo, blind MOS panels) consistently rotate among Inworld Realtime TTS 1.5 Max, ElevenLabs Eleven v3, and Cartesia Sonic-3.5 at the top. The relative ranking depends on the content type, the language, and whether the evaluation samples include long-form sessions. For most production voice agents that use real-time TTS, ElevenLabs Flash v2.5, Cartesia Sonic-3, and Rime Coda are the practical naturalness leaders inside the sub-100ms latency budget.


**What’s a good TTS latency for production voice agents?**


Sub-150ms TTFB is the working ceiling for production voice agents where turn-taking feels natural. Cartesia Sonic-3 (~90ms), Cartesia Sonic Turbo (~40ms), ElevenLabs Flash v2.5 (~75ms), Rime Coda (sub-100ms), and Deepgram Aura-2 (~200ms streaming) sit at or near the top. End-to-end voice-to-voice latency budgets are usually 500–700ms across STT + LLM + TTS combined; OpenAI Realtime collapses STT and TTS into one model for sub-300ms native speech-to-speech.


**How much does TTS cost at production scale in 2026?**


The honest answer is “it depends on which vendor and how much premium-tier traffic vs. base-case traffic you route.” A more useful framing: model your unit economics as a *blended* per-minute rate across three buckets. (1) **Premium tier** for naturalness-critical interactions: ElevenLabs Flash v2.5 / Cartesia Sonic-3 / Inworld at $0.04–$0.10/min equivalents. (2) **Mid-tier** for typical agent traffic: Deepgram Aura-2 / LMNT / PlayAI in the $0.02–$0.04/min range. (3) **Budget tier** for high-volume base-case interactions (reminders, verifications, IVR menus): Vapi Voices Beta at $0.0025/min collapses this line item to near-zero. The blended rate matters more than any single per-1M-char number, because production traffic is rarely 100% premium.


**Should I use a single TTS provider or multiple?**


Production teams at scale typically use multiple: primary + fallback for resilience, traffic splitting for continuous evaluation, best-of-breed routing for use-case-specific quality. The orchestration cost is real but is recovered through outage protection and per-segment cost optimization. Single-provider stacks work when traffic volume doesn’t justify the orchestration overhead, when the use case is narrow enough that one provider clearly wins, or when a single vendor’s compliance posture is the gating factor.


**Are vendor-published TTS benchmarks trustworthy for procurement?**


Treat them as starting points, not decision inputs. Three practical checks before a vendor-reported number influences a procurement decision. (1) Confirm the percentile: vendors quote P50 by default, but P95 and P99 are what callers experience on bad-network days. (2) Confirm the audio profile: clean studio inputs aren’t representative if your traffic includes phone audio, accents, or background noise. (3) Confirm the comparison cohort: most vendor “we beat X” claims compare against an older or simpler competitor model rather than the current flagship. Independent leaderboards (Artificial Analysis TTS Elo, Coval’s continuous[TTS benchmarks](https://benchmarks.coval.ai/tts) ) close the comparison-cohort gap. Your own production-audio test set closes the audio-profile gap.


**Which TTS providers support voice cloning?**


ElevenLabs (Instant + Professional), Cartesia (Professional), Microsoft Azure (Custom Neural Voice + Personal Voice, gated behind Responsible AI review), LMNT (instant), PlayAI, MiniMax, Hume, Inworld, and Rime. OpenAI Realtime does not currently support custom voice cloning, only the preset voices. Sample requirements range from sub-minute for instant cloning to multiple hours of consistent studio-grade audio for professional cloning. Consent and provenance attestation are now standard expectations across the cloning-capable vendors, partly driven by the AIUC-1 framework.


**How often should I re-evaluate my TTS provider choice?**


Continuously, not annually. Vendor-pushed model updates can change voice behavior without changing version strings; new vendor launches arrive every quarter; pricing resets reshape unit economics overnight (the May 7, 2026 ElevenLabs reset cut TTS prices up to 55%). Teams running voice AI at scale build continuous evaluation into their CI/CD pipeline rather than treating TTS choice as a one-time decision. See the[voice observability guide](https://www.coval.ai/blog/voice-observability-guide) for the methodology.


## Where to go from here


For provider-specific depth, the[ElevenLabs review](https://www.coval.ai/blog/elevenlabs-review-2026-voice-cloning-and-synthesis-capabilities-explained) and[Vapi review](https://www.coval.ai/blog/vapi-review-2026-is-this-voice-ai-platform-right-for-your-project) cover those two stacks in detail. For STT-side analysis, the[best STT providers in 2026](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) guide is the companion piece. For the broader voice AI model landscape, see[voice AI models in 2026](https://www.coval.ai/blog/voice-ai-models-2026) .


If you want to talk through how to measure TTS providers against your specific deployment,[book a call with the Coval team](https://cal.com/forms/b7126bad-a5c3-4ed0-af4b-fdfd4b268381) .
