---
schema_version: "1.0.0"
document_id: "b4feed4dcc10ef77b94d04013a1f00d7f6fa319f47bdbf4ad48d2c2ccbbc9189"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/why-speech-recognition-isnt-solved"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:ace9d57ae37c5c2d56ad67fd61907a39b273bc55054c9bbcbcd588a2ef8d706f"
---

# Why Speech Recognition Isn't “Solved”

"Speech recognition is solved." Sure, it is. Until you add background noise. Until your user has an accent. Until two people are speaking at once. I've heard this claim since I started covering voice AI, and the more I dig in, the less it holds up.


Ricardo Herreros Symons has been hearing it since 2017. As Co-founder of Speechmatics, he's spent years building for everything the demo doesn't show - the edge cases, the noise, the accents, the real-time complexity that separates a proof of concept from a production system. His answer to "it's solved" is three words: solved for whom?


I sat down with Ricardo in a conversation that cut straight to where voice AI stands - and what it genuinely takes to get it right.


## **The Bar Isn’t One Human - It’s All of Humanity**


When we discuss accuracy, we usually compare AI to a single person. Ricardo reframed this entirely.


*"We're not expecting the models to be as good as a human. We're expecting the models to be as good as all humans."*


The expectation for modern Speech AI isn't to be as good as *you* or *me* ; it’s to be as good as what all of humanity can achieve collectively. Think about what that means in practice. A specialist radiologist knows medical terminology that 99% of the population couldn't spell. A linguist speaks six languages. We expect a single AI model to master 100+ languages, understand every regional dialect, handle specialized jargon, and do all of this reliably in real-world noisy conditions. That's not one human's capability; that's the collective intelligence of humanity as the benchmark.


And for Ricardo, this isn't an abstract technical challenge. His younger brother in Spain has struggled with literacy his entire life. When voice interfaces arrived on phones, his world opened up - searching YouTube, navigating apps, communicating in ways that reading and typing never allowed. "That is the real stakes of understanding every voice," Ricardo said. The mission is democratization, not just accuracy.


## **Diarization: The Essential Security Layer**


Most people focus purely on the transcript, but in high-stakes industries like finance and healthcare, *what* was said is often less important than *who* said it. This is where Diarization becomes critical.


In a regulated environment, for example, consider a voice agent handling a call with a financial institution, and if a second voice appears in the background, the entire interaction could be compromised. Real-time diarization serves as an authentication layer, ensuring the agent responds only to the authorized speaker. It provides a level of metadata that text alone simply cannot capture.


*"A quick response is great," Ricardo noted, "but a quick response to the wrong thing? Game over."*


The same principle applies in healthcare. Physicians are drowning in administrative work. AI that can listen to a consultation, distinguish between doctor and patient, and automatically populate an Electronic Health Record isn't just saving time. It's preventing errors by ensuring every instruction is attributed to the right person.


## **Latency: The "First Correct Word" Rule**


The industry is obsessed with "Time to First Byte," but Ricardo argues this is a vanity metric. Many systems "game" this by spitting out a random placeholder token immediately just to lower their perceived score, even if that token is eventually corrected.


At Speechmatics, the focus is on **Time to First Correct Word** .


- ‍ **The 1.2-Second Sweet Spot:** Too slow feels like a walkie-talkie; too fast (under a second) feels like the AI is constantly interrupting you or "barging in."
- ‍ **Demographic Nuance:** Interestingly, different users prefer different speeds. An older demographic buying high-value wine might prefer a 2-second pause to allow for a more "thoughtful" conversation, whereas a tech-savvy user wants near-instant results.


Speechmatics operates on the **Pareto Frontier** : they aren't trying to be the absolute fastest model at the cost of "hallucinations." They prioritize the highest possible accuracy at a competitive latency, giving the downstream LLM the time it needs to generate a high-quality response.


## **Cascaded vs. Speech-to-Speech: The Enterprise Reality**


There is a lot of hype around direct speech-to-speech models that skip the text phase entirely. While they offer a natural "feel" and lower latency, Ricardo remains firm on the necessity of **Cascaded Architectures** (Speech-to-Text → LLM → Text-to-Speech) for enterprise-grade applications.


Why? Because text is a controllable, auditable pillar. In a regulated industry, you need to be able to apply guardrails, PII redaction, and safety filters to the text before it ever becomes audio. You cannot afford a "hallucination" when a medical dose or a financial trade is on the line. Control is the currency of the enterprise.


**What Ricardo Wants Builders to Take Away**


Speech AI is not a commodity, and the teams treating it like one are finding that out the hard way. The real differentiation lives in the long tail: noise, accents, multiple speakers, regulated environments, and the edge cases no benchmark warned you about.


Ricardo's position is clear. Get the speech-to-text right first, because everything downstream inherits its mistakes. Invest in diarization before it feels urgent. And close the gap between demo-room performance and production readiness before your users close it for you.


The full conversation goes deeper - including Ricardo's take on why AI models may already understand certain accents better than most humans do, and what that says about where this technology is genuinely headed.


**Watch the full episode here:**[https://www.youtube.com/watch?v=YNIbq4cFVzo](https://www.youtube.com/watch?v=YNIbq4cFVzo)


**Ready to build?**


- ‍ **Explore:**[Agora’s Conversational AI Engine](https://www.agora.io/en/products/conversational-ai-engine/)
- ‍ **Learn:**[The Anatomy of Voice AI Agents](https://www.agora.io/en/blog/the-anatomy-of-voice-ai-agents/)
- ‍ **Join:**[Our Developer Community on Discord](https://discord.gg/agora)
