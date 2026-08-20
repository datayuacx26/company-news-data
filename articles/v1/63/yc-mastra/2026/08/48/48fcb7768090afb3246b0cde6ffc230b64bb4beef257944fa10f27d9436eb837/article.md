---
schema_version: "1.0.0"
document_id: "48fcb7768090afb3246b0cde6ffc230b64bb4beef257944fa10f27d9436eb837"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-create-coding-agent-helper"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T01:15:02.935337+00:00"
fetched_at: "2026-08-18T01:15:05.081943+00:00"
content_hash: "sha256:7ce8cf1789502a2ea33c0073ea1f80adb431ee62cd60ee370645b02577452df8"
---

# Introducing Coding Agent Helper for Mastra

You can now use[createCodingAgent](https://mastra.ai/reference/coding-agent/create-coding-agent) to develop your own coding agent with all the essentials it needs. Configure it with a model, instructions, and memory. It can read files, run commands, fix bugs, and track its own progress.


If you're building a coding agent, the` createCodingAgent` helper starts you off with the right primitives: a sandbox for reading and writing files, a task list the agent can work through and complete, and a goal-judge prompt to validate the changes. Agents created with` createCodingAgent` are like any other Mastra agent, they work with[AgentController](https://mastra.ai/docs/harness/agent-controller) , and use the same primitives[Mastra Code](https://code.mastra.ai/) runs on.


Your browser does not support the video tag.


Use the[buildBasePrompt](https://mastra.ai/reference/coding-agent/build-base-prompt) to define your agent's behavior. Point it at your repo with` projectPath` , and set a branch using` gitBranch` . Use a mode to plan and propose changes, or build when it should implement them. Pass instructions as a function that accesses` requestContext` to modify behavior per request.


The` createCodingAgent` defaults can be customized — swap the workspace, replace the error processors, add your own signals, or modify the judge prompt. For additional functionality, you can drop in Mastra's[built-in tools](https://mastra.ai/blog/introducing-built-in-tools) — web search, URL fetch, and ask-user prompts.


## Get started


Install` @mastra/core` and` @mastra/memory` :


Terminal


```text
npm   install   @mastra/core   @mastra/memory
```


note


Requires` @mastra/core@1.48.0` or later, added in[PR #18695](https://github.com/mastra-ai/mastra/pull/18695) .


Configure the agent with a` model` and` memory` . Build instructions dynamically per request, reading values from` requestContext` :


src/mastra/agents/coding-agent.ts


```text
import   {   buildBasePrompt,   createCodingAgent   }   from   "  @mastra/core/coding-agent  "  ;
import   {   webSearchTool,   webFetchTool,   askUserTool   }   from   "  @mastra/core/tools  "  ;
import   {   Memory   }   from   "  @mastra/memory  "  ;


export   const   codingAgent   =   createCodingAgent  ({
id:   "  coding-agent  "  ,
name:   "  Coding Agent  "  ,
instructions  : ({ requestContext })   =>   {
const   mode   =   (requestContext.  get  (  "  mode  "  )   as   string  )   ??   "  build  "  ;
const   projectPath   =   (requestContext.  get  (  "  projectPath  "  )   as   string  )   ??   process.  cwd  ();
const   projectName   =   (requestContext.  get  (  "  projectName  "  )   as   string  )   ??   "  my-app  "  ;
const   gitBranch   =   (requestContext.  get  (  "  gitBranch  "  )   as   string  )   ??   "  main  "  ;


return   buildBasePrompt  ({
mode,
projectPath,
projectName,
gitBranch,
modelId:   "  anthropic/claude-sonnet-4.6  "  ,
productName:   "  Acme Coder  "  ,
coAuthorName:   "  Acme Bot  "  ,
coAuthorEmail:   "  bot@acme.dev  "
});
},
defaultOptions: {
maxSteps:   50
},
model:   "  anthropic/claude-sonnet-4.6  "  ,
memory:   new   Memory  (),
tools: { webSearchTool, webFetchTool, askUserTool }
});
```


### Customize the sandbox


The default` LocalSandbox` runs commands on the host machine. Swap in a remote sandbox like[E2B](https://mastra.ai/integrations/sandboxes/e2b) when the agent needs isolation:


src/mastra/agents/coding-agent.ts


```text
import   {   createCodingAgent   }   from   "  @mastra/core/coding-agent  "  ;
import   {   LocalFilesystem,   Workspace   }   from   "  @mastra/core/workspace  "  ;
import   {   E2BSandbox   }   from   "  @mastra/e2b  "  ;


export   const   codingAgent   =   createCodingAgent  ({
// ...
workspace:   new   Workspace  ({
filesystem:   new   LocalFilesystem  ({ basePath: process.  cwd  () }),
sandbox:   new   E2BSandbox  ()
})
});
```


### Customize task tracking


With memory configured, the default` TaskSignalProvider` is added to any signals you pass. Task tracking remains enabled when you pass your own signal providers:


src/mastra/agents/coding-agent.ts


```text
import   {   createCodingAgent   }   from   "  @mastra/core/coding-agent  "  ;
import   {   WebhookSignalProvider   }   from   "  @mastra/core/signals  "  ;
import   {   Memory   }   from   "  @mastra/memory  "  ;


export   const   codingAgent   =   createCodingAgent  ({
// ...
memory:   new   Memory  (),
signals: [
new   WebhookSignalProvider  ({
extractResourceId  : (payload)   =>   (payload   as   { repository  :   string   }).repository
})
]
});
```


### Customize error retries


The default retry stack handles` ECONNRESET` , bad-request errors, and prefill/history compatibility. Replace it with your own[StreamErrorRetryProcessor](https://mastra.ai/reference/processors/stream-error-retry-processor) to tune retry counts, delays, or matchers:


src/mastra/agents/coding-agent.ts


```text
import   {   createCodingAgent   }   from   "  @mastra/core/coding-agent  "  ;
import   {   StreamErrorRetryProcessor   }   from   "  @mastra/core/processors  "  ;


export   const   codingAgent   =   createCodingAgent  ({
// ...
errorProcessors: [
new   StreamErrorRetryProcessor  ({
retryUnknownErrors:   true  ,
maxRetries:   5  ,
delayMs  : ({ retryCount })   =>   Math.  min  (  1000   *   2   **   retryCount,   30000  )
})
]
});
```


### Customize the goal judge


Customize the[goal](https://mastra.ai/reference/coding-agent/create-coding-agent#goal) with your own` judge` model, run budget, and` prompt` instructions:


src/mastra/agents/coding-agent.ts


```text
import   {   createCodingAgent   }   from   "  @mastra/core/coding-agent  "  ;


export   const   codingAgent   =   createCodingAgent  ({
//...
goal: {
judge:   "  anthropic/claude-haiku-4.5  "  ,
maxRuns:   50  ,
prompt:   "  Return `complete` when the code compiles and tests pass.  "
}
});
```


For more information and full configuration options, see:


- [createCodingAgent reference](https://mastra.ai/reference/coding-agent/create-coding-agent)
- [buildBasePrompt reference](https://mastra.ai/reference/coding-agent/build-base-prompt)
- [Agent reference](https://mastra.ai/reference/agents/agent)
- [AgentController reference](https://mastra.ai/reference/agent-controller/agent-controller-class)
- [TaskSignalProvider reference](https://mastra.ai/reference/signals/task-signal-provider)
- [Workspace reference](https://mastra.ai/reference/workspace/workspace-class)
