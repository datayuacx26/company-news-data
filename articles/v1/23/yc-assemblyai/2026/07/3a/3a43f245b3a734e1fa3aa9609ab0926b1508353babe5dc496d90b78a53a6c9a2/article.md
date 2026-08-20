---
schema_version: "1.0.0"
document_id: "3a43f245b3a734e1fa3aa9609ab0926b1508353babe5dc496d90b78a53a6c9a2"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/announcing-ruby-sdk"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:c6f868d8c95bd17ce37f56c15a0a67433c20d76eefa22d54ba30c6b58919a280"
---

# Introducing the AssemblyAI Ruby SDK

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


We are thrilled to release[the AssemblyAI Ruby SDK](https://github.com/AssemblyAI/assemblyai-ruby-sdk) , making it easier to use the latest Voice AI models from AssemblyAI with Ruby. Use the SDK to transcribe audio, analyze audio using our audio intelligence models, and apply LLMs to your audio data using LeMUR.


Here are a couple of examples showcasing the Ruby SDK.


## 1. Transcribe an audio file


` require 'assemblyai' client = AssemblyAI::Client.new(api_key: 'YOUR_API_KEY') transcript = client.transcripts.transcribe( audio_url: 'https://storage.googleapis.com/aai-docs-samples/nbc.mp3' ) abort transcript.error if transcript.status == AssemblyAI::Transcripts::TranscriptStatus::ERROR puts transcript.text`


You can also transcribe a local file, as shown here.


` uploaded_file = client.files.upload(file: '/path/to/your/file') transcript = client.transcripts.transcribe( audio_url: uploaded_file.upload_url )`


Learn how to transcribe audio files by following[the step-by-step instructions in our docs](https://www.assemblyai.com/docs/getting-started/transcribe-an-audio-file) .


## 2. Use LeMUR to build LLM apps on voice data


` response = client.lemur.task( transcript_ids: \[transcript.id\], prompt: 'Summarize this transcript.' ) puts response.response`


Learn[how to use LLMs with audio data using LeMUR in our docs](https://www.assemblyai.com/docs/getting-started/apply-llms-to-audio-files) .


## 3. Use audio intelligence models


` transcript = client.transcripts.transcribe( audio_url: 'https://storage.googleapis.com/aai-docs-samples/nbc.mp3', sentiment_analysis: true ) abort transcript.error if transcript.status == AssemblyAI::Transcripts::TranscriptStatus::ERROR transcript.sentiment_analysis_results.each do |result| puts result.text puts result.sentiment puts result.confidence printf("%<start>d - %<end>d\\n", start: result.start, end: result.end_) end`


Learn more[about our audio intelligence models in our docs](https://www.assemblyai.com/docs/audio-intelligence) .


## Get started with the Ruby SDK


You can find[installation instructions](https://github.com/AssemblyAI/assemblyai-ruby-sdk/tree/main?tab=readme-ov-file#quickstart) and more information in the[README of the Ruby SDK GitHub repository](https://github.com/AssemblyAI/assemblyai-ruby-sdk) .[File an issue](https://github.com/AssemblyAI/assemblyai-ruby-sdk/issues) or contact us with any feedback.
