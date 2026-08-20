---
schema_version: "1.0.0"
document_id: "bd7350160cee4e8897bcb0f094a323ff560cb4077b9b91af0238af73837b38d0"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-csharp"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:fd29dce6b4783c0b4229a512cb92ca7d016387e1b06e07e94a1353c13927f21b"
---

# How to Build an AI Phone Agent with Twilio Conversation Relay in C Sharp

Time to read:


-
-
-
-
-


July 10, 2026


**Written by**[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


---


## How To Build An AI Phone Agent With Twilio Conversation Relay in C Sharp


Building a voice AI agent from scratch means wiring together speech recognition, text-to-speech, turn detection, and real-time audio streaming, all at low enough latency to feel natural.[Twilio Conversation Relay](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) handles the entire voice pipeline for you, so you can focus on the AI logic. In this tutorial, you will build a fully functional AI phone agent in C# using .NET 9's API tools.


## Programming Language Support


This post is for C# users. You can also find this tutorial in the following programming languages:


- [Javascript](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-node)
- [Python](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay)
- [PHP](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay)


## Prerequisites


To follow this tutorial, you will need:


- A free[Twilio account](https://www.twilio.com/try-twilio) with a voice-capable phone number
- An[OpenAI API key](https://platform.openai.com/api-keys)
- .NET 9 SDK
- [ngrok](https://ngrok.com/) to expose your local server to Twilio


## Build the app


### Step 1: How Conversation Relay works


Before writing any code, it helps to understand how Conversation Relay fits into the call flow. When someone dials your Twilio number, Twilio requests TwiML from a webhook you configure. Your server returns a[<ConversationRelay>](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) verb inside a[<Connect>](https://www.twilio.com/docs/voice/twiml/connect) block, which tells Twilio to open a[WebSocket](https://websocket.org/guides/road-to-websockets/) connection to your server and hand the call off to Conversation Relay.


From that point on, Conversation Relay acts as the voice pipeline. It transcribes everything the caller says using speech-to-text (STT) and sends the transcript to your WebSocket server as a JSON message. Your server calls an AI model with the transcript, streams the response back over the WebSocket, and Conversation Relay synthesises each token into speech using text-to-speech (TTS), playing it to the caller in real time.


The flow looks like this:


Conversation Relay's median response latency is under 0.5 seconds, so conversations feel natural rather than robotic.


For this tutorial, you will build Hoot, an AI phone support agent for Owl Air, a fictional airline. Hoot can answer questions about flight status, baggage policy, loyalty points, and booking changes.


### Step 2: Set up your project


Start by creating a project directory and a new .NET web application.


Copy code


```text
dotnet new web -n OwlAir
cd OwlAir
```


Add required nuget packages:


Copy code


```text
dotnet add package OpenAI
dotnet add package dotenv.net
```


Next, create a *.env* file for the environment variables your application needs:


Copy code


```text
OPENAI_API_KEY=your_openai_api_key_here
DOMAIN=your-ngrok-hostname.ngrok.app
```


Paste your API key from OpenAI into this placeholder space.


` DOMAIN` is your ngrok hostname without the` https://` prefix. You will get this value in a later step once ngrok is running.


You will have a *Program.cs* file as a starting point for your application. In this file, paste in the following code to start:


Copy code


```text
using System.Text.Json;
using dotenv.net;
using OwlAir;


DotEnv.Load();


var builder = WebApplication.CreateBuilder(args); builder.Services.AddSingleton<OpenAiService>();


var app = builder.Build();


app.UseWebSockets();


app.Run();
```


### Step 3: Write a voice-optimized system prompt


The system prompt is one of the most important parts of a phone agent. Text-to-speech synthesizers read your agent's responses verbatim, so responses that look fine on screen can sound unnatural when spoken aloud. Markdown formatting, bullet points, emoji, and symbols like` $` or` %` all produce awkward output.


To create this we will create a service that interfaces with OpenAI, and paste those instructions in there. Create a new file called *OpenAiService.cs* and paste in the following:


Copy code


```text
using System.Net.WebSockets;
using System.Text.Json;
using System.Text.Json.Nodes;
using OpenAI;
using OpenAI.Chat;
namespace OwlAir;
sealed class OpenAiService
{
const string Model = "gpt-4o-mini";
const string SystemPrompt = """
You are Hoot, the friendly AI phone support agent for Owl Air.
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
- If the caller asks to speak with a human, live agent, or representative, use the transfer_to_agent tool immediately
- If you cannot help with something, say so briefly and offer to transfer them to a human agent
- Never make up information not listed above
""";
```


This handler will handle your interactions with OpenAI. Pay close attention to the system prompt here.


Spelling out numbers in the prompt itself (` thirty dollars` instead of` $30` ) can encourage the model to use the same format in its responses. Capping replies at two or three sentences keeps the conversation from sounding like a wall of text being read aloud. And, explicitly forbidding Markdown prevents the model from adding asterisks, headers, or lists that would be spoken as literal characters.


### Step 4: Create a Conversation Relay handler


You need a handler to process the information from ConversationRelay. First of all, create this file: *ChatMessage.cs* . Paste in this code:


Copy code


```text
namespace OwlAir;
record ChatMessage(string Role, string Content);
```


This is a one-line record to help hold information about the conversation.


Now create your handler, *ConversationRelayHandler.cs* . Paste in the necessary code:


Copy code


```text
using System.Net.WebSockets;
using System.Text;
using System.Text.Json;
namespace OwlAir;
static class ConversationRelayHandler
{
public static async Task HandleAsync(WebSocket ws, OpenAiService openAi)
{
string? callSid = null;
var messages = new List<ChatMessage>();
var buffer = new byte[8192];
try
{
while (ws.State == WebSocketState.Open)
{
using var ms = new MemoryStream();
WebSocketReceiveResult result;
do
{
result = await ws.ReceiveAsync(buffer, CancellationToken.None);
if (result.MessageType == WebSocketMessageType.Close)
{
await ws.CloseAsync(WebSocketCloseStatus.NormalClosure, null, CancellationToken.None);
Console.WriteLine($"[{callSid}] Call ended");
return;
}
ms.Write(buffer, 0, result.Count);
} while (!result.EndOfMessage);
var json = Encoding.UTF8.GetString(ms.ToArray());
using var doc = JsonDocument.Parse(json);
var root = doc.RootElement;
var msgType = root.TryGetProperty("type", out var t) ? t.GetString() : null;


switch (msgType)
{
case "setup":
callSid = root.TryGetProperty("callSid", out var sid) ? sid.GetString() : null;
Console.WriteLine($"[{callSid}] Call connected");
break;
case "prompt":
if (!root.TryGetProperty("last", out var last) || !last.GetBoolean())
break;
var userText = root.TryGetProperty("voicePrompt", out var vp) ? vp.GetString() ?? "" : "";
Console.WriteLine($"[{callSid}] Caller: {userText}");
messages.Add(new ChatMessage("user", userText));
var responseText = await openAi.StreamResponseAsync(ws, callSid, messages);
messages.Add(new ChatMessage("assistant", responseText));
break;
case "interrupt":
var spoken = root.TryGetProperty("utteranceUntilInterrupt", out var u) ? u.GetString() ?? "" : "";
Console.WriteLine($"[{callSid}] Interrupted after: '{spoken}'");
if (messages.Count > 0 && messages[^1].Role == "assistant")
messages.RemoveAt(messages.Count - 1);
break;


case "error":
var desc = root.TryGetProperty("description", out var d) ? d.GetString() : "";
Console.WriteLine($"[{callSid}] Conversation Relay error: {desc}");
break;
}
}
}
catch (WebSocketException)
{
Console.WriteLine($"[{callSid}] Call ended");
}
}
public static Task SendJsonAsync(WebSocket ws, object payload)
{
var text = JsonSerializer.Serialize(payload);
var bytes = Encoding.UTF8.GetBytes(text);
return ws.SendAsync(bytes, WebSocketMessageType.Text, true, CancellationToken.None);
}
}
```


*ConversationRelayHandler.cs* is the persistent heart of a live phone call. When a call comes in, Twilio opens a WebSocket connection to` /ws` and this file takes over. It sits in a loop for the entire duration of the call, reading JSON messages from Twilio one at a time.


Each message has a type field that tells it what just happened.


The` setup` message arrives first, immediately after Conversation Relay opens the WebSocket connection. It contains the` callSid` for the call and any custom parameters you passed via` <Parameter>` elements in your TwiML. The` messages` list is initialised as empty here; the system prompt is prepended inside` stream_response` before each OpenAI API call.


The` prompt` message carries the caller's transcribed speech in` data\["voicePrompt"\]` . The` last` field indicates whether this is the final transcript for a turn. When` partialPrompts` is disabled (the default), you will only receive` prompt` messages with` last: true` , so the` data.get("last")` check ensures you only send requests to the model once the caller has finished speaking.


The` interrupt` message fires when the caller speaks while Hoot is talking. The` utteranceUntilInterrupt` field tells you how much of Hoot's response was played before the interruption. Because that response was cut short, it would be misleading to keep it in the conversation history as a complete assistant turn. The handler removes the last assistant message from the` messages` list, so the next request starts from a clean state.


The` error` message carries a numeric error code and a description. Common errors include invalid provider configurations and WebSocket timeout events. Logging them makes debugging significantly easier.


### Step 5: Configure Conversation Relay attributes


In your *P* *rogram.cs* file, paste in the following (above` app.Run();` ):


Copy code


```text
app.MapPost("/twiml", (IConfiguration config) =>
{
var domain = Environment.GetEnvironmentVariable("DOMAIN") ?? config["DOMAIN"] ?? "";
var xml = $"""
<?xml version="1.0" encoding="UTF-8"?>
<Response>
<Connect>
<ConversationRelay
url="wss://{domain}/ws"
welcomeGreeting="Thanks for calling Owl Air! I'm Hoot. I can help with flight status, baggage policy, loyalty points, or booking changes. Which of those can I help you with?"
ttsProvider="ElevenLabs"
transcriptionProvider="Deepgram"
speechModel="nova-3-general"
eotThreshold="0.8"
ignoreBackchannel="true"
hints="Owl Air, loyalty points, baggage, carry-on, check-in, boarding pass"
interruptible="any"
interruptSensitivity="medium"
elevenlabsTextNormalization="auto"
/>
</Connect>
</Response>
""";
return Results.Content(xml, "text/xml");
});
```


The` url` attribute points to the WebSocket endpoint on your server. It must use` wss://` (the secure WebSocket protocol), not` ws://` . The` welcomeGreeting` is played to the caller immediately after Conversation Relay connects, before your WebSocket handler sends any response. Listing Hoot's four topic areas in the greeting orients callers right away and reduces the chance they ask for something outside the agent's scope.


The` <ConversationRelay>` verb accepts a rich set of attributes for tuning STT and TTS behaviour. The configuration above uses several that are worth understanding in detail.


` ttsProvider="ElevenLabs"` and` transcriptionProvider="Deepgram"` select the voice and speech recognition providers. ElevenLabs is the default TTS provider as of the Conversation Relay GA release, and it produces the most natural-sounding voices. Deepgram is the default STT provider and supports Deepgram Flux, the latest speech model.


` speechModel="nova-3-general"` enables Deepgram Flux, which combines transcription and end-of-turn detection in a single model. Compared to earlier models, Flux delivers 200 to 600 milliseconds less latency per response turn and approximately thirty percent fewer false interruptions. This means Hoot is less likely to cut a caller off mid-sentence or wait awkwardly after they have finished speaking.


` eotThreshold="0.8"` controls how confident Deepgram Flux must be that a speaker has finished before finalising the transcript. The value ranges from 0.5 (more aggressive, fires sooner) to 0.9 (more conservative, waits longer). A value of 0.8 is a good starting point for most use cases.


` ignoreBackchannel="true"` filters out conversational filler words like "yeah", "uh huh", and "okay" that callers say while listening. Without this setting, those sounds would trigger your WebSocket handler and send unnecessary requests to the model mid-response.


` hints` is a comma-separated list of words and phrases that Deepgram should give extra weight to during transcription. Business-specific terms like product names, place names, and technical vocabulary often trip up general speech recognition models. Adding them as hints improves accuracy for your specific use case.


` interruptible="any"` and` interruptSensitivity="medium"` control how the agent responds when a caller speaks while it is talking. Setting` interruptible` to` any` means both speech and DTMF keypresses can interrupt the agent. The` medium` sensitivity setting reduces false interruptions in noisier environments while still responding quickly to intentional interruptions.


` elevenlabsTextNormalization="auto"` tells ElevenLabs to automatically normalise numbers and abbreviations in the text before synthesising speech. This is useful as a safety net if your LLM occasionally outputs a digit or symbol despite your instructions.


### Step 6: Build the WebSocket handler


Above the line` app.Run();` , add the WebSocket endpoint to *Program.cs* :


Copy code


```text
app.MapGet("/ws", async (HttpContext context, OpenAiService openAi) =>
{
if (!context.WebSockets.IsWebSocketRequest)
{
context.Response.StatusCode = 400;
return;
}


using var ws = await context.WebSockets.AcceptWebSocketAsync();
await ConversationRelayHandler.HandleAsync(ws, openAi);
});
```


This registers a GET route at /ws that handles the WebSocket upgrade. When Twilio's Conversation Relay connects, it first checks that the incoming request is actually a WebSocket handshake — if not, it rejects it with a 400. If it is, it accepts the upgrade (which switches the connection from HTTP to WebSocket) and then hands it off to` ConversationRelayHandler.HandleAsync` , which takes over and runs for the entire duration of the call. The await on that last line means the route handler stays open and doesn't return until the call ends.


### Step 7: Stream responses and calling tools


Right now, Hoot can only answer questions about facts baked into the system prompt. To look up real flight status, you need to give the model a tool it can call. The OpenAI function calling API lets you define tools as JSON schemas, and the model decides whether to call one based on the caller's request.


In your *OpenAiService.cs* file, you are going to add a tool that will allow the agent to get more detailed flight information. Add the following code to this file:


Copy code


```text
static readonly ChatTool LookupFlightStatusTool = ChatTool.CreateFunctionTool(
"lookup_flight_status",
"Look up the current status of an Owl Air flight by flight number.",
BinaryData.FromString("""
{
"type": "object",
"properties": {
"flight_number": {
"type": "string",
"description": "The Owl Air flight number, e.g. OA101"
}
},
"required": ["flight_number"]
}
""")
);


static readonly Dictionary<string, FlightInfo> MockFlightData = new()
{
["OA101"] = new("on time", "B12", "two thirty PM"),
["OA205"] = new("delayed by forty minutes", "C4", "five fifteen PM"),
["OA318"] = new("cancelled", null, null),
};


readonly ChatClient _client;


public OpenAiService()
{
var apiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY")
?? throw new InvalidOperationException("OPENAI_API_KEY is not set.");
_client = new OpenAIClient(apiKey).GetChatClient(Model);
}


public async Task<string> StreamResponseAsync(WebSocket ws, string? callSid, List<ChatMessage> messages)
{
var openAiMessages = new List<OpenAI.Chat.ChatMessage>
{
new SystemChatMessage(SystemPrompt)
};
foreach (var m in messages)
openAiMessages.Add(m.Role == "user"
? new UserChatMessage(m.Content)
: new AssistantChatMessage(m.Content));


var fullResponse = "";
try
{
// Phase 1: stream with tools available
var options = new ChatCompletionOptions
{
MaxOutputTokenCount = 300,
Tools = { LookupFlightStatusTool },
};


var stream = _client.CompleteChatStreamingAsync(openAiMessages, options);
ChatFinishReason? finishReason = null;
var toolCallsAcc = new Dictionary<int, ToolCallAccumulator>();
await foreach (var update in stream)
{
if (update.FinishReason.HasValue)
finishReason = update.FinishReason.Value;
foreach (var part in update.ContentUpdate)
{
if (!string.IsNullOrEmpty(part.Text))
{
fullResponse += part.Text;
await ConversationRelayHandler.SendJsonAsync(ws,
new { type = "text", token = part.Text, last = false });
}
}


foreach (var tc in update.ToolCallUpdates)
{
if (!toolCallsAcc.TryGetValue(tc.Index, out var acc))
{
acc = new ToolCallAccumulator();
toolCallsAcc[tc.Index] = acc;
}
if (!string.IsNullOrEmpty(tc.ToolCallId)) acc.Id = tc.ToolCallId;
if (!string.IsNullOrEmpty(tc.FunctionName)) acc.Name = tc.FunctionName;
var argUpdate = tc.FunctionArgumentsUpdate;
if (argUpdate != null && !argUpdate.ToMemory().IsEmpty)
acc.Arguments += argUpdate.ToString();
}
}


// Phase 2: execute tools and stream follow-up
if (finishReason == ChatFinishReason.ToolCalls && toolCallsAcc.Count > 0)
{
var toolCalls = toolCallsAcc.Values.ToList();
var assistantMsg = new AssistantChatMessage(
toolCalls.Select(tc => ChatToolCall.CreateFunctionToolCall(tc.Id, tc.Name, BinaryData.FromString(tc.Arguments))).ToList()
);
openAiMessages.Add(assistantMsg);
foreach (var tc in toolCalls)
{
var args = JsonSerializer.Deserialize<Dictionary<string, string>>(tc.Arguments) ?? new();
var result = ExecuteTool(tc.Name, args);
Console.WriteLine($"[{callSid}] Tool {tc.Name}({tc.Arguments}) -> {result}");
openAiMessages.Add(new ToolChatMessage(tc.Id, result));
}


fullResponse = "";
var options2 = new ChatCompletionOptions { MaxOutputTokenCount = 400 };
var stream2 = _client.CompleteChatStreamingAsync(openAiMessages, options2);
await foreach (var update in stream2)
foreach (var part in update.ContentUpdate)
if (!string.IsNullOrEmpty(part.Text))
{
fullResponse += part.Text;
await ConversationRelayHandler.SendJsonAsync(ws,
new { type = "text", token = part.Text, last = false });
}
}
}
finally
{
await ConversationRelayHandler.SendJsonAsync(ws,
new { type = "text", token = "", last = true });
}


Console.WriteLine($"[{callSid}] Hoot: {fullResponse}");
return fullResponse;
}


static string ExecuteTool(string name, Dictionary<string, string> args)
{
if (name != "lookup_flight_status")
return "Unknown tool.";


var flightNumber = args.TryGetValue("flight_number", out var fn) ? fn.ToUpperInvariant() : "";
if (!MockFlightData.TryGetValue(flightNumber, out var flight))
return "No flight found with that number.";
if (flight.Status == "cancelled")
return "That flight has been cancelled.";
return $"Flight {flightNumber} is {flight.Status}, departing at {flight.Departure} from gate {flight.Gate}.";
}


record FlightInfo(string Status, string? Gate, string? Departure);
sealed class ToolCallAccumulator
{
public string Id { get; set; } = "";
public string Name { get; set; } = "";
public string Arguments { get; set; } = "";
}
}
```


` MockFlightData` is a stand-in for a real database or API call. In a production agent, you would replace this with a live lookup. When a caller gives a flight number that is not in the data,` ExecuteTool` returns` "No flight found with that number."` , so Hoot always has a graceful response rather than hallucinating status information. The flight data uses spelled-out times (` "two thirty PM"` ) following the same voice-optimized convention as the system prompt, so ElevenLabs reads them naturally, when they appear in Hoot's response.


If you add more Owl Air–specific flight numbers to your real data, add them to the` hints` attribute in your TwiML as well. Deepgram gives extra weight to hinted terms during transcription, which reduces misheard flight numbers like "OA one oh one" being transcribed as "0-8101".


Each` text` message you send to Conversation Relay contains a` token` (a text fragment) and a` last` flag. Sending tokens one by one as they arrive with` last: False` lets Conversation Relay begin synthesising and playing speech before the full response is ready. The final message with` last: True` and an empty token signals the end of the turn. The function returns the full accumulated text so the WebSocket handler can append it to the conversation history.


### Step 8: Running your server


You are ready to run the server. Start ngrok as follows:


Copy code


```text
ngrok http 5000
```


ngrok will display a` Forwarding` line that looks something like this:


Copy code


```text
Forwarding  https://1234abcd.ngrok.app -> http://localhost:5000
```


Copy the hostname (` 1234abcd.ngrok.app` , **without**` https://` ) and update your *.env* file:


Copy code


```text
DOMAIN=1234abcd.ngrok.app
```


Now start your .NET project by opening the terminal in VS code and typing:


Copy code


```text
dotnet build dotnet run
```


You should see output that shows your server is up and running, listening on port 5000.


### Step 9: Configuring your Twilio phone number


Log in to the[Twilio Console](https://console.twilio.com/) and navigate to **Numbers & Senders** in the left sidebar, then **Overview** then **My Inventory** . Click your voice-capable phone number to open its configuration page.


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


If Hoot does not respond, check that your ngrok tunnel is still running and that the` DOMAIN` value in *.env* matches your current ngrok hostname. ngrok generates a new hostname each time you restart the free tier, so you will need to update your Twilio webhook URL and restart your .NET server whenever you restart ngrok.


If you hear an error message on the call or see an` error` message in your terminal logs, check the error code against the[Conversation Relay error code reference](https://www.twilio.com/docs/voice/conversationrelay/websocket-messages#error-message) in the Twilio documentation.


## What's next?


The agent you built here covers the core Conversation Relay pattern, but there are several natural extensions worth exploring.


**Proactive topic re-offering** - If a caller seems uncertain, Hoot will stay silent and wait. You can fix this with a single addition to the system prompt: Instruct Hoot to re-offer the four topic areas if the caller has not asked a clear question after one or two exchanges. No code change required.


**Human handoff** - The system prompt already tells Hoot to offer to connect callers with a live agent. To implement it, detect when the model's response includes that offer and send` {"type": "end-session", "handoffData": "caller needs live support"}` from the WebSocket handler. You will also need to add an` action` attribute to the` <ConversationRelay>` element in your TwiML (e.g.` action="/handoff"` ). Twilio POSTs to that URL when the session ends, passing the` handoffData` string, where you can return new TwiML to route the call to a queue or a direct number using standard` <Dial>` logic.


**Caller personalization** - You can pass data into the` setup` message using` <Parameter>` elements inside` <Connect>` in your TwiML. For example,` <Parameter name="customerName" value="Alex"/>` makes` customParameters.customerName` available in the` setup` message, so you can have Hoot greet the caller by name. In a real deployment you would look up the caller's name from the` From` phone number before generating the TwiML.


## Conclusion


You have built a fully functional AI phone agent using Twilio Conversation Relay, .NET 9, and the OpenAI API. Conversation Relay handled the speech-to-text and text-to-speech pipeline, while your WebSocket server connected the model to the call and managed the conversation history, including live flight lookups via tool calling. See the[Conversation Relay documentation](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) for the full list of attributes and message types.


If you ran into any problems working with this solution, you can[view the full sample on Github](https://github.com/twilio-samples/ai-phone-agent-conversation-relay-csharp) .


*Amanda Lange is a .NET Engineer of Technical Content. She is here to teach how to create great things using C# and .NET programming. She can be reached at amlange \[ at\] twilio.com.*
