---
schema_version: "1.0.0"
document_id: "ac46c70635dc1aefbb608081078ca964faa41413374c92b515381f4be7a2fbb1"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/updated-zapier-app"
published_at: null
first_seen_at: "2026-07-22T08:04:49.593567+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:2d1498365b8f168f4a3c138e13133e84329ded148b1c276bf08798ecb231fa05"
---

# Introducing the enhanced AssemblyAI app for Zapier

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


Customers have been using the AssemblyAI app for Zapier to transcribe audio and extract data from the rich transcript returned from our API, but until now, many of the options and features that our API offers have not been accessible via Zapier.


Today, we are releasing a new version of the AssemblyAI app for Zapier with more options and features.


## Updated transcribe event


The Transcribe event (formerly "Get Transcript") now has all the options available to our transcript API, making the Speech Recognition and Audio Intelligence features available to Zapier customers. For example, you can set a language code or punctuation and casing optionally.


Additionally, by setting the new "Wait until transcript is ready" field to False, you can start the transcription and perform other tasks while waiting for the transcription.


## New events


The new AssemblyAI app for Zapier comes with 5 more events:


- **Get Transcript** : Retrieve a transcript that you have previously created.
- **Get Transcript Subtitles** : Generate STT or VTT subtitles for the transcript.
- **Get Transcript Paragraphs** : Retrieve the transcript segmented into paragraphs.
- **Get Transcript Sentences** : Retrieve the transcript segmented into sentences.
- **Get Transcript Redacted Audio Result** : Retrieve the result of the PII audio redaction model. The result contains the status and the URL to the redacted audio file.


## What about LeMUR?


Unfortunately, the Zapier platform doesn't have the necessary features for us to reliably offer[LeMUR](https://www.assemblyai.com/docs/lemur?ref=assemblyai.com) , our LLM framework for speech understanding. We're looking at different avenues to add support in the future, but we have no timeline for when LeMUR support will be available.


## Next steps


You can try out the new AssemblyAI app for Zapier today. Read our[Zapier integration with AssemblyAI guide](https://www.assemblyai.com/docs/integrations/zapier?ref=assemblyai.com) to get started, or follow this tutorial to learn[how to generate subtitles for your videos using the AssemblyAI app for Zapier](https://www.assemblyai.com/blog/generate-subtitles-with-zapier) .
