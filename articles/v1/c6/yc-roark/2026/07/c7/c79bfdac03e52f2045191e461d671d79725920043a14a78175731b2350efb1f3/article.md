---
schema_version: "1.0.0"
document_id: "c79bfdac03e52f2045191e461d671d79725920043a14a78175731b2350efb1f3"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-background-noise"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:b2eb17699df8510645053f1f1d64f57bddaa902123140cbb28407620c8027460"
---

# Testing voice agents against background noise

The floor for building a voice agent is now a two-minute afternoon. On July 1, xAI shipped[Voice Agent Builder in beta](https://openai.com/index/introducing-gpt-live/) , a no-code layer that stands up a production phone agent on Grok Voice in about two minutes. A week later OpenAI released[GPT-Live](https://openai.com/index/introducing-gpt-live/) , a full-duplex voice model that listens and speaks at the same time. Ship time keeps collapsing. Test surface keeps expanding.


The gap between "the demo worked on my laptop" and "the agent works when a caller dials from a moving car" is where most voice deployments quietly fail. Your ASR benchmark is a clean-room number. Production is a food-truck drive-thru at lunch, a warehouse floor with a forklift beeping, a customer in the parking lot with wind hitting the mic. If your test suite feeds the agent synthesized speech at 30 dB SNR and calls it a day, you have no idea where your agent's cliff is. You will find it in production, one angry caller at a time.


This post lays out a concrete SNR-tiered test framework for voice agents: what to inject, what to measure beyond WER, how to catch the exact SNR at which your agent falls apart, and how to run the whole thing as a regression test that gates deploys.


## The clean-audio illusion


On clean, studio-recorded English, the best 2026 ASR models sit at[5 to 7% WER, roughly human parity](https://www.forasoft.com/blog/article/speech-recognition-accuracy-noisy-environments) . Vendor pages love this number. It is also almost totally uninformative about how your agent will behave on a real call.


Two things break the number the moment you leave the lab.


First, the audio pipeline itself degrades. Narrowband telephony at 8 kHz sampling adds 8 to 12% absolute WER on top of your baseline, and the[G.729 codec used in VoIP introduces another 10 to 15% relative WER degradation independent of any acoustic noise](https://deepgram.com/learn/noise-robust-speech-recognition-techniques) . Before a single background sound enters the picture, your production WER is already meaningfully worse than the leaderboard.


Second, the acoustic environment changes second by second. IEEE research cited by Deepgram documents[WER climbing from 8% at 20 dB SNR to 40% at 0 dB SNR](https://deepgram.com/learn/noise-robust-speech-recognition-techniques) once packet loss enters the mix. Production data tells the same story from another angle:[agents that hit 94% accuracy in quiet lab settings can drop to 58% in a moving car](https://altersquare.io/voice-agent-production-readiness-checklist-stress-test-enterprise-deployment/) . And[background noise at typical contact-center levels of 55 to 65 dB reduces transcription accuracy by 15 to 30% without a noise-robust model](https://www.builtwithagents.ai/blog/best-ai-voice-agents-background-noise-cancellation) .


The point of a noise test suite is not to reproduce these numbers. It is to find your agent's cliff, on your prompt, with your tools, on your telephony path, before your callers do.


SNR tiers used in a voice-agent noise test suite


## What noise actually breaks


Word error rate is the easy metric to fixate on. It is also incomplete. In a real conversation, an ASR miss cascades. Consider what an extra 10 dB of babble noise does to a typical stack:


- **Endpointing gets jittery.** VAD false positives spike when there is speech-shaped noise in the background, so the agent cuts itself off or starts talking over the caller. This is the actual thing callers notice; they will not describe it as "elevated WER."
- **Barge-in becomes unreliable.** Cross-talk in the room sounds like the caller trying to interrupt. The agent halts mid-sentence for no apparent reason, or fails to yield when the caller genuinely does interrupt.
- **Confirmations blow up.** Names, addresses, account numbers, dates and drug names. These are the tokens that never appear in your general-domain training data, and they are the tokens where a 1% WER hit becomes a 20% task failure.
- **Retries multiply.** Every "sorry, can you repeat that?" doubles latency, doubles the chance of a caller giving up, and doubles the load on any downstream tools waiting on a slot to be filled.
- **Tool calls fire on garbage.** A misheard slot value doesn't fail loudly. It calls your booking API with the wrong date, or your CRM with the wrong account, and the agent confidently confirms nonsense to the caller.


A noise test suite that only reports WER misses all of this. What you want to score, per SNR tier and per noise environment, looks closer to this:


Metric What it catches


Word error rate on slot values Confusable digits, names, medication and product names


Endpoint precision Agent cutting itself off, or waiting forever, when noise is speech-shaped


False-barge rate Agent yielding to background chatter instead of the caller


Turn latency The retries-and-clarifications tax under noise


Task completion Did the agent still finish the workflow, or did it stall


Tool-call correctness Did the extracted slot values actually match the caller's intent


Every one of these should have a threshold that fails the test loudly when crossed. WER alone will not do it.


## An SNR-tiered test framework


Noise robustness is not a boolean. It is a curve, and your job is to trace the curve until you find the cliff. A practical framework:


**Tier 1: 20 dB SNR, mild background.** Coffee-shop chatter at conversational distance, HVAC hum, a TV in another room. This is the everyday call. Your agent should be indistinguishable from a clean run at this level. Any degradation here is a red flag, not a tolerance.


**Tier 2: 10 dB SNR, moderate background.** Open-plan office ambience, a car with the radio off on a highway, kitchen appliances running. This is the median caller. Aim for WER under 10% on slot values and task completion within 5% of your clean-audio baseline. Studies of ASR noise robustness cite[WER under 10% at 10 dB SNR and under 20% at 0 dB SNR](https://altersquare.io/voice-agent-production-readiness-checklist-stress-test-enterprise-deployment/) as reasonable production thresholds.


**Tier 3: 5 dB SNR, hard background.** Restaurant at lunch, a moving car with windows cracked, a small warehouse. Task completion will drop; the point is knowing by how much. If completion falls off a cliff here (rather than degrading gracefully) your endpointing or your confirmation strategy is the problem, not your ASR.


**Tier 4: 0 dB SNR, adversarial.** Signal and noise at equal power. Callers in a stadium, on a construction site, in a vehicle at highway speed with windows down. You will not pass Tier 4 at Tier 1 quality. What you want is that the agent fails cleanly: escalates to a human, offers a callback, does not confidently misfire a tool call.


Beyond raw WER,[Noise-Adjusted WER (NA-WER)](https://altersquare.io/voice-agent-production-readiness-checklist-stress-test-enterprise-deployment/) , the ratio of noisy WER to clean WER, is the metric that actually tells you whether your stack degrades gracefully. An agent whose clean WER is 3% but jumps to 15% under noise (a 5x factor) is less robust than one whose clean WER is 8% but only rises to 12% (a 1.5x factor). The first agent wins the vendor benchmark and loses in production.


## The noise environments that actually matter


Which environments you sample from is not aesthetic. It matters more than most people realize. Research on the CHiME-3 challenge documented that[systems trained on bus environment noise achieved 21.03% WER, while the same systems tested on cafe environment noise achieved 13.06% WER, an 8-point difference from noise-type mismatch alone](https://deepgram.com/learn/noise-robust-speech-recognition-techniques) . If you only test one type, you are not testing noise robustness; you are testing one specific noise.


Academic benchmarks like[EVA-Bench include eight noise environments](https://arxiv.org/pdf/2605.13841) : airport gate, baby crying, background music, bad-connection static, coffee shop, loud construction, NYC street, and road noise. That is a reasonable starting palette, but the environments that matter are the ones your callers actually inhabit. A restaurant reservations agent should be stress-tested against restaurant ambience. A home-services agent should be tested against a caller standing next to a running dishwasher. A roadside-assistance agent should be tested against real road noise with variable wind.


Also test the telephony layer, not just the acoustic layer. A clean caller on a bad connection sounds nothing like a noisy caller on a clean connection. G.711 vs G.729 vs Opus, packet loss at 1% / 3% / 5%, jitter, and codec transcoding across carriers all matter. If your test injection sits above the codec, you are missing half the failure surface.


## Making the whole thing a regression test


Noise robustness testing is not a one-time exercise. Every prompt change, every LLM version bump, every ASR provider switch, every VAD threshold tweak can shift the cliff by 5 dB in either direction. The moment noise testing lives in a Google Doc labeled "we tested this before launch," it stops being real.


Three principles for keeping it live:


1. **Freeze a scenario set.** Persona plus environment plus target task. "Spanish-accented caller booking a hair appointment from a cafe at 10 dB SNR" is a scenario. Same audio, same script, same tool expectations, run against every candidate build. Vary independently: one axis at a time.
2. **Score every run against the same metric threshold.** WER on slot values, false-barge rate, endpoint precision, task completion, tool-call correctness. When a build regresses on any one, the run fails, and the diff points at what changed.
3. **Turn production failures into new scenarios.** When a real caller hangs up because the agent mistook background chatter for a barge, that call is the next regression test. Otherwise the same failure ships again in two weeks.


This is where a synthetic test set built on clean recordings and audio-mixed noise runs out of runway. The end-to-end path you actually care about, telephony codec, jitter, VAD, ASR, LLM, TTS, barge handling, only exists when a call is actually placed. Which is the argument for running this over real phone lines, not through a text or file-based loopback.


## How Roark runs this


[Roark](https://roark.ai/) simulates voice agents over real phone calls, PSTN and WebRTC, and personas are the primary knob. A persona defines the caller's voice, language and accent, speech pace, emotional register, and background-noise environment. The same scenario runs against your agent as a clean cafe-goer, a stressed driver in traffic, an elderly caller with a TV on in the background, or a Spanish-accented warehouse manager next to a forklift.[Roark simulates and scores in 45 languages and accents](https://roark.ai/) , so noise and accent robustness get tested on the same run.


Every call is scored on 64+ built-in metrics and any custom ones you write. The audio-native metrics matter here:[Roark's models score the sound of the call, not just the transcript](https://roark.ai/) , so pronunciation, pace, pauses, vocal stress, emotion and interruption behaviour are graded directly from the audio. That is the layer where noise-induced failures actually surface. When a metric fails, Roark files it as an issue against that call automatically.


Illustrative pre-launch noise robustness suite


Two more pieces make the loop close. First, runs can be scheduled or triggered over HTTP, so a full noise suite can gate a deploy in CI the same way unit tests gate a merge. Second,[Roark's production call replay](https://docs.roark.ai/) captures a real production call, the one where the caller hung up because the agent misheard "seven" as "eleven" over traffic noise, and replays it against your updated agent. Real failures become repeatable regression tests. You do not have to remember to add them.


## The short version


Voice agents ship in minutes now. That does not mean they are ready. The single largest gap between demo quality and production quality is what the audio actually sounds like when a real caller dials in, and background noise is the biggest source of that gap. Test with SNR tiers. Score more than WER. Sample noise environments your callers actually inhabit. Run it every build. Turn production failures into new tests, forever.


The demo will always sound clean. Your callers will not.
