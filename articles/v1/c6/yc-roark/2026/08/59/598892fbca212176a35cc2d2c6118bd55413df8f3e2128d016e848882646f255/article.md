---
schema_version: "1.0.0"
document_id: "598892fbca212176a35cc2d2c6118bd55413df8f3e2128d016e848882646f255"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-that-remember"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T15:53:26.284965+00:00"
fetched_at: "2026-08-13T15:53:27.733467+00:00"
content_hash: "sha256:3aa2dc554703e8e8d48729f798a2f20a783ac84a767976c2c2b015d1854b9427"
---

# Testing voice agents that remember

Two things happened in the last few weeks that quietly changed what "voice agent QA" has to cover. Assort Health raised a $120M Series C at a $1.2B valuation to scale what it calls[Patient Journey Memory](https://www.prnewswire.com/news-releases/assort-health-raises-120-million-series-c-to-scale-largest-deployment-of-ai-agents-for-the-patient-journey-302808634.html) , a continuous record that carries context across every touchpoint a patient has with a practice. And Mem0 published its 2026 agent memory report, which argued that[voice agents have a memory problem that is qualitatively different](https://mem0.ai/blog/state-of-ai-agent-memory-2026) from text agents, because the caller cannot scroll back, cannot paste in context from a prior session, and cannot manually remind the agent of what happened last time.


Put those together and the picture is clear. The voice agent your team launched twelve months ago was probably one call, one shot, one hangup. The voice agent your team is launching this quarter remembers the caller. It knows they called last Tuesday, that the referral is still open, that the prescription was flagged. Which means the QA process built around single-call transcripts is now missing an entire class of bug: the ones that only appear on call two, three, or seven.


## What changes when the agent remembers


A stateless voice agent has one truth to defend on every call: does the transcript match the policy. A stateful voice agent has three:


1. **The lookup was correct.** The right record was pulled for the right caller at the moment the call started.
2. **The personalization was appropriate.** Using that record in the conversation actually helped the caller, and did not overreach.
3. **The write-back was clean.** What the agent learned or promised on this call is what shows up next call, without silently corrupting the record.


Each of those is a separate failure mode. In practice they compound: a bad write on Monday becomes a bad warm start on Wednesday becomes a wrong-account personalization on Friday. And unlike a stateless bug, none of it shows up in a single-call transcript review. Two of the three touch data that lives outside the call. All three depend on state that only exists because a prior call put it there.


Assort's own description of the pattern is instructive:[language preference, visit history, tone, open tasks and sentiment all carry through automatically](https://www.fiercehealthcare.com/ai-and-machine-learning/assort-health-scores-120m-series-c-scale-voice-ai-agent-platform-healthcare) , across channels and across every step of the patient journey. That is the aspiration for stateful voice, and it is also the surface area you now have to test.


How call two differs from call one


## The failure modes single-call QA misses


Once you accept that the unit of test is a sequence, not a call, a specific set of failure modes shows up:


**The wrong-account warm start.** The agent opens the call already committed to the wrong customer's context. Caller ID matched a stale record, a household member is calling from the primary's number, the CRM associated the number to the wrong contact, or the model was fed a "trusted" caller value that came from the LLM's own transcript rather than from telephony. ElevenLabs' own guidance on this is that a phone number by itself[isn't sufficient to authenticate a caller](https://elevenlabs.io/blog/designing-secure-caller-identity-authentication-flows-for-voice-agents) , and needs a second signal before granting access to account data.


**The stale-fact bug.** The memory layer stores that the caller's insurance is Aetna. Six months later it is Cigna. The agent confidently corrects the caller. Mem0's 2026 report calls out[staleness in high-relevance memories](https://mem0.ai/blog/state-of-ai-agent-memory-2026) as an open research problem. That doesn't help you in production. What helps you in production is a test that fails when the agent presents stale state as fact.


**The overreach.** The record says the caller is grieving, or has an eating disorder note, or churned two products ago. Referencing that is technically "correct" and functionally awful. Overreach is the failure mode that the vocal side of the call surfaces before the transcript does: it lands in tone, pace, and the caller's pause before the next word.


**The cross-caller leak.** Two callers share a number. The primary is verified, the second is not. The agent uses the primary's context anyway. This is not a hypothetical for healthcare, home services, or family accounts.


**The ghost caller.** A returning caller does not want to be recognized this time. They want to start over. Most agents cannot handle that at all, because the personalization is a warm-start prompt rather than a runtime decision.


**The forgotten commitment.** Call one: agent promises a callback in 24 hours. Call two: agent has no idea. That is a memory bug, but its symptom is a broken promise, and the metric that catches it lives at the arc level, not the call level.


None of these appear in an eight-minute transcript spot check. All of them appear in what the caller actually experienced.


## Why sample-based QA cannot cover this


Sample-based QA already fails at scale for stateless voice agents, because a two-percent listen-through catches almost nothing that regressed since last week. Introduce memory and the math gets worse. Every call is now dependent on the state written by prior calls. The bugs are conditional: they only surface when a specific sequence occurs. A random sample almost never hits the sequence.


The correct unit of coverage is the arc: a scripted set of related calls from the same persona, with the state that survives between them treated as a first-class test artifact. That is not something you can build out of transcripts and a spreadsheet.


Old QA vs memory-aware QA


## Building test cases for stateful voice


The mental model that works: think of the tests as multi-call scenarios attached to personas that persist. The persona has a phone number, a history, a set of preferences, and expected state carried over from the previous call in the sequence. Each call in the arc is scored, and the arc itself is scored on whether the state is coherent from one call to the next.


A workable memory-aware test suite covers, at minimum:


- **Identity resolution.** Matched caller ID, unknown number, spoofed number, withheld number, duplicate contact, stale contact, mid-call number handoff. This is the entry-level testing checklist for any agent that personalizes at all.
- **Warm start correctness.** For every "known caller" scenario, an explicit assertion of which fields entered the prompt, which fields reached tools, and which fields stayed server-side. The pattern for stateful agents is[read live, write on the end-of-call webhook](https://techsy.io/en/blog/voice-agent-crm-integration) : if the test cannot see what got read, it cannot verify what was warm-started from.
- **Staleness scenarios.** Deliberately mutate the backend record between calls two and three. Verify the agent picks up the change on call three and does not defend the old value.
- **Overreach probes.** For each sensitive field in the memory layer, a scenario where the agent should have that field available but should not proactively reference it. Score the audio, not just the transcript. Tone and pacing carry the overreach.
- **Cross-caller isolation.** Same number, different speaker. Different voice, different vocabulary, sometimes a language switch. Score whether the agent asked for a second signal before touching account data.
- **Ghost-caller opt-out.** Explicit test that a returning caller can say "start fresh" or "I'm calling about someone else" and be believed.
- **Commitment coherence.** Call one makes a promise. Call two references it. The commitment is a fact under test, not a nice-to-have.
- **Memory poisoning.** A caller asserts a false fact ("my policy number is X") and the agent writes it back. Call two starts with that false fact as if it were verified. Any pathway that writes caller-asserted facts into durable memory without an out-of-band verification step is a bug factory.


The uncomfortable truth about most of this list is that none of it can be tested from a text transcript alone. The audio matters. The state between calls matters. The webhook payload matters. The metric coverage has to match.


## The simulation shape that actually works


The reason simulation testing pulls ahead here is that a memory-aware suite is not really a QA problem, it is a call-orchestration problem. You need to place real calls from personas that persist, in the right order, with backend state mutated between them in controlled ways, and score each call on audio-native metrics that catch overreach and tone in addition to correctness.


That is the shape[Roark](https://roark.ai/) is built around. Simulations run over real telephony, either dialing your agent's inbound number or letting your agent dial a provisioned number, with personas that carry voices, languages, accents, pace, and background environments across a run. Scenarios are the unit of test; run plans and schedules stitch them into recurring, automated arcs. That is the substrate a memory-aware suite needs: not a one-shot dialer, but a scheduler that can place call two only after the end-of-call webhook from call one has closed.


The scoring has to match. Roark's audio-native metrics score the sound of the call, not just the transcript: pronunciation, emotion, vocal stress, pace, pauses, interruptions. Overreach lives in exactly those signals. So does the false confidence of a stale-fact bug. There are 64+ built-in metrics and unlimited custom metrics, so the "commitment coherence" or "warm-start correctness" checks you care about become saved, monitored measurements rather than one-off audits. When a metric fails on a live call, an[issue is filed automatically](https://docs.roark.ai/) with the audio and the transcript attached.


The other capability that pays for itself specifically with memory bugs is production call replay. When a real caller hits the wrong-account warm start on Tuesday, you don't just want to know it happened. You want to capture that call, mutate the state around it, and replay it against every subsequent version of the agent as a regression test. Stateful bugs are the ones most likely to come back after a prompt change or model rev, precisely because their surface area is invisible to prompt-level review.


Illustrative memory-aware simulation suite


## What a memory-aware launch bar looks like


For teams shipping voice agents that carry any state across calls, and increasingly that is most teams shipping voice agents at all, the launch bar has to expand. A pragmatic minimum:


- The identity resolution suite passes at your target rate across matched, unknown, duplicate, and spoofed cases.
- Every field the memory layer exposes to the model has a documented "may reference proactively" flag, and there is a test that fails when the agent violates it.
- Every write path from an agent call into durable state has a corresponding read test on a subsequent call. If nothing reads it, it should not be written.
- Staleness scenarios are part of the regression pack, not an ad hoc exercise before launches.
- The scored metric suite includes at least one audio-native measure of overreach or tonal mismatch, because transcript-only QA does not catch the caller's flinch.
- Production calls that fail any of the above are automatically captured and become replayable tests.


Healthcare is the loudest example of this shift because Assort put a number on it, but the pattern travels. Any voice agent that says "welcome back" is now stateful. Any voice agent that opens with "I see you called last Tuesday about the water heater" is now stateful. The QA process either follows or it doesn't, and the ones that don't will meet the failure modes above at production volume before they meet them in test.


If you're already thinking about this in the context of patient access or clinical follow-up, our[healthcare](https://roark.ai/industries/healthcare) writeup is the shortest path to the healthcare-specific version of the problem. If you're on Vapi, Retell, LiveKit, Pipecat, Bland, or ElevenLabs, the simulation, ingestion, and metric layer all connect through one-click integrations rather than a rebuild. The point of all of it is that the QA has to catch up to the product. Voice agents are no longer stateless. Test them like it.
