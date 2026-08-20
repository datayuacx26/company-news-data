---
schema_version: "1.0.0"
document_id: "9e32c9e8a0e52bea398ffeb0df3eccf585678113408c58c56e830a14d72f684c"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-goals"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:430d9844570462bd61f1c27484391c55852075c4a124c5d3aff2efb05e875740"
---

# Introducing Goals for Mastra Agents

You can now give your agents a durable, thread-scoped[goal](https://mastra.ai/docs/long-running-agents/goals) to work towards. Set an objective using` setObjective` , and an LLM judge scores each loop until the goal is met or a run budget is exhausted.


Your browser does not support the video tag.


On each completed loop, the judge scores the conversation against the objective and returns` 1` or` 0` — a pass ends the loop, a fail injects feedback prompting the agent to try again. Evaluations are emitted as goal chunks containing the` objective` ,` status` , and` results` array with a` score` , and` reason` . For example:


```text
{
"  objective  "  :   "  Write a detailed report for all LLM releases in 2026  "  ,
"  iteration  "  :   1  ,
"  maxRuns  "  :   10  ,
"  passed  "  :   true  ,   // or false
"  status  "  :   "  done  "  ,   // or "active" or "paused"
"  pausedReason  "  :   null  ,
"  judgeFailed  "  :   false  ,
"  waitingForUser  "  :   false  ,
"  results  "  :   [
{
"  score  "  :   1  ,
"  passed  "  :   true  ,
"  reason  "  :   "  The goal to write a detailed report on 2026 LLM releases has been fully satisfied...  "  ,
"  scorerId  "  :   "  goal-scorer  "  ,
"  scorerName  "  :   "  Goal (LLM)  "  ,
"  duration  "  :   3054
}
]
// ...
}
```


Before goals, keeping an agent focused on one objective across multiple messages meant re-supplying the success criteria on every` .generate()` or` .stream()` call, or using an[isTaskComplete](https://mastra.ai/docs/agents/supervisor-agents#task-completion-scoring) check. With goals, the objective is set once on the thread and the judge runs inside the loop where it decides to pass or fail on each iteration.


You can manage objectives using` setObjective` ,` getObjective` ,` updateObjectiveOptions` , and` clearObjective` — and if` maxRuns` is exhausted the objective stays active, letting you resume the loop again from where it left off.


## Get started


Goals require a configured[storage](https://mastra.ai/docs/storage/overview) backend and a[memory](https://mastra.ai/docs/memory/overview) -backed thread.


note


Requires` @mastra/core@1.42.0` or later. Added in[PR #17889](https://github.com/mastra-ai/mastra/pull/17889) .


Add` goal` to your agent config, defining a model for the` judge` and value for` maxRuns` :


src/mastra/agents/research-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   Memory   }   from   "  @mastra/memory  "  ;
import   {   exaSearchTool   }   from   "  ../tools/exa-search-tool  "  ;


export   const   researchAgent   =   new   Agent  ({
id:   "  research-agent  "  ,
name:   "  Research Agent  "  ,
instructions:   "  You research topics by calling the search tool with different queries...  "  ,
model:   "  anthropic/claude-opus-4-8  "  ,
tools: { exaSearchTool },
memory:   new   Memory  (),
goal: {
judge:   "  anthropic/claude-haiku-4-5  "  ,   // smaller model to run evaluations
maxRuns:   10
}
});
```


Set the objective on a thread using` setObjective()` , then pass the` objective` to` .stream()` :


```text
const   agent   =   mastra.  getAgent  (  "  researchAgent  "  );


const   threadId   =   "  research-123  "  ;
const   resourceId   =   "  acme-research-agent  "  ;
const   objective   =   "  Write a detailed report for all LLM releases in 2026  "  ;


await   agent.  setObjective  (objective,   {   threadId,   resourceId });


const   stream   =   await   agent.  stream  (objective,   {
memory: { thread: threadId, resource: resourceId }
});


console.  log  (chunk.type);
switch   (chunk.type)   {
case   "  tool-call  "  :
console.  log  (chunk.payload);
break  ;
case   "  tool-result  "  :
console.  log  (chunk.payload);
break  ;
case   "  goal  "  :
console.  log  (chunk.payload);
break  ;
}
}


const   record   =   await   agent.  getObjective  ({   threadId });
console.  log  (  `  objective=  ${  record?.objective  }   status=  ${  record?.status  }`  );
```


For more information and full configuration options, see:


- [Goals](https://mastra.ai/docs/long-running-agents/goals)
- [ChunkType reference — Goal](https://mastra.ai/reference/streaming/ChunkType#goal)


## Appendix


Chunk names emitted by` stream.fullStream` during a goal-driven run:


**Run lifecycle** — one pair per` .stream()` call:


- ` start` : The agent run has started
- ` finish` : The run has ended


**Step lifecycle** — one pair per iteration through the LLM (model call + tools + response):


- ` step-start` : A new step begins
- ` step-finish` : The current step ends


**Text streaming** — one set per assistant text block:


- ` text-start` : The model begins emitting text
- ` text-delta` : Each chunk of streamed text (many per block)
- ` text-end` : The text block is complete


**Tool calls** — the model streams the args before invoking:


- ` tool-call-input-streaming-start` : The model begins writing the tool's args
- ` tool-call-delta` : A chunk of the args JSON
- ` tool-call-input-streaming-end` : Args finished streaming
- ` tool-call` : The parsed tool call (` toolName` ,` toolCallId` ,` args` )
- ` tool-result` : The tool executed and returned (` toolName` ,` toolCallId` ,` result` )


**Signals** — state and notification signals projected into the model's context:


- ` data-signal` : A signal (including the current` <current-objective>` from the goal system)


**Goals** — one per evaluation from the judge:


- ` goal` :` objective` ,` iteration` ,` maxRuns` ,` passed` ,` status` ,` results` ,` reason` ,` pausedReason` ,` waitingForUser` ,` judgeFailed` ,` duration` ,` timedOut` ,` maxRunsReached` ,` suppressFeedback` ,` pending` ,` activity`
