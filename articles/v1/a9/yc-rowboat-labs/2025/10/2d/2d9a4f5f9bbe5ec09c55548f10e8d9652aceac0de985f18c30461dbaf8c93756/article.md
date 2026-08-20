---
schema_version: "1.0.0"
document_id: "2d9a4f5f9bbe5ec09c55548f10e8d9652aceac0de985f18c30461dbaf8c93756"
company_key: "yc-rowboat-labs"
company: "RowBoat Labs"
source_id: "yc-rowboat-labs-rss-ecb33482b2dc"
canonical_url: "https://blog.rowboatlabs.com/flowcharts-vs-handoffs-a-simple-math-framing/"
published_at: "2025-10-06T17:42:00+00:00"
first_seen_at: "2026-07-25T21:47:42.719396+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:d3c5aec1b66b741c641c6d5fbd95ff8fc74840a1b2f429c74c663cc6aa3c3b55"
---

# Flowcharts vs Handoffs: a simple math framing

In this post we compare a multi‑agent system that supports handoffs to a visual flowchart builder such as n8n or OpenAI’s new agent builder. We assume no direct code is written.


TL;DR: In a handoff‑based system, any agent can pass control to any other agent and the entire conversation history moves with it. Mathematically, this gives you a compact way to create a dynamic call graph that grows with the task. A pure flowchart has a fixed graph. To get the same flexibility you must pre‑wire a large number of edges and conditions, which leads to combinatorial blow‑ups and brittle diagrams.


## **The core difference**


- Flowchart: You must either pre‑draw the handoff edges or centralize them in a router that holds the branch table. In both cases, the space of paths is bounded by what you design.
- Handoffs: The next step is chosen at runtime by an agent on the basis of the full history. The possible call graph is generated on the fly.


This sounds small, but it has first‑order consequences for expressiveness, complexity, and maintenance.


## **A simple example**


Goal: answer a user question with quality control.


Agents:


- Searcher: finds candidate passages
- Responder: drafts an answer
- Verifier: checks claims against sources


**Handoff version:**


1. Searcher(H) appends sources into H, chooses Responder.
2. Responder(H) drafts an answer, chooses Verifier.
3. Verifier(H) either accepts and halts, or hands back to Responder for polish and then halts, or routes to Searcher to gather more evidence.


Routing is expressed inside agents as a small rule: choose next agent based on coverage and confidence in H. The call graph that emerges depends on the question.


**Flowchart version:**


You must connect Searcher to Responder and Verifier, Responder to Verifier and back to Searcher, Verifier to both Responder and Searcher, add guards for confidence thresholds, and thread the right parts of H through all branches. As soon as you add a Citer agent or a Tool‑Caller agent, you add more cross‑edges and more guards. The diagram grows quickly.


## **What this means in practice**


- When the task is a fixed pipeline: Flowcharts are great. You get a clean, visual, linear path.
- When the task requires open‑ended delegation: Handoffs shine. You decide the next step from the actual conversation, not from a diagram that tried to anticipate every path.
- Failure handling: With handoffs, retries and escalation are rules on H. In a flowchart, they are extra edges and nodes that make the diagram harder to read.
- Extensibility: Adding a new agent in the handoff model means adding a function and a small routing rule. In a flowchart, you usually add multiple edges and conditions in several places.


##
**Two models in one minute**


###


**1) Flowchart model**


A visual workflow is a directed graph


G = (V, E)


with a token of control that moves along edges. Edges and branching conditions are fixed at design time. You can add loops and conditionals, but you must lay out the branches explicitly.


**2) Handoff model**


There is a finite set of agents A = {a1, a2, ..., an} and a shared conversation history H - full dialog plus agent outputs and metadata.


Each agent is a function on history:


```text
fa: H -> (H', next_agent ∈ A ∪ {halt})
```


When agent a runs, it updates the history to H' and returns the next agent to receive control. The key is that any agent can choose any other agent based on the actual content of H, and the entire history H always moves with control.


You can think of this as continuation passing with state: control is passed along with the full context, not just a single output.


## **Flexibility without pre‑wiring**


This is where the models diverge.


- **Dynamic routing without enumeration** : In the handoff model, “pick the most relevant next agent given H” lives inside each agent. In a flowchart, you must compile this into explicit branches. You can wire a full mesh with n*(n‑1) edges, or use a hub‑and‑spoke router with ~2n edges but an n‑way branch table that grows with every new agent. Either way, the flowchart accumulates branching logic in the diagram, while the handoff model keeps routing as a small function over H.
- **History is first‑class** : Handoffs read the entire history H. Flowchart conditions typically read local outputs or a few variables. You can simulate history by threading variables across many edges, but diagrams become noisy and fragile.
- **Cost model** : A handoff system has description cost about O(n) for n agents: define each agent and its routing rule. The runtime call graph grows only as needed for a given input. A flowchart that permits the same choices concentrates at least O(n) branch cases in routers and often approaches O(n^2) edges or cases as you add pair‑specific guards, retries, or return‑to‑caller paths. As tasks become more open‑ended, handoffs add states in the data (history in H), while flowcharts add states in the diagram (more nodes and edges).


##
**A little bit of theory behind this**


- **Static vs dynamic topology** : A flowchart’s topology is fixed at design time. A handoff system generates its topology at run time from H.
- **State location** : Flowcharts tend to encode state in the graph (more nodes and edges). Handoffs keep more state in the data (the evolving history H), which agents can read.
- **Closure under composition** : If agents are functions on H, then composing agents by handoff is just function composition with state. Adding a new agent is adding a new function. You do not need to rewire the whole system.


You can simulate one model inside the other, but the description length is different. For open‑ended tasks, a handoff system often needs far less wiring to describe the same behavior.


## **Conclusion**


A single primitive changes the game: handoff(agent, history). When any agent can pass control to any other agent and the whole conversation history travels with it, you get a compact, dynamic way to express complex behaviors. Flowcharts can emulate this only by drawing many edges and conditions in advance, which makes them harder to build and harder to maintain as the problem becomes more open‑ended.


We built Rowboat around handoffs using OpenAI’s Agents SDK. The code is[open-source](https://github.com/rowboatlabs/rowboat?ref=blog.rowboatlabs.com) .
