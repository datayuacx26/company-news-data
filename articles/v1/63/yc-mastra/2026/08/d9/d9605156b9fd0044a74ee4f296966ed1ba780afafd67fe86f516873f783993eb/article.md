---
schema_version: "1.0.0"
document_id: "d9605156b9fd0044a74ee4f296966ed1ba780afafd67fe86f516873f783993eb"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-built-in-tools"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T02:33:29.457591+00:00"
fetched_at: "2026-08-14T02:33:30.100066+00:00"
content_hash: "sha256:00386e2011a00187fe3aefb6371d4dc58604615d7bf70041cd5368f4f3bddc74"
---

# Introducing Built-in Tools for Mastra Agents

Your Mastra agents can now use five[built-in tools](https://mastra.ai/docs/agents/using-tools#built-in-tools) for common tasks like: creating a TODO list, or searching the web.


The following tools are included in` @mastra/core` and can be used with both agents and workflows:


- [Keeping a durable task list](https://mastra.ai/docs/agents/using-tools#task-tracking)
- [Searching the web](https://mastra.ai/docs/agents/using-tools#use-provider-web-search)
- [Fetching a URL](https://mastra.ai/docs/agents/using-tools#fetch-a-web-page)
- [Asking users questions](https://mastra.ai/docs/agents/using-tools#ask-the-user-a-question)
- [Submitting plans for approval](https://mastra.ai/docs/agents/using-tools#submit-a-plan-for-review)


Your browser does not support the video tag.


If you're prototyping an agent, built-in tools can get you up and running faster, and when you’re ready, you can swap them out for provider-specific tools.


All built-in tools integrate with Studio — task lists render as a live panel, ask-user prompts as a chat pause, and plan approvals as an approve/reject card, making it easier to interact and test your agent during development.


## Get started


Install` @mastra/core` :


Terminal


```text
npm   install   @mastra/core
```


note


- ` TaskSignalProvider` : added in` 1.42.0` ,[PR #17820](https://github.com/mastra-ai/mastra/pull/17820) .
- ` webSearchTool` : added in` 1.55.0` ,[PR #20345](https://github.com/mastra-ai/mastra/pull/20345) .
- ` webFetchTool` : added in` 1.54.0` ,[PR #20232](https://github.com/mastra-ai/mastra/pull/20232) .
- ` askUserTool` : added in` 1.42.0` ,[PR #17806](https://github.com/mastra-ai/mastra/pull/17806) .
- ` submitPlanTool` : added in` 1.42.0` ,[PR #17817](https://github.com/mastra-ai/mastra/pull/17817) .


### Task tracking


Manage a durable, thread-scoped task list — write, update, complete, and check tasks. Available using[TaskSignalProvider](https://mastra.ai/reference/signals/task-signal-provider) , which registers four task tools (` task_write` ,` task_check` ,` task_update` , and` task_complete` ). Memory is required to persist list states.


src/mastra/agents/task-tracking-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   Memory   }   from   "  @mastra/memory  "  ;
import   {   TaskSignalProvider   }   from   "  @mastra/core/signals  "  ;


export   const   taskTrackingAgent   =   new   Agent  ({
id:   "  task-tracking-agent  "  ,
name:   "  Task Tracking Agent  "  ,
instructions:   "  Track your progress with the task tools.  "  ,
model:   "  anthropic/claude-opus-5  "  ,
memory:   new   Memory  (),
signals: [  new   TaskSignalProvider  ()]
});
```


### Web search


Use the model's native web search capability. Supports OpenAI, Anthropic, Google Gemini, and xAI:


src/mastra/agents/web-search-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   webSearchTool   }   from   "  @mastra/core/tools  "  ;


export   const   webSearchAgent   =   new   Agent  ({
id:   "  web-search-agent  "  ,
name:   "  Web Search Agent  "  ,
instructions:   "  Use web search when you need current information.  "  ,
model:   "  anthropic/claude-opus-5  "  ,
tools: { webSearchTool }
});
```


### Web fetch


Fetch a URL over HTTP or HTTPS and return the page's text content. Blocks localhost and private IPs, follows up to 5 redirects, and truncates at 100,000 characters:


src/mastra/agents/web-fetch-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   webFetchTool   }   from   "  @mastra/core/tools  "  ;


export   const   webFetchAgent   =   new   Agent  ({
id:   "  web-fetch-agent  "  ,
name:   "  Web Fetch Agent  "  ,
instructions:   "  Fetch the page the user links to before answering.  "  ,
model:   "  anthropic/claude-opus-5  "  ,
tools: { webFetchTool }
});
```


### Ask user


Suspend the run to ask the user a question, then resume with their answer. Supports free-text, single-select, and multi-select prompts.


src/mastra/agents/ask-user-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   askUserTool   }   from   "  @mastra/core/tools  "  ;


export   const   askUserAgent   =   new   Agent  ({
id:   "  ask-user-agent  "  ,
name:   "  Ask User Agent  "  ,
instructions:   "  Ask the user for clarification when the request is ambiguous.  "  ,
model:   "  anthropic/claude-opus-5  "  ,
tools: { askUserTool }
});
```


### Submit plan


Suspend the run with a plan file path so the user can approve or reject it. Pair with workspace tools so the agent can write the plan file before submitting.


src/mastra/agents/submit-plan-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   submitPlanTool   }   from   "  @mastra/core/tools  "  ;
import   {   LocalFilesystem,   Workspace,   mkdirTool,   readFileTool,   writeFileTool   }   from   "  @mastra/core/workspace  "  ;


export   const   submitPlanAgent   =   new   Agent  ({
id:   "  submit-plan-agent  "  ,
name:   "  Submit Plan Agent  "  ,
instructions:   "  You draft plans and submit them for review.  "  ,
model:   "  anthropic/claude-opus-5  "  ,
workspace:   new   Workspace  ({
id:   "  submit-plan-workspace  "  ,
filesystem:   new   LocalFilesystem  ({ basePath: process.  cwd  () })
}),
tools: { submitPlanTool, writeFileTool, readFileTool, mkdirTool }
});
```


For more information and full configuration options, see:


- [Built-in tools](https://mastra.ai/docs/agents/using-tools#built-in-tools)
- [Task tools reference](https://mastra.ai/reference/tools/task-tools)
- [TaskSignalProvider reference](https://mastra.ai/reference/signals/task-signal-provider)
- [webSearchTool](https://mastra.ai/docs/agents/using-tools#use-provider-web-search)
- [webFetchTool](https://mastra.ai/docs/agents/using-tools#fetch-a-web-page)
- [askUserTool reference](https://mastra.ai/reference/tools/ask-user-tool)
- [submitPlanTool reference](https://mastra.ai/reference/tools/submit-plan-tool)
- [Workspace overview](https://mastra.ai/docs/workspace/overview)
