---
schema_version: "1.0.0"
document_id: "e9c51edbea729d1e03d212a67ed8a18796c003630d8727483e47749d147a0750"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c3d7277c32d9"
canonical_url: "https://www.assemblyai.com/blog/sync-api"
published_at: null
first_seen_at: "2026-08-19T11:49:15.223909+00:00"
fetched_at: "2026-08-19T11:49:16.837282+00:00"
content_hash: "sha256:e89e90e53c5b4b8337bfac8586f7b123b9b62cdef88db4c0012f1d10c70b42be"
---

# Introducing the Sync API: transcripts in a single call

Some audio is inherently short. A voice agent capturing a spoken command after turn detection completes. A user voice-typing a sentence and expecting words on screen as they speak. An IVR call where you capture one utterance and branch the flow. A push-to-talk message.


For these workloads, you don't need a job queue or a WebSocket connection open for two minutes. You need one HTTP call, done.


Until now, transcribing a short clip on AssemblyAI meant choosing between two surfaces that weren't built for it. The Async API made you submit a job and poll, adding 5–6 seconds of latency to audio that takes 2 seconds to play. The Realtime API worked, but standing up a WebSocket for a one-shot request adds integration overhead that doesn't match the problem.


Today we're launching the Sync API. Send a short audio clip in one HTTP request. Get a finished Universal-3.5 Pro transcript back in the same response in ~134 ms. No polling, no WebSocket, no chunking, no job to manage. One call.


## One decision, three paths


Every transcription workload answers one question: do you need the result fast, soon, or eventually?


Path Transport Pattern Best for


Realtime


WebSocket Live, ongoing session Live captions, real-time voice agents


New


Sync


HTTP POST One request → one response Short clips: dictation, voice agents with turn detection


Async


HTTP POST + poll Submit → job → poll Long audio, batch, latency-tolerant


If you've been routing short clips through async polling or a WebSocket because nothing else fit, the Sync API closes that gap.


## Flagship accuracy at sub-300 ms


The Sync API runs on Universal-3.5 Pro, the same model behind our async and realtime products. It's the model with the lowest word error rate on real-world audio, built to handle audio the way it actually happens.


The response comes back in ~134 ms at p50. There's no chunking: the full clip is decoded to a single 16 kHz audio array and passed to the model in one shot. The model attends to every frame at once, which is what makes sub-300 ms responses without quality loss possible.


Time to transcript


### 2-second clip, request to finished transcript


Lower is better


~134
ms


5–6
s


Sync API


Async submit-and-poll


That matters because a single misheard word becomes a wrong intent, a failed action, or a frustrated user. You don't trade accuracy for speed here.


## Anywhere the audio is short and the answer is needed now


The pattern repeats across products that don't look alike: the audio is already complete, and the user is waiting on what it says.


Dictation is the clearest case, and the one the Sync API was built to unlock. A user speaks a short burst and expects words on screen instantly: speak, see it typed. At ~134 ms, the round trip fits inside the natural pause after speaking, so the words feel like they were always there. That loop wasn't buildable on our other surfaces. Async turns a two-second clip into a five-second wait, and a streaming session is heavy machinery for one sentence.


It's also where Sync breaks the on-device tradeoff. Local models have to stay small to stay fast, and accuracy pays for it. The Sync API delivers flagship accuracy at near-local speed, with a 1.59% word error rate on short-form audio and no model to bundle, update, or manage. If you're building a dictation app, this is the API to build it on.


The same shape shows up anywhere audio arrives as a finished clip:


- **Voice agents:** your stack already owns turn detection, so the moment the turn ends you have one complete utterance. Send it to Sync, get the transcript back in a single call, and let your LLM respond, with no WebSocket to keep alive and no session state.
- **IVR & call routing:** transcribe one utterance, branch the flow, move on. No submit-and-poll pause the caller hears as dead air.
- **Push-to-talk & voice commands:** mobile and desktop captures that resolve in a single request the moment the user releases the button.
- **Voicemail & short clips:** quick transcripts for files under 2 minutes, without spinning up a job and polling for it.


## How it works


Send the audio itself in one request. Read the transcript off the response. No URL, no upload-then-poll step.


```text
import requests


resp = requests.post(
"https://sync.assemblyai.com/transcribe",
headers={
"Authorization": "YOUR_API_KEY",
"X-AAI-Model": "universal-3-5-pro",
},
files={"audio": ("call.wav", open("call.wav", "rb"), "audio/wav")},
)


result = resp.json()
print(result["text"], result["confidence"])
```


The transcript, word-level timing, and confidence come back in the response:


```text
{
"text": "Book me a table for two at seven.",
"words": [
{ "text": "Book", "start": 120, "end": 360, "confidence": 0.99 }
],
"confidence": 0.97,
"audio_duration_ms": 2140,
"session_id": "b3a1c8e0-..."
}
```


That's the whole integration. One POST with your existing API key. No new auth, no new concepts.


### Dropping it into an agent's turn loop


In a voice agent, the Sync call is one function: your turn detector hands you a finished utterance, and the transcript comes back in time to prompt the LLM. And because your agent already has the conversation, you can pass the previous turns in` conversation_context` . The model still transcribes only the current turn, but it hears the reply the way a person would: as the next line in an exchange, not a clip out of nowhere.


```text
import json


def on_turn_end(utterance_wav: bytes, previous_turns: list[str]) -> str:
resp = requests.post(
"https://sync.assemblyai.com/transcribe",
headers={
"Authorization": "YOUR_API_KEY",
"X-AAI-Model": "universal-3-5-pro",
},
files={
"audio": ("turn.wav", utterance_wav, "audio/wav"),
"config": (None, json.dumps(
{"conversation_context": previous_turns}
), "application/json"),
},
)
return resp.json()["text"]
```


Because each turn is its own request, there's no connection state to manage between turns: no reconnect logic, no keepalives, no session affinity. Any worker can take any turn.


## Key details


Request model One HTTP POST with the transcript in the same response; no polling, no socket, no job lifecycle


Endpoint POST https://sync.assemblyai.com/transcribe


Clip length From 80 ms up to 2 minutes of audio per request


File size Up to 40 MB


Input WAV or raw PCM as a local file, file object, or bytes


Sample rate 16 kHz default, auto-resampled with high-quality SoXR


Languages 18, the same coverage as Universal-3.5 Pro; defaults to English, prompt steering for others


Response Transcript text, per-word timing and confidence, overall transcript confidence


Context & steering conversation_context for prior turns, custom prompts, word_boost keyterms


Auth Your existing API key


Price $0.45/hr


Here's what helps: the Sync API won't let you wait through a doomed request. Since latency tracks with file size, it catches oversized inputs upfront and nudges you toward Async. For raw PCM, just specify sample rate and channels—the rest is automatic.


## Pricing


$0.45/hr—same rate as Universal-3.5 Pro Realtime. We price for latency, which means you pay based on what you need, not artificial limits. Scale without rate limits or commitments, and volume discounts kick in at any tier.


## How it compares


Sync transcription is a category where developers have reached for Whisper-quality models because that's what's available as a single call. The Sync API changes the tradeoff: you get flagship-accuracy Universal-3.5 Pro in one HTTP request, faster than 300 ms. Not a smaller, faster, less accurate model.


Sync API benchmark


### Normalized word error rate by audio type


Lower is better


1.59%


5.30%


4.97%


4.01%


5.53%


3.33%


Short-form


Accented speech


Proper nouns


Alphanumerics


Email addresses


Postal addresses


Compared to on-device embedded models, you get significantly better accuracy and nearly the same speed. No model to bundle or manage. Compared to full realtime, you skip the WebSocket and the connection overhead when you already know your utterance is complete.


## Get started


The Sync API is available now. Use your existing API key: POST your clip to` https://sync.assemblyai.com/transcribe` and read the transcript off the response. And when you're ready to go further, the same request takes` conversation_context` for multi-turn accuracy, custom prompts to control formatting and language, and` word_boost` for the terms you can't afford to miss.


Test it in the[Playground](https://www.assemblyai.com/dashboard/playground/sync) with your own audio, or read the[Sync API guide](https://www.assemblyai.com/docs/sync-stt/getting-started/transcribe-a-short-audio-file) to get started.
