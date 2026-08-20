---
schema_version: "1.0.0"
document_id: "74f805fcb9f7afe04ef034ec9c529f929a98b2ca4864878e175fb5e791e1fbd0"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-and-less-robotic-in-python"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-22T01:46:36.263837+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:4dc4f3b41da01853bbfd6d724c4f9753e72036d85fd437d81b38d8a5e5b09259"
---

# How to Make Your AI Voice Sound More Human and Less Robotic with Python

Time to read:


-
-
-
-
-


July 21, 2026


**Written by**[Dylan Frankcom](https://www.twilio.com/en-us/blog/authors/author.dylan-frankcom) Twilion


[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


---


## How to Make Your AI Voice Sound More Human and Less Robotic with Python


If you've called a customer service helpline recently, chances are good that you've encountered some kind of AI assistant voice helper. But not all AI helpers are created equally. Some AI assistants are truly a pain to deal with, with long latency that creates a frustrating conversation. Others are more human-like, with a pleasant tone and cadence and the ability to handle your requests and interruptions gracefully.


What's the difference? If you want your AI to sound more human and less robotic on the phone, having a great simulated voice really helps. But aside from that, creating a low-latency bot that handles interruptions gracefully can be a big benefit to your users and make them feel more at ease.


In this tutorial, you will build a real-time voice agent using Python,[Twilio Conversation Relay,](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) and[OpenAI](https://openai.com/) . In[previous tutorials](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) , we've created an AI that operated as a concierge to help users with questions they have about our fictional airline, Owl Air. This time, you will program your AI as a flight assistant dealing with a panicked traveler who is running late. This will allow you to demonstrate and test two important principles that help make an AI sound more human: a patient tone, and gracefully handling user interruptions.


## Prerequisites


To complete this tutorial, be sure to have:


- [Python 3.12 or later](https://www.python.org/downloads/)
- A[Twilio Account](https://www.twilio.com/login) (with a voice-enabled phone number)
- An[OpenAI API Key](https://openai.com/index/openai-api/)
- ngrok (to tunnel your local development server to the internet)
- An IDE of your choice (such as[Visual Studio Code](https://code.visualstudio.com/) )


## Building your application


### Step 1: Create a new Python project


Open up your terminal and type the following to create a new Python project and install the required dependencies.


Copy code


```text
mkdir voice-humanizer
cd voice-humanizer
python -m venv .venv
source .venv/bin/activate
pip install fastapi uvicorn python-dotenv openai websockets
```


On Windows, activate the virtual environment with` .venv\\Scripts\\activate` instead.


### Step 2: Setting up environment variables


Create a file named *.env* in your project folder. This will safely store your API key and your URL to avoid exposing them if you decide to use git or share your project in some other way. In this file, add the following information:


Copy code


```text
OPENAI_API_KEY=your_actual_openai_api_key_here
DOMAIN=ABC12345.ngrok.app
```


Replace the placeholder for your OpenAI key with your real OpenAI API key, found on[the OpenAI dashboard](https://platform.openai.com/api-keys) .


For` DOMAIN` , this is where you will add your ngrok tunneling url, when that is available in a future step.


Create a file named *main.py* in your project folder. At the top of the file, load your environment variables with the following:


Copy code


```text
import os
from dotenv import load_dotenv
load_dotenv()
```


### Step 3: Creating your API


When a customer calls your Twilio number, Twilio needs to fetch configuration instructions. You will write a small FastAPI endpoint that returns[TwiML](https://www.twilio.com/docs/voice/twiml) instructing Twilio to pipe the call details directly into our local Python WebSocket.


Open *main.py* and add the following baseline structure after your environment variable setup:


Copy code


```text
import asyncio
import json
import os
from dotenv import load_dotenv
from fastapi import FastAPI, WebSocket
from fastapi.responses import Response
from openai import AsyncOpenAI
load_dotenv()
app = FastAPI()
@app.post("/voice")
async def voice():
domain = os.getenv("DOMAIN", "")
twiml = f"""<Response>
<Connect>
<ConversationRelay
url="wss://{domain}/ws"
welcomeGreeting="Hey there! Oh no, I see you missed your flight. Don't worry, we'll get you sorted out. Where are you trying to head?"
ttsProvider="ElevenLabs"
transcriptionProvider="Deepgram"
speechModel="nova-3-general"
eotThreshold="0.8"
ignoreBackchannel="true"
interruptible="any"
interruptSensitivity="medium"
elevenlabsTextNormalization="auto"
/>
</Connect>
</Response>"""
return Response(content=twiml, media_type="application/xml")
@app.websocket("/ws")
async def ws_endpoint(websocket: WebSocket):
await websocket.accept()
await handle_voice_session(websocket)
```


The line` ttsProvider="ElevenLabs"` uses[ElevenLabs](https://elevenlabs.io/) for your TTS (Text to Speech). This gives you a more human-like voice cadence for conversations right out of the box. In this tutorial, you are using the` nova-3-general` speech model.


If you want more details about how some of these attributes work, check out our tutorial on Making an AI Phone Agent With Twilio Conversation Relay, which goes into the specifics about these attributes.


You might notice that your code is not yet complete, because you need to write the` handle_voice_session` function that actually processes the incoming transcript. You will create that in the next step.


### Step 4: Core functionality and interruption handling


In this step you will write the core functionality for your AI. To make our AI sound human, this block of code needs to:


1. Listen to incoming transcripts sent from Twilio.
2. Stream OpenAI answers word-by-word back to Twilio over the WebSocket with minimal latency.
3. Listen for the` interrupt` event from Twilio. If the user starts talking, we must use` asyncio.Task.cancel()` to abruptly kill the active OpenAI stream and send a clear signal back to Twilio to stop talking instantly.


Add this function to *main.py* :


Copy code


```text
async def handle_voice_session(websocket: WebSocket):
# Keep track of our active streaming task so we can cancel it on demand
active_task: asyncio.Task | None = None
try:
while True:
raw = await websocket.receive_text()
msg = json.loads(raw)
msg_type = msg.get("type")
# Case A: The user spoke!
if msg_type == "prompt":
user_text = msg.get("voicePrompt", "")
print(f"[User Spoke]: {user_text}")
# Cancel any outbound speech that is currently processing
if active_task:
active_task.cancel()
# Fire the task in the background so our loop can keep listening for interruptions!
active_task = asyncio.create_task(
stream_llm_response(websocket, user_text)
)
# Case B: The user cut the robot off mid-sentence!
elif msg_type == "interrupt":
print("Interruption detected.")
# Tell our local background task to stop calling OpenAI
if active_task:
active_task.cancel()
# Tell Twilio's audio player to clear its current buffer instantly
await websocket.send_text(json.dumps({"type": "clear"}))
except Exception as ex:
print(f"Session error: {ex}")
```


After pasting this code, you can move on to the next step.


### Step 5: Structuring the streamed data for human-like conversation


This helper method calls OpenAI's chat completion endpoint, streams the response token-by-token, wraps the output in[SSML markup tags](https://en.wikipedia.org/wiki/Speech_Synthesis_Markup_Language) , and passes it directly to Twilio.


Add this function to *main.py* , above the` voice()` and` ws_endpoint()` functions.


Copy code


```text
openai_client = AsyncOpenAI(api_key=os.getenv("OPENAI_API_KEY"))
async def stream_llm_response(websocket: WebSocket, user_input: str):
try:
stream = await openai_client.chat.completions.create(
model="gpt-4o",
stream=True,
messages=[
{
"role": "system",
"content": (
"You are an empathetic, lightning-fast airport flight concierge. "
"Keep answers extremely short (under 2 sentences) because this is a phone call. "
"Insert a comma or ellipsis '...' where you want natural breathing pauses. "
"Use occasional casual conversational fillers like 'Oh, okay...', 'Uhm...', or 'Let me check...' to sound natural. "
"Use plain sentences only. No bullet points, no markdown, no emojis. "
"Spell out all numbers ('thirty dollars', not '$30')."
),
},
{"role": "user", "content": user_input},
],
)
async for chunk in stream:
text_token = chunk.choices[0].delta.content or ""
# Humanizer Trick: Dynamic SSML Formatting!
# If the LLM generates a dramatic ellipsis '...', turn it into an explicit breathing break.
if text_token == "...":
text_token = '<break time="200ms"/>'
# Format token to Twilio specifications
await websocket.send_text(json.dumps({
"type": "text",
"token": text_token,
"last": False,  # Tells Twilio to expect more streaming speech chunks
}))
# Send a final token indicating that this conversational turn is complete
await websocket.send_text(json.dumps({"type": "text", "token": "", "last": True}))
except asyncio.CancelledError:
print("LLM streaming task cancelled successfully.")
```


Focus your attention on the system prompts that we're creating for this AI. You are using system prompts to make the AI voice model mimic human conversational traits:


- Speaking in concise, run-on phrases rather than structured bullet points.
- Using micro-pauses (` <break time="150ms"/>` ) to simulate taking a breath.
- Occasional usage of natural conversational fillers ("Oh...", "I see...", "Give me just a second").
- Making sure that all numbers are articulated clearly, and the voice isn't trying to respond in emojis or a bulleted list.


## Testing Your Application


With all the code in place, you're ready to test your application.


Open your terminal in your IDE and type:


With all the code in place, you're ready to test your application.


Open your terminal in your IDE and type:


Copy code


```text
uvicorn main:app --port 8000
```


Take note of what port (usually 8000) your server is running on. Now, in a separate terminal, expose your local port using ngrok:


Copy code


```text
ngrok http 8000
```


Ngrok will provide you with a secure url to hit with your web endpoint. Update the` DOMAIN` in your *.env* file to point to this new endpoint.


Now, restart your server. (` uvicorn main:app --port 8000` )


Point your Twilio Phone Number’s "A Call Comes In" webhook to your ngrok domain. In[the Twilio Console](https://console.twilio.com/) , navigate through **Products and Services > Numbers and Senders >** choose your number. Then go to **Voice and Emergency Calling** and click **Edit Configuration Details** in the upper right hand corner. Choose your appropriate country. Then go to **How do you want to set up your primary method?** And choose "Use Webhooks". Paste the webhook url from your ngrok endpoint, followed by` /voice` (as in` \[https://1234abcd.ngrok.app/voice\](https://1234abcd.ngrok.app/voice)` ). Set **Method to** "HTTP POST" from the dropdown menu. Then click **Save** .


Call your Twilio number to test your new bot!


Say hello, and observe how quickly the assistant answers you. Because we stream the output token-by-token directly from OpenAI, your assistant should react to your voice in under 500 milliseconds.


Now, try interrupting your agent as it's speaking to test its interruption handling. While the assistant is explaining flight details, talk directly over it saying: *"Wait! Stop, when does that flight leave?"* Because of your background task listener, the AI will immediately cease playing audio, clear its queue, and smoothly begin answering your interruption.


## Next steps


Making an AI sound human isn't about synthesizing a perfect accent: it is an engineering challenge. By ditching slow, request-response pipelines in favor of Python WebSockets via FastAPI and[Twilio Conversation Relay](https://www.twilio.com/docs/voice/conversationrelay) , you can build voice systems that react in real-time, handle interruptions with grace, and speak with realistic pauses.


Your AI here isn't designed to collect any real information for the user and is just a quick demonstration, but there are a lot of additional features you could add. For example,[adding tool calling to your AI](https://www.twilio.com/en-us/blog/add-function-tool-calling-twilio-voice-openai-integration) could allow you to get real flight numbers as your user needs them. For more information, check out our Conversation Relay templates on GitHub. Happy building!


*Dylan Frankcom is a Software Engineer on Twilio's Developer Content team. He builds educational content to help developers get the most out of Twilio's APIs. You can reach him at dfrankcom \[at\] twilio.com or find him on*[LinkedIn](https://ca.linkedin.com/in/dylanfrankcom) *.*
