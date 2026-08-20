---
schema_version: "1.0.0"
document_id: "0fb0c3b5df0a76497c53f7963dc0ea4bce8d6acfaf97536cd1f6c55cab6bdfb1"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-and-less-robotic-in-csharp"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T14:41:51.679553+00:00"
fetched_at: "2026-07-29T14:41:54.220955+00:00"
content_hash: "sha256:02781c13f507366e6f16da6fc00e4ce15176d3dc87386549a90ee8ae8fa070a8"
---

# How to Make Your AI Voice Sound More Human and Less Robotic with C#

Time to read:


-
-
-
-
-


July 28, 2026


**Written by**[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


---


If you've called a customer service helpline recently, chances are good that you've encountered some kind of AI assistant voice helper. But not all AI helpers are created equally. Some AI assistants are truly a pain to deal with, with long latency that creates a frustrating conversation. Others are more human-like, with a pleasant tone and cadence and the ability to handle your requests and interruptions gracefully.


What's the difference? If you want your AI to sound more human and less robotic on the phone, having a great simulated voice really helps. But aside from that, creating a low-latency bot that handles interruptions gracefully can be a big benefit to your users and make them feel more at ease.


In this tutorial, you will build a real-time voice agent using C#,[Twilio Conversation Relay,](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) and[OpenAI](https://openai.com/) . In[previous tutorials](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) , we've created an AI that operated as a concierge to help users with questions they have about our fictional airline, Owl Air. This time, you will program your AI as a flight assistant dealing with a panicked traveler who is running late. This will allow you to demonstrate and test two important principles that help make an AI sound more human: a patient tone, and gracefully handling user interruptions.


Programming Language Support This tutorial is geared towards .NET developers, but you can find other languages below:


- [Javascript](https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-less-robotic-nodejs)
- [Python](https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-and-less-robotic-in-python)
- [PHP](https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-less-robotic-php)


## Prerequisites


To complete this tutorial, be sure to have:


- The .[NET 8 or later SDK](https://dotnet.microsoft.com/en-us/download)
- A[Twilio Account](https://www.twilio.com/login) (with a voice-enabled phone number)
- An[OpenAI API Key](https://openai.com/index/openai-api/)
- ngrok (to tunnel your local development server to the internet)
- An IDE of your choice (such as[Visual Studio Code](https://code.visualstudio.com/) )


## Building your application


### Step 1: Create a new ASP.NET Core Web API project


Open up your terminal and type the following to create a new .NET project.


Copy code


```text
dotnet new web -o VoiceHumanizer
cd VoiceHumanizer
```


### Step 2: Setting up environment variables


Create a file named *.env* in your project folder. This will safely store your API key and your URL to avoid exposing them if you decide to use git or share your project in some other way. In this file, add the following information:


Copy code


```text
OPENAI_KEY=your_actual_openai_api_key_here
DOMAIN=ABC12345.ngrok.app
```


Replace the placeholder for your OpenAI key with your real OpenAI API key, found on[the OpenAI dashboard](https://platform.openai.com/api-keys) .


For` DOMAIN` , this is where you will add your ngrok tunneling url, when that is available in a future step.


Type this into your terminal to initiate use of the environment variables:


Copy code


```text
dotnet add package dotenv.net
```


### Step 3: Creating your API


When a customer calls your Twilio number, Twilio needs to fetch configuration instructions. You will write a small Minimal API endpoint that returns[TwiML](https://www.twilio.com/docs/voice/twiml) instructing Twilio to pipe the call details directly into our local C# WebSocket.


You will have a file called *Program.cs* in your new project. Open *Program.cs* and replace its contents with the following baseline structure:


Copy code


```text
using System.Net.WebSockets;
using System.Text;
using System.Text.Json;
// Load .env file into environment variables
var envPath = Path.Combine(Directory.GetCurrentDirectory(), ".env");
if (File.Exists(envPath))
{
foreach (var line in File.ReadAllLines(envPath))
{
var parts = line.Split('=', 2);
if (parts.Length == 2)
Environment.SetEnvironmentVariable(parts[0].Trim(), parts[1].Trim());
}
}
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();
app.UseWebSockets();
app.MapPost("/voice", (IConfiguration config) =>
{
var domain = Environment.GetEnvironmentVariable("DOMAIN") ?? config["DOMAIN"] ?? "";
var twiml = $"""
<Response>
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
</Response>
""";
return Results.Content(twiml, "application/xml");
});
// Map our WebSocket path
app.Map("/ws", async (HttpContext context) =>
{
if (context.WebSockets.IsWebSocketRequest)
{
using var webSocket = await context.WebSockets.AcceptWebSocketAsync();
await HandleVoiceSession(webSocket, app.Configuration);
}
else
{
context.Response.StatusCode = StatusCodes.Status400BadRequest;
}
});
app.Run();
```


The line` ttsProvider="ElevenLabs"` uses[ElevenLabs](https://elevenlabs.io/) for your TTS (Text to Speech). This gives you a more human-like voice cadence for conversations right out of the box. In this tutorial, you are using the` nova-3-general` speech model.


If you want more details about how some of these attributes work, check out our tutorial on[Making an AI Phone Agent With Twilio Conversation Relay](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) , which goes into the specifics about these attributes.


You might notice that your code is not yet complete, because you need to write the` HandleVoiceSession` method that actually processes the incoming transcript. You will create that in the next step.


### Step 4: Core functionality and interruption handling


In this step you will write the core functionality for your AI. To make our AI sound human, this block of code needs to:


- Listen to incoming transcripts sent from Twilio.
- Stream OpenAI answers word-by-word back to Twilio over the WebSocket with minimal latency.
- Listen for the` interrupt` event from Twilio. If the user starts talking, we must use a` CancellationTokenSource` to abruptly kill the active OpenAI stream and send a clear signal back to Twilio to stop talking instantly.


Add this method to the bottom of your[Program.cs](http://program.cs/) *—* but above` app.Run();` *:*


Copy code


```text
static async Task HandleVoiceSession(WebSocket webSocket, IConfiguration config)
{
var buffer = new byte[1024 * 64];
var openAiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY") ?? config["OPENAI_API_KEY"] ?? "YOUR_OPENAI_API_KEY";
// keep track of our active streaming task so we can cancel it on demand
CancellationTokenSource? activeLlmCts = null;
try
{
while (webSocket.State == WebSocketState.Open)
{
var ms = new MemoryStream();
WebSocketReceiveResult result;
do
{
result = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), CancellationToken.None);
ms.Write(buffer, 0, result.Count);
} while (!result.EndOfMessage);
if (result.MessageType == WebSocketMessageType.Close) break;
var rawJson = Encoding.UTF8.GetString(ms.ToArray());
using var doc = JsonDocument.Parse(rawJson);
var root = doc.RootElement;
if (!root.TryGetProperty("type", out var typeProp)) continue;
var msgType = typeProp.GetString();
// Case A: The user spoke!
if (msgType == "prompt")
{
var userText = root.GetProperty("voicePrompt").GetString();
Console.WriteLine($"[User Spoke]: {userText}");
// Cancel any outbound speech that is currently processing
activeLlmCts?.Cancel();
activeLlmCts = new CancellationTokenSource();
// Fire the task in the background so our loop can keep listening for interruptions!
_ = StreamLlmResponseToTwilio(webSocket, userText, openAiKey, activeLlmCts.Token);
}
// Case B: The user cut the robot off mid-sentence!
else if (msgType == "interrupt")
{
Console.WriteLine("Interruption detected.");
// Tell our local background task to stop calling OpenAI
activeLlmCts?.Cancel();
// Tell Twilio's audio player to clear its current buffer instantly
var clearMsg = JsonSerializer.Serialize(new { type = "clear" });
var bytes = Encoding.UTF8.GetBytes(clearMsg);
await webSocket.SendAsync(new ArraySegment<byte>(bytes), WebSocketMessageType.Text, true, CancellationToken.None);
}
}
}
catch (Exception ex)
{
Console.WriteLine($"Session error: {ex.Message}");
}
}
```


After pasting this code, you can move on to the next step.


### Step 5: Structuring the streamed data for human-like conversation


This helper method calls OpenAI's chat completion endpoint, streams the response token-by-token, wraps the output in[SSML markup tags](https://en.wikipedia.org/wiki/Speech_Synthesis_Markup_Language) , and passes it directly to Twilio.


This code will go in your *Program.cs* file. Again, be sure to paste this in above` app.Run();` in your code block, as that will remain the final line.


Copy code


```text
static async Task StreamLlmResponseToTwilio(WebSocket webSocket, string userInput, string apiKey, CancellationToken ct)
{
using var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", $"Bearer {apiKey}");
var payload = new
{
model = "gpt-4o",
stream = true,
messages = new[]
{
new {
role = "system",
content = @"You are an empathetic, lightning-fast airport flight concierge.
Keep answers extremely short (under 2 sentences) because this is a phone call.
Insert a comma or ellipsis '...' where you want natural breathing pauses.
Use occasional casual conversational fillers like 'Oh, okay...', 'Uhm...', or 'Let me check...' to sound natural.
Use plain sentences only. No bullet points, no markdown, no emojis. Spell out all numbers (""thirty dollars"", not ""$30""). "
},
new { role = "user", content = userInput }
}
};
var request = new HttpRequestMessage(HttpMethod.Post, "https://api.openai.com/v1/chat/completions")
{
Content = new StringContent(JsonSerializer.Serialize(payload), Encoding.UTF8, "application/json")
};
try
{
using var response = await client.SendAsync(request, HttpCompletionOption.ResponseHeadersRead, ct);
using var stream = await response.Content.ReadAsStreamAsync(ct);
using var reader = new StreamReader(stream);
while (!reader.EndOfStream && !ct.IsCancellationRequested)
{
var line = await reader.ReadLineAsync(ct);
if (string.IsNullOrWhiteSpace(line) || !line.StartsWith("data: ")) continue;
var data = line["data: ".Length..];
if (data == "[DONE]") break;
using var doc = JsonDocument.Parse(data);
var delta = doc.RootElement.GetProperty("choices")[0].GetProperty("delta");
if (delta.TryGetProperty("content", out var contentProp))
{
var textToken = contentProp.GetString();
// Humanizer Trick: Dynamic SSML Formatting!
// If the LLM generates a dramatic ellipsis '...', turn it into an explicit breathing break.
if (textToken == "...")
{
textToken = "<break time=\"200ms\"/>";
}
// Format token to Twilio specifications
var twilioMsg = JsonSerializer.Serialize(new
{
type = "text",
token = textToken,
last = false // Tells Twilio to expect more streaming speech chunks
});
var bytes = Encoding.UTF8.GetBytes(twilioMsg);
await webSocket.SendAsync(new ArraySegment<byte>(bytes), WebSocketMessageType.Text, true, ct);
}
}
// Send a final token indicating that this conversational turn is complete
var finalMsg = JsonSerializer.Serialize(new { type = "text", token = "", last = true });
await webSocket.SendAsync(new ArraySegment<byte>(Encoding.UTF8.GetBytes(finalMsg)), WebSocketMessageType.Text, true, CancellationToken.None);
}
catch (OperationCanceledException)
{
Console.WriteLine("Llm streaming task cancelled successfully.");
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


Open your terminal in your IDE and type:


Copy code


```text
dotnet run
```


Take note of what port (usually 5000 or 5100) your server is running on. Now, in a separate terminal, expose your local port using ngrok:


Copy code


```text
ngrok http 5000
```


Ngrok will provide you with a secure url to hit with your web endpoint. Update the DOMAIN in your *.env* file to point to this new endpoint.


Now, restart your server. (` dotnet run` )


Point your Twilio Phone Number’s "A Call Comes In" webhook to your ngrok domain. In[the Twilio Console](https://console.twilio.com/) , navigate through **Products and Services > Numbers and Senders >** choose your number. Then go to **Voice and Emergency Calling** and click **Edit Configuration Details** in the upper right hand corner. Choose your appropriate country. Then go to **How do you want to set up your primary method?** And choose "Use Webhooks". Paste the webhook url from your ngrok endpoint, followed by` /voice` (as in` https://1234abcd.ngrok.app/voice` ). Set **Method to** "HTTP POST" from the dropdown menu. Then click **Save** .


Call your Twilio number to test your new bot!


Say hello, and observe how quickly the assistant answers you. Because we stream the output token-by-token directly from OpenAI, your assistant should react to your voice in under 500 milliseconds.


Now, try interrupting your agent as it's speaking to test its interruption handling. While the assistant is explaining flight details, talk directly over it saying: *"Wait! Stop, when does that flight leave?"* Because of your background task listener, the AI will immediately cease playing audio, clear its queue, and smoothly begin answering your interruption.


## Next steps


Making an AI sound human isn't about synthesizing a perfect accent: it is an engineering challenge. By ditching slow, request-response pipelines in favor of C# WebSockets and[Twilio Conversation Relay](https://www.twilio.com/docs/voice/conversationrelay) , you can build voice systems that react in real-time, handle interruptions with grace, and speak with realistic pauses.


Your AI here isn't designed to collect any real information for the user and is just a quick demonstration, but there are a lot of additional features you could add. For example,[adding tool calling to your AI](https://www.twilio.com/en-us/blog/add-function-tool-calling-twilio-voice-openai-integration) could allow you to get real flight numbers as your user needs them. For more information, check out our[Conversation Relay templates on GitHub](https://github.com/twilio-samples/ai-phone-agent-conversation-relay-csharp) . Happy building!


*Amanda Lange is a .NET Engineer of Technical Content. She is here to teach how to create great things using C# and .NET programming. She can be reached at amlange \[ at\] twilio.com.*
