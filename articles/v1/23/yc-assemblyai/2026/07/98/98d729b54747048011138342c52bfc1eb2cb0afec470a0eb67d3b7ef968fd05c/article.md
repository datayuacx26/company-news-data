---
schema_version: "1.0.0"
document_id: "98d729b54747048011138342c52bfc1eb2cb0afec470a0eb67d3b7ef968fd05c"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/99-languages"
published_at: null
first_seen_at: "2026-07-24T08:06:00.112884+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:98163cce4ebd0d69fdef1bbafb6087ee395d02e4a6f6129b39790e22e77747b8"
---

# Now Available: 99 Languages, Advanced Features, One Price

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


Building Voice AI for a global audience has always meant compromise. Premium providers charge more per language. Budget options deliver subpar accuracy. Essential features like automatic language detection or speaker diarization? They're either unavailable or locked behind expensive enterprise tiers.


**Today, that changes.**


Universal now supports **99 languages** at the same flat rate of **$0.27/hour** —with automatic language detection for all supported languages, and speaker diarization for 95 of them.


No hidden fees. No degraded performance. Just powerful, production-ready Voice AI that scales globally.


**The improvements are live now for all customers.**


[\[Try it now\]](https://www.assemblyai.com/dashboard/login)


## Real global coverage. Real production quality.


Most providers treat international language support as a luxury. Universal changes that, delivering top-tier accuracy and advanced features for **everyone, everywhere.**


What's new in Universal:


- **99 languages, one price:** $0.27/hour, from English to Hindi to Portuguese
- **53.2% higher accuracy** than comparable solutions
- **Automatic language detection** across all 99 languages
- **Speaker diarization** for 95 languages
- **2–3x faster processing** for high-volume languages (like English, Spanish, German)


## How Universal compares


‍


Provider


Languages


Price/hour


ALD Languages


Speaker Diarization Languages


Universal (AssemblyAI)


99


$0.27


99


95


Deepgram Nova-2


36


$0.31


16


36


OpenAI Whisper


100


$0.36


100


No


Gladia Pro


100


$0.61


100


100


Speechmatics


32


$0.80


32


32


Rev.ai


36


$1.20


No


36


## Feature highlights


### Automatic language detection with smart fallback


Detect the language of any audio file — across all 99 supported languages — with just one API call. Perfect for unpredictable user inputs or global platforms.


**Customize the detectable languages for better accuracy,** if you work with a subset of the 99 languages or heavy-accented audio **.**


```text
language_detection=true
language_detection_options {
expected_languages: [  "en"  ,   "es"  ,   "de"  ,   "it"  ],
}


```


**New fallback logic** ensures a confident result, even when language detection is uncertain. Set fallback_language to "auto" to let our model choose the fallback language from expected_languages with the highest confidence score.


```text
language_detection=true
language_detection_options {
expected_languages: [  "en"  ,   "es"  ,   "de"  ,   "it"  ],
fallback_language:   "auto"
}


```


### Speaker diarization that works globally


Identify who’s speaking — not just what they’re saying — in **95 languages** .
Ideal for multi-person calls, interviews, and meetings where speaker attribution matters.


```text
"speaker_labels"  : true
```


### Fast, consistent performance


Universal delivers **2–3x faster processing** in high-volume languages like English, Spanish, and German — with consistent uptime and quality across all 99.


## The technology behind 99-language support


### 1. Unified architecture


One model that understands all languages, not 99 separate models. This means:


- Improvements to model infrastructure benefit all languages
- Lower latency through shared processing


### 2. Intelligent language detection


Our detection goes beyond basic classification:


- **Expected languages:** Enables language detection with restriction to specified languages
- **Confidence scoring:** Routes to the most likely language
- **Smart fallbacks:** Automatically selects the best match from your expected set


### 3. Infrastructure built for scale


- Optimized pipelines deliver 2-3x faster processing for major languages
- Consistent uptime across all 99 languages


## Built to solve real problems


### AI meeting assistants


Build meeting intelligence that works for every team, everywhere. Whether the meeting is in English, Japanese, or Arabic, deliver the same high-quality summaries, action items, and insights. With speaker diarization working across 95 languages, you'll know exactly who committed to what—regardless of the language spoken.


### Global customer support


Transform international customer service with accurate transcription at scale. Nordic contact centers, Eastern European support teams, and Southeast Asian help desks all get the same production-grade quality. No more choosing between unusable budget transcripts or enterprise pricing.


### Content and media platforms


With 60% of user content in non-English languages, platforms need transcription that works everywhere. Universal enables:


- Automatic captions in 99 languages
- Content creation across all markets
- Search and discovery that breaks language barriers


All at a price that scales with your growth.


## Pricing that makes sense


Stop paying language taxes. Universal delivers premium features at a single, transparent price:


- **$0.27/hour** for any language
- **All features included:** Automatic detection, speaker diarization, no gates
- **Same API:** No integration complexity as you scale


How we compare:


- 25% less than OpenAI Whisper ($0.36/hour)
- 70% less than Google Cloud ($0.96/hour)
- 80% less than AWS Transcribe ($1.44/hour)


## Migration made simple


### For current Nano users


If you've been using Nano for international languages, you know the quality tradeoffs. Universal delivers 53.2% better accuracy at the same price point. Migration is seamless:


1. Change your model parameter from *nano* to *universal*
2. That's it - same API dramatically better results


### For new implementations


Getting started with 99-language support is straightforward:


```text
import   assemblyai   as   aai


aai.settings.api_key =   "<YOUR_API_KEY>"


audio_file =   "https://assembly.ai/wildfires.mp3"


config = aai.TranscriptionConfig(language_detection=  True  )


transcript = aai.Transcriber(config=config).transcribe(audio_file)


print  (transcript.text)
print  (transcript.json_response[  "language_code"  ])
```


## Start building for the world today


The world speaks 99 languages. Now your Voice AI does too.


**Get started in 30 seconds:**


1. **Current Universal users:** You already have access to all 99 languages
2. **New users:**[\[Start with $50 in free credits →\]](https://www.assemblyai.com/dashboard/signup)
3. **Test it now:** Try your audio in our[\[Playground →\]](https://www.assemblyai.com/playground)
