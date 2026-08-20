---
schema_version: "1.0.0"
document_id: "9db2f77dc68b526e7bca1333d3ceea40d75b67586cbb985dd749bb4e4763b6fb"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agent-medication-name-pronunciation"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:aeef40e544885cc405cf8205a0b5597893ba728dec66ce2d85576f56757e27db"
---

# Testing voice agent pronunciation of medication names

If your voice agent handles refills, intake, discharge instructions, or any other call where a drug name gets spoken back to a patient, your worst regressions will almost never be latency or barge-in. They will be pronunciation. A caller who hears "hy-DRAL-a-zeen" when the chart says hydroxyzine has been handed a safety issue, not a UX one, and the failure mode is silent: your logs say the agent read the correct token, your ASR loop transcribes fine, and the only person who noticed the drift is the patient hanging up confused.


The problem is getting more attention this quarter. Deepgram's June 2026 self-hosted release shipped an upgraded Nova-3 Medical batch model with, in the changelog's phrasing, expanded medical vocabulary and improved medical term recognition ([Deepgram changelog](https://developers.deepgram.com/changelog) ), and back in May, Aura-2 added runtime pronunciation controls in English and Spanish on both batch and streaming endpoints ([Deepgram changelog](https://developers.deepgram.com/changelog) ). Better inputs on both sides of the loop are helpful, but they do not remove the need for a regression harness. If you don't test drug-name pronunciation on every voice or model bump, you are shipping on vibes.


## Why medication names are the hardest thing your agent has to say


Two things make drug names uniquely hostile to TTS. The first is that they are phonetically dense strings that sit outside the training distribution of any general voice model, full of Greek roots, Latin stems, and manufacturer coinages. General-purpose TTS reads them as if they were English words, guesses stress placement, and produces something confidently wrong. ElevenLabs' own best-practices docs acknowledge this bluntly: <cite index="14-1,14-2">V3's IPA support achieves 80 to 90 percent pronunciation consistency, more reliable than v2's XML phoneme tags but not 100 percent consistent</cite>. That is with an explicit IPA transcription. Without one, you are relying on grapheme-to-phoneme guessing every time the model warms up.


The second problem is that drug names cluster into confusable pairs, and the confusability is often acoustic, not orthographic. <cite index="19-6,19-7">If a look-alike or sound-alike (LASA) error is not intercepted, it can reach the patient and may result in harm, and these errors occur because of shared linguistic properties between names</cite>. Estimates for how often this happens in human workflows range from <cite index="19-8">7 percent of near misses to between 6.2 and 14.7 percent of all medication error events</cite>. A voice agent that mispronounces hydralazine as hydroxyzine has not just made a UX error, it has moved a real-world LASA pair from "distinguishable on paper" to "identical on the line."


The regulator response is already codified. <cite index="22-3,22-4,22-5">The FDA has incorporated the tall man lettering system for drug names that may be confused due to similarities in appearance or sound; using TML, dexmedetomidine and dexamethasone appear as dexmedeTOMidine and dexameTHASONE, drawing attention to the portions of the name which are dissimilar</cite>. Tall man letters help pharmacists reading a label. They do nothing for your TTS engine, which cheerfully lowercases the input and hits the same phoneme sequence for both members of the pair.


## The two failure modes to test for


Before you build anything, be clear about what you are actually trying to catch. There are exactly two failure modes on the TTS side of a voice agent:


1. **Pronunciation drift.** The agent says a drug name, and the acoustic output does not match the accepted clinical pronunciation. This is the "atorvastatin becomes AY-tor-VAY-stayn" case. It is annoying, it erodes trust, and it correlates strongly with model or voice updates.
2. **LASA confusability.** The agent's pronunciation of drug A is acoustically indistinguishable from drug B, where A and B are on a published LASA list. This is the safety case. It can be triggered by a bad phoneme choice, wrong stress placement, or over-smoothed prosody that collapses two similar-sounding names.


Your test suite has to cover both, because a fix for one can break the other. Aliasing "clonidine" to "cloNIDine" for stress may pull it closer to klonopin. Sound engineering means measuring drift and confusability together on every release candidate.


How the medication-pronunciation test loop fits together


## Build the golden set


Start with a set of names, not sentences. You want three overlapping pools:


- **Top-N by call volume.** The medications your agent actually reads back. Pull them from production transcripts (Roark's call review is the fastest way in if you're already ingesting calls, but any transcript store works). Bias toward the long tail: your top 20 drugs will be fine on any half-decent voice, and your bugs live in positions 200 through 2000.
- **LASA adversarial pairs.** Import from the ISMP and USP confused-drug lists. <cite index="20-7,20-8,20-9">Organizations like the FDA and the Institute for Safe Medication Practices (ISMP) regularly issue an updated list of LASA medicines, identifying pairs such as metronidazole and metformin</cite>. For every drug in your top-N, if it has a LASA partner, add the partner even if it never appears in your call logs. You are testing whether your TTS can distinguish them.
- **Structural stressors.** Newly approved brand names, hyphenated compounds ("insulin-glargine"), suffix families (the -mab monoclonal antibodies, the -pril ACE inhibitors, the -statin family), and dose expressions that ride alongside the name ("500 milligrams, twice daily"). These are where G2P and text normalization interact.


Assign each entry a **canonical pronunciation** : an IPA string, a CMU Arpabet transcription, or, at minimum, a sounds-like hint. This is the ground truth you regress against. It is fine to generate a first pass with an LLM (ElevenLabs' docs suggest as much: <cite index="10-13">you can use AI tools like Claude or ChatGPT to help generate IPA or CMU notations for specific words</cite>) but a pharmacist should sign off on the final list. That review is a one-time cost per drug and it eliminates whole categories of downstream noise.


## Wire in the pronunciation dictionary


Every serious voice platform now supports a pronunciation dictionary or its equivalent. Use it. The mechanism differs by provider:


- **ElevenLabs** : <cite index="10-2,10-3,10-4">pronunciation dictionaries let you customize how the agent says specific words using IPA or CMU alphabets, though phoneme tags only work with eleven_flash_v2 and eleven_v3 models</cite>. Store the dictionary as a` .pls` file and attach it to the agent's voice config.
- **Cartesia** : <cite index="12-13">"sounds-like" aliases and IPA notation, with IPA supported on the sonic-3 model</cite>.
- **Vapi** : routes through the underlying provider. <cite index="12-13">Pronunciation dictionaries are supported by ElevenLabs (phoneme rules) and Cartesia (aliases and IPA), and Vapi's built-in voices support a unified locator</cite>.
- **Deepgram Aura-2** : runtime pronunciation controls now available in English and Spanish across batch and streaming, per the changelog above.


If you're on a stack that doesn't support IPA, alias-style substitution ("write hydroxyzine as hi-DROX-ih-zeen in the SSML pipeline") gets you 80 percent of the win with none of the phoneme headaches. It also works across model swaps, which matters because you will swap models.


A worked example for a Vapi + ElevenLabs stack, storing the entries as a PLS file:


```text
<?xml version="1.0" encoding="UTF-8"?>
<lexicon version="1.0"
xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
alphabet="ipa" xml:lang="en-US">
<lexeme>
<grapheme>hydroxyzine</grapheme>
<phoneme>haɪˈdrɒksɪziːn</phoneme>
</lexeme>
<lexeme>
<grapheme>hydralazine</grapheme>
<phoneme>haɪˈdrælə.ziːn</phoneme>
</lexeme>
<lexeme>
<grapheme>atorvastatin</grapheme>
<phoneme>əˌtɔːrvəˈstætɪn</phoneme>
</lexeme>
</lexicon>
```


Two practical notes from working with these files. First, <cite index="14-49,14-50">phoneme tags only work for individual words; if you have a name with a first and last part that you want pronounced a certain way, you will need a phoneme tag for each word</cite>. So multi-part brand names ("Lantus Solostar") get one entry per token, not one entry per brand. Second, <cite index="14-6">include stress markers (ˈ for primary stress, ˌ for secondary stress) for multi-syllable words</cite>. Stress placement is the single largest driver of LASA collapse in our experience.


## Snapshot regression: catch drift the moment it happens


A dictionary sets the intended pronunciation. It does not guarantee the model honors it, and even when the model does honor it, output can drift between voice versions or model updates. You need a snapshot regression: render every entry in the golden set on every candidate build, and diff the audio against a stored reference.


The mechanism most teams land on:


1. For each drug name, render an isolated audio clip on the current production model. Store it as the reference.
2. On every candidate build (new voice ID, new model version, new dictionary revision), render again.
3. Compare using a **phonetic string distance** on the ASR re-transcription (this catches gross substitutions), plus an **acoustic distance** on the audio itself (mel-cepstral distortion or a learned embedding). Flag anything above threshold.
4. For flagged entries, route to a blind human listen. Rating scale: correct, minor drift, wrong. Two out of three raters agreeing is enough for a block.


The ASR-loop step alone catches a surprising amount. FutureAGI's team makes a similar observation in a different context, noting that <cite index="2-15">common symptoms of TTS accuracy problems include transcript mismatches between intended output and ASR re-transcription, and eval failures clustered around a provider, voice, accent, or prompt version</cite>. Cluster your fails by voice ID and model version and you'll see the pattern in a week.


Illustrative pre-launch pronunciation regression run over a medication golden set


## Adversarial simulation for LASA pairs


Snapshot regression tells you the pronunciation of drug A is stable. It does not tell you that drug A is distinguishable from drug B. For that, you need adversarial simulation.


The setup:


1. Build a scenario per LASA pair. Two variants: one where the caller asks about drug A, one where they ask about drug B. The agent must speak the name back in context ("your refill for hydroxyzine is ready").
2. Run the scenario against the agent over a real transport (PSTN or WebRTC), not a text-only harness. Codec loss and jitter are part of what collapses close pairs, and a purely offline TTS eval will miss them entirely.
3. Score two things: did the agent pronounce the intended drug's name correctly, and does an independent ASR transcribe the recorded audio as the intended drug? A pass requires both.


Running this against Vapi, Retell, LiveKit, Pipecat, Bland, or ElevenLabs agents is exactly what Roark's simulation testing is built for. It[dials your agent over PSTN or WebRTC](https://roark.ai/) and executes scenario suites end-to-end, which is what you want for LASA testing because the failure mode you care about is what a phone caller hears, not what the TTS renders in a quiet booth. The[one-click integrations for Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs](https://docs.roark.ai/) mean you're not building this harness from scratch.


The reason to test over the wire rather than on the TTS output directly: telephony codecs (G.711 μ-law, Opus at low bitrates) smear formants. Two pronunciations that are cleanly distinguishable in the studio render can collapse on the line. If your regulator asks whether your agent can safely say "hydralazine" vs. "hydroxyzine" over a phone call, the correct answer references a test that involved a phone call.


## Score pronunciation on the actual audio


The last piece is scoring. String-level checks and ASR round-trips get you far, but the ground truth is the audio. Roark's audio-native models measure pronunciation directly on the recorded call ([along with emotion, vocal stress, pace and pauses, and interruptions](https://docs.roark.ai/) ), which is the level of analysis you need when the failure is a stress placement or a vowel collapse rather than a missed word. You get a per-utterance pronunciation score you can threshold against, and calls that fail the threshold get filed as reviewable issues automatically.


Illustrative Roark review of a mispronounced medication name flagged on a live call


The reason to do this on production calls, not just pre-launch simulations, is that pronunciation regressions from provider-side model updates are the ones you cannot predict. A voice you froze on eleven_flash_v2 in April can shift subtly when the provider updates the underlying weights. If you are not continuously scoring pronunciation on shipped calls, you find out about the regression from a patient complaint.


## A minimum viable regression checklist


Pull this into your CI and you will catch the vast majority of medication-name failures before they reach a call:


- **Golden set** : top-200 by call volume, plus every LASA pair for any drug in that set, plus 50 structural stressors. Each entry has a pharmacist-signed IPA transcription.
- **Dictionary** : PLS or provider-native, checked into the same repo as the agent config, versioned. Every top-200 entry is covered. Stress markers on multi-syllable words.
- **Snapshot regression** : on every model or voice change, render the golden set, compare acoustic distance against the reference, threshold, route flagged entries to human review.
- **LASA adversarial simulations** : one scenario per pair, run over PSTN or WebRTC, pass requires both correct human-judged pronunciation and correct independent ASR of the recording.
- **Production monitor** : audio-native pronunciation scoring on every shipped call that mentions a drug name from the golden set. Alert on threshold breach.
- **Rollback plan** : pinning the exact model and voice IDs your dictionary was validated against, plus a documented process to unpin when you have re-validated.


None of this eliminates the risk. What it does is move the failures from "surfaced by a patient" to "surfaced by CI," which is the entire game. In a regulated call flow, that's the difference between an issue you fix in a sprint and one you fix in front of a regulator.


If you're building the harness yourself, most of the primitives (PSTN dialing, transcript diffing, phoneme-level scoring on the audio, ISMP LASA import) are things you can wire up. If you'd rather not, that's what[Roark](https://roark.ai/) is for: the simulation dialer, the audio-native pronunciation model, and the production-call replay all sit on infrastructure that's[SOC 2 Type II certified with a HIPAA BAA available](https://roark.ai/industries/healthcare) . Either way, the harness needs to exist before your agent handles its next refill call.
