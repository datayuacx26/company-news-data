---
schema_version: "1.0.0"
document_id: "d02a504ed4e56f6074d4a409f744ddc17eb509b9e431023d0ba81406be4c92ec"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-for-patient-access"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T12:48:12.522755+00:00"
fetched_at: "2026-08-19T12:48:15.447311+00:00"
content_hash: "sha256:48643bf3adbbf94f0af31278b82f09854ade87ff6984a1dc76e98950c8c71f49"
---

# Testing voice agents for patient access

Patient access is the front door of American healthcare, and it just got a lot louder. In late June,[Assort Health closed a $120 million Series C led by Menlo Ventures at a $1.2 billion valuation](https://www.fiercehealthcare.com/ai-and-machine-learning/assort-health-scores-120m-series-c-scale-voice-ai-agent-platform-healthcare) , the same month[Cedar's Kora voice agent crossed nearly 400,000 patient calls with 80%-plus post-call satisfaction](https://uscomplianceinstitute.com/blogs/news/ai-voice-agent-in-healthcare) . Industry surveys now put[over 60% of U.S. clinics using or evaluating AI for call handling](https://uscomplianceinstitute.com/blogs/news/ai-voice-agent-in-healthcare) . Patient access voice AI has stopped being a pilot category.


That shift changes what QA has to catch. A scheduling bot that misroutes 2% of callers is fine in a design partner deployment and a very expensive HIPAA problem at 400,000 calls a month. This is a playbook for the product and QA leaders standing up patient access voice agents, or trying to keep the ones already in production from earning a headline.


## What "patient access" actually means on a call


"Patient access" is a compact label for a sprawling workflow. When a patient calls a specialty practice, an ambulatory center, or a health system, the voice agent has to move through a chain of decisions that used to belong to a front-desk team.[Assort's platform, for example, spans scheduling, intake forms, referrals, document processing, medication refills, real-time eligibility, lab requests, and payments](https://www.assorthealth.com/blog/assort-health-raises-120-million-series-c-to-scale-largest-deployment-of-ai-agents-for-the-patient-journey) . Cedar's Kora, Hyro, Phreesia, Relatient, and a growing set of specialty players all touch overlapping surfaces.


A single inbound call can hit most of them:


Patient access voice agent call flow


Every step is a place where the agent can fail quietly. An eligibility check that returns "we couldn't verify your coverage" instead of routing to a live human. A provider match that books a new-patient slot when the caller is an existing patient. A refill request that misses a controlled-substance flag. None of these read as failures in a call transcript unless you know the workflow well enough to spot them.


## Why sampled call review misses the failures that matter


Most healthcare teams still QA voice agents the way they QA human call center staff: pull a random sample, score them against a rubric, meet weekly. That works when the failure rate is 5% and every failed call teaches you something new. It falls apart when your agent is placing tens of thousands of calls a week and the failure modes are narrow, silent, and specialty-specific.


The failures we see teams miss with sample-based QA in patient access:


- **Silent identity-verification drift.** The agent stops asking for date of birth on ~3% of calls after a prompt change. Nothing in the transcript flags it. A random sample of 100 calls has a 95% chance of missing the regression entirely.
- **Insurance edge cases that swallow the call.** A caller with dual coverage (Medicare + a supplemental) gets a confused agent that loops through eligibility questions. Duration explodes, the caller drops, the metric that fires is "call length" not "insurance handling failed."
- **Care-gap outreach that skips consent.** Outbound calls to close mammogram or colonoscopy gaps go out under an AI disclosure phrasing that a compliance officer would rewrite in five minutes, if a compliance officer heard them.
- **Escalation that never happens.** The agent handles a caller describing chest pain by scheduling a routine visit three weeks out. Containment metrics look great. This is the failure that ends careers.


The common thread is that none of these are visible from a random 1% call sample. You need every call scored on the specific things patient access has to get right, and you need to catch them before launch, not after.


## The pre-launch simulation suite


Before a patient access agent takes a real call, it should have run against a scenario suite that covers the shape of the caller population, not just the happy path. In a specialty practice, that population is not uniform: it skews older, includes callers with speech impairments, includes a meaningful share of non-English primary speakers, and includes a long tail of angry, confused, or scared callers.


A reasonable pre-launch suite for a specialty practice looks something like this:


Illustrative patient access simulation suite


The scenarios that earn their place are the ones tied to specific failure modes, not vague "difficult caller" categories:


1. **Identity verification under stress.** Caller gives the wrong date of birth twice, then the right one. Agent should not lock them out and should not skip verification.
2. **Existing patient vs. new patient disambiguation.** Caller says "I saw Dr. Chen last year." Agent should route to the returning-patient flow and pull the record, not book a new-patient slot at 2x the length.
3. **Insurance disambiguation.** Caller has Medicare Advantage and a secondary. Agent should not enter an eligibility loop.
4. **Symptom triage escalation.** Caller mentions symptoms that require a nurse line. Agent should escalate immediately, not book a routine appointment.
5. **Refill with controlled substance.** Agent should not attempt to process, should route to a clinician.
6. **Care-gap outbound with disclosure.** Outbound scenario: agent identifies itself as AI, states purpose, offers opt-out cleanly, complies on request.
7. **Language and accent coverage.** The same scenarios run against callers with Spanish, Mandarin, Vietnamese, Tagalog primary speech.
8. **Emotional register.** Scared, angry, grieving callers. The agent should not soldier through a booking script when the appropriate response is "let me get you to a person."


Each scenario needs to run over real telephony, not a text loopback, because the failures that matter in voice AI are audio failures: barge-in that clips a critical word, endpointing that cuts a caller off mid-symptom, TTS that mispronounces a medication name badly enough that the caller thinks they're on the wrong line.


This is where a simulation platform earns its keep.[Roark](https://roark.ai/) dials your agent over real PSTN and WebRTC with defined personas, scenarios, and schedules, then scores each simulated call against the metrics you care about. Runs can gate CI over HTTP, so a prompt change that regresses on the "existing patient disambiguation" scenario never reaches production.


## What to actually measure


Containment rate and average handle time are the two metrics that end up on every voice AI dashboard. Neither of them tells you whether the agent is safe to keep on the phone.


The metrics that matter for patient access break into three groups.


**Workflow completion.** Did the agent finish the task the caller called about, with the right data written back to the EHR? Scheduling completion, intake form completion, eligibility verification success, and referral loop closure.[Thoughtly's healthcare guide argues call volume alone is not a success metric](https://thoughtly.com/blog/10-best-hipaa-compliant-ai-voice-agents-for-healthcare-and-clinics-in-2026) , which is the polite version of the point.


**Safety and escalation.** The rate at which the agent escalates when it should, does not escalate when it should not, and never processes something it should not touch (controlled substances, symptom triage, minor consent). Escalation is a positive signal in patient access, not a failure.


**Audio-native quality.** How the call sounds, not just what it said. Interruptions, dead air, TTS pronunciation of clinical terms, caller emotional register, pacing. A transcript-only score gives you a scheduling agent that reads perfectly and interrupts every fourth caller.


Roark scores every production call against 64+ built-in metrics plus any custom metric you define, using audio-native models that hear pronunciation, emotion, vocal stress, pace, pauses, and interruptions. When a call breaks a threshold, it becomes an issue automatically rather than sitting in a dashboard for a human to notice.


Audit surface: what you should see per call


## HIPAA in the test environment, not just production


Testing a patient access agent creates its own compliance surface. The moment your test harness records or replays a call that contains PHI, the harness is in scope for HIPAA. This is the layer most teams under-think.


Three practical rules:


1. **Use synthetic PHI in simulation.** Personas should carry generated names, DOBs, MRNs, and insurance IDs. Real PHI should never touch a synthetic test path. This is standard practice at healthcare voice AI vendors and worth enforcing in your own testing.
2. **Cover BAAs end-to-end.**[A compliant voice AI system often requires BAAs across the LLM, STT, TTS, telephony, and platform layers](https://www.greetmate.ai/blog/best-hipaa-compliant-voice-ai-agents-2026) . Your testing platform is one of those layers if it stores call audio, transcripts, or scores derived from real production calls.
3. **Handle production replay carefully.** Turning a real failed call into a regression test is one of the most valuable QA moves you can make. It is also the one where PHI most easily leaks into places it should not sit. Either scrub PHI before replay, or keep replay inside the same compliance perimeter as production.


Roark is SOC 2 Type II certified, pen-tested, and offers a HIPAA BAA, which matters here because[business associates were involved in 8 of the 14 largest healthcare breaches in 2024](https://www.greetmate.ai/blog/best-hipaa-compliant-voice-ai-agents-2026) . A testing platform sitting outside your BAA chain is a compliance gap, not a testing shortcut.


## Turn production failures into permanent tests


Even a well-tested patient access agent will fail in production. The specialty is too broad, callers too varied, and prompt or model changes will surface new failure modes. What separates the deployments that stay in production from the ones that get quietly pulled is what happens after a failure.


The workflow that works:


1. Every production call is scored against the same metric suite as the pre-launch simulations.
2. When a call fails a metric, an issue is filed automatically with the call, the transcript, the audio, and the specific metric that broke.
3. The failed call is replayed against updated agent logic to confirm the fix.
4. The scenario becomes part of the recurring regression suite so it cannot silently regress on the next prompt or model change.


Roark's production call replay does this end-to-end: capture the real call, replay it against the updated agent, and turn the failure into a repeatable test. This is the loop that turns a fragile launch into a compounding one, and it is specifically what sample-based QA and dashboard-only observability cannot give you.


## What launch-readiness looks like


If you are the product or QA lead standing up a patient access voice agent, launch-readiness is not a Jira column. It is a small, defensible list of things the agent has demonstrated it can do, reproducibly, before you point real callers at it.


A minimum bar:


- A scenario suite that covers identity verification, insurance disambiguation, symptom triage escalation, refill routing, care-gap outbound with disclosure, and non-English speakers, all run over real telephony.
- Every scenario scored on workflow completion, safety and escalation, and audio-native quality.
- Every production call scored against the same suite, with failures filed as issues automatically.
- Every production failure captured for replay against updated agent logic.
- A testing environment that sits inside your HIPAA BAA chain, using synthetic PHI in simulation and handling real PHI in replay with the same posture as production.


Assort Health didn't ship[190 million patient interactions](https://www.assorthealth.com/blog/assort-health-raises-120-million-series-c-to-scale-largest-deployment-of-ai-agents-for-the-patient-journey) on a random 1% sample review. Cedar didn't hit 400,000 Kora calls at 80% satisfaction by watching a dashboard. Whatever it looks like inside those companies, patient access at scale requires the same three things: simulate before launch, score every live call, and turn every failure into a test you never fail again.


If you're standing up a patient access agent on Vapi, Retell, LiveKit, Pipecat, Bland, or ElevenLabs,[Roark integrates in one click](https://docs.roark.ai/) and can gate your next launch on the scenarios that matter for your specialty. More at[roark.ai/industries/healthcare](https://roark.ai/industries/healthcare) .
