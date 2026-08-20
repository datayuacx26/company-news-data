---
schema_version: "1.0.0"
document_id: "6fe8495a499c4f56a06e4acf377dd786faa2f61381be536ed88d73e23a15ca1d"
company_key: "yc-paragon"
company: "Paragon"
source_id: "yc-paragon-news-import-425709159824"
canonical_url: "https://www.useparagon.com/blog/what-is-an-agent-harness"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-13T22:52:47.126118+00:00"
fetched_at: "2026-08-13T22:52:50.044387+00:00"
content_hash: "sha256:9709dd43553df1a658d3e929e43c19ed02320a8d8c088535a9c4dcd173d29f10"
---

# What Is an Agent Harness?

# What Is an Agent Harness?


Take a team building an agent to triage internal IT tickets. They pick CrewAI to structure the code,
write a system prompt, wire up three tools — create a ticket, look up an asset, page the on-call
engineer — and give the agent a rolling memory of its last ten tickets. CrewAI is the framework. The
specific combination they assembled with it — that prompt, those three tools, that memory window,
tied together in one loop — is the agent harness. And the process running right now, triaging
ticket #4,391, is the agent.


Agent harness, agent framework, and agent get used interchangeably in practice, but they name three
different layers of the same system, and mixing them up is why two people can compare "agent
projects" and mean entirely different things — one comparing frameworks, the other comparing what a
team built with one.


## Agent harness vs. agent framework vs. agent: what's the difference?


A harness is a specific, configured instance; a framework is the toolkit used to build one; an agent
is the running process that comes out the other end. In the IT-ticket example, CrewAI answers "what
was this built with," the prompt-plus-tools-plus-memory combination answers "what is this specific
system made of," and the live triage run on ticket #4,391 answers "what is it doing right now" —
three different questions, three different answers, one system.


The distinction holds regardless of which framework a team picks. A framework such as LangGraph or
CrewAI supplies reusable abstractions — a way to define steps, route between them, and manage state —
but it doesn't specify what any particular agent should do. A team uses that framework to assemble a
harness: a chosen model, a system prompt, a specific set of tools, a memory strategy, and a loop that
ties them together for one product's use case. Run that harness, and the agent is the live process
executing it. Two teams can use the identical framework and end up with harnesses that behave nothing
alike, because the framework only supplies the scaffolding.


Agent harness


Agent framework


Agent


What it is


A specific configuration of loop, context, tools, and memory


The software toolkit used to assemble that configuration


The running process executing the configuration


Example


The IT-ticket harness: its prompt, its three tools, its ten-ticket memory window, tied together in one loop


CrewAI, LangGraph, or a hand-rolled agent loop


That harness running right now against ticket #4,391


Who controls it


The team building the specific product


The framework's maintainers


Whichever process or user invoked it


A related term worth separating out is an agent skill — a packaged, reusable capability a harness can
load as a component, distinct from the harness itself.[What qualifies as an agent skill](https://www.useparagon.com/blog/what-are-agent-skills) covers that distinction in more depth.


## What are the components of an agent harness?


A harness is made of four working parts: a loop, context, tools, and memory. Changing any one of them
changes how the same underlying model performs the same task.


The **loop** is the control flow that lets the model act more than once before answering — call a
tool, read the result, decide whether to call another tool or respond. Without a loop, a model can
only produce one response from one prompt; with one, it can gather information and revise its own
plan mid-task.[Anthropic's writing on building effective agents](https://www.anthropic.com/engineering/building-effective-agents) is a useful primer on how
much of an agent's behavior traces back to this loop, not the model in isolation.


**Context** is the system prompt plus whatever working information the model currently has —
instructions on how to behave, the current conversation, and any retrieved facts relevant to the
task. A harness with the same tools but a vaguer system prompt will behave less predictably, even on
identical requests.


**Tools** are the functions the model can call to take action or fetch information outside its own
training data — searching a database, sending a message, running a calculation. A harness can wire
tools up by hand, following a schema like[OpenAI's function-calling format](https://platform.openai.com/docs/guides/function-calling) , or by connecting to an MCP server —
the Model Context Protocol, an open standard for exposing tools, prompts, and resources through one
common interface instead of a custom integration per tool.[MCP is defined in more depth here](https://www.useparagon.com/blog/ai-agent-tool-calling-access-saas-apps) : in short, the protocol started out built for a
locally-running process and only picked up a remote HTTP option afterward. Both transports are in
active use today, and which one a harness's tools rely on changes where they execute and how the
harness authenticates to reach them.


**Memory** is the record the harness carries forward — earlier turns of a conversation, prior tool
results, or decisions made in an earlier step of a longer task. Some harnesses keep memory only for
the length of one session; others persist it across sessions so the agent doesn't start from zero
each time.


**Components at a glance:**


-


**Loop** — lets the model act, observe, and act again before answering


-


**Context** — the system prompt and working information the model currently has


-


**Tools (including MCP)** — the functions the model can call to take action or fetch information


-


**Memory** — what the harness carries forward from earlier turns or steps
