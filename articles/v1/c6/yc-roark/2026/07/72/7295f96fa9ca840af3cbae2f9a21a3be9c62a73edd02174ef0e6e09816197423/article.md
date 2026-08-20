---
schema_version: "1.0.0"
document_id: "7295f96fa9ca840af3cbae2f9a21a3be9c62a73edd02174ef0e6e09816197423"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/test-voice-agent-barge-in-interruption-handling"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:81f9dcee21b7d3e7f3fd9d7d870e81432f7486d4310308988c981f74d4006434"
---

# How to test voice agent barge-in and interruption handling

Barge-in, a caller interrupting the agent mid-sentence, is the single feature that decides whether your voice agent feels human or infuriating. Get it right and conversations flow. Get it wrong and the agent talks over people, ignores "stop", or cuts itself off at every "um". Yet almost nobody tests it, because it is invisible in a transcript and only shows up in the audio.


This is a walkthrough of how to test interruption handling deliberately, and how[Roark](https://roark.ai/) turns those tests into a suite you run before every deploy.


## Why barge-in breaks in production


Interruption handling sits at the seam of three subsystems that each have their own timing, and the bugs live in how they disagree:


- **Endpointing** , deciding the caller has stopped speaking. Tune it too eager and the agent barges in on a mid-sentence pause; too patient and it feels laggy. Providers like[Deepgram](https://developers.deepgram.com/docs/endpointing) expose this directly.
- **VAD and turn detection** , whether inbound speech during agent playback counts as an interruption or as background noise.[LiveKit's turn detection](https://docs.livekit.io/agents/build/turns/) is a good reference model.
- **TTS cancellation** , how fast the agent actually stops talking once it decides it was interrupted. Buffered audio that keeps playing for 400ms after "stop" is what makes an agent feel like it is not listening.


How an interruption travels through the stack


None of this is visible in a text transcript. Two agents with identical transcripts can feel completely different depending on *when* each word was spoken and *how fast* the agent yielded the floor.


## What a text log will never show you


A response that starts 3.8 seconds late reads as a perfectly good transcript. In the audio it is a caller sitting in silence, wondering if the line dropped.


A scored production call in Roark


Roark scores what happened in the audio, so dead air, talk-over, and slow yields become numbers instead of vibes.


## Testing it by hand does not scale


You can catch the worst cases by dialing the agent and rudely interrupting it a few times. That finds the obvious "it never stops" bug. It does not find the regression three weeks later when someone bumps the endpointing threshold by 100ms to fix a different complaint and quietly reintroduces talk-over on every third call.


Ad hoc testing vs. a repeatable suite


Manual testing also cannot answer the question that actually matters: did this change make interruption handling better or worse, on average, across a representative set of callers? That is a measurement problem, and measurement needs to be automated.


## Turning barge-in into a repeatable suite with Roark


Roark simulates real callers against your agent, including the interruption patterns above, and scores what happens in the audio. A barge-in suite runs the hard-interrupt, backchannel, mid-pause and noise cases on every candidate build:


A pre-launch barge-in suite


Because it is a suite, you run it in CI on every prompt change, model swap or TTS update. A regression in interruption latency fails the build the same way a broken unit test would, before it reaches a caller, not after a week of bad calls.


## Where to start


Pick your worst offender first. Most teams find their false barge-in rate is the embarrassing one: the agent stops talking every time the caller breathes. Write that case, get a baseline score, tune endpointing, and watch the number move. Then add the hard-interrupt case as your guardrail so you never trade one failure for the other.


The point is not to hit a perfect score. It is to make interruption handling a number you can see and defend, instead of a vibe you hope survived the last deploy. That is the whole idea behind Roark: if you can measure it, you can improve it, and you can prove you did.
