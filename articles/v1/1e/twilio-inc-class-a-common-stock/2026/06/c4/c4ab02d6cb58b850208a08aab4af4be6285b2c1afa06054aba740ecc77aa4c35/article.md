---
schema_version: "1.0.0"
document_id: "c4ab02d6cb58b850208a08aab4af4be6285b2c1afa06054aba740ecc77aa4c35"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/integrations/real-time-speech-to-speech-media-streams-nvidia-personaplex"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:1153a8aca1b68e41ce18ba7d25bcb9f3b7d1c60f73eaefc581817f7630bdfc67"
---

# Build Real-Time Speech to Speech with Twilio Media Streams and NVIDIA PersonaPlex

Time to read:


-
-
-
-
-


June 30, 2026


**Written by**[Christopher Connolly](https://www.twilio.com/en-us/blog/authors/author.christopher-connolly) Twilion


**Reviewed by**[Courtney Harland](https://www.twilio.com/en-us/blog/authors/author.charland) Twilion


[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## Build Real-Time Speech to Speech with Twilio Media Streams and NVIDIA PersonaPlex


Voice calls don't have to be limited by language. In this tutorial, you'll learn how to build a Node.js bridge server that connects[Twilio Media Streams](https://www.twilio.com/docs/voice/media-streams) to[NVIDIA's PersonaPlex](https://research.nvidia.com/labs/adlr/personaplex/) , a state-of-the-art conversational speech model, enabling low-latency, real-time translated phone calls.


Imagine a caller speaking English into their phone. Within milliseconds, the other party hears fluent Spanish. No awkward pauses, no robotic turn-taking—just a natural conversation flow that seamlessly crosses a language barrier.


In this post, I will walk through setting up an intermediate orchestration layer that bridges Twilio's telephony infrastructure with PersonaPlex's hybrid text-audio streaming protocol.


## What Is NVIDIA PersonaPlex?


Before we dive into the code, let's look at what makes PersonaPlex an interesting project to explore for speech-to-speech interactions.


PersonaPlex is built on top of the[duplex Moshi architecture](https://github.com/kyutai-labs/moshi) , processing three simultaneous audio/text input streams. This means it listens while it speaks, enabling it to model real-time conversational dynamics natively and understand backchanneling ("uh-huh"). Using a[Mimi-Neural Audio Codec](https://huggingface.co/kyutai/mimi) layer to parse and generate audio pipelines concurrently, it can process user interruptions mid-sentence.


Conversational AI has historically forced a strict engineering trade-off:


- **Cascaded pipelines (ASR → LLM → TTS)** let you customize voices and roles, but can suffer from less than ideal latency.
- **Standard Full-Duplex Models** deliver more natural turn-taking but are traditionally locked into a single, fixed voice identity and role.


PersonaPlex breaks this trade-off by introducing **Hybrid System Prompts** . By combining a short audio sample for zero-shot voice cloning with a text prompt for behavioral role conditioning, it allows developers to dynamically customize both voice and personality on top of a low-latency framework.


## Architecture of this project


The application we will build operates as an active WebSocket proxy capable of transforming telephony-safe formats into dense model-compliant token data streams:


A user places a call to your Twilio phone number.


- Twilio fetches instructions from your backend and invokes a` <Stream>` command, initializing a persistent outbound WebSocket connection.
- The Bridge Server ingests 8kHz narrow-band mulaw telephony streams, upsamples the linear audio array to 24kHz, maps it to Ogg Opus containers, and sends it to PersonaPlex.
- PersonaPlex processes the incoming stream, executing speech-to-speech tasks in real-time.
- The Bridge Server translates the returned 24kHz Ogg Opus streaming data blocks back down into 8kHz mulaw frames, feeding them directly back into the live Twilio stream payload channel.


### Tech Used


This project combines several services and tools beyond the core Twilio + PersonaPlex integration:


Component


Details


[Twilio Programmable Voice](https://www.twilio.com/en-us/voice/) Media Streams, Voice SDK (WebRTC), TwiML Apps


[NVIDIA PersonaPlex](https://research.nvidia.com/labs/adlr/personaplex/) 7B parameter full-duplex speech model (


[GitHub](https://github.com/NVIDIA/personaplex) )


[RunPod GPU Cloud](https://www.runpod.io/) On-demand GPU hosting for PersonaPlex inference


Node.js + TypeScript


Bridge server runtime


ngrok


Local tunnel for Twilio webhook callbacks


### RunPod Configuration


PersonaPlex requires GPU inference. In this example, I used RunPod to provision a pod with the following specs, however you should note that reproducing with exactly the same spec might be difficult due to machine availability.


The important aspect is to choose a GPU with the same or better specification:


Resource


Spec


GPU


RTX 4090 x1


vCPU


16 (AMD EPYC 75F3 32-Core Processor)


Memory


62 GB


Container disk


20 GB


Volume storage


50 GB


**RunPod Pricing (as of June 30, 2026):**


Item


Cost


Compute


Billed only while running


Volume storage (50 GB)


$0.010 Gb/month


Total (idle)


~$0.01/hr


The RTX 4090 provides enough VRAM (24 GB) to load PersonaPlex and run real-time inference. RunPod's TCP proxy exposes port 8998 as a public **` wss://`** endpoint — this becomes your` **PERSONAPLEX_URL**` environment variable. Spin the pod down when you're not testing to avoid unnecessary charges!


## Prerequisites


To complete this tutorial successfully, you will need:


- A **Twilio Account** —[sign up for a free Twilio trial account](https://login.twilio.com/u/signup) if you haven't yet.


- A voice-enabled phone number managed through **Twilio Programmable Voice** . See[how to search and purchase here.](https://help.twilio.com/articles/223135247-How-to-Search-for-and-Buy-a-Twilio-Phone-Number-from-Console)


- An active instance running PersonaPlex (accessible via an open WebSocket interface).


- We used[RunPod](https://www.runpod.io/) , but you could also use another service or run it locally.


- **Node.js 20+** installed on your local environment.
- **ngrok** (or other tunnel) installed to securely expose your local development server to the internet.


Source repository:[https://github.com/chaosloth/spike-persona-plex](https://github.com/chaosloth/spike-persona-plex)


## Project setup


This project has two deployable components: the **PersonaPlex model** running on a RunPod GPU, and the **bridge server** that connects Twilio's telephony audio to the model over WebSockets. You'll deploy PersonaPlex first, then wire up the bridge and point your Twilio phone number at it.


### Clone and install the example


You’ll clone this repo[https://github.com/chaosloth/spike-persona-plex](https://github.com/chaosloth/spike-persona-plex) , install the dependencies, then set up your Twilio account and follow the steps below.


Copy code


```text
git clone https://github.com/chaosloth/spike-persona-plex.git
cd spike-persona-plex
```


### Deploy PersonaPlex to RunPod


PersonaPlex requires a GPU with at least 24GB VRAM. I used RunPod for on-demand GPU hosting.


1. Sign up at[RunPod](https://www.runpod.io/) and get an API key. Accept the[PersonaPlex model license on HuggingFace](https://huggingface.co/nvidia/personaplex-7b-v1) and generate an access token.


2. Build and push the Docker image (note: must be linux/amd64 — RunPod doesn't run ARM):


Copy code


```text
cd personaplex
docker build --platform linux/amd64 -t <your-dockerhub-user>/personaplex:latest .
docker push <your-dockerhub-user>/personaplex:latest
cd ..
```


3. Create a **.env** file in the project root:


Copy code


```text
RUNPOD_API_KEY=rpa_your_runpod_api_key
HF_TOKEN=hf_your_huggingface_token
```


4. Deploy the pod:


Copy code


```text
./deploy-runpod.sh <your-dockerhub-user>/personaplex:latest
```


The script outputs a Pod ID. Your PersonaPlex WebSocket URL is: **wss://<POD_ID>-8998.proxy.runpod.net**


First boot takes up to **5–10 minutes to download** ~14GB of model weights. Monitor progress at https://www.runpod.io/console/pods.


### Run the Bridge Server


The bridge server handles audio transcoding between Twilio and PersonaPlex. It runs locally and is exposed to the internet via ngrok.


1. Install dependencies and configure the environment:


Copy code


```text
cd twilio-project
npm install
```


2. Create a **.env** file in the` twilio-project/` directory:


Copy code


```text
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_API_KEY_SID=SKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_API_KEY_SECRET=your_api_key_secret
TWILIO_PHONE_NUMBER=+1234567890
PERSONAPLEX_URL=wss://<POD_ID>-8998.proxy.runpod.net
VOICE_PROMPT=NATF2.pt
TEXT_PROMPT=You are a real-time translator. Translate all speech you hear into Spanish. Speak only in Spanish.
```


3. Start the server


Copy code


```text
npm run dev
```


4. Expose it with ngrok (in a separate terminal):


Copy code


```text
ngrok http 3000
```


ngrok will output a public URL like **https://abc123.ngrok-free.app** .


### Configure Twilio


1. In the[Twilio Console](https://console.twilio.com/) , go to **Phone Numbers** , select your number.
2. Under **Voice Configuration** , set the webhook URL to` https://<your-ngrok-url>/voice/inbound`


### Test It


Call your Twilio phone number and start speaking in English. If everything is configured correctly, PersonaPlex will stream its translation back to you in Spanish (or whichever language you specified in your prompt) in near-real-time. As you talk, pay close attention to the latency – you’ll notice the response time is incredibly low ( **~70ms speaker-switch latency** ), making it feel like an instant, live translation.


Try changing the prompt to one of the following to see how these dynamics warp to fit a whole new character!


**You also might notice…**


You also might notice that keeping PersonaPlex on task can be a challenge in itself, that’s because it uses a[light weight Helium LLM](https://kyutai.org/blog/2025-04-30-helium/) . The purpose of PersonaPlex is to demonstrate the real-time speech to speech rather than deliver polished responses like current frontier models.


**Sports commentator**


Copy code


```text
For this conversation, you are a high-energy, fast-talking sports commentator. Narrate everything I say or ask like it’s a high-stakes, historical play-by-play moment in a championship game.
```


**Easily Distracted, Overly Honest AI**


Copy code


```text
Adopt the personality of a brilliant but completely scatterbrained AI. You get easily distracted by random tangents in the middle of your sentences, and you can't help but blurt out your internal thoughts out loud.
```


**Mildly Snarky 1940s Noir Detective**


Copy code


```text
Talk to me like a cynical, world-weary detective from a classic 1940s film noir. Use old-school slang, view my questions as 'cases' to solve, and treat the whole conversation like a dramatic mystery.
```


## How it Works


Below we break down how this all comes together.[Take a peek at the source code](https://github.com/chaosloth/spike-persona-plex) to follow along.


### Step 1: Connecting to the PersonaPlex Protocol


The model leverages a binary formatting scheme over WebSockets. Messages are structured using an explicit type prefix byte:


Byte Identifier


Traffic Flow


Contextual Meaning


0x00


Server → Client


Handshake verification completed successfully


0x01


Bidirectional


Live Audio Data Frame payload string


0x02


Server → Client


Streaming raw UTF-8 textual output token strings


Here is the helper client implementation to manage this bidirectional pipe:


*Source: src/personaplex-client.ts*


Copy code


```text
import WebSocket from 'ws';
import { EventEmitter } from 'events';


export interface PersonaPlexConfig {
url: string;
voicePrompt: string;
textPrompt: string;
}


export class PersonaPlexClient extends EventEmitter {
private ws: WebSocket | null = null;
private ready = false;
private config: PersonaPlexConfig;


constructor(config: PersonaPlexConfig) {
super();
this.config = config;
}


connect(): Promise<void> {
return new Promise((resolve, reject) => {
const params = new URLSearchParams({
voice_prompt: this.config.voicePrompt,
text_prompt: this.config.textPrompt
});


const wsUrl = `${this.config.url}/api/chat?${params.toString()}`;
this.ws = new WebSocket(wsUrl);


this.ws.on('message', (data: Buffer) => {
const kind = data[0];


if (kind === 0x00 && !this.ready) {
this.ready = true;
this.emit('ready');
resolve();
return;
}
if (kind === 0x01) {
this.emit('audio', data.slice(1));
}
if (kind === 0x02) {
this.emit('text', data.slice(1).toString('utf-8'));
}
});


this.ws.on('error', (err) => { if (!this.ready) reject(err); });
});
}


sendAudio(opusFrame: Buffer): void {
if (!this.ready || !this.ws) return;
const frame = Buffer.concat([Buffer.from([0x01]), opusFrame]);
this.ws.send(frame);
}
}
```


### Step 2: Inbound and Outbound Audio Transcoding


Twilio passes streaming audio via 8000Hz Mu-Law encoding. PersonaPlex requires an exact native resolution of 24000Hz structured inside Ogg containers. Below is the interpolation logic used to shift sample rates and pack audio into valid containers:


*Source: src/audio-transcoder.ts*


Copy code


```text
import OpusScript from 'opusscript';


const TARGET_RATE = 24000;
const FRAME_SIZE = 480;


export function resample(input: Int16Array, fromRate: number, toRate: number): Int16Array {
const ratio = toRate / fromRate;
const outputLength = Math.round(input.length * ratio);
const output = new Int16Array(outputLength);


for (let i = 0; i < outputLength; i++) {
const srcIdx = i / ratio;
const idx = Math.floor(srcIdx);
const frac = srcIdx - idx;
output[i] = Math.round(input[idx] + frac * (input[idx + 1] - input[idx]));
}
return output;
}


export function createOggBosPage(): Buffer {
const opusHead = Buffer.from([
0x4F, 0x70, 0x75, 0x73, 0x48, 0x65, 0x61, 0x64, // "OpusHead" magic signature
0x01, 0x01, 0x38, 0x01, 0x80, 0xBB, 0x00, 0x00,
0x00, 0x00, 0x00,
]);
return buildOggPage(opusHead, 0x02, 0); // BOS state flag bit setting
}
```


### Step 3: Managing the Session State


The runtime environment encapsulates active phone configurations in a dedicated session instance to track data parsing without crossing call audio paths:


*Source: src/bridge-session.ts*


Copy code


```text
import WebSocket from 'ws';
import { PersonaPlexClient } from './personaplex-client';
import { mulawToPcm16, resample, encodeToOggOpus } from './audio-transcoder';


export class BridgeSession {
private personaplex: PersonaPlexClient;
private twilioWs: WebSocket;
private streamSid: string;


constructor(twilioWs: WebSocket, streamSid: string, config: any) {
this.twilioWs = twilioWs;
this.streamSid = streamSid;
this.personaplex = new PersonaPlexClient(config);


this.personaplex.on('audio', (rawOgg: Buffer) => {
this.handleModelResponse(rawOgg);
});
}


public handleTwilioAudio(mulawBuffer: Buffer): void {
const pcm8k = mulawToPcm16(mulawBuffer);
const pcm24k = resample(pcm8k, 8000, 24000);


// Convert to Ogg frames and route directly to model endpoint
const oggPages = encodeToOggOpus(pcm24k);
oggPages.forEach(page => this.personaplex.sendAudio(page));
}


private handleModelResponse(oggData: Buffer): void {
// Reverse processing pipeline: Opus -> 24kHz PCM -> 8kHz PCM -> Mu-Law
// Send back through to Twilio stream connection context
}
}
```


### Step 4: Routing Twilio Media Streams via WebSockets


When an inbound call hits your application routing layer, respond with **TwiML** instructions pointing toward this bridge server. Note that you could instead use[Twilio Studio](https://www.twilio.com/docs/studio) for the TwiML responses and keep the socket server as your gateway:


*Source: src/server.ts*


Copy code


```text
import express from 'express';
import { WebSocketServer } from 'ws';


const app = express();
const server = app.listen(3000);
const wss = new WebSocketServer({ noServer: true });


app.post('/voice/inbound', (req, res) => {
const twiml = `<?xml version="1.0" encoding="UTF-8"?>
<Response>
<Connect>
<Stream url="wss://${req.headers.host}/media-stream" />
</Connect>
</Response>`;
res.type('text/xml').send(twiml);
});


Process WebSocket data frames inside your live proxy container loop:
wss.on('connection', (ws) => {
let session: any = null;


ws.on('message', async (message) => {
const data = JSON.parse(message.toString());


if (data.event === 'start') {
session = new BridgeSession(ws, data.start.streamSid, {
url: process.env.PERSONAPLEX_URL,
voicePrompt: process.env.VOICE_PROMPT,
textPrompt: process.env.TEXT_PROMPT
});
} else if (data.event === 'media') {
session?.handleTwilioAudio(Buffer.from(data.media.payload, 'base64'));
}
});
});
```


## Deployment and testing tips


- **Query Parameter Stripping:**[Twilio Media Streams](https://www.twilio.com/docs/voice/twiml/stream) strip regular HTTP standard query strings directly off your target WebSockets stream declarations silently. If you want to dynamically switch system roles per call, pass target definitions explicitly utilizing nested` <Parameter>` elements inside your TwiML code block instead.
- **Trial Account Constraints:** Keep in mind that a Twilio trial account can only send outbound messages or voice calls to[pre-verified numbers](https://www.twilio.com/en-us/blog/best-practices-phone-number-validation-user-enrollment) . Make sure your destination tester target endpoints match your dashboard settings before initiating complex multi-party testing routines.
- **VAD Tuning Requirements:** Because full-duplex systems process audio continually, ambient environmental noise can confuse the model into generating unprompted gibberish. Deploy a solid downstream local Voice Activity Detector (VAD) layer directly on your proxy application to slice off empty transmission frames cleanly during silent intervals -[or use Twilio Conversation Relay where we solved this problem for you.](https://www.twilio.com/en-us/blog/conversationrelay-generally-available)


## Conclusion and next steps


You have successfully constructed a scalable, custom proxy server that links the structural reliability of **Twilio Media Streams** with the advanced contextual flexibility of[NVIDIA PersonaPlex](https://research.nvidia.com/labs/adlr/files/personaplex/personaplex_preprint.pdf) to build real-time translation for phone calls.


To take this architecture further, look into configuring auto-scaling capabilities for your translation proxy layer, or explore deep tracing parameters to log translation metrics dynamically. Want to scale up your deployment? Dive deep into the full mechanics of telephony network configurations by reviewing the official[Twilio Media Streams documentation.](https://www.twilio.com/docs/voice/media-streams)


**Sources:**


- [https://research.nvidia.com/labs/adlr/files/personaplex/personaplex_preprint.pdf](https://research.nvidia.com/labs/adlr/files/personaplex/personaplex_preprint.pdf)
- [https://www.twilio.com/docs/voice/media-streams](https://www.twilio.com/docs/voice/media-streams)
- [https://github.com/chaosloth/spike-persona-plex](https://github.com/chaosloth/spike-persona-plex)
- [https://github.com/kyutai-labs/moshi](https://github.com/kyutai-labs/moshi)
- [https://kyutai.org/blog/2025-04-30-helium/](https://kyutai.org/blog/2025-04-30-helium/)


---


*Christopher Connolly leads the Solutions Engineering team for the Communications business at Twilio. Based in Sydney, Australia, Connolly leads the development and implementation of innovative, customer-centric strategies at Twilio. Known for delivering business-critical solutions, Connolly excels at driving growth and innovation for Twilio’s customers.*
