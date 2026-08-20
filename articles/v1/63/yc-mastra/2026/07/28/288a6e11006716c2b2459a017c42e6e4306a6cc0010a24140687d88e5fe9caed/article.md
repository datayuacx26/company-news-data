---
schema_version: "1.0.0"
document_id: "288a6e11006716c2b2459a017c42e6e4306a6cc0010a24140687d88e5fe9caed"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-tool-hooks"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:6685117552813a3b8d9c2934ca3cdc5072c0d57577f0da9f9c2758f7d5a85f7c"
---

# Introducing Tool Hooks for Mastra Agents

You can now add[tool hooks](https://mastra.ai/docs/agents/using-tools#run-logic-around-tool-calls) to your Mastra agents, letting you log, audit, and validate tool calls before, during, and after they run — or block them entirely.


Agent-level hooks fire at two points during a tool call:


1. ` beforeToolCall` : Just before the tool is called — can block the call or return a pre-defined result
2. ` afterToolCall` : After the tool has run and returned its output, or thrown an error (skipped when the call is blocked)


Your browser does not support the video tag.


[Tool-level hooks](https://mastra.ai/docs/agents/using-tools#streaming) fire at four separate points during tool calls:


1. ` onInputStart` : When the model has picked a tool and is about to start writing its arguments
2. ` onInputDelta` : On each chunk of the arguments as they stream in
3. ` onInputAvailable` : When the full arguments are parsed and validated, ready to be passed to the tool
4. ` onOutput` : After the tool has run and returned its output (only when execution succeeded)


When using` .stream()` , tool-level hooks fire in real time, letting you monitor different stages of the execution. With` .generate()` , the same hooks fire in the same order, but won't be surfaced until the agent's run is complete.


Before hooks, adding logging or policy checks to a tool call meant manually writing them as part of its` execute` function. This approach still works, but only if you can modify the tool — and only if every agent using it needs the same behavior. Now with hooks that logic lives at the agent or workspace level, or can be configured per-call. Tools also expose their own hooks — one for each stage of the call lifecycle, defined on the tool itself.


## Get started


Add` hooks` to your agent to run logic before and after every tool call.


note


Requires` @mastra/core@1.49.0` or later, added in[PR #17637](https://github.com/mastra-ai/mastra/pull/17637) .


### Agent-level hooks


Use` beforeToolCall` for policy checks: return` proceed: false` to skip the tool call, and an` output` object matching the tool's` outputSchema` with your own values:


src/mastra/agents/weather-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   weatherTool   }   from   "  ../tools/weather-tool  "  ;


const   BLOCKED_LOCATIONS   =   [  "  london  "  ,   "  paris  "  ];


export   const   weatherAgent   =   new   Agent  ({
// ...
tools: { weatherTool },
hooks: {
beforeToolCall  : ({ toolName, input })   =>   {
if   (toolName   ===   "  weatherTool  "  ) {
const   { location }   =   input   as   { location  :   string   };
if   (BLOCKED_LOCATIONS.  some  ((blocked)   =>   location.  toLowerCase  ().  includes  (blocked))) {
return   {
proceed:   false  ,
output: {
temperature:   null  ,
feelsLike:   null  ,
humidity:   null  ,
windSpeed:   null  ,
windGust:   null  ,
conditions:   `  Weather data unavailable for "  ${  location  }  " (blocked by policy).  `  ,
location
}
};
}
}
},
afterToolCall  : ({ toolName, output, error })   =>   {
if   (error) {
console.  error  (  `  [afterToolCall]   ${  toolName  }   |   ${  error  }`  );
}   else   {
console.  log  (  `  [afterToolCall]   ${  toolName  }   |   ${  output  }`  );
}
}
}
// ...
});
```


### Per-call hooks


Pass` hooks` to` .generate()` or` .stream()` to override the agent-level hooks for a single execution:


```text
await   weatherAgent.  generate  (  "  What's the weather in London?  "  ,   {
hooks: {
beforeToolCall  : ({ toolName })   =>   {
console.  log  (  `  [beforeToolCall]   ${  toolName  }`  );
}
}
});
```


### Tool-level hooks


Tools expose four hooks — one for each stage of a call:


src/mastra/tools/weather-tool.ts


```text
import   {   createTool   }   from   "  @mastra/core/tools  "  ;


export   const   weatherTool   =   createTool  ({
// ...
onInputStart  : ({ toolCallId })   =>   {
console.  log  (  `  [onInputStart]   ${  toolCallId  }`  );
},
onInputDelta  : ({ inputTextDelta })   =>   {
console.  log  (  `  [onInputDelta]   ${  inputTextDelta  }`  );
},
onInputAvailable  : ({ input, toolCallId })   =>   {
console.  log  (  `  [onInputAvailable]   ${  toolCallId  }   |   ${  input  }`  );
},
onOutput  : ({ output, toolName })   =>   {
console.  log  (  `  [onOutput]   ${  toolName  }   |   ${  output  }`  );
},
execute  :   async   (inputData)   =>   {
// ...
}
});
```


### Workspace hooks


[Workspace hooks](https://mastra.ai/docs/workspace/overview#tool-hooks) work the same as agent-level hooks but the context also includes` workspaceToolName` :


src/mastra/workspaces.ts


```text
import   {   Workspace   }   from   "  @mastra/core/workspace  "  ;


const   workspace   =   new   Workspace  ({
// ...
tools: {
hooks: {
beforeToolCall  : ({ toolName, workspaceToolName, input })   =>   {
console.  log  (  `  [beforeToolCall]   ${  toolName  }   |   ${  workspaceToolName  }   |   ${  input  }`  );
},
afterToolCall  : ({ toolName, output, error })   =>   {
if   (error) {
console.  error  (  `  [afterToolCall]   ${  toolName  }   |   ${  error  }`  );
}   else   {
console.  log  (  `  [afterToolCall]   ${  toolName  }   |   ${  output  }`  );
}
}
}
}
});
```


For more information and full configuration options, see:


- [Run logic around tool calls](https://mastra.ai/docs/agents/using-tools#run-logic-around-tool-calls)
- [Tool streaming lifecycle hooks](https://mastra.ai/docs/agents/using-tools#available-hooks)
- [Workspace tool hooks](https://mastra.ai/docs/workspace/overview#tool-hooks)
- [Agent reference — tool hooks](https://mastra.ai/reference/agents/agent#tool-hooks)
- [Workspace reference — tool hooks](https://mastra.ai/reference/workspace/workspace-class#tool-hooks)
- [createTool() reference](https://mastra.ai/reference/tools/create-tool)
