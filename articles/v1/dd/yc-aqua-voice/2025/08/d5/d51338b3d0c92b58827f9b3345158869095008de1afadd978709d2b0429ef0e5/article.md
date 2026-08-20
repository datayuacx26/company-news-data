---
schema_version: "1.0.0"
document_id: "d51338b3d0c92b58827f9b3345158869095008de1afadd978709d2b0429ef0e5"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/introducing-avalon"
published_at: "2025-08-22T00:00:00+00:00"
first_seen_at: "2026-07-21T07:23:55.387885+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:c0a255f00dfb6d54d782a6d2cafa41ca0a8b8d3552141bc50c1478ff98eba487"
---

# Introducing Avalon

# Introducing Avalon


August 22, 2025


Finnian Brown


Today, we are introducing Avalon, a speech recognition model optimized for human-computer interaction. It has improved transcription performance on all tasks, and significantly improved performance in domains like software and coding.


[Model Card (PDF)](https://app.aquavoice.com/research/avalon-model-card.pdf)


## State-of-the-Art ASR Performance


On the OpenASR suite of benchmarks, Avalon outperforms Whisper Large v3 on 7 of 8 standard test splits and ElevenLabs Scribe on 6 of 8.


WER by dataset across standard public benchmarks. Lower is better.


This was a surprise to us. We decided to build Avalon because we wanted to build a better model for how people actually talk to their computer, the words and phrases like "git checkout dev" were the things we wanted to nail.


It turned out that the data pipelines we stood up for both publicly available audio and for pre-existing audio datasets not only boosted the performance on this important domain, but also made the model better across the board.


**Here are some samples that show Avalon's strengths:**


## The Best Model for Talking to AIs


One of our goals with Avalon was to build the best ASR model for people talking to AIs. To test this, we created a new benchmark called AISpeak, which is an evaluation dataset where the speaker is using AI jargon or domain-specific terms. Think of it as a bunch of clips from Twitch streams and YouTube where the keywords “Claude Code” or “MCP” are used.


Instead of just measuring word error rate against human labels, AISpeak clips also have a key term, and we benchmarked Avalon against other models, counting what percent of clips did the model get the key term correct.


Accuracy on Coding and AI Terms


Higher is better


Set


Avalon


NVIDIA Canary 1B


Voxtral Mini 3B


ElevenLabs Scribe


Whisper Large v3


AISpeak-10


97.4%


51.5%


59.5%


78.8%


65.1%


AISpeak-50


97.5%


71.8%


79.4%


86.7%


82.4%


AISpeak-500


95.8%


74.1%


82.9%


87.5%


84.9%


On AISpeak-10, a challenging subset of the larger dataset, Avalon transcribed the key term correctly 97.4% of the time, compared to 51.5% from NVIDIA Canary 1B and 65.1% from Whisper Large v3.


Here are some samples that show Avalon's strengths:


## Why we built Avalon


People don't talk like they're reading an audiobook. But a lot of public audio training data is heavy on just that: audiobooks, meetings, court/parliamentary proceedings, and news broadcasts. These areas have decades of human-made transcripts readily available, so it makes sense in a way.


But this means that models can often benchmark incredibly well, but make very silly and obvious mistakes in real-world testing (cough cough, Nvidia Parakeet).


We saw this data distribution and thought “No one is using transcription for this.” What people actually use transcription for is writing; writing AI prompts, writing messages, writing emails, and so on.


That may sound like a distinction without a difference, but it's not. People speak very differently when they are writing a prompt for Claude Code than when they're talking to a colleague, expecting a meeting transcript at the end.


## Data and privacy


We did not use users audio or transcripts for training unless they explicitly opted in.


By default, Aqua processes audio ephemerally to power transcription and enhancement features; opt‑in contributions from users and trainers are used to improve future versions of Avalon. You can review and change your data settings at any time.


For more details, see the model card.


## Availability and what's next


Avalon is available in Aqua starting today for English. We're currently training models for the rest of Aqua's supported languages. We plan to release the multilingual version in the coming weeks.


--Finn
