---
schema_version: "1.0.0"
document_id: "311ae0a8da4db86b302b0300cf56dbdfd4567a69e42ec934680a99827bebc042"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-endpointing-voice-ai-agents"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T18:46:06.789725+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:44d048787588edc6b20f9ca418580dc1bd780743cebc0ae355522a92807ad286"
---

# Testing endpointing and turn detection in voice AI agents

Endpointing is where most voice-agent complaints actually come from. The transcript looks fine. The prompt is fine. The LLM said the right thing. But the caller feels talked over, or waits a full second staring at silence before the agent responds, and the call score drops before the agent ever gets to the useful part.


That failure mode is not a model quality problem. It is a timing problem inside the turn-taking loop, and it hides from every kind of offline evaluation. If you are shipping a voice agent, endpointing needs its own test surface, with its own metrics, run against audio that behaves like production.


## What endpointing actually is


Two things get conflated constantly. Voice activity detection (VAD) is frame-level: is this 20ms of audio speech or not. Endpointing is turn-level: has the caller finished their thought and expect a reply.[FutureAGI's writeup calls out the confusion directly](https://futureagi.com/glossary/voice-activity-detection/) , noting that "VAD labels speech frames; endpointing decides the user turn is complete." Same signal chain, different decisions, different failure modes.


The classical approach stacks them: VAD flips to silence, a timer starts, once the silence timer crosses some threshold you call the turn complete.[LiveKit's docs describe this as the default](https://docs.livekit.io/agents/build/turns/) : most turn detection techniques use VAD to detect silence, then apply heuristics on top to decide the end of a phrase.


That threshold is the source of most of the pain. Set it short and the agent cuts callers off during natural pauses. Set it long and the agent feels laggy.[ElevenLabs frames it as the biggest controllable latency in the pipeline](https://elevenlabs.io/blog/voice-agent-latency-optimization) : a 700ms silence window adds 700ms to every single turn, invisible in transcription benchmarks and painfully visible on a live call.


The industry has been moving off pure silence-based endpointing for a while.[LiveKit shipped Turn Detector v1 in June 2026](https://livekit.com/blog/solving-end-of-turn-detection) , fusing semantic and acoustic signals into one end-of-turn prediction. AssemblyAI ships semantic endpointing inside their streaming STT. OpenAI's Realtime API exposes a` semantic_vad` turn-detection mode.[Pipecat's Smart Turn v2](https://pypi.org/project/livekit-plugins-turn-detector/) and the LiveKit turn detector plugin do the same job at the framework layer. The choices are better than they were a year ago. The testing story has not moved much.


Where endpointing sits


## The two failures, and why they need separate tests


Endpointing fails in exactly two ways, and they are not symmetric.


**False cutoff.** The agent starts speaking while the caller is still mid-thought. This happens when the silence threshold is too aggressive, when the semantic model is overconfident, or when a caller pauses more than the model was trained for. On a live call it feels like being interrupted by a human who is not listening. Recovery is expensive: the agent has to stop, the caller has to restart, and by the time the turn resolves the caller has lost trust in the interaction.


**Dead air.** The caller finishes and nothing happens. Silence threshold is too conservative, semantic model wants more evidence, ASR is still finalizing.[OnCallClerk's latency writeup](https://oncallclerk.com/blog/how-to-reduce-latency-voice-ai-agents) notes that STT provider defaults deliberately err this direction because "false interruption is way more annoying than dead air." That may be true at 100ms scale; it is not true at 900ms scale. Callers hang up.


These are separate metrics, and averaging them together produces meaningless results. An agent with a 2% false cutoff rate and 1.5s median dead air is not "0% wrong on turn-taking." It is two different agents failing two different callers.


[LiveKit's v1 evaluation](https://livekit.com/blog/solving-end-of-turn-detection) is the right shape here: they plot the tradeoff explicitly, sweeping the policy settings that control how confidently and how quickly the assistant takes the floor, and measuring the tradeoff between response latency and false cutoffs. Their reported numbers put v1 at a 9.9% false-cutoff rate at a 300ms latency budget, against 12.9% for Deepgram Flux and 27.7% for ultraVAD on the same evaluation. Whatever you think of those specific numbers, the shape is right: latency budget on one axis, false-cutoff rate on the other, and you pick your operating point deliberately.


Your test suite needs to produce the same shape for your agent, on your traffic, at your operating point.


## Why offline eval misses endpointing bugs


The failure mode most teams walk into is running endpointing through the same evaluation harness they use for the LLM. That harness usually looks like a transcript dataset, a set of golden responses, and a scorer. It catches nothing about turn-taking.


[FutureAGI's list of common mistakes](https://futureagi.com/glossary/voice-activity-detection/) is worth reading in full, but the shortest version is: tuning only on clean audio, scoring only final transcripts, letting silence thresholds hide latency. Quiet lab clips miss car noise, hold music, speakerphone echo, and overlapping speech. Final-transcript scoring can look acceptable while turn timing still breaks the call.


There are three specific reasons offline eval fails:


1. **Codec effects change VAD behavior.** G.711 at 8kHz sounds different than a 48kHz WebRTC stream. A VAD tuned on the second will fire wrong on the first, and your offline clips are almost certainly the second.
2. **Interim vs final transcripts change the timing.** Semantic endpointing often looks at interim ASR output. Batch eval sees only finals. You cannot reproduce the actual endpointing decision from a batch dataset.
3. **Real callers pause weird.** Hesitation, code-switching, thinking noises, disfluency. Studio audio has none of this. Personas that model the way real callers actually speak are non-optional.


The only test that catches these is one that runs your agent over real telephony, with audio that models the acoustic and prosodic behavior of your traffic, and scores what the agent actually does with the timing.


Offline eval vs simulation-based endpointing test


## What a real endpointing test looks like


A useful endpointing suite has four layers.


**Persona-driven audio.** The caller side of the test needs the properties production audio has: pace variation, hesitation, code-switching, accents you actually see, background environments you actually see. A persona that speaks slowly with mid-sentence pauses will surface false cutoffs no clean-read persona will. A fast-talker with a light stutter will surface different bugs. If your production traffic includes Spanish-accent English and outdoor callers, both need coverage.


**Adversarial timing scenarios.** Design specific scenarios for each failure mode:


- Mid-sentence pauses of 400ms, 800ms, 1200ms after common utterance starts ("I want to book for", "my policy number is").
- Trailing disfluency ("...um" or "...like" after apparent completion) to catch premature commits.
- Fast rapid-fire clarifications to catch dead air between short turns.
- Overlapping speech where the caller barges in during agent playback.


**Scoring on turn-level metrics, not transcript metrics.** Score every turn on:


- False cutoff rate: fraction of user turns where the agent started speaking before the user finished.
- Dead-air p50, p95: elapsed time from actual end-of-user-speech to first agent audio byte.
- Turn-accuracy: fraction of turns where the agent replied to the intended complete utterance, not a partial.
- Barge-in latency: how long it takes the agent to stop when the caller interrupts.


**Runs across configurations.** The same suite should run against each turn-detection mode you might ship, so you can compare.[LiveKit exposes turn_detection as a session parameter](https://docs.livekit.io/agents/v1/build/turn-detection) with modes for realtime models, VAD only, STT endpointing, or the turn detector model. If you might change modes, or your vendor might change defaults, your suite should tell you what breaks.


## Testing across the mode matrix


The trap with the current generation of turn-detection models is that the good ones look great on general benchmarks and fail specifically on your traffic.


An agent handling insurance policy numbers has a different failure surface than one handling restaurant bookings. Callers reading a 12-digit policy number pause in patterns that break naive semantic models. Callers ordering food produce short overlapping turns that break naive silence-based ones.[Gradium's writeup on semantic VAD](https://gradium.ai/content/semantic-vad-voice-agents-turn-detection-2026) makes this concrete: acoustic VAD is the right tool for opening the microphone and rejecting noise, and semantic VAD is the right tool for deciding when a turn has actually ended, and both need to work together.


A matrix worth running for a new agent:


Mode When it wins When it fails


VAD-only, aggressive silence Ultra-low latency, short command turns Long-form callers, hesitation, disfluency


VAD-only, conservative silence Callers who pause a lot Quick back-and-forth, feels sluggish


STT endpointing (phrase) Well-formed callers, dictated content Fragmented speech, code-switching


Semantic turn detector General case, hesitant callers Domain-specific pause patterns not in training


Realtime model built-in S2S latency wins Opaque, hard to diagnose when it misbehaves


Run the same scenario suite against each and compare on the same axes. You will find one that dominates for your traffic. You will also find scenarios where none of them are good, and those are the scenarios you have to design your agent's recovery around: fallback prompts, "sorry, please continue", explicit confirmation before committing.


Illustrative Roark call with a false cutoff on a hesitant caller


## Turning production failures into regression tests


The single biggest thing to protect against is a silent regression: your endpointing was fine on Monday, you changed the prompt on Tuesday, and now the agent cuts callers off on Wednesday because the response latency shifted and the timing loop moved with it. This is not hypothetical. Prompt length changes change TTFT. TTS voice changes change TTFB. Any of those can shift the effective turn boundary as perceived by the caller.


Endpointing regressions do not surface in aggregate dashboards for weeks, because false cutoffs and dead air rarely trigger a hard escalation. They show up as slightly lower CSAT and slightly higher hangups, and by then you have shipped a month of degraded calls.


The counter is straightforward: every real call that exhibits a false cutoff, dead air, or interruption failure gets captured, parameterized, and added to the regression suite. Next time you change anything in the timing loop, prompt, model, TTS, VAD threshold, semantic model version, that specific caller's utterance runs against the new agent and must pass. Real callers are the best test authors you have.


## Where Roark fits


This is what[Roark](https://roark.ai/) is built for. Simulations dial your agent over[real PSTN and WebRTC calls](https://docs.roark.ai/) with personas that carry distinct pace, hesitation, accent, and background environments. Every simulated and every production call is scored on audio-native metrics, including pace, pauses, and interruptions, so false cutoffs and dead air are first-class findings and not something you have to grep transcripts to find. Failing production calls become replayable regression tests against updated agent logic. Integrations for[Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs](https://docs.roark.ai/) mean the harness sits in front of whatever turn-detection stack you actually ship.


You still have to decide which mode to run and which operating point to pick. Nobody else can make those calls for your traffic. What you do get is the feedback loop: every mode change, every prompt change, every threshold tweak runs against the same suite of adversarial personas and the same set of captured real callers, and the false cutoff and dead air numbers move before the calls do.


## Ship checklist


Before you launch a voice agent, or before you change anything in its timing loop, the endpointing questions worth being able to answer:


- What is your false cutoff rate at your target dead-air p95? What is the shape of the tradeoff curve?
- What happens to those numbers when the caller hesitates for 800ms mid-sentence? When they trail off with an "um"?
- What happens when the caller has an accent your VAD was not tuned for?
- What happens when you change the prompt or the TTS voice? Do you detect the regression before real callers do?
- Do you have production calls captured as regression tests, so the exact utterance that broke the agent last quarter cannot break it again next quarter?


If the answers involve "we mostly listen to a few calls per week" or "our LLM eval covers it", the answer is no. Endpointing needs its own tests, on real audio, scored on turn-level metrics, run every time anything upstream changes. The good news is the tooling to do this exists now; the bad news is that shipping without it means the caller runs the test suite for you.
