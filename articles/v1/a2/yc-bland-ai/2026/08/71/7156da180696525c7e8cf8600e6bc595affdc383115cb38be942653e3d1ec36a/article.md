---
schema_version: "1.0.0"
document_id: "7156da180696525c7e8cf8600e6bc595affdc383115cb38be942653e3d1ec36a"
company_key: "yc-bland-ai"
company: "Bland AI"
source_id: "yc-bland-ai-news-import-c77b84ac3c61"
canonical_url: "https://www.bland.ai/blog/bland-speech-v3-audio-realism-bench"
published_at: "2026-08-04T17:00:00+00:00"
first_seen_at: "2026-08-05T06:36:26.192411+00:00"
fetched_at: "2026-08-05T06:36:27.857379+00:00"
content_hash: "sha256:ec02cf02db3ac498a3b51cd323ab5d0d8ff61d275106dae59c4a9626169eee04"
---

# Introducing Bland Speech v3: The first Human Speech Engine

[Back to blog](https://www.bland.ai/blog)


# Introducing Bland Speech v3: The first Human Speech Engine


A text-to-speech platform trained on real human conversations, for developers building voice AI that sounds just like a person.


[Cole Baker](https://www.bland.ai/blog/author/cole-baker) August 4, 2026


5 min read


Design Arena, Audio Realism Benchmark. Bars start at 500, not zero.


Today, we’re introducing Bland Speech, a TTS platform for developers building real-time voice applications. We are launching with our first model and will add new models over time.


We built Bland Speech v3 to be the most human-sounding text-to-speech platform for voice AI.


On Design Arena's Audio Realism Benchmark, Speech v3 ranked ahead of ElevenLabs, OpenAI, Cartesia, and xAI, and lost first place only to real humans.


Speech v3 is fast, accurate, expressive, and stable, and it does not immediately register as AI. If an application is talking to a person, sounding human is part of the product. It shapes whether the conversation feels natural, whether users stay engaged, and whether voice belongs in the experience at all.


## Why this benchmark matters#


The TTS industry often measures models one quality at a time: latency, robustness, text accuracy, naturalness, or expressiveness. Those measurements are useful. Developers need to know that a voice will respond quickly, pronounce the right words, and remain stable in production.


But a caller does not experience those qualities one at a time. They hear one voice and make one immediate judgment: does this sound like a person?


Audio realism measures that complete impression. It captures how pronunciation, rhythm, pacing, emphasis, pauses, and tone work together.


That distinction matters most in conversational AI. A synthetic cue in a short narration clip may be easy to overlook. During a live call, every turn creates another opportunity for an evenly paced sentence, a misplaced pause, or the wrong emphasis to reveal the system. Small misses accumulate across a conversation.


A useful benchmark therefore needs to test the experience developers are actually shipping.


The Audio Realism Benchmark is[Design Arena's blind listening test](https://www.designarena.ai/methodology/audio-realism-benchmark) . It scores a fixed, versioned set of naturalistic prompts, most of them up to roughly 40 words, or about 15 seconds, across three registers: phone agents, conversations, and explainers. The 500 prompts used for scoring are held out and kept private. They come from permissively licensed datasets, HarperValleyBank and The People's Speech, with entities and topics swapped so no model can have trained on them, while the underlying speech patterns stay intact.


Design Arena picks two voices per model, one female and one male, each the provider's most naturally conversational American-English voice. Pairings are same-gender, and the prompt is selected at random. A listener hears two clips of the same prompt from two different models. The clips are unlabelled and the left-right order is randomized. The listener picks the one that sounds more human. Real human recordings enter the same pool, each prompt read by a native speaker, so the benchmark measures how often a model gets picked over a person. Those pairwise votes are aggregated with the Bradley-Terry model, the maximum-likelihood method behind Elo-style ratings. Acting-heavy speech, such as game characters, dramatized fiction, and persona-driven companion voices, is out of scope for this version and deferred to a future expressive track.


## Most TTS are trained to perform. Bland Speech is trained to converse.#


Many speech models learn from professional recordings: audiobooks, podcasts, voiceovers, narration, and carefully staged studio reads. That data is excellent for teaching a model to pronounce words clearly and deliver complete sentences with a polished cadence.


But a conversation is not a performance.


In real conversations:


- People speak in fragments, then correct themselves.
- They interrupt, hesitate, repeat words, and change direction.
- Their pace and tone shift with the meaning of the moment.
- They pronounce the same sentence differently depending on what was said before it.
- They use small signals like pauses, emphasis, breaths, and fillers to show understanding.


These signals are what make a voice sound human.


Bland Speech was built from over 100 million real human conversations, and that training data teaches a different kind of speech. Consider a simple response such as “Sure, I can help with that.” Said after a routine request, it should sound quick and confident. Said after someone explains a difficult problem, it may need more space and care. The words are identical. The conversation tells the voice how it should sound.


## A voice is more than an interface#


We saw what that can mean while working with James Piazza, a stroke survivor who lost the ability to speak in his original voice.


Using a set of home videos from before his stroke, our team rebuilt a voice that felt recognizable as his own. His wife, Stacy, sent us two of them, a Father's Day morning and a haircut in the kitchen, and between them they held about thirty seconds of him talking. We isolated the audio, trained a voice model on it, and built him an app with a predictive keyboard designed for imprecise typing, because the stroke affected his right hand, and a library of preset phrases his family can edit and grow.


James knew a company was coming to film his family's story, but he did not know why we were really there until we handed him the phone. His own verdict on hearing himself again was “It is going to be me,” and his mother, Rose, listening on speakerphone, said “It sounds just like Jay.” In July he walked into a burrito shop he had been going to for years and ordered a large breakfast burrito with red salsa in his own voice. His family is bringing the app to his speech therapist.


This is an unusually personal application of TTS, but it reflects the same principle behind Bland Speech. A voice carries identity, history, personality, and emotion. Generating it well requires more than converting text into clean audio.


For developers, the lesson is practical: the voice is part of the product experience. Users do not experience a latency score, an architecture diagram, or an API response in isolation. They experience the person, or agent, they believe is speaking to them.


## Built for developers#


Bland Speech is a platform, and Speech v3 is the first model on it. The first release gives developers:


- Generation through a single endpoint, POST /v1/speak, returning PCM16 WAV at 44.1 kHz.
- Streaming over both HTTP chunked transfer and WebSocket.
- A Speech studio with Director, which drafts the dialogue as well as speaking it.
- Performance tags such as \[laughs\] and \[clears throat\].
- Instant voice cloning from about ten seconds of audio, and professional cloning from thirty minutes or more.
- A stock voice library that ships with Karen, Valentine, David, and Allie.


We will continue adding models so developers can choose the right voice and performance profile without rebuilding their speech stack. Our documentation includes an API reference, a quickstart, and implementation guides for cURL, Python, and Node, along with a CLI, a Node library, and an MCP server:[https://docs.bland.ai/sdks/bland-tts](https://docs.bland.ai/sdks/bland-tts)


If your AI talks to people, its voice should be trained for the way people actually talk.


## The first release#


Bland Speech is available to everyone, free to start, beginning today. Developers can try it through the Speech studio at[https://studio.bland.ai/signup](https://studio.bland.ai/signup) . New accounts get 133,000 characters, a little over two hours of speech, and after that it is $0.015 per 1,000 characters, the same rate in the studio and through the API. Credits do not expire, and a $5 credit load unlocks professional voice cloning.


We will keep adding models, voices, languages, controls, and developer tools as the platform grows. We want to give every developer the speech infrastructure to build voice AI that holds up when a real person answers.
