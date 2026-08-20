---
schema_version: "1.0.0"
document_id: "2b8d09b013ad147b869c5bf6159526ab00d19c7ef860384d629d4ddf9dfceccc"
company_key: "yc-hypercubic"
company: "Hypercubic"
source_id: "yc-hypercubic-news-import-1849a5c8a5f4"
canonical_url: "https://www.hypercubic.ai/insights/mainframe-modernization-is-a-hard-and-fun-problem-to-solve"
published_at: "2025-12-28T00:00:00+00:00"
first_seen_at: "2026-07-21T23:26:19.477967+00:00"
fetched_at: "2026-07-28T22:24:49.305726+00:00"
content_hash: "sha256:dc2ea58c106a43bd61e873c4692a8f173d5e3d6d6e39735ab1f300b3ac3267cb"
---

# Mainframe Modernization is a Hard and Fun Problem to Solve

The title makes two claims:


1. Mainframe modernization is hard.
2. Mainframe modernization is fun.


My aim in this essay is to justify these claims. If I succeed, I hope to persuade a small number of technical folks who are encouraged—rather than deterred—by enormous difficulty to join us on our quest.


To start, I should explain what I’m talking about.


When you think of a mainframe, you likely imagine a monochrome green-screen terminal. For over forty years, that glowing phosphorus text has been the steady heartbeat of global finance.


Essentially, mainframe modernization is converting a software application or suite of applications running on a mainframe into a modern distributed system, while preserving behavior, correctness, and operational guarantees.


What is a mainframe computer? Per IBM, the largest provider of mainframes, “\[they\] are data servers that are designed to process up to 1 trillion web transactions daily with the highest levels of security and reliability.” Despite the rise of cloud computing, mainframes continue to play an essential role in IT infrastructure, especially in more traditional industries that have a large volume of critical transactions.


Based on a recent IBM report, “45 of the top 50 banks, 4 of the top 5 airlines, 7 of the top 10 global retailers and 67 of the Fortune 100 companies leverage the mainframe as their core platform.” When you swipe a credit card in London or withdraw pesos in Bogotá, you are interacting with these machines.


## The Why


So, why modernize? If mainframes are so good, why move away from them? Organizations purport to have a variety of reasons. But if you stand in an operations room of a major bank today, you’ll see that it boils down to two fundamental points:


1. Agility. The number of people who still know COBOL and the internals of 40-year old banking infrastructure is rapidly dwindling. This makes it dangerous to change these programs, causing parts of a business to freeze in time. A business that stagnates eventually dies, and thus agility is the antecedent to survival.
2. Cost. IBM holds a near monopoly in the mainframe market. Operating a single mainframe can cost millions per year. While not a dealbreaker for everyone, many enterprises would switch away from expensive IBM contracts if there were a clear, low-risk path to do so.


Thus, even though there is nothing wrong with mainframes *per se* , the market conditions and state of the world create strong tailwinds for modernizing away from them, with the mainframe modernization market estimated to be in the hundreds of billions of dollars.


## Mainframe modernization is hard


Mainframe modernization is a wickedly hard problem because it is an amalgamation of some of the hardest problems in all of computer science and software engineering, all at once.


We must simultaneously deal with issues like implicit shared state, undocumented control flow, non-obvious transactional boundaries, numerical edge cases that have accumulated over decades, and distributed consistency guarantees that the mainframe provides implicitly. Distributed systems do not.


This is the reality of the “green screen” legacy: Mainframe modernization tackles decades of tech debt in a language almost no one knows (COBOL), written before the advent of modern software engineering and object-oriented programming. Design patterns are absent, documentation painfully incomplete.


Consider something as simple as numbers.


COBOL commonly uses packed decimal formats like COMP-3, which encode base-10 arithmetic directly in memory to guarantee exact precision. If you migrate that logic to a cloud-native floating-point type, you have subtly changed the system’s mathematical behavior. IEEE 754 floats are base-2 approximations, and values like 0.1 cannot be represented exactly. Although each individual discrepancy is tiny, in a banking system over millions of daily computations, the error compounds.


The system begins to unintentionally create or destroy money.


In theory, and in practice at this scale, no static analysis can fully characterize the behavior of a system this large and this old. There will always be behaviors that only reveal themselves under real inputs, real data, and real timing.


The target application must accurately capture the “Ghost in the Shell” of the mainframe, often including its flaws and bugs. Downstream applications might be handling known bugs in idiosyncratic ways, and fixing these known bugs can lead to unforeseen second and higher order catastrophes.


As we mentioned in our post on the Lindy Effect, history is littered with large scale failures of traditional human-focused modernization. We must strive to do better.


## Solving the hard problem (is fun)


To modernize safely, we must demonstrate—with mathematical precision—that our new system behaves exactly like the old one. This requires creating a sophisticated functional test suite that acts as a perfect reflection of reality.


To create this, we need to consider two objectives:


1. The Customer View: For all production inputs, does the target system generate the exact same output as the source?
2. The Engineering View: Can we write the test suite in a way that creates an unambiguous, understandable spec for AI agents from which to create the target application?


A human can reason about one execution path at a time. This problem requires reasoning about millions. The state space is too large, the interactions too subtle, and the feedback loop too slow for human intuition alone. What we need is not more hands writing code, but a system that can explore possibilities, absorb constraints, and iterate at a speed no team of humans can match. But that system is only as good as the reality it is constrained to obey.


## The Frontier


This is the frontier where the engineer’s role shifts from writing code to designing the constraints that define reality.


At Hypercubic, we believe this—the ability to constrain reality for agentic AI—is the defining skill of the next decade of software engineering.


We are still discovering the right way to build these constraints. It requires reasoning about high-level invariants like balance preservation, while also tracing low-level behaviors like file I/O ordering or transaction retries. It spans static code analysis, dynamic data flow graphs, and ordering transactions across massively distributed systems.


Most software engineering today involves gluing together well-documented APIs or moving buttons on a screen. It is predictable, safe work.


Mainframe modernization is the opposite. It requires engaging directly with the chaotic and undocumented foundations of the global economy.


We are finally reaching behind that glowing green screen to pull the logic into the light of the 21st century.


If the idea of solving problems that are theoretically impossible but practically necessary appeals to you, we want to meet you.


### References


1. [https://www.ibm.com/think/topics/mainframe](https://www.ibm.com/think/topics/mainframe)
