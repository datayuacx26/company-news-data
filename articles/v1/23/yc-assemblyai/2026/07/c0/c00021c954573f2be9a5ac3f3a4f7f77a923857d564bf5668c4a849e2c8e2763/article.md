---
schema_version: "1.0.0"
document_id: "c00021c954573f2be9a5ac3f3a4f7f77a923857d564bf5668c4a849e2c8e2763"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/how-to-use-assemblyai-with-csharp"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:6132e63cde46b0ae50d00fbefa621ee56dae93761d66f5a99a124cac368a6a22"
---

# How to use AssemblyAI with C#

Learn how to use AssemblyAI's Voice AI models directly with C# using the built-in \`HttpClient\`. Transcribe audio, analyze audio using audio intelligence models, and apply LLMs to your audio data using LLM Gateway.


## 1. Transcribe an audio file


```text
using   System.Net.Http.Json;
using   System.Text.Json;


var httpClient =   new     HttpClient  ();
httpClient.DefaultRequestHeaders.  Add  (  "Authorization"  ,   "YOUR_API_KEY"  );


// Submit transcription request
var response = await httpClient.  PostAsJsonAsync  (
"https://api.assemblyai.com/v2/transcript"  ,
new   {
audio_url =   "https://storage.googleapis.com/aai-docs-samples/nbc.mp3"  ,
speech_models =   new  [] {   "universal-3-pro"  ,   "universal-2"   },
language_detection =   true
}
);
var transcript = await response.Content.ReadFromJsonAsync<JsonElement>();
var transcriptId = transcript.  GetProperty  (  "id"  ).  GetString  ();


// Poll until completed
while   (  true  )
{
var result = await httpClient.GetFromJsonAsync<JsonElement>(
$  "https://api.assemblyai.com/v2/transcript/{transcriptId}"
);
var status = result.  GetProperty  (  "status"  ).  GetString  ();


if   (status ==   "completed"  )
{
Console.  WriteLine  (result.  GetProperty  (  "text"  ).  GetString  ());
break  ;
}
else     if   (status ==   "error"  )
{
Console.  WriteLine  (  "Transcription failed"  );
break  ;
}


await Task.  Delay  (  3000  );
}


```


You can also transcribe a local file, as shown here.


```text
using   System.Net.Http.Json;
using   System.Text.Json;


var httpClient =   new     HttpClient  ();


// Step 1: Upload the file
var fileContent =   new     ByteArrayContent  (await File.  ReadAllBytesAsync  (  "./audio.mp3"  ));
var uploadResponse = await httpClient.  PostAsync  (
"https://api.assemblyai.com/v2/upload"  ,
fileContent
);
var uploadResult = await uploadResponse.Content.ReadFromJsonAsync<JsonElement>();
var uploadUrl = uploadResult.  GetProperty  (  "upload_url"  ).  GetString  ();


// Step 2: Submit transcription with upload URL
var response = await httpClient.  PostAsJsonAsync  (
"https://api.assemblyai.com/v2/transcript"  ,
new   {
audio_url = uploadUrl,
speech_models =   new  [] {   "universal-3-pro"  ,   "universal-2"   },
language_detection =   true
}
);
var transcript = await response.Content.ReadFromJsonAsync<JsonElement>();
var transcriptId = transcript.  GetProperty  (  "id"  ).  GetString  ();


// Step 3: Poll until completed
while   (  true  )
{
var result = await httpClient.GetFromJsonAsync<JsonElement>(
$  "https://api.assemblyai.com/v2/transcript/{transcriptId}"
);
var status = result.  GetProperty  (  "status"  ).  GetString  ();


if   (status ==   "completed"  )
{
Console.  WriteLine  (result.  GetProperty  (  "text"  ).  GetString  ());
break  ;
}
else     if   (status ==   "error"  )
{
Console.  WriteLine  (  "Transcription failed"  );
break  ;
}


await Task.  Delay  (  3000  );
}


```


‍


## 2. Transcribe audio in real-time using Streaming Speech-to-Text


```text
using   System.Net.WebSockets;
using   System.Text;
using   System.Text.Json;


var ws =   new     ClientWebSocket  ();
ws.Options.  SetRequestHeader  (  "Authorization"  ,   "YOUR_API_KEY"  );


await ws.  ConnectAsync  (
new     Uri  (  "wss://api.assemblyai.com/v2/realtime/ws?sample_rate=16000"  ),
CancellationToken.None
);


// Receive transcripts in background
_ = Task.  Run  (  async   () =>
{
var buffer =   new   byte[  8192  ];
while   (ws.State == WebSocketState.Open)
{
var result = await ws.  ReceiveAsync  (buffer, CancellationToken.None);
var message = JsonDocument.  Parse  (Encoding.UTF8.  GetString  (buffer,   0  , result.Count));
var messageType = message.RootElement.  GetProperty  (  "message_type"  ).  GetString  ();


if   (messageType ==   "PartialTranscript"  )
Console.  WriteLine  ($  "Partial: {message.RootElement.GetProperty("  text  ")}"  );
else     if   (messageType ==   "FinalTranscript"  )
Console.  WriteLine  ($  "Final: {message.RootElement.GetProperty("  text  ")}"  );
}
});


// Pseudocode for getting audio from a microphone
GetAudio  (  async   (byte[] chunk) =>
{
var audioMessage = JsonSerializer.  Serialize  (  new   { audio_data = Convert.  ToBase64String  (chunk) });
await ws.  SendAsync  (Encoding.UTF8.  GetBytes  (audioMessage), WebSocketMessageType.Text,   true  , CancellationToken.None);
});


// Close connection
var closeMessage = JsonSerializer.  Serialize  (  new   { terminate_session =   true   });
await ws.  SendAsync  (Encoding.UTF8.  GetBytes  (closeMessage), WebSocketMessageType.Text,   true  , CancellationToken.None);


```


## 3. Use LLM Gateway to build LLM apps on voice data


```text
using   System.Net.Http.Json;
using   System.Text.Json;


var httpClient =   new     HttpClient  ();


// Use transcript text from a previous transcription
var transcriptText =   "Your transcript text here..."  ;


var response = await httpClient.  PostAsJsonAsync  (
"https://llm-gateway.assemblyai.com/v1/chat/completions"  ,
new
{
model =   "claude-sonnet-4-5-20250929"  ,
messages =   new  []
{
new   { role =   "user"  , content = $  "Provide a brief summary of the transcript.\n\nTranscript: {transcriptText}"   }
},
max_tokens =   1000
}
);


var result = await response.Content.ReadFromJsonAsync<JsonElement>();
var content = result
.  GetProperty  (  "choices"  )[  0  ]
.  GetProperty  (  "message"  )
.  GetProperty  (  "content"  )
.  GetString  ();


Console.  WriteLine  (content);


```


Learn[how to use LLMs with audio data using LLM Gateway in our docs](https://www.assemblyai.com/docs/getting-started/apply-llms-to-audio-files) .


## 4. Use Speech Understanding models


```text
using   System.Net.Http.Json;
using   System.Text.Json;


var httpClient =   new     HttpClient  ();


// Submit with sentiment_analysis enabled
var response = await httpClient.  PostAsJsonAsync  (
"https://api.assemblyai.com/v2/transcript"  ,
new
{
audio_url =   "https://storage.googleapis.com/aai-docs-samples/nbc.mp3"  ,
sentiment_analysis =   true
}
);
var transcript = await response.Content.ReadFromJsonAsync<JsonElement>();
var transcriptId = transcript.  GetProperty  (  "id"  ).  GetString  ();


// Poll until completed
JsonElement result;
while   (  true  )
{
result = await httpClient.GetFromJsonAsync<JsonElement>(
$  "https://api.assemblyai.com/v2/transcript/{transcriptId}"
);
var status = result.  GetProperty  (  "status"  ).  GetString  ();


if   (status ==   "completed"  )   break  ;
if   (status ==   "error"  ) { Console.  WriteLine  (  "Failed"  );   return  ; }


await Task.  Delay  (  3000  );
}


// Print sentiment results
foreach (var item in result.  GetProperty  (  "sentiment_analysis_results"  ).  EnumerateArray  ())
{
Console.  WriteLine  (item.  GetProperty  (  "text"  ).  GetString  ());
Console.  WriteLine  (item.  GetProperty  (  "sentiment"  ).  GetString  ());   // POSITIVE, NEUTRAL, or NEGATIVE
Console.  WriteLine  (item.  GetProperty  (  "confidence"  ).  GetDouble  ());
Console.  WriteLine  ($  "Timestamp: {item.GetProperty("  start  ")} - {item.GetProperty("  end  ")}"  );
}


```


Learn more[about our Speech Understanding models in our docs](https://www.assemblyai.com/docs/speech-understanding/) .


‍
