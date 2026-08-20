---
schema_version: "1.0.0"
document_id: "4ecdb8f8fc7f10cca76d3ce63667584e555ef59a5fc7d19682f828545e61a551"
company_key: "yc-sigmamind-ai"
company: "SigmaMind AI"
source_id: "yc-sigmamind-ai-news-import-39ccea9bc83d"
canonical_url: "https://www.sigmamind.ai/blog/building-voice-agents-that-actually-sound-human"
published_at: null
first_seen_at: "2026-07-22T13:27:04.229547+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:b1c4e521bbf4439ba0d4ffdd5fa309359080bc0c3f20f7cbb81221ccfb183d59"
---

# Building Voice Agents That Actually Sound Human: 2026

## TL;DR


Most voice agents sound robotic not because of poor voice quality, but because of bad timing, high latency, and lack of context. Voice quality accounts for roughly 10% of the problem. This glossary covers every technical term in the voice agent stack, organized by pipeline stage, with real benchmarks and thresholds so you can diagnose exactly where your agent breaks down and fix it.


---


Your voice agent uses a state-of-the-art TTS engine. The voice itself sounds indistinguishable from a real person. But callers still hang up, still say “let me talk to a real person,” still describe the experience as robotic.


The disconnect is simple: sounding human is not a voice problem. It’s an architecture problem, a timing problem, and a context problem. As one practitioner insight from Alchemyst AI puts it, the leading TTS models in 2026 produce speech that’s virtually indistinguishable from human voice in controlled settings, and the “robotic” perception comes from everything else in the stack.


This glossary exists to give you the vocabulary for building voice agents that actually sound human. It’s not organized alphabetically. Instead, every term maps to the specific layer of the voice pipeline where it lives, so when something sounds off, you know exactly where to look.


If you’re evaluating platforms for this kind of work, SigmaMind’s[voice AI platform](https://www.sigmamind.ai/platform) is designed around model-agnostic orchestration with sub-800ms latency targets, which puts it squarely in the “production sweet spot” you’ll read about below.


---


## The Voice Agent Pipeline: Foundational Terms


Before getting into individual layers, you need the big picture. Every voice agent, regardless of platform, follows some version of a pipeline that converts speech to meaning and meaning back to speech. The architecture you choose determines your latency floor, your debugging options, and ultimately whether your agent sounds human or mechanical.


### Voice Agent


A software system that conducts spoken conversations with humans, typically over phone (PSTN/SIP) or web (WebRTC). Unlike chatbots that process text, voice agents must handle the full complexity of spoken language: accents, interruptions, background noise, emotional tone, and the brutal timing requirements of real conversation.


### Cascade (Pipeline) Architecture: STT → LLM → TTS


The dominant production architecture in 2026. Three separate models work in sequence:


1. **STT (Speech-to-Text)** converts the caller’s audio into text (100-300ms)
2. **LLM (Large Language Model)** processes that text and generates a response (300-900ms)
3. **TTS (Text-to-Speech)** converts the response back into audio (300-1,200ms)


Each layer adds latency. The total compounds. This is why[production voice AI delivers 1,400-1,700ms at median](https://hamming.ai/resources/voice-ai-latency-whats-fast-whats-slow-how-to-fix-it) , far above the 200-500ms window that feels natural in human conversation.


The advantage of cascade: you can swap individual components (use Deepgram for STT, switch LLMs for cost, pick a different TTS voice) and debug each layer independently. For a deeper look at how this architecture works in practice, see this guide on[building real-time voice agents](https://www.sigmamind.ai/blog/building-enterprise-realtime-voice-agents-from-scratch) .


### Speech-to-Speech (S2S) Architecture


A single model that takes audio in and produces audio out, skipping the text intermediary entirely. Think of it as one neural network that “hears” and “speaks” without ever writing anything down.


S2S promises dramatic latency reduction (up to 85% in benchmarks) and better emotional prosody preservation because vocal nuance never gets stripped away by transcription. But the tradeoffs are real. Enterprise adoption sits[below 15% in early 2026](https://www.coval.ai/blog/speech-to-speech-vs-cascaded-voice-ai-which-architecture-should-you-deploy) , with H2 2026 identified as the inflection point.


Why the slow adoption? Three reasons:


- **Debuggability.** When something goes wrong in a cascade, you can read the transcript, see what the LLM decided, and listen to the TTS output. With S2S, it’s a black box.
- **Compliance.** Regulated industries need transcripts, audit trails, and the ability to explain decisions. S2S makes this harder.
- **Cost unpredictability.** Cascade costs run[$0.0095 to $0.17 per minute](https://speko.ai/blog/s2s-vs-cascaded) . S2S costs range from $0.00165/min (Gemini) to $0.30/min (OpenAI Realtime GPT-4o) and grow with conversation length.


**When to use S2S:** Short, emotionally nuanced interactions where latency matters more than auditability. Think concierge bots, not insurance claims.


**When to stick with cascade:** Anything requiring tool calling, compliance logging, or predictable costs. Which is most enterprise voice AI.


### Full-Duplex


A voice agent that can listen and speak simultaneously, just like humans do. Most current agents are half-duplex: they either listen or speak, taking turns. NVIDIA’s PersonaPlex represents the frontier here, a full-duplex model that learns not just what to say but when to pause, interrupt, or backchannel.


Full-duplex is not yet mainstream in production, but it’s the direction the industry is heading because it solves many of the turn-taking problems discussed later.


### Model-Agnostic Stack


An architecture where you can mix and match STT, LLM, and TTS providers without rebuilding your agent. This matters because the “best” provider for each layer changes constantly, and different use cases have different optimal configurations. A debt collection call might prioritize STT accuracy over TTS naturalness, while a luxury retail agent might need the opposite. Learn more about[model-agnostic orchestration](https://www.sigmamind.ai/blog/what-is-model-agnostic-orchestration-for-conversational-ai) and why it’s become a core platform requirement.


---


## Listening: How Agents Hear (The STT/ASR Layer)


The first step in building voice agents that actually sound human starts with hearing accurately. If your agent misunderstands the caller, everything downstream fails, and no amount of beautiful TTS output will save it.


### Speech-to-Text (STT) / Automatic Speech Recognition (ASR)


The model that converts spoken audio into text. These terms are used interchangeably in practice, though ASR is the older academic term and STT is more common in modern voice AI stacks.


STT errors cascade through the entire system. If the STT hears “I need to cancel my flight” as “I need to cancel my fight,” the LLM generates the wrong response, and the caller hears something nonsensical delivered in a perfectly natural voice. This is another version of the uncanny valley: correct behavior said wrong is more frustrating than obviously robotic behavior.


A critical insight from Speechmatics captures this perfectly: any latency metric needs the context of accuracy, because a bot that takes 100ms longer to respond but doesn’t force the user to repeat themselves is the better experience.


### Word Error Rate (WER)


The standard accuracy metric for STT. It measures transcription errors (substitutions, deletions, and insertions) as a percentage of total words. Lower is better.


**Benchmark:** Deepgram Nova-3 achieves[6.84% WER on streaming audio](https://nextlevel.ai/best-speech-to-text-models/) across 2,703 production audio files spanning nine domains.


**Common mistake:** Optimizing for WER on clean benchmark audio. Production calls include background noise, accents, speakerphones, and callers who mumble. Always test with realistic audio. For a detailed comparison of engines suited to production environments, see this guide to[STT engines for contact centers](https://www.sigmamind.ai/blog/best-contact-center-speech-to-text-engines) .


### Streaming vs. Batch Transcription


Streaming STT processes audio in real time, sending partial transcripts as the caller speaks. Batch STT waits for the full audio clip before transcribing.


For voice agents, streaming is non-negotiable. Batch adds seconds of latency, which destroys conversational flow. Streaming lets the LLM begin processing before the caller finishes speaking, shaving hundreds of milliseconds off total response time.


### Noise Suppression


Pre-processing that removes background sounds (traffic, office chatter, wind) before audio reaches the STT model. Without it, STT accuracy degrades significantly in real-world conditions. Most production stacks include a noise suppression layer, either built into the STT provider or as a separate pre-processing step.


### Diarization


The ability to distinguish between different speakers in an audio stream. Less critical for 1:1 phone calls (where there’s one caller and one agent), but essential for conference calls, multi-party support scenarios, or post-call analytics where you need to attribute statements to specific speakers.


---


## Thinking: How Agents Decide What to Say (The LLM Layer)


The LLM is the brain of your voice agent, and it’s also the biggest bottleneck. It accounts for[roughly 70% of total pipeline latency](https://hamming.ai/resources/best-voice-agent-stack) , making model selection the single most impactful architectural decision for response speed.


### LLM Inference


The process of a large language model generating a response. In a voice agent context, inference means taking the transcribed caller input (plus system prompt, conversation history, and any retrieved context) and producing the agent’s next utterance.


The speed of inference is measured primarily by time-to-first-token.


### Time-to-First-Token (TTFT)


How quickly the LLM produces its first word of output. This metric matters more than total generation time because of streaming: the TTS can begin synthesizing audio as soon as the first few tokens arrive, rather than waiting for the complete response.


Faster TTFT means the caller hears the agent start speaking sooner, which dramatically affects the perception of responsiveness. The difference between a 200ms and 600ms TTFT is the difference between “this feels like talking to a person” and “this feels like talking to a system.”


### Prompt Engineering for Voice


Here is where building voice agents that actually sound human gets surprisingly non-technical. LLMs are trained on text, and they’re post-trained to produce clean, grammatically correct writing. That’s great for chatbots and emails, but it’s not how humans talk.


Practitioners at LiveKit have documented this extensively: real speech is full of filler words, mid-sentence course corrections, little laughs, soft pauses, and sentences that meander. The model will fight you unless you’re very explicit about what natural speech sounds like.


Practical techniques that work:


- **Use “calm” emotion tags over “big” ones.** Tags like “peaceful” or “warm” tend to sound more human than “excited” or “enthusiastic,” which often come across as performative.
- **Study real call recordings.** If you have recordings of human agents, look for speech patterns you want your AI to replicate. Then describe those patterns explicitly in the prompt.
- **Inject disfluencies deliberately.** Tell the model it’s okay to say “let me check on that” or “so what I’m seeing is…” rather than launching into perfectly structured responses.
- **Keep responses short.** Human agents don’t monologue. They speak in 1-2 sentence bursts and then listen.


### Function Calling / Tool Calling


The mechanism by which a voice agent does things, not just talks. Function calling lets the LLM trigger external actions: look up an order, process a refund, check appointment availability, update a CRM record.


This matters enormously for sounding human because a voice agent that sounds perfectly natural but can’t actually help is, as one practitioner put it, just a very expensive IVR. Explore[pre-built integrations](https://www.sigmamind.ai/app-library) to see how tool calling connects to CRMs, helpdesks, and e-commerce systems.


### Hallucination


When the LLM generates information that isn’t true. In a text chatbot, this is annoying. In a voice agent handling financial transactions or medical scheduling, it’s dangerous. Hallucination is harder to catch in voice because callers can’t “re-read” what the agent said, and they may not realize the information was fabricated.


### Guardrails


Rules and constraints that prevent the LLM from going off-script, sharing incorrect information, or engaging with topics outside its scope. In voice agents, guardrails need to be faster and stricter than in text because there’s no “undo” button in a live phone call.


---


## Speaking: How Agents Sound (The TTS Layer)


TTS is what most people think of first when they talk about building voice agents that actually sound human. It’s the most audible layer, the one callers notice immediately. But as we’ve established, it’s roughly the last 10% of the problem.


That said, 10% still matters. A great TTS engine with poor prosody will undo all the latency optimization you’ve done elsewhere.


### Text-to-Speech (TTS)


The model that converts the LLM’s text response into spoken audio. Modern neural TTS engines produce remarkably natural speech, with the top commercial and open-source models now[overlapping in the 4.5-4.8 MOS range](https://www.codesota.com/speech) , a gap small enough that model selection often comes down to latency, licensing, or voice cloning rather than raw quality.


### Mean Opinion Score (MOS)


The standard measure of voice naturalness, scored by human raters on a 1-5 scale. A MOS of 5.0 would be indistinguishable from a real human. Current state-of-the-art TTS models cluster between 4.5 and 4.8.


**Common mistake:** Chasing higher MOS scores when your agent’s response latency is above 1 second. The “uncanny valley of timing,” as one Medium practitioner described it, means that human-quality voice combined with robot timing is more jarring than obviously robotic voice with fast response. Users report feeling more frustrated with high-quality, slow AI than with low-quality, fast AI.


### Time-to-First-Audio (TTFA)


How quickly the TTS engine begins producing audio output after receiving text input. This is the TTS equivalent of TTFT for LLMs.


**2026 benchmarks:**


- ElevenLabs Flash v2.5:[75ms TTFA](https://www.assemblyai.com/blog/top-text-to-speech-apis) , engineered specifically for low-latency voice agents
- Cartesia Sonic: Under 150ms, the lowest general-availability latency in the industry
- Deepgram Aura: Under 250ms, optimized for streaming conversational AI


### Prosody


The rhythm, stress, and intonation of speech. Prosody is what makes the difference between “I can help you with that” (warm, reassuring) and “I can help you with that” (flat, mechanical). Good TTS prosody includes natural emphasis on important words, appropriate pausing between clauses, and pitch variation that matches the emotional content.


### Streaming TTS


TTS that begins playing audio before the full response is synthesized. The TTS engine starts converting the first few words to audio while the LLM is still generating the rest of the response. This is essential for achieving sub-800ms end-to-end latency because it lets the three pipeline stages overlap rather than run sequentially.


### Voice Cloning


Creating a synthetic voice that matches a specific person’s vocal characteristics. Used for brand consistency (the same “agent voice” across all calls) or for creating agents that sound like specific team members. Voice cloning quality has improved dramatically, but it raises ethical and legal considerations around consent and deepfake potential.


### SSML (Speech Synthesis Markup Language)


An XML-based markup language that gives you fine-grained control over TTS output. You can specify pauses, emphasis, pronunciation, speaking rate, and pitch changes. SSML is useful for edge cases (pronouncing product names, spelling out confirmation codes) but adds complexity. Many modern TTS APIs offer simpler alternatives through emotion tags or natural language instructions.


### Emotion Infusion


Controlling the emotional tone of TTS output. This can be done through SSML, emotion tags in the TTS API, or through prompt engineering that signals the desired tone to the LLM (which then gets reflected in the TTS output through word choice and phrasing).


For guidance on choosing the right TTS provider across languages and use cases, this comparison of[multilingual TTS engines](https://www.sigmamind.ai/blog/multilingual-tts-best-engines-for-voice-agents) covers the current options.


---


## Timing: The Conversational Dance (The Turn-Taking Layer)


This is arguably the most under-discussed factor in building voice agents that actually sound human. Practitioners consistently report that timing issues cause more “robotic” complaints than voice quality issues. As one builder on a voice AI forum put it: voice is the hardest channel to make feel natural because conversational timing is brutal. Humans interrupt, hesitate, trail off, speak over background noise, and start talking before the other side is done. If your AI agent cannot handle that turn-taking dance, everything else barely matters.


### Voice Activity Detection (VAD)


The most basic mechanism for determining when a caller is speaking and when they’ve stopped. VAD typically combines volume thresholds with a small ML model (Silero is the most common) that detects the presence of human speech. The agent responds once it hasn’t detected speech for a configurable number of milliseconds.


**Benchmark:** In realistic conversations,[600ms is the bare minimum](https://cresta.com/blog/engineering-for-real-time-voice-agent-latency) silence threshold. Even at 600ms, agents often misjudge pauses during tasks like spelling out numbers or reading confirmation codes.


**Why it matters:** VAD determines when your agent starts “thinking.” Set the threshold too low and the agent interrupts callers mid-thought. Set it too high and the agent feels sluggish. There’s no perfect setting because different callers and different moments within the same call require different thresholds.


### Turn Detection / End-of-Utterance (EOU) Detection


A more sophisticated approach than raw VAD. Turn detection models use semantic signals (not just silence) to determine whether the caller has finished their thought. For example, “My order number is…” followed by a 400ms pause is clearly not finished. A pure VAD system might start responding; a good turn detection model waits.


This distinction matters because it’s the difference between an agent that feels attentive and one that feels jumpy. You can[test and debug voice flows](https://www.sigmamind.ai/playground) before going live to catch these edge cases early.


### Barge-In


When a caller starts speaking while the agent is still talking. This happens in[about 1 in 5 calls](https://poly.ai/blog/barge-in-voice-ai-interruption-handling) , and it’s the single most decisive factor in whether voice AI feels human or robotic.


Proper barge-in handling means the agent:


1. Detects the interruption quickly
2. Stops speaking
3. Listens to what the caller is saying
4. Responds appropriately to the new input


Poor barge-in handling means the agent either ignores the interruption (talking over the caller) or stops at every tiny sound (becoming jittery and unreliable).


### Backchannel


Short verbal signals that indicate listening without intending to take a turn: “mm-hmm,” “yeah,” “right,” “okay.” These are the nemesis of simple VAD systems.


The problem: most voice agents treat every detected speech as a full interruption. But many sounds can trigger detection, including brief backchannels, sighs, coughs, or background sounds. Treat every one of those as a full interruption and your agent becomes jittery and robotic, constantly stopping mid-sentence because the caller said “uh-huh.”


The solution requires semantic analysis that distinguishes between “I’m listening, keep going” and “I want to say something.” This is an active area of development, with LiveKit’s adaptive interruption model reportedly[detecting true barge-ins faster than VAD in 64% of cases](https://livekit.com/blog/adaptive-interruption-handling) .


### Adaptive Interruption Handling


ML-based systems that go beyond binary VAD to classify interruptions by intent. Rather than a simple “speech detected / no speech detected” signal, adaptive systems categorize incoming audio as: backchannel (ignore), attempted barge-in (stop and listen), background noise (ignore), or hesitation/filler (wait).


### Dead Air


Silence during the conversation when neither party is speaking. Small amounts are natural (humans pause to think). Extended dead air (1.5+ seconds) signals that something has gone wrong. The agent might be waiting for LLM inference, dealing with a failed API call, or stuck in a bad state.


**Practical fix:** Insert filler phrases (“Let me look that up for you,” “One moment while I check”) when the system knows a response will take longer than normal. This is a simple technique that dramatically improves perceived naturalness because it mirrors what human agents do.


### Silence Threshold


The configurable duration of silence that the system interprets as “the caller has finished speaking.” This is the “x” in the VAD equation: respond once you haven’t seen human speech for x milliseconds.


Too short (under 400ms): constant false triggers and interruptions.
Too long (over 1,200ms): the agent feels unresponsive.
Sweet spot for most deployments: 600-800ms, with semantic turn detection handling the edge cases.


---


## Connecting: How Agents Reach the Phone (The Telephony Layer)


Telephony is the invisible layer that practitioners often overlook until it adds 300ms of unexplained latency. Even the fastest STT + LLM + TTS pipeline will sound sluggish if the telephony infrastructure is slow.


### SIP Trunking


The protocol that connects your voice agent to the phone network. SIP (Session Initiation Protocol) handles call setup, teardown, and media transport. Quality SIP trunking adds 100-300ms of latency. Cheap providers can add 500ms or more, which alone can push you from “natural” into “noticeably delayed.”


### BYOC (Bring Your Own Carrier)


The ability to use your existing telephony provider (Twilio, Telnyx, etc.) rather than being locked into a voice AI platform’s built-in carrier. BYOC matters for enterprises that have negotiated rates, have existing phone numbers, or need specific geographic coverage. For setup instructions, see this guide on[configuring SIP with Twilio and Telnyx](https://www.sigmamind.ai/blog/how-to-configure-sip-with-twilio-and-telnyx-for-byoc) .


### WebRTC


A browser-based real-time communication protocol. WebRTC typically offers lower latency than PSTN because it stays on the internet rather than traversing the traditional phone network. It’s used for web-based voice agents, in-app voice, and situations where you control both endpoints.


### PSTN (Public Switched Telephone Network)


The traditional phone network. PSTN audio is limited to 8 kHz sampling rate, which is significantly lower quality than the 16-48 kHz audio that most modern STT and S2S models are trained on. This means your models may perform worse on actual phone calls than in development testing, a common source of “it worked in the demo” frustration.


### Jitter Buffer


A small buffer that smooths out variations in network packet delivery. Network packets don’t arrive at perfectly regular intervals, and without jitter buffering, you’d hear audio glitches and dropouts. The tradeoff: bigger buffers mean smoother audio but add latency. Smaller buffers mean lower latency but risk audio artifacts.


---


## Performing: How Agents Actually Help (The Action Layer)


A voice agent that sounds perfectly natural but can’t do anything useful is just expensive hold music. The action layer is what separates a demo from a production system, and it’s a critical but often underappreciated part of building voice agents that actually sound human. Why? Because when an agent has to say “I’m sorry, I can’t help with that, let me transfer you,” the illusion of humanness shatters.


### API Integration


Connections between the voice agent and external systems (CRMs, order management, scheduling tools, payment processors). These integrations are what enable the agent to actually resolve issues rather than just talk about them.


### Warm Transfer


Handing a call from the AI agent to a human agent with full context preserved. The human receives a summary of the conversation, the caller’s intent, any account details already gathered, and the reason for escalation. This prevents the dreaded “can you repeat everything you just told the robot?” experience.


Handing off to a human isn’t a failure state. It’s a feature.[Well-designed escalation flows](https://www.sigmamind.ai/blog/how-to-escalate-calls-to-humans-without-losing-context) often result in higher customer satisfaction than an AI that struggled through something it couldn’t handle.


### Context Preservation / Session State


The agent’s ability to remember what happened earlier in the conversation and, ideally, across previous interactions. This is what the Zowie team was pointing to when they said the “robotic” problem lives in the architecture underneath the voice: agents that start every call from zero, follow rigid scripts, and ignore everything they should know about the person will sound mechanical regardless of how good the voice is.


Context preservation means the agent knows the caller’s name, their recent orders, their previous issues, and can reference them naturally: “I see you called about this last week, let me pick up where we left off.”


### Escalation Logic


The rules that determine when and how a call should be transferred to a human. Good escalation logic considers: caller sentiment, conversation length, number of failed resolution attempts, topic sensitivity, and explicit requests for a human.


**Common mistake:** Setting escalation thresholds too high because transfers feel like “failure.” In practice, a smooth handoff at the right moment delivers better outcomes than an AI agent circling on an issue it can’t resolve.


---


## Measuring: How to Know If Your Agent Sounds Human (The Analytics Layer)


You can’t improve what you don’t measure. These metrics tell you whether your efforts at building voice agents that actually sound human are working, or if you’re just making expensive assumptions.


### Voice-to-Voice Latency (V2V)


The single most important metric. Measured from the moment the caller stops speaking to the moment the agent’s audio begins playing. This encompasses the entire pipeline: VAD processing + STT + LLM inference + TTS synthesis + network transmission.


**Thresholds that matter:**


Latency Experience


Under 300ms[Feels magical](https://hamming.ai/resources/voice-ai-latency-whats-fast-whats-slow-how-to-fix-it) . Users can’t distinguish it from a responsive human.


300-800ms Production sweet spot. Natural flow maintained.


800-1,200ms Users notice but adapt. Requires careful turn detection.


Above 1,500ms Conversation breaks down. Users talk over the agent and abandon calls.


**Reality check:** Production voice AI currently delivers 1,400-1,700ms at median. Contact centers report[40% higher call abandonment](https://telnyx.com/resources/voice-ai-agents-compared-latency) when agents take longer than 1 second to respond.


This is why latency optimization matters more than voice quality for most teams. To understand the cost implications of optimizing each layer, SigmaMind’s[pricing page](https://www.sigmamind.ai/pricing) breaks down per-layer costs transparently.


### TTFB (Time-to-First-Byte)


How quickly the first byte of audio data is produced by the TTS engine after receiving text input. This is the TTS-specific latency metric and is a subset of total V2V latency.


### VART (Voice Agent Response Time)


An alternative name for V2V latency used by some platforms. Same concept, different label.


### Cost per Call


Total cost of all AI services consumed during a single call: STT minutes + LLM tokens + TTS characters + telephony minutes. In a cascade architecture, this is predictable and auditable by layer. In S2S, it’s harder to decompose.


**Benchmark:** Cascade costs range from $0.0095 to $0.17 per minute depending on model choices. Understanding your cost drivers layer by layer is critical for optimization. A guide on[tracking per-call AI model costs](https://www.sigmamind.ai/blog/tracking-per-call-ai-model-usage-and-cost-guide) walks through this in detail.


### Call Abandonment Rate


The percentage of callers who hang up before their issue is resolved. Track this against latency to see the direct relationship between response speed and caller patience.


### False Interruption Rate


How often the agent incorrectly detects a barge-in when the caller was just backchanelling, coughing, or producing background noise. High false interruption rates make the agent sound nervous and erratic, constantly stopping and starting.


### Missed Barge-In Rate


The flip side: how often the agent fails to detect a genuine interruption, continuing to talk while the caller is trying to say something. High missed barge-in rates make the agent sound oblivious and rude.


**Important:** Track these separately from latency. An agent can have excellent latency but terrible interruption handling, and the caller experience will still feel robotic.


### Resolution Rate


The percentage of calls where the caller’s issue was fully resolved without human escalation. This is the ultimate measure of whether your voice agent is actually useful, not just pleasant to listen to.


---


## The 10/90 Rule: Why Voice Quality Is Not Your Real Problem


If there’s one framework worth internalizing from this entire glossary, it’s this: voice quality is about 10% of why callers perceive an agent as robotic. The other 90% breaks down into:


- **Latency (40%):** Slow responses destroy the illusion of conversation
- **Turn-taking and timing (25%):** Bad barge-in handling, missed backchannels, dead air
- **Context and intelligence (15%):** Starting every call from scratch, rigid scripts, inability to do things
- **Prosody and emotion mismatch (10%):** Cheerful tone when acknowledging a complaint, flat tone when congratulating


The “uncanny valley of timing” makes this worse than it sounds. When an AI sounds human but responds with robot timing, the cognitive dissonance is more jarring than if it just sounded robotic from the start. Practitioners report that callers are more frustrated with high-quality, slow AI than with low-quality, fast AI.


This means your optimization priority should be: latency first, then turn-taking, then context/actions, then voice quality. Most teams do it backwards.


---


## Frequently Asked Questions


### What latency target should I aim for in a production voice agent?


Aim for 300-800ms voice-to-voice latency. Under 300ms feels indistinguishable from a human. Above 1,200ms and callers start talking over the agent, repeating themselves, and abandoning calls. The current production median of 1,400-1,700ms explains why most deployed voice agents still feel slow.


### Is speech-to-speech (S2S) better than cascade for sounding human?


S2S offers significant latency and prosody advantages, but cascade dominates enterprise deployments in 2026 for good reasons: debuggability, compliance requirements, and predictable costs. Less than 15% of enterprises use S2S today. Most teams should start with an optimized cascade pipeline and evaluate S2S as the technology matures.


### Which layer of the pipeline causes the most latency?


The LLM inference step accounts for approximately 70% of total pipeline latency. This makes LLM selection and optimization (model size, TTFT, streaming output) the highest-leverage improvement for most teams.


### How do I handle barge-in without making my agent jittery?


Move beyond simple VAD to semantic turn detection. Basic VAD treats every sound as a potential interruption, including backchannels like “mm-hmm” and background noise. Adaptive interruption models classify incoming audio by intent, distinguishing between “I’m listening” and “I want to speak,” and handle each appropriately.


### Does better TTS alone make a voice agent sound human?


No. Top TTS models in 2026 already cluster between 4.5 and 4.8 MOS, which is near-human quality. If your agent sounds robotic, the cause is almost certainly in latency, turn-taking, or context handling, not voice synthesis. Improving TTS quality while ignoring these other factors can actually make things worse due to the uncanny valley effect.


### What’s the minimum VAD silence threshold for production?


600ms is the floor for realistic conversations. Below that, you’ll get constant false triggers, especially during tasks that involve natural pauses like spelling names or reading numbers. Pair the silence threshold with semantic turn detection to handle edge cases.


### How much does a cascade voice agent cost per minute?


Cascade costs range from $0.0095 to $0.17 per minute, depending on your choice of STT, LLM, and TTS providers. The modular architecture makes costs predictable and auditable by layer, which is one of its advantages over S2S pricing.


### What’s the difference between warm transfer and cold transfer?


In a cold transfer, the caller is sent to a human agent with no context, forcing them to repeat everything. In a warm transfer, the AI passes along a conversation summary, caller intent, gathered data, and escalation reason so the human agent can pick up seamlessly. Warm transfer with context preservation is one of the most impactful features for caller satisfaction.


---


Building voice agents that actually sound human is an engineering discipline, not a magic trick. Every term in this glossary maps to a specific, measurable part of the pipeline. When something sounds off, you now have the vocabulary to diagnose exactly where it’s breaking and the benchmarks to know what “good” looks like.


Ready to put this into practice?[Start building for free](https://www.sigmamind.ai/sign-up) on a platform that gives you model-agnostic control over every layer discussed here.
