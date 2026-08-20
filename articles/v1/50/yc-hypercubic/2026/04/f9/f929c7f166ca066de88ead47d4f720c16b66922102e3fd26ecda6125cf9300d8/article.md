---
schema_version: "1.0.0"
document_id: "f929c7f166ca066de88ead47d4f720c16b66922102e3fd26ecda6125cf9300d8"
company_key: "yc-hypercubic"
company: "Hypercubic"
source_id: "yc-hypercubic-news-import-1849a5c8a5f4"
canonical_url: "https://www.hypercubic.ai/insights/replicate-the-environment-not-the-knowledge"
published_at: "2026-04-08T19:44:00+00:00"
first_seen_at: "2026-07-21T23:26:19.477967+00:00"
fetched_at: "2026-07-28T22:15:57.305389+00:00"
content_hash: "sha256:89d12cc0bb02828ace0e66e85ffeb21ac206bb6f8a8b38ab02f858d7560293a0"
---

# Replicate the Environment, Not the Knowledge

There is a vast amount of specialized knowledge that is not on the internet. It is not in any book or database either. It is distributed across human brains, encoded in running systems, and embedded in processes that nobody fully understands.


Friedrich Hayek articulated this in 1945 better than anyone has since:


> “There is beyond question a body of very important but unorganized knowledge which cannot possibly be called scientific in the sense of knowledge of general rules: the knowledge of the particular circumstances of time and place.” ** \[1\]


He was writing about economics, but the observation is far more general. Much of the world runs on local, tacit, distributed knowledge. Think of the logistics coordinator who makes a living by knowing about temporarily unallocated backhaul capacity on a trucking lane. The estate agent who spots fleeting opportunities before anyone else does. The arbitrageur who profits from local price differences nobody has noticed yet. Each possesses knowledge that, as Hayek put it, “by its nature cannot enter into statistics and therefore cannot be conveyed to any central authority in statistical form.”


This is the knowledge problem. Not a shortage of information, but a recognition that the most important information resists centralization.


It also describes, almost perfectly, what happened inside the world's largest software systems.


## The knowledge locked in running code


Mainframe COBOL systems process trillions of dollars in transactions every day. They were built over decades by thousands of developers, each responding to their own particular circumstances: a regulatory change, a production incident on a Tuesday night, a customer complaint about a rounding error in quarterly interest.


Nobody designed these systems as a whole. They grew. Layer by layer, patch by patch, each developer contributed local knowledge to a shared codebase. Business rules governing transaction approvals, interest calculations, exception handling, and account state transitions were rarely written down cleanly. They were embedded in code, data layouts, calling conventions, operational habits, and production fixes. As the original authors retire, much of that knowledge retires with them.


This is Hayek’s knowledge problem, made executable.


The knowledge is too distributed across millions of lines of code, too implicit in program interactions, and too dependent on specific data and state to be cleanly captured by any one person or document. No one understands it in full. The system does. And yet it runs. Every day it processes the right transactions, applies the right rules, and produces the right results.


## Why the standard approach fails


The conventional approach to mainframe modernization is, in Hayekian terms, a form of central planning. Hire a large consulting team. Read the code. Document the business rules. Rewrite the system from the documentation.


This fails for the same reason central planning fails. The abstraction loses the very details that matter.


A consultant reads` IF WS-FICO-SCORE < 300` and writes "FICO scores below 300 are rejected." But the real knowledge is not in that sentence. It is in the behavior of the system under actual conditions. What happens when that check fails halfway through a transaction? What happens when the communication area contains a particular state? What happens when a cursor is positioned at a specific record and an exception path is triggered two programs later? That is where the real rule lives.


The knowledge lives partly in code, but more importantly in the behavior of the running system under constraint.


A common but naive response is: can’t you just point a frontier LLM at the COBOL and ask what it does? A frontier LLM pointed at a COBOL file can often tell you what a paragraph appears to do in isolation. It cannot reliably tell you what happens when seventeen programs interact through shared byte buffers, inter-program communication areas, transaction boundaries, and production-shaped data. Reading is not enough. The important knowledge is expressed through execution.


## Build the environment, let AI explore


So the alternative is to replicate the environment in which that knowledge is expressed.


Rather than asking humans to read the code and reconstruct the rules, you build a faithful, instrumented replica of the legacy system: one that behaves like the original but runs in an environment where every branch, every mutation, every intermediate state, and every interaction can be observed. Then you let AI operate inside that environment, forming hypotheses, running experiments, observing results, and revising its understanding.


This is a fundamentally different orientation. You stop treating understanding as a reading task. Instead, you build a world in which understanding can be discovered through interaction.


In a[previous essay](https://www.hypercubic.ai/insights/code-comprehension-at-scale-learning-from-the-bitter-lesson) , I argued that Rich Sutton’s Bitter Lesson \[6\] applies to code comprehension. General methods that leverage computation tend to outperform methods built around human-crafted abstractions. Summarization, RAG, manually designed ontologies, knowledge graphs: these are all attempts to compress understanding through heuristics that work up to a point and then break at sufficient scale and complexity.This is the Bitter Lesson applied to modernization.


Do not build ever-cleverer compression schemes for tacit knowledge. Build the environment in which that knowledge is expressed, and let exploration—using general methods like search and learning—do the work.


In practice, that means building a behavioral twin of the legacy system and giving AI agents the tools to interact with it: navigate screens, send inputs, inspect data, observe execution traces, compare outcomes, and test conjectures about business logic against actual system behavior.


Is this just testing in fancier clothing? Not really. Testing starts with a specification and checks whether the system satisfies it. This starts without a specification and tries to discover one. The direction is reversed. It is closer to the scientific method than to QA.


## Why build the twin instead of reading the code?


At first glance, building a faithful replica sounds even harder than understanding the code directly. In one sense, it is. But it is a different kind of hard.


Reading a large legacy system and reconstructing its true behavior is a judgment problem. It depends on interpretation, tacit context, and heroic human effort. Building a faithful twin is an engineering problem. The goal is to make fidelity a property of the system you build, not a byproduct of someone’s intuition.


That means translating the legacy system into a representation whose behavior can be checked systematically, then tightening equivalence through formal methods and continuous differential validation. Run the same inputs through the original system and the twin. Compare outputs, traces, and side effects. Where they diverge, close the gap. Over time, you turn faithfulness into something measurable.


This matters because once the infrastructure exists, each new system becomes explorable by construction. We trade a brittle, manual, irreducibly contextual process for one grounded in repeatable engineering.


## The AI labs already understand the pattern


One of the clearest lessons from modern AI is that environments beat static datasets.


DeepMind did not achieve superhuman Go by building a giant database of expert commentary. They built a game engine and let the agent learn by acting within it. When AlphaGo Zero \[2\] removed human game data entirely and learned through self-play, it surpassed the earlier system. The environment was the curriculum.


The same pattern shows up elsewhere. SWE-bench \[4\] evaluates coding agents inside real repositories where they can inspect code, make edits, and run tests. WebArena \[5\] evaluates web agents inside functioning web applications rather than static datasets of browser traces. The big labs have all moved toward computer-use systems where the model operates a real interface with screenshots, mouse movements, and keyboard actions. In each case, capability comes not just from more data, but from acting inside a structured world that produces feedback.


The pattern is already visible: if you want a system to master a domain, descriptions are often a poor substitute for the domain itself.


That principle applies just as much to enterprise software as it does to games, code repositories, or browsers.


If you want AI to understand a forty-year-old banking system, do not just hand it source files and ask for a summary. Give it an environment to operate in.


## Fidelity is the whole game


Of course, this only works if the replica is faithful. If the twin diverges from the real system, the extracted understanding will be wrong.


That is why verification is not just a nice-to-have, but the entire game.


The twin must be validated continuously against the original system through parallel execution. The same inputs should produce the same outputs, the same key state transitions, and the same observable behavior. Every discrepancy is useful. It reveals where the model of the system is incomplete and where the environment must become more precise.


Done properly, the environment becomes a progressively sharper instrument for extracting understanding.


This is what we are building at Hypercubic: not documentation tools, not static code analysis platforms, but faithful, explorable environments where AI can discover the knowledge that was never written down.


We will have much more to say about the specifics soon.


## References


1. Hayek, F.A. (1945).[The Use of Knowledge in Society](https://www.econlib.org/library/Essays/hykKnw.html) . American Economic Review, 35(4), 519-530.
2. Silver, D. et al. (2017).[Mastering the game of Go without human knowledge.](https://www.nature.com/articles/nature24270) *Nature* , 550, 354-359.
3. Open Ended Learning Team, DeepMind. (2021).[Open-Ended Learning Leads to Generally Capable Agents](https://arxiv.org/abs/2107.12808) .
4. Jimenez, C.E. et al. (2023).[SWE-bench: Can Language Models Resolve Real-World GitHub Issues?](https://arxiv.org/abs/2310.06770)
5. Zhou, S. et al. (2023).[WebArena: A Realistic Web Environment for Building Autonomous Agents](https://arxiv.org/abs/2307.13854) .
6. Sutton, R. (2019).[The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) .
