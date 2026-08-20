---
schema_version: "1.0.0"
document_id: "9d88fb359f69ec88eb3c48aa17e4b29b94af639c3683ff95d3192e7c1174a6c6"
company_key: "yc-butter"
company: "Butter"
source_id: "yc-butter-rss-cfe99054af6a"
canonical_url: "https://blog.butter.dev/the-messy-world-of-deterministic-agents"
published_at: "2025-10-23T20:26:26+00:00"
first_seen_at: "2026-07-24T22:18:01.434032+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:beff3302acb3ae463a668dc5dba7bb0d47436ec57d558897b522a05a2e5ce430"
---

# The Messy World of “Deterministic Agents”

With the daily use of “agentic” tools like[Cursor](https://cursor.com/) and[Claude Code](https://www.claude.com/product/claude-code) , we’re all becoming deeply familiar with the feeling of working with autonomous agents, and increasingly aware of their shortcomings.


I’m here to talk about one of those shortcomings: **non-determinism** .


You’ve probably run into it yourself, it goes as follows:


1.


You have a relatively repeatable task to do.


2.


The agent does it right the first time. Awesome!


3.


You ask the agent to do that same task again, with slightly different input.


4.


The agent chooses a dramatically different approach. Weird!


This sense of randomness can be unsettling, and lead to a **lack of trust in agentic systems** , which is a strict hurdle to get over before widespread adoption. We want to think of agents as employees we can delegate to, but **employees learn skills** which builds trust, whereas agents do not. Yet.


### What we’ll be covering


This blog is an overview of nine different attempts at introducing “determinism” (skills, trust, reliability, repeatability) into agents. These approaches range from nascent ideas and theoretical concepts to full products being used in the enterprise.


For each approach, I’ll be highlighting products or research that’s relevant. We’ll walk through them from highest abstraction down to the lowest abstraction.


Note that this coverage will include our own product[Butter](https://butter.dev/) , but it’s not my intention to shill our own thing or draw any “us-vs-them” comparisons. Those blogs always suck.


The goal is to inform, and paint a clear picture for the many ways this world could end up going.


Let’s start from the top!


## Workflow Builders


Workflow Builders are Zapier-like canvases which allow technical and non-technical users to chain prebuilt integrations together, including LLM blocks for operations such as data transformation and classification-based routing.


They are *not* agents by the strict definition, but are embraced by the more cautious enterprise user due to their explainability and true determinism. No magic, just smarter data processing. If an agent block is included, it’s opt-in and generally well scoped.


These tools have exploded in usage, with[n8n](https://n8n.io/) being a popular choice.


And earlier this month, OpenAI launched their[Agent Builder](https://platform.openai.com/docs/guides/agents/agent-builder) in the same category:


Following the OpenAI launch, many were disappointed to see a workflow builder use the “agent” name. Regardless of if it’s truly an agent, users are happy and workflow builders deserve their place in this blog as today’s most viable route to “determinism.”


You may have wondered why I said they’re not agents, which warrants a quick definition:


**An LLM agent runs tools in a loop to achieve a goal.** –[Simon Willison](https://simonwillison.net/2025/Sep/18/agents/)


```text
while   task_incomplete:
tool_choice = llm()
do(tool_choice)


```


Under this definition, the control flow is dictated by the LLM, allowing agents to perform tasks they were never explicitly programmed to do.


All of the following approaches apply specifically to agents by this definition, exploring what it means to bring (pseudo)determinism into fundamentally random LLM branching decisions.


As we walk through them, remember:


-


The architecture: there’s always a model, and it’s always choosing tools.


-


The goal: “determinism” is defined as the consistent reproduction of a trajectory of tool calls given a repeat task.


## Context Engineering


Many view skills and repeatability as a *context* problem, focusing on the fact that an LLM performing an automation does not have the accumulated knowledge from prior runs.


So… some people simply put the successful run in the context.


Products in this camp still use LLMs at every turn, but they aim to increase the reliability and skill-retention by injecting additional content into the context window.


Dynamic context engineering is nothing new, tracing its lineage back to few-shot prompting (hardcoding examples into your prompt) and[RAG](https://python.langchain.com/docs/concepts/rag/) (appending semantically relevant content into the prompt). Notable products in the space, dubbed “memory layers,” include[mem0](https://github.com/mem0ai/mem0) and[Supermemory](https://supermemory.ai/) .


The space has historically focused on user context, such as remembering birthdays, but applied to deterministic replay, a memory layer would be repurposed to store and retrieve content related to **instructions & behaviors,** which may include:


-


User preferences


-


User-written SOPs (standard operating procedures)


-


Recorded prior agent trajectories


-


An LLM-summarization of prior trajectories


-


Reasoning traces or summaries explaining why certain branches are taken.


This does not force determinism by the strict definition, but it does *guide* the model.


### Explicit Skills


One pattern is to build up a knowledge-base in advance, similar to an employee onboarding document, which may be selectively referenced by the agent during operation.


Most recently, you can see this approach used by Anthropic’s newly released[Claude Skills](https://www.anthropic.com/news/skills) , which seems to be RAG across SOPs and docs.


> While working on tasks, Claude scans available skills to find relevant matches. When one matches, it loads only the minimal information and files needed—keeping Claude fast while accessing specialized expertise.


These skills are generated in-advance by a user via interacting with a separate “skill-creator” agent. Doing so requires advance knowledge of the types of tasks your agents will perform.


### Learned Skills


The process of “skill creation” could be done after the fact, inferred from the message history.


You can see this in action with[Cursor’s Memory](https://cursor.com/docs/context/memories) feature, which uses a special “save to memory” tool which it can invoke if it detects a behavior would be useful in the context of all future runs.


Another creative approach for learned skills is Letta’s[Sleep-Time Agents](https://docs.letta.com/guides/agents/sleep-time-agents) , which use async agents to continuously overwrite earlier message history context with more compressed summaries, allowing agents to resume from their prior history rather than needing to start from fresh states.


## Code Generation


With the goal of consistently reproducing tool calls, the most deterministic tool we could reach for is code itself.


Rather than using LLMs in the hot-loop, interpreting every case and choosing a tool to invoke, what if they were used more like compilers, generating optimized code in advance? LLMs are especially well suited for this, given strong programming language representation in their training sets.


In its most basic form, **codegen could produce one-off disposable scripts** , which when interpreted in our client environment would call tool functions directly.


This bypasses the indirection of the ToolCall response type, and allows a single LLM generation to invoke as many tools as it needs.


The team at[Cloudflare](https://www.cloudflare.com/) recently launched “[Code Mode](https://blog.cloudflare.com/code-mode/) ” (great blog), and[Browser Use](https://browser-use.com/) recently launched[Code Use](https://x.com/gregpr07/status/1981116900223701091) , both of which do exactly this concept. The scripts are ephemeral, but the concept of calling tools from code is the building block for the next topic.


### Meta-Tools


Sometimes, the code generated to invoke multiple tools is worth storing as its own tool.


In doing this, we’re allowing the models to not only use tools, but to build their own abstractions, making each tool call decision to be that much more powerful.


There’s no common name for this, so we’ll just call them “meta-tools.”


What’s most worth highlighting here is how well it fits into the “agents are loops with tools” architecture. The model keeps using tools, it’s just that those tools perform increasingly long (and deterministic) tasks.


The universally agreed pioneer of this concept is the[Voyager paper](https://arxiv.org/abs/2305.16291) , which used on-the-fly tool generation to evolve primitive Minecraft bot APIs into higher-abstracted tools:


Voyager was well ahead of its time, published in 2023, and there’s yet to be a clear follow-up paper or product that expands on it.


### Script-Agent Fallback


Script-agent fallback refers to systems whose default operating mode is pure-software, with agent loops only being used for initial discovery and self-healing.


To generate these scripts, it’s usually done post-hoc, after seeing multiple examples. In these cases, you employ an agent (or human!) to perform a multi-step workflow, and use the tool-call trace from that to generate reusable scripts. Note that “generate” is loosely defined here, ranging from full-blown LLM codegen to simple json runbooks.


This approach is especially popular in computer automation, where humans can be there to describe the task, perform or monitor the learning runs, leave comments, and iterate on the produced scripts.


Stars in this space include[Browserbase’s Director](https://www.director.ai/) (congrats on the recent[v2 launch](https://x.com/pk_iv/status/1980653648310071663) ) and[Browser Use’s](https://browser-use.com/)[Workflow Use](https://github.com/browser-use/workflow-use) .


We also experimented at this level, with a tool tracing and replay SDK called[Muscle Mem](https://github.com/pig-dot-dev/muscle-mem) . Read[here for why we started it](https://erikdunteman.com/blog/muscle-mem) , and[here for why we moved on](https://blog.butter.dev/muscle-mem-as-a-proxy) .


Similar to the workflow builder UIs, script-agent fallback systems require you to know in advance which workflow you’re about to run. The branching behavior does not need to be known in advance, but the task does need to be discrete and namable.


### Script Generators


This is “Lovable for Automations,” where technical or nontechnical users work with codegen agents to produce pure-software scripts. No agents at runtime.


This ahead-of-time generation can be quite tricky, as it’s hard to know in advance exactly which tools will get run in a particular workflow, but with enough iteration and end user feedback, you can get there.


Particularly creative approaches in this space even build DSLs to represent the automation, and force generation using custom grammars, reducing the surface area for error and hallucinations.


Notable teams in this space are[Forge](https://withforge.com/) and[Sola](https://www.sola.ai/) .


As with workflow builder UIs, it’s unclear if this fits under the strict definition of an agent, but worth shouting out, as these products tend to be happily adopted.


## Response Caching


Response Caching means running an HTTP proxy in front of the LLM provider and caching responses as they flow through. On repeat requests, the cache can serve responses, as if a model had generated them, resulting in deterministic behavior.


By spoofing the LLM layer, the agent loop remains simple, unaware that the endpoint is guiding it down a deterministic path.


This is coincidentally what we’re building at[Butter.dev](http://butter.dev/) .


To have any meaningful cache-hit rate (more than the rare exact context matches) you’d need to figure out how to correctly group subtly different prompts, identify dynamic data, ignore noisy contexts, handle complex conditional control flows, etc. Much is yet to be solved, which we’ve[written more about here](https://blog.butter.dev/template-aware-caching) .


## LLM-Layer Improvements


The obvious VC question is “won’t the big labs just do this?”


Quite possibly! As taught in the[The Bitter Lesson](https://en.wikipedia.org/wiki/Bitter_lesson) , one must not ignore the consistent trend that one-size-fits-all LLMs have always ended up superseding incremental progress made outside of the models.


If the goal is to make models more deterministic, surely there’s a way it could happen in the architecture of the models themselves.


I’m in no way an expert in these topics, and there’s probably some secret other thing being cooked up in the labs or a paper I haven’t seen, but here are a few model-level shoutouts.


### Action Models


Action Models are special language models where the decoder is trained to emit tool calls rather than tokens, allowing them to map input stimuli to actions without text as an intermediate.


These models are used heavily in robotics, where the specialized domain allows them to be a quite a bit smaller and runnable on-device.


Adjacent work has been done in computer use by the team at[General Agents](http://generalagents.com/ace/) , resulting in shockingly fast fully-agentic computer automation:


> Ace leverages a new behavioral training paradigm. Unlike language and vision models which are trained on text and images, Ace is trained on behavior.


### Reinforcement Learning


Many process automation tasks have quick feedback for success/fail, which makes these domains optimal for using that feedback as a reward function in RL.


It’s quite early, but[early tinkerers](https://x.com/brendanh0gan/status/1923135962789364206) are seeing promising results.


# Mapping them all together


Below are all of the topics we’ve discussed, highlighting how well each approach satisfies the important aspects of reliable “deterministic replay”:


And below is a map showing where in the stack the approaches sit, and how explicitly tasks must be known in advance:


I trust this overview helps untangle the mess of different approaches, breaking them down into clearer sub-groups, so we all can make more informed tradeoffs in our own building.


Cheers!


Erik
