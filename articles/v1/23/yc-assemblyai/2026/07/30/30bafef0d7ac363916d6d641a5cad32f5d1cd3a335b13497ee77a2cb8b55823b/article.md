---
schema_version: "1.0.0"
document_id: "30bafef0d7ac363916d6d641a5cad32f5d1cd3a335b13497ee77a2cb8b55823b"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/redact-pii-audio-with-make"
published_at: null
first_seen_at: "2026-07-22T08:04:49.593567+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:cfe4ad59d680b80bb3699596f7ab008ff91b8c742ff4253b7433495dfb16c186"
---

# Redact PII in Audio with Make and AssemblyAI

[Make](https://make.com/?ref=assemblyai.com) (formerly Integromat) is a workflow automation tool that lets you integrate various services without requiring coding knowledge. With the[AssemblyAI app for Make](https://www.assemblyai.com/docs/integrations/make?ref=assemblyai.com) you can use our AI models to process audio data by transcribing it with speech recognition models, analyzing it with audio intelligence models, and building generative features on top of it with LLMs.


In this tutorial, you'll create a Make scenario that watches a Google Drive folder for new audio files, and then creates both a transcript and an audio file in which PII is redacted.


**Note**


We're using Google Drive as the source of audio files, but you could pull them from any source including e.g. AWS S3. If the audio files already have public URLs, you don't need to upload them to AssemblyAI.


## Build a PII redaction scenario


[Sign up](https://www.make.com/en/register?ref=assemblyai.com) or[log into your Make account](https://www.make.com/en/login?ref=assemblyai.com) .


Create a new scenario and set the Google Drive` Watch Files in a Folder` as the trigger.
If this is the first time you're creating a Google Drive module, you'll need to create a new connection and select it.
Configure the trigger to watch a specific folder in your Google Drive where you will upload your audio files. Let's assume this folder is named` media` .


Next, add a Google Drive` Download a File` module connected to the trigger, and configure the` File ID` with the` File ID` from the trigger.


Right-click on the connection between the trigger and` Download a File` module, and click on **Set up a filter** . Configure the filter condition so that it is true when the file name does not contain the word "redacted".


Later, when your scenario is fully built, your scenario will upload the redacted transcript and audio back to Google Drive. As a result, those uploaded redacted files will trigger your scenario again, but you don't want to run the scenario for the files that have been redacted already.


Add an AssemblyAI` Upload a File` module, to the` Download a File` module you just defined. The Google Drive file should automatically be selected.
If this is the first time you added an AssemblyAI module, you'll need to create a connection and select it.


Add an AssemblyAI` Transcribe an Audio File` module. Since this module has a lot of parameters, I recommend clicking on the three dots button and then clicking on **Collapse all** .


Pass the` Uploaded File URL` from the previous module to the` Audio URL` parameter.


Configure the PII redaction model by setting the following parameters:


- Set` Redact PII` to` Yes`
- Set` Redact PII Audio` to` Yes`
- Configure at least on PII policy in the` Redact PII Policies` list


Add an AssemblyAI` Get Redacted Audio of a Transcript` and pass the transcript` ID` from the` Transcribe an Audio File` module to the` Transcript ID` parameter.


Create a Google Drive` Create a File from Text` module and configure the parameters:


- Set` New text File Location` to the` media` folder
- Set` File Name` to the` Original Filename` from the Google Drive` Download a File` module, but[replace](https://www.make.com/en/help/functions/string-functions?ref=assemblyai.com) the file extension of the file name with` redacted.txt`
- Set` File Content` to the` Text` property from the AssemblyAI` Transcribe an Audio File` module


**Warning**


The` Transcribe an Audio File` module output has multiple` Text` properties, some of which are nested inside other objects and arrays. Use the` Text` property from the root of the output.


**Warning**


The output you see from the` Transcribe an Audio File` module is sample data if you haven't run the scenario before. Once the scenario has run, it'll use real data.


Add a Google Drive` Upload a File` module and configure the parameters:


- Set` New Folder Location` to` media`
- Select` Map` under the` File` parameter


- Set` File Name` to the` Original Filename` from the Google Drive` Download a File` module, but[replace](https://www.make.com/en/help/functions/string-functions?ref=assemblyai.com) the file extension of the file name with` redacted.mp3`
- Set` Data` to the` redacted_audio_file` property from the` Get Redacted Audio of a Transcript` module.


Your scenario is complete. Save it and let's test it out.


Go to Google Drive and upload an audio file to the` media` folder. Now, switch back to your Make scenario and click the **Run once** button.[Here's a sample audio file of a phone call](https://storage.googleapis.com/aai-web-samples/architecture-call.mp3?ref=assemblyai.com) that you can use.


Once your scenario is finished, you should see the transcript and redacted audio file appear in your Google Drive folder.


## Conclusion


You just learned how to build a Make scenario that redacts PII from audio files from Google Drive. Instead of Google Drive, you could plug in other file services, CRMs, or wherever you store your audio files, as long as there's an app available for your service in Make to download and upload your files.


You can do a lot more with the AssemblyAI app. You can use additional speech recognition features, analyze your audio with audio intelligence models, and build generative features with LeMUR.[Check out the AssemblyAI app for Make documentation](https://www.assemblyai.com/docs/integrations/make?ref=assemblyai.com) to learn more.
