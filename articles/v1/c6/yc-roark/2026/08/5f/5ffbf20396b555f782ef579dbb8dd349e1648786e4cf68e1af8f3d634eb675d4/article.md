---
schema_version: "1.0.0"
document_id: "5ffbf20396b555f782ef579dbb8dd349e1648786e4cf68e1af8f3d634eb675d4"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-code-switching"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:05:25.772588+00:00"
fetched_at: "2026-08-06T14:05:27.239901+00:00"
content_hash: "sha256:3868125b1a932d5f613c410b00a5da7e01ec37981233db0359106cd67e4bdb46"
---

# Testing voice agents against code-switching callers

Two weeks ago, Japanese retailer Yamada Denki turned an OpenAI GPT-Realtime voice agent loose on its online store. In fourteen days,[about 30,000 shoppers used it and 92% of post-conversation survey responses were positive](https://openai.com/index/avatarin/) . The agent runs 24/7, in multiple languages, and it will keep the thread of a conversation when a shopper changes their mind mid-sentence. That is the shape of the deployment we are all now testing against.


The uncomfortable part, if you build voice agents, is that "multilingual" is not one feature. It is at least three: recognition of the language the caller is speaking, reasoning in that language, and speech synthesis back in that same voice. Every one of them has to agree on the same language, every turn, before the caller notices a gap. And when a real bilingual caller mixes two languages in a single sentence, most agents fall over in a very specific way that transcript-only test suites cannot see.


## Code-switching is not "supporting more languages"


Adding Spanish to an English agent means picking an STT that transcribes Spanish, a model that can answer in Spanish, and a TTS voice that can speak it. Code-switching is the caller doing this inside one utterance: "Can you help me with mi cuenta, I think there is a problem with the last charge." The utterance is not English. It is not Spanish either. It is a mixed-language sentence, and a bilingual caller producing it is normal, not adversarial.


A recognizer only needs to identify the language.[An agent has to identify it, reason in it, and speak it back, with three independent components agreeing every turn](https://soniox.com/wiki/multilingual-voice-agents) , and a system that passes a recognizer test can still fail at the agent layer. Which is why generic ASR benchmarks are misleading here: a WER number on a monolingual test set tells you nothing about what happens when a caller switches partway through.


GPT-Realtime made this worse in the good way. OpenAI shipped a model that can[switch seamlessly between languages mid-sentence](https://openai.com/index/introducing-gpt-realtime/) , and its translate variant added[12.5% lower Word Error Rates than any other model tested across Hindi, Tamil, and Telugu](https://openai.com/index/advancing-voice-intelligence-with-new-models-in-the-api/) . Every voice AI platform now has to catch up, and every builder now has callers who will actually try it.


Where a code-switched utterance can fail in the pipeline


## The four failure modes worth naming


I have watched enough post-mortems on multilingual launches to reduce this to four categories. Each one is a different bug, in a different component, and needs a different test.


**1. Recognition falls back to the dominant language.** The caller says "I need to cancel mi reserva for Friday." The STT is configured for English only, so "mi reserva" comes across as garbage tokens (` mih reh sur vah` ) or drops out entirely. The LLM never sees the Spanish. This is a config bug that transcript diffing can catch, if you have paired audio and transcript.


**2. The model replies in the wrong language.** Recognition works. The transcript preserves "mi reserva." But[the language decision does not survive the trip through the pipeline](https://soniox.com/wiki/multilingual-voice-agents) : the model, seeing mostly English, replies in English. To a Spanish-dominant caller this reads as the agent refusing to meet them. This is a prompt or routing bug, and transcript-only evals will happily mark it "correct."


**3. TTS mispronounces the switched span.**[When a TTS model speaks a Spanish phrase inside an otherwise English response, many systems apply English phoneme rules to the Spanish words](https://murf.ai/blog/challenges-in-building-multilingual-voice-agents) . "Reserva" gets read with English R and V. A native speaker hears a slur, not a bilingual agent. This one is invisible to any test that scores the text output, which is almost all of them.


**4. Context resets on the language switch.** The caller starts in English, code-switches to Spanish for one clarifier, then switches back. A well-architected agent[retains all session state across the switch and continues from where it left off](https://murf.ai/blog/challenges-in-building-multilingual-voice-agents) . A poorly-architected one orphans the entities gathered under the previous language model, and the caller repeats their account number. This shows up as a slot-filling regression that only appears in mixed-language calls.


## Why transcript-only tests miss all four


If you scroll through most "multilingual voice agent testing" guides, they describe a matrix of monolingual test cases and an aggregate WER target. That catches failure mode 1 sometimes. It does not catch the other three.


Modes 2 and 4 need conversational evaluation across a multi-turn call, with the language of each turn as a first-class variable. Mode 3 is not a text problem at all: the transcript will say the right words in the right language, and the audio will still be wrong. You need to score the audio, not the transcript, to catch it.


This is where audio-native scoring earns its keep.[Roark's evaluation models score the sound of the call, not just the transcript](https://roark.ai/) : pronunciation, emotion, vocal stress, pace and pauses, and interruptions. Pronunciation scoring is the specific one that catches TTS mode-3 failures, because the score drops when the synthesized audio does not match the target-language phonology.


## Building a code-switching test suite


A working code-switching suite has three moving parts: a persona matrix that generates realistic mixed-language callers, a scenario library that covers the intents you actually serve, and a rubric that scores what matters at each layer of the pipeline.


### Persona matrix


Pick the language pairs that match your caller base. For a US healthcare deployment that probably means English/Spanish first, with English/Mandarin, English/Vietnamese, and English/Tagalog behind it. For UK financial services, English/Urdu, English/Polish, English/Punjabi. For each pair, define personas along four dimensions:


- **Dominant language.** Which language do they start in? Which do they fall back to under stress?
- **Switch pattern.** Do they insert single nouns ("mi cuenta")? Full phrases ("puedes decirme, is this covered")? Or do they switch by turn?
- **Accent within each language.** A Mexican Spanish speaker and a Castilian Spanish speaker will trigger different ASR paths. Do not collapse them.
- **Speech pace and register.** Fast casual code-switching is a harder test than slow careful code-switching, and both happen in production.


Six to eight personas per language pair is enough to cover the surface without exploding into a matrix nobody runs. In Roark,[personas define the caller: distinct voices, languages and accents, speech pace, emotional register, and background-noise environments](https://roark.ai/) , and[the platform simulates and scores in 45 languages and accents](https://docs.roark.ai/) , so you configure the pair once and reuse it across scenarios.


### Scenario library


Seed scenarios from real production calls. Every voice AI platform hands you recordings, and the calls that already contain code-switching are the best source of scenarios you have. Turn each into a persona-driven simulation: same intent, same emotional beats, but now run against your current agent, on demand, over real phone lines.


Roark[dials voice agents over real phone calls, PSTN and WebRTC, not a text loopback](https://roark.ai/) , which matters here because code-switching failures often hide in codec artifacts and packet timing that a text harness never sees.


The minimum scenario set for a mixed-language production deployment:


Scenario Why it exists


Single-noun switch mid-utterance Catches mode 1 (ASR fallback)


Full-phrase switch inside a turn Catches mode 2 (LLM replies in wrong language)


Response contains a switched proper noun Catches mode 3 (TTS phoneme rules)


Switch mid-slot-fill, then switch back Catches mode 4 (context reset)


Switch triggered by frustration Catches emotional-register regression under stress


### Rubric


Score at each pipeline layer, and weight the audio ones more than you think you should.


- **ASR accuracy per language span.** Not aggregate WER. WER on the English spans and WER on the Spanish spans, reported separately.
- **Language-of-response match.** Did the agent reply in the language the caller last used? Or in the language of the majority of the turn? Both are valid targets, but you have to declare one.
- **Pronunciation on switched spans.** Audio-native scoring: does the synthesized Spanish word sound like Spanish?
- **Slot retention across the switch.** Are the entities gathered before the switch still present in the agent's state after it?
- **Task completion.** Did the caller get what they came for, in the language they wanted?


Illustrative pre-launch run of a code-switching suite


## Turning a live failure into a regression test


The value of testing this on real audio compounds when a production call goes wrong. A caller code-switches, the agent mishandles it, the review flags a low pronunciation score, and now you have the exact audio that broke your agent.


The Roark pattern here is production call replay:[capture real production calls and replay them against updated agent logic, turning real failures into repeatable regression tests](https://roark.ai/) . Fix the prompt, fix the router, or swap in a multilingual STT for the affected region, then replay that specific call against the new build before you ship it. If it passes, add it to the suite. If it fails, keep iterating.


This is the piece that transcript-based CI cannot do. A code-switched call is not a text prompt; it is audio with a specific acoustic profile, a specific switch point, and a specific downstream expectation. Replaying it is the only way to know your fix actually holds under the same input.


## What to gate a launch on


For a mixed-language production launch, three gates matter:


1. **Every language pair, every scenario, passes at target.** No aggregate. If English/Spanish passes at 95% and English/Mandarin fails at 62%, you have not shipped a multilingual agent, you have shipped a Spanish agent that also has a Mandarin skin.
2. **Pronunciation scores on switched spans clear a floor.** Set the floor with your voice team, not with the eval defaults. This is the only gate that catches mode-3 TTS failures, and it will be the one your callers judge you on hardest.
3. **Slot retention across switch survives regression.** A specific test where the agent gathers three entities in English, the caller switches to Spanish for one clarifier, then switches back. All three entities should still be present. If they are not, your session-state design is language-scoped and needs to be flattened before launch.


None of this is new theory. It is the same discipline as any other regression suite: fail fast, fail on the input that broke, keep the fixture forever. The difference is that the fixture is a persona speaking mixed-language audio to a real phone number, and the score is on the sound coming back, not the transcript.


Yamada Denki's agent works because someone did this work in Japanese first. The next hundred deployments will be judged on whether their teams did it in the languages their callers actually speak.
