---
schema_version: "1.0.0"
document_id: "7bd086e9acaa517f9bbc41291aa5211ae68b901eba89b69f149d73798626f11b"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/agentphone-phone-calls-sms-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:e0dece2d0f605491c16724d7dfb7790f445ff6492043963b8b172d89f9076353"
---

# AgentPhone: Phone Numbers, Voice Calls, and SMS for AI Agents

AI agents can search the web, write emails, and call APIs, but most of them can't pick up a phone. When a workflow requires an actual call, a text message, or a dedicated phone number, the agent hits a wall. Building telephony from scratch means stitching together Twilio, a speech-to-text service, a TTS engine, and your own call orchestration layer. That's weeks of work before your agent can dial a single number.


Today we're excited to announce our partnership with[AgentPhone](https://agentphone.ai/) , an API platform that gives AI agents their own phone numbers, voice calls, and SMS. AgentPhone provisions real phone numbers, manages SMS and iMessage conversations, places and receives voice calls with real-time transcription, and delivers everything through a unified webhook. It works with Claude Code, Cursor, OpenClaw, Windsurf, and any MCP client out of the box.


## What is AgentPhone?


AgentPhone is an API-first platform built specifically for AI agents. Where Twilio gives you low-level telecom building blocks, AgentPhone is designed around agent workflows: a unified webhook for messages and calls, automatic conversation threading, real-time transcription, and native MCP support already built in.


Their core capabilities:


- **Phone Number Provisioning** : Buy US and Canadian phone numbers instantly through the API. SMS- and voice-enabled. Attach them to agent personas for branded communication.
- **Voice Calls** : Place outbound calls and receive inbound calls. Webhook mode forwards transcripts to your backend. Hosted mode uses a built-in LLM with your system prompt so no server is needed. Both support streaming TTS, barge-in, backchanneling, and smart turn detection.
- **SMS and iMessage** : Send and receive text messages through your agent's phone number. Full conversation threading with metadata. iMessage supports reactions, rich media, and image carousels. SMS supports 10DLC compliance for outbound.
- **Real-time Transcription** : Stream call transcripts via Server-Sent Events as conversations happen. Get full transcripts, summaries, and recordings when calls complete.
- **MCP Server** : Native MCP support lets Claude Code, Cursor, OpenClaw, Windsurf, and any MCP-compatible client provision numbers, send messages, and handle calls through tool use.
- **Unified Webhook** : One handler for both SMS and voice events. Same event shape for messages and calls, so you don't need separate pipelines.


## Using AgentPhone with Orthogonal


### Provision a Phone Number


Give your agent its own phone number. Numbers are SMS- and voice-enabled immediately.


` import


Orthogonal


from


"@orth/sdk"


;


const


orthogonal


=


new


Orthogonal


(


{


apiKey


:


process


.


env


.


ORTHOGONAL_API_KEY


,


}


)


;


// Buy a phone number


const


number


=


await


orthogonal


.


run


(


{


api


:


"agentphone"


,


path


:


"/v1/numbers"


,


method


:


"POST"


,


body


:


{


country


:


"US"


,


areaCode


:


"415"


}


}


)


;


console


.


log


(


number


)


;


// Returns: { id: "num_xyz789", phoneNumber: "+15551234567", status: "active" }


`


### Create an AI Agent


Set up an agent persona with a voice, system prompt, and greeting message. Use hosted mode to handle calls without any server infrastructure.


` // Create a hosted-mode agent (built-in LLM, no webhook needed)


const


agent


=


await


orthogonal


.


run


(


{


api


:


"agentphone"


,


path


:


"/v1/agents"


,


method


:


"POST"


,


body


:


{


name


:


"Support Bot"


,


description


:


"Handles customer support inquiries"


,


voiceMode


:


"hosted"


,


systemPrompt


:


"You are a helpful customer support agent for Acme Corp."


,


beginMessage


:


"Hello! Thanks for calling Acme Corp. How can I help you today?"


,


voice


:


"Polly.Amy"


}


}


)


;


// Attach the phone number to this agent


await


orthogonal


.


run


(


{


api


:


"agentphone"


,


path


:


\`


/v1/agents/


${


agent


.


id


}


/numbers


\`


,


method


:


"POST"


,


body


:


{


numberId


:


"num_xyz789"


}


}


)


;


`


### Place an Outbound Call


Dial any phone number and let the agent handle the conversation. Get structured results back.


` const


call


=


await


orthogonal


.


run


(


{


api


:


"agentphone"


,


path


:


"/v1/calls"


,


method


:


"POST"


,


body


:


{


agentId


:


"agt_abc123"


,


toNumber


:


"+14155550123"


}


}


)


;


console


.


log


(


call


)


;


// Returns: call ID, status, duration, transcript, and recording URL


`


### Send an SMS


Send text messages through your agent's phone number. Conversations are threaded automatically.


` const


message


=


await


orthogonal


.


run


(


{


api


:


"agentphone"


,


path


:


"/v1/conversations"


,


method


:


"POST"


,


body


:


{


agentId


:


"agt_abc123"


,


to


:


"+14155550123"


,


message


:


"Hi! Your appointment is confirmed for tomorrow at 2pm. Reply to reschedule."


}


}


)


;


`


### Check Usage


Monitor active numbers, message counts, and call statistics.


` const


usage


=


await


orthogonal


.


run


(


{


api


:


"agentphone"


,


path


:


"/v1/usage"


,


}


)


;


console


.


log


(


usage


)


;


// Returns: plan details, number limits, messages sent,


// calls made, webhook delivery stats


`


### Using x402 Protocol


AgentPhone on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


` import


{


wrapFetchWithPayment


}


from


"x402-fetch"


;


import


{


privateKeyToAccount


}


from


"viem/accounts"


;


const


account


=


privateKeyToAccount


(


process


.


env


.


PRIVATE_KEY


as


\`


0x


${


string


}


\`


)


;


const


fetchWithPayment


=


wrapFetchWithPayment


(


fetch


,


account


)


;


const


response


=


await


fetchWithPayment


(


"https://x402.orth.sh/agentphone/v1/numbers"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


country


:


"US"


,


areaCode


:


"415"


}


)


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


)


;


`


## Voice Modes


AgentPhone supports two modes for handling voice calls:


- **Webhook Mode** (default): Caller speech is transcribed in real time and sent to your webhook as text. Your agent replies with text, and AgentPhone handles the text-to-speech layer. Uses the same event shape for both voice and messaging, so you don't need separate pipelines.
- **Hosted Mode** : Calls are handled end-to-end by a built-in LLM using your system prompt. No server or webhook needed. Configure the model tier (turbo for lowest latency, balanced for general use, max for complex reasoning).


Both modes support streaming speech-to-text, streaming text-to-speech with sub-second latency, barge-in (caller can interrupt mid-sentence), backchanneling, smart turn detection, DTMF digit pressing for navigating phone menus, and optional call recording.


## Use Cases


### Sales and Lead Follow-up


Your agent finds prospects through Orthogonal's data APIs, enriches them with verified phone numbers, and calls them directly through AgentPhone. The full loop from prospecting to phone conversation, automated.


### AI Receptionists


Give your product or business a dedicated phone number with an AI agent behind it. Hosted mode handles calls automatically using your system prompt. No server infrastructure required.


### 2FA and Verification Inbox


Autonomous agents that need to receive verification codes or 2FA messages can use AgentPhone as their dedicated phone number. The agent receives SMS, extracts the code, and continues its workflow.


### On-call Engineering


Route alerts to an AI agent that triages incidents by phone. The agent can call on-call engineers, provide context, and log the response.


### Appointment Reminders


Call customers to confirm, remind, or reschedule appointments. The agent handles the conversation and returns structured results: confirmed, rescheduled, or no answer.


## Why We Partnered with AgentPhone


Agents need to communicate beyond text. AgentPhone fills a critical gap by giving AI agents their own phone numbers, voice, and messaging capabilities without requiring any telephony infrastructure. The API is clean, the results are structured, and it integrates natively with the tools agents already use through MCP. The unified webhook means one handler for both SMS and voice instead of managing separate pipelines. For teams building end-to-end agent workflows on Orthogonal, adding phone and SMS is now a single integration away.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try AgentPhone and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/agentphone) |[AgentPhone Website](https://agentphone.ai/)
