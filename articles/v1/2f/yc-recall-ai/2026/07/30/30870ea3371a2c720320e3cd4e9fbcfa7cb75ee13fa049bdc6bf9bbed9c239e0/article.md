---
schema_version: "1.0.0"
document_id: "30870ea3371a2c720320e3cd4e9fbcfa7cb75ee13fa049bdc6bf9bbed9c239e0"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/code-switching-and-language-identification"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-29T15:02:05.105701+00:00"
fetched_at: "2026-07-29T15:02:07.084738+00:00"
content_hash: "sha256:61a55ad7cc754bbf9f997d53f5d4d79a0424d0471482ca0edba66a0cd2d72b39"
---

# Multilingual transcription: Code-switching and language identification

When developers evaluate[transcription APIs](https://www.recall.ai/product/meeting-transcription-api) for multilingual audio, the same terms tend to show up together: multilingual transcription, automatic language detection, code-switching, language identification, and multilingual speech recognition.


Though these terms are related, they refer to different parts of the transcription problem.


This article separates and explains multilingual transcription, code-switching, and language identification. It then shows you how to evaluate multilingual transcription providers using a[benchmark repo](https://github.com/recallai/codeswitching-benchmark) I created.


## Multilingual transcription


Multilingual transcription usually means one of two things: the API supports many languages, or the audio itself contains many languages. Both definitions fit multilingual transcription, but they are not the same capability.


A provider may transcribe English-only audio and Spanish-only audio well, but still struggle when English and Spanish appear in the same recording. Since most transcription providers support more than one language, language coverage alone is not an especially useful test. For most teams it is more important to evaluate whether a provider can handle one recording with multiple spoken languages and return a transcript that preserves each language as spoken.


Applications that only display transcripts might not need more functionality than simple transcription. Applications that translate, route, search, or analyze transcript content need metadata such as language labels, timestamps, speaker labels, and word-level details. Still other applications may need to track how languages are distributed in a recording.


## Code-switching


Code-switching broadly means languages alternate within a conversation, speaker turn, or utterance.


For transcription systems, the location of the switch matters. Language changes can occur between speakers, between sentences, and inside one sentence.


### Turn-level language alternation


```text
Speaker A: Hello everyone.


Speaker B: Buongiorno.


Speaker A: Let's get started.


```


This recording contains English and Italian, but the language changes at a speaker boundary. The system can use the speaker turn as a natural unit: one turn is English, the next is Italian, and the next is English again.


This is still multilingual audio. It falls under code-switching in the broad conversational sense. For transcription evaluation, it is useful to separate it from same-speaker switching because the language change lines up with a turn boundary.


### Inter-sentential code-switching


```text
Speaker A: Let's publish the repository with this blog on Friday.


Speaker A: I lettori potranno capire come confrontare diversi provider e testarli con i propri dati.


```


In inter-sentential code-switching, the same speaker changes languages between sentences.


The system can no longer rely on speaker changes to serve as the boundary for language changes. Now, it has to handle multiple languages spoken consecutively by the same speaker.. But each sentence is still in a separate language, so sentence-level or segment-level language metadata may be enough for many applications.


### Intra-sentential code-switching


```text
Speaker A: Let's publish the repository with this blog on Friday così i lettori possano capire come confrontare diversi provider e testarli con i propri dati.


```


In intra-sentential code-switching, the language changes inside the sentence.


The system cannot treat the full sentence as one language. It has to recognize the English words, recognize the Italian words, and preserve both in the same sentence.


### Why code-switching is hard for transcription systems


Code-switching removes an assumption many transcription systems rely on: that nearby words are likely to be in the same language.


When that assumption breaks, several errors can appear. For example, a short Italian phrase in mostly English audio might be rewritten as similar-sounding English words, or dropped entirely.


A system can also translate instead of transcribe. If the speaker says:


```text
I'll send it domani.


```


the expected transcript should preserve` domani` unless translation was requested. If the output says:


```text
I'll send it tomorrow.


```


the transcript no longer preserves the original speech.


A system can also lose the switch boundary. The words may be mostly correct, but the API may not show where English ended and Italian began. This typically isn’t an issue if the only consumers of the transcript are human. However, the switch boundaries are necessary for applications that need to translate only part of the transcript, measure language-switching behavior, route text to language-specific processing, or find all sections of the transcript in a given language.


Code-switching can also make speaker attribution harder to interpret. When a speaker change and a language change happen at nearly the same time, it can be difficult to discern whether an error came from speech recognition, language identification, or, when speakers share an audio channel, diarization.


Evaluating code-switched audio therefore requires language-identification metadata. In many cases, developers need a[transcription API](https://www.recall.ai/product/meeting-transcription-api) to return language labels. They also need information on how to apply the labels to the full recording, each segment, or each word – not just transcript text.


## Language identification


Certain use cases require language identification, often called LID, which is the language metadata attached to the transcript. Transcription tells you what was said. Language identification tells you which language the API assigns to that speech. The level of granularity that an API provides when it comes to language identification determines what the transcript can support downstream.


### Recording-level language identification


Some APIs return one language label for the full audio file.


```text
Recording
└── English


```


For this example scenario, an API may assign one language to the entire recording:


```text
{
"text": "Let's publish the repository with this blog on Friday così i lettori possano testarlo.",
"language": "en"
}


```


That can be enough for workflows that only need to tag the recording by language, but does not show that part of the sentence is Italian or identify where in the speech the language changes. To pick the language label, the API selects the dominant language, the first language it detects, or the language it considers most likely for the recording.


### Segment-level language identification


Some APIs attach a language label to each transcript segment.


> It is important to note that while there are generally agreed upon characteristics of a segment, segment boundaries are determined by the provider and may be based on pauses, sentence boundaries, speaker changes, language changes, or the model’s internal processing. A segment is not necessarily a complete sentence or a fixed length. It is a time-bounded section of speech that the API returns as a separate unit.


```text
Recording
├── English segment
├── Italian segment
└── Italian segment


```


An API with segment-level language identification may return a separate language label for each section:


```text
{
"segments": [
{
"text": "Let's publish the repository with this blog on Friday",
"start_ms": 0,
"end_ms": 2150,
"language": "en"
},
{
"text": "così i lettori possano testarlo",
"start_ms": 2150,
"end_ms": 3900,
"language": "it"
}
]
}


```


When an API labels each transcript segment by language, an application can identify sections in a specific language or send them to the appropriate processing workflow.


### Word-level language identification


Finally, some APIs attach a language label to each word.


```text
Recording
├── Let's      English
├── publish    English
├── così       Italian
├── i          Italian
└── lettori    Italian


```


At the word level, the response may attach a language label and timestamp to each word:


```text
{
"words": [
{
"text": "Let's",
"start_ms": 0,
"end_ms": 240,
"language": "en"
},
{
"text": "publish",
"start_ms": 240,
"end_ms": 620,
"language": "en"
},
{
"text": "così",
"start_ms": 2150,
"end_ms": 2440,
"language": "it"
},
{
"text": "i",
"start_ms": 2440,
"end_ms": 2550,
"language": "it"
},
{
"text": "lettori",
"start_ms": 2550,
"end_ms": 2920,
"language": "it"
}
]
}


```


Word-level language identification can show exactly where a speaker changes languages inside a sentence, making it the most useful form of language identification for analyzing intra-sentential code-switching.


However, individual words are often difficult to classify without the surrounding speech. A short word may exist in both languages. A borrowed word may be commonly used in another language. This is especially common in conversations where the subject matter being discussed didn’t exist when the language spoken was created and another language named the terms first. For example, the Italian word for computer is “computer” because computers were not around when Italian was created and the term was popularized in English. A dialect or accent may also sound similar to a related language.


Rapid code-switching gives the system less continuous speech in either language to use as evidence.


For example, a system might label a Spanish word as Portuguese because of the speaker’s pronunciation. It might also label a borrowed English term as English even when the speaker is using it as part of a Spanish sentence.


The API should use the surrounding words and audio to identify each word’s language rather than classifying every word in isolation.


## How transcription systems identify languages


Transcription systems do not all handle language identification the same way. Some identify which language is being spoken before transcription, some during transcription, and the rest identify language based on the output after the fact.


### Language-specific recognition


The simplest approach is to use a speech recognition system built for one language.


```text
Audio
↓
English speech recognition system
↓
English transcript


```


This works when the spoken language is known before transcription starts. If every recording is expected to be English, the application can send the audio directly to an English speech recognition system. However, if the language is unknown or more than one language appears in the same recording, the system needs some other way to decide which language it is hearing.


### Language identification before transcription


Some systems identify the language or languages being spoken before a transcript is even created. They then use that decision to select the speech recognition system that processes each part of the audio.


```text
Audio
↓
Language identification
↓
Selected speech recognition system
↓
Transcript


```


Here, language identification is a routing step. If one part of the audio is identified as Italian, it can be sent to an Italian speech recognition system, or recognizer. If another part is identified as English, it can be sent to an English recognizer.


The system has to decide where one language ends before it has a final transcript. If it splits the audio too early, too late, or at the wrong acoustic boundary, any involved recognizers will receive incomplete context which can, in turn, lead to transcription errors.


This approach is easier for recording-level or segment-level language changes than for rapid intra-sentential code-switching.


### Multilingual speech recognition


A multilingual speech recognition model can recognize more than one language within the same model.


```text
Audio
↓
Multilingual speech recognition model
↓
Transcript


```


Unlike a system that first identifies each language and routes the audio to separate monolingual recognizers, a multilingual model processes all supported languages directly. It may still detect the language being spoken, but transcription does not have to pause while the system selects a different recognizer.


There are two capabilities important to a multilingual speech recognition model: language coverage and language metadata. Language coverage describes which languages the model can transcribe. Language metadata describes whether the API identifies those languages in its response and at what level of detail.


A multilingual speech recognition model may transcribe several languages correctly without telling the application which language appears in each part of the transcript.


For example, an API that has language coverage might return just the transcript text. However, APIs that provide language metadata might also label the recording with one language or attach language labels to each segment or word.


> Note: Language identification can also happen after transcription. A pipeline may run a text-based language identifier on the completed transcript to label the full recording, individual segments, or words. This works best when the transcript is already accurate. If the speech recognizer mistranscribes, translates, or omits a phrase, the language identifier cannot recover what was actually spoken.


## Benchmarking multilingual transcription and code-switching


General speech-to-text benchmarks show how a model performs on a published test set. They do not necessarily show how a provider will handle the languages, switching patterns, speakers, and recording conditions in your product. Many also do not let developers test their own audio and ground truth.


I built this[open-source benchmark repository](https://github.com/recallai/codeswitching-benchmark) for that purpose. It runs the same multilingual and code-switched recordings through one or more transcription providers, compares the results with labeled ground truth, and generates a consistent report for each provider.


> Recall.ai supports many providers and multilingual transcription in both real-time and async workflows. This benchmark repo focuses on a narrower problem: evaluating saved audio files and ground-truth JSON after the fact.


The repository evaluates prerecorded audio files. It does not test live audio capture or streaming transcription. If those use cases are important to you, you should add that as an extension to the repository.


### What the benchmark measures


The benchmark reports transcript accuracy, language-identification accuracy, language-switch detection, speaker and diarization status, request latency, and the metadata fields returned by each API.


Word Error Rate, or WER, and Character Error Rate, or CER, measure how closely the returned text matches the reference transcript.


Those metrics do not show whether the API returned language labels, provided enough detail to locate language changes, included timestamps, or returned speaker labels. The benchmark records those capabilities separately.


> The benchmark does not fill in metadata that a provider failed to return. If an API does not provide language labels, the benchmark does not infer them from the transcript.


### How to run the benchmark


To run the benchmark:


#### Install the dependencies


```text
npm install


```


Copy the example environment file:


```text
cp .env.example .env


```


Open` .env` and add the credentials for each provider you want to test.


#### Create the manifest


The manifest tells the benchmark which recordings to run and where to find them.


Each item needs an ID, an audio path, and the languages expected in the recording. You can also include the type of code-switching, recording conditions, or other notes.


```text
[
{
"id": "english-spanish-example-001",
"audioPath": "./data/audio/english-spanish-example-001.wav",
"languagePair": ["en", "es"],
"scenario": "intra-sentential-code-switching",
"notes": "English and Spanish appear within one utterance."
}
]


```


#### Create the ground truth


Create one ground-truth JSON file for each recording in` data/ground-truth` .


At minimum, the file should contain the correct transcript. Include language labels, timestamps, and speaker labels when they are relevant to what you want to evaluate.


```text
{
"audioId": "english-spanish-example-001",
"segments": [
{
"speaker": "speaker_1",
"startMs": 0,
"endMs": 2400,
"text": "Let's review the roadmap porque necesitamos terminarlo esta semana.",
"words": [
{
"text": "Let's",
"language": "en",
"startMs": 0,
"endMs": 300
},
{
"text": "review",
"language": "en",
"startMs": 300,
"endMs": 700
},
{
"text": "the",
"language": "en",
"startMs": 700,
"endMs": 850
},
{
"text": "roadmap",
"language": "en",
"startMs": 850,
"endMs": 1200
},
{
"text": "porque",
"language": "es",
"startMs": 1200,
"endMs": 1500
}
]
}
]
}


```


The ground truth determines what the benchmark can evaluate. Reference text is required for WER and CER. Language labels are required for language-identification scoring, while word- or segment-level labels are needed to evaluate language-switch boundaries. Timestamps connect words and segments to the corresponding points in the audio. Speaker labels are needed to review diarization and map provider speaker IDs to reference speakers.


#### Run the providers


Run one configured provider:


```text
npm run benchmark -- --provider openai --manifest ./data/manifest.json


```


Or run all configured providers:


```text
npm run benchmark -- --provider all --manifest ./data/manifest.json


```


Score the provider outputs against the ground truth:


```text
npm run score -- --results ./results --groundTruth ./data/ground-truth


```


Then generate the report:


```text
npm run report -- --results ./results


```


### How to read the results


The report separates what each API returned from whether the available output matched the ground truth.


#### API output capabilities


A simplified capability table might look like this:


Provider Language tags Switch points scoreable Timestamps Word timestamps Segment timestamps Speaker labels


Provider A Word-level Yes Yes Yes Yes No


Provider B Recording-level No Yes No Yes Yes


“Switch points scoreable” does not mean that the API returned a separate switch-point field. The benchmark can locate a switch by comparing consecutive language labels. For example, a change from` en` to` it` between two words creates a scoreable switch point.


A recording-level label is not enough to score a switch inside a sentence because it does not show where the language changed.


#### Accuracy scores


A simplified scored result might look like this:


```text
{
"sample_id": "meeting_001",
"provider": "example_provider",
"transcription": {
"wer": 0.08,
"cer": 0.03
},
"language_id": {
"status": "scored",
"granularity": "word"
},
"switch_detection": {
"status": "scored"
},
"diarization": {
"status": "requires_speaker_mapping"
},
"latency": {
"request_ms": 1840
}
}


```


In this example, a WER of` 0.08` means that approximately 8 percent of the reference words were substituted, inserted, or deleted. A CER of` 0.03` means that approximately 3 percent of the reference characters differed.


This result also shows that word-level language identification and language-switch detection were available and could be scored. Speaker labels were also returned, but the provider’s speaker IDs still need to be matched with the speakers in the ground truth. The transcription request took 1,840 milliseconds.


A status such as` not_scoreable` or` unavailable` does not mean the provider returned incorrect metadata. It means the provider did not return enough information to calculate that metric.


Similarly,` requires_speaker_mapping` means that speaker labels such as` speaker_0` and` speaker_1` exist, but they must be matched with the reference speakers before diarization accuracy can be evaluated.


#### Interpret the metrics together


No single score describes the full result. A provider may have low WER but no language labels, accurate language labels but poor transcription, or correct words with inaccurate switch boundaries. It may also perform well overall while missing the less common language in a recording.


For example, a mostly English recording may receive a low overall WER even if the provider misses the only Spanish sentence. Review accuracy separately for each language and pay particular attention to the words around each switch.


The raw provider response is also worth inspecting. Check whether the API preserved each language as spoken, translated anything without being asked, dropped short phrases in the less common language, returned labels at the expected level, and placed timestamps around the correct words or segments. Also check whether enabling language detection changes the structure of the response.


**Record the provider configuration**


Benchmark results depend on more than the provider name. Record the model and model version, language hints or allowed-language lists, diarization and timestamp settings, audio preprocessing, input track type, region, and run date.


Without this information, the result may not be reproducible. A difference between two runs may come from a model update, request setting, region, or preprocessing change rather than from the provider itself.


### What to test with the benchmark


Though I can’t tell you exactly what you will need to test, here are some guidelines to get you started:


1. Build the test set around the audio your application actually receives.
2. Include monolingual recordings in each supported language, language changes between speakers, language changes between sentences, and switches inside a sentence. Also consider short phrases embedded in another language, borrowed words, names, acronyms, product terms, different accents and dialects, background noise, compression, and overlapping speech.
3. Include both mixed and separate per-speaker tracks only if your product receives both. Separate tracks already identify the speaker. A mixed track may require the provider to separate speakers, handle overlapping speech, and detect language changes at the same time.
4. Include simple control cases as well as difficult ones. Monolingual recordings can show whether a provider struggles with one language before code-switching is introduced.
5. Clean studio audio will not provide a representative result if your production audio comes from compressed meeting recordings. Separating English-only and Italian-only recordings, for example, will not show whether a provider can handle switches inside a sentence.
6. Use the level of ground-truth detail your application needs. Segment-level labels may be enough for some workflows, while evaluating a switch inside a sentence requires word-level labels.


## Conclusion


A provider can support many languages and still fail on mixed-language audio, so the most useful comparison tests representative recordings and scores both the transcript and returned metadata against labeled ground truth.


If you have questions about meeting recording, async or[real-time transcription](https://www.recall.ai/blog/realtime-transcription-api) , multilingual workflows, or choosing a transcription provider, feel free to[chat with us](https://www.recall.ai/book-a-demo) about your use case. You can also[sign up](https://www.recall.ai/signup) for a free Recall.ai account and use a unified API to test transcription providers.
