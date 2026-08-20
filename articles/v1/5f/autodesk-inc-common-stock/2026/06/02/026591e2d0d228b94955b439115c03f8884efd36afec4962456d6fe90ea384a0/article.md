---
schema_version: "1.0.0"
document_id: "026591e2d0d228b94955b439115c03f8884efd36afec4962456d6fe90ea384a0"
company_key: "autodesk-inc-common-stock"
company: "Autodesk Inc."
source_id: "autodesk-inc-common-stock-news-import-cd6ee38389ab"
canonical_url: "https://www.research.autodesk.com/blog/constrain-agent-not-user/"
published_at: "2026-06-30T13:08:42+00:00"
first_seen_at: "2026-07-21T08:37:27.992330+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:032abfbbf8c5bc3c58857d95246f856af1b93d861e868d4d6bb21fdd9b326316"
---

# Constrain the Agent, Not the User

# Constrain the Agent, Not the User


With the right constraints, coding agents can be both accurate and autonomous


Share page Link copied[Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.research.autodesk.com%2Fblog%2Fconstrain-agent-not-user%2F)[X](https://x.com/intent/tweet?url=https%3A%2F%2Fwww.research.autodesk.com%2Fblog%2Fconstrain-agent-not-user%2F)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fwww.research.autodesk.com%2Fblog%2Fconstrain-agent-not-user%2F)Copy Link


Patrick Nadeau


06/30/2026


Much of the advice around AI-assisted coding shifts the burden onto the user: go slow, break the work down, write a clear spec. This advice is useful, but it never sat well with me. It felt like a retreat from the promise of agentic coding.


What I found was that this compromise is not always necessary. With the right constraints in place, the work can be handed back to the agent — not by trusting it more, but by giving it something concrete to answer to.


I recently built a cycle-accurate emulator for a classic gaming console using agentic coding.


This required emulating three different chips and a bus, with exacting timing requirements. I chose this project specifically because it was hard. I had already started hand-coding the CPU implementation, and I estimated it would take a year to complete the whole project.


In other words, I expected it to push the AI to its limits.


It did, and each time the coding agent hit a wall, progress could only resume after I figured out the right way to ground it.


What I learned was that agentic coding is a game of constraints. The LLM proposes a solution, but for it to converge on a workable implementation, its output needs to be grounded in an external source of truth.


The demanding nature of this problem forced me to stop thinking of constraints in terms of mechanisms — prompts, specs, unit tests — and to focus instead on where error was accumulating and how to best mitigate it.


The key distinction was whether error was accumulating when the agent wrote code, when it interpreted an existing system, or when it elicited information from me.


Since these activities fail in different ways, overcoming each failure mode required a different type of constraint.


**The compiler analogy**


One analogy I often hear about coding with AI is that it’s just coding at a higher level of abstraction. Programmers who have misgivings about agentic coding often hear: “You don’t read the assembly output of your compiler, do you?”


The analogy seems to fit because both LLMs and compilers are black boxes that most of us never bother to look inside. But it falls down when you consider that compilers apply semantics-preserving transformations to their input. LLMs do not: they propose a plausible continuation of their input.


While I’m not claiming that LLMs are merely “stochastic parrots,” I am going to suggest that to get good results from them, we should methodologically treat them as such.


**The role of the harness**


The fact that LLMs are able to one-shot many programming problems causes many of us to make a category error when coding with AI. Since a single prompt is often sufficient to get a working program, this tempts us to try the same approach for more complex systems.


Most of us encounter an inflection point when moving beyond simple problems, where one-shot prompting can lead us in circles.


Fortunately, LLMs become much more reliable when embedded inside the feedback loop of an agentic system. A coding agent gives the model access to a compiler and other tools, allowing it to compile and run the code it generates and to iteratively self-correct. This simple feedback loop does a lot of the work.


This means that coding with an agent is more like guiding a stochastic search through a large space of possible solutions, and for that we need to introduce constraints.


These constraints can be expressed in prompts or specs, but these are often sidestepped by the model. Constraints become more effective when enforced by the harness.


The question then becomes: where is error accumulating, what constraint would ground that part of the loop, and how can that constraint be enforced?


This leads me to propose the following taxonomy of constraints:


- Generative: targets errors in artefacts the LLM writes.
- Interpretive: targets errors in how the LLM understands existing code.
- Elicitative: targets errors caused by missing, tacit, or contradictory human knowledge.


**Generative constraints**


Generative constraints help when error is accumulating in the artefacts being written.


Specs and unit tests are common generative constraints. The idea is that specs guide the model before it acts, while unit tests enforce specific behaviours in the generated code.


One limitation of specs is that they press natural language, with all its ambiguity, into doing the work of a formal one. As
[Edsger Dijkstra argued](https://www.cs.utexas.edu/~EWD/transcriptions/EWD06xx/EWD667.html) , formal languages such as programming languages and mathematical notation aren’t obstacles to thought, but tools for thought. By enforcing precision, they enable chains of reasoning that are awkward to express in everyday language.


The distinction is easy to show in miniature. Doug Slater gives the[following example](https://www.slater.dev/2026/05/why-english-will-never-be-a-programming-language/) : “Page the on-call engineer if the server is down or the response is slow and it’s during business hours.” — a sentence with two incompatible readings depending on how the clauses group together.


It might mean:


In the first reading, no one ever gets paged outside business hours.


In the second reading, a down server always pages someone, while a slow response only pages someone during business hours.


My emulator was full of rules like this, where even a single misplaced “and” or “or” would likely have caused games to freeze, crash, or misbehave in mysterious ways.


A full spec would have had to resolve dozens of such rules — effectively becoming a parallel implementation, written in prose — and would likely have sprawled over hundreds of pages.


**Test Oracles**


Sometimes an existing system can be turned into a generative constraint.


In writing my emulator, I had access to a reference implementation. I extracted the relevant parts from the old code base, and made it available to the coding agent through an API.


With every piece of implemented behaviour, the coding agent could now compare results against the older but tried-and-true implementation.


This turned out to be[very successful](https://keanw.com/2026/04/a-diary-of-an-agentic-retro-gamer-part-2.html) and enabled the AI to implement the sound and graphics chips with minimal supervision while staying faithful to the reference implementation.


Test oracles are likely to be very effective feedback mechanisms for migrating legacy code to new platforms.


But this only constrained the code the agent wrote. The next failure mode I encountered was not generative, but interpretive.


**Interpretive constraints**


Interpretive constraints help when error is accumulating in how the LLM interprets the meaning of existing code.


In working on the emulator, I came to a point where I tried to get the LLM to annotate a CP-1600 assembly listing. Because this is an obscure microprocessor from the 1970s, it is likely very under-sampled in the model’s training data. Attempts to have the LLM explain this code were[not successful](https://keanw.com/2026/04/a-diary-of-an-agentic-retro-gamer-part-3.html) . It was simply making stuff up.


Understanding code involves not only having to understand individual subroutines, but also how they interact when composed in long chains. When LLMs stumble on code interpretation, it can be harder to control because errors do not just accumulate, but also compound through inferential chains. A wrong explanation of just one subroutine can distort the interpretation of any other piece of code it interacts with.


I wasn’t able to make any headway until I added a debugger port to the emulator, which allowed the LLM to test each of its theories about the assembly listing against the internal state of the emulator. I instructed the AI to treat each of its theories about the assembly code as a hypothesis to be tested, and asked it to test each one against direct evidence using the debugger.


A similar approach might explain why Mythos is so good at discovering vulnerabilities. In[Project Glasswing: what Mythos showed us](https://blog.cloudflare.com/cyber-frontier-models/) Grant Bourzikas describes how Mythos enforces interpretive constraints directly in the harness, by having it write code probes to test its theories about the code under scrutiny.


Although my results using an interpretive prompt led to much better results than static analysis alone, I did later find errors in the annotations the LLM produced.


Since I had only prompted it to test its hypotheses, the AI had predictably sidestepped these instructions in a few places. Mythos suggests that interpretive constraints become much more powerful when enforced within the harness itself.


**Elicitative constraints**


Elicitative constraints help when error is accumulating because the agent is missing information which lives in people’s heads.


I ran into this on a project where we were asked to integrate several research prototypes into an LLM-powered workflow. Each prototype had a different owner, and the details of how they interacted were either underspecified or scattered across Slack threads, and half-remembered conversations.


I created an agentic project seeded with a rough description of the workflow, the components involved, their sequencing, and the owner of each component. The agent’s standing orders were, on startup, to ask who it was talking to, then to grill that person to resolve missing information, inconsistencies, contradictions, or unclear dependencies.


In effect, I had built a small requirements-elicitation harness: a chatbot whose sole purpose was to ferret out ambiguities in our evolving spec. It asked pointed questions like:


“Are entity IDs stable across graph regeneration after assembly modifications?”


After the researchers had taken multiple turns being interviewed in this way, a usable workflow specification started to emerge.


Other people seem to be converging on similar patterns:[Matt Pocock’s “grill-me” skill](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md) , for example, turns the agent into a structured interviewer whose job is to flesh out a plan before implementation.


**Conclusion**


These three domains — generative, interpretive, and elicitative — bleed into each other. Code which the agent writes will need to be read and interpreted later on, either in a separate session, or after a compaction event. Likewise, information elicited from a person becomes part of the agent’s working context and will influence future code outputs, interpretations, and questions.


My taxonomy has been useful to me, but I’d suggest that it’s less important than the habit behind it: when working with coding agents, ask where error is accumulating, and look for a way to ground that part of the loop.


Applied consistently, this approach rebalances the division of labour between user and agent, allowing more of the work to be handed back to the agent without the need for constant supervision.


* **This post condenses the methodological lessons from my longer four-part blog series:[Part 1](https://keanw.com/2026/03/a-diary-of-an-agentic-retro-gamer-part-1.html) ,[Part 2](https://keanw.com/2026/04/a-diary-of-an-agentic-retro-gamer-part-2.html) ,[Part 3](https://keanw.com/2026/04/a-diary-of-an-agentic-retro-gamer-part-3.html) , and[Part 4](https://keanw.com/2026/05/a-diary-of-an-agentic-retro-gamer-part-4.html) , covering the emulator build, the test oracle, the interpretive prompt, and the elicitation pattern.** *


Tags


- [Technical Insight](https://research.autodesk.com/blog/#/selected-filter/blog-content-type-technical-insight)
- [Artificial Intelligence](https://research.autodesk.com/blog/#/selected-filter/blog-research-area-artificial-intelligence)


React


- 12 Helpful
- 17 Insightful
- 19 Thought Provoking


Share page


## Related posts


[Read more about: From Vibe Coding to Procedural Automation](https://www.research.autodesk.com/blog/from-vibe-coding-to-prodedural-automation/) Article


2026


[From Vibe Coding to Procedural Automation](https://www.research.autodesk.com/blog/from-vibe-coding-to-prodedural-automation/)


Explore how procedural automation can improve productivity while…


[Read more From Vibe Coding to Procedural Automation](https://www.research.autodesk.com/blog/from-vibe-coding-to-prodedural-automation/)


[Read more about: Cognitive Architecture as a Blueprint](https://www.research.autodesk.com/blog/cognitive-architecture-as-blueprint/) Article


2026


[Cognitive Architecture as a Blueprint](https://www.research.autodesk.com/blog/cognitive-architecture-as-blueprint/)


Explore how cognitive architectures help engineers think beyond single…


[Read more Cognitive Architecture as a Blueprint](https://www.research.autodesk.com/blog/cognitive-architecture-as-blueprint/)


[Read more about: From Prediction to Prescription: Why Causality Matters for Responsible AI (Part 1)](https://www.research.autodesk.com/blog/from-prediction-to-prescription-why-causality-matters-responsible-ai/) Article


2026


[From Prediction to Prescription: Why Causality Matters for Responsible AI (Part 1)](https://www.research.autodesk.com/blog/from-prediction-to-prescription-why-causality-matters-responsible-ai/)


Explore the Responsible AI Pillars, historical motivations for causal…


[Read more From Prediction to Prescription: Why Causality Matters for Responsible AI (Part 1)](https://www.research.autodesk.com/blog/from-prediction-to-prescription-why-causality-matters-responsible-ai/)


Get in touch


Have we piqued your interest? Get in touch if you’d like to learn more about Autodesk Research, our projects, people, and potential collaboration opportunities


[Contact us](https://research.autodesk.com/?page_id=169)
