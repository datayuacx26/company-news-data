---
schema_version: "1.0.0"
document_id: "58d77b840d0947a295b3338b43d6b014a3adac375a21913428c096308b04a96b"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-build-a-voice-ai-agent-livekit-pipecat-ten-or-native/"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:d3cd7fb8eb609edd88c395c55b6eb6063b278a3b7df39e7a601f5292b2fc2d5a"
---

# How to Build a Voice AI Agent: LiveKit, Pipecat, Ten, or Native (2026)

Building a voice AI agent that feels human takes more than stringing together a few API calls. Voice agent phone calls cost between $0.30 to $0.50, which[cuts contact-center costs by 60 to 70% on routine workflows](https://blog.naitive.cloud/voice-ai-agents-cutting-customer-service-costs/) . That math is why every engineering team shipping conversational software in 2026 is evaluating the same question: do you reach for a voice agent framework, or wire the pieces yourself?


This guide walks through the four production paths developers actually choose today (LiveKit, Pipecat, Ten Framework, and a native no-framework build) with code samples, deployment options, and a decision matrix. Each section links to a working reference implementation in[Plivo's open-source python-agents-examples](https://github.com/plivo/python-agents-examples) repo so you can clone, run, and adapt rather than start from a blank file.


## Prerequisites for Voice AI Agent Development


Before writing code, line up the infrastructure. Missing any piece below will block deployment later.


**Development environment**


Install Python 3.12+ (Python 3.10 minimum). Three of the four approaches in this guide are Python-first; LiveKit also supports Node.js. Install[uv](https://docs.astral.sh/uv/) as the package manager, Git for version control, and Docker for packaging. You will need[ngrok](https://ngrok.com/) for local webhook testing.


**Telephony foundation**


Sign up for[Plivo's AI Agents platform](https://www.plivo.com/ai/) to access the SIP trunking, voice APIs, and bidirectional audio streaming WebSocket that bridges your agent code to the phone network. Without enterprise-grade voice infra, your agent stays trapped in a browser and cannot handle real customer calls.


**Cloud provider setup**


Pick AWS, GCP, or Azure for hosting. Configure IAM, networking, and storage. Right-size compute based on your STT, TTS, and LLM mix, since most providers stream audio and the agent server itself is mostly an orchestrator.


**Core voice agent concepts**


A few primitives matter more than any specific framework choice:


-


**Full-duplex audio streams** : simultaneous send and receive over a single WebSocket so the agent can listen while it speaks.


-


**Turn detection** : deciding when the user has finished a thought. Modern systems use a mix of VAD timing, semantic end-of-turn classifiers, and silence thresholds. Pure VAD-only turn detection is the single biggest source of interruptions feeling unnatural.


-


**Interruption handling (barge-in)** : when the user starts speaking while the agent is talking, the TTS stream stops, the LLM response is cancelled, and STT picks up the new user audio. Doing this in under 200 ms is what makes the agent feel like a person.


-


**Noise cancellation** : removing background office, traffic, and call-center hum before STT runs. Plivo handles this at the carrier edge for inbound and outbound calls; some frameworks also expose noise suppression as a pipeline node.


-


**Backchanneling** : small acknowledgements like "mhm" or "got it" that the agent emits while the user is still speaking. Optional, but it raises perceived warmth significantly.


The 800 ms end-to-end latency threshold is where conversations stop feeling robotic. According to[the 2026 audio AI state of the union](https://tutorialsdojo.com/the-state-of-audio-ai-in-2026-open-source-models-and-the-shift-to-edge-computing/) , the audio stack is now faster, cheaper, and more fragmented than any other AI modality, which is exactly why the framework choice matters less than the pipeline design.


## Overview of Voice AI Agent Approaches


Four production-grade paths dominate in 2026. Each makes a different bet about where complexity should live.


**LiveKit: real-time WebRTC orchestration**


LiveKit started as an open-source WebRTC server and grew an agents SDK on top. It excels at browser-first voice and video, handles room management automatically, and exposes a clean Python and Node.js API for STT, LLM, and TTS plug-ins. The strength is multimodal: when you need a voice agent that can also share video, screen share, or run in a meeting alongside humans, LiveKit is the most natural fit.


**Pipecat: modular voice pipelines in Python**


Pipecat models the agent as a left-to-right pipeline of processors. Each frame flows through VAD, STT, LLM, and TTS in sequence. The mental model is the cleanest in the space, and the integration library (Deepgram, AssemblyAI, OpenAI, Gemini, Cartesia, ElevenLabs, and dozens more) is the largest. Plivo's[gemini2.5-live-pipecat](https://github.com/plivo/python-agents-examples/tree/main/gemini2.5-live-pipecat) and[gpt4o-deepgramnova3-openaitts4o-pipecat](https://github.com/plivo/python-agents-examples/tree/main/gpt4o-deepgramnova3-openaitts4o-pipecat) examples are good starting points.


**Ten Framework: C++ core with Python bindings**


Ten Framework (previously RTC Agent) targets latency-critical workloads. Audio processing runs in C++ outside the Python GIL, which lifts the performance ceiling above pure-Python frameworks. Configuration is a YAML graph of nodes (STT, LLM, TTS, custom processors). The trade-off is steeper setup and a smaller ecosystem.


**Native (no framework)**


You can also build the whole thing yourself: a FastAPI server, webSocket handlers for Plivo's audio stream, STT, LLM, and TTS providers. This gives you the smallest possible runtime, full control over interruption logic, and zero framework abstractions in the hot path. Plivo's python-agents-examples repo includes 16+ native references covering S2S pipelines (like Gemini Live, GPT Realtime, Grok Voice), and STT+LLM+TTS pipelines.[gpt5.4-deepgramnova3-groktts3-native](https://github.com/plivo/python-agents-examples/tree/main/gpt5.4-deepgramnova3-groktts3-native) and[gemini2.5-live-native](https://github.com/plivo/python-agents-examples/tree/main/gemini2.5-live-native) are the canonical references.


### Comparison table


Dimension


LiveKit


Pipecat


Ten


Native


Primary language


Python, Node.js


Python


C++ with Python bindings


Python


Mental model


WebRTC rooms with agent participants


Pipeline of processors


YAML node graph


You write the loop


Best fit


Multimodal voice + video + chat


Phone-first agents with provider mixing


Sub-300 ms latency, edge hardware


Minimum dependencies, max control


Plivo integration


SIP bridge to LiveKit room


Plivo bidirectional WebSocket transport


SIP bridge


Plivo bidirectional WebSocket directly


Reference examples


[Plivo-LiveKit Integration Guide](https://www.plivo.com/docs/voice-agents/sip-trunking/integration-guides/livekit)


[gemini2.5-live-pipecat](https://github.com/plivo/python-agents-examples/tree/main/gemini2.5-live-pipecat)


[Plivo-Ten Integration Guide](https://github.com/TEN-framework/ten-framework/tree/main/ai_agents/agents/examples/voice-assistant-sip-plivo)


[gpt5.4-deepgramnova3-groktts3-native](https://github.com/plivo/python-agents-examples/tree/main/gpt5.4-deepgramnova3-groktts3-native)


Deployment options


-


LiveKit Cloud


-


Plivo Voice AI Infra


-


Self-host


-


Pipecat Cloud


-


Plivo AI Voice Infra


-


Self-host


-


Self-host


-


Plivo Voice AI Infra


-


Self-host


Learning curve


Medium


Low


High


Low to medium


> **Key Insight** : the framework you pick rarely changes the maximum latency you can hit. Network and model latency dominate end-to-end timing. Pick the abstraction your team can debug at 3am, then optimize the pipeline that runs underneath it.


## Setting Up LiveKit Voice Agents


LiveKit's agents SDK handles the orchestration layer that connects audio streams to AI processing.


**Installation and configuration**


```text
# Python
pip install livekit-agents livekit-plugins-openai livekit-plugins-deepgram livekit-plugins-cartesia


# Node.js
npm install @livekit/agents @livekit/agents-server
```


Set` LIVEKIT_URL` ,` LIVEKIT_API_KEY` , and` LIVEKIT_API_SECRET` in your environment. Provision a project in[LiveKit Cloud](https://cloud.livekit.io/) for the fastest path, or self-host the LiveKit server if you have strict data residency needs.


**Access tokens and rooms**


Generate an access token per participant. Tokens encode the room name, participant identity, and permissions (publish audio, subscribe, etc.). Each phone call gets its own room, which isolates audio streams and prevents crosstalk.


**SIP integration with Plivo**


Configure Plivo to forward inbound calls to LiveKit's SIP endpoint. In the Plivo console, configure a trunk with your LiveKit SIP URI as the primary URI . When the call arrives at LiveKit endpoint, LiveKit server creates a room, adds the caller as a SIP participant and adds the voice agent as a participant into the conversation.


**Voice Agent Quickstart**


```text
from livekit import agents
from livekit.agents import AgentServer, AgentSession, Agent, room_io


class Assistant(Agent):
def __init__(self) -> None:
super().__init__(instructions="You are a helpful voice AI assistant.")


server = AgentServer()


@server.rtc_session(agent_name="my-agent")
async def my_agent(ctx: agents.JobContext):
session = AgentSession(
llm=openai.realtime.RealtimeModel(
voice="coral"
)
)


await session.start(
room=ctx.room,
agent=Assistant(),
room_options=room_io.RoomOptions(
audio_input=room_io.AudioInputOptions(noise_cancellation=noise_cancellation.BVC()),
),
)


await session.generate_reply(
instructions="Greet the user and offer your assistance. You should start by speaking in English."
)


if __name__ == "__main__":
agents.cli.run_app(server)
```


Run the agent worker, place a test call, and confirm round-trip audio before adding STT, LLM, or TTS. Most production issues at this layer are network and WebRTC traversal, not AI.


**Deployment**


Two production paths:


1.


**LiveKit Cloud** : managed agent dispatch, autoscaling, global edge presence. Point your Plivo SIP at the project URI and LiveKit handles the rest.


2.


**Plivo Voice AI Infra** : Deploy the LiveKit agent worker to Plivo's voice AI infrastructure, without any code changes. You inherit Plivo's enterprise-grade SIP trunking with a reach across 150+ countries, and 99.99% platform uptime, while your LiveKit Agent worker runs as-is alongside the call mediaserver with the lowest hop count.


## Building a Voice AI Agent with Pipecat


Pipecat's pipeline architecture is the cleanest mental model in the space. Each component has a single input and a single output.


**Installation**


```text
uv add pipecat-ai[deepgram,openai,cartesia,plivo]
```


Pipecat requires Python 3.12+ for async and type-hint support.


**Pipeline definition**


The pipeline reads as a left-to-right data flow. Drop a logging processor between any two stages to watch frames in real time.


```text
from pipecat.frames.frames import LLMMessagesAppendFrame
from pipecat.pipeline.pipeline import Pipeline
from pipecat.pipeline.runner import PipelineRunner
from pipecat.pipeline.task import PipelineParams, PipelineTask
from pipecat.serializers.plivo import PlivoFrameSerializer
from pipecat.services.google.gemini_live.llm import GeminiLiveLLMService
from pipecat.transports.websocket.fastapi import (
FastAPIWebsocketParams,
FastAPIWebsocketTransport,
)


serializer = PlivoFrameSerializer(stream_id=stream_id, call_id=call_id, auth_id=PLIVO_AUTH_ID, auth_token=PLIVO_AUTH_TOKEN)
transport = FastAPIWebsocketTransport(websocket=websocket,
params=FastAPIWebsocketParams(
audio_in_enabled=True, audio_out_enabled=True, add_wav_header=False, vad_enabled=True, serializer=serializer))
llm = OpenAILLMService(api_key=OAI_KEY, model="gpt-4o-mini")
llm = GeminiLiveLLMService(
api_key=GEMINI_API_KEY, model=GEMINI_MODEL, voice_id=GEMINI_VOICE, system_instruction=SYSTEM_PROMPT)
pipeline = Pipeline([transport.input(), llm, transport.output()])


```


For a complete working example with inbound and outbound flows, see Plivo's[gemini2.5-live-pipecat](https://github.com/plivo/python-agents-examples/tree/main/gemini2.5-live-pipecat) (speech-to-speech with Gemini 2.5 Live) or[gpt4o-deepgramnova3-openaitts4o-pipecat](https://github.com/plivo/python-agents-examples/tree/main/gpt4o-deepgramnova3-openaitts4o-pipecat) (STT plus LLM plus TTS pipeline).


**Deployment**


Two production paths:


1.


**Pipecat Cloud** : managed runtime with per-call billing, autoscaling, and built-in observability. Best when you do not want to operate infrastructure.


2.


**Plivo Voice AI Infra** : deploy the Pipecat agent as a container on Plivo's voice AI infrastructure, fronted by Plivo's bidirectional audio streaming. This keeps the agent server, SIP termination, and PSTN edge co-located on a single platform.


## Building a Voice AI Agent with Ten Framework


Ten Framework targets workloads where every millisecond counts.


**When Ten makes sense**


Pick Ten when latency matters more than developer ergonomics. Common cases: in-car voice assistants where end-to-end response under 300 ms is non-negotiable, edge deployments on resource-constrained hardware, and high-stakes control loops where Python GIL contention is unacceptable.


**Installation**


Ten ships precompiled binaries for Linux x86_64 and ARM64.


```text
pip install ten-runtime
```


For custom hardware targets, build from source. Configuration uses a YAML graph of nodes connected by typed edges.


**Sample: Ten voice agent graph**


```text
nodes:
- name: stt
addon: deepgram_python
property:
api_key: ${DG_KEY}
- name: llm
addon: openai_chatgpt
property:
api_key: ${OAI_KEY}
model: gpt-4o-mini
- name: tts
addon: cartesia_tts
property:
api_key: ${CARTESIA_KEY}
connections:
- source: stt.text_data
dest: llm.text_data
- source: llm.text_data
dest: tts.text_data
```


**Deployment**


1.


**Self-host** : managed runtime for teams that want hosted scheduling and observability without owning the C++ build toolchain.


## Building a Voice AI Agent Without a Framework (Native)


Frameworks are great until you hit an abstraction that does not match your problem. The native path skips them entirely: a FastAPI server, a WebSocket handler for Plivo's audio stream, and direct calls to your STT, LLM, and TTS providers.


**When native makes sense**


Pick native when you want the smallest possible runtime, when you want to own the interruption and turn-detection logic line by line, or when your model choice (a single speech-to-speech model like Gemini Live, GPT Realtime, or Grok Voice) makes most of a pipeline framework redundant.


**Reference architecture**


1.


A call hits Plivo.


2.


Plivo invokes a webhook on your FastAPI server.


3.


Plivo opens a bidirectional WebSocket to stream audio.


4.


Your handler streams audio to STT, the transcript to the LLM, and the LLM output to TTS, then writes synthesized audio back to the same WebSocket.


**Sample: minimal native voice agent**


```text
from google import genai
from google.genai import types
import asyncio


config = types.LiveConnectConfig(...)   # build it first


async with client.aio.live.connect(model=GEMINI_MODEL, config=config) as session:
await session.send_realtime_input(text=initial_message)


while running:
message = json.loads(await websocket.receive_text())


if message.get("event") == "media":
mulaw_audio = base64.b64decode(message["media"]["payload"])


# 1. Decode Plivo μ-law → 8kHz PCM
pcm_8k = ulaw_to_pcm(mulaw_audio)


# 2. Client-side VAD for barge-in detection
speech_started, _ = vad.process(pcm_8k)
if speech_started and agent_speaking:
trigger_interruption()   # drain queue + send clearAudio to Plivo


# 3. Resample 8kHz → 16kHz for the model
pcm_16k = resample_audio(pcm_8k, PLIVO_SAMPLE_RATE, GEMINI_INPUT_RATE)
audio_buffer.extend(pcm_16k)


# 4. Forward fixed-size chunks to the S2S API
if len(audio_buffer) >= AUDIO_CHUNK_SIZE:
chunk = bytes(audio_buffer[:AUDIO_CHUNK_SIZE])
audio_buffer = audio_buffer[AUDIO_CHUNK_SIZE:]
await session.send_realtime_input(
audio=types.Blob(data=chunk, mime_type="audio/pcm")
)
```


For a complete production reference (including interruption handling, function calling, and outbound dialing), see Plivo's native examples:[gpt5.4-deepgramnova3-groktts3-native](https://github.com/plivo/python-agents-examples/tree/main/gpt5.4-deepgramnova3-groktts3-native) for STT plus LLM plus TTS,[gemini2.5-live-native](https://github.com/plivo/python-agents-examples/tree/main/gemini2.5-live-native) for the speech-to-speech path, and[gptrealtime1.5-native](https://github.com/plivo/python-agents-examples/tree/main/gptrealtime1.5-native) for GPT Realtime with Silero VAD and barge-in.


**Deployment**


Native agents are containerized FastAPI apps, so any container platform works. Most teams deploying on Plivo Voice AI Infra to keep the SIP termination, audio WebSocket, and agent server on a single platform with one observability surface.


## Choosing the Right Approach


The decision usually comes down to four questions:


1.


**Do you need full multimodal support (voice plus video plus chat)?** Pick **LiveKit** . WebRTC is in the DNA; video and screen sharing are first-class citizens.


2.


**Is your team Python-native and ergonomics-first?** Pick **Pipecat** . The pipeline abstraction is the cleanest mental model in the space, and the integration ecosystem is the largest.


3.


**Is sub-300 ms latency a hard requirement, even at the cost of C++ build complexity?** Pick **Ten** .


4.


**Do you want the smallest possible runtime and full control over interruption and turn-detection logic?** Go **native** . You will write more code, but every layer is yours to debug.


For most teams shipping phone-based voice agents in 2026, **LiveKit or native on Plivo Voice AI Infra** is the right starting point. You inherit Plivo's enterprise-grade SIP trunking, global PSTN reach across 150+ countries, and 99.99% platform uptime, while keeping the conversation logic in code your team can actually read.


If you want to skip the framework decision entirely, Plivo's no-code[Agent Studio](https://www.plivo.com/ai/) and Vibe Agent builder let you describe the use case in natural language and ship to production without writing the orchestration loop yourself.


## FAQ


**What is the difference between a voice AI agent SDK and a voice AI platform?**


An SDK gives you the building blocks (STT, LLM orchestration, TTS, audio transport) and asks you to write the agent loop, host the runtime, and integrate telephony. A platform like Plivo's AI Agents bundles the SDK with managed telephony, a no-code builder (Agent Studio and Vibe Agent), and per-minute pricing. SDKs maximize flexibility; platforms maximize time-to-production.


**When should I skip the framework and go native?**


Three signals push you to native: (1) you are using a single speech-to-speech model and a framework would mostly be wrapping one HTTP stream, (2) your team wants to own interruption and turn-detection logic line by line, (3) you want fewer dependencies in the hot path and shorter cold-start times. The native examples in Plivo's repo show the pattern end to end.


**What do I lose by skipping the framework?**


Pipeline ergonomics, batteries-included VAD and turn detection, and a large library of pre-built integrations. You will rebuild some of these (a basic VAD wrapper, retry logic, frame logging) but they typically take an afternoon, not a sprint.


**Which approach has the lowest end-to-end latency?**


Ten Framework leads on raw audio-pipeline performance because of its C++ core. In practice, network and model latency dominate end-to-end time, so a well-tuned Pipecat, LiveKit, or native agent on a fast LLM (Groq, Cerebras) can match Ten in most production scenarios. Latency should be measured at the 95th percentile, not just the average,[according to Coval's 2026 voice AI benchmarks](https://www.coval.dev/blog/voice-ai-platform-comparison-2026-benchmarks-performance-data-and-how-to-choose) .


**Can I use these approaches for phone calls, not just web audio?**


Yes. LiveKit, Pipecat, Ten, and native all integrate with Plivo's bidirectional WebSocket for SIP-terminated calls. The cleanest pattern is to terminate the call on Plivo's carrier-grade SIP trunk and bridge audio into whichever runtime you picked.


**How do I handle interruptions and barge-in?**


All three frameworks ship with VAD and turn-taking primitives. Pipecat's VADStopFrame is the most ergonomic. LiveKit handles interruption models out of the box. Ten exposes lower-level audio events. Native code handles this with a small state machine: cancel the in-flight LLM and TTS streams as soon as the VAD signals the user started speaking. The[gptrealtime1.5-native](https://github.com/plivo/python-agents-examples/tree/main/gptrealtime1.5-native) example shows this pattern with Silero VAD.


**What does production observability look like?**


Instrument three things: per-stage latency (STT, LLM, TTS), frame loss across the pipeline, and call-level outcomes (handled, transferred, dropped). Pipecat ships OpenTelemetry hooks. LiveKit exposes in-platform observability modules. Ten emits structured events. Native code emits whatever you write, which is both the upside and the downside.


## Conclusion


The voice AI agent stack in 2026 has settled around four production paths. LiveKit owns multimodal real-time. Pipecat owns Python ergonomics and the broadest integration ecosystem. Ten owns latency-critical workloads where C++ performance matters. And, native code owns the cases where you want zero abstractions between your code and the audio stream.


Pick the path that fits your team and stack, then plug it into voice infrastructure that already handles the hard parts: SIP trunking, PSTN reach, enterprise-grade reliability, and global compliance. That separation of concerns is what makes shipping voice agents in weeks, not quarters, actually possible.


Ready to build? Explore[Plivo's AI Agents platform](https://www.plivo.com/ai/) , clone a working reference from[python-agents-examples](https://github.com/plivo/python-agents-examples) , or[get started with Plivo AI Voice agents](https://cx.plivo.com/signup) to test your stack on real phone numbers in your own workflows.
