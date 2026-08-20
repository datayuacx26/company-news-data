---
schema_version: "1.0.0"
document_id: "f24b78ab23f5184929427ad251e432b1b28a9b1125b42e3fee54c1f8c3f10c62"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/multilingual-speech-to-text-achieving-native-level-accuracy-in-60-languages"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:386fe2cf6729172bddeec2a0c8b9ed7977a1bb01f8bc7519b1b5b7d1198c8840"
---

# Multilingual Speech-to-Text: Achieving Native-Level Accuracy in 60+ Languages

I’ve spent a lot of time recently talking to founders who are trying to crack the code on voice AI. Most of these conversations center on incremental gains - a slightly better Word Error Rate (WER), a marginal improvement in latency, or a faster inference loop. But my recent sit-down with **Klemen Simonic, the Founder and CEO of Soniox** , felt different.


Accuracy in speech AI isn’t about optimizing English benchmarks. It’s about making systems work reliably across languages, accents, and real-world conditions.


From what I’ve seen, most teams are optimizing at the margins. Klemen is solving a different problem entirely - redefining what “accuracy” actually means. Not incremental gains in controlled settings, but native-level performance across 60+ languages, in real time.


## The "English-First" Bias in Speech Recognition


The biggest challenge in speech-to-text isn’t accuracy in English—it’s achieving consistent accuracy across languages. If you have 15,000 hours of labeled data, English works great. The real challenge—and the question most developers ask—is: *How do you make a minority language, like Danish or Arabic, work with the same surgical precision as English when you have almost no labeled data for it?*


Klemen didn’t just stumble into this space during the recent LLM gold rush. He’s been in the trenches since 2008. After five years at Meta building the core speech systems for billions of users, he realized the industry had a fundamental bias. Most Automatic Speech Recognition (ASR) models are "English-first," leaving other languages as second-class citizens.


## The AI Data Factory: Beyond Labeled Data


At Soniox, Klemen has built what he calls an AI Data Factory. Instead of relying on slow, expensive human labeling, they use massive self-supervised learning on petabytes of audio data.


This approach creates a single universal model that "speaks" 60 languages fluently. One of the most important technical insights was how this solves the "Global Entity" problem. If a French-only model hasn't seen the name "Elon Musk" in its specific training set, it might stumble. But because the Soniox foundational model is a multilingual beast, it learns entities in English and naturally carries that knowledge over to French, Italian, or Japanese.


## Whisper vs. Soniox: Why Accuracy Matters


We can’t talk about ASR without mentioning OpenAI’s Whisper. It made speech recognition widely accessible, but production systems demand a different level of robustness. That’s the gap Klemen focuses on. In recent benchmarks, Soniox has shown significant improvements over Whisper, particularly in:


- **Real-time Latency** : While Whisper often requires "batch" processing (waiting for the audio to finish), Soniox provides low-latency, streaming ASR.
- **Hallucination Reduction** : One common issue with general models is "hallucinating" words that weren't spoken. Soniox’s proprietary architecture is tuned to minimize these errors, which is critical for medical and legal applications.


## Breaking the Real-Time Translation Barrier


Real-time translation still has a fundamental problem: latency gap.


We’ve all used tools where you speak, wait for the sentence to end, and then wait another three seconds for the translation to pop up. It kills the flow of conversation.


What’s interesting here is how Soniox approaches this differently. Instead of treating translation as something that happens after you finish speaking, the model works in-stream - predicting and translating in chunks as you speak. The result is a delay of just a second or two, not several. And that shift fundamentally changes how these systems can be used in the real world:


- **Global Business** : Closing deals where emotion and nuance are preserved across language barriers.
- ‍ **Accessibility** : Providing the Deaf and hard-of-hearing community with sub-second, highly accurate transcripts.
- **Healthcare:** Capturing patient-doctor interactions with HIPAA-compliant AI that ensures data privacy and residency.


## The Future: AGI and Self-Evolving Systems


I asked Klemen a broader question at the end: *Where is this all going?* He’s looking beyond just "transcription." He’s interested in self-evolving learning systems- AI that can explore new things and organize itself over time, moving closer to the goal of Artificial General Intelligence (AGI). Imagine an AI that doesn't just record what you said, but understands the context of a conversation held six months ago and applies to it today.


## Why This Matters for Developers


If you’re building real-time applications, you already know that the "real-time" is the hardest part to get right.


Speech systems don’t fail in controlled environments - they fail in production. Across languages, accents, network conditions, and noisy inputs. That’s where most systems break down.


What stood out in this conversation is a shift in how to think about the problem. It’s not just about accuracy in isolation - it’s about delivering consistent, low-latency performance across real-world scenarios. Whether you're building a global gaming platform with real-time voice chat or a telehealth app that needs SOC 2 Type 2 security, the goal is the same: language should never be a barrier to entry.


Check out the full episode here:[https://www.youtube.com/watch?v=0SIK7VJ-MWo&t=29s](https://www.youtube.com/watch?v=0SIK7VJ-MWo&t=29s)


## **Ready to build?**


- Explore:[Agora’s Conversational AI Engine](https://www.agora.io/en/products/conversational-ai-engine/)
- Learn:[The Anatomy of Voice AI Agents](https://www.agora.io/en/blog/the-anatomy-of-voice-ai-agents/)
- Join: Our[Developer Community on Discord](https://www.google.com/search?q=https://www.agora.io/en/community/)
