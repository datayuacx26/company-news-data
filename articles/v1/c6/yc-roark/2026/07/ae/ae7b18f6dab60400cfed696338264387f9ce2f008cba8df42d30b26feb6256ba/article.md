---
schema_version: "1.0.0"
document_id: "ae7b18f6dab60400cfed696338264387f9ce2f008cba8df42d30b26feb6256ba"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-full-duplex-voice-agents-gpt-live"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-22T12:05:46.538010+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:3f927983c8cbae45ccb63ecfea3ee1efbf6fb1feae2e486bd6cda568efdf4f7f"
---

# Testing full-duplex voice agents after GPT-Live

On July 8, 2026, OpenAI shipped[GPT-Live](https://openai.com/index/introducing-gpt-live/) , a full-duplex voice model that listens and speaks at the same time, drops in short backchannels like "mhmm" or "yeah," and delegates hard questions to GPT-5.5 while the conversation keeps going. It is now the default voice experience across ChatGPT, and OpenAI has[opened a waitlist for API access](https://www.techtimes.com/articles/320041/20260710/chatgpt-voice-goes-full-duplex-gpt-live-ends-turn-based-ai-conversations.htm) . The framing in the launch post is intentional: cascaded STT to LLM to TTS pipelines are the old world; a single continuous audio stream making sub-second speak/listen/pause/interrupt decisions is the new one.


If you build production voice agents, the interesting question is not whether GPT-Live is impressive. It is whether your test suite still means anything. Almost every barge-in test, endpointing threshold, and "did the agent respond correctly" assertion in the wild was written for half-duplex agents that wait for silence. Full-duplex breaks those assumptions quietly, and the resulting failures do not look like bugs. They look like agents that talk over people, stall on backchannels, or get tripped by a cough.


## Full-duplex is a different system, not a faster half-duplex


Half-duplex is a state machine with two states and a transition rule: user speaks, silence detected, agent responds. It fails in one direction, dead air, and you tune your endpointing threshold to trade off cutoff risk against latency. Testing is straightforward: feed audio, measure the pause, check the response, done.


Full-duplex tears that model up. As one recent architectural summary put it, the model <cite index="26-16">processes incoming audio while generating output, and it decides many times per second whether to speak, keep listening, pause, interrupt, or call a tool</cite>. There is no single moment when the turn "ends." The agent is continuously choosing, and every one of those choices is a place it can be wrong.


The Evalgent team, which follows this space closely, describes the target end-state cleanly: <cite index="11-1,11-2">full-duplex systems aim to transition between listening and speaking within roughly 100 to 300 milliseconds, matching human conversation, and hitting that requires tight control of VAD, endpointing, and the speech-to-speech path</cite>. Miss it and the illusion collapses. And unlike half-duplex, where latency is uncomfortable, in full-duplex the timing itself carries meaning: as Agora's measurement writeup notes, <cite index="29-4,29-5">in a text chatbot jitter is an inconvenience, but in a full-duplex system timing is meaning — a pause is input, so network delay can masquerade as user hesitation, and barge-in is a round trip</cite>.


The continuous decision loop that replaces turn-taking


## The failures your old suite will not catch


If you look at what actually breaks under full-duplex, three failure modes dominate, and none of them are what turn-based tests were built to find.


**Talk-over.** The agent keeps generating audio while the user is trying to interject. Evalgent's writeup is blunt about the cause: <cite index="11-5,11-6,11-7">talking over the user is the signature failure of full-duplex, and it happens when the agent misreads the floor — it treats a backchannel as silence and keeps going, or it fails to detect the interruption in time</cite>. Turn-based test suites do not exercise this at all, because the user was never allowed to speak during agent audio in the first place.


**Backchannel misclassification.** People say "uh huh," "right," and "okay" during someone else's speech to signal attention, not to take the floor. A model that treats those as new turns will change topic mid-answer. Agora observed the opposite behavior in GPT-Live's favor: <cite index="29-5">GPT-Live handled our backchannels without losing its thread, where older modes treated a murmured "mhmm" as a new turn and changed topic</cite>. Your suite needs to explicitly produce backchannels and assert the agent did not yield the floor.


**False barge-in.** The mirror of the previous failure: the agent stops itself for something that was not an interruption. The FireRedChat authors formalize the two barge-in metrics you actually want: <cite index="16-3,16-4,16-5">a barge-in success rate reported as the minimum latency required to reach 90% accuracy (T90), which quantifies how quickly the system responds to interruptions, and a false barge-in rate that measures erroneous interruptions triggered when the primary speaker is silent — any interruption induced by background noise or non-primary speakers is counted as an error</cite>. Both matter. Optimize only one and the agent either misses interruptions or flinches at every keystroke and door slam.


There is a quieter fourth failure worth naming: the agent that backchannels too aggressively. In GPT-Live's first 24 hours, early users flagged <cite index="23-6">"over-enthusiastic" filler words, with "mhmm" described by some users as arriving too frequently and feeling distracting rather than natural</cite>. That is exactly the kind of failure mode a full-duplex architecture opens up: <cite index="20-2">the model has to decide when not to interject, and the calibration of that decision is a new tuning knob</cite>. It is also a failure mode invisible to transcript-only evaluation, because the transcript looks fine.


## What breaks in your evaluation pipeline


Take a standard "the agent answered correctly" harness. It grabs the transcript, checks the final response text against an expected answer, and grades pass or fail. Now imagine the same call in a full-duplex world.


- The transcript has interleaved user and agent speech. Which segments count as the "answer"?
- The agent murmured "mhmm" three times before the user finished. Are those turns or not?
- Halfway through the agent's reply, the user said "wait, actually," and the agent stopped and pivoted. Did that count as a correct response or an incomplete one?
- The agent's stop latency after the barge-in was 480ms. Passing or failing? What is your threshold, and where in the harness does it live?


None of these questions have transcript-only answers. Which is why the audio itself has become first-class evidence. Overlapping-speech regions have to be measured, not just transcribed. Backchannels have to be tagged and excluded from turn-boundary logic. Barge-in latency has to be pulled from waveform onsets, not log timestamps. The evaluation surface widened, and text-based scoring does not cover it.


Half-duplex QA vs. full-duplex QA


## The metrics that actually matter


If you are rewriting your test plan for full-duplex agents, the list below is a good starting point. Every one of these needs a threshold, an owner, and a fixture that reliably produces the condition.


1. **Barge-in stop latency (T90).** Time from user speech onset to TTS actually cutting. Measure it from audio, not from event logs, because the audio is what the caller hears. Agora's writeup makes the point directly: <cite index="29-1,29-5">barge-in is a round trip, which is why stop latency is a systems number, not a model number</cite>. You cannot infer it from LLM traces alone.
2. **False barge-in rate.** Fraction of agent-side stops that were not real interruptions. Test corpus should include coughs, keyboard noise, background TV, a second voice in the room, and typed-message notifications.
3. **Backchannel yield rate.** Given a "mhmm" or "right" mid-agent-turn, what fraction of the time does the agent incorrectly abandon its turn? This is the metric your users will feel most and your logs will surface least.
4. **Turn-onset latency.** Time from user end-of-utterance to first agent audio, measured across a range of naturally paced conversations. Note that with a delegating architecture like GPT-Live's, "first audio" and "first meaningful audio" may be different: the voice model may say "one sec" while a heavier model works in the background. Both matter, separately.
5. **Talk-over duration.** How many milliseconds of simultaneous speech occurred per turn boundary, and how many of those were user-initiated versus agent-initiated. A well-behaved full-duplex agent overlaps briefly on backchannels and cleanly yields on interruptions.
6. **Delegation coherence.** If your agent uses a delegation pattern (fast voice model in front, heavier model in the background), you need to check that the fast model does not answer the question incorrectly while the slow model is still computing the real answer. GPT-Live productizes the pattern: <cite index="26-7,26-8,26-9">it solves the classic voice-AI dilemma that models fast enough for realtime chat are too shallow for hard questions and models smart enough for hard questions are too slow for conversation — the voice model keeps the conversation alive while a stronger model works in the background</cite>. That split is a testing surface, not just a features slide.


## Testing frameworks were not built for this either


Most of the popular voice agent orchestrators added interruption handling for the half-duplex-with-barge-in world, which is close to full-duplex but not the same thing. AssemblyAI's writeup on the field puts it plainly: <cite index="34-15,34-16">both LiveKit and Pipecat handle interruption logic and back-channel suppression at the orchestrator level, which means the quality of your turn detection and barge-in behavior depends as much on how well your orchestrator handles those events as it does on the underlying speech-to-text model</cite>.


That has direct implications for how you test. If you write unit tests against your STT provider in isolation, or evaluate the LLM on transcript-in-text-out prompts, you have tested the pieces least likely to fail. The failure mode lives in the orchestrator's floor-control logic, and the only way to exercise it is with real audio going in and out over a real transport. For anyone running[LiveKit Agents](https://docs.livekit.io/agents/) or[Pipecat](https://docs.pipecat.ai/) or a Vapi or Retell deployment, the harness has to sit outside the framework and drive it end to end.


Two other launches from the last two weeks reinforce this. xAI's[Voice Agent Builder](https://x.ai/news/grok-voice-agent-builder) , released July 1, ships as a bundled speech-to-speech stack, and Retell released Conductor, described in trade coverage as <cite index="1-13">a graph-native review interface designed to help enterprises test, evaluate, and improve production voice agents</cite>. The direction is the same across every vendor: the agent is a continuous audio loop, not a chain of turns, and the tools you use to test it need to look at it that way.


## What a full-duplex test suite actually looks like


Concretely, here is the shape of the suite we recommend teams build. This is roughly what our own customers stand up on top of Roark.


- **A barge-in battery.** A dozen or more scenarios where the caller interrupts at different points in the agent's response (start, mid, near end) at different volumes, over different background conditions. Each run scored on stop latency, false-barge rate, and whether the agent's recovery reply made sense given what it had already said.
- **A backchannel battery.** The caller drops "uh huh," "right," "okay," and "yeah" during the agent's speech, at spacings that model real listening. Assertion: the agent does not yield the floor and does not change topic. Nice-to-have assertion: the agent does not itself over-produce backchannels while the caller is talking.
- **A noise battery.** The same happy-path scripts run against a matrix of background conditions: quiet office, coffee shop, car with road noise, hands-free with echo, keyboard tapping, a TV in the room, a second voice nearby. Every one of these is a false-barge-in hazard.
- **A silence battery.** The caller pauses mid-sentence, or takes five seconds to answer a question. The agent should stay quiet, or backchannel appropriately, not talk over the thinking pause. GPT-Live's design explicitly targets this: <cite index="9-5">during conversations, GPT-Live can show it's paying attention with phrases like "mhmm" or "yeah", engage in quick back-and-forth, or just stay quiet when you need a moment to think</cite>.
- **A delegation-lag battery.** If your architecture hands off to a slower model in the background, tests where the caller asks something research-heavy and then either waits or interrupts. Assertion: the fast voice model does not invent an answer, and it recovers gracefully when the heavier model returns.
- **A regression battery pulled from production.** Any real call that failed one of the above metrics gets frozen into a replayable test. The next agent version does not ship until that call passes.


Illustrative full-duplex regression suite


## Where Roark fits


We built[Roark](https://roark.ai/) because we needed this suite ourselves, and none of the existing test tooling looked at the audio the way full-duplex agents demand. Two capabilities matter most for teams making the jump.


The first is simulation. Roark dials your agent over real telephony and WebRTC, with personas that carry distinct voices, pace, emotional register, and background-noise environments, and runs the batteries above on a schedule or over HTTP as a CI gate. Because the calls are real audio over a real transport, they exercise the orchestrator's floor-control logic exactly the way production traffic will. The[docs cover the setup](https://docs.roark.ai/) and there are one-click integrations for Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs, so this is not a rewire of your stack.


The second is audio-native scoring. Every call, simulated or live, gets scored on 64+ built-in metrics plus any custom ones you write, and the models score the sound of the call, not just the transcript. Pauses, interruptions, vocal stress, and pace are all in-band, which is exactly what a full-duplex evaluation needs. When a metric fails, Roark files an issue against the call automatically. When a production call exposes a new failure mode,[production call replay](https://docs.roark.ai/) captures it and replays it against your updated agent logic, so real failures become permanent regressions.


## What to do this quarter


If your team runs voice agents and the GPT-Live announcement has your product manager asking when you get similar behavior, the honest answer is: probably soon, and it will surface bugs you did not know you had. A few things worth doing before that happens.


1. Write down every place your current tests assume turn boundaries. Endpointing thresholds, silence timers, "was the response correct" oracles that read a single transcript field. Each of these needs a full-duplex analogue.
2. Instrument barge-in stop latency from audio, not from event logs. If you do not have this today, you will not know when it regresses.
3. Build the backchannel and noise batteries first. Talk-over and false-barge-in are the two most user-visible failure modes and the two most likely to break on a model upgrade.
4. Move any test that used to be "did the agent say X" toward "was the audio conversation coherent," and score audio directly. Transcripts will not tell you which of the two people were talking at the same time.


Full-duplex is the direction the entire industry is pointing. It makes voice agents feel like people, and it makes them fail like people too. The teams that ship well in this new regime are the ones whose test suites already assume the conversation is continuous.
