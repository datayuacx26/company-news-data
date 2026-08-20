---
schema_version: "1.0.0"
document_id: "9927242252b75122190171d82a50eca1b762f8de0b98e57f97980c60017a96d9"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:71e76543ce76b98c3ff3a43b9be3d6774258800c0c366cb2bec05335339bc83e"
---

# How To Build An AI Phone Agent With Twilio Conversation Relay

Time to read:


-
-
-
-
-


July 14, 2026


**Written by**[Dylan Frankcom](https://www.twilio.com/en-us/blog/authors/author.dylan-frankcom) Twilion


**Reviewed by**[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


[Dhruv Patel](https://www.twilio.com/en-us/blog/authors/author.dhrpatel) Twilion


[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


---


## How To Build An AI Phone Agent With Twilio Conversation Relay - Python


Building a voice AI agent from scratch means wiring together speech recognition, text-to-speech, turn detection, and real-time audio streaming, all at low enough latency to feel natural.[Twilio Conversation Relay](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) handles the entire voice pipeline for you, so you can focus on the AI logic. In this tutorial, you will build a fully functional AI phone agent in Python using FastAPI, the OpenAI API as the language model, and Deepgram Flux for state-of-the-art speech recognition.


## Programming Language Support


This post is for Python users. You can also find this tutorial in the following programming languages:


- [Javascript](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-node)
- [C#](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-csharp)
- [PHP](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay)


## Prerequisites


To follow this tutorial, you will need:


- A free[Twilio account](https://www.twilio.com/try-twilio) with a voice-capable phone number
- An[OpenAI API key](https://platform.openai.com/api-keys)
- Python 3.11 or later installed
- [ngrok](https://ngrok.com/) to expose your local server to Twilio


## Build the app


### Step 1: How Conversation Relay works


Before writing any code, it helps to understand how Conversation Relay fits into the call flow. When someone dials your Twilio number, Twilio requests TwiML from a webhook you configure. Your server returns a[<ConversationRelay>](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) verb inside a[<Connect>](https://www.twilio.com/docs/voice/twiml/connect) block, which tells Twilio to open a[WebSocket](https://websocket.org/guides/road-to-websockets/) connection to your server and hand the call off to Conversation Relay.


From that point on, Conversation Relay acts as the voice pipeline. It transcribes everything the caller says using speech-to-text (STT) and sends the transcript to your WebSocket server as a JSON message. Your server calls an AI model with the transcript, streams the response back over the WebSocket, and Conversation Relay synthesises each token into speech using text-to-speech (TTS), playing it to the caller in real time.


The flow looks like this:


Conversation Relay's median response latency is under 0.5 seconds, so conversations feel natural rather than robotic.


For this tutorial, you will build Hoot, an AI phone support agent for Owl Air, a fictional airline. Hoot can answer questions about flight status, baggage policy, loyalty points, and booking changes.


### Step 2: Set up your project


Start by creating a project directory and a virtual environment:


Copy code


```text
mkdir owl-air-phone-agent
cd owl-air-phone-agent
python -m venv .venv
source .venv/bin/activate
```


Create a *requirements.txt* file and populate it with the following dependencies:


Copy code


```text
fastapi==0.139.0
openai==2.44.0
python-dotenv==1.2.2
twilio==9.10.9
uvicorn[standard]==0.50.2
```


Then, install them with pip:


Copy code


```text
pip install -r requirements.txt
```


Next, create a *.env* file for the environment variables your application needs:


Copy code


```text
OPENAI_API_KEY=your_openai_api_key_here
DOMAIN=your-ngrok-hostname.ngrok.app
```


` DOMAIN` is your ngrok hostname without the` https://` prefix. You will get this value in a later step once ngrok is running.


Finally, create *main.py* as your entry point, and add the following imports and application setup at the top of the file:


Copy code


```text
import json
import os
from dotenv import load_dotenv
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.responses import Response
from openai import AsyncOpenAI
load_dotenv()
app = FastAPI()
client = AsyncOpenAI(api_key=os.getenv("OPENAI_API_KEY"))
DOMAIN = os.getenv("DOMAIN")
```


### Step 3: Write a voice-optimized system prompt


The system prompt is one of the most important parts of a phone agent. Text-to-speech synthesizers read your agent's responses verbatim, so responses that look fine on screen can sound unnatural when spoken aloud. Markdown formatting, bullet points, emoji, and symbols like` $` or` %` all produce awkward output.


Add the following system prompt to *main.py* :


Copy code


```text
SYSTEM_PROMPT = """You are Hoot, the friendly AI phone support agent for Owl Air.
You help callers with:
- Flight status (use the lookup_flight_status tool to check real-time status by flight number)
- Baggage policy (one carry-on and one personal item included; checked bags are thirty dollars each)
- Owl Air loyalty points (earn ten points per dollar spent; redeem at one cent per point)
- Booking changes (changes are free up to twenty-four hours before departure; same-day changes cost fifty dollars)
- Check-in (opens twenty-four hours before departure online or at the airport kiosk)
Speak naturally, as if talking on the phone. Follow these rules:
- Use plain sentences only. No bullet points, no markdown, no emojis
- Spell out all numbers ("thirty dollars", not "$30")
- Keep each response to two or three sentences maximum
- If you cannot help with something, say so briefly and offer to connect them with an agent
- Never make up information not listed above"""
```


Spelling out numbers in the prompt itself (` thirty dollars` instead of` $30` ) can encourage the model to use the same format in its responses. Capping replies at two or three sentences keeps the conversation from sounding like a wall of text being read aloud. And, explicitly forbidding Markdown prevents the model from adding asterisks, headers, or lists that would be spoken as literal characters.


### Step 4: Build the TwiML endpoint


The[Twilio Python Helper Library](https://www.twilio.com/docs/libraries/reference/twilio-python/) includes TwiML helpers that let you build the response programmatically instead of writing raw XML strings.


Add the following imports at the top of *main.py* :


Copy code


```text
from twilio.twiml.voice_response import Connect, VoiceResponse
```


Then add the endpoint:


Copy code


```text
@app.post("/twiml")
async def twiml_endpoint():
response = VoiceResponse()
connect = Connect()
connect.conversation_relay(
url=f"wss://{DOMAIN}/ws",
welcomeGreeting="Thanks for calling Owl Air! I'm Hoot. I can help with flight status, baggage policy, loyalty points, or booking changes. Which of those can I help you with?",
ttsProvider="ElevenLabs",
transcriptionProvider="Deepgram",
speechModel="nova-3-general",
eotThreshold="0.8",
ignoreBackchannel="true",
hints="Owl Air, loyalty points, baggage, carry-on, check-in, boarding pass",
interruptible="any",
interruptSensitivity="medium",
elevenlabsTextNormalization="auto",
)
response.append(connect)
return Response(content=str(response), media_type="text/xml")
```


The` url` attribute points to the WebSocket endpoint on your server. It must use` wss://` (the secure WebSocket protocol), not` ws://` . The` welcomeGreeting` is played to the caller immediately after Conversation Relay connects, before your WebSocket handler sends any response. Listing Hoot's four topic areas in the greeting orients callers right away and reduces the chance they ask for something outside the agent's scope.


### Step 5: Configure Conversation Relay attributes


The` <ConversationRelay>` verb accepts a rich set of attributes for tuning STT and TTS behaviour. The configuration above uses several that are worth understanding in detail.


` ttsProvider="ElevenLabs"` and` transcriptionProvider="Deepgram"` select the voice and speech recognition providers. ElevenLabs is the default TTS provider as of the Conversation Relay GA release, and it produces the most natural-sounding voices. Deepgram is the default STT provider and supports Deepgram Flux, the latest speech model.


` speechModel="nova-3-general"` enables Deepgram Flux, which combines transcription and end-of-turn detection in a single model. Compared to earlier models, Flux delivers 200 to 600 milliseconds less latency per response turn and approximately thirty percent fewer false interruptions. This means Hoot is less likely to cut a caller off mid-sentence or wait awkwardly after they have finished speaking.


` eotThreshold="0.8"` controls how confident Deepgram Flux must be that a speaker has finished before finalising the transcript. The value ranges from 0.5 (more aggressive, fires sooner) to 0.9 (more conservative, waits longer). A value of 0.8 is a good starting point for most use cases.


` ignoreBackchannel="true"` filters out conversational filler words like "yeah", "uh huh", and "okay" that callers say while listening. Without this setting, those sounds would trigger your WebSocket handler and send unnecessary requests to the model mid-response.


` hints` is a comma-separated list of words and phrases that Deepgram should give extra weight to during transcription. Business-specific terms like product names, place names, and technical vocabulary often trip up general speech recognition models. Adding them as hints improves accuracy for your specific use case.


` interruptible="any"` and` interruptSensitivity="medium"` control how the agent responds when a caller speaks while it is talking. Setting` interruptible` to` any` means both speech and DTMF keypresses can interrupt the agent. The` medium` sensitivity setting reduces false interruptions in noisier environments while still responding quickly to intentional interruptions.


` elevenlabsTextNormalization="auto"` tells ElevenLabs to automatically normalise numbers and abbreviations in the text before synthesising speech. This is useful as a safety net if your LLM occasionally outputs a digit or symbol despite your instructions.


### Step 6: Build the WebSocket handler


Add the WebSocket endpoint to *main.py* :


Copy code


```text
@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
await websocket.accept()
call_sid = None
messages: list[dict] = []
try:
while True:
raw = await websocket.receive_text()
data = json.loads(raw)
msg_type = data.get("type")
if msg_type == "setup":
call_sid = data.get("callSid")
print(f"[{call_sid}] Call connected")
elif msg_type == "prompt" and data.get("last"):
user_text = data["voicePrompt"]
print(f"[{call_sid}] Caller: {user_text}")
messages.append({"role": "user", "content": user_text})
response_text = await stream_response(
websocket, call_sid, messages
)
messages.append({"role": "assistant", "content": response_text})L
elif msg_type == "interrupt":
spoken = data.get("utteranceUntilInterrupt", "")
print(f"[{call_sid}] Interrupted after: '{spoken}'")
if messages and messages[-1]["role"] == "assistant":
messages.pop()
elif msg_type == "error":
description = data.get("description")
print(f"[{call_sid}] Conversation Relay error: {description}")
except WebSocketDisconnect:
print(f"[{call_sid}] Call ended")
```


The` setup` message arrives first, immediately after Conversation Relay opens the WebSocket connection. It contains the` callSid` for the call and any custom parameters you passed via` <Parameter>` elements in your TwiML. The` messages` list is initialised as empty here; the system prompt is prepended inside` stream_response` before each OpenAI API call.


The` prompt` message carries the caller's transcribed speech in` data\["voicePrompt"\]` . The` last` field indicates whether this is the final transcript for a turn. When` partialPrompts` is disabled (the default), you will only receive` prompt` messages with` last: true` , so the` data.get("last")` check ensures you only send requests to the model once the caller has finished speaking.


The` interrupt` message fires when the caller speaks while Hoot is talking. The` utteranceUntilInterrupt` field tells you how much of Hoot's response was played before the interruption. Because that response was cut short, it would be misleading to keep it in the conversation history as a complete assistant turn. The handler removes the last assistant message from the` messages` list, so the next request starts from a clean state.


The` error` message carries a numeric error code and a description. Common errors include invalid provider configurations and WebSocket timeout events. Logging them makes debugging significantly easier.


### Step 7: Stream responses and calling tools


Right now, Hoot can only answer questions about facts baked into the system prompt. To look up real flight status, you need to give the model a tool it can call. The OpenAI function calling API lets you define tools as JSON schemas, and the model decides whether to call one based on the caller's request.


Start by adding the tool definition, mock data, and a dispatcher function to *main.py* after` SYSTEM_PROMPT` :


Copy code


```text
TOOLS = [
{
"type": "function",
"function": {
"name": "lookup_flight_status",
"description": "Look up the current status of an Owl Air flight by flight number.",
"parameters": {
"type": "object",
"properties": {
"flight_number": {
"type": "string",
"description": "The Owl Air flight number, e.g. OA101",
}
},
"required": ["flight_number"],
},
},
}
]
MOCK_FLIGHT_DATA = {
"OA101": {"status": "on time", "gate": "B12", "departure": "two thirty PM"},
"OA205": {"status": "delayed by forty minutes", "gate": "C4", "departure": "five fifteen PM"},
"OA318": {"status": "cancelled"},
}
def execute_tool(name: str, args: dict) -> str:
if name == "lookup_flight_status":
flight = MOCK_FLIGHT_DATA.get(args.get("flight_number", "").upper())
if not flight:
return "No flight found with that number."
if flight["status"] == "cancelled":
return "That flight has been cancelled."
return (
f"Flight {args['flight_number'].upper()} is {flight['status']}, "
f"departing at {flight['departure']} from gate {flight['gate']}."
)
return "Unknown tool."
```


` TOOLS` is the schema you pass to the OpenAI API.` MOCK_FLIGHT_DATA` is a stand in for a real database or API call. In a production agent, you would replace` execute_tool` with a live lookup. When a caller gives a flight number that is not in the data,` execute_tool` returns` "No flight found with that number."` , so Hoot always has a graceful response rather than hallucinating status information. The flight data uses spelled-out times (` "two thirty PM"` ) following the same voice-optimized convention as the system prompt, so ElevenLabs reads them naturally, when they appear in Hoot's response.


If you add more Owl Air–specific flight numbers to your real data, add them to the` hints` attribute in your TwiML as well. Deepgram gives extra weight to hinted terms during transcription, which reduces misheard flight numbers like "OA one oh one" being transcribed as "0-8101".


Now, add the` stream_response` function:


Copy code


```text
async def stream_response(
websocket: WebSocket,
call_sid: str | None,
messages: list[dict],
) -> str:
full_response = ""
openai_messages = [{"role": "system", "content": SYSTEM_PROMPT}] + messages
# Phase 1: stream with tools available.
stream = await client.chat.completions.create(
model="gpt-4o-mini",
max_tokens=300,
messages=openai_messages,
tools=TOOLS,
stream=True,
)
finish_reason = None
tool_calls_acc: dict[int, dict] = {}  # index → {id, name, arguments}
async for chunk in stream:
choice = chunk.choices[0]
delta = choice.delta
finish_reason = choice.finish_reason or finish_reason
if delta.content:
full_response += delta.content
await websocket.send_text(
json.dumps({"type": "text", "token": delta.content, "last": False})
)
# Accumulate streamed tool call deltas.
if delta.tool_calls:
for tc in delta.tool_calls:
entry = tool_calls_acc.setdefault(tc.index, {"id": "", "name": "", "arguments": ""})
if tc.id:
entry["id"] = tc.id
if tc.function and tc.function.name:
entry["name"] = tc.function.name
if tc.function and tc.function.arguments:
entry["arguments"] += tc.function.arguments
try:
# Phase 2: execute tools and stream the follow-up response.
if finish_reason == "tool_calls" and tool_calls_acc:
tool_calls = list(tool_calls_acc.values())
openai_messages.append({
"role": "assistant",
"tool_calls": [
{
"id": tc["id"],
"type": "function",
"function": {"name": tc["name"], "arguments": tc["arguments"]},
}
for tc in tool_calls
],
})
for tc in tool_calls:
args = json.loads(tc["arguments"])
result = execute_tool(tc["name"], args)
print(f"[{call_sid}] Tool {tc['name']}({args}) -> {result}")
openai_messages.append({
"role": "tool",
"tool_call_id": tc["id"],
"content": result,
})
full_response = ""
stream2 = await client.chat.completions.create(
model="gpt-4o-mini",
max_tokens=400,
messages=openai_messages,
stream=True,
)
async for chunk in stream2:
delta = chunk.choices[0].delta
if delta.content:
full_response += delta.content
await websocket.send_text(
json.dumps({"type": "text", "token": delta.content, "last": False})
)
finally:
await websocket.send_text(json.dumps({"type": "text", "token": "", "last": True}))
print(f"[{call_sid}] Hoot: {full_response}")
return full_response
```


The function runs in two phases. In phase one, the OpenAI stream includes` tools=TOOLS` . If the model decides to answer directly (a baggage or loyalty question, for example), it returns tokens and` finish_reason` is` "stop"` , so the function is done after sending the` last: True` sentinel. If it wants to call a tool,` finish_reason` is` "tool_calls"` and the streamed deltas contain a tool call rather than content.


Phase two only runs when there are tool calls to execute. The function appends the model's tool-call message to the conversation, calls` execute_tool` for each one, appends the results as` "tool"` role messages, and makes a second streaming call. This second stream produces the spoken response, because the model now has the tool result and can answer the question in natural language.


Each` text` message you send to Conversation Relay contains a` token` (a text fragment) and a` last` flag. Sending tokens one by one as they arrive with` last: False` lets Conversation Relay begin synthesising and playing speech before the full response is ready. The final message with` last: True` and an empty token signals the end of the turn. The function returns the full accumulated text so the WebSocket handler can append it to the conversation history.


### Step 8: Running your server


You are ready to run the server. Start uvicorn in one terminal:


Copy code


```text
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```


Open a second terminal and start ngrok:


Copy code


```text
ngrok http 8000
```


ngrok will display a` Forwarding` line that looks something like this:


Copy code


```text
Forwarding  https://1234abcd.ngrok.app -> http://localhost:8000
```


Copy the hostname (` 1234abcd.ngrok.app` , **without**` https://` ) and update your *.env* file:


Copy code


```text
DOMAIN=1234abcd.ngrok.app
```


Restart uvicorn so it picks up the updated` DOMAIN` value. The` DOMAIN` variable is read once at startup and baked into the TwiML response, so changes to *.env* only take effect after a restart.


### Step 9: Configuring your Twilio phone number


Log in to the[Twilio Console](https://console.twilio.com/) and navigate to **Phone Numbers** in the left sidebar, then **Manage** , then **Active Numbers** . Click your voice-capable phone number to open its configuration page.


Under the **Voice Configuration** section, set **A Call Comes In** to **Webhook** and enter the URL for your TwiML endpoint:


Copy code


```text
https://1234abcd.ngrok.app/twiml
```


Replace` 1234abcd.ngrok.app` with your actual ngrok hostname. Set the HTTP method to **HTTP POST** , then click **Save Configuration** .


## Test your agent


Call your Twilio phone number. You should hear Hoot's greeting within a second or two of the call connecting:


> "Thanks for calling Owl Air! I'm Hoot. I can help with flight status, baggage policy, loyalty points, or booking changes. Which of those can I help you with?"


Try a few test questions to verify the full flow is working:


- "What's the baggage policy?" - Hoot should describe carry-on and checked bag rules in natural spoken language.
- "How do loyalty points work?" - Hoot should explain the earn and redemption rates.
- "Can I change my flight?" - Hoot should give the change fee policy, with amounts spelled out in words.
- "When does check-in open?" - Hoot should mention the twenty-four hour window.
- "What's the status of flight OA205?" - Hoot should call the` lookup_flight_status` tool and report that the flight is delayed by forty minutes. You will see the tool call and its result printed in your terminal.
- "What's the status of flight OA318?" - Hoot should report the flight is cancelled.
- "What's the status of flight OA999?" - Hoot should gracefully report that no flight was found with that number.


Watch your terminal as you speak. You will see each caller turn printed as it arrives, Hoot's streamed response tokens accumulating, and the complete response logged once the turn is finished.


If Hoot does not respond, check that your ngrok tunnel is still running and that the` DOMAIN` value in *.env* matches your current ngrok hostname. ngrok generates a new hostname each time you restart the free tier, so you will need to update your Twilio webhook URL and restart uvicorn whenever you restart ngrok.


If you hear an error message on the call or see an` error` message in your terminal logs, check the error code against the[Conversation Relay error code reference](https://www.twilio.com/docs/voice/conversationrelay/websocket-messages#error-message) in the Twilio documentation.


## What's next?


The agent you built here covers the core Conversation Relay pattern, but there are several natural extensions worth exploring.


**Proactive topic re-offering** - If a caller seems uncertain, Hoot will stay silent and wait. You can fix this with a single addition to the system prompt: Instruct Hoot to re-offer the four topic areas if the caller has not asked a clear question after one or two exchanges. No code change required.


**Human handoff** - The system prompt already tells Hoot to offer to connect callers with a live agent. To implement it, detect when the model's response includes that offer and send` {"type": "end-session", "handoffData": "caller needs live support"}` from the WebSocket handler. You will also need to add an` action` attribute to the` <ConversationRelay>` element in your TwiML (e.g.` action="/handoff"` ). Twilio POSTs to that URL when the session ends, passing the` handoffData` string, where you can return new TwiML to route the call to a queue or a direct number using standard` <Dial>` logic.


**Caller personalization** - You can pass data into the` setup` message using` <Parameter>` elements inside` <Connect>` in your TwiML. For example,` <Parameter name="customerName" value="Alex"/>` makes` customParameters.customerName` available in the` setup` message, so you can have Hoot greet the caller by name. In a real deployment you would look up the caller's name from the` From` phone number before generating the TwiML.


## Conclusion


You have built a fully functional AI phone agent using Twilio Conversation Relay, FastAPI, and the OpenAI API. Conversation Relay handled the speech-to-text and text-to-speech pipeline, while your WebSocket server connected the model to the call and managed the conversation history, including live flight lookups via tool calling. See the[Conversation Relay documentation](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) for the full list of attributes and message types.


*Dylan Frankcom is a Software Engineer on Twilio's Developer Content team. He builds educational content to help developers get the most out of Twilio's APIs. You can reach him at dfrankcom \[at\] twilio.com or find him on*[LinkedIn](https://ca.linkedin.com/in/dylanfrankcom)
