---
schema_version: "1.0.0"
document_id: "df87b9b071730c6e99fe407c045553065ea25aa66c8e666ad67665efb5baf58a"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/introducing-make-app"
published_at: null
first_seen_at: "2026-07-22T08:04:49.593567+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:60e6b4b892c21072faa885858c6476d9ec1e007e95ed036470d2bc26454b1989"
---

# Introducing the AssemblyAI app for Make (Integromat)

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


[Make](https://make.com/?ref=assemblyai.com) (formerly Integromat) is a workflow automation tool that lets you integrate various services together without requiring coding knowledge.


We've been working with Make to make[our state-of-the-art Voice AI](https://www.assemblyai.com/blog/announcing-universal-1-speech-recognition-model/) available to no-code and low-code builders. With the[AssemblyAI app for Make](https://www.assemblyai.com/docs/integrations/make?ref=assemblyai.com) , you can use our AI models to process audio data by transcribing it with speech recognition models, analyzing it with audio intelligence models, and building generative features on top of it with LLMs.
You can supply audio to the AssemblyAI app and connect the output of our models to other services in your Make scenarios.


Each module in the AssemblyAI app has intuitive input parameters and detailed output parameters, so Make users can quickly integrate AssemblyAI into their processes. For example, in the image below, you can see how to configure a list of[PII redaction policies](https://www.assemblyai.com/docs/audio-intelligence/pii-redaction?ref=assemblyai.com#pii-policies) . The user can add multiple policies to the list, and select each policy from a dropdown.


Below you can see a Make scenario that is triggered when a new audio file is uploaded to Google Drive, which uses the AssemblyAI app to transcribe, redact PII, and generate a summary using[LeMUR](https://www.assemblyai.com/docs/lemur?ref=assemblyai.com) , and to finally upload each output back to Google Drive.


Want to learn how to build with AssemblyAI and Make? Visit[our documentation to learn how to get started and which modules are available in the AssemblyAI app for Make](https://www.assemblyai.com/docs/integrations/make?ref=assemblyai.com) , or[follow this tutorial on redacting PII using Make](https://www.assemblyai.com/blog/redact-pii-audio-with-make/) .
