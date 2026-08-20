---
schema_version: "1.0.0"
document_id: "9b667e7d8a83a7eb678b385cc6c6188933c65ef656a12f64a7beb9cc0622289c"
company_key: "yc-dailybot"
company: "DailyBot"
source_id: "yc-dailybot-rss-59e03def7d75"
canonical_url: "https://www.dailybot.com/blog/agent-harnesses-are-the-new-ci/"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:17.151433+00:00"
fetched_at: "2026-07-28T22:15:23.630573+00:00"
content_hash: "sha256:7d7dc705305f48c3b91efb0c322cff2ea3bf3e5ba2e57729adeca8f5d0641cc9"
---

# Agent harnesses are the new CI: what Firefox, Google, and others are proving

In April 2026, Mozilla shipped Firefox 150 with fixes for 271 security bugs (180 of them rated sec-high), all found by a single agentic pipeline built on Claude Mythos Preview. That is not a typo. Two hundred and seventy-one bugs, in one release, from a system that did not exist six months prior. When we read[the technical breakdown](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/) , we stopped what we were doing and pulled the whole engineering team into a thread.


This post unpacks what an agent harness actually is, why the Firefox case matters beyond the headline number, and what other teams, including Google Project Zero, are doing in the same space. If you write software that ships to production, the patterns here are worth understanding now.


---


## The concept behind agent harnesses


An agent harness is the scaffolding around an LLM that turns it from a conversational tool into a structured, repeatable pipeline. The model is the core primitive; the harness is what makes it useful at scale.


In concrete terms, a harness defines:


- **What the agent can see.** Source files, diffs, commit history, test outputs, crash logs.
- **What the agent can do.** Compile code, run test cases, trigger sanitizers, write patches, file bugs.
- **How it gets feedback.** Runtime signals: did the test crash? Did AddressSanitizer fire? Did the proof-of-concept reproduce?
- **How failures are handled.** Retry strategies, deduplication against known issues, escalation to human review.


The key insight is that an agent with access to a build environment and a feedback loop is categorically different from an agent that only reads code and produces text. When a model can hypothesize that a bug exists, write a test case, compile it, run it, observe the result, and iterate — it stops being a suggestion engine and becomes a discovery system.


Static analysis


LLM reads code statically, generates a report. High false-positive rate. Requires human triage for every finding.


Agent harness


Agent harness hypothesizes, writes a proof-of-concept, runs it against the target, filters on reproducing evidence. Humans review confirmed bugs.


---


## How Mozilla built their bug-hunting pipeline


Mozilla’s[technical writeup](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/) provides an unusually detailed look at a production agent harness. Here is how the system works, broken down by layer.


### The inner loop


At the core, the harness prompts the model with a specific region of Firefox source code and a directive: *there is a bug in this part of the code, find it and build a testcase.* The model can:


1. Read the target code and surrounding context
2. Write a C++/JavaScript test case designed to trigger a suspected vulnerability
3. Compile and run the test inside Firefox’s test infrastructure
4. Observe whether AddressSanitizer, the crash reporter, or other signals confirm the bug
5. Iterate: refine the test case or move to a new hypothesis


This compile-run-observe cycle is what separates the harness from a static code review. The model does not just claim a bug exists; it proves it with a reproducible test case, or it moves on.


### The outer loop


Discovery alone is not enough. Mozilla built a full pipeline around the inner loop:


- **Target selection.** A mix of human judgment and automated signals determines which files and functions to scan. High-risk areas like the JIT compiler, IPC boundaries, and the XSLT engine get priority.
- **Parallelization.** Jobs are distributed across multiple ephemeral VMs, each assigned a specific target file. Results stream back to a central bucket.
- **Deduplication.** Incoming findings are checked against known issues to avoid duplicate reports.
- **Triage and tracking.** Bugs enter Mozilla’s standard security lifecycle: severity rating, patch assignment, code review, release management.
- **Model upgrades.** Because the pipeline is model-agnostic, swapping in a newer model (like moving from Claude Opus 4.6 to Claude Mythos Preview) improves every stage simultaneously: better hypotheses, better test cases, better explanations.


Mozilla’s key observation: model upgrades improve the entire pipeline. The system gets better at finding bugs, writing proof-of-concepts, and explaining their severity, all at once, with no harness changes.


### What the bugs looked like


The bugs Mozilla disclosed are not trivial. Consider a few examples from the[published sample](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/) :


- **Bug 2024918:** An incorrect equality check caused the JIT to skip initialization of a live WebAssembly GC struct, creating a fake-object primitive with potential arbitrary read/write. This was in code that had been extensively fuzzed.
- **Bug 2024437:** A 15-year-old bug in the` <legend>` element, triggered only by a precise orchestration of recursion stack depth limits, expando properties, and cycle collection.
- **Bug 2021894:** A race condition over IPC that let a compromised content process manipulate IndexedDB refcounts, triggering a use-after-free for a potential sandbox escape.
- **Bug 2025977:** A 20-year-old XSLT bug where reentrant` key()` calls caused a hash table rehash that freed its backing store while a raw entry pointer was still live.


Several of these are sandbox escapes, the hardest class of browser vulnerability to find. The model was allowed to patch Firefox’s sandboxed content process to simulate a compromised renderer, then search for ways to escalate into the privileged parent process. This type of reasoning about trust boundaries across process isolation is something fuzzers struggle with fundamentally.


---


## Google Big Sleep and the pattern across projects


Mozilla is not alone. Google’s Project Zero and DeepMind have been running a parallel effort called[Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html) , which evolved from the earlier Naptime project.


### The SQLite discovery


In late 2024, Big Sleep found a stack buffer underflow in SQLite, the database engine embedded in billions of devices. The vulnerability existed in a development branch and was caught before any official release. Existing fuzz testing infrastructure, including Google’s own OSS-Fuzz, had not detected it.


By mid-2025, the stakes escalated. Big Sleep discovered CVE-2025-6965, a critical memory corruption vulnerability in SQLite that was, according to Google, “only known to threat actors and was at risk of being exploited.” Google claims this was the first time an AI agent directly prevented a vulnerability from being exploited in the wild. The patch shipped in SQLite 3.50.2 before adversaries could scale their use of the flaw.


### AgentFlow and the Chrome zero-days


A separate line of research, documented in the paper[Synthesizing Multi-Agent Harnesses for Vulnerability Discovery](https://arxiv.org/html/2604.20801v1) , introduced AgentFlow, a system that automatically synthesizes multi-agent harnesses using a typed graph DSL. Instead of hand-crafting a single agent loop, AgentFlow defines agent roles, communication topology, tool access, and retry coordination as a graph that can be optimized.


The results: AgentFlow scored 84.3% on TerminalBench-2 (the highest public score at time of publication) and discovered ten zero-day vulnerabilities in Google Chrome, including two critical sandbox escapes (CVE-2026-5280 and CVE-2026-6297). It also found a 27-year-old denial-of-service vulnerability in OpenBSD’s TCP SACK implementation.


### The shared pattern


Across all these efforts, the engineering pattern is the same:


1. **A model as the reasoning core.** It reads code, forms hypotheses, generates test cases.
2. **A build/test environment the model can control.** Compilation, execution, sanitizers, crash analysis.
3. **A feedback loop that filters signal from noise.** Only reproducing bugs survive.
4. **An orchestration layer that handles scale.** Parallel VMs, target prioritization, deduplication, triage.
5. **A pipeline that plugs into existing workflows.** The output is a filed bug with a reproducer, not a PDF report.


---


## Why fuzzers missed what agents found


The natural question is: why did these bugs survive years of fuzzing? Mozilla, Google, and academic teams have been running state-of-the-art fuzzers against these codebases for over a decade.


The answer comes down to reasoning versus brute force. Fuzzers generate inputs at high speed and measure coverage, but they have no model of the program’s semantics. An agent, by contrast, can:


- **Read and reason about trust boundaries.** A sandbox escape requires understanding which process has which privileges, how IPC messages flow, and where a compromised process could inject malicious data. Fuzzers do not model these relationships.
- **Chain multi-step triggers.** Bug 2024437 in Firefox required a precise sequence: manipulate recursion depth limits, set expando properties, trigger cycle collection. A fuzzer would need to stumble into this exact sequence; an agent can reason about it.
- **Exploit domain knowledge.** The NaN-boxing bug (Bug 2022034) required understanding how Firefox serializes JavaScript values across IPC, and that a raw NaN can masquerade as a tagged pointer. That is architectural knowledge, not something random input generation discovers.


Mozilla noted something equally important: what the agents *could not* do. Attempts to exploit prototype pollution in the parent process failed because Mozilla had previously frozen prototypes by default. Watching agents fail against hardened defenses validated years of prior security work.


That said, agents and fuzzers are complementary. Fuzzers excel at covering massive input spaces cheaply. Agents excel at deep, targeted analysis of complex code paths. The strongest security posture uses both.


---


## What makes a good harness


Based on the patterns across these projects, a few engineering principles stand out for anyone building an agent harness, whether for security, testing, or other production workflows.


**Start simple, then iterate.** Mozilla’s initial prompts were not dramatically different from what any engineer might try on a first attempt. The sophistication came from observing the agent’s behavior, tuning prompts, and building orchestration around the core loop. The harness grew from terminal sessions to parallelized VM fleets through gradual iteration.


**The feedback loop is the product.** An agent that can only read and write text will always have a high false-positive rate. The moment you give it the ability to compile, run, and observe, the signal-to-noise ratio changes fundamentally. Invest in the environment before the prompt.


**Make model upgrades a one-line change.** Mozilla designed their pipeline to be model-agnostic. When Claude Mythos Preview became available, they swapped it in and immediately got better results. If your harness is tightly coupled to a specific model’s quirks, you lose the ability to benefit from the fastest-moving part of the stack.


**Plug into existing workflows.** The output of an agent harness should be a bug ticket with a reproducer, a failing test, or a PR. Not a standalone report that requires separate triage. Mozilla’s pipeline feeds directly into their security bug lifecycle. Google’s findings go through standard CVE processes. The harness is a producer; existing systems are the consumer.


**Plan for volume.** Mozilla fixed 423 security bugs across April 2026 releases. That is an order of magnitude beyond their historical baseline of 20-30 per month. The bottleneck shifted from discovery to patch review and release management. If you deploy an effective harness, make sure the downstream pipeline can handle the throughput.


---


## What this means for the rest of us


The Mozilla and Google examples are browser-scale projects with dedicated security teams. But the underlying patterns are accessible to any team that maintains a codebase with a test suite.


The minimum viable agent harness is simpler than it sounds: a prompt, access to` git` , a compiler or test runner, and a loop that checks whether the output reproduces. You do not need a fleet of VMs to start. You need a feedback loop.


Mozilla’s own advice: *“Anyone building software can start using a harness with a modern model to find bugs and harden their code today. We recommend getting started now.”*


The teams that build this infrastructure now, even at small scale, will be positioned to take advantage of every model upgrade that follows. The harness is the durable investment; the model is the part that gets better on its own.


For teams coordinating AI agents alongside human developers, the broader challenge is not just running agents but tracking what they find, routing their output into existing processes, and keeping humans in the loop on triage decisions. That coordination layer is where the operational complexity lives.


The moment to start building is now. Not because the current models are perfect, but because the infrastructure you build today compounds with every model that comes next.
