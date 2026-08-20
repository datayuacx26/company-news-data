---
schema_version: "1.0.0"
document_id: "9e7ea8cd51807dec610b63adf8e4c0979e8f04c80191adcbad091ad351960185"
company_key: "yc-concourse"
company: "Concourse"
source_id: "yc-concourse-news-import-c7979aaf4bde"
canonical_url: "https://www.concourse.ai/insights/meet-anvil-coding-agent"
published_at: "2026-07-28T13:00:00+00:00"
first_seen_at: "2026-07-29T00:20:17.786941+00:00"
fetched_at: "2026-07-29T00:20:18.962982+00:00"
content_hash: "sha256:1e1a202a02298876a1b81b13919bbd987542077b9aead1df85923dbfb0022f74"
---

# Meet Anvil: The Coding Agent That Lives Where We Work

## Coding agents changed where work begins


Anvil did more than help engineers write code faster. It changed *how engineering work starts* .


A task might begin as a well-defined Linear issue, complete with acceptance criteria and links. Or it might begin as a half-formed question in Slack: “Can you look into this?”, “Could we build an integration for that?”, or “Why is this happening in production?”


In either case, the handoff is the same. Assign the issue to Anvil or mention it in the conversation, and Anvil takes responsibility for the execution: gathering context, exploring the relevant systems, making a change when needed, running the appropriate checks, and bringing a concrete result back to the team, whether that is an answer, an RFC, or a pull request.


There is no separate agent workspace people have to remember to visit. No new queue to monitor. No need to copy a Slack conversation into another tool and rebuild the context from scratch. Delegating to Anvil happens in the same places where we already delegate work to each other.


That small interface decision changed how the agent was used. In its first few months, the results were hard to ignore:


- **700+** sessions initiated
- **40%** of our Linear issues delegated to Anvil
- **300+** PRs merged to production (166 in June alone)


Those numbers matter, but the more interesting story is why adoption happened. Coding ability was necessary, but it wasn't sufficient. The breakthrough was putting the agent between the structured world of Linear and the conversational world of Slack, and letting it carry work from an initial request to a concrete, reviewable result.


## The bridge between Linear and Slack


Engineering work begins in two very different modes.


The first is *structured* . A task has been discussed, scoped, and turned into an issue. It has an owner, a status, acceptance criteria, and a permanent place in the team's backlog. For us, that system of record is Linear.


The second is *conversational* . Someone notices a problem, asks a question, shares an idea, or starts debugging in a Slack thread. The request may not be fully formed yet, and important context emerges through replies, links, corrections, and follow-up questions.


Both modes are essential, but coding agents are often introduced through only one of them. An agent that lives exclusively in an issue tracker receives clean, structured tasks but can miss the conversation that produced them. An agent that lives exclusively in chat can join that conversation, but the work can disappear into a thread without durable ownership, status, or follow-through.


**Anvil connects the two.**


*Anvil listens in Linear and Slack, runs the work on managed infrastructure, and acts across the tools the team already uses: GitHub, Notion, and Linear itself.*


When a Linear issue is assigned to Anvil, the issue becomes the starting point. Anvil reads its description and linked context, explores the relevant code, makes the change, runs the appropriate checks, opens a pull request, and moves the issue forward.


When Anvil is mentioned in Slack, it begins with the conversation instead. It can answer a question, investigate a problem, do research, or start a complete engineering task using the context already in the thread. If the request turns into substantial work, the conversation can become a tracked Linear issue rather than staying buried in chat.


From there the work continues through the normal engineering systems. Linear preserves ownership and status. GitHub holds the resulting code and pull request. Slack remains the place where people ask questions, clarify intent, and see what happened.


The bridge is not simply copying messages between applications. It is carrying context across different modes of work. Slack is where intent is discovered and ambiguity is resolved. Linear is where a commitment becomes visible and accountable. GitHub is where the implementation becomes a reviewable artifact. Anvil connects those stages so the team does not have to reconstruct the task at every handoff.


That is what makes delegation feel natural. People do not have to decide they are going to “use an AI tool.” They assign an issue or ask for help in a conversation, exactly as they would with another member of the team.


## Four ways we use it


### 1 · Shipping a real migration


We handed Anvil a genuine backend migration as an ordinary Linear issue: move a workflow build off one-shot HTTP requests and onto a durable execution model. Anvil read the ticket and its linked context, explored the relevant code, made the change across the files involved, ran the checks, and opened a pull request. Eight files changed, one PR, and the issue moved to Done. This was meaningful, multi-file engineering work, not a cosmetic edit or a demo task.


*A migration delegated as a Linear issue: started by Anvil, eight files changed, PR opened, marked Done.*


### 2 · Turning a Slack thread into shipped work


This one started as a single message in a channel: “can you build a new integration? let's make it live.” The requirements weren't fully formed yet; they took shape through the replies that followed. Anvil used that evolving thread as its context, then turned the request into tracked work and, ultimately, a pull request. The conversation where the intent emerged stayed connected to the engineering result, instead of the request and its outcome becoming two disconnected pieces of work.


*Kicking off integration work with a single Slack mention, then iterating in the reply thread.*


### 3 · Researching before committing to code


Here the handoff was an open-ended question rather than a defined task: how could we make configurations swappable at runtime? Anvil investigated the relevant systems in parallel and synthesized what it found. The result wasn't code but a full RFC document the team could read, question, and refine before anyone committed to an implementation. The artifact gave the team something concrete to evaluate before a line of code was written.


*From an open question to a written RFC: research delegated end to end.*


### 4 · Starting an investigation from your phone


The request came in from a phone, away from any laptop: “How many agents have we run in production today?” Anvil queried our observability data, cross-checked more than one source, and came back with an actual number and the caveats around it, not a link to a dashboard. The unlock is immediacy: an engineer can begin a real investigation from anywhere, without waiting to get back to a desk or manually navigating several observability tools.


*“How many agents have we run in production today?” Answered from a phone, with the agent pulling and cross-checking live observability data.*


## We chose where to build


Anvil needed an environment where an agent could work for more than a single response. It needed to read files, run commands, execute code, use tools, browse the web, and maintain state while completing longer tasks.


We could have built that infrastructure ourselves. We chose not to.


Anvil runs on **Claude Managed Agents** , which provides the agent harness and managed execution environment. It gives Anvil a place to perform long-running work, use tools, interact with files, and maintain a session while it works toward an outcome.


That let us focus on the layer that was specific to our team. We built the layer around that runtime: how requests enter through Linear or Slack, how context follows them, how Anvil connects to the systems where engineering work happens, how tasks move through their lifecycle, and how results return to the people who delegated them.


This distinction became central to how we thought about the product. The managed runtime provides the ability to execute. Anvil provides the workflow around that execution: where work begins, what context it receives, how people interact with it, where the resulting artifact belongs, and how the task reconnects with the team.


Model capabilities and agent runtimes will keep improving. By building on that infrastructure rather than recreating it, those improvements become a tailwind. We can keep concentrating on the part that makes Anvil useful inside a real organization: turning an informal request into work that is visible, actionable, and easy to pick up again.


The technology made autonomous execution possible. The bridge between Linear and Slack made it something people actually used.


## What we learned


### Distribution is part of the product


An agent can be extremely capable and still go unused if people have to remember where it lives. Every additional step between noticing work and delegating it creates friction: opening another application, starting a session, copying context, rewriting the request, and later returning to check what happened.


Putting Anvil in Linear and Slack removed most of those decisions. People encounter the agent at the moment the work appears, and adoption did not require learning a new workflow; it extended ones they already understood. For an internal agent, where it is available can matter as much as what it can do.


### Conversation and systems of record serve different purposes


Slack and Linear are not competing interfaces for the same job. Slack captures emerging intent: the questions, links, disagreements, and incremental clarification through which a task takes shape. Linear preserves the resulting commitment by giving it ownership, priority, status, and a durable place in the plan.


The lesson was not to choose one interface over the other. It was to let work change shape without losing its context. A conversation can become a commitment; that commitment can become a pull request or another concrete artifact. Anvil carries the work across those transitions rather than trying to replace the systems on either side.


### A useful agent returns an artifact


The value of an agent session is not the length of the conversation or the number of actions it took. It is what the team can do with the result. For engineering work that usually means a pull request; for research, an RFC; for debugging, a sourced answer with enough evidence to make a decision.


These artifacts give the work a life beyond the session. They can be reviewed, discussed, improved, shared, and accepted through the team's existing processes. That reframed Anvil for us: its job is not merely to respond intelligently, but to carry work forward and return something concrete.


## What comes next


Anvil began with a simple bet: a coding agent becomes far more useful when delegating to it feels as natural as delegating to another person.


The first phase was about closing the loop from request to result. A conversation in Slack or an issue in Linear could become an investigation, a document, or a pull request without someone manually carrying the context between every system.


The next phase is about making that handoff more complete. We want Anvil to preserve context across longer timelines, resume work when new information or feedback arrives, and use the lessons from reviews and completed tasks to approach future work more effectively. The emphasis is not simply adding more places to invoke Anvil. It is making the work more continuous after the first handoff.


The opportunity is to shrink the distance between noticing work and meaningfully starting it: to begin investigations while an engineer is away from their desk, turn ideas into tracked work before their context disappears, and return more tasks to the team as concrete artifacts ready for judgment.


Writing code was the first capability. Building a reliable path from request to result is the larger opportunity.


That is what we are building with Anvil.


*Anvil · built at Concourse*
