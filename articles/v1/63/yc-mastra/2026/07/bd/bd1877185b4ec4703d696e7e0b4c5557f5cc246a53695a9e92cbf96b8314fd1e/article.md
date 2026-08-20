---
schema_version: "1.0.0"
document_id: "bd1877185b4ec4703d696e7e0b4c5557f5cc246a53695a9e92cbf96b8314fd1e"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/multi-agent-orchestration"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:ddd19df87855b8a853463e2dc3207078772dde2bb9ef5046e22a9b38d832c5c2"
---

# Multi-agent orchestration and how it works

One of Anthropic's research Eval found that a multi-agent system[beats a single agent by 90.2%](https://www.anthropic.com/engineering/built-multi-agent-research-system) . But the same setup also spends about fifteen times the tokens of a normal chat.


The question is: Are the additional tokens worth it for the task you're trying to complete?


A single agent loop is basically an LLM calling tool until it decides that the job is done. This setup generally holds up if you know the work doesn't go bigger than the context window. So when you need more complicated work or want to run things in parallel, you need to switch to a multi-agent setup.


In this piece I want to walk you through what exactly multi-agent orchestration is, how it works and how it differs from single tool calls.


*Everything below was run using[Mastra](https://mastra.ai/) , a TypeScript agent framework, wired to Claude in a Docker container.*


## What is multi-agent orchestration?


Multi-agent orchestration is the coordination of several specialized agents so they behave as one system, with a coordinator deciding which agent runs, when, and on what input.


If we strip the complication down, there are just four moving parts.


-


**Agents** do the reasoning. Each has its own instructions, its own tools, and often its own model.


-


**Tools** are the functions agents call to touch the outside world.


-


**State** is the memory shared, or deliberately not shared, between agents.


-


**The orchestrator** is the logic that routes work between all of them.


The orchestrator decides whether you get a reliable system or a pile of prompts that sometimes agree.


You build that coordinator one of two ways.


-


**Deterministic** . Your code fixes the path. Step one, step two, branch here, loop there.


-


**Dynamic** . A model picks the path at runtime, reading the request and choosing which agents to call.


Production systems mostly mix both, and a deterministic shell with a few dynamic calls inside it beats either extreme.


## When does multi-agent orchestration help?


There are three situations where multi-agent orchestration is beneficial. When the work is too big for one context window, when parts of the job can run in parallel, and when the job needs specialist agents that would otherwise collide in a single context window.


For anything one good agent already handles, extra agents only add cost and a bigger surface to debug.


**What multiple agents buy you** **Why one agent struggles**


More total context Each subagent gets its own window, so the system reasons over far more than one context holds


Specialization A focused agent with tight instructions beats a generalist juggling every concern


Parallelism Independent subtasks run at the same time instead of one after another


Isolation A worker that goes off the rails fails in its own lane, not across the whole task


If your job fits into any of these boxes, you probably should opt for multi-agent orchestration, but for most other use cases, a single agent setup works just fine.


My baseline was one agent answering the whole question alone.


## How does multi-agent orchestration work?


Orchestration is mostly just plumbing that moves typed data between agents and decides what runs next and through which agent.


The framework of your choice handles that “plumbing”. So I scaffolded a project with the[create-mastra](https://mastra.ai/docs/getting-started/installation) starter and pointed it at Anthropic to


```text
  npm   create   mastra  @  latest   --   \
--  project  -  name   mastra  -  demo   \
--  components   agents,tools,workflows   \
--  llm   anthropic   \
--  llm  -  api  -  key   "  $ANTHROPIC_API_KEY  "   \
--  example

```


That lays out a folder each for agents, tools, and workflows, with an index.ts wiring them together.


An[agent](https://mastra.ai/docs/agents/overview) is a config object with an id, instructions, a model, and optionally tools and memory.


```text
  import   {   Agent   }   from   '  @mastra/core/agent  '  ;


export   const   techAnalyst   =   new   Agent  ({
id:   '  tech-analyst  '  ,
name:   '  Tech Analyst  '  ,
instructions:   `  You assess technical maturity and feasibility.
Answer in at most 3 short bullet points. No preamble.  `  ,
model:   '  anthropic/claude-haiku-4-5  '  ,
});

```


Each agent names its own model in that model string. I put the workers on Claude Haiku for the narrow, fast jobs and the lead on Sonnet. You could always switch the setup to Opus for narrow jobs and Fable for orchestration.


Every demo below reuses those same workers plus one lead, on @mastra/core 1.51 and Node 22.


## The four approaches to multi-agent orchestrating


There are four approaches to orchestrate agents that handle almost every use case.


Anthropic lays them out in[Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) , And pretty much any framework that you use is a cleaner method for achieving the same result.


Anthropic's[Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) names five of these workflows. I ran the four you will reach for most, then cover the fifth at the end.


Workflow Who decides the path Best for


Orchestrator-workers An LLM lead, at runtime Open-ended tasks you cannot script up front


Prompt chaining Your code, fixed order Multi-stage work where each step feeds the next


Parallelization Your code, all at once Independent subtasks that do not depend on each other


Routing A classifier, then fixed paths One entry point, many kinds of request


I ran all four on the same agents, so the only thing that changes below is how they are wired together.


### 1. Orchestrator-workers


With orchestrator workers, there is a lead agent that reads the request and decides which specialist agents to consult, calls them as tools, and writes the final synthesis.


It makes those calls at runtime, which is what makes this approach dynamic. Each worker is[wrapped as a tool](https://mastra.ai/docs/agents/using-tools) , so a tool call is just a sub-question handed to another agent.


```text
  function   asTool  (id,   description,   agent,   label)   {
return   createTool  ({
id,   description,
inputSchema: z.  object  ({ question: z.  string  () }),
outputSchema: z.  object  ({ answer: z.  string  () }),
execute  :   async   (input)   =>   {
const   res   =   await   agent.  generate  (input.question);
return   { answer: res.text };
},
});
}


const   lead   =   new   Agent  ({
id:   '  research-lead  '  ,
instructions:   `  Break the question into angles, consult the
relevant specialists, then write a short synthesis.  `  ,
model:   '  anthropic/claude-sonnet-4-5  '  ,
tools: {
techAnalyst:   asTool  (  '  tech-analyst  '  ,   '  Ask the technical specialist  '  , techAnalyst),
costAnalyst:   asTool  (  '  cost-analyst  '  ,   '  Ask the cost specialist  '  , costAnalyst),
riskAnalyst:   asTool  (  '  risk-analyst  '  ,   '  Ask the risk specialist  '  , riskAnalyst),
},
});


await   lead.  generate  (question,   {   maxSteps:   6   });

```


The lead pulled in all three specialists and folded their answers into one recommendation.


That same research system runs on this shape, a lead that spawns subagents and merges their results. Reach for it when the steps depend on the request itself and cannot be scripted ahead of time.


The price is “control”. The lead might call the wrong specialist, or the same one twice, and you only find out at runtime.


When you do know the steps, the next three approaches hand that control back.


### 2. Prompt chaining


Next approach is prompt chaining that runs agents in a fixed order, each consuming the output of the one before. Here:


-


my writer agent drafts


-


my critic agent picks the draft apart


-


my editor agent applies the fixes


This order lives in my code, not a model's judgment, and Mastra spells it out as a[workflow of steps](https://mastra.ai/docs/workflows/overview) chained with .then().


```text
  const   pipeline   =   createWorkflow  ({   id:   '  draft-critique-edit  '  ,   /* ... */   })
.  then  (draftStep)       // Writer produces a draft
.  then  (critiqueStep)    // Critic lists concrete improvements
.  then  (editStep);       // Editor applies them
pipeline.  commit  ();


const   run   =   await   pipeline.  createRun  ();
const   out   =   await   run.  start  ({   inputData: { question } });

```


Each step's output schema is the next step's input schema, so a broken handoff throws a validation error rather than passing it off to the next agent and disturbing the context of the rest of the pipeline.


Prompt chaining wins when quality comes in stages, each pass fixing what the last one missed. Ask a single prompt to draft, critique, and edit at once and you get the blurry average of all three.


### 3. Parallelization


Parallelization runs independent subtasks at the same time, what Anthropic calls sectioning. When the parts of a job do not depend on each other, running them in sequence wastes wall-clock time.


Fan them out with Mastra's[.parallel()](https://mastra.ai/docs/workflows/control-flow) , which runs the steps together and merges their output for the next step.


```text
  const   fanout   =   createWorkflow  ({   id:   '  parallel-research  '  ,   /* ... */   })
.  parallel  ([
analystStep  (  '  tech  '  ,   '  Tech Analyst  '  , techAnalyst),
analystStep  (  '  cost  '  ,   '  Cost Analyst  '  , costAnalyst),
analystStep  (  '  risk  '  ,   '  Risk Analyst  '  , riskAnalyst),
])
.  then  (synthesize);
fanout.  commit  ();

```


All three analysts started on the same tick.


Each finished in four to five seconds and the whole run landed at thirteen, against the thirty-plus a sequential version would have cost.


Anthropic saw the same at scale, where parallel subagents "cut research time by up to 90% for complex queries."


### 4. Routing


When you want one agent instead of a committee, route the request. A cheap classifier can read it, pick a category, and a deterministic .branch() send it down exactly one route.


```text
  const   routed   =   createWorkflow  ({   id:   '  routed-research  '  ,   /* ... */   })
.  then  (classify)     // Haiku labels the question: tech | cost | risk
.  branch  ([
[  async   ({ inputData })   =>   inputData.category   ===   '  tech  '  , techStep],
[  async   ({ inputData })   =>   inputData.category   ===   '  cost  '  , costStep],
[  async   ({ inputData })   =>   inputData.category   ===   '  risk  '  , riskStep],
]);
routed.  commit  ();

```


In the example below, the router automatically identified that a “token economics question” needs to be answered by the cost analyst. and sent it directly to that agent.


Routing is the cheapest of the four, since only one agent ever runs, and it is how most "smart" support bots decide whether you land in billing, tech, or account.


### 5. Evaluator-optimizer


The fifth approach uses one agent that produces, another grades the result, and the first revises until the grader is satisfied.


Mastra writes this as .dowhile() and .dountil(), and it fits translation, code generation, or anything where "good enough" is a call a second agent can make.


## Should you use deterministic or dynamic agent orchestration?


Deterministic means my code owns the path. Dynamic means a model owns it.


Deterministic (workflows) Dynamic (agent-led)


Who picks the next step Your code An LLM at runtime


Predictability High, same path every time Low, path varies per request


Debugging Straightforward, it is code Hard, you replay a reasoning trace


Handles novelty Poorly, only paths you wrote Well, it improvises


Token cost Lower, no routing overhead Higher, the router thinks first


Mastra primitive createWorkflow with .then / .branch / .parallel An agent with sub-agents as tools


Workflows win when the path is known ahead of time. An[agent network](https://mastra.ai/blog/vnext-agent-network) wins when the path has to be decided at runtime, from the request itself.


When I can draw the flowchart, I write the workflow. When I cannot, I let an agent lead and accept the debugging tax that comes with it.


My default when unsure is deterministic, because loosening a rigid pipeline is far easier than debugging one that reinvents its path on every request.


## What does multi-agent orchestration cost?


It costs tokens, and more of them than you would guess. Anthropic found that "token usage by itself explains 80% of the variance" in performance, with the number of tool calls and the model choice making up most of the rest.


-


Token usage scales almost linearly with the number of agents and the context each one carries.


-


Tool calls each add a round trip, re-sending the input and generating fresh output.


-


Model choice sets the per-token rate, the one dial you can turn without touching the design.


Model choice is also the cheapest to turn. My Haiku workers cost a fraction of the Sonnet lead, with barely a drop in quality.


Routing is the other lever, since every request it sends down one path is a request that did not wake all the agents at once.


## What breaks in multi-agent systems, and how do I handle it?


Every extra agent is one more thing that can fail. Here are a few examples of what breaks in multi-agent systems.


**Failure** **What it looks like** **How to handle it**


Cost blows up A stuck loop or a chatty lead burns tokens with no ceiling Cap steps (maxSteps), set budgets, prefer routing over fan-out when you can


Error gets passed on One bad sub-answer poisons the synthesis Validate step outputs with schemas, isolate workers so one failure stays local


No observability A wrong answer and no idea which agent caused it Trace every agent call, log inputs and outputs at each hop


State drift Agents disagree because they saw different context Be explicit about shared versus isolated memory, do not let it happen by accident


Non-determinism The same input takes a different path twice Push decisions into deterministic code, keep LLM routing to the few spots that need it


Anthropic[stated](https://www.anthropic.com/engineering/multi-agent-research-system) :


> "Bad tool descriptions can send agents down completely wrong paths."


One vague description upstream, and every agent after it inherits the wrong turn.


So, schemas on every step can turn this potential bad handoff into a clear error at the boundary, which is why I reach for a framework instead of chaining prompts by hand.


## FAQs about multi-agent orchestration


### What is the difference between a workflow and an agent?


A workflow is control flow you write in code, running steps in the order you choose. An agent is an LLM choosing its own next move. Workflows are predictable and agents are flexible, so anything I ship uses both.


### Do I need a framework for multi-agent orchestration?


No, but you will rebuild one badly if you skip it. The typed handoffs, retries, tracing, and shared memory are the same every time, so I let Mastra own them and spend my hours on the coordination instead.


### How many agents is too many?


Match the count to the task, not your ambition. Anthropic's rough guide is one agent for simple lookups and ten or more only for heavy, parallel work. Every agent you add spends more tokens and opens one more way to fail, so I add them only when the task forces my hand.


### Is a single agent with tools ever better?


Often. If the work fits one context window and runs mostly in a line, one well-tooled agent is cheaper, faster to build, and far easier to debug. Multi-agent orchestration wins on breadth, parallelism, and scale, and not much else.


### What is the difference between orchestration and a simple prompt chain?


A prompt chain is one path, the output of A into B into C. Orchestration is the bigger box around it, adding branches, parallelism, loops, and runtime routing. A chain is the simplest thing in that box.


## Where this leaves you


Multi-agent orchestration comes down to five workflows: prompt chaining, parallelization, routing, orchestrator-workers, and evaluator-optimizer. And the axis underneath them is whether your code or a model decides the path.


If you're just starting out, the best way is to try a single agent with good tools. It's also the cheapest option. Test out all your workflows using this single agent and the moment you start hitting ceilings where you either have to wait for sequential runs to complete or you are unable to scale your output, that's when you can consider adding multiple agents.


But always remember, every extra agent and tool call adds to your token costs, so the task has to be worth that overhead before more agents earn their place.


Everything that I have demonstrated here uses Claude in a Docker container with the Mastra framework. If you want to try it locally, clone the repo and run the setup in Docker.
