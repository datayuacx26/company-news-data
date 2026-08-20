---
schema_version: "1.0.0"
document_id: "a7a77d70e33f282792a0c37de9840eff7a276dbdf2a111ef5dfdebdf97d5700a"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-ai-proposed-voice-agent-updates"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:04a145cbdcf7065919032e03d6604696c45581584682c2a3bd25598d890bcd3b"
---

# Codex proposes, callers dispose: testing AI-authored voice agent updates

On July 22, OpenAI shipped[Presence](https://openai.com/index/introducing-openai-presence/) , an enterprise product that packages the deployment, evaluation, and update lifecycle for voice and chat agents. The headline numbers are real: Presence[powers OpenAI's own English-language phone support line at 1-888-GPT-0090](https://openai.com/index/introducing-openai-presence/) , where OpenAI[says it resolves 75% of inbound issues without human assistance](https://www.helpnetsecurity.com/2026/07/22/openai-presence-ai-agent-platform/) and where[a Codex-powered improvement loop reduced human handoffs by 15 percentage points in ten days](https://openai.com/index/introducing-openai-presence/) .


The more interesting thing is the pattern Presence just normalized. After launch,[production sessions and escalations reveal gaps, Codex proposes updates, and teams test each proposed change against the version in production before approving a controlled rollout](https://openai.com/index/introducing-openai-presence/) . An LLM writes the fix. A human decides whether to ship it. Simulation and evaluation sit in the middle. That loop is now the enterprise expectation for how a voice agent evolves, and it applies whether you're on Presence, on[Vapi](https://vapi.ai/) , on[Retell](https://retellai.com/) , on[LiveKit](https://livekit.io/) , on[Pipecat](https://pipecat.ai/) , on[Bland](https://bland.ai/) , or on your own stack. This post is about the step in the middle: how you actually prove a proposed update is safe to ship on a voice agent.


The pattern OpenAI Presence just normalized


## The update loop is now the product


Read the Presence announcement carefully and you can see OpenAI making a specific claim about where the hard work in enterprise voice AI lives. The launch page opens with[the observation that the challenge is no longer proving agents can work, it's making them reliable enough to do high-value work in production, and that agent behavior must adapt as products, policies, and user behavior change](https://openai.com/index/introducing-openai-presence/) .


The mechanism they built for that adaptation is worth naming precisely. Presence's own[help center describes it as controlled rollout, monitoring, and rollback processes for new versions, plus simulations and evaluations that test common workflows and edge cases before release](https://help.openai.com/en/articles/20001405) . VentureBeat's writeup[calls out that the process is intended to address one of the hardest operational problems in enterprise AI: an agent that works at launch may become less reliable when policies, products, or user behavior change](https://venturebeat.com/orchestration/openai-unveils-presence-a-new-platform-that-lets-enterprises-launch-and-manage-realtime-voice-agents-and-chatbots) .


Two facts about this loop matter for anyone not on Presence:


1. OpenAI is telling the enterprise market that "AI proposes, humans approve, gated by simulations" is the shipping norm. Buyers will start expecting it.
2. That loop is not exclusive to Presence. You can build the same shape on any stack, but only if you have a real way to run the third step.


Which brings us to the uncomfortable part.


## The "test the proposed change" step is where most teams have nothing


Ask a voice-AI team how they review a prompt or tool change today. The honest answer, on most teams, is a mix of:


- Reading the diff and thinking hard about it.
- Making three or four calls into a staging number and vibing it.
- Skimming ten recent production transcripts to check the pattern the change was supposed to fix.
- Shipping to a small percentage of traffic, watching a dashboard, hoping.


That is not a review process, it's a coin flip with more steps. It works while the agent is small and the changes are small. It breaks the moment you accept an AI-authored change, because you no longer have the context the LLM used when it wrote the fix, and you can't rely on your own intuition about what it "probably" does. The whole point of the Codex loop is that the LLM read signals a human hadn't, and proposed a change a human wouldn't have written the same way. You cannot approve that change by staring at the diff.


The thing you need is boring and specific: a way to run the *new* version of the agent through the exact conversations that made you want the change in the first place, plus a suite of scenarios that guard the behavior you already trust, and get back a scored comparison. On voice, "run the agent" means over a real phone call, because turn-taking, endpointing, barge-in, and TTS artifacts are not visible in a text loopback. And "scored" means on audio-native metrics, not just transcript accuracy.


Before/after: what "test the proposed change" actually looks like.


## What a good pre-ship regression run looks like for a voice agent


Copy the shape of a good software regression suite, then adapt each piece for the fact that this is speech, on phone lines, in real time.


**Scenarios you own.** A named set of caller personas and situations that represent the calls you care about most: your top intents, your regulated flows (payments, disclosures, verifications), your known edge cases. Personas should vary voice, accent, pace, emotional register, and background noise, because those are the axes where voice agents actually fail. A regression that always uses the same clean American voice at the same speaking pace is not a regression, it's a happy-path smoke test.


**Real production calls that already broke.** The best test cases you will ever have are the ones your agent has already failed. Escalation to a human. A refund your agent processed at the wrong threshold. A caller who repeated themselves three times because the agent kept talking over them. These calls are your regression suite whether you treat them as one or not. Presence's own description[talks about production sessions and quality signals revealing where an agent is working and where it needs attention](https://openai.com/index/introducing-openai-presence/) , which is the same idea from the other end of the pipe.


**A real transport.** If your agent runs on PSTN in production, your regression should run on PSTN. If it runs on WebRTC through[LiveKit](https://livekit.io/) or[Daily](https://daily.co/) , the regression should run on WebRTC. There is a whole class of failures (packet loss recovery, barge-in with jitter, cold-start latency after a SIP INVITE) that only show up over the real transport.


**Audio-native scoring.** Score the sound of the call, not just the transcript. Did the agent interrupt? How long was the dead air before its first response? Did the pronunciation of the customer's name or the drug they asked about actually match the reference? Did the agent's emotional register match the caller's? A transcript-only pass/fail will happily green-light a change that made the agent 800 milliseconds slower and 20% more likely to talk over callers.


**A pass rate you can gate on.** The whole point of turning "test the change" into a real step is that it has to produce a boolean at the end, not vibes. A suite that runs and produces a scored report is only useful if a human can look at it and say "ship" or "don't ship" without a debate.


Illustrative simulation run against a proposed prompt change before it ships.


## Where Roark fits


[Roark](https://roark.ai/) is built for exactly this middle step. It dials your voice agent over real phone calls (PSTN and WebRTC), uses personas that vary voice, accent, language, pace, and background noise, and scores the resulting audio against a suite of built-in and custom metrics that model pronunciation, interruptions, pace and pauses, emotion, and vocal stress in addition to the usual transcript-level checks. Simulation suites can be triggered over HTTP, so the same run that a human clicks in the UI can gate a CI pipeline or block a deployment on a proposed change.


The piece that pairs most naturally with the Presence loop is production call replay. When a live call fails, or gets escalated, or trips a metric threshold, Roark files an issue and you can replay that exact conversation against the updated agent, turning a real failure into a repeatable regression test. That is what closes the loop: the signals that made an LLM (or a human) want to change something become the tests that decide whether the change ships.


Roark integrates with[Vapi](https://docs.roark.ai/) ,[Retell](https://docs.roark.ai/) ,[LiveKit](https://docs.roark.ai/) ,[Pipecat](https://docs.roark.ai/) ,[Bland](https://docs.roark.ai/) , and[ElevenLabs](https://docs.roark.ai/) directly, and there's a Node and Python SDK if you're on a stack that isn't on that list. For regulated workflows, a HIPAA BAA and SOC 2 Type II certification are available.


## What to change on Monday morning


You don't have to be running Presence for this to apply. If your team is shipping voice agent updates, three concrete changes will make you ready for the loop OpenAI just made canonical:


1. **Build a scenario library that's actually yours.** Ten to thirty scenarios that reflect real calls, with real personas, tied to specific intents and edge cases. Not a generic "test happy path" set from a vendor demo.
2. **Save every production call your agent hands off, refuses, or takes an unusual amount of time to close.** Those are your regression cases. If you can't replay them against a new version of the agent, you are not testing changes, you are guessing about them.
3. **Turn "ship the update" into a scored decision.** Every proposed change, whether an engineer typed it, a PM asked for it, or an LLM proposed it, runs against the same suite. Ship if the pass rate holds and the audio-native metrics don't regress. Don't ship if they do.


Two years ago the debate in voice AI was whether the agents could work at all. That's over.[Grok Voice Agent Builder shipped a no-code path to a production phone agent in about two minutes](https://www.digitalapplied.com/blog/grok-voice-agent-builder-no-code-voice-agents-2026) . Presence shipped an enterprise deployment layer with a self-updating loop. The interesting problem left is the one in the middle: proving that the agent you're about to ship, or the change that Codex just proposed, is actually better than the one already on the phone.


That problem is a testing problem. It has been the whole time.
