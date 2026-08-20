---
schema_version: "1.0.0"
document_id: "2b933e9b33d2794704137bac9788533492928bc3d49207e1358918c4a78db841"
company_key: "zoom-communications-inc-class-a-common-stock"
company: "Zoom Communications Inc."
source_id: "zoom-communications-inc-class-a-common-stock-rss-cc53c6254da1"
canonical_url: "https://www.zoom.com/en/blog/zoom-scribe-speech-accessibility/"
published_at: "2026-07-24T14:11:03+00:00"
first_seen_at: "2026-07-24T17:14:06.758504+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:81911c68de762e8c0082515166ffae4958eba4d7480c41987e50dcb7014416b1"
---

# What the Speech Accessibility Project teaches us about the future of speech AI

In this blog


- 01 Measuring what matters - Jumplink to Measuring what matters
- 02 Accessibility defines quality - Jumplink to Accessibility defines quality
- 03 Production makes research accountable - Jumplink to Production makes research accountable
- 04 From conversation to completion - Jumplink to From conversation to completion
- 05 A foundation for healthcare AI - Jumplink to A foundation for healthcare AI
- 06 A shared achievement - Jumplink to A shared achievement
- 07 The person behind every conversation - Jumplink to The person behind every conversation


Share this post


Xuedong Huang


Chief Technology Officer


Xuedong Huang is the Chief Technology Officer (CTO). Prior to Zoom, he was at Microsoft where he served as Azure AI CTO and Technical Fellow. His career is illustrious in the AI space: he began Microsoft’s speech technology group in 1993, led Microsoft’s AI teams to achieve several of the industry’s first human parity milestones in speech recognition, machine translation, natural language understanding, and computer vision, is an IEEE and ACM Fellow and an elected member of the National Academy of Engineering and the American Academy of Arts and Sciences.


Xuedong received his Ph.D. in EE from the University of Edinburgh in 1989 (sponsored by the British ORS and Edinburgh University Scholarship), his MS in CS from Tsinghua University in 1984, and BS in CS from Hunan University in 1982.


For more than four decades, I have worked in speech recognition. When I began, the goal was clear: teach a computer to recognize human speech. It was an extraordinarily difficult problem. Early systems struggled even with carefully spoken words under controlled conditions. Every improvement was hard won.


Since then, the field has made remarkable progress. What once belonged largely in research laboratories has become part of everyday life. Speech recognition is one of artificial intelligence's great success stories. Yet after all these years, I have come to believe that the real challenge was never simply recognizing speech. It was recognizing the extraordinary diversity of the people who speak.


People communicate differently across languages, accents, ages, environments, abilities, and health conditions. A child learning to speak. An older adult whose voice has changed. A person recovering from a stroke. Someone living with Parkinson's disease, cerebral palsy, or another condition that affects speech. These are not edge cases. They are part of everyday humanity.


They also remind us of a standard our industry must hold itself to:


*The true measure of speech AI is not how well it recognizes the average voice, but how faithfully it recognizes every voice.*


## Measuring what matters


That is why the Speech Accessibility Project is so important. Led by the University of Illinois Urbana-Champaign, with participation from organizations across academia and industry, the project was created to address a persistent gap in speech technology. Its dataset includes about 1,000 hours of contributed speech from people with diverse and non-standard speech patterns.


The project asks a better and more demanding question than many traditional benchmarks:


***Who does speech AI still fail to understand?***


That shift matters. It moves the focus from average performance to individual experience—from what works under common conditions to what works for people who have historically been underrepresented in training data and evaluation.


Internally, we evaluated the production Zoom AI Services Scribe API on this benchmark exactly as it is available today. There was no benchmark-specific tuning, and no separate model created for the evaluation.


The result was a word error rate of 5.72%.


Scribe's word error rate was approximately 52% lower than that of the next-closest system:


Model Word Error Rate ↓


Zoom Scribe API


5.72%


Microsoft/Azure-LLM-speech


11.99%


Elevenlabs/Scribe_v2


12.20%


Microsoft/Azure-fast-transcription


12.93%


Assemblyai/Universal-3.5-pro


13.31%


Deepgram/Nova-3


17.06%


Cartesia/Ink2


19.70%


Openai/gpt-realtime-whisper


21.68%


Lower WER = better accuracy. Benchmark: Speech Accessibility Project dataset.


On a benchmark this demanding, reducing recognition errors by half is more than an incremental improvement. It suggests that the frontier is beginning to move on a problem our field has wrestled with for decades.


We are proud of the result. But I have also learned to approach benchmarks with humility. No single dataset captures the full complexity of human communication. No benchmark represents every speaker, language, environment, or use case.


What matters is whether a result changes our understanding of what can be built—and whether that progress reaches the people who need it.


## Accessibility defines quality


Accessibility is sometimes treated as a specialized feature or a separate track of development. I see it differently. *Accessibility is not a feature added after quality. It is how quality is proven.*


A speech-recognition system that performs well only for the statistical average is not yet a truly robust system. When a model can recognize a wider range of voices, speaking patterns, environments, and conditions, it becomes better for everyone.


The work required to serve people who have historically been hardest for technology to understand often strengthens the underlying system itself. It forces us to build models that generalize more effectively, respond more reliably to real-world variation, and remain useful outside carefully controlled settings. That is not only good accessibility engineering. It is good AI engineering.


## Production makes research accountable


One lesson has remained constant: speech recognition is not won in the laboratory alone. Real conversations are spontaneous. People interrupt one another. They change topics, speak softly, use unfamiliar names, mix languages, sit in noisy rooms, and communicate with voices that may change over time. This is why production matters.


At Zoom, the same core speech technology that powers our own products is also made available to developers through Zoom AI Services. We describe this principle as first party equals third party— **1P equals 3P** .


The model delivered through the Scribe API is not a benchmark-specific research system. It is the production model used to support experiences across Zoom, including meeting transcription, conversation summaries, virtual agents, and developer applications built with the Scribe API. The model we trust in our own products is the model we make available to the broader ecosystem. For us, 1P equals 3P is more than a product strategy. It is an engineering discipline.


Our products continuously test the technology against the complexity of real conversations. Improvements made through that experience can benefit customers and developers alike. Research strengthens production, and production gives research a more honest understanding of the problems that still need to be solved. What makes this result genuinely surprising is that a model built for everyday production use—designed to handle the full complexity of real conversations—also performed exceptionally on one of the field's hardest accessibility benchmarks.


## From conversation to completion


Speech recognition was once the destination. Today, it is the foundation.


As voice agents become more capable, voice will increasingly become one of the most natural interfaces between people and AI—and an important interface through which people collaborate with one another.


People do not naturally think in prompts. They think aloud. They ask questions, explain situations, revise their ideas, express uncertainty, and work through problems in conversation.


But people do not begin conversations because they want transcripts. They begin conversations because they want to accomplish something: help a customer, resolve an issue, care for a patient, make a decision, coordinate a team, learn something new, or create something together. That is the transformation we at Zoom describe as *conversation to completion* .


A conversation expresses human intent. Completion is where that intent becomes an outcome. Speech recognition is the first step in that journey. If the system fails to understand what was said, then everything built above it—summarization, reasoning, search, workflow automation, and agentic action—is compromised. The quality of the final outcome depends on the quality of the listening at the beginning.


## A foundation for healthcare AI


Healthcare makes the importance of this work especially clear. Many of the people interacting with healthcare systems are also among those most likely to have speech that differs from the patterns commonly represented in training data: stroke survivors, people living with neurodegenerative conditions, older adults, and patients whose voices may be affected by illness, fatigue, treatment, or injury. For them, being understood is not simply a matter of convenience. It can be a matter of dignity, effective care, and meaningful participation in decisions about their own health.


A misrecognized word in an ordinary conversation may cause frustration. In a healthcare setting, it could contribute to a missed symptom, an inaccurate note, or a person feeling unheard at precisely the moment when being heard matters most. But strong performance on difficult, diverse speech provides an important foundation. It tells us whether the underlying technology is beginning to generalize to the people and situations that have historically been hardest to serve. To be clear: benchmark performance does not establish clinical readiness. That is progress worth pursuing carefully—and with humility.


## A shared achievement


None of this work belongs to one person, one model, or one company.


The Speech Accessibility Project exists because researchers, institutions, industry partners, and participants chose to work together. Most importantly, individuals contributed their voices so that future technology could serve others more effectively.


At Zoom, this result reflects the sustained effort of an extraordinary AI team: researchers, engineers, and product leaders working together across disciplines. I am proud of both the technical achievement and the spirit behind it.


The hardest problems in AI are rarely solved by a single breakthrough. They are solved through years of patient work: better models, better data, better evaluation, stronger infrastructure, careful product decisions, and a willingness to keep learning from the people the technology is meant to serve.


## The person behind every conversation


When I began working in speech recognition, we celebrated each small improvement because every percentage point represented years of effort. I appreciate the numbers. They help us measure progress, compare approaches, and understand where more work is needed. What stays with me most is not the number.


*It is the person behind every conversation.*


A person trying to contribute in a meeting. A customer asking for help. A patient explaining how they feel. A family member trying to be understood. Someone whose voice has too often been treated by technology as an exception. When I began in this field, progress was measured one word at a time. Today, I believe it must also be measured one person at a time.


*Every voice carries intent, experience, and dignity.*


*Every voice deserves to be understood.*


If we remain committed to that principle, we will build more than better speech-recognition systems. We will build AI systems that listen more faithfully, and include and help more people move from conversation to completion.


Because in the end, technology reaches its highest purpose not when it speaks more intelligently, but when it helps every person be heard.


## Everything you need for speech-to-text


[Try Zoom Scribe API](https://www.zoom.com/en/products/ai-services/scribe-api/)
