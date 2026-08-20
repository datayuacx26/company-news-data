---
schema_version: "1.0.0"
document_id: "8699662165a4d6e0b61e282639943c6dc5a4f54cc2ffcbd7a55b1696666a9db4"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-less-robotic-nodejs"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T13:09:03.715655+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:9daf6611cb14b8576e90cdac1467812e8bb4236b3c6abd2c9c398b93aca23b57"
---

# How to Make Your AI Voice Sound More Human and Less Robotic with Node.js

Time to read:


-
-
-
-
-


July 22, 2026


**Written by**[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


[Dhruv Patel](https://www.twilio.com/en-us/blog/authors/author.dhrpatel) Twilion


---


## How to Make Your AI Voice Sound More Human and Less Robotic with Node.js


If you've called a customer service helpline recently, chances are good that you've encountered some kind of AI assistant voice helper. But not all AI helpers are created equally. Some AI assistants are truly a pain to deal with, with long latency that creates a frustrating conversation. Others are more human-like, with a pleasant tone and cadence and the ability to handle your requests and interruptions gracefully.


What's the difference? If you want your AI to sound more human and less robotic on the phone, having a great simulated voice really helps. But aside from that, creating a low-latency bot that handles interruptions gracefully can be a big benefit to your users and make them feel more at ease.


In this tutorial, you will build a real-time voice agent using Node.js,[Twilio Conversation Relay,](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) and[OpenAI](https://openai.com/) . In[previous tutorials](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) , we've created an AI that operated as a concierge to help users with questions they have about our fictional airline, Owl Air. This time, you will program your AI as a flight assistant dealing with a panicked traveler who is running late. This will allow you to demonstrate and test two important principles that help make an AI sound more human: a patient tone, and gracefully handling user interruptions.


## Prerequisites


To complete this tutorial, be sure to have:


- [Node.js](https://nodejs.org/) (version 18 or later)
- A[Twilio Account](https://www.twilio.com/login) (with a voice-enabled phone number)
- An[OpenAI API Key](https://openai.com/index/openai-api/)
- ngrok (to tunnel your local development server to the internet)
- An IDE of your choice (such as[Visual Studio Code](https://code.visualstudio.com/) )


## Building your application


### Step 1: Create a new Node.js project


Open up your terminal and type the following to create a new Node.js project.


Copy code


```text
mkdir voice-humanizer
cd voice-humanizer
npm init -y
```


### Step 2: Install dependencies


Install the required packages for your project:


Copy code


```text
npm install express dotenv express-ws
```


This installs:


- ` express` - Web framework for handling HTTP requests
- ` express-ws` - WebSocket library for real-time communication
- ` dotenv` - Environment variable management


### Step 3: Setting up environment variables


Create a file named *.env* in your project folder. This will safely store your API key and your URL to avoid exposing them if you decide to use git or share your project in some other way. In this file, add the following information:


Copy code


```text
OPENAI_API_KEY=your_actual_openai_api_key_here
DOMAIN=ABC12345.ngrok.app
```


Replace the placeholder for your OpenAI key with your real OpenAI API key, found on[the OpenAI dashboard](https://platform.openai.com/api-keys) .


For` DOMAIN` , this is where you will add your ngrok tunneling url, when that is available in a future step.


### Step 4: Creating your API


When a customer calls your Twilio number, Twilio needs to fetch configuration instructions. You will write a small Express endpoint that returns[TwiML](https://www.twilio.com/docs/voice/twiml) instructing Twilio to pipe the call details directly into our local Node.js WebSocket.


Create a file called *index.js* in your project folder and add the following baseline structure:


Copy code


```text
const express = require('express');
const expressWs = require('express-ws');
const dotenv = require('dotenv');
// Load environment variables from .env file
dotenv.config();
const app = express();
expressWs(app);
const PORT = process.env.PORT || 3000;
// TwiML endpoint for incoming calls
app.post('/voice', (req, res) => {
const domain = process.env.DOMAIN || '';
const twiml = `
<Response>
<Connect>
<ConversationRelay
url="wss://${domain}/ws"
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
</Response>
`;
res.type('application/xml');
res.send(twiml);
});
// WebSocket endpoint for handling voice sessions
app.ws('/ws', async (ws, req) => {
console.log('New WebSocket connection established');
await handleVoiceSession(ws);
});
app.listen(PORT, () => {
console.log(`Server is running on port ${PORT}`);
});
```


The line` ttsProvider="ElevenLabs"` uses[ElevenLabs](https://elevenlabs.io/) for your TTS (Text to Speech). This gives you a more human-like voice cadence for conversations right out of the box. In this tutorial, you are using the` nova-3-general` speech model.


If you want more details about how some of these attributes work, check out our tutorial on[Making an AI Phone Agent With Twilio Conversation Relay](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) , which goes into the specifics about these attributes.


You might notice that your code is not yet complete, because you need to write the` handleVoiceSession` function that actually processes the incoming transcript. You will create that in the next step.


### Step 5: Core functionality and interruption handling


In this step you will write the core functionality for your AI. To make our AI sound human, this block of code needs to:


1. Listen to incoming transcripts sent from Twilio.
2. Stream OpenAI answers word-by-word back to Twilio over the WebSocket with minimal latency.
3. Listen for the` interrupt` event from Twilio. If the user starts talking, we must use an` AbortController` to abruptly kill the active OpenAI stream and send a clear signal back to Twilio to stop talking instantly.


Add this function to your *index.js* file, above the` app.listen()` line:


Copy code


```text
async function handleVoiceSession(ws) {
const openAiKey = process.env.OPENAI_API_KEY;
// Keep track of our active streaming task so we can cancel it on demand
let activeAbortController = null;
ws.on('message', async (message) => {
try {
const data = JSON.parse(message.toString());
const msgType = data.type;
// Case A: The user spoke!
if (msgType === 'prompt') {
const userText = data.voicePrompt;
console.log(`[User Spoke]: ${userText}`);
// Cancel any outbound speech that is currently processing
if (activeAbortController) {
activeAbortController.abort();
}
activeAbortController = new AbortController();
// Fire the task in the background so our loop can keep listening for interruptions!
streamLlmResponseToTwilio(ws, userText, openAiKey, activeAbortController.signal)
.catch(err => {
if (err.name !== 'AbortError') {
console.error('Error streaming LLM response:', err);
}
});
}
// Case B: The user cut the robot off mid-sentence!
else if (msgType === 'interrupt') {
console.log('Interruption detected.');
// Tell our local background task to stop calling OpenAI
if (activeAbortController) {
activeAbortController.abort();
}
// Tell Twilio's audio player to clear its current buffer instantly
const clearMsg = JSON.stringify({ type: 'clear' });
ws.send(clearMsg);
}
} catch (err) {
console.error('Error processing message:', err);
}
});
ws.on('close', () => {
console.log('WebSocket connection closed');
if (activeAbortController) {
activeAbortController.abort();
}
});
ws.on('error', (err) => {
console.error('WebSocket error:', err);
});
}
```


After pasting this code, you can move on to the next step.


### Step 7: Structuring the streamed data for human-like conversation


This helper function calls OpenAI's chat completion endpoint, streams the response token-by-token, wraps the output in[SSML markup tags](https://en.wikipedia.org/wiki/Speech_Synthesis_Markup_Language) , and passes it directly to Twilio.


This code will go in your *index.js* file. Again, be sure to paste this in above app.listen() in your code block, as that will remain the final line.


Copy code


```text
async function streamLlmResponseToTwilio(ws, userInput, apiKey, signal) {
try {
const response = await fetch('https://api.openai.com/v1/chat/completions', {
method: 'POST',
headers: {
'Authorization': `Bearer ${apiKey}`,
'Content-Type': 'application/json'
},
body: JSON.stringify({
model: 'gpt-4o',
stream: true,
messages: [
{
role: 'system',
content: `You are an empathetic, lightning-fast airport flight concierge.
Keep answers extremely short (under 2 sentences) because this is a phone call.
Insert a comma or ellipsis '...' where you want natural breathing pauses.
Use occasional casual conversational fillers like 'Oh, okay...', 'Uhm...', or 'Let me check...' to sound natural.
Use plain sentences only. No bullet points, no markdown, no emojis. Spell out all numbers ("thirty dollars", not "$30"). `
},
{ role: 'user', content: userInput }
]
}),
signal
});
if (!response.ok) {
throw new Error(`OpenAI API error: ${response.status}`);
}
const reader = response.body.getReader();
const decoder = new TextDecoder();
while (true) {
const { done, value } = await reader.read();
if (done || signal.aborted) break;
const chunk = decoder.decode(value, { stream: true });
const lines = chunk.split('\n').filter(line => line.trim() !== '');
for (const line of lines) {
if (!line.startsWith('data: ')) continue;
const data = line.slice(6);
if (data === '[DONE]') break;
try {
const parsed = JSON.parse(data);
const delta = parsed.choices[0]?.delta;
if (delta?.content) {
let textToken = delta.content;
// Humanizer Trick: Dynamic SSML Formatting!
// If the LLM generates a dramatic ellipsis '...', turn it into an explicit breathing break.
if (textToken === '...') {
textToken = '<break time="200ms"/>';
}
// Format token to Twilio specifications
const twilioMsg = JSON.stringify({
type: 'text',
token: textToken,
last: false // Tells Twilio to expect more streaming speech chunks
});
ws.send(twilioMsg);
}
} catch (parseErr) {
// Skip invalid JSON lines
continue;
}
}
}
// Send a final token indicating that this conversational turn is complete
const finalMsg = JSON.stringify({ type: 'text', token: '', last: true });
ws.send(finalMsg);
} catch (err) {
if (err.name === 'AbortError') {
console.log('LLM streaming task cancelled successfully.');
} else {
console.error('Error in streamLlmResponseToTwilio:', err);
}
}
}
```


Focus your attention on the system prompts that we're creating for this AI. You are using system prompts to make the AI voice model mimic human conversational traits:


- Speaking in concise, run-on phrases rather than structured bullet points.
- Using micro-pauses (` <break time="150ms"/>` ) to simulate taking a breath.
- Occasional usage of natural conversational fillers ("Oh...", "I see...", "Give me just a second").
- Making sure that all numbers are articulated clearly, and the voice isn't trying to respond in emojis or a bulleted list.


## Testing Your Application


With all the code in place, you're ready to test your application.


Your server will be running on port 3000. In a separate terminal, expose your local port using ngrok:


Copy code


```text
ngrok http 3000
```


Ngrok will provide you with a secure **Forwarding** url to hit with your web endpoint. Update the` DOMAIN` in your *.env* file to point to this new endpoint.


Open your terminal in your IDE and type to start your application:


Copy code


```text
node index.js
```


Point your Twilio Phone Number's "A Call Comes In" webhook to your ngrok domain. In[the Twilio Console](https://console.twilio.com/) , navigate through **Products and Services > Numbers and Senders >** choose your number. Then go to **Voice and Emergency Calling** and click **Edit Configuration Details** in the upper right hand corner. Choose your appropriate country. Then go to **How do you want to set up your primary method?** And choose "Use Webhooks". Paste the webhook url from your ngrok endpoint, followed by` /voice` (as in` https://1234abcd.ngrok.app/voice` ). Set **Method to** "HTTP POST" from the dropdown menu. Then click **Save** .


Call your Twilio number to test your new bot!


Say hello, and observe how quickly the assistant answers you. Because we stream the output token-by-token directly from OpenAI, your assistant should react to your voice in under 500 milliseconds.


Now, try interrupting your agent as it's speaking to test its interruption handling. While the assistant is explaining flight details, talk directly over it saying: *"Wait! Stop, when does that flight leave?"* Because of your background task listener, the AI will immediately cease playing audio, clear its queue, and smoothly begin answering your interruption.


## Next steps


Making an AI sound human isn't about synthesizing a perfect accent: it is an engineering challenge. By ditching slow, request-response pipelines in favor of Node.js WebSockets and[Twilio Conversation Relay](https://www.twilio.com/docs/voice/conversationrelay) , you can build voice systems that react in real-time, handle interruptions with grace, and speak with realistic pauses.


Your AI here isn't designed to collect any real information for the user and is just a quick demonstration, but there are a lot of additional features you could add. For example,[adding tool calling to your AI](https://www.twilio.com/en-us/blog/add-function-tool-calling-twilio-voice-openai-integration) could allow you to get real flight numbers as your user needs them. For more information, check out our Conversation Relay templates on GitHub. Happy building!


*Dhruv Patel is a Developer on Twilio's Developer Voices team. You can find Dhruv working in a coffee shop with a glass of cold brew or he can be reached at dhrpatel \[at\]*[twilio.com](https://twilio.com/) *.*
