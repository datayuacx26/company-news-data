---
schema_version: "1.0.0"
document_id: "1f4f56340bb8619b528c4286d3effda918a20e1ade004191e5a817ced3fd124b"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/from-dark-matter-to-voice-ai-deepgrams-journey-to-speech-recognition"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:3b7b527b8a8d221c56461243361d4e245d5a034cc283916209e5deffbca6fca5"
---

# From Dark Matter to Voice AI: Deepgram’s Journey to Speech Recognition

Jensen Huang (NVIDIA) said it at a recent GTC keynote:


> “Speech recognition is a solved problem.”


I’ve heard this sentiment echoed across the industry from engineers, investors, and product managers alike. After spending an hour with Andrew Seagraves, VP of Research at Deepgram, I’m convinced we’re nowhere close to that in real-world conditions.


The reality is more nuanced and more interesting than the “solved” narrative suggests. What we have is a technology that works remarkably well under specific conditions. The moment you step outside those boundaries, things fall apart in ways that would surprise most people building with these systems.


## Dark Matter & Speech Decoding


Deepgram’s origin story isn’t what you’d expect from a speech recognition company. The founders were dark matter physicists shooting audio signals into the earth and measuring what came back, using machine learning to detect whether dark matter was present. Around 2015, they were also working on an unrelated side project: recording themselves for weeks at a time, attaching mics to their clothing and capturing their everyday lives.


They ended up with this massive corpus of audio they could never actually listen to — who has time to review weeks of recordings to find the interesting moments? So they started building machine learning tools to search it. This side project became the foundation of the company.


The early team built a deep search algorithm and indexed all of YouTube. They demoed it on stage with Jensen at GTC, showing they could find random audio clips across a YouTube-scale corpus. But here’s the thing about timing: search wasn’t a hot market. Speech recognition, on the other hand, was emerging as a green field with few players and models with limited performance across most use cases.


The initial hypothesis was straightforward: combine an end-to-end deep learning system with lots of high-quality data, and you could build a model that transcribes potentially any human in any situation. They saw early adoption from call center AI platforms that had massive volumes of audio they wanted to transcribe and analyze. The call center domain was narrow enough that early deep learning models could hit 80–90% accuracy when specialized on that data.


## The Misconception That Everything Just Works


When Andrew said speech recognition isn’t solved, I questioned that assumption. I use transcription tools constantly. They work great. What’s actually broken?


His answer reframed how I think about the problem: “It’s only solved in some very narrow situations. It works well where large volumes of data are available.”


Think about the data distribution. Call center audio in English, there’s an ocean of it. Podcast conversations between native speakers on high-quality microphones, abundant. Earnings calls from Fortune 500 companies, are extremely well documented.


But step outside those domains and the models start hallucinating, dropping words, or producing outputs that sound plausible but are completely wrong. Non-English languages remain “pretty terrible across the board” in Andrew’s words, and it’s not because the research community hasn’t tried. It’s a data scarcity problem that compounds itself.


The challenge that surprised me most was rare and localized words. Every customer has terminology specific to their domain, product names that don’t exist in any training corpus, jargon that their team uses daily. How the user’s name is spelled. Drug names that even pharmacists mispronounce. These aren’t edge cases — they’re the core content that matters most to the transcription’s actual usefulness.


Andrew shared a scenario that made me laugh because it’s so painfully relatable: a customer calling their pharmacy, and neither the customer nor the pharmacist knows how to pronounce the drug name. You have this incredibly broad distribution of possible pronunciations, and the model has never seen any of them because nobody knows what the “correct” pronunciation is in the first place.


## From Web-Scale Chaos to Production Quality


The training process for state-of-the-art speech models follows a pattern that parallels what we’ve seen with large language models: a two-stage approach with pre-training and post-training.


In the first stage, you’re maximizing scale. Scrape as much audio from the web as possible, covering as many voices and acoustic conditions as you can find. You filter for audio that has human transcripts where those transcripts are reasonably accurate. The goal is broad exposure — speakers, environments, the full range of how words are actually pronounced in the wild.


This is roughly how Whisper was produced, and it gets you a model that’s “pretty good” in Andrew’s estimation. But it’s not production ready.


The problem becomes apparent when you examine what happens at scale. As your corpus grows, common words like “and” and “the” appear millions of times. Meanwhile, the words that actually matter in domain-specific applications might appear once or twice. Seven orders of magnitude separate the most frequent words from the least frequent ones. This class imbalance is brutal for machine learning and significantly impacts model performance.


Andrew shared something Deepgram has observed but hasn’t published: there’s a threshold around 10,000 to 100,000 exposures where the model saturates on a word. Providing more examples beyond that point doesn’t improve accuracy. Below that threshold, word error rate directly correlates with exposure count. The long tail of rare words never gets enough examples to reach saturation.


The second stage addresses this through fine-tuning on a smaller, more carefully curated corpus with gold-standard labels from humans following prescriptive style guides. This is where you “unlearn” the inconsistencies from web-scale training. Different humans transcribe in different ways — some use punctuation liberally, others don’t; some spell out numbers, others use digits. The first stage absorbs all that chaos. The second stage imposes consistency.


This two-stage pattern explains a lot of frustration developers have when deploying Whisper directly. You’re using stage one of a production pipeline. The model will insert words that aren’t there, omit words that are clearly present, and produce inconsistent formatting. These aren’t bugs in Whisper specifically — they’re expected behaviors from any model that hasn’t gone through rigorous post-training.


## The Physics of Real-Time: Buffers, Latency, and Trade-offs


Real-time transcription operates under fundamentally different constraints than batch processing. In batch mode, you have the luxury of seeing the entire audio file. You can look forward and backward in time. The model has complete context about what was said.


Real-time strips that away. You’re making predictions on small chunks of audio as they arrive, without knowing what’s coming next. This creates a core tension between latency and accuracy that forces hard engineering decisions.


The key parameter is how much future context you accumulate before emitting a prediction. With zero buffer, you transcribe the moment audio arrives — but you’re often catching someone mid-word. The model doesn’t actually know what word they’re saying yet, and it’s likely to guess wrong.


Adding a half-second buffer significantly improves prediction accuracy. The model sees enough context to make accurate predictions, and most users barely notice the delay. But shrink that to 200 milliseconds and the trade-offs bite. At 50–100 milliseconds, you’re hitting fundamental limits where the model must wait some minimum time just to hear enough audio to have a chance at correctness.


I found myself thinking about this like video streaming buffers. You’re constantly building up a queue, processing buffered audio once sufficient confidence is achieved, and serving that to the user while the next chunk accumulates. The queue grows and shrinks in real time as you balance between waiting for accuracy and responding quickly.


The interesting frontier is forward prediction — could you model what someone is about to say? Andrew compared it to what Google’s Genie is doing with video prediction, where the system predicts multiple possible futures (user turns left, user turns right) and then validates against what actually happens. Applied to speech, you might predict several possible word completions, rank them, and commit once you have enough audio to confirm which prediction was correct.


This starts looking like distributed systems architecture rather than traditional ML. Separate models working asynchronously, checking in with each other, with something like a load balancer serving as source of truth. Andrej Karpathy has described transformers as nodes communicating with each other, and maybe there are generalizations of that idea that would let you train this kind of multi-stream prediction system end-to-end.


## Data Beats Architecture


When I asked what differentiates Deepgram’s approach from competitors, Andrew’s answer was immediate: “Getting the data right is really the biggest competitive advantage and secret sauce.”


The architectural innovations matter less than you’d think. Transformers work. If you take a transformer, train it on a lot of data, it trains robustly and produces a good model. This paradigm has held across domains — vision, language, audio. Deepgram rode that wave early, building encoder-decoder transformer models similar to Whisper by late 2021, before OpenAI released Whisper publicly.


The scarier part of that story stuck with me. Those early encoder-decoder models had the ability to “say anything.” The decoder is essentially a language model that predicts one token at a time while looking at an audio representation. When your training data is noisy — when there’s imperfect correspondence between audio and transcript — the model learns to hallucinate. It gains degrees of freedom to say things that aren’t in the audio.


Earlier CTC-based models were much more constrained. They didn’t have the freedom to generate arbitrary text, which made them more predictable but less capable. The trade-off was explicitness: newer architectures are more powerful but require cleaner data to avoid pathological behaviors.


Deepgram’s bet has been to stay in a constrained design space where models must be small and fast while investing heavily in data quality. They won’t ship giant models regardless of accuracy gains because giant models don’t meet latency requirements. Instead, they leverage data advantages to make smaller models more efficient. Better data means you can achieve the same accuracy with a smaller architecture.


## From NASA to the Drive-Through


The range of environments where speech recognition is being deployed today would have seemed like science fiction a decade ago. Andrew walked me through some of the more challenging acoustic conditions they’ve tackled.


NASA wanted real-time transcription of astronauts. Not in a studio, not in a quiet office — in actual space mission acoustic environments with all the noise and communication artifacts that entails. Deepgram built custom models trained on narrow but complex data distributions, and it worked. The recipe of taking a general model and fine-tuning on domain-specific audio holds up even when that domain involves orbital vehicles.


Food ordering through voice AI is another frontier that’s proven surprisingly difficult. The customer is often outside — traffic noise, wind, other people in the background. Sometimes the audio is so degraded that what the person says is literally indiscernible. The model doesn’t know that. It still produces an output. And when it hallucinates “number one” instead of “number three” due to poor audio quality, someone gets the wrong order and their day is ruined.


Until last year, this was a real problem in production. Models would randomly transcribe the wrong menu item number even though those words sound completely different to a human ear. The reason wasn’t model architecture — it was that the audio itself contained insufficient information to make a correct determination.


The pathology gets worse when models trained on multiple customers’ data start mixing up menu items across companies. If the model is guessing at what it heard and hasn’t been properly scoped to a single customer’s vocabulary, it might confidently produce a menu item from a competitor. That’s not a small error. This introduces significant business and compliance risks.


Medical transcription presents the word frequency problem in its most extreme form. The acoustic environment is relatively easy — doctors in offices, standard room acoustics. But medical terminology is wildly diverse. Every specialty has its own vocabulary. Drug names are invented by pharmaceutical marketing teams with no concern for pronounceability. And the model needs to get every one of them right because clinical notes affect patient care.


## Generating the Data That Doesn’t Exist


For languages and domains with insufficient training data, the traditional approach has been human labeling. You identify interesting audio streams, have humans transcribe them, and add that to your training corpus. It’s slow, expensive, and doesn’t scale to the pace required to support hundreds of languages.


Deepgram is betting on a paradigm shift: generating synthetic training data using audio and speech generation models. Instead of waiting to collect rare audio samples, they’re building models that can generate them. A person speaking Spanish with a specific regional accent, standing on a street corner with traffic noise, talking into their phone while wind distorts the audio. Generate millions of variations, train on those, and bootstrap your way to coverage you could never achieve through collection alone.


This mirrors what’s happening in language modeling, where the community has hit limits on naturally occurring data and shifted focus toward post-training with reinforcement learning and synthetic data augmentation. The paradigm shift changes what you use humans for. Instead of brute-force labeling, humans identify what data distributions are interesting, help build auxiliary models for clustering and embedding, and validate synthetic outputs.


The timeline compression this enables is significant. Low-resource languages that would take years to accumulate sufficient training data might be synthesized in months. It’s still early days for this approach, and Andrew was careful to note that it’s “a frontier problem that not a lot of people are working on yet.” But the direction seems clear.


## The Audio Intelligence Frontier


Beyond transcription, there’s a category of capabilities Andrew calls “audio intelligence” — understanding not just what someone is saying but what state they’re in. Are they at their baseline emotional state or are they upset? Sad? Excited? This context dramatically affects how a conversation should unfold.


Text-to-speech systems can now accept emotional conditioning and produce appropriately modulated responses. But that capability is meaningless if the speech-to-text side can’t detect the emotional state in the first place. You end up with AI systems that are tone-deaf in a literal sense — unable to match the energy of the conversation because they have no way to measure it.


The approaches people have tried so far involve grafting audio encoders onto LLMs. Take an LLM because it’s smart and understands content, integrate Whisper or another audio encoder, fine-tune the combination. The results are models that understand language well but don’t effectively model acoustic features. The representations they produce internally suggest they haven’t learned the acoustic features that encode emotion.


Andrew’s intuition is that real audio intelligence needs to be built from the ground up, trained on tasks that involve both audio and text simultaneously. The challenge is discovering what those tasks should be. One provocative idea he mentioned: bootstrapping from video intelligence models. If you train a model on video where you can see facial expressions and body language along with audio, it might learn audio-only features that correlate with emotional states. Then you could potentially distill those capabilities into an audio-only system.


## When to Stop Listening and Start Talking


End-of-turn detection — knowing when someone has finished speaking so you can respond — is a problem that sounds simple until you try to model it. Most voice agents today use a basic heuristic: if I think the person has stopped, I’ll start responding as quickly as possible.


This produces conversations that feel robotic. Humans don’t always wait until the other person is completely finished. We interject. We overlap. We know through some complex social calculus when it’s appropriate to jump in and when it’s not.


Andrew describes the “start of speaking” signal as much harder to model because it’s inherently subjective. Is this the right time to interject? The answer depends on context, relationship, cultural norms, and individual personality. Some people are natural interrupters. Others wait patiently for clear openings. Both can be appropriate in different settings.


The parallel to LLM alignment is illuminating. You can train a model on human conversations and it will learn to exhibit all kinds of different interruption behaviors — some appropriate, some rude, some perfectly timed, some awkward. Then you need a separate alignment stage to shape those behaviors into something appropriate for the specific use case.


We joked about putting two voice-to-voice models in a room together. At Agora we do this frequently to test our systems. What you get is surprisingly theatrical — the models take turns, wait politely, never interrupt. It’s civilized in a way that actual human conversation rarely is. The models are missing whatever signal tells humans when it’s okay to step on each other’s words.


## Models That Learn From You


The closing part of our conversation ventured into what feels like the next major capability gap: personalization and real-time adaptation.


Currently, when a speech model gets something wrong and you correct it, that correction doesn’t improve the model for next time. You have to repeat the same correction the next time the same situation occurs. The model isn’t learning from its mistakes.


The vision Andrew outlined involves federated learning approaches where your phone’s local model updates its weights based on how you specifically speak and what corrections you provide. You say a word it gets wrong, you correct it, and that feedback signal propagates into the model so it doesn’t make the same error again.


The mechanics are tricky. You probably can’t update model weights in real-time during a conversation — the computational cost is too high. But you might be able to edit the activations that the model produces, take a different computational path to the output, and store that modification. Later, those stored signatures get incorporated into actual training.


This is emerging research, not production capability. But it represents the direction the field is moving. The goal is models that start generic and become increasingly personalized through use, learning your vocabulary, your accent, your terminology, without requiring manual fine-tuning or explicit customization.


## What I’m Taking Away


After an hour with Andrew, my mental model of speech recognition has shifted substantially. This isn’t a solved problem with minor remaining challenges. It’s a partially solved problem where the partial part — English, clean audio, common vocabulary — has been commoditized while the remaining challenges represent years of research ahead.


The technical trade-offs are more nuanced than I appreciated. Buffer size, model size, training data composition, and post-training approach all interact in ways that require deep expertise to navigate. The gap between a Whisper-style foundation model and a production-ready system is substantial.


Most importantly, data remains the dominant factor. Architectural innovations help at the margins, but the teams winning in this space are the ones who’ve figured out how to source, label, and curate high-quality audio across the domains they need to support. Synthetic data generation might compress those timelines, but the underlying principle holds: better data yields better models, and there’s no shortcut around that fundamental constraint.


For anyone building voice AI applications, the implication is clear: don’t assume the foundation models are production-ready out of the box. Understand the acoustic environments your users are actually in. Identify the vocabulary that matters to your domain. And be prepared to invest in data quality if you want reliable results.


The future Andrew described — models that understand emotion, that learn from interaction, that handle code-switching between languages — sounds like science fiction. But so did the current capabilities a decade ago. The field moves fast when the fundamental approach is sound and the scale continues to increase. I’m betting we’ll see more progress in the next five years than we’ve seen in the past ten.


## Generating the Data That Doesn’t Exist


## The Audio Intelligence Frontier


The approaches people have tried so far involve grafting audio encoders onto LLMs. Take an LLM because it’s smart and understands content, bolt Whisper or another audio encoder onto it, fine-tune the combination. The results are models that understand language well but don’t really understand audio. The representations they produce internally suggest they haven’t learned the acoustic features that encode emotion.


## When to Stop Listening and Start Talking


## Models That Learn From You


## What I’m Taking Away


Watch my discussion with Andres in its entirety:


*This post is based on a conversation from the*[Convo-AI World podcast](https://podcast.convoai.world/) *. Andrew Seagraves is VP of Research at*[Deepgram](https://deepgram.com/) *, where he leads the team building next-generation speech recognition systems.*


‍
