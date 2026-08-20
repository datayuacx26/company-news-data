---
schema_version: "1.0.0"
document_id: "569f7ec964683a6b5ba8266337edf95b495d7c31a1a0ea59e25e23038b21c70b"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/you-cannot-verify-what-you-cannot-reason"
published_at: "2026-01-05T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:df93f26a5d26c24e58d7e109d131c227b7bce84c8c756b9e57d972f5d9e92153"
---

# You cannot verify what you cannot reason

There is a new[slop](https://momentic.ai/blog/the-slop-is-out-of-the-bottle) on the block:[product slop](https://x.com/darrinhenein/status/2012227737998487679) . If you can ship a feature today, tomorrow, or even tonight, then why not slopship? Any feature that can come to mind can come to fruition. Move fast, break minds.


There are some good reasons to follow this strategy, but there are also many to pull back code generation and shipping to just warp .9. Chief among them, a simple concept:


**You cannot verify what you cannot reason about**


Before you can test whether a system behaves correctly, you must be able to say what "correctly" means. You must know what must be true. When you are generating and shipping at the speed of light, this clarity disappears. The codebase, edge cases, and interactions grow too quickly for anyone to develop the mental model required to say what should be true in the first place.


Testing is a tool for confirming that a system behaves as intended. But if no one can articulate what "intended" means at a given boundary, or if the system's behavior emerges from interactions no one designed, testing becomes an exercise in confusion.


The good news is that, though history doesn’t repeat, it does rhyme. Software engineering has seen a crisis like this before, where complexity outpaced systems. What can we learn from the past to better prepare us for the future?


## What the software crisis actually was


The “[software crisis](https://en.wikipedia.org/wiki/Software_crisis) ” was a period in the 60s and 70s when software projects failed at alarming rates. Budgets exploded. Timelines slipped. Systems that worked in testing failed in production.


This wasn’t a quality problem. The root cause was unmanaged complexity, not incompetence.


Because programming had suddenly become so much more powerful, systems behavior exceeded human comprehension. Codebases outpaced the ability to reason about them, and productivity collapsed despite more engineers, more tools, and more money.


No one could reason about what the system would do. Short (path) king, Edsger Dijkstra, put it best:


*As long as there were no machines, programming was no problem at all; when we had a few weak computers, programming became a mild problem, and now we have gigantic computers, programming has become an equally gigantic problem.*


The resolution came from architecture, not from testing:


- **Modularity** decomposed systems into parts that could be understood independently.
- **Explicit interfaces** defined what each component promised and required.
- **Layering** separated concerns so that changes in one area did not propagate unpredictably.
- **Abstractions** like type systems, operating systems, and databases encoded constraints that the compiler or runtime could enforce.


These innovations made systems comprehensible again. Once teams could reason about their systems, testing became effective. Tests could verify that modules honored their contracts. Integration tests could confirm that composed systems behaved as designed. Coverage metrics meant something because there was a "something" to cover.


Testing improved after architecture improved. Not before.


## AI is replaying the crisis, faster


Sudden all-powerful computer technology? Hmm, sounds familiar.


AI is recreating the conditions of the software crisis, but compressed into January instead of over decades. Three properties of modern AI accelerate the problem.


1. **Non-determinism as a baseline** . Traditional software is deterministic: the same input produces the same output. When it doesn't, that's a bug. An LLM can produce different outputs for identical inputs. Behavior shifts when the training data changes, prompts are modified, or the model is updated. The system changes without the code changing. Testing strategies built on determinism break down.
2. **Hidden state and implicit coupling** . In traditional software, you can trace behavior back to code. With AI, behavior emerges from weights, prompts, retrieved documents, and orchestration, all interacting in ways that are difficult to inspect. The "logic" is not written anywhere a human can read. Static analysis is nearly useless. Debugging requires inference about statistical patterns rather than step-through of execution paths.
3. **Rapid composition without shared abstractions** . Teams are assembling systems from LLMs, vector databases, tool-calling frameworks, agents, and external APIs. There are no stable primitives. No shared vocabulary for failure modes. No enforceable contracts between components. Each team invents its own glue, and that glue encodes assumptions that are invisible to everyone else.


The result is systems that work incredibly fast, until they don't, in ways no one can explain or reproduce.


## Why "test more" fails as a primary response


Momentic is a testing platform. You should expect our answer to this problem to be “more tests.”


Testing alone cannot solve architectural ambiguity. When you test a system you don't understand, you end up testing symptoms rather than invariants. Your tests encode whatever the system happens to do, rather than what it should do. When behavior shifts (as it will), tests break, but the breakage doesn't tell you whether something went wrong or whether the system simply changed in ways your tests didn't anticipate.


This produces a specific pathology: coverage increases while confidence decreases. Teams accumulate thousands of test cases. The test suite becomes a brittle mirror of the system's emergent behavior. Maintaining the tests becomes a significant engineering burden. But no one trusts the tests more, because the tests don't verify anything fundamental. They just document what happened last time.


Testing is most effective when:


- The system has clear contracts that specify expected behavior.
- Failure modes are enumerable and finite.
- Interfaces are stable enough that tests don't break constantly.
- Responsibility boundaries are explicit, so you know what each test is actually checking.


Without these conditions, testing doesn’t work. To get to “more tests,” we need “more structure.”


## Defining what must be true


Architectural constraints narrow the space of possible system behaviors. They make explicit what was implicit. They convert unknown failure modes into known, handleable cases.


Consider what architectural constraints do in traditional software. Type systems restrict the values that can flow through a program, catching errors at compile time rather than runtime. Memory safety guarantees eliminate entire classes of bugs by making them unrepresentable. Process isolation ensures that one component's failure cannot corrupt another. Transaction boundaries define where operations succeed or fail atomically.


They are structural decisions that make systems comprehensible and therefore verifiable.


How to achieve the same effect for AI?


Traditional testing verifies that code behaves as intended. But that framing assumes engineers write the code and therefore know what "intended" means. When AI generates code at a volume humans cannot review, that assumption breaks. The bottleneck shifts from writing code to verifying it, and verification requires a stable reference.


That stable thing is not the code. Code changes constantly. The stable thing is a definition of what must be true about the system's behavior.


A test that says "a logged-out user cannot access /billing" is not a verification step. It is a constraint definition. It does not check implementation details. It specifies an invariant that any implementation must satisfy. Written at this level, tests function as architecture. They narrow the state space. They define boundaries. They convert "anything could happen" into "these specific things must hold."


This reframes the problem. The question is not "how do we test this system?" The question is "what must be true about this system?"[The test is the truth](https://momentic.ai/blog/the-test-is-the-truth) .


This is harder than it sounds. It requires clear thinking about boundaries: what the system is allowed to do, what it must do, and what it must never do. It requires distinguishing between essential and incidental behaviors. It requires precision about failure modes, not just success cases.


When engineers can answer that question, they have defined the system's architecture in a form that is executable and enforceable. Tests become contracts. CI enforces them. Nothing ships unless the constraints are satisfied. Refactoring becomes safe because the definition of correctness is external to the implementation.


When engineers cannot answer that question, no amount of testing will help. Tests will encode whatever the system happens to do. They will break when the system changes, but the breakage will not tell anyone whether something went wrong. Coverage will increase while confidence decreases.


## An old answer to a new problem


The software crisis ended when the industry learned to design systems that humans could reason about. Modularity, interfaces, contracts, and abstractions were not concessions to limited human cognition. They were engineering responses to a fundamental constraint: verification requires comprehension.


AI is testing this lesson again. We are shipping systems whose behavior emerges from processes we cannot inspect, are built from components we cannot isolate, and change in ways we cannot predict. The instinct to test harder is understandable. It is also missing the point.


The fix is not more testing in isolation. It is architecture that makes systems comprehensible, and rigorous testing on that foundation.


This is an old lesson. Ignoring it would be the truly modern mistake.
