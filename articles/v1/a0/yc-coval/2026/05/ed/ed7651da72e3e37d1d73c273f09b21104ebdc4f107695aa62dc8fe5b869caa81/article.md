---
schema_version: "1.0.0"
document_id: "ed7651da72e3e37d1d73c273f09b21104ebdc4f107695aa62dc8fe5b869caa81"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/elevenlabs-review-2026-voice-cloning-and-synthesis-capabilities-explained/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:07d3a9744868fdf02daebfedff5ce5d42df67f23b5d54aac6400a1692a92079d"
---

# ElevenLabs Voice Cloning Review 2026: v3, Scribe & Agents

ElevenLabs in 2026 is not the company that made the “shockingly human TTS” voices most teams first encountered in 2023. It’s now a[$500M ARR audio AI platform](https://elevenlabs.io/blog/500m-arr-and-new-investors) that ships voice cloning, two distinct TTS families, a 90+-language speech recognition stack, a conversational agents platform with Fortune 500 traction, a government vertical, on-premise deployment options, and a one-prompt wrapper that turns any text chat agent into a voice agent.


This review covers what ElevenLabs actually delivers in 2026 across voice cloning (where it earned its reputation), the v3 ↔ Flash v2.5 trade-off most buyers don’t understand until they’re in production, Scribe v2 on the speech-to-text side, ElevenAgents as a voice agent platform, the May 7 pricing reset, and the enterprise posture (Government, on-prem, AIUC-1 insurance certification). Where helpful, this guide compares ElevenLabs to other 2026[TTS providers](https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)) and[STT providers](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) , and ends with how teams evaluate ElevenLabs in production using vendor-agnostic test infrastructure.


> **Key takeaways**
>
>
> - Voice cloning is split into Instant Voice Cloning (IVC, sub-minute samples, available from the $6 Starter tier) and Professional Voice Cloning (PVC, 3–6 hours of fine-tuned audio, Creator tier and up). Requirements come down to *audio length × audio quality × plan tier* .
> - [Eleven v3 went GA on Feb 2, 2026](https://elevenlabs.io/blog/eleven-v3-is-now-generally-available) with audio tags, multi-speaker dialogue, and 70+ languages. It’s the most expressive ElevenLabs TTS yet, but it isn’t real-time, so production voice agents still run on Flash v2.5 (~75ms latency, 32 languages).
> - Scribe v2 + Scribe v2 Realtime cover the STT side, with sub-150ms latency in 90+ languages. Scribe v1 is deprecated.
> - ElevenAgents (rebranded from Conversational AI) is the production agent platform. Major 2026 additions: Templates, Experiments, Guardrails 2.0, multimodal WhatsApp, and Speech Engine (the May 20 launch that wraps existing chat agents into voice agents from a single prompt).
> - Pricing dropped on May 7, 2026 (TTS −55%, STT −45%, ElevenAgents −20%) and pay-as-you-go was added. Business tier offers TTS at roughly 5¢/minute.
> - For regulated and enterprise deployments, the relevant artifacts are SOC 2 Type 2, GDPR, CPRA, HIPAA, with FedRAMP and AIUC-1 (the first AI-voice-agent insurability certification) layered on top. On-Premise and On-Device are early-access for offline / Confidential Computing workloads.


## What actually shipped in 2026


ElevenLabs has shipped roughly one major launch or partnership per week through 2026. The headline facts that should reset any review:


- **[$500M Series D at an $11B valuation (Feb 4)](https://elevenlabs.io/blog/series-d) and $500M ARR crossed on May 5.** That puts ElevenLabs on a different financial footing than most voice AI infrastructure vendors. Q1 2026 added more than $100M in net-new ARR, mostly attributed to enterprise conversational agents.
- **[Eleven v3 went GA Feb 2](https://elevenlabs.io/blog/eleven-v3-is-now-generally-available)** after eight months in alpha. Audio tags (` \[whispers\]` ,` \[sighs\]` ,` \[laughs\]` ) and multi-speaker dialogue are now first-class.
- **Scribe v2 Realtime shipped Nov 11, 2025** with sub-150ms latency in 90+ languages. Scribe v1 is officially deprecated.
- **ElevenAgents** (the rebrand of Conversational AI) added Expressive Mode, Templates, Experiments, Guardrails 2.0, multimodal WhatsApp, and Speech Engine across Q1 + Q2.
- **May 7 pricing reset:** TTS down 55%, STT down 45%, ElevenAgents down 20%, plus pay-as-you-go for self-serve developers.
- **[ElevenLabs for Government](https://elevenlabs.io/blog/introducing-elevenlabs-for-government) launched Feb 11.**[On-Premise + On-Device](https://elevenlabs.io/blog/enterprise-voice-ai-deployed-locally) entered early access on Apr 9.
- **AIUC-1 certification (Feb 18):** first AI voice agents that an enterprise can insure. Critical procurement unlock for regulated buyers.


If your prior mental model of ElevenLabs was “best TTS, weak everything else,” that hasn’t held since at least Q4 2025.


## Voice cloning: IVC vs PVC


Voice cloning is where ElevenLabs has its largest functional lead over every other TTS vendor. Two products sit under the “voice cloning” umbrella and they answer different needs.


### Instant Voice Cloning (IVC)


A near-instant clone built from a short audio sample. Available from the $6 Starter tier onward.


- **Sample requirements:** roughly 1–3 minutes of clean audio is typical; the docs accept shorter samples but quality degrades quickly under 30 seconds. Mono, 22 kHz or higher, minimal background noise. Cell-phone audio works; auto-tuned music tracks do not.
- **Time to deploy:** seconds. The clone is generated synchronously and is callable through the API immediately.
- **Use cases:** content creators, podcasters, prototypes, single-language narration, internal demos, accessibility tooling, “what would this voice sound like” exploration.
- **Limits:** quality lags PVC for emotional range, edge cases, and out-of-distribution prosody. Some accents and lower-resource languages will sound less natural than the source sample.


### Professional Voice Cloning (PVC)


A fine-tuned voice model trained on a much larger corpus from the speaker. Available from the Creator tier ($11/mo) and above.


- **Sample requirements:** at least 30 minutes of audio; ElevenLabs recommends 3 hours for production-grade quality. The audio should be consistent in style, recording conditions, and prosody. Varied delivery is fine, varied microphones is not. Studio-grade audio dominates the final quality more than any single other variable.
- **Time to deploy:** fine-tune runs typically complete in a few hours.
- **Use cases:** audiobook narration, dubbing, branded voice agents, voice-cloned customer support agents, long-form content where consistency matters.
- **Quality benchmark:** PVC voices are the ones humans most often fail to distinguish from the source speaker in blind listening tests. The trade-off is the upfront audio capture and fine-tune time.


### Voice cloning plan availability


Which plans include voice cloning, and which cloning type each tier unlocks:


Plan IVC PVC Pro Voice Clones Commercial use


Free — — — —


Starter ($6/mo) ✅ — — ✅


Creator ($11/mo first month / $22/mo) ✅ ✅ (1) — ✅


Pro ($99/mo) ✅ ✅ — ✅


Scale ($299/mo) ✅ ✅ 3 ✅


Business ($990/mo) ✅ ✅ 10 ✅


Enterprise (custom) ✅ ✅ Custom ✅


PVC is gated behind paid tiers because each clone consumes meaningful fine-tune compute; ProVoice Clones (the 44.1 kHz / 192 kbps tier-specific clones) require Scale or above.


### Choosing between IVC and PVC


The decision tree is shorter than the marketing pages suggest:


- **You need a voice in 60 seconds and quality is “good enough”:** IVC.
- **You’re shipping consumer-facing content at scale, or running a branded voice agent in production:** PVC. The recording session pays for itself the first time someone notices the difference.
- **You need many voices fast (e.g., character voices for a game):** IVC for prototyping, PVC for shipped versions of recurring characters.
- **You’re cloning a real-life person for commercial use:** legal/consent comes first. ElevenLabs’ policy and the AIUC-1 framework both expect documented consent.


## Eleven v3 vs Flash v2.5: the trade-off that decides your stack


ElevenLabs ships two TTS families in parallel and the choice between them shapes how your voice product behaves end-to-end. The docs (` elevenlabs.io/docs/overview/models` ) are now explicit about the trade-off:


Model Latency Languages Max chars / request Expressive features Real-time agents?


` eleven_v3` not real-time 70+ 5,000 Audio tags, multi-speaker dialogue, Text-to-Dialogue No


` eleven_flash_v2_5` ~75 ms 32 40,000 Standard expressiveness Yes


` eleven_multilingual_v2` offline 29 10,000 Lifelike, emotional range No


**Eleven v3 is the most expressive model ElevenLabs has shipped.** Audio tags let you mark` \[whispers\]` ,` \[sighs\]` ,` \[laughs\]` ,` \[sarcastic\]` inline; multi-speaker dialogue (“Text to Dialogue”) generates an entire scripted exchange in one call. ElevenLabs reports 72% user preference over the alpha and a 68% reduction in symbol/notation errors. The 5,000-character cap per request and the offline-only latency profile are the trade-offs.


**Flash v2.5 is the model that lives in real-time agents.** ~75 ms latency, 32 languages, the 40,000-character limit (8x v3) means you can stream long agent responses in a single call. The expressive ceiling is lower than v3, but it’s the only model that fits inside a sub-200 ms turn-taking budget.


**Multilingual v2** stays in the catalog for long-form, non-real-time content where you want a lifelike voice but don’t need v3’s audio tags.


**Practical guidance:** if you’re shipping anything live and bidirectional (call center agents, voice assistants, IVR replacement, drive-thru, telephony), Flash v2.5 is the default. If you’re producing audio assets (audiobooks, podcasts, video voice-over, multi-character dialogues), v3 is the default. Many production teams ship both: v3 for asset generation, Flash v2.5 for the live agent loop.


> ElevenLabs Turbo v2 and Turbo v2.5 are deprecated as of 2026. Flash supersedes Turbo on every dimension the docs measure.


## Scribe v2 and Scribe v2 Realtime


Most ElevenLabs reviews skip speech-to-text because the recognition story used to be thin. That’s no longer true.


- **Scribe v2.** ElevenLabs’ current flagship STT model. 90+ languages, accepts audio + text conditioning, designed for accurate offline transcription.
- **Scribe v2 Realtime.** The streaming variant launched Nov 11, 2025. Sub-150 ms latency, “negative latency” framing (predictions emit before the speaker finishes), automatic language detection, 93.5% accuracy across 30 European and Asian languages.
- **Scribe v1.** Deprecated. The Feb 2025 launch model that originally hit 96.7% English / 98.7% Italian on FLEURS and Common Voice.


In a 2026 voice agent stack, Scribe v2 Realtime makes ElevenLabs an end-to-end option (STT + LLM orchestration + TTS) instead of forcing teams to pair ElevenLabs TTS with Deepgram or AssemblyAI for the STT layer. Whether you choose that bundle or a multi-vendor stack is a measurement question. See the[Coval STT benchmarks](https://benchmarks.coval.ai/stt) for the head-to-head data and the broader[STT providers guide](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) for the methodology.


## ElevenAgents in 2026


What was “Conversational AI 2.0” in 2025 is now branded **ElevenAgents** , and it has shipped on a once-a-week cadence in 2026.


The big additions, in rough chronological order:


- **Expressive Mode (Feb 10):** agent speech that adjusts emotional register based on the conversation context.
- **AIUC-1 certification (Feb 18):** the first AI voice agents an enterprise can insure. This is a procurement unlock for regulated buyers (financial services, healthcare, government).
- **Experiments (Feb 19):** controlled A/B testing for agent configurations (prompt variants, workflow branches, voice choice).
- **Guardrails 2.0 (Mar 24):** safety layer redesigned to support custom business policies on top of prebuilt protections.
- **Agent Templates (Apr 27):** 50+ pre-built agent configs across support, sales, ops, front desk. Useful starting points; not a substitute for a use-case-specific eval suite.
- **Multimodal WhatsApp (May 6):** agents on WhatsApp now handle images, PDFs, audio messages, contacts, and location pins.
- **Speech Engine (May 20):** A single-prompt configuration converts a text chat agent into a voice agent: TTS, STT, and orchestration combined into one ElevenAPI pipeline. The “I have a chat agent and I want a voice agent next quarter” path collapses from months to one prompt.


Distribution and telephony coverage:


- Web widget, React / iOS / Android / React Native SDKs, WebSocket API
- Telephony via Twilio, Genesys, Vonage, Telnyx, Plivo, or any SIP PBX
- MCP server support (agents reach external tools/data via Model Context Protocol)
- 5,000+ voices across 31 voice-cloned languages, TTS in 70+ languages
- Sub-100 ms agent turn-taking latency claim (latency is a measurement question; see Coval’s evaluation methodology below)


Customer momentum: Deutsche Telekom (Jan 14 partnership),[Klarna (“10x faster time-to-resolution”)](https://elevenlabs.io/blog/klarna) , Revolut, Mahindra (XUV 7XO demand-spike absorption), Razorpay (outbound merchant engagement), Customers Bank ($25B-asset US bank), Better’s “Betsy” mortgage agent. Government deployments live in Ukraine, Czech Republic (~5,000 calls/day, 85% independent resolution), Midland, TX. Poland’s national healthcare voice agent (Centralna e-Rejestracja) is mid-rollout.


The customer list matters because it reflects the durability question. ElevenAgents is running production workloads at large enterprises, not clearing demo bars.


## Pricing in 2026


The May 7 pricing reset changed the affordability math on every tier. The current published tiers:


Tier Price Credits/mo Voice cloning Seats Headline feature


Free $0 10,000 — 1 TTS, STT, SFX, Voice Design, Music, 3 Studio projects


Starter $6/mo 30,000 IVC 1 + Instant Voice Cloning, Dubbing Studio, 20 Studio projects


Creator $11/mo (first month) → $22/mo 121,000 PVC 1 + Professional Voice Cloning


Pro $99/mo 600,000 PVC 1 + 44.1 kHz PCM, 192 kbps audio


Scale $299/mo 1,800,000 PVC (3 voices) 3 Team collaboration


Business $990/mo 6,000,000 PVC (10 voices) 10 Low-latency TTS as low as ~5¢/minute


Enterprise Custom Custom Custom Custom DPA / SLA / BAAs (HIPAA), custom SSO, private deployment


Pay-as-you-go was introduced for self-serve developers in the same May 7 reset. Useful for spiky workloads where flat tiers would over-provision.


For voice agents specifically, the Business tier’s ~5¢/minute TTS quote is the relevant benchmark for unit economics. At scale, the per-minute cost of TTS is rarely the dominant line item once you account for STT + LLM + telephony. But it’s where ElevenLabs deliberately competes with cheaper providers like[Cartesia](https://www.coval.ai/blog/elevenlabs-vs-cartesia-which-tts-provider-is-right-for-your-voice-ai-project) , Vapi Voices, and the open-source long tail.


## Enterprise posture: Government, on-prem, insurance


The procurement question for ElevenLabs in 2026 is less “is the voice quality good enough” and more “can my legal, security, and compliance team sign off on it.” The 2026 enterprise posture answers that:


- **[ElevenLabs for Government](https://elevenlabs.io/blog/introducing-elevenlabs-for-government) (Feb 11):** dedicated public-sector offering. SOC 2 Type 2, GDPR, CPRA, HIPAA in place, with GovRAMP, FedRAMP, CJIS, CMMC in progress.
- **[On-Premise and On-Device](https://elevenlabs.io/blog/enterprise-voice-ai-deployed-locally) (Apr 9):** early access. On-Premise runs on customer-controlled servers with Confidential Computing GPUs. On-Device targets offline inference for automotive, wearables, kiosks. Adds to the existing Cloud and VPC deployment models.
- **AIUC-1 certification (Feb 18):** the first AI voice agents an enterprise can insure. Designed with Fortune 500 risk leaders. This is the single most important procurement unlock for regulated industries.
- **Workspace + access controls:** RBAC, billing groups, shared resources, SCIM, SSO.
- **Named the 2026 Google Cloud Marketplace Partner of the Year** for Applied AI (Apr 21). Useful when your buyer prefers procurement through their cloud marketplace.


Two things to verify with ElevenLabs directly during your evaluation:


1. **Data residency commitments** for your region. The docs reference EU, US, and on-prem options, but specifics depend on your contract.
2. **Audit log and call-recording retention.** Varies by tier and by region.


## How teams actually evaluate ElevenLabs in production


ElevenLabs publishes its own benchmarks. So does every other vendor in this space. Vendor-reported benchmarks pick conditions that flatter the system being measured: studio-quality audio, simple input, the languages the model was trained hardest on. Production traffic looks nothing like that.


The teams running voice AI at scale build their own evaluation infrastructure. The standard pattern, in three layers:


1. **A test set drawn from your actual use case.** Not the ElevenLabs demo voices, but the audio your callers will actually produce. Speakerphone, accents, hold music bleed-through, frustrated tones, multi-intent utterances.
2. **Behavioral graders, not just exact-match transcripts.** Language-model graders that score whether the agent did the right thing, with the right tone, against the rubric your business cares about. STT WER is one input; “did the conversation resolve” is the outcome metric.
3. **Continuous regression testing.** Every prompt change, every model update (including vendor-pushed updates that don’t change the version string), every backend integration tweak runs against the same suite before it ships.


Coval is the evaluation infrastructure layer for that pattern. We’re vendor-agnostic: the test set you build for ElevenLabs Flash v2.5 runs unchanged against Cartesia Sonic 2, Vapi Voices, OpenAI Realtime, or any combination, and produces apples-to-apples scoring. The same applies to STT: Scribe v2 Realtime, Deepgram Nova, AssemblyAI Universal can all be measured on your audio with the same rubric. Public benchmarks live at[benchmarks.coval.ai/tts](https://benchmarks.coval.ai/tts) and[benchmarks.coval.ai/stt](https://benchmarks.coval.ai/stt) ; the evaluation methodology is documented in our[voice observability guide](https://www.coval.ai/blog/voice-observability-guide) .


Any honest 2026 ElevenLabs review depends on what your production traffic actually looks like, not what the vendor’s marketing page measures.


## Frequently asked questions


**Is ElevenLabs voice cloning legal?**


Voice cloning is legal in most jurisdictions when you have documented consent from the person whose voice is being cloned, or when the voice is your own. ElevenLabs’ terms require consent attestation for any non-self clone, and the AIUC-1 framework formalizes that expectation. Cloning a public figure or a third party without consent is both a terms-of-service violation and, in many jurisdictions, a legal liability.


**What audio quality is required for ElevenLabs voice cloning in 2026?**


For Instant Voice Cloning, 1–3 minutes of clean mono audio at 22 kHz or higher, with minimal background noise, will produce a usable clone. Cell-phone recordings work; auto-tuned or musically processed audio does not. For Professional Voice Cloning, ElevenLabs recommends 3 hours of consistent studio-grade audio for production-grade quality, with at least 30 minutes as a floor.


**Which ElevenLabs plans include voice cloning in 2026?**


Instant Voice Cloning starts on the $6/mo Starter tier. Professional Voice Cloning starts on the $11/mo (first-month price) Creator tier. ProVoice Clones (44.1 kHz / 192 kbps) require Scale ($299/mo) or higher. Enterprise tiers add custom voice clone counts and dedicated support.


**What’s the difference between Eleven v3 and Flash v2.5?**


Eleven v3 is the most expressive model (audio tags, multi-speaker dialogue, 70+ languages), but it’s not real-time and caps requests at 5,000 characters. Flash v2.5 is the real-time model (~75 ms latency, 32 languages, 40,000-character cap) used for voice agents and any live bidirectional voice product. Most teams ship both: v3 for asset generation, Flash v2.5 for the live agent loop.


**Is ElevenLabs Scribe v2 better than Deepgram Nova for production STT?**


It depends on your audio, your languages, and your latency budget. Scribe v2 Realtime publishes sub-150 ms latency with 93.5% accuracy across 30 European + Asian languages. Deepgram Nova, AssemblyAI Universal, and OpenAI Whisper Large v3 all publish competitive numbers. The honest answer is to run the same test set across all of them. That’s exactly what the[Coval STT benchmark dashboard](https://benchmarks.coval.ai/stt) does publicly.


**How much does an ElevenAgents voice agent cost per minute in 2026?**


The post-May-7 pricing puts Business-tier TTS at roughly 5¢/minute. Total per-minute cost for a deployed agent depends on STT + LLM + telephony pass-through; ElevenAgents pricing dropped 20% in the May 7 reset. Enterprise contracts negotiate further. The total of unit economics under your real traffic mix is the only number that matters.


**Can ElevenLabs deploy on-premise or air-gapped for regulated industries?**


Yes, as of Apr 9, 2026. On-Premise runs on customer-controlled servers using Confidential Computing GPUs (early access). On-Device targets offline inference. Cloud and VPC remain the standard deployment modes. ElevenLabs for Government adds public-sector-specific compliance commitments.


**What is AIUC-1, and why does it matter?**


AIUC-1 changes the procurement workflow for AI voice agents. Without it, a regulated buyer evaluating an AI voice deployment had to choose between (a) accepting unbounded liability for hallucinations and policy violations, or (b) waiting for their existing E&O / cyber insurance carrier to underwrite AI-specific risk — a process that historically took quarters. AIUC-1 collapses that loop: the framework was co-designed with Fortune 500 risk leaders specifically so insurers can underwrite AI voice agents inside existing policy frameworks. For procurement, the practical effect is that legal and compliance reviews now have a defensible certification to anchor on, and the deployment timeline drops from “indefinite” to weeks.


## Where to go from here


If you’re early in evaluating voice infrastructure, the broader[voice AI models guide](https://www.coval.ai/blog/voice-ai-models-2026) walks through the model landscape across providers. If you’ve narrowed in on ElevenLabs versus a specific competitor,[ElevenLabs vs. Cartesia](https://www.coval.ai/blog/elevenlabs-vs-cartesia-which-tts-provider-is-right-for-your-voice-ai-project) covers that comparison directly. For STT specifically, the[STT providers guide](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) frames the trade-off across Scribe, Deepgram, AssemblyAI, and Whisper.


If you’re past the vendor-choice stage and want to talk through how to measure your specific deployment,[book a call with the Coval team](https://cal.com/forms/b7126bad-a5c3-4ed0-af4b-fdfd4b268381) .
