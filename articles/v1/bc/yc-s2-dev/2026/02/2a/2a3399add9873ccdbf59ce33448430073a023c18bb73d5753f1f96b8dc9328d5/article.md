---
schema_version: "1.0.0"
document_id: "2a3399add9873ccdbf59ce33448430073a023c18bb73d5753f1f96b8dc9328d5"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/distributed-ai-agents"
published_at: "2026-02-22T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:226df87046addff72342f8fba17292b1575b541a81d6606831cf8ce3b2c4e032"
---

# Coordinating adversarial AI agents

When you ask a single AI model to examine both sides of a difficult question, it can usually produce something that, at first glance, appears reasonable. Often the structure is there with a claim, counterclaims, tradeoffs, but the balance can seem artificial. This is understandable as the generation originates from the same context window.


In practice, many of us have learned to compensate for this. For example, after dealing with repeated mistakes and oversights from Claude Code, I've fallen into a pattern where one Claude session writes most code and other independent Codex (or Claude) sessions review it.


The underlying goal is complete context separation. My reviewer is not constrained by the same intermediate assumptions that shaped the original output, it is unbiased by the context, even if not the training data. What we are doing informally is creating parallel reasoning paths, separating generation from critique to produce structural disagreement.


We are rediscovering in small, practical ways what has long existed in other disciplines. In science, it appears as[blind peer review](https://en.wikipedia.org/wiki/Peer_review) . In forecasting, it is the[Delphi method](https://en.wikipedia.org/wiki/Delphi_method) . In risk analysis, it becomes competitive hypothesis testing. In law, as the[adversarial system](https://en.wikipedia.org/wiki/Adversarial_system) , where structured opposition precedes judgment.


These systems are all predicated on the idea that it is beneficial for participants to generate perspectives independently, and thus prevent cross-contamination, and only afterwards work toward consensus.


## Independence requires separate contexts


Telling a model to "consider the opposing view" doesn't create independence. By the time that instruction runs, it's already committed to a framing. The attention mechanism within a single forward pass may reweigh information, but it's all happening inside a shared state. Independence requires distinct contexts with no shared memory or conversational history.


To make that separation concrete, the unit of reasoning really can't be a single model invocation. It should be some bounded execution context with its own internal memory. We can consider the agents who share a bounded context to be forming a cohort. A single cohort might contain multiple agents — say, one that researches and another that critiques — but the key is that while they share context with each other, that context is isolated from other cohorts.


Only after cohorts have independently reached some stopping point should reconciliation occur.


## Enforcing isolation at the infrastructure layer


Capturing all context for a cohort on S2 streams allows us to enforce this independence at the infra layer.


In my setup, each cohort treats a stream as its shared, append-only journal. All agents within a cohort can append and read from their stream, allowing it to serve as both a message bus between agents, and as a longer-term context store. Streams can be created on demand, meaning cohort definitions and membership can also vary over the course of an experiment.


A record in a stream named` group/growth` is invisible to readers of` group/risk` . This makes isolation not an instruction but an intrinsic property of the system.


Within a cohort, agents maintain a persistent read session on their stream. When one agent appends a finding, it is pushed immediately to the others. Discussion unfolds as a live, ordered exchange grounded in specific prior records, enabling genuine multi-turn reasoning inside a cohort.


If a process crashes midway, the prior records remain. You can resume from the tail, or replay from the beginning to audit how a conclusion formed. Since S2[assigns timestamps](https://s2.dev/blog/timestamping#time-in-s2) to records, you can compare what different groups believed at a moment in time without merging their histories.


## parallax


To explore this architecture in practice, I built a simple tool.[parallax](https://github.com/s2-streamstore/parallax) is a CLI that accepts a research question, instantiates multiple cohorts of agents (Claude, Codex, or any agent capable of emitting structured output), and assigns each cohort its own stream in S2.


Then it kinda just lets it rip.


It also handles convergence. A moderator agent reads across cohort streams during the run, deciding when to steer groups, spawn breakouts, or transition phases. Once the run concludes,` parallax` performs a synthesis pass across the selected streams and writes a final report.


As it turns out, using streams makes topology experimentation effectively plug and play. Rather than hard-coding coordination patterns into prompts or agents, you can experiment with new architectures simply by reconfiguring the stream graph that connects them. The architecture admits patterns such as:


**Dynamic moderation:** A moderating agent can build coordination strategy on the fly by rewiring the stream topology as the discussion evolves. Since streams are just append-only logs, you can create, fork, or merge them at any point — the coordination structure doesn't have to be decided upfront.


**Applied epistemology:** Run thought experiments across AI models and observe how opinions evolve across independently formed positions and structured convergence rounds. This can serve as an empirical lab for testing moral hypotheses.


**Parallelized engineering workflows:** Make distributed debugging, building, and researching tractable. Because S2 streams are durable and shared, agents running across different machines, operating systems, or worktrees can coordinate without any additional infrastructure, and even join an active run via` parallax join` . The stream is the coordination layer, with concurrency controls like[fencing tokens](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html) and conditional appends baked in. You can also monitor those streams in real time and observe what each agent knows and when, which makes it possible to catch a group drifting off-course before the reasoning compounds into bad output.


## Examples


### [Adversarial cohorts](https://en.wikipedia.org/wiki/Adversarial_system)


```text
parallax   research   \
"What is S2's (s2.dev) competitive positioning in the   \
streaming infrastructure market, and what adjacent   \
opportunities should they pursue?"
```


The idea is that different personas, even on the same underlying model, can influence how reasoning unfolds.` parallax` assigns each cohort a distinct personality, so you get genuinely different angles rather than cosmetic variation.


Your browser does not support the video tag.


### [Delphi forecasting](https://en.wikipedia.org/wiki/Delphi_method)


```text
parallax   research   \
"What percentage of production AI agent deployments will   \
require durable stream infrastructure (not just ephemeral   \
message passing) for coordination, memory, and auditability   \
by 2028?"   \
--hint   "delphi forecasting, 3 rounds. Include one codex   \
panelist that analyzes existing open-source agent frameworks   \
to check what infrastructure patterns they actually use today,   \
grounding the forecast in code rather than speculation"   \
--groups   5
```


Five independent panels converged on 73% across three rounds starting from a wider spread. The Codex panelist focused on actual open-source agent frameworks and was the most bullish. It turns out agent frameworks already reach for queues and logs when things get stateful.


But anyway, we are betting on 100%, AI agents can sit this one out.


Your browser does not support the video tag.


## Conclusion


So what was the point of all this?


It was not just to introduce a tool or describe an architecture – but to also argue about how intelligence can be built and revive something older: the Socratic idea that understanding emerges from disagreement, not from a single uncontested source. Capability alone is not enough as structure also determines whether a result is convenient or credible. Distributed agents change the conditions under which conclusions are formed, as they can turn disagreement and parallel reasoning into a design choice.


If AI is going to shape complex engineering decisions, forecasts, or moral trade-offs, then how it arrives at those conclusions matters just as much as the conclusions themselves. Did the reasoning hold up when challenged from an independent context? Can you replay the full chain of thought? Streams give you that — every record is durable, ordered, and attributable. That's not a feature of the tool, it's the point of building on this kind of infrastructure.
