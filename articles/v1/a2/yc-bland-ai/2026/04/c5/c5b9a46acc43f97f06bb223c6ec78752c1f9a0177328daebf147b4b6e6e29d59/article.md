---
schema_version: "1.0.0"
document_id: "c5b9a46acc43f97f06bb223c6ec78752c1f9a0177328daebf147b4b6e6e29d59"
company_key: "yc-bland-ai"
company: "Bland AI"
source_id: "yc-bland-ai-news-import-c77b84ac3c61"
canonical_url: "https://www.bland.ai/blog/fluent-next-generation-multilingual-transcription-voice-agents"
published_at: "2026-04-09T21:00:00+00:00"
first_seen_at: "2026-07-21T10:41:07.173500+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:981ee7ea82f4ae7bd2875cc1d25a5a812f0e8d4350c3bb2a633a4ce9d785d2f7"
---

# Introducing Fluent: Next-Generation Multilingual Transcription for Voice Agents

[Back to blog](https://www.bland.ai/blog)


# Introducing Fluent: Next-Generation Multilingual Transcription for Voice Agents


5.9% WER outperforms leading real-time voice AI transcription provider


[Spencer Small](https://www.bland.ai/blog/author/spencer-small) April 9, 2026


Updated June 16, 2026


4 min read


We're rolling out **Fluent** , a new multilingual speech-to-text model now available on the Bland platform. Fluent represents our latest investment in transcription infrastructure and is purpose-built for the demands of real-time, two-way voice conversations.


If you're running multilingual voice agents on Bland today using our existing Babel or Auto languages, Fluent is worth evaluating. Here's what's different and why we built it.


## What Fluent Does Better#


### Significantly More Accurate in English#


Our internal benchmarks across 250+ hours of real-world audio (call centers, sales conversations, noisy environments, accented speakers) show Fluent achieving a word error rate (WER) of ~5.9% in English. For comparison, the leading real-time voice AI transcription provider sits at ~8.1% WER on the same evaluation set. That's roughly a 27% reduction in transcription errors against the strongest external competitor in the voice AI space.


Fluent also outperforms widely-used baselines like OpenAI's Whisper (~6.5% WER) on the same benchmarks.


For voice agents, fewer transcription errors mean fewer misunderstood requests, fewer awkward clarifications, and more conversations that resolve on the first pass.


### Faster, More Accurate End-of-Speech Detection#


This is the biggest improvement for anyone building conversational agents.


Fluent uses a more sophisticated voice activity detection (VAD) system that is substantially better at distinguishing between a user pausing mid-thought and a user finishing their turn. In practice, this means:


- **Less interjection.** Your agent stops cutting people off mid-sentence. Fluent's endpointing is more patient with natural speech pauses, the kind that happen when someone is thinking about a date, looking up an account number, or switching between languages.
- **Lower effective latency.** Paradoxically, better endpointing *reduces* latency. When the model is more confident that the user is done speaking, it can finalize the transcript faster, eliminating the hesitation buffer that conservative endpointing requires. The result is tighter turn-taking that feels more natural.
- **Fewer false starts.** The agent doesn't begin generating a response to half a sentence, only to get interrupted and have to restart.


This is the kind of improvement that doesn't show up in WER benchmarks but has an outsized impact on conversation quality. We've tuned the VAD thresholds specifically for phone-quality audio with background noise, and the results have been noticeable across our internal testing.


### Native Code-Switching#


Fluent handles intra-utterance language switching. A speaker can start a sentence in English and finish it in Spanish, and the model transcribes both correctly without requiring a language hint or a separate model. This is a meaningful upgrade for serving bilingual populations, which is common in customer service, healthcare, and financial services use cases.


## Supported Languages#


Fluent currently supports **six languages** with high-accuracy, real-time transcription: English, Spanish, German, French, Portuguese, and Italian. These are the six most common languages requested by our enterprise customers for real-time voice agent deployments. Fluent also supports automatic language detection, so you don't need to specify the language upfront.


## A Narrower, Sharper Tool#


There's a deliberate philosophy behind Fluent. Rather than trying to cover every language at the expense of accuracy, Fluent focuses on fewer languages and delivers the best possible transcription quality within that set. It is, by design, the most accurate multilingual model we offer.


That said, we know language coverage matters. That's why Auto and Babel aren't going anywhere.


**Auto** continues to support 10 languages (English, Spanish, French, German, Portuguese, Italian, Hindi, Russian, Japanese, and Dutch) and remains a strong option for teams that need broader coverage with solid accuracy.


**Babel** remains our widest-reaching model, supporting roughly 99 languages. For customers operating in markets that require less common or low-resource languages, Babel is purpose-built for you, and we're proud of the breadth it provides. Most transcription providers simply don't serve these languages at all.


The way we see it: Fluent is for precision. Auto is for range. Babel is for reach. Pick the one that matches your deployment, and know that all three are fully supported and actively maintained.


For most customers running agents in English, Spanish, or Western European languages, Fluent should be your new default. You'll notice the difference immediately in conversation quality.


## How to Use It#


Set your agent's language to fluent in the API:


json


```text
{
"language": "fluent"
}
```


‍


Or select it in the dashboard when configuring a Persona or dispatching a call:


Fluent includes built-in redundancy. If the primary transcription path encounters any issues, your agent will seamlessly fall back to Auto with no interruption to the call.


## What's Next#


We're continuing to invest in transcription accuracy and latency across the board. Our near-term focus is on improving our single-language offerings, starting with English, where we see the highest volume and the most opportunity to push accuracy even further. Expect updates on that front soon.


Transcription is the foundation of every voice agent interaction. If the model mishears your customer, everything downstream suffers. Fluent is the latest step in our ongoing commitment to running the most accurate, lowest-latency transcription infrastructure available for real-time voice AI.


‍
