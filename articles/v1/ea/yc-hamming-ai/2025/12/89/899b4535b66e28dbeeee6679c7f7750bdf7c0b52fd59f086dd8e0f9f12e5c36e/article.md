---
schema_version: "1.0.0"
document_id: "899b4535b66e28dbeeee6679c7f7750bdf7c0b52fd59f086dd8e0f9f12e5c36e"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-88fd2d772bc9"
canonical_url: "https://hamming.ai/blog/are-speech-to-speech-models-ready-to-replace-cascade-models"
published_at: "2025-12-21T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:38.132930+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:fcc92c31468c6dccc9cbd84277388a16cd8835ad87139508393c0e1cdc23f657"
---

# Are Speech-to-Speech Models Ready to Replace Cascade Models?

# Are Speech-to-Speech Models Ready to Replace Cascade Models?


*This post was adapted from Hamming’s[podcast conversation with Zach Koch](https://youtu.be/WJx5XJzDr1E) , co-founder of[Ultravox](https://www.ultravox.ai/) . Ultravox builds audio-native speech models and infrastructure so teams can run real-time voice agents without relying on traditional ASR, LLM and TTS pipelines.*


Most production voice agents today still use a cascading architecture. Audio comes in, ASR converts it to text, a language model reasons and calls tools, then TTS turns the response back into speech. It’s familiar, debuggable, and well understood.


Speech-to-speech models take a different approach. They operate on audio directly, learning to understand and respond without ever producing an intermediate transcript. In theory, this makes agents more natural and responsive. In practice, teams have worried about trade-offs in reasoning, tool-calling, and robustness.


**Quick filter:** If your cascaded stack is already stable and audited, you don't have to jump just because speech-to-speech is trendy. The interesting question is where the audio-native approach is now *good enough* to be the default.


## Why Ultravox Bet on Native Audio


Ultravox did not start as a speech-model company. Zach and his co-founders began by experimenting with multi-agent frameworks on top of GPT-3, before tool calling existed.


Early demos showed something important:


- Large language models were already good at natural dialogue
- Latency, ASR speed, and TTS quality were not yet ready for real-time agents


The first attempts at a voice interface were slow and brittle. Time-to-first-token was high, ASR models lagged, and TTS quality was inconsistent. Voice AI was clearly interesting, but not obviously deployable.


That part of the conversation felt familiar. We all remember demos where the system *technically* worked but still felt like it was holding its breath between turns.


By mid-2023, the ecosystem had shifted. Faster LLMs with Whisper-level ASR, and better TTS engines changed the equation. Ultravox built a more capable voice stack, and by January 2024, the team made two linked strategic decisions:


1. Pivot the company fully to voice.
Voice would be the primary interface they optimized for, not a side feature.
2. Stop being an orchestration company and become a model company.
Instead of chaining ASR, LLM, and TTS, Ultravox would train models that understand audio natively, without ever emitting a transcript.


The motivation was simple: pipelining was limiting the ceiling. Converting rich audio to text removes timing, emphasis, and prosody, and passing partial text through multiple components introduced context loss. Trying to stitch everything back together in a controller layer became harder over time.


If the goal is human-like dialogue, Ultravox concluded that the model itself needs to learn speech as a first-class modality, not as a preprocessing step.


## From Llama to GLM 4.6: Choosing a New Base Model


Ultravox’s early models were built on[Llama 3 70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B) , which was strong when it launched. Over time, however, it fell behind fast, low-latency proprietary models like GPT-4.1 on reasoning and tool-use quality.


The team evaluated a wide range of open-weight successors. Many of them looked impressive on public benchmarks, but failed under Ultravox’s own tests.


Internally, Ultravox uses[Voice Agent Bench](https://arxiv.org/abs/2510.07978) : a suite of evaluations tailored to voice workloads, focused on:


- Short, rapid utterances
- Fast context growth over many turns
- Instruction following under barge-ins and interruptions
- Tool-calling accuracy in conversational flows


On this benchmark, most models "looked benchmark-maxed, but actually struggled" when dropped into voice-agent scenarios. This mirrors what teams see with[intent recognition testing at scale](https://hamming.ai/resources/intent-recognition-voice-agents-at-scale) - standard metrics don't capture the cascade failures that occur in real conversations.


[GLM 4.6](https://huggingface.co/zai-org/GLM-4.6) stood out because:


- It performed unexpectedly well on Voice Agent Bench
- It matched or exceeded the cascading setups they had been using
- It did so while still being viable for real-time use


GLM 4.6 is also much larger than Llama 3 70B, on the order of hundreds of billions of parameters, with a mixture-of-experts design. That size introduced new complexity for Ultravox’s training pipeline, especially because of a key constraint: when you fine-tune a model for a narrow purpose, you usually make it worse at everything else.


Ultravox’s training objective was twofold:


- Teach the model to align audio representations with text representations, so it can leverage the base model’s world knowledge
- Preserve (or improve) reasoning and tool-calling performance instead of degrading it


Ultravox v0.7 is the current iteration of that strategy: an audio-native, GLM-based model designed for real-time speech understanding.


## Where I’d Still Choose Cascade Today


Even after this conversation, there are still a few scenarios where I’d keep a cascade:


- **High-stakes compliance:** If you need line-by-line transcripts for audits, cascades are still easier to reason about.
- **Hard voice branding requirements:** If your product lives or dies on a specific voice identity, TTS control still matters.
- **Debug-heavy teams:** When an incident happens, cascades make it faster to isolate whether the failure came from ASR, reasoning, or TTS.


## Why Cascading Architectures Are Still Dominant


If models like Ultravox v0.7 exist, why hasn’t the industry flipped to speech-to-speech by default? Zach points to a simple reason: until very recently, there was a real trade-off.


For most of the past year, speech-to-speech stacks offered better latency with more natural conversational flow but weaker reasoning and tool-calling compared to top cascading stacks.


Cascading stacks offered stronger model quality (thanks to frontier text LLMs), easier debugging and instrumentation, and familiar failure modes for enterprise teams.


Other practical factors also kept cascades in place:


-


**Voice branding and cloning.**
Many speech-to-speech systems ship with a small fixed set of voices. Teams that care about custom voices or clones frequently revert to a TTS-centric stack.


-


**Operational familiarity.**
Tools, logging, and observability around ASR + LLM + TTS are more mature. Rebuilding that stack for a new architecture takes time.


Zach’s view is that Ultravox v0.7 helps remove some of those trade-offs: it reaches parity with strong cascades on their internal benchmarks while preserving the benefits of audio-native modeling. But he also acknowledges that adoption curves lag technical capability. Teams change architectures only when the benefits are clear and failure modes are well understood.


## What’s Still Hard About Speech-to-Speech?


Despite the progress, speech-to-speech is not “solved.” If you say someone’s name three different ways, gently, neutrally, and sharply, humans immediately understand that these intonations imply different emotional states and require different responses. Today’s models generally do not.


The gap comes from how current systems are trained:


- Text-first LLMs already have rich world models: they understand concepts, emotions, and abstract reasoning over text.
- They do not inherently understand what the world sounds like, how prosody, timing, and emphasis map onto that world model.


Ultravox’s approach is to align audio and text spaces, rather than build a speech-only foundational model from scratch. That alignment has to:


- Preserve the base model’s knowledge and reasoning
- Carry through subtle audio signals from input all the way to output
- Avoid early bottlenecks that “flatten” nuance into generic text


Beyond alignment, other open challenges remain:


- Truly talking and listening at the same time requires new strategies for scheduling inference, detecting interruptions, and managing overlapping turns.
- Many systems today simply hallucinate emotion labels (“you sound happy”) without consistent grounding in real audio cues.


We should be honest about this: the “emotion understanding” layer is still fragile. A lot of teams handle it with prompt heuristics rather than robust signal understanding.


## What Builders Should Watch Going Into 2026


For teams deploying voice agents today, a few practical lessons emerge from Ultravox’s journey:


-


**Expect a hybrid world for a while:** Cascading stacks won’t disappear overnight. Speech-to-speech will coexist and gradually take over as the trade-offs disappear in specific workloads.


-


**Use task-specific benchmarks, not generic ones:** Public leaderboards are not enough. You need your own “Voice Agent Bench” tuned to the tasks, accents, and tools that matter to you.


-


**Treat pre-processing as part of the model:** VAD, noise handling, and device quirks are first-class components. They belong in your test matrix and monitoring, not just your audio SDK.


-


**Plan for resets:** Architectures will change. Some branches of your roadmap will get thrown out. The goal is not to avoid resets, but to recognize early when a path is structurally wrong.


The bigger shift is conceptual: as audio-native models mature, the unit of design moves from “a pipeline of components” to “a conversational system that happens to be built on audio representations.”


Getting there will take better models, better training data, and better tooling around evaluation and observability. But the trajectory is clear: speech-to-speech is moving from experiment to serious default option.


[Listen to the full conversation on The Voice Loop](https://youtu.be/WJx5XJzDr1E)
