---
schema_version: "1.0.0"
document_id: "2ca4eb9de030cc42b5372188b59d7b885840481d836ba5521a218043c1e7f9af"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/agents-write-code-they-dont-do-software-engineering/"
published_at: "2026-03-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:b8db7b5f112df77737a3eeb7d58aec5df8c39623391beceaf6b6dcaa03fabec9"
---

# Agents Write Code. They Don't Do Software Engineering.

*This article was originally published on[The New Stack](https://thenewstack.io/ai-agents-software-engineering/) .*


---


Long-running and background coding agents have hit a new threshold. When an agent runs for hours, manages its own iteration loop, and submits a pull request without hand-holding, it stops being a tool you invoke and starts being more like a worker you assign tasks to. Like any worker, the question isn’t how closely you supervise them. It’s what work you assign them in the first place.


We are all figuring this out in real time, and I see many teams making an understandable but critical error. They tune the autonomy dial, adding more review checkpoints or removing them, when the actual variable that matters is which categories of work agents should own versus which categories developers should own. That distinction isn’t about risk tolerance. It’s about capability boundaries.


## Code writing and software engineering are not the same job


Writing code is pattern recognition. Take what’s been done before, apply it to a new context, and scaffold it out. Large language models are exceptional at this because that’s exactly what they do: recognize and reproduce patterns from massive corpora of prior work.


Software engineering is something else. It’s trade-offs. Constraints. Decisions that require context no model has access to: your business domain, your product strategy, your customers, your technical debt, the conversation your team had last week about why you chose one approach over another.


Most teams split on importance or risk tolerance. The real divide is between work that can be reasoned from prior patterns and work that requires context, strategy, and judgment that lives outside the codebase.


## What developers actually own


The work that actually requires developers is more specific than “anything important,” but it doesn’t reduce to a tidy list of task types. It cuts across every part of the engineering process.


> “Agents can read code. They cannot read the room.”


Developers own the work where the right answer depends on context that doesn’t exist in the codebase. Product strategy, business constraints, team dynamics, conversations in Slack threads, and architecture reviews are part of the history of why a system is built the way it is. Agents can read code. They cannot read the room.


Developers own the work where the risk profile is ambiguous, or the failure modes are hard to predict. Some changes cascade in ways that depend on organizational boundaries, deployment timing, or data contracts baked into a system over years of iteration. Evaluating correctness in those cases requires judgment that no model can drive from code alone. The higher the uncertainty, the more you need someone who understands not just what the code does but why it was written that way.


Developers own the work where the output is a decision, not an artifact: what to build, what to cut, and which technical bets to place six months from now. Agents can generate options. They cannot tell you which option is right for your situation because “right” depends on factors that reside in human heads and organizational contexts, not in training data.


And all of this is still evolving. As teams invest in making context more explicit through better documentation, clearer contracts, and more structured decision records, the boundaries shift. Work that once required a developer’s institutional knowledge becomes accessible to an agent. But the frontier of unstructured, high-judgment work keeps moving, too, and that’s where developer time is most valuable.


In distributed systems, this problem gets worse. The more services you spread across multiple teams and codebases, the more that critical context lives outside any single codebase. A change to one service’s event schema can break downstream consumers in ways that no test in that service’s own suite will catch.


Also: The agent doesn’t know what it doesn’t know. The developer on that team does — not because they wrote the code — but because they were in the meeting where the schema was agreed upon. For cloud-native teams, this scales badly: the more services, the more implicit contracts, and the more context that only people carry.


## Where agents deliver the most value


There is a mountain of work in every codebase that is a waste of human brainpower. Boilerplate, scaffolding, repetitive refactors, unit test generation, configuration templating, and data formatting. This work is rote, mechanical, and can be reasoned entirely from prior patterns. Agents should own it.


Once a developer specifies the interface, contract, and expected behavior, an agent can implement faster and more consistently than a developer could. The implementation is the repeatable part. The reasoning that precedes it is not.


> “The developers who thrive in this model won’t be the ones who write the most code. They’ll be the ones who make the best decisions.”


Iteration speed matters here, too. Generating multiple implementations, running test suites, checking contract conformance: agents do this at a pace no developer can match. Circle CI’s State of Software Delivery Report found that throughput bottlenecks most commonly appear in the feedback and validation loop, not the code-writing phase. Agents compress that loop significantly when the acceptance criteria are clear and they have access to the runtime environment and tools they need to validate their work.


The developers who thrive in this model won’t be the ones who write the most code. They’ll be the ones who make the best decisions about what to build and how to architect it, then hand off the execution to agents that can move faster than any human.


## A three-tier model for dividing the work


In our team, we have found it useful to implement a rough framework for bucketing engineering work categories to make decisions about how it is distributed between agents and developers.


### Tier 1: Agent-led, developer-reviewed


Tasks where agent execution is high-confidence and the output is self-verifiable. Boilerplate generation, configuration templating, adding endpoints within established patterns, running and reporting on test suites, and scaffolding new services or modules from existing conventions. The developer reviews the output, but the agent owns the work.


Routing this to developers wastes your most expensive resource. This category should expand as teams get better at making their patterns explicit and testable.


### Tier 2: Agent-assisted, developer-guided


Tasks that require context beyond the codebase to validate. The agent implements, but the developer defines the scope, constraints, and success criteria. Feature work within a well-understood domain, refactoring within established boundaries, and test implementation for developer-defined strategies fall here.


The developer provides the engineering judgment. The agent provides the implementation throughput. Most feature work, across any architecture, falls into this tier.


### Tier 3: Developer-led, agent-supported


Tasks where the core work is judgment, not implementation. Architectural decisions, cross-boundary contract changes, debugging emergent failures, and defining what to build next. Agents can assist with subtasks: drafting proposals, analyzing logs, and generating candidate implementations for evaluation. But a developer must drive because the work itself is reasoning, not pattern execution.


The distinction from Tier 2 is that the developer isn’t just validating output. They’re doing the intellectual work that no amount of training data can substitute for.


## The cost of getting the split wrong


Most teams I speak to are either under-allocating or over-allocating work to agents. Both are expensive mistakes.


Over-allocation is the more visible failure. Push agents into Tier 3 work, and they produce output that requires significant rework because the necessary context wasn’t available to them. The rework cost is real, but the opportunity cost is worse: developers who should be doing Tier 3 work are instead reviewing and correcting agent output that shouldn’t have been delegated in the first place.


Under-allocation is quieter but equally damaging. Teams that default to developer-owned work because agent output seems uncertain are paying developer rates for Tier 1 tasks. Developer time is the highest-cost resource on the team. Burning it on pattern-execution work that agents could handle is a slow drag on velocity that compounds over months.


This is why many teams adopting agentic workflows see limited gains or even slight decreases in merged code throughput. They haven’t solved the allocation problem. They’ve added a new tool without changing how work gets distributed.


## Audit the work, not just the agents


The question isn’t whether agents replace developers. It’s what the right engineering model looks like when agents handle the mechanical work, and humans focus on the strategic work.


The teams navigating this well don’t just audit their agents. They audit their work. They ask which tasks could be agent-led if the boundaries were made explicit, then invest in making those boundaries explicit. That investment returns developer time to the high-judgment, context-dependent work that agents won’t own well anytime soon.


The answer won’t come from the AI labs. It’ll come from engineering teams actually building software this way every day, figuring out where the line is through practice, and learning what their specific codebase, team, and domain require on each side of the divide. Making the agent side of that line reliable means giving it a way to[validate changes against real systems](https://www.signadot.com/validate-ai-generated-code-kubernetes/) before a human reviews them.
