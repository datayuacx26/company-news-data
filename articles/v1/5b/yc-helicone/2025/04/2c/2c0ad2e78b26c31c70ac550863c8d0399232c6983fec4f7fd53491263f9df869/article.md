---
schema_version: "1.0.0"
document_id: "2c0ad2e78b26c31c70ac550863c8d0399232c6983fec4f7fd53491263f9df869"
company_key: "yc-helicone"
company: "Helicone"
source_id: "yc-helicone-news-import-836d1e549638"
canonical_url: "https://www.helicone.ai/blog/openai-realtime-api-with-helicone"
published_at: "2025-04-11T00:00:00+00:00"
first_seen_at: "2026-07-21T22:44:44.297475+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:cecfa8e0248e24de6f46c1c1032103ed28e47476d7f2f602ca0051abac18129f"
---

# How to Monitor OpenAI's Realtime API with Helicone

# How to Monitor OpenAI's Realtime API with Helicone


April 11, 2025


· 12 minute read


Lina Lam


· April 11, 2025


OpenAI's[Realtime API](https://platform.openai.com/docs/guides/realtime) enables low-latency, multi-modal conversations with support for both text and audio. By integrating with Helicone, you can monitor performance, analyze interactions, and gain insights into your real-time applications without changing your core code.


This guide shows you how to integrate Helicone with OpenAI's Realtime API using a simple WebSocket connection. We'll create a basic implementation that logs interactions while maintaining the stateful nature of realtime conversations.


## 1. Setting up your environment


First, let's set up our environment variables and install the necessary dependencies.


```text
# Set your API keys as environment variables
# For OpenAI
export   OPENAI_API_KEY=<your-openai-api-key>
export   HELICONE_API_KEY=<your-helicone-api-key>


# For Azure (if applicable)
export   AZURE_API_KEY=<your-azure-api-key>
export   AZURE_RESOURCE=<your-azure-resource>
export   AZURE_DEPLOYMENT=<your-azure-deployment>


```


Install the required packages:


```text
npm install ws dotenv


```


## 2. Creating a WebSocket connection through Helicone


With our environment ready, let's implement the WebSocket connection through Helicone. The key difference here is that we're using Helicone's gateway URL instead of connecting directly to OpenAI.


```text
import    WebSocket    from    "ws"  ;
import   { config }  from    "dotenv"  ;


config  ();


// This is where the magic happens - simply point to Helicone's gateway instead of OpenAI
const   url =  "wss://api.helicone.ai/v1/gateway/oai/realtime?model=gpt-4o-realtime-preview-2024-12-17"  ;


const   ws =  new    WebSocket  (url, {
headers  : {
"Authorization"  :  `Bearer  ${process.env.OPENAI_API_KEY}  `  ,
"Helicone-Auth"  :  `Bearer  ${process.env.HELICONE_API_KEY}  `  ,
// Optional Helicone properties for better analytics
"Helicone-Session-Id"  :  `session_ ${ Date  .now()}  `  ,
"Helicone-User-Id"  :  "user_123"  ,
},
});


ws. on  ( "open"  ,  function    open  (  ) {
console  . log  ( "Connected to server"  );
// Initialize session with desired configuration
ws. send  ( JSON  . stringify  ({
type  :  "session.update"  ,
session  : {
modalities  : [ "text"  ,  "audio"  ],
instructions  :  "You are a helpful AI assistant..."  ,
voice  :  "alloy"  ,
input_audio_format  :  "pcm16"  ,
output_audio_format  :  "pcm16"  ,
}
}));
});


```


If you're using Azure, the implementation is very similar:


```text
const   url =  `wss://api.helicone.ai/v1/gateway/oai/realtime?resource= ${process.env.AZURE_RESOURCE}  &deployment= ${process.env.AZURE_DEPLOYMENT}  `  ;


const   ws =  new    WebSocket  (url, {
headers  : {
"Authorization"  :  `Bearer  ${process.env.AZURE_API_KEY}  `  ,
"Helicone-Auth"  :  `Bearer  ${process.env.HELICONE_API_KEY}  `  ,
// Optional Helicone properties
"Helicone-Session-Id"  :  `session_ ${ Date  .now()}  `  ,
"Helicone-User-Id"  :  "user_123"  ,
},
});


```


## 3. Handling WebSocket events


Next, we need to handle various events from the WebSocket connection:


```text
ws. on  ( "message"  ,  function    incoming  ( message  ) {
try   {
const   response =  JSON  . parse  (message. toString  ());
console  . log  ( "Received:"  , response);


// Handle specific event types
switch   (response. type  ) {
case    "input_audio_buffer.speech_started"  :
console  . log  ( "Speech detected!"  );
break  ;
case    "input_audio_buffer.speech_stopped"  :
console  . log  ( "Speech ended. Processing..."  );
break  ;
case    "conversation.item.input_audio_transcription.completed"  :
console  . log  ( "Transcription:"  , response. transcript  );
break  ;
case    "error"  :
console  . error  ( "Error:"  , response. error  . message  );
break  ;
}
}  catch   (error) {
console  . error  ( "Error parsing message:"  , error);
}
});


ws. on  ( "error"  ,  function    error  ( err  ) {
console  . error  ( "WebSocket error:"  , err);
});


// Handle cleanup
process. on  ( "SIGINT"  ,  () =>   {
console  . log  ( "\nClosing connection..."  );
ws. close  ();
process. exit  ( 0  );
});


```


## 4. Enhancing analytics with Helicone headers


Helicone provides additional headers to help you track and analyze your sessions. These headers enable you to group related interactions, trace user journeys, and gain deeper insights:


```text
const   headers = {
// Required headers
"Authorization"  :  `Bearer  ${process.env.OPENAI_API_KEY}  `  ,
"Helicone-Auth"  :  `Bearer  ${process.env.HELICONE_API_KEY}  `  ,


// Optional headers for better analytics
"Helicone-Session-Id"  :  `session_ ${ Date  .now()}  `  ,
"Helicone-User-Id"  :  "user_123"  ,
"Helicone-Session-Name"  :  "VoiceAssistant"  ,
"Helicone-Session-Path"  :  "/voice-assistant/conversation"  ,
};


```


When you run your application with these headers, Helicone will automatically log and organize your interactions based on the provided metadata.


## Testing your integration


When you run your application, you should see events from the Realtime API being logged in your Helicone dashboard. This includes audio transcription events, message completions, and other interaction metrics.


Remember that the Realtime API maintains a stateful connection. If the connection is interrupted, you'll need to recreate the session and reinitialize the conversation state.


## Why use Helicone with OpenAI's Realtime API


By integrating with Helicone, you get:


- **Zero-latency overhead:** Helicone's architecture ensures that logging doesn't impact your application's performance
- **Comprehensive visibility:** Monitor both text and audio interactions in one place
- **Session analysis:** Track user journeys across multiple interactions
- **Cost monitoring:** Understand how your Realtime API usage translates to costs
- **Performance insights:** Identify latency issues or potential optimizations


These headers help you group related API calls, track user sessions, analyze conversation patterns, and monitor usage and performance. For Realtime API, see[docs](https://docs.helicone.ai/integrations/openai/realtime) . See[Sessions](https://docs.helicone.ai/features/sessions) for tracing regular OpenAI API calls.


## Further reading


- [Using Custom Properties in Helicone](https://docs.helicone.ai/docs/custom-properties)
- [Managing Sessions in Helicone](https://docs.helicone.ai/features/sessions#sessions)
- [Understanding OpenAI's Realtime API](https://docs.helicone.ai/integrations/openai/realtime)


---


### Questions or feedback?


Are the information out of date? Please[raise an issue](https://github.com/Helicone/helicone/issues) or[contact us](https://www.helicone.ai/contact) , we'd love to hear from you!
