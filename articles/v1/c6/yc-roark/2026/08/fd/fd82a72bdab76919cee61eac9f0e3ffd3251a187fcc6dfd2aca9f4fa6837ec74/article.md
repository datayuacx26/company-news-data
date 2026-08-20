---
schema_version: "1.0.0"
document_id: "fd82a72bdab76919cee61eac9f0e3ffd3251a187fcc6dfd2aca9f4fa6837ec74"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-that-take-payments"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T21:21:45.043968+00:00"
fetched_at: "2026-08-11T21:21:46.377796+00:00"
content_hash: "sha256:51cd125e2b18f0a431b2beaa6056653f2ffd5120191c672ddc2b95af4cd7ecd2"
---

# Testing voice agents that take payments over the phone

Sierra shipped[the first Level 1 PCI-compliant payment capability for AI agents](https://sierra.ai/blog/payments) this spring, verified by the Visa Global Service Provider Registry, and Bland followed with[its own guide to running card payments through a voice agent](https://www.bland.ai/blog/pci-compliance-credit-card-over-phone) this week. Payments are moving into the conversation itself, not a transfer to a separate IVR. That is a real product improvement, and it is also a new QA problem sitting on every voice PM's roadmap.


The mistake to avoid is treating "PCI-compliant payments" as a green light to skip the testing. The vault is certified. The choreography around it is where your agent will break, and the failures show up in transcripts, recordings, and support tickets long before they show up in a QSA report.


## The payment moment is the highest-risk 30 seconds of your call


For most voice agents, taking a payment is a narrow window with three things that must happen in order: the agent recognizes the intent, control transfers to a PCI-scoped capture path (typically DTMF, sometimes a mid-call SMS link), and control returns cleanly to the agent to confirm the transaction and close the call. Any one of those three can fail, and each fails in its own way.


The industry consensus is that[speech capture of spoken card numbers is a compliance anti-pattern](https://www.shuttleglobal.com/guides/ai-voice-agent-pci-payments/) . PCI DSS does not name DTMF specifically, but[treats it as cardholder data the moment it enters a system you operate](https://www.shuttleglobal.com/guides/dtmf-payments/) . Any architecture that lets a spoken PAN reach your STT, your LLM, or your call recording drags that entire path into scope. So a "payment test" is really two tests stacked: does the flow work, and does the flow keep card data out of everything that is not the payment layer.


The payment handoff pipeline every scenario has to survive


## Three failure modes worth testing for


**Card data residue.** The right design has[DTMF captured in isolation by the payment layer, never entering the AI pipeline or the call recording](https://www.shuttleglobal.com/guides/ai-voice-agent-pci-payments/) . That works, provided your agent actually stops encouraging the caller to say the number out loud. Callers who do not hear the "please enter your card using the keypad" prompt clearly, or who default to speech from habit, will read the digits aloud. Your STT captures them. They land in the transcript. Your PCI scope quietly grows, and nobody notices until an auditor asks to see a sample of call recordings.


**The silent handoff failure.** In Sierra's design,[the agent switches to a secure transaction flow which removes the agent while the processor completes the payment](https://sierra.ai/blog/payments) . That is exactly the right architecture, and it also means the LLM is not there to gracefully recover if the payment session fails to start. If the DTMF cutover does not happen (the caller's carrier strips tones, the session times out, the processor is degraded), the agent should notice and route the customer somewhere useful. Left untested, "somewhere useful" is often dead air.


**Abandonment mid-capture.** Even when everything works, the payment moment is where callers drop off. They cannot find their card. Their toddler shouts. They hear the flat masking tone and worry the call died. The scope-reduction win of moving from[SAQ D (329 controls) to SAQ A (22)](https://www.paytia.com/solutions/dtmf-masking) is only real if payments actually complete; abandoned flows push the volume back to a human queue and quietly negate the automation gain that justified the project.


## You cannot test the vault. Test the choreography.


The PCI-certified payment layer is closed to you as a customer. You cannot fuzz it, and you should not try. The testable surface is the agent's behavior on either side of the vault: the words leading up to the capture, the transitions in and out, and the recovery when the transaction succeeds, fails, or hangs.


Framed that way, the test plan writes itself. You need scenarios that exercise every branch the agent can take before the payment layer takes over, every state the payment layer can return, and every combination of caller behavior around those states. That is dozens of scenarios per agent for a bill-pay flow, and hundreds for a full-stack use case like healthcare, where[platforms are now handling scheduling, intake, referrals, medication refills, real-time eligibility, and payments](https://www.assorthealth.com/blog/assort-health-raises-120-million-series-c-to-scale-largest-deployment-of-ai-agents-for-the-patient-journey) in the same conversation.


What the PCI vault handles versus what your tests have to cover


## The pre-launch payment test suite


A minimum viable pre-launch suite for a voice agent that takes payments has the following scenarios. Each one runs over a real phone call rather than a text transcript, and each is scored on both the transcript and the audio. If a caller can reach the payment step, every branch below needs at least one deliberate test.


**Intent and framing**


- Caller who has been told the amount and is ready to pay
- Caller who wants the amount broken down first
- Caller disputing the amount before entering the flow
- Caller who wants to pay a different amount than the balance
- Caller with a stored card on file who wants to use "the card you have"


**Handoff to capture**


- Successful DTMF handoff, first attempt
- Caller who starts to say the card number aloud instead of pressing keys
- Caller on a device or carrier that does not cleanly send DTMF
- Caller who interrupts the "enter your card" prompt with a question
- Caller who asks a support question mid-capture ("does this include tax?")


**Payment layer outcomes**


- Approved
- Declined (insufficient funds)
- Declined (fraud rules or issuer block)
- Timeout with no response from processor
- Duplicate charge risk (caller submits, then re-enters "just in case")


**Recovery**


- Retry with a different card, same call
- Fall back to a payment link sent by SMS mid-call
- Escalate to a human with full context, including what was and was not attempted
- End the call cleanly with a callback promise on file


**Compliance safety net**


- Assert that no full or partial PAN, expiry, or CVV appears in the transcript
- Assert that the audio recording contains only masked tones during the capture window
- Assert that any tool call fired during capture carries no card data in its arguments


The last three checks are the ones that get skipped, and they are the ones that matter when your quarterly PCI review shows up. They also cannot be verified by reading a transcript. They require replaying the actual audio and inspecting the actual event log for the call.


## What to score on every live payment call


Pre-launch scenarios buy you a launch. What keeps you launched is scoring every real payment call against the same suite. There are five metrics worth watching separately from your overall call quality dashboard, because payment failure modes hide inside otherwise "successful" calls.


1. **Payment completion rate.** Of calls where the agent reached the payment intent, what percentage produced a processor confirmation. Split by branch: first attempt, retry, SMS fallback, escalation.
2. **Time-to-capture.** Seconds from the agent's first "please enter your card" prompt to the first DTMF digit received. Leading indicator of caller confusion.
3. **Speech-in-window rate.** Percentage of payment windows where speech was detected in the caller channel. A non-zero number is usually a prompt-wording problem, and it is always worth investigating.
4. **Silent-recovery rate.** Percentage of failed payment sessions where the agent produced no coherent recovery turn in the four seconds following the failure. Dead air after a decline is the worst possible ending.
5. **PAN residue rate.** Percentage of call transcripts and recordings that contain content matching card-shaped patterns during or after the payment window. Target zero, alert on anything above.


None of these five can be measured from the transcript alone. Silent-recovery and speech-in-window require audio-native scoring; PAN residue requires inspecting both the audio and the transcript against pattern rules; time-to-capture requires timestamps aligned with the payment layer's event log. Your evaluation harness has to see the sound of the call, not just its words.


## Turning a failed payment into a regression test


The pattern that separates teams that ship payment flows safely from teams that ship them and hope is boring: every real failure becomes a test that prevents that same failure from ever shipping again. The caller who read their card aloud last Tuesday becomes a persona in your simulation suite. The BIN that consistently declines through your test gateway becomes a scenario in your regression run. The dead-air-after-decline call gets replayed against the fixed prompt to confirm the fix and against every subsequent prompt change to confirm the fix stays.


This is where[Roark](https://roark.ai/) fits into a payment-taking voice agent's lifecycle. Roark dials your agent over real phone calls, using personas that speak the way your callers actually speak, in[45 languages and accents](https://docs.roark.ai/) . The same runs can be scheduled, gated into CI, or triggered on every prompt change, and every simulated call is scored against the metric suite you defined for payments, along with any custom metrics you add for compliance checks like PAN residue. When something breaks in production, production call replay turns that call into a repeatable regression test so the same failure cannot ship twice.


A pre-launch payment suite run against a voice agent


For teams in regulated verticals, Roark is SOC 2 Type II certified with a HIPAA BAA available, which matters when the call recordings and transcripts you are replaying can contain health information, financial information, or both. If you are building payment flows into a healthcare agent, the[healthcare industry page](https://roark.ai/industries/healthcare) covers what changes when HIPAA meets PCI in the same call.


## The rule of thumb


Every payment your voice agent takes is a small transaction inside a much larger transaction with the caller: a promise that you handle their money safely. The certified payment layer handles the first promise. Your testing handles the second one. Ship both, and the "PCI-compliant voice payment" your vendor sold you actually is one.
