---
schema_version: "1.0.0"
document_id: "ac326e408d81ce10d34f3de2cf3f470f88a6e856722b08bd7d267ad4aef15c63"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/voice-ai-models-2026/"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:5b67ed36d4c4e564f71f46939d20ca779c2e506a16cee60399a273b44ab6b60a"
---

# Voice AI Models in 2026: Comparing the LLMs Powering Voice Agents

A **voice AI model** can mean any of the models in a voice agent’s stack: the speech-to-text (ASR) model that converts caller audio to a transcript, the reasoning LLM that decides what the agent says next, the text-to-speech model that renders the response back to audio, or a unified speech-to-speech model that handles all three in one pass. In a cascaded pipeline the “voice AI model” you spend the most time choosing is usually the LLM; in a speech-to-speech pipeline it’s the single audio-to-audio model. Picking the wrong one is among the most expensive mistakes in a deployment. Teams that pick on benchmarks and brand names instead of on their own data routinely ship agents that hesitate, hallucinate tool calls, or burn 300ms of latency the caller can hear. At[Coval](https://www.coval.ai/) , our voice AI evaluation platform sees this play out across thousands of voice agent test runs each week: the “obvious” model often loses to a less-hyped option once you grade it on your own scenarios. Our public[voice AI model benchmarks](https://benchmarks.coval.ai/) capture the head-to-head data across STT, LLM, and TTS layers.


This guide compares the voice AI models on the market in 2026 (GPT-4o, Claude, Gemini, Llama, and the voice-native model class led by Sesame and Kyutai’s Moshi), the cascaded versus speech-to-speech trade-off, and how teams are choosing among the options for production deployments.


## Key takeaways


- **Latency is the constraint.** The LLM has 200 to 700ms of budget between STT and TTS. Models with great chat benchmarks but slow time-to-first-token are unusable for voice.
- **Cascaded pipelines still dominate enterprise, but speech-to-speech is catching up.** Three-stage pipelines (STT → LLM → TTS) win on observability and debuggability because each stage produces traces that pinpoint exactly where a conversation went wrong. Speech-to-speech surfaces only a transcript and (so far) leaves the model’s reasoning opaque, though the most recent S2S releases are starting to expose deeper reasoning observability.
- **Speech-to-speech is winning latency-sensitive use cases.** OpenAI’s GPT-Realtime-2 and Google’s Gemini 3.1 Flash Live are the production options when sub-second response is the deciding factor.
- **TTS choice often matters more than LLM choice.** Callers notice voice naturalness before they notice word quality. Premium TTS is one of the highest-ROI line items in a voice stack.
- **Model selection should be empirical rather than narrative.** Build a representative test set, run it across candidates, grade on your own data. Vendor benchmarks are not your benchmarks.


**What makes a model “good for voice”?**


A voice AI model has to pass tests that don’t appear on chat leaderboards. The same large language models that work for chat applications don’t always work as well for voice. The constraints are different, and the failure modes are different.


**Latency budget.** A natural conversational turn happens in 600 to 1,200 milliseconds end-to-end. Subtract STT (typically 150 to 300ms streaming) and TTS (typically 100 to 400ms time-to-first-byte), and the LLM has somewhere between 200 and 700 milliseconds to produce its response. Models that work great in batch settings are unusable for voice if their time-to-first-token sits at 2 seconds.


**Instruction following over long conversations.** Voice conversations often span 10 to 30 turns. The model has to maintain context, stay on-policy, and not drift across all of them. Some models that excel at single-turn benchmarks degrade over multi-turn voice contexts.


**Conciseness.** TTS is slow to render. Every extra word the model produces is another 50 to 150 milliseconds the caller has to wait. Models that default to verbose chat-style answers create a worse voice experience than equally capable models that produce tighter responses.


**Robustness to noisy transcripts.** Phone audio produces imperfect transcripts. The model has to do the right thing when “I’d like to book an appointment for next Tuesday” comes in as “I’d like to book an appointment for next Twosday.” Models that are brittle to STT errors compound the problem.


**Tool-calling reliability.** Voice agents lean heavily on tool calls (database lookups, payment processing, calendar queries). A model that calls the wrong tool, passes the wrong parameter, or fails to call a tool when it should derails the agent in ways the caller will feel immediately. We treat[tool-call accuracy](https://docs.coval.dev/concepts/metrics/overview) as a first-class metric in our evaluation framework rather than a side check.


These constraints push voice AI toward a different model selection process than chat applications use. Latency dominates, and verbose models are penalized in ways that don’t show up in standard chat benchmarks.


## The two architectural paths


There are two ways to wire models into a voice agent, and the choice has cascading implications for every other decision the team will make.


### Cascaded pipelines


Audio comes in, gets transcribed by an STT model, the transcript is processed by a language model, the language model’s response gets rendered by a TTS model back to audio. Three separate models, three separate vendors, three separate evaluation surfaces.


**Strengths.** You can swap any component independently. You can evaluate each stage on its own metrics: STT word error rate, LLM intent accuracy, TTS naturalness. You have transcripts and reasoning traces, which means you can debug failures and grade conversations at scale. Most production deployments in 2026 are cascaded for these reasons.


**Weaknesses.** Latency budget gets tight. Three models in series means three sources of delay. You also lose paralinguistic information: the LLM never knows whether the caller sounded angry, hesitant, or relieved, because that signal didn’t survive the transcription step.


### Speech-to-speech models


A single model takes audio in and produces audio out, with no intermediate text representation. OpenAI’s[GPT-Realtime family](https://openai.com/index/introducing-gpt-realtime/) (now on GPT-Realtime-2, with GPT-5-class reasoning in the live audio loop and a 128K-token context window), Google’s[Gemini 3.1 Flash Live](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-1-flash-live/) (audio-to-audio across 90+ languages, leading on tool-call benchmarks at the time of release), and Sesame’s models are the prominent examples in 2026.


**Strengths.** Lower latency, often by a wide margin. More natural prosody because the model is generating audio directly rather than handing a transcript to a separate TTS system. The model can hear and respond to tone, emotion, and emphasis in the caller’s voice.


**Weaknesses.** Observability is much harder. When the entire conversation happens inside one model, the seams where you’d normally evaluate accuracy disappear. You can’t grade transcription quality independently when there’s no transcript. You can’t audit the planner’s reasoning because reasoning isn’t externalized in text. This makes rigorous evaluation harder, even when the user experience is better.


For most enterprise voice AI deployments in 2026, cascaded still wins because of the evaluation advantage. Speech-to-speech is gaining ground in latency-sensitive applications (live customer service, drive-through) where the experience improvement justifies the observability cost. We covered this trade-off in depth in[speech-to-speech vs. cascaded voice AI](https://www.coval.ai/blog/speech-to-speech-vs-cascaded-voice-ai-which-architecture-should-you-deploy) , and the[cascaded voice AI architecture deep dive](https://www.coval.ai/blog/cascaded-voice-ai-architecture-why-enterprise-teams-choose-traditional-pipelines-over-s2s) walks through the enterprise reasoning at length.


### Cascaded vs. speech-to-speech: at a glance


Dimension Cascaded pipeline Speech-to-speech


Typical end-to-end latency 600 to 1,200ms 300 to 700ms


Paralinguistic awareness (tone, emotion) No Yes


Observability and debuggability High (transcripts, traces per stage) Low (no intermediate text)


Independent component swap Yes. Swap STT, LLM, or TTS in isolation No. Single vendor stack


Best for regulated and audited use cases Yes Limited


Best for latency-critical UX (drive-through, live chat) Sometimes Yes


Verdict Default for enterprise voice AI in 2026 Pick when latency is the deciding factor and you accept reduced observability


## The language models powering voice AI in 2026


The LLM layer is where most of the variation across voice AI model deployments shows up. Here’s where the major options stand.


### OpenAI: GPT-4o and the GPT-Realtime family


OpenAI’s models remain the default starting point for many voice AI teams. GPT-4o is the workhorse for cascaded pipelines: fast, strong at instruction following, reliable at tool calling. Higher-tier OpenAI models tend to be more capable but slower and more expensive, which makes them a harder fit for latency-sensitive voice applications outside of premium tiers.


The[GPT-Realtime family](https://openai.com/index/introducing-gpt-realtime/) is OpenAI’s speech-to-speech offering. It accepts audio directly, produces audio directly, and has been the most widely adopted speech-to-speech option since its initial release in 2024. The current generation, GPT-Realtime-2 (released May 2026), brings GPT-5-class reasoning into the live audio loop, expands the context window from 32K to 128K tokens, and adds adjustable reasoning-effort settings for trading latency against quality. The trade-off: less mature observability tooling than the cascaded path, and instruction following over long conversations has historically been weaker than the cascaded GPT-4o equivalent in our internal evaluations, though GPT-Realtime-2 closes much of that gap.


### Anthropic: Claude Sonnet 4.6 and Opus 4.7


Anthropic’s[Claude family](https://platform.claude.com/docs/en/about-claude/models/overview) is the strongest competitor to GPT in cascaded voice deployments. Claude Sonnet 4.6 (released February 2026) is fast, strong at multi-turn instruction following, and known for handling complex prompts and policies more reliably than GPT-4o on longer conversations. Claude Opus 4.7 (released April 2026) is more capable but slower; teams typically reserve it for the planner role in multi-agent voice architectures rather than the front-line conversational model.


Claude does not have a speech-to-speech model in production as of mid-2026. Teams using Claude for voice are using it in cascaded pipelines.


### Google: Gemini Flash, Gemini Pro, and Gemini 3.1 Flash Live


Gemini Flash is widely used for voice deployments where cost matters more than absolute capability: cheaper than GPT-4o or Claude Sonnet at comparable latency, based on published per-token pricing. Gemini Pro is competitive for high-stakes deployments but trades off latency.


[Gemini 3.1 Flash Live](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-1-flash-live/) is Google’s current speech-to-speech offering, released March 2026. It’s audio-to-audio across 90+ languages and led on tool-call benchmarks (ComplexFuncBench Audio, Audio MultiChallenge) at the time of release. It competes directly with GPT-Realtime-2 and has gained traction in deployments where Google Cloud is already the infrastructure provider.


### Open-source models: Llama, Mistral, Qwen


For teams with the engineering capacity to self-host, open-source models running on dedicated inference (vLLM, TensorRT-LLM, custom infrastructure) can match the latency of proprietary models at lower cost per token. The current frontier of open-source: Meta’s[Llama 4](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) (Scout and Maverick, both MoE architectures with 17B active parameters),[Mistral 3](https://mistral.ai/news/mistral-3) and Mistral Medium 3.5 (released April 2026), and Alibaba’s Qwen 3.5 family. All ship under permissive licenses (Apache 2.0 in most cases) and are competitive with proprietary models on conversational voice tasks.


The trade-off is the engineering investment. Self-hosting an open model competitively requires GPU infrastructure, inference optimization, observability tooling, and a team that knows how to keep all of it running. For most teams, the math doesn’t work until call volume is high enough that API costs dominate the engineering overhead. In our experience that crossover sits in the high-hundreds-of-thousands of calls per month, though it varies widely by use case.


### Voice-specific models: Sesame, Moshi, Kyutai


A new category of models built specifically for voice from the ground up.[Sesame CSM](https://www.sesame.com/research/crossing_the_uncanny_valley_of_voice) is a 1B-parameter open-weights conversational speech model built on a Llama-style backbone with an audio decoder for expressive output.[Moshi from Kyutai](https://github.com/kyutai-labs/moshi) is a full-duplex spoken dialogue framework that achieves around 200ms end-to-end latency by modeling user and system audio simultaneously on parallel streams. Kyutai also ships a[Pocket TTS](https://kyutai.org/tts) model (released January 2026) light enough to run on a CPU in real time. These models trade some general-purpose capability for voice-native behaviors: full-duplex conversation, low latency, naturalistic prosody. They aren’t yet the default choice for most enterprise voice deployments but are gaining ground in consumer applications and embedded use cases. We covered the technology in[the future of speech-to-speech AI](https://www.coval.ai/blog/the-future-of-speech-to-speech-ai-inside-gradium-and-kyutai-s-approach-to-full-duplex-conversation) , which sits alongside our broader[voice AI stack](https://www.coval.ai/blog/the-ultimate-voice-ai-stack) overview.


## The supporting cast: STT and TTS models


In cascaded pipelines, the model decisions don’t stop at the LLM. The speech-to-text and text-to-speech choices have nearly as much impact on the deployed experience.


### Speech-to-text


- [Deepgram Nova-3](https://deepgram.com/product/speech-to-text) **.** The most widely deployed STT in commercial voice AI as of 2026. ~150ms streaming latency, real-time diarization, strong accent handling, strong on noisy phone audio.
- [Whisper](https://openai.com/index/whisper/) **.** OpenAI’s open-source STT remains the strongest in the open model space. Higher latency than commercial streaming options, better at non-English languages than most commercial alternatives.
- [AssemblyAI](https://www.assemblyai.com/) **, **[Speechmatics](https://www.speechmatics.com/)** .** Competitive in specific verticals, particularly call centers with strong compliance requirements.
- [Cartesia Ink](https://cartesia.ai/ink) **.** Newer entrant paired with Cartesia’s TTS and agent stack. Built for the same sub-100ms latency floor Cartesia targets on the synthesis side.


Provider Streaming latency Quality tier Cost tier Best for


Deepgram Nova-3 ~150ms High Mid Default commercial choice for production English-language voice


Whisper (open) 300-600ms High (non-English standout) Free / self-host Lower-resource languages, batch transcription, open-source stacks


AssemblyAI ~200ms High Mid Call centers with compliance and post-call analytics needs


Speechmatics ~250ms High Mid-High Accent and dialect breadth, on-prem and regulated deployments


Cartesia Ink <100ms High Mid Latency-critical real-time agents paired with Cartesia TTS


We benchmarked the leading options in[best speech-to-text providers in 2026](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) .


### Text-to-speech


- [ElevenLabs Eleven v3](https://elevenlabs.io/) **.** Market leader on voice naturalness. Premium pricing, strong prosody, 70+ language coverage. Their Conversational AI product bundles Eleven v3 with turn-taking, function calling, and RAG; on-premise enterprise deployment landed in April 2026.
- [Cartesia Sonic 3](https://cartesia.ai/sonic) **.** Strong competitor on latency and quality, with sub-100ms time-to-first-byte. Their[Line platform](https://www.cartesia.ai/agents) (launched April 2026) pairs Sonic 3 with Cartesia Ink STT and an agents runtime. We compared the two in[ElevenLabs vs. Cartesia](https://www.coval.ai/blog/elevenlabs-vs-cartesia-which-tts-provider-is-right-for-your-voice-ai-project) .
- **OpenAI TTS, Google Cloud TTS, Amazon Polly.** Cheaper, less natural, still adequate for some deployments. The gap between premium TTS and budget TTS is one of the most noticeable differences a caller hears.
- [Mistral Voxtral](https://mistral.ai/news/voxtral) **.** Recent open-source TTS option from Mistral covering nine languages. Useful for teams that need an Apache-2.0 TTS path and have the inference capacity to host one.


Provider Time-to-first-byte Quality tier Cost tier Best for


ElevenLabs Eleven v3 200-400ms Top (most natural) Premium Brand-voice deployments where prosody is the deciding factor


Cartesia Sonic 3 <100ms High Mid-Premium Latency-critical real-time agents and full-stack Cartesia Line setups


OpenAI TTS / Google Cloud TTS / Amazon Polly 150-300ms Mid Low Budget-sensitive deployments, internal tools, non-customer-facing


Mistral Voxtral (open) 200-500ms self-hosted Mid-High Free / self-host Apache-2.0 stacks, 9 supported languages, in-house infra teams


The benchmarks for TTS quality and the trade-offs across providers are covered in[best text-to-speech providers in 2026](https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)) .


## How to choose models for your voice AI deployment


The decision tree depends on the constraints that matter most for your use case. A few common patterns:


**Optimizing for latency.** Cascaded pipeline with streaming STT (Deepgram Nova-3 or similar), a fast LLM (GPT-4o or Gemini Flash), and a low-latency TTS (Cartesia). Or skip the cascade and go speech-to-speech with GPT-Realtime-2 or Gemini 3.1 Flash Live, accepting the observability trade-off.


**Optimizing for instruction following at scale.** Claude Sonnet as the LLM in a cascaded pipeline. Higher latency than GPT-4o, but the multi-turn consistency advantage matters when your agent has a 30-page policy document.


**Optimizing for cost.** Self-hosted Llama or Mistral if you have the volume and engineering capacity. Otherwise, Gemini Flash as the LLM.


**Regulated industries (healthcare, financial services, government).** Cascaded pipeline with attested vendors. The compliance review process is easier when each component has its own attestation and you have transcripts to audit. We covered the financial industry deployment patterns in[how to optimize your voice AI stack for the financial industry](https://www.coval.ai/blog/how-to-optimize-your-voice-ai-stack-for-the-financial-industry) , and the broader patterns in[voice AI in banking](https://www.coval.ai/blog/voice-ai-in-banking-why-evaluation-is-critical-for-compliance-and-customer-experience) .


**Multilingual deployments.** Quality varies a lot across models in non-English languages. Claude and Gemini outperform GPT-4o in several Romance and East Asian languages in our internal tests. Whisper is the strongest open option for lower-resource languages. Evaluation in target languages is essential: most teams discover quality issues only after a multilingual launch goes wrong.


The most important point: model choice should be empirical rather than narrative. Run the same test scenarios across multiple candidate stacks and compare the results on the metrics that matter most for your deployment. In our work with voice AI teams, we routinely see the “obvious” model choice underperform on a customer’s specific data by double-digit percentage points compared to a less-hyped alternative, which is exactly why we built[Coval’s simulation engine](https://docs.coval.dev/concepts/simulations/overview) for side-by-side comparisons.


## How to evaluate voice AI models for your use case


Model comparison without evaluation is vibes-based decision making. Choosing among voice AI models is a measurement problem rather than a marketing problem, and the reason most teams end up at Coval. The pattern that works for teams shipping voice AI at scale:


**Start with a representative test set.** A library of 100 to 500 conversational scenarios drawn from your actual use case, with expected outcomes defined. The scenarios should cover happy paths, edge cases, audio variation, and adversarial conditions. We documented how this works in practice in our[test sets concept guide](https://docs.coval.dev/concepts/test-sets/overview) .


**Run the same set across candidate models.** Same prompts, same scenarios, same grading criteria. The point is comparability: if you change anything besides the model, you can’t attribute differences to model choice.


**Grade across multiple dimensions.** Functional correctness (did it complete the task), tool-call accuracy (did it call the right APIs with the right parameters), behavioral quality (was the tone right, did it ask the right questions), latency (was it fast enough). A model that wins on one dimension and loses on another is a real result rather than a tie. See[voice AI evaluation in 2026](https://www.coval.ai/blog/voice-ai-evaluation-in-2026-the-5-metrics-that-actually-predict-production-success) for the five metrics that predict production success.


**Test on noisy and accented audio.** Studio-quality test audio will not predict production performance. Use real phone audio if you have it, or synthetic audio with realistic noise and accent variation if you don’t. Diverse[personas](https://docs.coval.dev/concepts/personas/overview) are how we surface accent and demographic edge cases in simulations.


**Run a regression suite on every model update.** Vendor model versions change. GPT-4o today is not GPT-4o six months ago. Without a regression suite, model updates silently change your agent’s behavior. The[three-layer testing framework](https://www.coval.ai/blog/the-three-layer-testing-framework-for-voice-ai-regression-adversarial-and-production-derived) (regression, adversarial, and production-derived) is the structure we recommend.


Teams that do this catch problems weeks earlier than teams that rely on production traffic to surface issues. The teams that don’t end up explaining to leadership why a model upgrade that was supposed to improve performance instead caused a double-digit drop in customer satisfaction. That’s the asymmetric outcome that makes evaluation infrastructure load-bearing.


Our guide on[voice AI agent evaluation](https://www.coval.ai/blog/voice-ai-agent-evaluation-guide) covers the methodology in depth. The shorter[how to evaluate voice agents](https://www.coval.ai/blog/how-to-evaluate-voice-agents-a-practical-guide-to-testing-and-quality-assurance) is a faster read if you’re just getting started, and[voice AI evaluation infrastructure: why most teams skip it](https://www.coval.ai/blog/voice-ai-evaluation-infrastructure-why-most-teams-skip-it-and-how-to-build-it) explains the build-or-buy decision.


## Common mistakes when choosing voice AI models


A few patterns that show up repeatedly in conversations with teams shipping voice AI.


**Choosing on benchmarks that are not representative.** Public LLM benchmarks (MMLU, HumanEval, etc.) measure things that have little to do with whether the model will work for a healthcare scheduling agent or a drive-through order taker. Vendor-published voice benchmarks tend to be even less representative because vendors choose conditions favorable to their model. Build your own test set against your own data.


**Picking the most capable model when the use case doesn’t need it.** A scheduling agent doesn’t need the top tier of any frontier family. Gemini Flash or GPT-4o will handle the task at a fraction of the cost and lower latency. Over-specifying the model is one of the most common ways teams burn money in voice AI deployments.


**Ignoring tool-call quality.** Most voice AI failures in production are tool-call failures rather than language failures: wrong parameter, wrong API, missing argument. Some models are better at tool calling than their reputation in chat benchmarks suggests. Evaluate this explicitly.


**Underestimating the TTS contribution to perceived quality.** Callers can’t tell the difference between GPT-4o and Claude Sonnet, but they can tell the difference between ElevenLabs and Amazon Polly. The TTS choice often has more impact on caller perception than the LLM choice.


**No regression testing across model versions.** Vendors update their models without changing the version string. A “stable” model today behaves measurably differently from the same model six months ago. Without regression testing, these silent updates are how agents degrade over time. We dug into this pattern in[voice AI continuous improvement](https://www.coval.ai/blog/voice-ai-continuous-improvement-how-to-build-learning-systems-that-get-better-over-time) .


## What the leading voice AI deployments look like in 2026


A representative production architecture for a mid-to-large voice AI deployment in 2026, and the voice AI model choices inside it:


- **Telephony layer.** Twilio, Telnyx, or a managed offering from the voice agent platform.
- **STT.** Deepgram Nova-3 streaming, with fallback to Whisper for non-English or harder audio.
- **LLM.** Claude Sonnet or GPT-4o for the primary agent. The top Opus or GPT tier for a planner role in multi-agent deployments.
- **TTS.** ElevenLabs or Cartesia, depending on whether quality or latency dominates.
- **Orchestration.** Vapi, Pipecat, or a custom orchestrator on LiveKit.
- **Evaluation infrastructure.** A scenario library running on every release, plus production observability surfacing behavioral and tool-call metrics in real time.


The exact stack varies by use case and budget, but the structural pattern is consistent: cascaded pipelines, premium components in the layers callers perceive directly (TTS), cost-optimized components in the layers they don’t (STT, fallback LLMs), and evaluation infrastructure underneath all of it. The[voice AI platform comparison](https://www.coval.ai/blog/voice-ai-platform-comparison-2026-benchmarks-performance-data-and-how-to-choose) is a deeper benchmark companion to this guide.


## Where to go from here


The voice AI model space is going to keep moving. New entrants, new capabilities, and new architectural patterns will continue to reshape what’s possible. The teams that ship voice AI well don’t try to predict the winners; they build evaluation infrastructure that lets them compare candidates rigorously and switch when the data justifies it.


If you’re at the stage of choosing or comparing models for a voice AI deployment, the right next step is building the test set that will let you compare them on your data rather than vendors’ marketing data. Our[guide on voice AI agent evaluation](https://www.coval.ai/blog/voice-ai-agent-evaluation-guide) walks through the methodology, and the team at[Coval](https://www.coval.ai/) works with voice AI teams to set this up.[Book a call with the Coval team](https://cal.com/forms/b7126bad-a5c3-4ed0-af4b-fdfd4b268381) if you want to talk through what evaluation looks like for your stack.


## Frequently asked questions


**Should I use a cascaded pipeline or a speech-to-speech model in 2026?**


For most enterprise deployments, cascaded. The observability advantage matters more than the latency improvement from speech-to-speech for almost any use case where you need to audit, debug, or evaluate the agent rigorously. Speech-to-speech is the right choice when latency is the deciding factor and you can accept reduced observability. The fuller breakdown is in our[speech-to-speech vs. cascaded](https://www.coval.ai/blog/speech-to-speech-vs-cascaded-voice-ai-which-architecture-should-you-deploy) post.


**Is GPT-4o still the best LLM for voice AI?**


It’s still the most common default. Claude Sonnet is often picked for deployments where multi-turn instruction following matters most. Gemini Flash is often picked for cost-sensitive deployments. The best choice depends on your specific constraints: latency, cost, capability, multilingual quality. Empirical comparison on your own data is the right answer. We see teams running[4-way model bake-offs](https://docs.coval.dev/concepts/simulations/overview) in Coval before they commit to a primary LLM.


**How much does the choice of TTS matter compared to the LLM?**


In most cases, the TTS choice has more impact on caller perception than the LLM choice. Callers don’t notice that the LLM picked the slightly better word; they notice that the voice sounds natural or robotic. Premium TTS (ElevenLabs, Cartesia) is one of the highest-ROI investments in a voice AI stack. See our deeper comparison in[ElevenLabs vs. Cartesia](https://www.coval.ai/blog/elevenlabs-vs-cartesia-which-tts-provider-is-right-for-your-voice-ai-project) .


**Can I run voice AI on open-source models?**


Yes, if you have the volume and engineering capacity. Recent Llama and Mistral releases are competitive in cascaded voice deployments. The break-even point versus commercial APIs depends on engineering cost, GPU pricing, and your traffic shape; for most teams in our customer base it lands in the high-hundreds-of-thousands of calls per month before self-hosting beats API pricing.


**How often should I re-evaluate my model choice?**


The major vendors release meaningfully improved models every 6 to 12 months. A formal re-evaluation against your test set every 6 months catches the cases where switching makes sense. More frequently than that is usually overkill unless the model is materially under-performing.


**What’s the difference between a voice AI model and a voice AI agent?**


The model is the underlying LLM that generates the response. The agent is the full system that uses the model: speech-to-text, the LLM, text-to-speech, tools, orchestration, and the prompts and policies that govern behavior. When teams say “we use Claude for voice,” they mean Claude is the LLM inside their cascaded pipeline rather than that Claude is doing the speech parts. Our[voice AI platform architecture](https://www.coval.ai/blog/voice-ai-platform-architecture-why-multi-model-systems-outperform-single-llms) post unpacks this distinction.
