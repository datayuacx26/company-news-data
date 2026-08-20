---
schema_version: "1.0.0"
document_id: "02e13599cf03d7dc36fb8fbf6324df5e3db00496edda2c9fc5620d53d693b848"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/agora-agents-sdk-build-voice-agents-in-minutes"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:3cf9b90dd78e1f6817711039f092fadd9c996897fc16e195e0fd379cf073b5a0"
---

# Agora Agents SDK: Build Voice Agents in Minutes

*Introducing* ** ***Agora Agents SDK*** *, a typed SDK for building voice agents on Agora's Conversational AI Engine in Python, Node.js, and Go.*


Voice agents have moved past demos. Teams are putting them in support flows, onboarding, scheduling, field operations, and internal tools where a delayed response or missed interruption feels broken.


Today we're releasing **Agora Agents SDK** : typed builders, built-in auth, token generation, and session management on top of the existing REST API. It ships for **Python** , **Node.js / TypeScript** , and **Go** .


The REST API remains fully supported. The SDK sits on top of it.


## What the SDK Handles


You configure the agent in typed code instead of assembling payloads by hand.


- **Typed pipeline builders.** Build voice agents in Python, TypeScript and Go. Compose your voice agent configuration with speech-to-text using` .with_stt()` , the LLM using` .with_llm()` , and text-to-speech using` .with_tts()` . For real-time multimodal models, use the MLLM builder path` .with_mllm()` for providers such as OpenAI Realtime and Gemini Live.
- **Dynamic token generation.** Pass your Agora App ID and App Certificate once. The SDK generates fresh auth tokens for each agent request, so developers don't have to build or manage token logic themselves. When you expose agent management endpoints for web or mobile apps, the SDK can also help configure the token server your clients use to join Agora channels.
- **Session lifecycle.** Start, stop, query, and recover sessions through SDK objects instead of keeping raw agent IDs, status values, and retry logic scattered through your backend.
- **Python, Node.js / TypeScript, and Go support.** The SDKs use the same agent model, with APIs shaped for each language instead of one generic wrapper.


## Install the SDK


Use your preferred package manager to install the SDK:


```text
pip install agora-agents
npm install agora-agents
go get github.com/AgoraIO/agora-agents-go
```


## Quick Start


This is a basic Python voice-agent pipeline: STT -> LLM -> TTS.


```text
import   time


from   agora_agent   import   Agora, Agent, Area
from   agora_agent.utils   import   expires_in_hours
from   agora_agent.agentkit   import   DeepgramSTT, MiniMaxTTS, OpenAI


client = Agora(
area=Area.US,
app_id=APP_ID,
app_certificate=APP_CERTIFICATE,
)


agent = (
Agent(client=client, turn_detection={  "language"  :   "en-US"  })
.with_stt(
DeepgramSTT(
model=  "nova-3"  ,
language=  "en"  ,
)
)
.with_llm(
OpenAI(
model=  "gpt-4o-mini"  ,
system_messages=[{  "role"  :   "system"  ,   "content"  : AGENT_PROMPT}],
greeting_message=GREETING,
failure_message=  "Please wait a moment."  ,
max_history=  50  ,
params={
"max_tokens"  :   1024  ,
"temperature"  :   0.7  ,
"top_p"  :   0.95  ,
},
)
)
.with_tts(
MiniMaxTTS(
model=  "speech_2_6_turbo"  ,
voice_id=  "English_captivating_female1"  ,
)
)
)


session = agent.create_session(
channel=f  "demo-channel-{int(time.time())}"  ,
agent_uid=  "123456"  ,
remote_uids=[  "*"  ],
name=f  "conversation-{int(time.time())}"  ,
idle_timeout=  30  ,
expires_in=expires_in_hours(  1  ),
debug=False,
)


return   session.start()
```


That code starts an agent and joins it to an Agora RTC channel. The builder keeps the agent configuration readable: turn detection, providers, model behavior, channel, session name, timeout, and expiry all stay in one place. The same setup can carry model parameters, history limits, and debug settings when you need them.


## If You Already Use the REST API


Nothing breaks. The REST API remains the foundation of the platform, and it is still a good fit for teams that need direct HTTP control or use a backend language without an SDK.


For Python, Node.js, and Go services, the SDK removes the code most teams don't want to own:


- manual token signing
- large config objects assembled across files
- repeated start/stop boilerplate
- one-off retry and timeout handling around session startup


You can migrate one boundary at a time. Keep the rest of your backend as it is, replace the raw setup and lifecycle calls with the SDK, and compare the code side by side.


## Your Models, Agora's Real-Time Infrastructure


The SDK doesn't lock you into one model stack. Bring your own STT, LLM, and TTS providers, or use Agora-managed models when you want fewer vendor credentials in your app. You keep control of the model choices and prompts that define the agent. Agora runs the real-time media path, interruption behavior, and channel lifecycle users feel the moment they start talking.


We built **Agora Agents SDK** so the first working agent doesn't require a pile of glue code. The SDK keeps the setup close to the way you think about the agent: models, prompts, voice, channel, and session behavior.


Source and examples for Python, Node.js/Typescript and Go are on Docs:[https://docs.agora.io/en/ai/build/start-stop-agent#install-the-sdk](https://docs.agora.io/en/ai/build/start-stop-agent#install-the-sdk)


Build an agent, put it in front of a real conversation, and tell us where the SDK still makes you think about plumbing. That feedback shapes the next version.


‍
