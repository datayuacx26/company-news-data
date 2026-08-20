---
schema_version: "1.0.0"
document_id: "b8792b6ccca63ee4a747d7c18d27ad58128c64aea0cc63658a7e0ccdd350a4b7"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/ai-sdk-v7-support"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T03:50:08.536877+00:00"
fetched_at: "2026-08-19T03:50:09.988336+00:00"
content_hash: "sha256:a57fb4c87dd9377ad1ddef6b6151b5e70ad12e9627e4f6c0afa8d9a46c6497b8"
---

# AI SDK v7 support in Mastra

Mastra supported[AI SDK v7](https://mastra.ai/integrations/agentic-ui/ai-sdk-ui) from the day it was released, June 25, 2026. Since then we've continued to support and improve the overall end-to-end developer experience.


AI SDK v7 introduced a new provider spec (` LanguageModelV4` ), a top-level` reasoning` parameter, and a number of API renames.` @mastra/core` hides the spec differences behind the` Agent` API and detects the spec to maintain support for v5, v6, and v7. Upgrading your model providers from` @ai-sdk/openai@2.x.x` (v6) to` @ai-sdk/openai@4.x.x` (v7) won't require any Mastra code changes.


Changes have been made to the following Mastra packages:


- ` @mastra/core` : Handles the runtime, agent definitions,` .stream()` /` .generate()` calls, and` modelSettings` .
- ` @mastra/ai-sdk` : Handles route handlers, stream handlers, and converters that adapt a Mastra agent to` useChat` ,` useCompletion` , or` useObject` on the frontend.


Your browser does not support the video tag.


## @mastra/core


v7 support in` @mastra/core` was released in stages and includes model provider support, reasoning control, message types, and multimodal content.


Terminal


```text
npm   install   @mastra/core
```


### Image support


Agent-generated images now return from` .stream()` as files you can save to disk. Added in[PR #19430](https://github.com/mastra-ai/mastra/pull/19430) ,` @mastra/core 1.52.0` :


src/mastra/agents/image-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   openai   }   from   "  @ai-sdk/openai  "  ;


export   const   imageAgent   =   new   Agent  ({
// ...
model:   openai  (  "  gpt-5.6-sol  "  ),
tools: {
imageGeneration: openai.tools.  imageGeneration  ({
size:   "  1536x1024  "  ,
quality:   "  high  "
})
}
});
```


```text
import   {   writeFileSync   }   from   "  fs  "  ;
import   {   imageAgent   }   from   "  ./mastra/agents/image-agent  "  ;


const   result   =   await   imageAgent.  stream  (  "  A cyberpunk neon green space squirrel  "  );


for   await   (  const   chunk   of   result.fullStream)   {
if   (chunk.type   ===   "  tool-result  "   &&   chunk.payload.toolName   ===   "  imageGeneration  "  )   {
const   base64   =   chunk.payload.result.result;
writeFileSync  (  "  a-cyberpunk-neon-green-space-squirrel.png  "  ,   Buffer.  from  (base64,   "  base64  "  ));
}
}
```


### Model providers


Upgrade to a` @4.x.x` provider package to access the latest models. Added in[PR #18477](https://github.com/mastra-ai/mastra/pull/18477) ,` @mastra/core 1.47.0` :


src/mastra/agents/assistant-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   openai   }   from   "  @ai-sdk/openai  "  ;


export   const   assistantAgent   =   new   Agent  ({
// ...
model:   openai  (  "  gpt-5.6-sol  "  )
});
```


### Reasoning control


Pass` modelSettings.reasoning` to` .stream()` to control reasoning effort. Added in[PR #18500](https://github.com/mastra-ai/mastra/pull/18500) ,` @mastra/core 1.47.0` :


```text
const   result   =   await   assistantAgent.  stream  (  "  Plan a 14-day trip to Tokyo in November with a mix of culture and food.  "  ,   {
modelSettings: { reasoning:   "  high  "   }
});
```


### Typecast messages


Type message payloads with` UIMessage` or` ModelMessage` when calling` .generate()` or` .stream()` . Added in[PR #18997](https://github.com/mastra-ai/mastra/pull/18997) ,` @mastra/core 1.50.0` :


```text
import   type   {   UIMessage,   ModelMessage   }   from   "  ai  "  ;


const   uiMessages  :   UIMessage  []   =   [{ id:   "  1  "  , role:   "  user  "  , parts: [{ type:   "  text  "  , text:   "  Hello!  "   }] }];
const   stream   =   await   assistantAgent.  stream  ({   messages: uiMessages });


const   modelMessages  :   ModelMessage  []   =   [{ role:   "  user  "  , content:   "  Hello!  "   }];
const   result   =   await   assistantAgent.  generate  ({   messages: modelMessages });
```


### Multimodal prompts


Send file attachments to` .generate()` , normalized to v7's file part shape. Added in[PR #19316](https://github.com/mastra-ai/mastra/pull/19316) ,` @mastra/core 1.51.0` :


```text
const   result   =   await   assistantAgent.  generate  ({
messages: [
{
role:   "  user  "  ,
content: [
{ type:   "  text  "  , text:   "  What's in this image?  "   },
{ type:   "  file  "  , data: imageBase64, mediaType:   "  image/png  "   }
]
}
]
});
```


### Tool-result media


Tool-returned images can now be viewed and inspected by` @4.x.x` models. Added in[PR #19755](https://github.com/mastra-ai/mastra/pull/19755) ,` @mastra/core 1.53.0` :


src/mastra/tools/screenshot.ts


```text
import   {   createTool   }   from   "  @mastra/core/tools  "  ;


export   const   screenshotTool   =   createTool  ({
id:   "  screenshot  "  ,
description:   "  Take a screenshot of the current page.  "  ,
execute  :   async   ()   =>   ({
content: [{ type:   "  image  "  , data: buffer.  toString  (  "  base64  "  ), mimeType:   "  image/png  "   }]
})
});
```


## @mastra/ai-sdk


v7 support in` @mastra/ai-sdk` includes route handlers, stream handlers, and converters for` useChat` ,` useCompletion` , and` useObject` .


Terminal


```text
npm   install   @mastra/ai-sdk
```


### Route handlers


Register a` POST` endpoint on your Mastra server that streams an agent, network, or workflow — pass` version: "v7"` to format the response for AI SDK v7. Added in[PR #21720](https://github.com/mastra-ai/mastra/pull/21720) ,` @mastra/ai-sdk 1.9.0` .


#### chatRoute


[chatRoute](https://mastra.ai/reference/ai-sdk/chat-route) creates a POST endpoint that streams chat from an agent:


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core  "  ;
import   {   chatRoute   }   from   "  @mastra/ai-sdk  "  ;


export   const   mastra   =   new   Mastra  ({
// ...
server: {
apiRoutes: [
chatRoute  ({
path:   "  /chat  "  ,
agent:   "  assistant-agent  "  ,
version:   "  v7  "
})
]
}
});
```


#### workflowRoute


[workflowRoute](https://mastra.ai/reference/ai-sdk/workflow-route) creates a POST endpoint that streams workflow execution:


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core  "  ;
import   {   workflowRoute   }   from   "  @mastra/ai-sdk  "  ;


export   const   mastra   =   new   Mastra  ({
// ...
server: {
apiRoutes: [
workflowRoute  ({
path:   "  /workflow  "  ,
workflow:   "  my-workflow  "  ,
version:   "  v7  "
})
]
}
});
```


### Stream handlers


Stream agents, networks, or workflows from your own HTTP endpoint — pass` version: "v7"` to get back a` V7UIMessageStream` . Added in[PR #21720](https://github.com/mastra-ai/mastra/pull/21720) ,` @mastra/ai-sdk 1.9.0` .


#### handleChatStream


[handleChatStream](https://mastra.ai/reference/ai-sdk/handle-chat-stream) streams chat output from an agent:


app/api/chat/route.ts


```text
import   {   handleChatStream   }   from   "  @mastra/ai-sdk  "  ;
import   {   createUIMessageStreamResponse   }   from   "  ai  "  ;
import   {   mastra   }   from   "  @/src/mastra  "  ;


export   async   function   POST  (req  :   Request  )   {
const   params   =   await   req.  json  ();
const   stream   =   await   handleChatStream  ({
mastra,
agentId:   "  assistant-agent  "  ,
params,
version:   "  v7  "
});
return   createUIMessageStreamResponse  ({   stream });
}
```


#### handleNetworkStream


[handleNetworkStream](https://mastra.ai/reference/ai-sdk/handle-network-stream) streams output from an agent network:


app/api/network/route.ts


```text
import   {   handleNetworkStream   }   from   "  @mastra/ai-sdk  "  ;
import   {   createUIMessageStreamResponse   }   from   "  ai  "  ;
import   {   mastra   }   from   "  @/src/mastra  "  ;


export   async   function   POST  (req  :   Request  )   {
const   params   =   await   req.  json  ();
const   stream   =   await   handleNetworkStream  ({
mastra,
agentId:   "  assistant-agent  "  ,
params,
version:   "  v7  "
});
return   createUIMessageStreamResponse  ({   stream });
}
```


#### handleWorkflowStream


[handleWorkflowStream](https://mastra.ai/reference/ai-sdk/handle-workflow-stream) streams output from a workflow:


app/api/workflow/route.ts


```text
import   {   handleWorkflowStream   }   from   "  @mastra/ai-sdk  "  ;
import   {   createUIMessageStreamResponse   }   from   "  ai  "  ;
import   {   mastra   }   from   "  @/src/mastra  "  ;


export   async   function   POST  (req  :   Request  )   {
const   params   =   await   req.  json  ();
const   stream   =   await   handleWorkflowStream  ({
mastra,
workflowId:   "  assistant-workflow  "  ,
params,
version:   "  v7  "
});
return   createUIMessageStreamResponse  ({   stream });
}
```


### Converters


Convert the output of` .stream()` or an agent's messages into AI SDK UI shapes — pass` version: "v7"` to format the output for AI SDK v7. Added in[PR #21720](https://github.com/mastra-ai/mastra/pull/21720) ,` @mastra/ai-sdk 1.9.0` .


#### toAISdkStream


[toAISdkStream](https://mastra.ai/reference/ai-sdk/to-ai-sdk-stream) converts a` .stream()` result into an AI SDK UI-message stream:


```text
import   {   toAISdkStream   }   from   "  @mastra/ai-sdk  "  ;


const   stream   =   toAISdkStream  (mastraAgentStream,   {   version:   "  v7  "   });
```


#### toAISdkMessage


[toAISdkMessages](https://mastra.ai/reference/ai-sdk/to-ai-sdk-messages) converts an agent's messages into AI SDK UI messages:


```text
import   {   toAISdkMessages   }   from   "  @mastra/ai-sdk  "  ;


const   uiMessages   =   toAISdkMessages  (mastraMessages,   {   version:   "  v7  "   });
```


For more information and full configuration options, see:


- [AI SDK UI](https://mastra.ai/integrations/agentic-ui/ai-sdk-ui)
- [chatRoute](https://mastra.ai/reference/ai-sdk/chat-route)
- [workflowRoute](https://mastra.ai/reference/ai-sdk/workflow-route)
- [handleChatStream](https://mastra.ai/reference/ai-sdk/handle-chat-stream)
- [handleNetworkStream](https://mastra.ai/reference/ai-sdk/handle-network-stream)
- [handleWorkflowStream](https://mastra.ai/reference/ai-sdk/handle-workflow-stream)
- [toAISdkStream](https://mastra.ai/reference/ai-sdk/to-ai-sdk-stream)
- [toAISdkMessages](https://mastra.ai/reference/ai-sdk/to-ai-sdk-messages)
