---
schema_version: "1.0.0"
document_id: "de6217b5a1aff04598b42f6d4ebd833a52479ad6206c13c3b1f8a9dd75f38042"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-outbound-voice-agents-ivr-navigation"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T20:51:50.087453+00:00"
fetched_at: "2026-08-04T21:21:09.879682+00:00"
content_hash: "sha256:4c164d9048372f802af8f337cf66384a32881983cb6e6ffe48f8033ca57005de"
---

# Testing outbound voice agents that navigate phone trees

Outbound voice agents now spend a large chunk of their first sixty seconds talking to something that is not a human. They call insurance carriers, utilities, DME suppliers, dealerships, or the customer's own bank, and before they can do their job they have to defeat somebody else's phone tree. Press 1 for billing. Press 2 for claims. Enter your ten-digit member ID followed by the pound key.


That leg of the call is where most outbound agents quietly fail, and it is the leg that almost nobody covers in QA. Teams write scenarios for the human-facing part of the mission (the pitch, the objection, the confirmation) and skip the two-menu, one-hold, one-DTMF-block preamble that decides whether the mission gets to happen at all. This post is about building a test suite for that preamble.


## The outbound agent's opening is now a real problem


The runtime side of IVR navigation has grown up fast.[Vapi's IVR navigation guide](https://docs.vapi.ai/ivr-navigation) walks through pause characters, batched digit sequences, and provider-by-provider DTMF variance.[Retell's press-digit primitive](https://docs.retellai.com/build/single-multi-prompt/press-digit) separates audio-input IVRs (spoken responses) from DTMF-input IVRs (keypad only).[Telnyx's June 2026 outbound-hold-agent walkthrough](https://telnyx.com/resources/how-to-build-outbound-hold-agent) splits the outbound stack into an IVR assistant and a representative assistant, each with a different job. The pattern is now standard: agent dials out, listens to a menu, sends DTMF without overtalking, waits on hold, detects a human, resumes with context.


The testing side has not caught up. If your outbound agent selects the wrong branch on a menu, sends digits too fast for the far end to register, or starts its pitch to a hold-queue announcement, no eval running against transcripts will catch it. You need calls that hit a real phone-tree target, and you need scoring that understands both audio behavior and DTMF sequences.


Outbound IVR navigation, end to end


## What actually breaks


Six failure modes cover most of the ground.


**Overtalk during menu playback.** The system prompt says the agent should stay silent until it hears the full menu; the LLM decides five seconds is long enough and starts talking. On a real IVR this either drops the digit or bounces you back to the root menu. On a webhook transcript this looks like a normal turn.


**DTMF timing.** IVR systems are picky about how fast digits arrive. Vapi's own guidance is to insert` w` (0.5s) or` W` (1s) pause characters between keys and to send required digits in a single call rather than several,[because separate DTMF calls can arrive too slowly to be registered as a sequence](https://docs.vapi.ai/ivr-navigation) . The same code path can pass Twilio and fail Telnyx, or pass on a wideband demo and fail on 8kHz PSTN.


**Wrong-branch selection.** "Press 2 for billing questions or press 3 for account changes." The agent is calling to update a payment method. Is that billing or account changes? A model can guess right nine times and wrong the tenth. That tenth call now belongs to whoever answers on the billing queue.


**Silent hangups when the menu changes.** IVRs get updated. Options move. "Press 3" was billing last month and is now "Spanish." The agent presses 3, hears Spanish, and either hangs up or spends thirty seconds trying to language-switch a menu. A test suite that only asserts against yesterday's menu wording will not catch this.


**Modality switching.** Some flows require voice input for one prompt and keypad for the next. ("Say the reason for your call, then enter your account number followed by pound.") Voice agents that switch cleanly between speaking and DTMF are rare, and the switch is a common source of stuck states.


**Voicemail masquerading as human.** Answering machine detection is a probabilistic signal. If AMD returns "unknown" the agent has to decide whether the polite pause on the line is a human waiting or a voicemail greeting still loading. Guessing wrong wastes a message or delivers a pitch to nobody.


## DTMF is not a text field


The mental model that trips people up: DTMF looks like typing into a form, so teams reason about it like typing into a form. It is not. Each digit is a two-tone audio signal transmitted in-band or out-of-band depending on your carrier's setup. Timing, gaps, and duration all matter.[SwiftCase's IVR navigation reference](https://swiftcase.co.uk/2026/01/17/ai-ivr-navigation-dtmf-voice-agents/) puts the accepted character set at` 0-9` ,` *` ,` #` , and` w` , and notes that connection failures during DTMF can be recoverable because the call itself remains active even when the media stream drops. That "call is up, media is down" state is the kind of thing that lives in production incident write-ups and not in scenario docs.


Provider variance is real. Vapi explicitly recommends[comparing Twilio, Telnyx, Vonage, and BYOK SIP transports for the specific IVRs you target](https://docs.vapi.ai/ivr-navigation) , because DTMF implementation differs across providers. Your test suite has to cover the transport too, not just the agent logic.


## The behaviors you actually need to score


Scoring the human-facing part of an outbound call is well understood. Scoring the IVR-facing preamble is not. A useful test suite scores at least these behaviors on every call:


- **Silence during menu playback.** Continuous audio energy from the agent while the target IVR is speaking is a defect. This is an audio-level check, not a transcript check.
- **Digit sequence and order.** The agent sent` 1` , then` 4` , then` #` , in that order, with the right pauses.
- **Time from menu-end to first digit.** P50/P95 latency between the target finishing its prompt and the agent sending the first tone. Too fast and IVRs miss digits; too slow and the menu times out.
- **Branch selection correctness.** The agent landed in the queue the scenario says it should have. This is the hardest one to score without a target IVR that knows what state it's in.
- **Retry behavior.** When the target IVR says "we didn't get that, please try again," does the agent retry with the same input, retry slower, or bail?
- **Human-detected vs voicemail-detected.** Did the agent correctly identify the far end as a human before launching its pitch?
- **Hold behavior.** Did the agent stay quiet during hold music, or start pitching to the on-hold recording?


None of these are optional if the agent is expected to complete missions against real phone trees.


## How to simulate this properly


The uncomfortable part is that you need a target IVR to test against, and no two real IVRs are the same. Building the harness right means three things:


**1. A programmable target.** The other end of the call has to be a fake phone tree that plays menus, listens for DTMF, times out on missing input, and can be reshaped between runs. You want to be able to say: "for this scenario, the menu is three options deep, option 2 requires a 10-digit account number, and the second prompt asks for a spoken reason." Then you want a second scenario with the same mission but a different menu structure, to confirm the agent doesn't hard-code the branches.


**2. Real telephony.** Not a text-in/text-out loopback. If the goal is to catch DTMF timing bugs, codec-related overtalk, and provider-specific weirdness, the test call has to travel through PSTN or WebRTC the same way a production call does. Bench-level unit tests that stub out audio will pass while the same code fails on Twilio.


**3. Audio-native scoring.** Transcript-only scoring cannot see overtalk during a menu, cannot see the length of the pause between the menu ending and the first digit, and cannot see whether the agent kept its mouth shut during hold music. The scoring layer has to look at the audio, not just the words.


Overtalk during menu playback caught mid-simulation


## Roark, briefly


This is the shape of testing[Roark](https://roark.ai/) does. Simulations dial your outbound agent over real PSTN or WebRTC. Personas play the target: an IVR persona can speak menu prompts in a chosen voice, at a chosen pace, and expect DTMF in return; the same persona framework covers 45 languages and accents when the far-end IVR is not English. Every call is scored on Roark's built-in metric suite plus any custom metrics you define, and the scoring is audio-native, so overtalk during menu playback and undue silence between prompt-end and first digit both show up as failed checks rather than as clean transcripts. Failures are filed as issues, and the failed call becomes a replay that runs on the next candidate build.


If you already run inbound simulations, the outbound-with-IVR case is the same primitive with the roles inverted. Roark supports both: it can dial your agent's inbound number, or provision a number that your outbound agent dials,[triggered over HTTP so runs can gate CI](https://docs.roark.ai/) . Real production calls captured from your telephony stack can be replayed against updated agent logic, which is how the "menu changed last Tuesday" failure mode stops repeating.


## A concrete outbound-IVR suite


A minimum-viable suite for an outbound agent that navigates other companies' phone trees looks roughly like this:


Scenario What it exercises


` two-level-menu-happy-path` Baseline. Agent selects the right options and reaches the target department.


` two-level-menu-shuffled` Same tree, options in a different order. Agent must listen, not hard-code.


` deep-menu-with-account-number` Four-level tree ending in a 10-digit DTMF entry followed by` #` .


` menu-with-spoken-prompt` One level requires a spoken reason for the call before the keypad prompt.


` menu-times-out` Target IVR waits for input and repeats the menu twice, then hangs up. Agent should not overtalk repeats.


` wrong-input-retry` Target rejects the first digit sequence. Agent must retry rather than proceed.


` answering-machine` Target plays a voicemail greeting. Agent should identify and either leave a message or hang up per policy.


` hold-then-representative` After correct DTMF, target plays 20s of hold audio then a human greeting. Agent must stay silent, then pitch on human.


` menu-change` Same target as` two-level-menu-happy-path` , but option labels are swapped. Regression check for hard-coded branches.


` narrowband-8khz` Any of the above, but over an 8kHz codec instead of wideband. Catches codec-driven ASR errors mid-menu.


Each scenario has three assertions minimum: correct final state (which queue or which human), correct DTMF sequence sent, and zero overtalk on menu audio. Add latency percentiles per scenario if the outbound platform has hard timeouts.


Outbound-IVR suite results


## Ship the suite before the agent


Outbound agents that call other businesses look impressive in demos because the demo target is either a real IVR the founder already knows or a scripted mock the team built by hand. Both cases hide the real risk: on the next call, the menu is different, the timing is different, and the model has to reason from audio, not from a fixture.


The fix is not clever prompting. The fix is a target-IVR simulator, a real telephony path, audio-native scoring on every run, and every real-world failure turned into a repeatable regression test. That is table stakes for any outbound agent whose job description includes "gets past a phone tree." If it is not in your pre-launch suite, the phone tree is your acceptance test.
