---
schema_version: "1.0.0"
document_id: "fae319deade7f7576ddea7bdf9acfdf5dd7d8090d920e24e560a9c0fd8ced638"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-full-duplex-voice-agents"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:a58d4ba6f3796c75bebefbc139eb730d18fae841e9122e8c59350c7652e35d0f"
---

# Testing full-duplex voice agents: what changes when your model listens and speaks at once

On July 8, OpenAI shipped[GPT-Live](https://openai.com/index/introducing-gpt-live/) , a new generation of voice models built on a full-duplex architecture that can listen and speak at the same time. During a conversation the model can backchannel with "mhmm" or "yeah", stay quiet while you think, and delegate hard reasoning to GPT-5.5 in the background while the conversation keeps flowing. The API access is[described as coming soon](https://techcrunch.com/2026/07/08/openai-releases-new-voice-models-for-more-natural-live-conversations/) , but the direction is clear enough: full-duplex is quickly becoming the default assumption for what a voice agent feels like.


For anyone running production voice agents, this is also the moment your eval stack goes half-blind. A test harness built around transcripts, text loopbacks, and strict half-duplex turn boundaries cannot see the failure modes a full-duplex agent actually produces. That gap is what this post is about: what full-duplex changes at the architecture level, why transcript-based evaluation stops working, and what it takes to test these agents before your callers do.


## What full-duplex actually changes


The previous generation of voice systems, including OpenAI's own Advanced Voice Mode, still operated through discrete turns. <cite index="29-3,29-4,29-5">The model had to wait for the user to stop speaking before responding, and because turn detection is based on silence, even a brief pause or background noise could be mistaken for the end of turn, causing the model to interrupt at unnatural times.</cite>


GPT-Live throws that pipeline out. <cite index="29-8,29-9">Instead of processing a sequence of separate messages, GPT-Live continuously processes input while generating output, and the model can make interaction decisions many times per second: whether to speak, continue listening, pause, interrupt, or invoke a tool.</cite> There is no single "end of user turn" event anymore. There is a continuous loop of micro-decisions.


Three things fall out of that architecture that matter for testing:


1. **Backchannels become first-class outputs.** The agent emits short cues like "mhmm" and "got it" *while the user is still talking* . Those tokens never make it into a clean speaker-separated transcript the way a normal reply does.
2. **Silence is signal, not turn-end.** <cite index="23-5,23-6">The model can produce backchannel responses like "mhmm" or "yeah" while you're still speaking, handle pauses without interpreting them as turn-endings, and users can explicitly tell it to stay silent by saying something like "just hold on".</cite>
3. **Reasoning is decoupled from voicing.** <cite index="22-18,22-19,22-20,22-21">OpenAI decoupled continuous interaction from heavier reasoning, so when a task needs search, reasoning, or more agentic capabilities, GPT-Live delegates it to another model such as GPT-5.5 in the background, while GPT-Live keeps the conversation flowing.</cite> That "keeps the conversation flowing" part is where new failure modes live.


How a full-duplex agent decides, many times per second


This is not just an OpenAI change. LiveKit shipped[adaptive interruption handling](https://docs.livekit.io/agents/logic/turns/adaptive-interruption-handling/) whose <cite index="36-9">adaptive interruption model is trained on real conversational audio to distinguish true interruption attempts from non-interruptive speech</cite>. Research benchmarks like[Full-Duplex-Bench-v2](https://arxiv.org/pdf/2510.07838) and[τ-Voice](https://arxiv.org/abs/2603.13686) exist because <cite index="17-6">when full-duplex systems are tested, they often get confused when people talk at the same time, struggle to handle corrections smoothly, and sometimes lose track of who or what is being talked about</cite>. The problem is industry-wide, and it is empirical.


## The failure modes text can't see


Here is a partial catalogue of things that go wrong in a full-duplex agent and are essentially invisible in a transcript diff:


- **Talk-over.** The agent starts speaking during a real user statement, not a pause. In a written transcript both turns look fine; on the audio you hear one voice on top of another.
- **Mis-timed backchannels.** Simon Willison[described](https://kie.ai/blog/gpt-live-full-duplex-voice-model-deep-dive) an early GPT-Live bug where <cite index="24-2">the model was "interrupting me and laughing at my (not really intended as) jokes while I was still talking"</cite>. That laugh is a two-hundred-millisecond audio artifact in the middle of the user's turn. It is not a "wrong answer" any semantic eval can catch.
- **False barge-ins.** A TV in the next room, a passing siren, or a stray "uh-huh" from the caller trips the agent into cancelling its own reply and restarting from silence. The transcript shows a truncated agent turn followed by a new one. It looks fine on paper; on the phone it feels broken.
- **Delegation stalls.** The foreground model routes a hard question to the background reasoner and, if not tuned right, goes uncannily silent instead of filling the wait with a natural "let me check that for you". The transcript records the eventual correct answer. It does not record the four seconds of dead air that made the caller hang up.
- **Endpoint hallucinations.** A caller pauses to breathe. The agent decides they are done, cuts them off, answers the half-question, and now the whole conversation is off the rails.


Every one of these is a **sound** problem, not a text problem. And every one of these is exactly the class of failure full-duplex architectures create more of, not less. <cite index="25-1">Full-duplex invites its own new failure mode, a model that talks over you when it thinks it heard a backchannel.</cite>


## Why the old eval stack breaks


Most voice-agent test harnesses today do one of three things:


1. Diff a transcript against a golden transcript.
2. Run a text loopback: feed prompts to the LLM part of the stack, score the text out.
3. Run a turn-based simulator that alternates cleanly: user says X, agent says Y, user says Z.


Each of these was reasonable when the underlying architecture was a cascade of STT into an LLM into TTS with strict turn boundaries. None of them survives contact with full-duplex.


A transcript diff cannot represent overlapping audio. The best it can do is note that agent tokens and user tokens have interleaved timestamps, and that is not the same as knowing whether the caller heard the agent talking over them. A text loopback skips the entire audio path, which means it skips VAD, endpointing, echo cancellation, TTS cancel-on-interrupt, and every timing-sensitive decision the agent actually makes on a phone call. A turn-based simulator, by construction, cannot produce the overlap conditions that make full-duplex agents fail.


The point[Evalgent made recently is blunt](https://www.evalgent.com/blog/full-duplex-voice-agents) and correct: <cite index="12-9,12-10,12-11">full-duplex moves the hardest behaviour into turn-taking, and turn-taking is invisible to text-based testing; you cannot catch a talk-over by reading a transcript, you have to test over real audio, with overlapping speech and interruptions built in.</cite>


Old eval stack vs what full-duplex requires


## What audio-native testing actually requires


If you take the failure catalogue seriously, the requirements for a real eval harness fall out fairly cleanly:


**Real audio path, not a text shortcut.** Your test callers need to speak to your agent over the same transport your users do. For phone-based agents that means PSTN. For embedded or browser agents that means WebRTC. Anything less bypasses the parts of the stack that matter.


**Synthetic callers with configurable overlap behavior.** You need to be able to say "this caller barges in aggressively", "this one pauses mid-sentence to think", "this one backchannels with 'right, right'". If you cannot programmatically produce a caller who talks over your agent, you cannot regression-test talk-over recovery.


**Audio-native metrics.** You need scores that come from the sound of the call, not from the transcript. That means metrics for interruption timing, talk-over rate, backchannel appropriateness, latency to first token after end-of-turn, silence gap distribution, pronunciation of proper nouns. LiveKit's own[documentation](https://docs.livekit.io/agents/logic/turns/adaptive-interruption-handling/) makes the point: <cite index="36-1">an agent session collects interruption metrics for each barge-in detection, including per-event latency, and the number of requests and interruptions.</cite> Those numbers only exist if you are looking at the audio.


**Persona diversity.** Different accents, different speech pace, different background noise, different languages. The failure surface of a full-duplex agent is not uniform across those axes; a Silero VAD tuned for clean American English will endpoint differently on a caller with a heavy accent in a moving car.


**Regression suites, not one-off runs.** Full-duplex behavior is unstable in a specific way: the same code shipped against a new hosted model version will fail differently. You need to be able to replay a specific past interaction against a new build and see what changed.


This is exactly what we built[Roark](https://roark.ai/) for. Roark's simulation testing dials your voice agent over real phone calls or WebRTC, drives the call from a persona with a distinct voice, accent, pace, emotional register and background noise, and scores what came back with audio-native metrics for interruptions, pace and pauses, vocal stress, and 60+ others. Simulations are built from personas, scenarios, run plans, and schedules, so recurring runs can gate CI over HTTP. There are one-click integrations for[Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs](https://docs.roark.ai/) .


## Simulating a full-duplex agent before launch


The concrete pre-launch loop looks something like this. Define a small set of personas that stress the parts of the pipeline you think are most fragile:


```text
personas:
- id: aggressive_barge_in
voice: en-US-female-fast
behavior:
barge_in_probability: 0.6
mid_utterance_pauses: rare
background_noise: none


- id: thoughtful_pauser
voice: en-GB-male-slow
behavior:
barge_in_probability: 0.0
mid_utterance_pauses: frequent  # 800ms - 2s
backchannels: ["mhmm", "right"]


- id: noisy_environment
voice: es-MX-female-medium
behavior:
barge_in_probability: 0.2
background_noise: coffee_shop
accent_strength: 0.8
```


Then build scenarios that exercise a specific decision path. An outbound sales agent might have "caller asks the same question three times", "caller interrupts the pitch to ask price", "caller says 'hold on' mid-answer". Each scenario becomes a scored run.


The output is a suite result, not a single number:


Illustrative pre-launch simulation suite result


Two things are worth pointing out about this loop. First, most of the value shows up when you *look at the failed calls* , not the aggregate score. A pass rate on a full-duplex suite is a lagging indicator; the actual signal is in the specific rows where the agent talked over the caller for 3.8 seconds or endpointed on a natural in-breath. Second, the persona list is where taste matters. A hundred easy calls tell you nothing. Ten hard, diverse calls tell you everything.


## Regression when the model changes underneath you


Full-duplex agents built on hosted foundation models have a specific operational problem: the model changes and you find out about it in production. OpenAI has said <cite index="22-22">GPT-Live is designed so it can adopt newer frontier models as they ship.</cite> That is a nice property for capability. It is a hostile property for stability.


If you are wiring your production stack to a hosted realtime model (OpenAI's Realtime API, xAI's[Grok Voice](https://x.ai/news/grok-voice-agent-builder) , Gemini Live, or one of the platform-level products from Vapi or Retell that sit on top of them), the model version you launched against will not be the model version live traffic hits next month. Sometimes the change is announced. Often it is not. Either way, your talk-over rate can shift overnight.


The mitigations that actually work are boring:


- **Version-pin where you can.** Where the underlying model exposes a stable version, pin it and upgrade deliberately.
- **Replay real production calls against every candidate build.** Capture what happened yesterday. Play it back against tomorrow's stack. Diff the audio-native scores. This is where Roark's production call replay earns its keep: real failed calls become the regression suite that guards future launches.
- **Score every live call, not a 1% sample.** With full-duplex, the pathological calls are individually rare and collectively frequent. A random sample will miss the interesting ones. Score everything, file issues on threshold breaches, and let a human triage the filed queue.


Illustrative Roark call review, full-duplex agent


## What to do this week


If you are shipping a voice agent right now, treat GPT-Live as a signal that the ground under your test harness moved:


1. **Audit what your evals actually see.** If your regression suite is text-based, note it. That is not evidence your agent works; it is evidence your evals cannot fail.
2. **Add three overlap scenarios by end of week.** Aggressive barge-in, mid-utterance pauser, noisy background. Run them on your current build. The failures will not be subtle.
3. **Turn every production incident into a regression test.** The next talk-over complaint you get should end up as a replayable scenario you run before every deploy.
4. **Score interruption metrics, not just task success.** Per-event barge-in latency, talk-over duration, endpoint-hallucination rate. Track them over builds.


Full-duplex is a genuine step forward for how voice agents feel to talk to. It is also the point at which "vibes-based QA plus a few golden transcripts" stops being a defensible position. The teams that adapt first will notice, because their launch weeks get quiet. The teams that do not will notice too, because their callers will tell them, one talk-over at a time.
