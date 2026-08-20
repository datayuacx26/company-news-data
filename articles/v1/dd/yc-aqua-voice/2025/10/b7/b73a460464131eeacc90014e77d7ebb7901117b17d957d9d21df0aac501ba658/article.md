---
schema_version: "1.0.0"
document_id: "b73a460464131eeacc90014e77d7ebb7901117b17d957d9d21df0aac501ba658"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/avalon-openasr-leaderboard"
published_at: "2025-10-08T00:00:00+00:00"
first_seen_at: "2026-07-21T07:23:55.387885+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:4a395d630533e4a4dd4a75483122a3e84bd31d2ec7e7187e6a5b5da445e0057c"
---

# Avalon lands at #6 on OpenASR Leaderboard

OpenASR ran their speech recognition benchmarks against Avalon, and it is now the #1 commercial model in the world, surpassing OpenAI Whisper-large-v3, ElevenLabs Scribe v1, and Rev AI Fusion.


Avalon was the best performing proprietary model, with a WER average of **6.24** .🔥


Avalon also outperformed open models like NVIDIA Canary 1B, CrisperWhisper, Voxtral Mini 3B, and Distil Whisper.


You can explore the[OpenASR Leaderboard on Hugging Face](https://huggingface.co/spaces/hf-audio/open_asr_leaderboard) to see the full breakdown.


## Availability


Avalon is available on the Aqua Voice app for English and via our[API](https://aquavoice.com/avalon-api) . We're making the API available for free until October 30th.


## About Avalon


Avalon is a transcription model optimized for human-computer interaction. Our goal was to fix the annoying mistakes that are common in other ASR systems when used in human-talking-to-computer interactions.


When we were building Avalon, our goal wasn't to reduce overall word error rate, but to get better at programming and coding terms and company names that are often mistranscribed.


We evaluated our performance on these terms in our AISpeak benchmark, which we unpack in[Introducing Avalon](https://aquavoice.com/blog/introducing-avalon) . Compared to a baseline accuracy on these jargon terms of 65% for Whisper Large v3 and 78% for ElevenLabs Scribe v1, a leading commercial model, Avalon achieves an accuracy of 97%.


However, when we evaluated Avalon on the industry standard benchmarks, we were pleasantly surprised to see a significant reduction in overall word error rate.


## Why OpenASR matters


OpenASR is the industry standard benchmark for transcription models. It measures the accuracy in word error rate of models on a variety of public audio datasets compared with human labels. The benchmark suite currently consists of seven datasets spanning different audio domains.


The leaderboard highlights Avalon's balance of technical fluency and broad accuracy:


-


**#1 proprietary model globally.** Avalon beats models like Whisper Large v3, ElevenLabs Scribe, and Rev AI Fusion.


-


**Top-10 performance overall.** Avalon ranks #6 across every model—open source and proprietary.


-


**Practical accuracy gains.** The same optimizations that help Avalon nail jargon also reduce total word errors in standard industry benchmarks.


We're excited to see more teams build with Avalon. If you'd like early access or to chat about integrations,[get in touch with us](https://aquavoice.com/avalon-api) .
