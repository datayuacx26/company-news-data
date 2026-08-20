---
schema_version: "1.0.0"
document_id: "02b9d9ecfab331b0b8fc8ad7818b0111dd3f4744fcd7920b19fce9fb4a1ca838"
company_key: "yc-alter"
company: "Alter"
source_id: "yc-alter-rss-08ac1704852e"
canonical_url: "https://blog.alterauth.com/p/is-authorization-for-ai-agents-actually"
published_at: "2026-04-01T06:10:28+00:00"
first_seen_at: "2026-07-24T15:31:02.362502+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:963c715fb62b974dd7aa1e1f341f4deefdfbf009254bd0cb4e594d4a7fe1dc1b"
---

# Is authorization for agents actually a new problem

# Is authorization for agents actually a new problem


### If machine-to-machine authorization works under fixed assumptions, what changes with agents?


[Alter](https://substack.com/@srikardandamuraju)


Apr 01, 2026


There is a lot of hype around agents and rightfully so. It is a technology still very much in its infancy, with believers pointing to the possibility of agents automating away vast swaths of knowledge work as we know it.


If true, the ramifications on how we think about human work and society are enormous. Much of the digital software we interact with today could become agentic. Given a shift of this magnitude, it is only natural that entirely new categories of products begin to emerge to support this world.


At the same time, some skepticism is warranted. Is agentic software really that different? Can the problems we solved in traditional software not be reapplied here? After all, we have already built systems for machine-to-machine interaction. Is this not simply another rendition of existing machine-to-machine auth problems?


While the initial skepticism is natural, agentic systems do introduce fundamentally different authorization challenges. To understand why, it is worth stepping back and examining how agentic systems differ from traditional software.


To be precise, while often referred to broadly as “auth,” the most significant new challenges for agentic systems lie in authorization rather than authentication.


##
What are agents


Traditional software is built around deterministic execution, a predefined set of rules that dictate how the system behaves. Each step in the flow is explicitly designed, and the system is expected to operate within a well-understood set of states. When a scenario is not accounted for, the system enters a state that is typically classified as a bug. Much of the software engineering lifecycle, from design to testing, is centered around ensuring that these states and transitions are well understood and controlled.


Agents represent an evolution of this model. At a high level, most agents consist of a probabilistic, non-deterministic decision-making component, often an LLM, that determines what to do next, and a set of tools, APIs, services, or functions that are no different from traditional deterministic software, executing predefined logic. The difference is not in the tools themselves, but in how they are selected and composed. Rather than following a fixed flow, the agent dynamically decides which tools to use and in what sequence.


This introduces a fundamental shift. Instead of being purely deterministic, agentic software combines deterministic modules with probabilistic decision-making that drives key parts of execution. The system is no longer just executing predefined steps, it is deciding what to do next.


As a result, agentic systems can enter states that were not explicitly anticipated during development, allowing them to handle problems that are impractical to fully specify ahead of time.


To make this more concrete, consider a simple agent tasked with scheduling a meeting:


> In a traditional software system, the flow might look like:
>
>
> -
>
>
> parse structured user input and extract required fields (date, time range, participants)
>
>
> -
>
>
> validate inputs and map them to predefined parameters
>
>
> -
>
>
> query calendar systems using fixed API calls with known schemas
>
>
> -
>
>
> compute availability and suggest time slots based on predefined rules
>
>
> -
>
>
> confirm selection and create the event via a deterministic booking flow
>
>
> Each step is explicitly defined, with well-understood inputs, outputs, and state transitions. The system operates within a predefined flow, where every possible path has been anticipated and implemented ahead of time.
>
>
> Now consider an agent performing the same task. Instead of following a fixed flow, it might:
>
>
> -
>
>
> interpret vague instructions like “sometime early next week” and convert them into concrete time ranges for tool execution
>
>
> -
>
>
> infer relevant participants from context and resolve identities via directory APIs
>
>
> -
>
>
> call calendar and availability APIs across systems to assemble a real-time view of schedules
>
>
> -
>
>
> use messaging tools to propose times, gather responses, and iteratively resolve conflicts
>
>
> -
>
>
> adapt mid-execution through additional tool calls, updating holds, canceling events, or re-querying availability as constraints evolve
>
>
> Each of these steps is not hardcoded, but dynamically selected and executed via tool calls, with the agent continuously deciding what to do next based on context and intermediate results.


What this really means is that agentic software operates over an effectively unbounded and non-enumerable state space driven by probabilistic decision-making


**.** In contrast, traditional software operates over a bounded and enumerable state space driven by deterministic execution.


This means we are no longer authorizing a fixed set of actions, but delegating decision-making authority to a system whose behavior cannot be fully specified ahead of time.


Now to add to this, there is a second, equally important challenge:


**identity and delegation.**


##
Agent identity and delegation


Now that we have established that agents operate over an effectively unbounded and non-enumerable state space, a second implication follows. Agents begin to resemble entities with their own decision-making capability, rather than traditional software executing predefined logic.


This creates an interesting challenge in how we model agent identity.


In traditional systems, software identity is typically represented through static accounts, often referred to as service accounts, which are tightly scoped and explicitly controlled. These service accounts are what allow software running in production to access third-party resources such as APIs and databases.


With agents, this model begins to break down. Static service accounts are no longer sufficient, as agents do not operate under a single identity. They have their own system identity, but also act on behalf of users, dynamically exercising delegated permissions.


This distinction is subtle, but critical. Agents operate across contexts and make decisions over time while using user-level access.


As a result, authorization is no longer evaluated against a single identity. The system must account for both the agent’s identity and the end user’s identity, since the agent is making decisions using the user’s permissions.


> **The decision-maker and the permission-holder are no longer the same entity. The agent determines what to do, but does so using the user’s permissions.**


## **** Why traditional authorization models break for agents


To see why this breaks in practice, consider a simple scenario.


An agent is given access to a user’s calendar, email, and internal systems with the goal of “scheduling and coordinating meetings.” In a traditional system, the scope of actions would be tightly defined, with clear boundaries on what can and cannot be done.


In an agentic system, however, the agent is not just executing predefined actions. It is deciding which actions to take based on context, while operating under the user’s identity.


The challenge is not in any individual action. Reading emails, accessing calendars, sending invites, or modifying events are all valid operations within the scope of the task.


The problem emerges from how these actions are combined. Because the agent is continuously deciding what to do next, the sequence, timing, and composition of these actions are not fixed ahead of time. The effective scope of the system emerges during execution rather than being predefined.


In practice, this means an agent could:


-


access data that was not originally intended to be part of the task


-


take actions that exceed the user’s expectations


-


chain together permissions across systems in ways that were never explicitly designed


The issue is not that any individual action is unauthorized, but that the combination of actions leads to outcomes that are difficult to anticipate or constrain.


Traditional authorization models assume that actions and execution paths can be defined ahead of time. Agentic systems violate this assumption.


> **We are no longer authorizing actions. We are authorizing systems that decide which actions to take.**


## Implications for authorization in an agentic world


This shift has significant implications for how we design authorization systems in an agentic world. It is no longer sufficient to define permissions as a static set of allowed actions tied to a single identity.


Instead, authorization must account for systems that act over time, make decisions dynamically, and operate under composed identities. This requires new models that can reason about intent, constrain behavior across sequences of actions, and provide stronger guarantees around how delegated authority is used.


The problem is no longer just controlling access, but governing how autonomous systems exercise it.


This is the problem space we are building for at


[Alter](http://alterauth.com/) .


**Thanks for reading. If this resonated, consider subscribing for future posts.**
