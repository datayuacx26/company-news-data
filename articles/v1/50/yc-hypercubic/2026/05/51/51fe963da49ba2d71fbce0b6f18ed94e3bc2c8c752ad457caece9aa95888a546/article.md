---
schema_version: "1.0.0"
document_id: "51fe963da49ba2d71fbce0b6f18ed94e3bc2c8c752ad457caece9aa95888a546"
company_key: "yc-hypercubic"
company: "Hypercubic"
source_id: "yc-hypercubic-news-import-1849a5c8a5f4"
canonical_url: "https://www.hypercubic.ai/insights/hyperloop-a-verification-driven-system-for-mainframe-modernization"
published_at: "2026-05-20T07:00:00+00:00"
first_seen_at: "2026-07-21T23:26:19.477967+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:551449e86f5eee8d819b39153441029d1c10a5524c3bf28fd81a0329a61968a3"
---

# HyperLoop: A Verification-Driven System for Mainframe Modernization

Mainframes still run the business logic behind the world’s largest institutions.


Decades of COBOL, JCL, BMS, and CICS encode how banks process transactions, how insurers underwrite policies, how airlines confirm reservations, and how governments deliver services. These systems are reliable, deeply optimized, and operationally critical, but maintaining them is expensive, staffing them is getting harder, and evolving them is painfully slow.


For years, large enterprises have wanted to move these systems onto modern, cloud-native architectures. The opportunity is obvious. So is the danger. Every modernization effort carries the risk that the new system behaves differently from the legacy one. In financial workloads, even a single divergent calculation can be catastrophic.


Translating COBOL into a modern language is the easier problem. Proving that the modern system preserves legacy behavior is the hard one.


HyperLoop is an end-to-end system for mainframe modernization built around continuous, layered verification.


[Get in touch →](https://www.hypercubic.ai/contact)


## Why mainframe modernization stalls


Across large enterprises, the pattern is consistent: testing and verification are the hardest parts of modernization.


AI-powered code translation has become a popular starting point. Modern LLMs can read a COBOL program and produce plausible Java in seconds.


But plausible code is not production-ready code.


A translated program may compile. It may pass a few hand-written tests. It may even appear correct in a demo. Then it can fail under a specific data condition, an unusual error path, a downstream dependency, or a pattern of production traffic no one remembered to test.


Before a single modernized program can safely reach production, enterprises have to answer three questions:


- Does the modern code preserve the behavior of the COBOL?
- How do we know it preserves that behavior under inputs we did not think to test?
- How do we cut over without putting the business at risk?


Existing approaches each solve part of the problem:


- Rehosting preserves the COBOL, but carries forward the skills gap.
- Automated translation produces output, but leaves correctness unresolved.
- AI-assisted coding accelerates implementation, but offers no behavioral guarantees.
- Traffic replay validates after the fact, but does not drive the modernization loop.
- Formal verification provides strong guarantees, but is difficult to scale across millions of lines of legacy code.


Each approach has value. None of them closes the loop between transforming a system and proving the transformation correct.


That is the gap HyperLoop is built to close.


## Introducing HyperLoop


HyperLoop is a mainframe modernization system that combines human judgment, AI-powered development, and rigorous verification loops, so teams can transform critical COBOL applications without the testing and cutover risk that has stalled this work for decades.


We modernize one package at a time, typically 10,000 to 100,000 lines of COBOL. Within each package, HyperLoop runs three verification loops at progressively coarser resolution: an inner loop that produces a first-draft modern app, a middle loop that proves behavioral equivalence against a verified simulation twin, and an outer loop that validates against the real mainframe. We unpack each below.


HyperLoop is not a translation tool with a chatbot bolted on. It is a verification system that uses AI inside layered correctness loops.


The design principle is simple: Deterministic where correctness matters, AI where creativity matters.


## Inside the three loops


### Loop 1: From source code to a first-draft modern app


HyperLoop begins by reading the legacy COBOL and generating *conjectures* : testable claims about how the program behaves under specific inputs.


A conjecture is not accepted because an AI model thinks it sounds right. It has to survive mechanical validation. HyperLoop runs each claim against the legacy program under counterfactual inputs, then uses static and dynamic analysis to check whether the claim holds across every reachable branch.


Only after that validation does a second agent audit the result and decide whether to promote the conjecture into a platform-independent business rule.


Every promoted rule preserves a machine-readable chain back to the original COBOL branch it came from. That traceability matters. It means the modern system is not built from vibes, summaries, or lossy documentation. It is built from claims that were tested against the legacy system itself.


The audited business rules become unit and integration tests. Those tests define the first modern implementation: typically a Java Spring Boot backend with a TypeScript React frontend.


The result is a first-draft modern application that is approximately correct by construction. It still has to prove itself against the legacy system in the next loop.


### Loop 2: Simulation-driven equivalence


The second loop checks the first-draft modern application against a verified simulation twin.


Our compiler suite, Isomorphic, transpiles COBOL into Dafny, a verification-aware language created by Microsoft Research. Dafny then compiles into executable Go. We also compile BMS, copybooks, and JCL, and provide simulated runtimes for CICS and batch jobs on commodity hardware.


The result is a full simulation twin that runs without IBM middleware.


The Dafny step creates a verification fence. The verifier proves that the compiled twin cannot crash, corrupt memory, or enter undefined behavior at any reachable program state. That verified twin becomes the reference oracle for everything downstream.


HyperLoop then drives the first-draft modern application and the simulation twin through the same scenarios, side by side. A custom comparator flags every divergence between them.


The diff is structural at the JSON and HTTP level, with declarative tolerances for fields that are intentionally nondeterministic. HyperLoop also generates adversarial scenarios designed to probe the edge cases that audits often miss: coordinated cross-field validation, ordered numeric relationships, role gates, and stateful sequencing.


Each divergence becomes a repair target. HyperLoop updates the modern application, reruns the scenario set, and repeats the loop until the modern system is behaviorally identical to the twin across the relevant cases, except where teams have explicitly chosen to improve legacy behavior.


### Loop 3: Validation against the live mainframe


The third loop validates the modernized application against the real mainframe.


Loop 2 proves equivalence against the verified simulation twin. Loop 3 checks that equivalence against live mainframe behavior using production scenarios, captured traffic, and additional adversarial test cases.


The same comparator infrastructure drives both systems side by side and flags any divergence. Captures from the mainframe feed back into the regression suite, so observed production behavior becomes part of the ongoing verification harness for the modern stack.


HyperLoop also introduces a third comparator status: *finding* .


A finding captures a documented divergence whose bounds are explicitly understood and accepted. For example, a modern system may intentionally reject malformed inputs that the legacy system tolerated, normalize a field more strictly, or improve behavior around a known defect.


Findings are first-class regression objects. They are tracked, reviewed, and enforced like any other part of the verification system.


### What we ship at the end of a run


Once a package has passed all three loops, traffic for that slice can be migrated to the modern system.


The legacy system remains available as a fallback throughout the migration. The outer loop continues to enforce equivalence after cutover, so regressions in the modern stack are caught before they reach customers.


At the end of each run, teams receive:


- A modernized application for the selected package.
- A verified simulation twin of the legacy behavior.
- A regression suite built from business rules, adversarial tests, production captures, and accepted findings.
- A comparator harness that continues to check the modern system against the legacy baseline.
- A migration path that remains reversible until the legacy system is fully decommissioned.


Modernization becomes incremental, testable, and reversible at the granularity of a single package.


## Why this is hard to build


Building a verification-driven modernization system is fundamentally different from building an AI coding agent.


Modern code agents operate over files, repositories, tests, package managers, and cloud APIs. Modernization agents have to operate across COBOL, Dafny, Go, Java, TypeScript, BMS, JCL, VSAM, IMS, CICS regions, EBCDIC encoding, packed decimal arithmetic, mainframe job scheduling, fault injection, instrumented traces, decision-point coverage, scenario diffing, capture replay, and strangler-fig routing.


They also operate in a domain where one bad transaction is unacceptable.


HyperLoop gives AI agents a layered correctness substrate to work inside. That substrate is built from deterministic components: compilers, runtimes, comparators, replay engines, and code generation templates.


Inside that substrate, AI agents do the exploratory work deterministic systems cannot do well: conjecture generation, disposition audit, adversarial scenario synthesis, and implementation against derived tests.


The deterministic layer constrains the agents’ hypotheses. The agents generate candidates the deterministic layer can falsify.


That separation is what lets AI accelerate modernization without turning correctness into a matter of trust.


## What changes when verification scales


Mainframe modernization has been bottlenecked on verification for thirty years.


With layered correctness loops driven by AI inside a deterministic substrate, the bottleneck moves.


Critical systems can be modernized faster, package by package, with measurable coverage at every step. Traffic cuts over only after equivalence has been proven.


Migrations become safer. The legacy system stays available throughout the transition. Cutover remains reversible at the granularity of a package. The outer loop continues to catch regressions after production traffic begins moving to the modern stack.


The modern system also becomes easier to sustain. The conjecture corpus, audited business rules, accepted findings, and replay captures form a living regression suite that compounds with every workflow exercised.


At Hypercubic, we are building AI-native infrastructure for the full lifecycle of legacy modernization.


Each product handles a different part of that lifecycle:


- HyperDocs explains the system.
- HyperTwin captures expert knowledge.
- Hopper lets teams operate inside the mainframe environment.
- HyperLoop transforms and verifies change.


HyperLoop is the piece that turns modernization from a translation exercise into an engineering process with a measurable path to production.


## Work with us


HyperLoop is available today for enterprise modernization engagements.


If your organization is planning a mainframe modernization, or already has one underway and is running into the verification problem, we would like to hear from you.


[Schedule a demo →](https://www.hypercubic.ai/contact)
