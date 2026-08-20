---
schema_version: "1.0.0"
document_id: "ed3f1fd9db1362fd4800a708ff9e110edd6673d0002573dca233403c8cf3c2f9"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/ai-disclosure-testing-voice-agents"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:e4cf3520445190e3f955e47952bd9824af936e7d73b2f91a8340e3aef3e69297"
---

# AI disclosure is a testable requirement. Here's how to prove your voice agent gets it right.

Last week,[Tennessee SB 1580 took effect](https://stackcyber.com/posts/ai-state-laws) , prohibiting marketing AI as a qualified mental health professional and adding a private right of action. A few days later, AI receptionist startup Pie[raised $19.5 million](https://www.voiceaispace.com/news/voice-ai-news-2026-07-06) , with reporting explicitly framing the round against "shifting state laws regarding automated bot disclosures." These are the latest entries in a stack that already includes[Texas SB 140, California SB 1001 and AB 489, TRAIGA (HB 149), and variants across Colorado, Illinois, and Utah](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) . Layer the FCC's[2024 declaratory ruling that AI-generated voice counts as "artificial voice" under the TCPA](https://www.henson-legal.com/ai-voice-compliance) on top, and the picture is clear: an AI voice agent that fails to disclose itself is a per-call statutory violation, exposed to damages of[$500 to $1,500 per call, with no cap](https://www.henson-legal.com/ai-voice-compliance) .


The interesting question for a product or QA lead isn't whether disclosure matters. It's whether you can actually prove your agent delivers the disclosure on every call, in the right window, in language a regulator would accept. Most teams cannot. This post is about how to close that gap.


## The disclosure requirement, in one paragraph


The details vary by jurisdiction, but the operational shape is remarkably consistent. Texas SB 140 requires AI voice technology[to be disclosed within the first 30 seconds of a call](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) . California's SB 1001 has[covered bot disclosure in commercial and political contexts since 2019](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) , and AB 489[prohibits AI from falsely claiming healthcare credentials and requires disclosure when AI communicates with patients](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) . Utah's AI Policy Act,[amended in 2025, narrowed proactive disclosure to "high-risk AI interactions" (health, finance, biometric, significant advice)](https://softcery.com/lab/us-voice-ai-regulations-founders-guide) , with a heavier cadence for mental health chatbots. State AGs and plaintiff firms are[treating explicit AI disclosure as expected practice everywhere](https://www.caller.digital/blog/tcpa-compliant-ai-calling-us-enterprises-2026) , even where not yet legally required.


Simplify to what your agent has to do:


- Say the disclosure. Direct, plain language ("This is an automated assistant" or a specific script your legal team approved).
- Say it early. First 30 seconds is a common statutory floor; first 10 seconds is where practitioners are landing as a safe operational default.
- Say it in the recording. Both the audio and the transcript must contain the disclosure, retrievable on demand.
- Say it every time. Every persona, every language, every scenario. Statutory damages are per-call.


That last one is where QA falls apart.


## Why traditional QA misses disclosure failures


Contact-center QA has historically meant a person listening to a sample of calls after the fact. Retell has publicly noted that[traditional QA teams can review maybe 1 to 2% of calls](https://www.assemblyai.com/blog/voice-ai-in-2026-series-1) . Fine for spotting tone drift or coaching opportunities. Not remotely enough when the failure mode you care about, disclosure omission, generates statutory damages every time it happens.


Three specific ways spot-check QA leaks:


1. **Coverage.** If 3% of your calls miss the disclosure and you sample 1%, you find the miss on 0.03% of your traffic and hear about the rest from a plaintiff's lawyer.
2. **Prompt regressions.** Someone edits a system prompt to fix an escalation bug. A downstream branch that used to emit the disclosure now skips it under one specific caller intent. Your QA sample is a random slice; that branch may not show up for weeks.
3. **Audio vs. transcript.** Transcripts are lossy. An agent that says "I'm an assistant" fast enough to be unintelligible, or that overlaps with a caller's opening, passes a transcript regex and fails the actual test: could a caller hear and understand the disclosure?


The correct posture is not "sample and hope." It's "verify on every call, before launch and after."


Two ways to run disclosure QA


## Turning disclosure into a testable spec


Before you can test it, disclosure has to become a written specification your team owns. This is the part product leads keep skipping.


A workable spec has four parts:


- **Trigger.** When must the disclosure fire? (Every outbound call. Every inbound call that reaches the AI. Every re-engagement after transfer back from a human.)
- **Content.** What exact language, or which of several approved variants, is acceptable? Route ambiguous cases through legal review, not to whoever wrote the prompt.
- **Timing.** Within how many seconds of the call opening, and before which other events (data collection, marketing pitch, appointment booking) must the disclosure land?
- **Evidence.** What does the artifact look like that proves you delivered it? Audio segment, transcript span, timestamp, agent version.


Once you have that spec, you have a metric. And once you have a metric, you can test.


## Pre-launch: simulate every persona and scenario


The first place disclosure fails is under caller conditions your prompt author didn't anticipate. A caller who barges in with "Wait, are you a real person?" during the opening. A Spanish-speaking caller when the agent switches languages. A cold-transfer scenario where the second agent inherits mid-conversation. Each is a distinct code path, and each has its own way of dropping the disclosure.


Coverage here is a testing problem, not a policy problem. Build a persona and scenario matrix and simulate against it before you push a change to production. At minimum:


Axis Coverage to include


Persona Compliant caller, interrupter, non-native speaker, hostile caller, elderly caller


Language Every language your agent supports


Entry path Inbound cold, inbound warm-transfer, outbound cold, outbound re-engagement


Interruption timing No interruption, interrupt at 2s, 5s, 10s


Ambient noise Quiet, moderate, high (drive-through, cafe, on-street)


For each cell, the disclosure spec runs as a single pass/fail metric alongside your normal task-completion checks. A missed disclosure in even one cell is a blocker, the same way an authentication bug would be. This is exactly the class of test that[Roark's simulation runs](https://roark.ai/) are built for: personas with distinct voices, accents, pace, emotional register, and background-noise environments, dialed over real telephony against your agent, scored with custom metrics that check for the disclosure content and timing you specified.


Illustrative pre-launch disclosure suite result


The point of running this before launch is not "we tested the happy path." It is having a written, dated record that your v3.2 prompt delivers the disclosure across every combination you care about, produced automatically on every release. That record is a defense artifact if a claim ever lands. Absent it, you have "we thought it was fine."


## Live: score every production call, not a sample


Pre-launch coverage catches the paths you thought of. Live monitoring catches the ones you didn't.


The unit of QA in a live voice agent is the single call, and the constraint is that a human can't listen to all of them. The workaround is audio-native scoring: models that score the sound of the call, not just a transcript, applied to 100% of traffic. For a disclosure check, that means:


- Was the required phrase spoken, in the required window, in the required language?
- Was it audible (not clipped, not overlapped by caller speech, not so fast as to be effectively omitted)?
- Did the caller acknowledge or respond in a way that indicates they heard it?


Every production call scored, and every failure filed as an issue with the audio segment attached. That is the exact shape of what[Roark scores automatically](https://docs.roark.ai/) on live calls, using audio-native models that go beyond transcripts to catch pronunciation, emotion, pace, and the interruption dynamics that transcript regex misses.


Disclosure miss surfaced automatically on a live call


The operational win here isn't "we caught 100 disclosure misses last month." It's that on the day someone ships a prompt edit that breaks the disclosure branch, you know that day. Not the day the settlement letter arrives.


## Regression: turn every failure into a replayable test


The pattern that separates teams who ship reliably from teams who don't is: every real-world failure becomes a regression test.


When a live call fails a disclosure check, the audio, the caller side, and the exact conditions that produced the miss should get captured and become an entry in your pre-launch suite. Next release, that scenario runs automatically. If the fix regresses, the suite blocks the deploy.


This is where a lot of homegrown testing setups collapse. Capturing a real caller's audio and replaying it against a modified agent isn't trivial infrastructure. It requires production call capture, replay against updated agent logic over real telephony, and scoring back against the same metric that failed originally. This is exactly what Roark's[production call replay](https://roark.ai/) does, turning real failures into repeatable regression tests.


## What "good" looks like


If you want a checklist to walk your team through, this is the shape of a disclosure QA program that will hold up under scrutiny:


- A written disclosure spec, versioned, with legal sign-off. Trigger, content, timing, evidence.
- A pre-launch simulation suite covering every persona, language, entry path, and interruption timing, run automatically on every release, with dated pass/fail records.
- Live scoring of every production call for disclosure delivery, using audio-native metrics that check both content and audibility.
- Automatic issue-filing when a call fails the disclosure metric, with the audio segment and call metadata attached.
- A replay path so every real disclosure miss becomes a regression test in the pre-launch suite.
- OTEL traces or an equivalent audit trail linking each call to the exact agent version that handled it, so you can answer "which version was live when this call happened" in seconds.


If you build voice agents in[healthcare](https://roark.ai/industries/healthcare) , financial services, or any other regulated vertical, this stops being optional. Utah's mental-health chatbot rules require[disclosure before access, every 7 days, and on demand](https://softcery.com/lab/us-voice-ai-regulations-founders-guide) . California AB 489[attaches disclosure to any healthcare interaction](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) . Tennessee's new SB 1580 comes with a private right of action, which means plaintiffs, not just regulators, can bring claims.


## The wider point


Voice AI is entering the phase where the legal, operational, and testing stacks converge. Every new state statute pushes builders to make behavior that was implicit ("the agent should probably say it's a bot") into behavior that is explicit, verifiable, and enforced. That's a testing problem before it's a legal problem, and the teams that treat it that way will spend a lot less time in settlement conversations than the teams that treat compliance as a policy document.


The good news: none of the ingredients are exotic. A disclosure spec, a persona matrix, a simulation harness, audio-native scoring on live traffic, and a replay path for regressions. Wire those together, and disclosure stops being a per-call statutory risk and starts being just another passing check on your dashboard.
