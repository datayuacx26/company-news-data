---
schema_version: "1.0.0"
document_id: "e53981644b98814ad6d00b34fcc4553f153c0a967253166eac74728e8fc0b184"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/orbital-hotline-twilio-voice-conversationrelay-python-openai"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T13:09:03.715655+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:7a37e8298378d8d7aa4d1389ad3c29e2f404d9c982259034a2d923f462a5799c"
---

# How I Built an Orbital Hotline with Twilio Voice, ConversationRelay, Python, and OpenAI

Time to read:


-
-
-
-
-


July 22, 2026


**Written by**[Rishab Kumar](https://www.twilio.com/en-us/blog/authors/author.rikumar) Twilion


**Reviewed by**[Dhruv Patel](https://www.twilio.com/en-us/blog/authors/author.dhrpatel) Twilion


[Dylan Frankcom](https://www.twilio.com/en-us/blog/authors/author.dylan-frankcom) Twilion


---


## How I Built an Orbital Hotline with Twilio Voice, ConversationRelay, Python, and OpenAI


I wanted to build a phone number that felt like calling a tiny mission control desk.


You dial in, ask where the International Space Station is, find out when it will pass over your city, get a quick sky report, hear sounds from across the universe, or ask a general question about planets, stars, missions, and the cosmos.


That became the Orbital Hotline: a voice-first space guide built with Twilio Voice,[Conversation Relay](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) ,[FastAPI](https://fastapi.tiangolo.com/) , WebSockets,[OpenAI](https://openai.com/) , and a handful of live astronomy APIs.


In this post, I'll show you how I built it. By the end, you'll understand how to connect a phone call to a Python WebSocket server, route caller requests to deterministic space features, use a Large Language Model (LLM) only where it makes sense, and keep the responses friendly for spoken audio.


Let's dive in and make it happen!


## Prerequisites


To follow along, you'll need:


- Python 3.11 or later
- A[Twilio account](https://twil.io/try-twilio) and a Twilio phone number
- Conversation Relay enabled for your Twilio Voice flow
- An OpenAI API key
- An[N2YO](https://www.n2yo.com/) API key for ISS pass predictions
- [Ngrok](https://ngrok.com/) or another public HTTPS tunnel for local development
- A phone to place a test call


The[complete app for my version](https://github.com/rishabkumar7/space-voice-hotline) uses these Python packages:


Copy code


```text
uvicorn
fastapi
python-dotenv
websockets
openai
httpx
skyfield
timezonefinder
```


Copy code


```text
pip install -r requirements.txt
```


The main dependencies are FastAPI, Uvicorn, httpx, python-dotenv, OpenAI's Python SDK, Skyfield, and timezonefinder.


## What the hotline can do


I did not want the app to be just another "talk to an AI over the phone" demo. I wanted it to feel like a real hotline with specific things it could do well.


The final version handles:


- ISS location right now
- Next visible ISS pass from the caller's location
- A location-aware sky report
- Curated space sound playback
- General space Q and A using OpenAI
- Follow-up prompts
- Silence handling
- Unknown intent fallback
- A small Twilio-powered stats dashboard


The important design decision was to route known intents first and only use AI fallback for open-ended space questions. If someone asks "Where is the ISS?", the app should not ask a model to guess, it should call the ISS endpoint. If someone asks "Why does Jupiter have a giant storm?", that is a great LLM turn.


## How it works


Before you build it, here's the full picture. A caller's phone connects through a Twilio phone number to Twilio Programmable Voice and Conversation Relay, which handles speech-to-text and text-to-speech. Conversation Relay exchanges events with the` FastAPI` app over a WebSocket, the app routes each request through an intent router, and the matched handler reaches out to OpenAI or a live astronomy API only when it needs to. The following diagram shows how the call, the app, and the external data sources fit together, along with how the app is hosted.


## Project structure


This[project](https://github.com/rishabkumar7/space-voice-hotline) is intentionally small:


Copy code


```text
main.py
requirements.txt
.env.example
major_meteor_showers.json
de421.bsp
sounds/
Dockerfile
```


Most of the application lives in main.py. That file includes the FastAPI routes, the WebSocket handler, the intent router, the external API calls, and the Twilio stats dashboard.


For a demo project, keeping everything in one file made iteration fast. For a production app, I would split the code into modules for routing, space data services, audio, and stats.


## Configure environment variables


I used` python-dotenv` so local development could read configuration from a *.env* file.


Create a *.env* file:


Copy code


```text
cp .env.example .env
```


Then add the required values:


Copy code


```text
OPENAI_API_KEY="your-openai-api-key"
NGROK_URL="your-ngrok-domain.ngrok-free.app"
N2YO_API_KEY="your-n2yo-api-key"
```


The` NGROK_URL` should be the domain only. Do not include` https://` and do not include a trailing slash. The app uses this value to build the secure WebSocket URL that Twilio connects to.


There are also optional values for audio clips, Twilio call stats, TTS settings, and silence timeout behavior:


Copy code


```text
TWILIO_ACCOUNT_SID="your-twilio-account-sid"
TWILIO_AUTH_TOKEN="your-twilio-auth-token"
TWILIO_PHONE_NUMBER="your-twilio-phone-number"
AUDIO_JUPITER_URL="https://example.com/jupiter.wav"
AUDIO_PULSAR_URL="https://example.com/pulsar.mp3"
AUDIO_CMB_URL="https://example.com/cmb.wav"
AUDIO_BLACK_HOLE_URL="https://example.com/black-hole.mp3"
```


## Create the FastAPI app


Let's start with the foundation. The app loads environment variables and builds the public WebSocket URL:


Copy code


```text
load_dotenv()


PORT = int(os.getenv("PORT", "8080"))
DOMAIN = os.getenv("NGROK_URL")
WS_URL = f"wss://{DOMAIN}/ws" if DOMAIN else None
MODEL = os.getenv("OPENAI_MODEL", "gpt-4o-mini")


openai = AsyncOpenAI(api_key=os.getenv("OPENAI_API_KEY"))
sessions: dict[str, dict[str, Any]] = {}
```


The` sessions` dictionary stores per-call state. Each active call gets its own entry keyed by the Twilio` callSid` .


For this project, session state includes:


- The current flow path
- Whether the app is waiting for a location
- The caller's resolved location
- OpenAI message history
- Pending follow-up state
- Unknown intent count
- Silence timeout state
- Pending audio task


This is enough for a single-process demo. **Note:** If I scaled the app across multiple replicas, I would move the session state to Redis or use sticky routing so each call stays on the same instance.


## Return TwiML for Twilio Voice


When a call reaches the Twilio number, Twilio requests instructions from the` /twiml` endpoint.


The app responds with[TwiML (Twilio Markup Language)](https://www.twilio.com/docs/voice/twiml) that connects the call to Conversation Relay:


Copy code


```text
@app.post("/twiml")
async def twiml_endpoint() -> Response:
if not WS_URL:
return Response(
content="NGROK_URL not configured",
media_type="text/plain",
status_code=500,
)
welcome = await build_welcome_message()
xml_response = f"""<?xml version="1.0" encoding="UTF-8"?>
<Response>
<Connect>
<Conversation Relay
url="{html.escape(WS_URL, quote=True)}"
welcomeGreeting="{html.escape(welcome, quote=True)}"
ttsProvider="{html.escape(TTS_PROVIDER, quote=True)}"
voice="{html.escape(TTS_VOICE, quote=True)}" />
</Connect>
</Response>"""
return Response(content=xml_response, media_type="text/xml")
```


The` welcomeGreeting` is generated dynamically. I use the people-in-space API so the call begins with a live detail:


Copy code


```text
async def build_welcome_message() -> str:
people = await get_people_in_space()
if isinstance(people.get("number"), int):
population = str(people["number"])
else:
population = "several"
return (
"Welcome to the Orbital Hotline. I am your friendly connection to space. "
f"There are {population} humans in space right now. "
"You can ask me things like: Where is the space station? What is above me right now? "
"And I can also answer space questions about planets, stars, missions, and the universe. "
"What would you like to explore?"
)
```


This gives the caller a useful menu without making it sound like a traditional phone tree.


## Handle Conversation Relay WebSocket events


Conversation Relay sends real-time events over the WebSocket. The main events I handle are:


- ` setup`
- ` prompt`
- ` interrupt`
- ` error`
- ` end`


Here is the shape of the WebSocket loop:


Copy code


```text
@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket) -> None:
await websocket.accept()
call_sid = None
try:
while True:
payload = await websocket.receive_text()
message = json.loads(payload)
msg_type = message.get("type")
if msg_type == "setup":
call_sid = message.get("callSid")
sessions[call_sid] = {
"current_path": "entry",
"awaiting_location_for": None,
"location": None,
"unknown_count": 0,
"pending_followup": None,
"messages": [{"role": "system", "content": SYSTEM_PROMPT}],
"awaiting_user": False,
"silence_deadline": None,
"watchdog_task": asyncio.create_task(
run_silence_watchdog(websocket, call_sid)
),
}
elif msg_type == "prompt":
session = sessions[call_sid]
cancel_pending_audio_task(session)
session["awaiting_user"] = False
session["silence_deadline"] = None
session["silence_count"] = 0
user_text = message.get("voicePrompt", "")
await process_user_prompt(websocket, call_sid, user_text)
elif msg_type == "interrupt":
if call_sid and call_sid in sessions:
cancel_pending_audio_task(sessions[call_sid])
elif msg_type == "end":
break
finally:
if call_sid:
session = sessions.pop(call_sid, None)
if session and session.get("watchdog_task"):
session["watchdog_task"].cancel()
```


The important part is that each prompt goes through` process_user_prompt()` . That function decides whether to answer with deterministic data, play audio, ask for a location, call OpenAI, or reprompt the caller.


## Route deterministic intents before using AI


Think of the intent router as a switchboard operator at a real hotline: it listens to what the caller asks, and connects them to the right desk before anyone bothers the AI. It normalizes the transcript, checks session state, and returns a route name.


Copy code


```text
def route_intent(text: str, session: dict[str, Any]) -> str:
normalized = normalize_text(text)
if contains_any_keyword(normalized, GOODBYE_KEYWORDS):
return "goodbye"
if any(keyword in normalized for keyword in NAVIGATION_KEYWORDS):
return "navigation"
if any(keyword in normalized for keyword in OUT_OF_SCOPE_KEYWORDS) and not any(
keyword in normalized for keyword in SPACE_KEYWORDS
):
return "out_of_scope"
if detect_sound_topic(normalized, session):
return "space_sounds"
if any(token in normalized for token in ("next pass", "when can i see", "visible", "see the iss")):
return "iss_next_pass"
if any(token in normalized for token in ("where is the iss", "where is the space station", "iss location")):
return "iss_location"
if any(token in normalized for token in ("what is above me", "what is in my sky", "sky report")):
return "sky_report"
if any(token in normalized for token in SPACE_KEYWORDS):
return "general_space_qa"
if normalized.endswith("?"):
return "general_space_qa"
return "unknown"
```


This keeps the high-confidence paths fast and predictable.


For example:


- "Where is the space station?" goes to the ISS location API.
- "When can I see the ISS from Chicago?" goes to location capture and N2YO.
- "What is in my sky tonight?" goes to Skyfield.
- "Can I hear a black hole?" goes to a curated audio clip.
- "Why is Mars red?" goes to OpenAI.


The result feels more reliable than sending every transcript to an LLM and hoping the model chooses the right tool.


## Send voice-safe text back to Twilio


Voice apps need text that sounds good when read aloud. URLs, Markdown, bullets, slashes, and punctuation-heavy formatting can become awkward.


I added a small cleanup layer before sending text back to Conversation Relay:


Copy code


```text
def sanitize_tts_text(text: str) -> str:
cleaned = text.replace("\n", " ")
cleaned = re.sub(r"\[([^\]]+)\]\([^)]+\)", r"\1", cleaned)
cleaned = re.sub(r"https?://\S+", "", cleaned)
cleaned = re.sub(r"[`*_#~]", "", cleaned)
cleaned = re.sub(r"(?<=\w)/(?=\w)", " and ", cleaned)
cleaned = re.sub(r"(?<=\d)\.(?=\d)", " point ", cleaned)
cleaned = re.sub(r"\s{2,}", " ", cleaned).strip()
return cleaned
```


Then the app sends a Conversation Relay text message:


Copy code


```text
await websocket.send_text(
json.dumps(
{
"type": "text",
"token": chunk,
"last": idx == len(voice_chunks) - 1,
}
)
)
```


I also estimate the text-to-speech duration before starting the silence timer. That prevents the app from asking "Are you still there?" while the caller is still listening to the previous answer.


## Build the ISS location path


The ISS path uses Open Notify for the station's current latitude and longitude, then tries to reverse geocode the result into a more human-friendly location.


Copy code


```text
async def get_iss_location() -> dict[str, Any]:
async with httpx.AsyncClient(timeout=5.0) as client:
response = await client.get(ISS_API_URL)
response.raise_for_status()
data = response.json()
position = data.get("iss_position", {})
lat = position.get("latitude")
lon = position.get("longitude")
return {
"timestamp": data.get("timestamp"),
"latitude": lat,
"longitude": lon,
"location_summary": "the open ocean",
}
```


In the full app, I also call Nominatim to convert the coordinates into a location summary. Sometimes the ISS is over land, and sometimes it is over the ocean, so the response needs to handle both.


The spoken response adds a fact and a follow-up:


*Great question. The International Space Station is currently over the open ocean*


*at latitude 12.34 and longitude 56.78. It is traveling about seventeen thousand*


*five hundred miles per hour and completes an orbit around Earth in roughly*


*ninety minutes. Want to know when the next pass is, or would you like to explore*


*something else?*


That pattern shows up throughout the app: answer the question, add one interesting detail, then invite the next turn.


## Ask for location only when needed


For local sky reports and ISS pass predictions, the app needs a city. I did not want to ask for location at the start of every call, because many callers may only want general space facts or sounds.


Instead, the app waits until a feature actually needs it:


Copy code


```text
async def handle_iss_next_pass(websocket: WebSocket, call_sid: str) -> None:
session = sessions[call_sid]
location = session.get("location")
if not location:
session["awaiting_location_for"] = "iss_next_pass"
await send_text_chunks(
websocket,
call_sid,
"I can tell you exactly when it will be visible from your location. Where are you calling from? You can say your city and state.",
prompt_type="request_location_iss",
current_path="iss_next_pass",
expect_response=True,
)
return
```


The next user prompt is handled by handle_location_capture().


If the caller says "Chicago, Illinois", the app geocodes the location, stores it in the session, and continues the original path.


This makes the conversation feel more natural because location capture is contextual.


## Predict the next visible ISS pass


Once the app has latitude and longitude, it calls the N2YO visual passes endpoint:


Copy code


```text
async def get_iss_next_pass(lat: float, lon: float, tz_name: str | None) -> dict[str, Any]:
url = (
f"{N2YO_BASE_URL.rstrip('/')}/rest/v1/satellite/visualpasses/25544/"
f"{lat}/{lon}/{N2YO_OBSERVER_ALT_KM}/{N2YO_LOOKAHEAD_DAYS}/{N2YO_MIN_VIS_SECONDS}"
)


async with httpx.AsyncClient(timeout=8.0) as client:
response = await client.get(url, params={"apiKey": N2YO_API_KEY})
response.raise_for_status()
payload = response.json()


passes = payload.get("passes") or []
if not passes:
return {"error": "No visible ISS pass found in the current look-ahead window."}


entry = passes[0]
return {
"start_time_local": parse_iso_datetime(entry.get("startUTC"), tz_name),
"duration_min": round((entry.get("duration") or 0) / 60, 1),
"max_elevation_deg": entry.get("maxEl"),
"start_direction": entry.get("startAzCompass", "").lower(),
"end_direction": entry.get("endAzCompass", "").lower(),
}
```


Then, the hotline says something like:


Copy code


```text
Look northwest first and track it toward southeast. It should be visible for
about 4.8 minutes and reach roughly 61 degrees above the horizon.
```


For a voice app, that is the right amount of detail. It gives the caller enough to go outside and try it without reading a chart.


## Build a local sky report


The sky report uses Skyfield and a local` de421.bsp` ephemeris file to estimate what is overhead and which planets are visible.


Copy code


```text
async def get_sky_report(lat: float, lon: float, tz_name: str | None) -> dict[str, Any]:
ephemeris, timescale, constellation_map = _load_skyfield_assets()


zone = safe_zoneinfo(tz_name)
local_now = datetime.now(zone)
t = timescale.from_datetime(local_now.astimezone(timezone.utc))


earth = ephemeris["earth"]
observer = wgs84.latlon(lat, lon)
observer_at = (earth + observer).at(t)


zenith = observer_at.from_altaz(alt_degrees=90.0, az_degrees=0.0)
ra, dec, _ = zenith.radec()
constellation_code = constellation_map(ra, dec)
```


The app also checks a short list of planets and includes a nearby meteor shower if one is coming up.


The response is intentionally concise:


*Here is what is happening in the sky above Austin, Texas tonight.*


*The constellation Orion is near overhead, and Orion is one of the easiest*


*constellations to spot because of its bright belt stars. You should be able*


*to spot Jupiter toward the eastern sky after sunset, and Saturn in the southern*


*sky later tonight.*


This is where voice UX matters again. The app is not trying to read an astronomy table. It is giving the caller one useful thing to look for.


## Add curated space sounds


This was my favorite part of the project.


Conversation Relay can send a` play` message with an audio source URL. I used that to add curated space sounds for across the universe, including[NASA’s sonification of black-hole pressure waves](https://chandra.si.edu/sound/perseus) ,[radio emissions recorded by Juno during its 2021 Ganymede flyby](https://science.nasa.gov/photojournal/audio-of-junos-ganymede-flyby/) ,[pulsar signals from Jodrell Bank Observatory](https://www.jb.man.ac.uk/~pulsar/Education/Sounds/) , and[the sound of the cosmic microwave background](https://faculty.washington.edu/jcramer/BBSound_2013) . The clips are hosted online and optimized for clear playback over a phone call.


Copy code


```text
async def send_play(
websocket: WebSocket,
source_url: str,
*,
loop: int = 1,
preemptible: bool = False,
interruptible: bool = True,
) -> None:
await websocket.send_text(
json.dumps(
{
"type": "play",
"source": source_url,
"loop": loop,
"preemptible": preemptible,
"interruptible": interruptible,
}
)
)
```


For space sounds, I set interruptible to False so the clip does not get cut off by ambient noise. Then, I schedule a delayed follow-up after the clip has had time to play:


Copy code


```text
session["pending_audio_task"] = asyncio.create_task(
send_space_sound_outro_after_delay(websocket, call_sid, outro, play_seconds)
)
```


If the caller interrupts or says something new, that pending audio task is cancelled cleanly.


I also added a small` /media/jupiter-8k.wav` endpoint that transcodes a Jupiter WAV into a telephony-friendly 8 kHz mono WAV. That helped make playback more reliable over a phone call.


## Use OpenAI for general space questions


For general space Q and A, the app sends the caller's prompt to OpenAI with a voice-safe system prompt:


Copy code


```text
SYSTEM_PROMPT = (
"You are the Orbital Hotline host. Keep responses conversational and concise: two to four sentences. "
"Include one interesting or surprising fact when possible. End with a clear follow-up invitation or question, "
"unless explicitly ending the call. Focus on space and astronomy topics only. "
"Output must be voice safe: do not say or spell punctuation symbols, do not output standalone punctuation tokens, "
"and avoid formats that are read aloud as symbols."
)
```


The app stores the conversation history in the session:


Copy code


```text
async def generate_general_space_answer(session: dict[str, Any], user_text: str, topics: list[str]) -> str:
session["messages"].append({"role": "user", "content": user_text})


completion = await openai.chat.completions.create(
model=MODEL,
messages=session["messages"],
)
content = completion.choices[0].message.content or ""


session["messages"].append({"role": "assistant", "content": content})
session["messages"] = trim_messages(session["messages"], MAX_TURNS)


return content
```


The app also detects topics like Mars, Jupiter, Saturn, black holes, the Moon, and exoplanets. If the caller says "yes" after a topic answer, the hotline can go deeper on the same topic instead of treating "yes" as unknown input.


For black hole questions, the app offers a related sound clip:


*Want to hear what a black hole sounds like, or explore something else?*


That kind of handoff makes the AI path feel connected to the rest of the hotline.


## Handle silence and unknown prompts


Voice calls need recovery paths.


If the caller says something the router cannot classify, the first miss asks them to repeat:


*I may have missed that. Can you repeat your question?*


If the app misses again, it falls back to the menu:


*You can ask me where the space station is, what is in your sky tonight,*


*hear sounds from across the universe, or ask questions about any planet,*


*star, or mission. What interests you?*


Silence handling works similarly. The app waits until after the estimated TTS playback window, then starts checking whether the caller has responded.


Copy code


```text
async def run_silence_watchdog(websocket: WebSocket, call_sid: str) -> None:
while True:
await asyncio.sleep(0.5)
session = sessions.get(call_sid)
if not session or not session.get("awaiting_user"):
continue


deadline = session.get("silence_deadline")
if not deadline or datetime.now(timezone.utc) < deadline:
continue


# Reprompt, then eventually end the call.
```


This part is easy to overlook, but it makes a big difference. Without it, a demo can feel broken as soon as the caller pauses, talks over the assistant, or gets background noise in the transcript.


## Add a stats dashboard


I also added` /stats` and` /stats.json` endpoints backed by the Twilio Calls API.


The dashboard tracks:


- Total calls
- Average call duration
- Longest call duration
- Caller country counts
- Date filters
- Rolling period filters


The stats path is intentionally separate from the live call loop. I did not want the WebSocket path to write counters or add latency during the call. Instead, the stats page reads from Twilio's REST API and caches results briefly in memory.


That means the voice app stays focused on the caller, and the dashboard can evolve independently.


## Run the app locally


Start your tunnel:


Copy code


```text
ngrok http 8080
```


Copy the forwarding domain into *.env* :


Copy code


```text
NGROK_URL="your-ngrok-domain.ngrok-free.app"
```


Then run the FastAPI app:


Copy code


```text
python main.py
```


The server starts on port` 8080` by default.


## Configure Twilio


In the Twilio Console, open your phone number's Voice settings and set the incoming call webhook to:


Copy code


```text
https://your-ngrok-domain.ngrok-free.app/twiml
```


Use` HTTP POST` .


Now call your Twilio number. You should hear the Orbital Hotline greeting, then you can try prompts like:


*Where is the space station?*


*When can I see the ISS from Seattle?*


*What is in my sky tonight?*


*Can I hear sounds from space?*


*Tell me about black holes.*


And Voila! You are now talking to your own tiny mission control desk.


## Deploying it


For my deployment, I containerized the app with Docker:


Copy code


```text
docker build -t orbital-hotline:latest
docker run --rm -p 8080:8080 --env-file .env orbital-hotline:latest
```


For a hosted deployment, make sure your environment supports long-lived WebSocket connections and public HTTPS. I also recommend keeping at least one warm instance for demos so the first caller does not hit a cold start.


For this project, the container runs on[Azure Container Apps](https://learn.microsoft.com/azure/container-apps/overview) which fits a WebSocket workload like this one well: it terminates public HTTPS, keeps long-lived connections open, and handles autoscaling and load balancing for you. The Orbital Hotline runs with a minimum of one replica so a caller never waits on a cold start, and each replica is sized at 2 vCPU and 4 GB of RAM to comfortably handle the concurrent WebSocket, audio, and astronomy-API work during a call. Because call session state lives in memory, the app is pinned to a single replica for now; scaling out horizontally would mean moving that state into a shared store first.


**Note:** The current app stores call session state in memory. If you run multiple replicas, use sticky routing for the lifetime of a call or move session state into a shared store like Redis.


## What's next?


This project started as a fun way to make space data feel conversational, but the same pattern works for many voice apps:


- Use Conversation Relay for the live phone call
- Use deterministic routing for known tasks
- Use APIs for facts that should be fresh
- Use an LLM for open-ended questions
- Keep all output voice-safe
- Add recovery paths for silence, interruptions, and unknown prompts


The next things I would add are richer observability, a better admin view for call transcripts, more curated audio, and function-style tool calls for space data instead of a hand-written router.


But even in this version, the experience is simple: call a number, ask about space, and get an answer you can actually use.


That's the[Orbital Hotline](https://github.com/rishabkumar7/space-voice-hotline/) . Now it's your turn to build something that makes the cosmos feel a little closer. I can't wait to see what you build!


*Rishab Kumar is a Developer Evangelist at Twilio and a cloud enthusiast. Get in touch with Rishab on Twitter*[@rishabincloud](https://x.com/rishabincloud) *and follow his cloud, DevOps, and DevRel adventures on YouTube at*[youtube.com/@rishabincloud](http://youtube.com/@rishabincloud) *.*
