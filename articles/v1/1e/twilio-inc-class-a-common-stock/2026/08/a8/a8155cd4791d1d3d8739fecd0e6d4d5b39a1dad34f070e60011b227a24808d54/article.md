---
schema_version: "1.0.0"
document_id: "a8155cd4791d1d3d8739fecd0e6d4d5b39a1dad34f070e60011b227a24808d54"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/add-ai-voice-assistant-twilio-video-room-conversation-relay"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T16:59:47.521529+00:00"
fetched_at: "2026-08-19T16:59:49.632670+00:00"
content_hash: "sha256:0854c13c4d04d187ee3143060581a5d89648514053ff4d1b7cd81a4850e16e52"
---

# Add an AI Voice Assistant to a Twilio Video Room with Conversation Relay

Time to read:


-
-
-
-
-


August 18, 2026


**Written by**[Donal Toomey](https://www.twilio.com/en-us/blog/authors/author.dtoomey) Twilion


**Reviewed by**[Dhruv Patel](https://www.twilio.com/en-us/blog/authors/author.dhrpatel) Twilion


---


## Add an AI Voice Assistant to a Twilio Video Room with Conversation Relay


Telehealth applications often waste patient time in empty waiting rooms before a provider joins. An AI voice assistant can use that time productively by collecting intake information: reason for visit, symptom updates, and current medications.


This tutorial shows you how to add an AI voice assistant to a Twilio Video room. The AI joins as an audio-only participant, listens to the patient speak, and responds with natural speech. The solution combines[Twilio Programmable Video](https://www.twilio.com/docs/video) with[Conversation Relay](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) , a Twilio feature that manages the full complexity of real-time voice AI interactions. It handles speech-to-text transcription, text-to-speech synthesis, interruption detection, and turn-taking, all over a single WebSocket connection to your server. Your application just receives transcribed text and sends back text responses.


By the end, you'll have a working application where a patient can join a video room, interact with an AI intake assistant through natural conversation, and then be joined by a provider. The complete source code is available on GitHub if you'd like to jump ahead.


## Prerequisites


Before you get started, make sure you have the following:


- A Twilio account upgraded from Trial.[Sign up for an account here](https://login.twilio.com/u/signup) .
- [Node.js](https://nodejs.org/) v18 or higher installed
- npm (comes with Node.js)
- [ngrok](https://ngrok.com/) installed and authenticated
- An[OpenAI API key](https://platform.openai.com/api-keys) (or another LLM provider; this tutorial uses OpenAI, but the WebSocket handler is provider-agnostic)
- A Twilio phone number (any number on your account will work)


You'll also need your Twilio API Key and API Secret. If you haven't created one yet, head to the[Twilio Console API Keys page](https://www.twilio.com/console/project/api-keys) and create a new Standard key. Save both the SID (starts with SK) and the Secret.


## How the architecture works


Before writing any code, it helps to understand the approach at a high level. The key insight is that[Twilio Programmable Voice](https://www.twilio.com/docs/voice) calls can join a Video room as audio-only participants. By creating a two-leg Voice call, you bridge the Video room to Conversation Relay: one leg sits in the room hearing (and being heard by) all participants, while the other leg connects to Conversation Relay, which streams transcribed audio to your WebSocket server and speaks the LLM's responses back. Your server just needs to handle the WebSocket messages and talk to an LLM.


## Build the app


### Step 1: Set up the project


Create a new directory for your project and initialize it with npm.


Copy code


```text
mkdir video-cr-demo
cd video-cr-demo
npm init -y
```


Install the dependencies you'll need: Express for the web server, the Twilio helper library, the OpenAI SDK, the` ws` package for WebSocket support, and` dotenv` for environment variables.


Copy code


```text
npm install express twilio openai ws dotenv
```


For development, you can optionally install` nodemon` to automatically restart the server when you make changes:


Copy code


```text
npm install --save-dev nodemon
```


Open *package.json* and update the` scripts` section so that` npm start` runs your server:


Copy code


```text
"scripts": {
"start": "node server.js",
"dev": "nodemon server.js"
}
```


Now create a *.env* file in the root of your project to store your credentials. Add the following, replacing the placeholder values with your actual keys:


Copy code


```text
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_API_KEY=SKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_API_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_PHONE_NUMBER=+1xxxxxxxxxx
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWIML_APP_SID=
SERVER_URL=https://your-ngrok-url.ngrok.app
PORT=3000
```


Leave` TWIML_APP_SID` empty for now. You'll generate it with a setup script in the next section.


### Step 2: Create the TwiML App with a setup script


Conversation Relay needs a TwiML App to know where to route the B-leg of the call. Instead of creating this manually in the Twilio Console, you can write a quick script that creates it programmatically and writes the SID back into your *.env* file.


Create a file called *setup.js* in the root of your project:


Copy code


```text
require('dotenv').config();
const twilio = require('twilio');
const fs = require('fs');
const path = require('path');
async function main() {
const missing = ['TWILIO_ACCOUNT_SID', 'TWILIO_API_KEY', 'TWILIO_API_SECRET', 'SERVER_URL']
.filter(k => !process.env[k]);
if (missing.length) {
console.error('Missing required .env vars:', missing.join(', '));
process.exit(1);
}
const client = twilio(process.env.TWILIO_API_KEY, process.env.TWILIO_API_SECRET, {
accountSid: process.env.TWILIO_ACCOUNT_SID,
});
const voiceUrl = `${process.env.SERVER_URL}/voice-handler`;
console.log(`Creating TwiML App with VoiceUrl: ${voiceUrl}`);
const app = await client.applications.create({
friendlyName: 'video-cr-demo',
voiceUrl,
voiceMethod: 'POST',
});
console.log(`TwiML App created: ${app.sid}`);
const envPath = path.join(__dirname, '.env');
let envContent = fs.readFileSync(envPath, 'utf8');
if (envContent.includes('TWIML_APP_SID=')) {
envContent = envContent.replace(/^TWIML_APP_SID=.*$/m, `TWIML_APP_SID=${app.sid}`);
} else {
envContent += `\nTWIML_APP_SID=${app.sid}\n`;
}
fs.writeFileSync(envPath, envContent);
console.log('TWIML_APP_SID written to .env');
}
main().catch(err => { console.error(err.message); process.exit(1); });
```


This script creates a TwiML App whose Voice URL points to your server's` /voice-handler` endpoint. When the bridge call is placed, Twilio will hit that URL to get TwiML instructions for the B-leg.


Don't run this script yet. You'll need ngrok running first, which you'll set up after the server is built.


### Step 3: Build the Express server


Now for the main event. Create a file called *server.js* . This file will contain your Express app, the API routes for token generation and call management, and the WebSocket handler for the AI.


Start by setting up the imports and Express app:


Copy code


```text
require('dotenv').config();
const express = require('express');
const http = require('http');
const WebSocket = require('ws');
const twilio = require('twilio');
const { AccessToken } = require('twilio').jwt;
const { VideoGrant } = AccessToken;
const OpenAI = require('openai');
const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(express.static('public'));
const twilioClient = twilio(process.env.TWILIO_API_KEY, process.env.TWILIO_API_SECRET, {
accountSid: process.env.TWILIO_ACCOUNT_SID,
});
const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });
```


Here you're initializing Express, the Twilio client (using API Key authentication), and the OpenAI client. The` express.static('public')` line will serve your frontend files from a` public` directory.


### Step 4: Generate Video access tokens


Your frontend needs a short-lived access token to connect to a Video room. Add this route to *server.js* :


Copy code


```text
app.get('/token/video', (req, res) => {
const identity = req.query.identity || 'user';
const roomName = req.query.room || 'demo-room';
const token = new AccessToken(
process.env.TWILIO_ACCOUNT_SID,
process.env.TWILIO_API_KEY,
process.env.TWILIO_API_SECRET,
{ identity }
);
token.addGrant(new VideoGrant({ room: roomName }));
res.json({ token: token.toJwt(), identity, room: roomName });
});
```


The access token is a short-lived credential that grants a specific user access to a specific Video room. The` VideoGrant` scopes the token so it can only be used for the room you specify.


### Step 5: Create the TwiML endpoints


You need two TwiML endpoints: one for the A-leg (joining the Video room) and one for the B-leg (connecting to Conversation Relay).


Add both to *server.js* :


Copy code


```text
// B-leg: routes through Conversation Relay to the AI WebSocket
app.post('/voice-handler', (req, res) => {
const wsUrl = `${process.env.SERVER_URL.replace(/^https/, 'wss')}/ai-ws`;
const twiml = `<?xml version="1.0" encoding="UTF-8"?>
<Response>
<Connect>
<ConversationRelay url="${wsUrl}" welcomeGreeting="Hello, how can I help you today?" />
</Connect>
</Response>`;
res.type('text/xml').send(twiml);
});
// A-leg: joins the Video Room as an audio-only participant
app.post('/join-room', (req, res) => {
const roomName = req.query.room || 'demo-room';
const twiml = `<?xml version="1.0" encoding="UTF-8"?>
<Response>
<Connect>
<Room participantIdentity="cr-ai-agent">${roomName}</Room>
</Connect>
</Response>`;
res.type('text/xml').send(twiml);
});
```


The` /voice-handler` endpoint is called by Twilio when the TwiML App is triggered. It responds with TwiML that tells Twilio to connect the call to Conversation Relay, pointing at your WebSocket URL. The` welcomeGreeting` attribute is what the AI will say when it first joins.


The` /join-room` endpoint tells the A-leg to join the Video room with the identity` cr-ai-agent` . Since it's joining via a Voice call, it automatically becomes an audio-only participant.


### Step 6: Invite the AI agent into the room


This is where the two-leg bridge call gets created. When the frontend sends a request to invite the AI, your server places a call using the Twilio REST API.


Add the` /invite-ai` endpoint to *server.js* :


Copy code


```text
app.post('/invite-ai', async (req, res) => {
const roomName = req.body.room || 'demo-room';
try {
const call = await twilioClient.calls.create({
from: process.env.TWILIO_PHONE_NUMBER,
to: `app:${process.env.TWIML_APP_SID}`,
url: `${process.env.SERVER_URL}/join-room?room=${encodeURIComponent(roomName)}`,
});
res.json({ callSid: call.sid });
} catch (err) {
console.error('invite-ai error:', err.message);
res.status(500).json({ error: err.message });
}
});
```


Let's break down what happens here:


- ` from` : Your Twilio phone number (required for any outgoing call).
- ` to` : The` app:` prefix tells Twilio to route the B-leg through the specified TwiML App. Twilio will fetch TwiML from the app's Voice URL (` /voice-handler` ), which connects to Conversation Relay.
- ` url` : The TwiML URL for the A-leg, which joins the Video room.


The result is a single bridged call where one end is in the Video room and the other end is connected to Conversation Relay.


### Step 7: Add cleanup logic


When a participant leaves the room, you'll want to end the bridge call and optionally complete the Video room. Add a` /cleanup` endpoint:


Copy code


```text
app.post('/cleanup', async (req, res) => {
const { callSid, roomName } = req.body;
const results = {};
if (callSid) {
try {
await twilioClient.calls(callSid).update({ status: 'completed' });
results.callEnded = true;
} catch (err) {
results.callError = err.message;
}
}
if (roomName) {
try {
const rooms = await twilioClient.video.v1.rooms.list({
uniqueName: roomName,
status: 'in-progress',
limit: 1,
});
if (rooms.length) {
await twilioClient.video.v1.rooms(rooms[0].sid).update({ status: 'completed' });
results.roomCompleted = true;
}
} catch (err) {
results.roomError = err.message;
}
}
res.json(results);
});
```


### Step 8: Handle the AI WebSocket


This is where Conversation Relay sends transcribed speech and where you respond with LLM-generated text. Add the WebSocket server at the bottom of *server.js* :


Copy code


```text
const server = http.createServer(app);
const wss = new WebSocket.Server({ server, path: '/ai-ws' });
wss.on('connection', (ws) => {
const history = [];
ws.on('message', async (raw) => {
let msg;
try { msg = JSON.parse(raw); } catch { return; }
if (msg.type === 'connected') {
console.log('ConversationRelay connected:', msg.sessionId);
return;
}
if (msg.type === 'prompt') {
const userText = msg.voicePrompt;
console.log('User:', userText);
history.push({ role: 'user', content: userText });
try {
const completion = await openai.chat.completions.create({
model: 'gpt-4o-mini',
messages: [
{
role: 'system',
content: 'You are a helpful voice assistant. Keep responses concise and conversational — two or three sentences at most.',
},
...history,
],
});
const reply = completion.choices[0].message.content;
history.push({ role: 'assistant', content: reply });
console.log('Assistant:', reply);
ws.send(JSON.stringify({ type: 'text', token: reply }));
} catch (err) {
console.error('OpenAI error:', err.message);
ws.send(JSON.stringify({ type: 'text', token: 'Sorry, I had trouble with that. Could you try again?' }));
}
return;
}
if (msg.type === 'interrupt') {
return;
}
if (msg.type === 'disconnected') {
console.log('ConversationRelay disconnected:', msg.reason);
ws.close();
}
});
ws.on('error', (err) => console.error('WebSocket error:', err.message));
});
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
```


Conversation Relay sends several message types over the WebSocket:


- ` connected` : The session has started. You get a` sessionId` for logging.
- ` prompt` : Contains` voicePrompt` , which is the transcribed speech from the room. This is where you call your LLM and send the response back.
- ` interrupt` : The user started speaking while the AI was still responding. You can use this to cancel in-progress LLM calls if needed.
- ` disconnected` : The session ended.


To respond, you send a JSON message with` type: 'text'` and a` token` field containing the text you want spoken. Conversation Relay handles the text-to-speech conversion automatically.


**Note:** This tutorial uses OpenAI's` gpt-4o-mini` model, but the WebSocket handler is provider-agnostic. You can swap in Anthropic's Claude, Google's Gemini, or any other LLM by replacing the` openai.chat.completions.create` call with your provider's equivalent.


### Step 9: Build the frontend


Create a *public* directory and add an *index.html* file inside it. This will be the interface your users interact with.


Copy code


```text
mkdir public
```


Create public/index.html with the HTML structure and styles:


Copy code


```text
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Video + Conversation Relay</title>
<style>
body { font-family: sans-serif; max-width: 800px; margin: 40px auto; padding: 0 20px; }
h1 { font-size: 1.2rem; margin-bottom: 20px; }
.controls { display: flex; gap: 8px; margin-bottom: 20px; flex-wrap: wrap; }
button { padding: 10px 20px; font-size: 0.95rem; cursor: pointer; }
button:disabled { opacity: 0.4; cursor: default; }
#status { padding: 10px; background: #f4f4f4; border-radius: 4px; font-size: 0.9rem; margin-bottom: 16px; }
#participants { display: flex; flex-wrap: wrap; gap: 12px; }
.participant {
background: #222; color: #fff; border-radius: 6px; padding: 8px;
text-align: center; min-width: 160px;
border: 3px solid transparent;
transition: border-color 0.15s ease, box-shadow 0.15s ease;
}
.participant video { width: 270px; height: 360px; background: #000; display: block; border-radius: 4px; transform: scaleX(-1); object-fit: cover; }
.participant .label { font-size: 0.75rem; margin-top: 4px; opacity: 0.8; }
#log { margin-top: 20px; font-size: 0.78rem; color: #555; white-space: pre-wrap; max-height: 180px; overflow-y: auto; background: #fafafa; padding: 8px; border-radius: 4px; }
</style>
</head>
<body>
<h1>Video + Conversation Relay</h1>
<div id="status">Idle</div>
<div class="controls">
<button id="btn-join">Join Room</button>
<button id="btn-invite-ai" disabled>Invite AI Agent</button>
<button id="btn-leave" disabled>Leave Room</button>
</div>
<div id="participants"></div>
<div id="log"></div>
<script src="https://sdk.twilio.com/js/video/releases/2.28.1/twilio-video.min.js"></script>
```


The page loads the Twilio Video JS SDK from the CDN. The UI consists of three buttons (Join, Invite AI, Leave), a status bar, a container for participant video tiles, and a log panel.


Now add the JavaScript. Still inside *public/index.html* , below the SDK script tag, add:


Copy code


```text
<script>
const ROOM_NAME = 'demo-room';
const btnJoin = document.getElementById('btn-join');
const btnInviteAI = document.getElementById('btn-invite-ai');
const btnLeave = document.getElementById('btn-leave');
const statusEl = document.getElementById('status');
const participantsEl = document.getElementById('participants');
const logEl = document.getElementById('log');
let room = null;
let aiInvited = false;
let bridgeCallSid = null;
function log(msg) {
const ts = new Date().toLocaleTimeString();
logEl.textContent += `[${ts}] ${msg}\n`;
logEl.scrollTop = logEl.scrollHeight;
}
function setStatus(msg) {
statusEl.textContent = msg;
log(msg);
}
```


These are your utility functions and state variables. The` room` variable will hold the Twilio Video Room instance,` aiInvited` tracks whether the AI has been invited, and` bridgeCallSid` stores the call SID so you can end it during cleanup.


Next, add the functions to manage participant tiles:


Copy code


```text
function addParticipantTile(participant) {
if (participant.identity === 'cr-ai-agent' && !aiInvited) return;
const tile = document.createElement('div');
tile.className = 'participant';
tile.id = `p-${participant.sid}`;
const label = document.createElement('div');
label.className = 'label';
label.textContent = participant.identity;
tile.appendChild(label);
participantsEl.appendChild(tile);
log(`Participant joined: ${participant.identity}`);
participant.tracks.forEach(pub => { if (pub.track) attachTrack(tile, pub.track); });
participant.on('trackSubscribed', track => attachTrack(tile, track));
participant.on('trackUnsubscribed', track => {
if (track.detach) track.detach().forEach(el => el.remove());
});
}
function removeParticipantTile(participant) {
const tile = document.getElementById(`p-${participant.sid}`);
if (tile) tile.remove();
log(`Participant left: ${participant.identity}`);
}
function attachTrack(tile, track) {
if (track.kind === 'audio' || track.kind === 'video') {
tile.appendChild(track.attach());
}
}
```


These functions create a visual tile for each participant, attach their audio and video tracks, and clean up when someone leaves. Notice the guard at the top of` addParticipantTile` : it prevents showing the AI agent's tile before you've explicitly invited it.


Now add the room join logic:


Copy code


```text
btnJoin.addEventListener('click', async () => {
btnJoin.disabled = true;
setStatus('Fetching video token...');
const identity = `user-${Math.random().toString(36).slice(2, 6)}`;
const res = await fetch(`/token/video?identity=${identity}&room=${ROOM_NAME}`);
const { token } = await res.json();
setStatus('Connecting to room...');
try {
const localTracks = await Twilio.Video.createLocalTracks({ audio: true, video: { width: 640 } });
room = await Twilio.Video.connect(token, {
name: ROOM_NAME,
tracks: localTracks,
});
setStatus(`Joined room: ${room.name}`);
// Show local participant
const localTile = document.createElement('div');
localTile.className = 'participant';
localTile.id = 'p-local';
const localLabel = document.createElement('div');
localLabel.className = 'label';
localLabel.textContent = `${room.localParticipant.identity} (you)`;
localTile.appendChild(localLabel);
localTracks.forEach(track => {
if (track.kind === 'video') localTile.appendChild(track.attach());
});
participantsEl.appendChild(localTile);
// Handle existing and new participants
room.participants.forEach(p => addParticipantTile(p));
room.on('participantConnected', p => addParticipantTile(p));
room.on('participantDisconnected', removeParticipantTile);
room.on('disconnected', () => {
setStatus('Left room');
participantsEl.innerHTML = '';
btnJoin.disabled = false;
btnLeave.disabled = true;
btnInviteAI.disabled = true;
aiInvited = false;
room = null;
});
btnInviteAI.disabled = false;
btnLeave.disabled = false;
} catch (err) {
setStatus(`Error: ${err.message}`);
btnJoin.disabled = false;
}
});
```


This fetches a token from your server, creates local audio and video tracks, connects to the room, and sets up event listeners for participants joining and leaving.


Finally, add the AI invite and leave handlers:


Copy code


```text
btnInviteAI.addEventListener('click', async () => {
btnInviteAI.disabled = true;
aiInvited = true;
setStatus('Inviting AI Agent...');
try {
const res = await fetch('/invite-ai', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({ room: ROOM_NAME }),
});
const { callSid, error } = await res.json();
if (error) throw new Error(error);
bridgeCallSid = callSid;
setStatus('AI agent invited — joining room shortly...');
} catch (err) {
setStatus(`Invite failed: ${err.message}`);
btnInviteAI.disabled = false;
aiInvited = false;
}
});
btnLeave.addEventListener('click', async () => {
if (room) {
setStatus('Cleaning up...');
try {
await fetch('/cleanup', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({ callSid: bridgeCallSid, roomName: ROOM_NAME }),
});
} catch (err) {
log(`Cleanup error: ${err.message}`);
}
bridgeCallSid = null;
room.disconnect();
}
});
</script>
</body>
</html>
```


The **Invite AI Agent** button sends a POST to` /invite-ai` , which triggers the bridge call. After a brief moment, the AI agent appears as a participant in the room. The **Leave Room** button calls` /cleanup` to end the bridge call and complete the Video room, then disconnects the local participant.


## Run and test the application


Now let's put it all together. Open a terminal and start ngrok to create a public URL for your local server:


Copy code


```text
ngrok http 3000
```


Copy the HTTPS URL (it will look something like` https://abc123.ngrok.app` ) and paste it as the` SERVER_URL` value in your *.env* file.


Next, run the setup script to create your TwiML App:


Copy code


```text
node setup.js
```


You should see output confirming the TwiML App was created and the TWIML_APP_SID was written to your .env file.


Now start the server:


Copy code


```text
npm start
```


Open your browser and navigate to` http://localhost:3000` . You should see the application with three buttons.


- Click **Join Room** . Your browser will ask for camera and microphone permissions. After granting them, you'll see your video feed appear.
- Click **Invite AI Agent** . After a moment, a new participant tile labeled` cr-ai-agent` will appear. You'll hear the AI greet you with "Hello, how can I help you today?"
- Speak naturally. The AI will listen, transcribe your speech, process it through GPT-4o-mini, and respond with spoken audio.
- Click **Leave Room** when you're done.


### Troubleshooting


If things aren't working as expected, here are some common issues:


- **AI agent never joins the room.** Check that your` SERVER_URL` in *.env* matches your current ngrok URL. Ngrok generates a new URL each time you restart it, so you'll need to update *.env* and re-run` node setup.js` to update the TwiML App's Voice URL.
- **"invite-ai error" in the server console.** Verify that` TWIML_APP_SID` is populated in your *.env* file. If it's empty, run node setup.js again.
- **AI joins but doesn't respond to speech.** Confirm your` OPENAI_API_KEY` is valid and has available credits. Check the server console for "OpenAI error" messages.
- **Browser denies camera/microphone access.** Ensure you're accessing the app via` localhost` or an HTTPS URL. Browsers block media access on plain HTTP.
- **"Cannot connect to room" error.** Your Twilio API Key may not have Video permissions, or the key/secret pair may be incorrect. Double-check the values in .env against the Twilio Console.


## Conclusion


Congratulations! You've built a video application where an AI voice assistant joins a Twilio Video room via a bridged Voice call and Conversation Relay. The same pattern works for any scenario where you want an AI participant in a video call, from customer support triage to automated meeting summaries. To explore further, try customizing the system prompt for a specific use case, swapping in a different LLM provider, or adding a real-time transcript to the UI. The complete source code is available at[https://github.com/donaltoomey/twilio-video-cr-demo](https://github.com/donaltoomey/twilio-video-cr-demo) .


### Additional resources


- [Conversation Relay documentation](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay)
- [Twilio Programmable Video documentation](https://www.twilio.com/docs/video)
- [Twilio Video JavaScript SDK guide](https://www.twilio.com/docs/video/javascript-getting-started)
- [TwiML Connect verb reference](https://www.twilio.com/docs/voice/twiml/connect)
