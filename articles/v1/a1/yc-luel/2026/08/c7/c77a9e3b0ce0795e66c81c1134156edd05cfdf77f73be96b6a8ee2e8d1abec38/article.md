---
schema_version: "1.0.0"
document_id: "c77a9e3b0ce0795e66c81c1134156edd05cfdf77f73be96b6a8ee2e8d1abec38"
company_key: "yc-luel"
company: "Luel"
source_id: "yc-luel-news-import-6e1b6cc0a46e"
canonical_url: "https://www.luel.ai/blog/introducing-luel-lab"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-18T01:26:46.028870+00:00"
fetched_at: "2026-08-18T01:26:48.460011+00:00"
content_hash: "sha256:0579521c07c91650580868e22e2847fa3a200801b06973ee5973af66a0629f96"
---

# Introducing Luel Labs

Today we're launching[Luel Labs](https://www.luel.ai/lab) : our end-to-end multimodal data lab, built on top of our globally deployed contributor network and multimodal QA engine.


## The engine underneath


More than 750,000 contributors across 96+ countries complete audio, video and image tasks and are paid per accepted submission. Every submission is consented and quality-assured before anything downstream can use it.


That engine is what makes the Labs possible. Collection and quality assurance built for the platform can be pointed at anything: a native-speaker speech corpus for a language with almost no recorded data, real overlapping conversation instead of studio reads, the checks themselves. Results are then measured on public benchmarks, so they stay comparable to everyone else's.


## From the tips of the Himalayas to across the Sahara


Speech comes first. The platform records every modality, but speech is where the distance between what people speak and what technology serves is widest. Most of the world's languages have almost no recorded, transcribed audio at all, and a contributor network that pays native speakers to create it changes what can be built.


Languages spoken vs. languages served Documented languages (UNESCO)


8,324


Any open research TTS voice


1,107


Frontier commercial TTS


74


UNESCO's Atlas documents 8,324 languages; open research covers 1,107 with a single-speaker voice each; the most multilingual commercial system speaks 74. The sliver is where the industry is; the rest is where our contributors are.


We're delivering the **first commercial-grade TTS for Tibetan and Dzongkha** — shippable, licensed voices for two languages where the major platforms list none at all — and **outperforming existing Masri and Darija models** .


Masri is the Arabic that more than a hundred million people actually speak, not the formal MSA that generic systems fall back to. It code-switches into English and French and reads numbers and dates in dialect-specific ways, and general multilingual voices tend to mispronounce it or slip into a foreign accent. Measured on a held-out set through one pipeline, our voice reads the lowest character error rate of every system on the board.


Egyptian Arabic · character error rate (lower is better) Luel TTS · Masri


0.1633


VoiceTut


0.1706


ElevenLabs v2 · multilingual


0.1707


ElevenLabs v3


0.1757


Azure Neural ar-EG


0.1912


NileTTS


0.1913


Panel-median character error rate on a held-out set, every system scored through the same pipeline. The full board is on the[Egyptian Arabic benchmark](https://www.luel.ai/lab/benchmarks/egyptian-arabic-tts) .


Each voice is built from scratch on its own native-speaker corpus, never a multilingual checkpoint borrowed from the language next door. The[Moroccan Darija](https://www.luel.ai/lab/benchmarks/moroccan-darija-tts) board tells the same story.


## Foundational research on messy, real-world data


The rest of the program applies **48 kHz speaker separation** , **conversational-overlap evaluation** , **STT modeling** , **data integrity** and **speech understanding** to audio the industry usually throws away.


Separation is the clearest example. Hand the model a recording of two people talking over each other and it returns each voice on its own. The harder test is what it does when only one person is talking: the right answer is silence on the second output, and most separators invent a speaker rather than admit there isn't one. On the gain-invariant audit, our base model records a 5.4% false-source rate; the lowest of 22 public checkpoints records 33.5%, and every public checkpoint scores higher than ours. On public corpora, false-source rates fall from 2.1% to 0.0% on LibriSpeech, 23.1% to 1.0% on VCTK, and 0.6% to 0.0% on FLEURS-fr. The model is trained on real multilingual conversation, where speech is mostly one person at a time, rather than synthetic mixtures of two voices in constant collision. Radio, classrooms and oral histories all arrive as overlapping speech, and separation is how they become usable data.


A data-integrity program runs under all of it, pointed at the platform's own checks and measurements: every evaluation preregistered and bound to data reserved before measurement.


## Where this goes next


The work above shares one direction: pick a place where the data is missing, use the platform to create it, and put the result on a public benchmark. The same build-from-scratch principle spans specialist voices, separation for radio, classrooms and oral histories, and quality assurance across audio, video and images.


New voices, corpora, and benchmarked results continue on[Luel Labs](https://www.luel.ai/lab) .
