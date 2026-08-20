---
schema_version: "1.0.0"
document_id: "eec98377ab5e9bbf8895b3f292c4bf5731f4b88571623fce16237dd84327224c"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/universal-3-5-pro-async"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:38b2c452649d0dc23fcecf7756996fdfe0af5dad98f1f1b752aa4701dd351bb2"
---

# Universal-3.5 Pro: native code switching, our most accurate speaker diarization yet, and expanded language support

## The most accurate speaker diarization we've ever shipped


Diarization has traditionally been built as an afterthought. A separate system answers "who is speaking when," working completely independently from the ASR system that answers "what is being said." In the conventional approach, these two streams are stitched together by aligning timestamps. The result is brittle: sentences get split at awkward positions, short turn-taking gets lost, and overlapped speech falls apart.


Universal-3.5 Pro solves both problems jointly. The model produces not just the transcript, but also where in that transcript the speaker changes. The result is a speaker-annotated transcript that follows the natural flow of conversation. It captures short turns and rapid back-and-forth that timestamp-merging approaches miss.


On real call center audio, this matters. Agents don't speak in clean blocks. They interrupt each other, confirm details mid-sentence, and change hands multiple times. Every word assigned to the wrong speaker corrupts the downstream analysis. Universal-3.5 Pro captures this complexity with higher accuracy than anything we've shipped, measured on the metric that actually matters: the combination of transcription accuracy and speaker attribution together.


See Universal-3.5 Pro speaker diarization on real audio:


Emergency dispatch call


0:00 / 0:00


Speaker A:


Ambulance emergency, which town or suburb?


Speaker B:


Redfern.


Speaker A:


Okay, tell me exactly what’s happened.


Speaker B:


I’m not— just not feeling so good. Chest pain.


Speaker A:


How old is the patient?


Speaker B:


51.


Speaker A:


Has he ever had a heart attack or angina?


Speaker B:


No.


This also changes how diarization should be measured. The field has historically relied on diarization error rate (DER), but DER compares time regions — who was speaking when — and never looks at the words, so it can rank outputs backwards from what any listener would say. A near-perfect transcript with every word on the right speaker could score a 30%+ DER simply because the system won't label laughter as speech or because ground-truth speech segments are looser than the word timestamps returned by STT models (which is typically the case). cpWER (concatenated minimum-permutation word error rate) instead measures what users actually see ("who said what"): for each speaker, what fraction of their words did the system get wrong or credit to someone else? Universal-3.5 Pro achieves the highest overall cpWER, measured across a wide range of diarization conditions.


### How it compares


cpWER across diarization conditions (lower is better)


expand_more


cpWER


Average


Meetings (ami)


Telephony 1 (callhome_dev_eval)


Telephony 2 (callhome_train)


Far Field (dipco)


Conversational 1 (notsofar-dev)


Conversational 2 (notsofar-test)


Universal-3.5 Pro


30.17


27.36


17.18


17.78


33.48


48.22


37.02


Deepgram Nova-3 English


37.92


29.28


23.70


25.47


38.31


62.55


48.24


ElevenLabs Scribe v2


35.26


36.14


14.82


16.18


48.78


51.29


44.37


Gladia


36.87


34.34


20.06


21.02


41.90


56.12


47.81


Grok STT Batch


42.58


32.10


36.85


33.68


41.60


58.92


52.35


Voxtral Mini Transcribe V2


37.52


34.86


19.50


19.36


38.04


61.42


51.91


Soniox v5 Async


44.58


58.93


16.11


17.45


49.39


67.29


58.36
