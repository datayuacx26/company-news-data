---
schema_version: "1.0.0"
document_id: "8a7d4c6dd610c6b94e5a5763a521bf1504701401aa1315ee916b229f43413f3d"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/the-exploit-race"
published_at: "2026-01-16T21:59:22+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:23:30.670356+00:00"
content_hash: "sha256:2a4c1d79b52a97f702217a442e4c0b54908ca40e58afe584b151ac3ce797cf5b"
---

# The Exploit Race

### The Exploit Race


Web3 is different from “normal software” for one brutal reason: bugs turn directly into money. In 2025 alone, an estimated $3.4B was stolen through crypto exploits \[1\]. That incentive creates a uniquely hostile environment where attackers systematize vulnerability search \[2\].


‍


Recent research by Anthropic shows what happens when exploitation becomes automated: agents can repeatedly test at scale with rapidly falling cost, and they surfaced previously unknown issues during large-scale evaluation \[3\].


‍


When attacks scale this cheaply, obvious bugs are exploited immediately. What remains is a race: how quickly defenders adapt, and how effectively agents find what humans missed.


‍


### Scaling Defense


If attackers scale, defenders must scale too. That’s why AI security agents are quickly becoming part of the baseline. But the current landscape is noisy: benchmarks disagree, performance claims are difficult to compare, and teams struggle to evaluate which agents to trust and how to fit them into the dev cycle.


‍


*Our view is simple: if agents are going to become part of the security baseline, they must be evaluated like infrastructure.*


‍


At Quantstamp, we’ve built a dedicated team of AI security engineers focused on focused on benchmarking agents in realistic environments and auditing agentic systems end-to-end, from model behavior to deployment. This work is supported by research grants from OpenAI and Anthropic.


This post (and evolving blog series) distills what we’ve learned so far: why agents are necessary, what they actually do, where they work well, and where they still fail.


‍


### Prompting LLMs is Unreliable


Most web3 developers have tried asking an LLM to “find vulnerabilities”. And it does help: models are often good at catching textbook issues, like missing access control checks or suspicious external calls. As a fast hygiene check, this is genuinely useful.


‍


But smart contracts are a harsher environment than most software. Smart contract bugs are often easy to verify once suspected, but hard to discover in the first place. Attackers don’t need mainnet to do the discovery; they can simulate contracts and chain state locally, iterate across state variations, and then only execute a proven path on-chain.


‍


In that world, it’s not enough for an LLM to be occasionally insightful; you need outputs that are consistent and repeatable.


‍


“Raw prompting” usually fails in two predictable ways:


- **Prompt strategy + context sensitivity.** Small changes in phrasing, context, or stated assumptions can swing the result from “no issues” to “critical vulnerability.” In controlled evaluations, prompt design alone can reduce false positives by over **60%** – a strong result, and also a warning: output quality is highly dependent on prompt discipline and standardization \[4\].
More importantly, *what you ask for* matters as much as how you phrase it. Targeted prompts (e.g., “Does reentrancy exist here?”) tend to be more reliable than broad prompts (“What vulnerabilities exist?”), which is why one-shot prompting is inherently unstable \[4\]. This also hints at why multi-pass approaches work better: different checks benefit from different framing and context.


- **False positives pile up; false negatives stay invisible.** LLMs can produce findings that sound right but aren’t exploitable (or are mitigated elsewhere), and too much of that quickly becomes un-actionable noise. At the same time, misses don’t announce themselves… Until someone extracts value. Even agentic audit frameworks highlight this exact tradeoff: structure improves results, but meaningful assurance still requires verification and human validation \[5\].


This is where “LLM-as-a-prompt” hits a ceiling: it’s a strong assistant, but not a security workflow. Agents exist to make this operational - standardized prompts, scoped context, multi-pass checks, and verification loops - so results are reproducible, comparable over time, and steadily improvable.


‍


### Turning AI into a Security Workflow


A security agent is basically an LLM wrapped in a repeatable process: it decides what to check, runs multiple targeted passes, pulls only the context it needs, uses tools when needed, and tries to validate findings before reporting them.


‍


A single prompt - no matter how well written - forces the model to choose everything at once: what matters, what doesn’t, which hypotheses to pursue, and when to stop. Agents shift that balance. You define the workflow (checks, sequencing, verification, reporting), and the model keeps flexibility where it’s actually valuable: interpreting code, generating hypotheses, and adapting based on evidence.


‍


### **Optimizing an agent**


A single prompt can already be structured: you can chain prompts, add checklists, and iterate manually. The point of an **agent** is that this structure becomes **systematic** : a workflow you can run consistently, benchmark, and evolve. This way, results depend on a process you control.


‍


That’s why there’s no single “right” way to build an agent. Most teams in this space are experimenting with a big design space: **which strategies to use, in what order, with what tools, and with what stopping criteria** . The same base model can behave very differently depending on these choices. \[4\]\[6\]


‍


Here are the main levers agents are built from:


- **Prompt discipline + targeted checks (control the question)** Instead of asking broadly “what vulnerabilities exist?”, agents run a **battery of targeted questions** (e.g., “is reentrancy possible along this value-flow path?”). Controlled evaluations show outputs shift materially with prompt and context choices, and that targeted framing can be more reliable than open-ended prompting - one reason agents bake prompts into standardized routines. \[4\]\[6\]


- **Context + memory management (the context of each question)** Agent performance often hinges on **how context is managed across passes** , not just what fits in a single prompt. Strong agents treat analysis as stateful: they extract intermediate observations (threat-model notes, call-path summaries, assumptions, candidate invariants), compress them into reusable artifacts, and then **feed those artifacts into later phases** (validation, exploitability testing, and write-up). This reduces “re-derivation” and makes results more consistent across runs. \[7\]\[10\]
Two common retrieval/structuring approaches inside this layer are:


- **Retrieval (RAG)** Used to pull the *right* snippets - code slices, protocol docs, standards, known patterns. Smart-contract-focused systems explicitly integrate RAG to inject domain knowledge at the right step. \[7\]\[10\] More generally, knowledge-level RAG has shown meaningful gains for vulnerability detection by retrieving structured “vulnerability knowledge” instead of relying on the base model’s memory. \[11\]
- **Knowledge graphs (structured relationships you can query)** Used when relationships matter more than raw text - e.g., access-control roles, privilege edges, call relationships, and state dependencies. Graph representations enable more deterministic checks (queries/rules) and can also serve as a **structured scratchpad** : the agent can store inferred relationships/claims in the graph and retrieve/query them later instead of repeatedly re-deriving them from the raw code. \[8\]


- **Multi-pass orchestration (separate “spot” from “prove”)** Many useful workflows deliberately split phases: for example, one pass flags suspicious patterns, another pass tests exploitability and preconditions, and a final pass produces a structured write-up. This tends to reduce confident but ungrounded findings, because the agent is forced to justify claims rather than stopping at pattern recognition. \[4\]\[5\]\[9\]


- **Tools + verification loops (convert hypotheses into evidence)** Agents become meaningfully more useful when they can **verify** using executable checks. That might mean compilation/tests, static analyzers, symbolic execution, traces, sandboxed runs, or exploit-attempt harnesses. Tools help reduce hallucinations and filter false positives. \[12\]\[13\]


- **Specialization / multi-agent collaboration (coverage + second opinions)** Some systems split roles (e.g., analyser, verifier) or run independent passes and reconcile disagreements. The goal is to reduce single-thread failure modes and improve coverage through deliberate redundancy and challenge. \[5\]\[9\]


- **Fine-tuning (works best when it supports the workflow)** Fine-tuning can improve detection on certain setups, but it tends to be most valuable when paired with the workflow layers above: targeted checks, scoped context, and verification loops. \[6\]\[10\]


However, even the best agents still have blind spots, and they’re the ones that tend to matter most in high-value protocols.


‍


### Current Landscape & Benchmarks


Now that we’ve established why agents are the right direction for web3 security, the obvious question is: **which agents can you actually trust?** That’s where the landscape gets messy, because **it’s surprisingly hard to measure performance in a way that’s fair and comparable.**


‍


One unexpected divergence is the benchmark evaluator. Some public evaluators score a finding as a **true positive** because it looks “close enough” to something in the ground truth - even if the **location** is different or the **attack scenario** doesn’t match. That sounds reasonable until you look closer: a finding isn’t just a label, it’s a specific claim: *this function* , *this path* , *these preconditions* , *this is how value moves* . If an agent flags reentrancy in the wrong place, it’s still a false alarm for the developer… even if the contract happens to have a real reentrancy bug somewhere else. Such evaluators quietly inflate scores and make tools look more interchangeable than they actually are.


‍


That’s why we built our own evaluator: to get the right metrics for comparing agents and comprehend publicly claimed benchmarks.


‍


With cleaner evaluation, the first observation is obvious: agents are optimized for different users. Some aim for **precision** (fewer false positives), which is great for developers who need a signal they can act on quickly. Others aim for **recall** (catch more real issues) and tolerate more noise. This is often better for security teams who are willing to validate and dig deeper. Over time, we expect the best agents to push both higher, but right now it’s still a real tradeoff.


‍


It’s also not “one agent fits all” (yet). Performance shifts with **ecosystem, contract style, and protocol complexity** . An agent that looks strong on common, well-known patterns (think standard token/NFT implementations) can degrade fast when you move to complex systems like lending protocols. We have even observed them to fall apart entirely on novel designs where there isn’t much prior data, and the attack surface depends on deeper reasoning (e.g., unusual account abstraction setups). In other words, today’s agents are good at **pattern spotting and sanity checks** , and much less reliable when the job is to find creative attack vectors or perform an exhaustive audit.


‍


To get a clearer signal, we benchmarked multiple accessible agents using our own evaluation system on a contract set we believe is out-of-distribution for current models, bucketed into Easy / Medium / Hard. We’ll likely publish deeper numbers later, but the qualitative takeaway is already useful: **tools are not converging to one “best agent.”** They’re differentiating by noise tolerance, by workflow quality, and by which protocol families they handle well.


‍


So the path forward isn’t more hype or more one-off leaderboards. It’s **independent, verifiable benchmarking** that makes the tradeoffs explicit: precision vs recall, evidence-backed vs pattern-only findings, and strengths by protocol type and ecosystem. Ideally, developers shouldn’t need to become benchmark experts just to pick a tool. They should have clear, comparable, reproducible statistics that make AI security feel like dependable infrastructure.


‍


### Advice for Devs


At the current stage of AI security agents, developers can get real value **if they treat them like a CI check** : they surface leads continuously, but they don’t certify safety. Agents help you keep up by making vulnerability discovery cheap and repeatable on the defender side too. Our best practice advice: turn them into a routine of consistent runs, triage, and validation.


‍


Here’s a workflow that we’ve found useful:


- **Use agents early (pre-audit), not late.** Run them during development at every release candidate. Early runs are cheap to fix. Late runs get ignored because you’re already in ship mode.


- **Treat outputs as hypotheses to verify.** Handle findings like a failing test: actionable, but not automatically true. For anything non-trivial, force “prove mode”: *what’s the exact call path, what are the preconditions, what state assumptions are required, and can we reproduce it* (test/invariant break/trace/minimal PoC)?
Some agents produce PoCs for you, but they might not be automatically valid.


- **Run a *small* handful of agents.** Two or three tools with different strengths provide coverage without drowning you in noise.


- **Triage systematically.** Use a simple loop with a clear definition of done:


- **Confirmed** → reproducible PoC, failing test, invariant break, or clear trace → fix immediately.


- **Plausible** → credible hypothesis but unclear preconditions → assign a short validation task (targeted test, instrumentation, tool run).


- **Likely false positive / intended behavior** → document why, and add a guardrail (test/invariant/comment) so it doesn’t regress into a real issue later.
This prevents the two bad equilibria: “trust everything” or “ignore everything.”


- **Iterate until you reach a stable baseline.** Re-run agents after each fix. The goal isn’t “zero findings”. The goal is a **predictable noise floor** , where remaining flags are either known false positives or explicitly accepted design choices. That’s what makes the tool usable over time.


- **Handle privacy like a vendor-risk decision.** If code is sensitive, don’t default to pasting it into a black box. Prefer self-hosted options or vendors with clear retention and training-on-input policies, and never include secrets (keys, privileged endpoints, unreleased parameters). Agents will always ask for “more context”; your workflow should default to the **minimum necessary context** .


Done right, this workflow removes low-hanging bugs quickly and saves human time for what agents still miss: economic edge cases, design flaws, and spec–implementation mismatches. Use agents to raise your baseline so auditors can focus on non-textbook risk.


‍


### We’re here to help


AI agents are becoming the new baseline for smart contract development. Used well, they reduce attack surface before audits even begin. But as the exploit race accelerates, the uncomfortable truth remains: **the failures that cost the most are still the ones agents miss** - design flaws, economic edge cases, and spec ↔ implementation mismatches that only show up when you threat-model the full system.


‍


That’s where we focus:


- **End-to-end smart contract audits,** beyond pattern-level issues.


- **Helping teams integrate agents responsibly,** without over-trusting them.


- **Auditing agentic systems themselves:** reviewing the full stack: model behavior, prompt-injection resistance, tool-use safety, data handling, and deployment risks – so your “security tooling” doesn’t become a new attack surface.


The strongest security posture today is layered: agents for speed and coverage, and experienced auditors for adversarial reasoning and assurance where it matters most.


‍


[Request an audit,](https://audit.quantstamp.com/new?from=9c957f52f76a11b) and we’ll help you design that workflow – and ship with confidence.


‍


### Sources


\[1\][https://www.chainalysis.com/blog/crypto-hacking-stolen-funds-2026](https://www.chainalysis.com/blog/crypto-hacking-stolen-funds-2026?utm_source=chatgpt.com) \[2\][https://www.chainalysis.com/blog/organized-crime-crypto/](https://www.chainalysis.com/blog/organized-crime-crypto/?utm_source=chatgpt.com) \[3\][https://red.anthropic.com/2025/smart-contracts/](https://red.anthropic.com/2025/smart-contracts/?utm_source=chatgpt.com) \[4\] Logic Meets Magic: LLMs Cracking Smart Contract Vulnerabilities —[https://arxiv.org/html/2501.07058v1](https://arxiv.org/html/2501.07058v1) \[5\] LLM-SmartAudit: Advanced Smart Contract Vulnerability Detection —[https://arxiv.org/abs/2410.09381](https://arxiv.org/abs/2410.09381?utm_source=chatgpt.com)


\[6\] Detection Made Easy: Potentials of Large Language Models for Solidity Vulnerabilities —[https://arxiv.org/html/2409.10574v2](https://arxiv.org/html/2409.10574v2) \[7\] SCALM: Detecting Bad Practices in Smart Contracts Through LLMs —[https://arxiv.org/abs/2502.04347](https://arxiv.org/abs/2502.04347?utm_source=chatgpt.com) \[8\] CKG-LLM: LLM-Assisted Detection of Smart Contract Access Control Vulnerabilities Based on Knowledge Graphs —[https://arxiv.org/html/2512.06846v1](https://arxiv.org/html/2512.06846v1?utm_source=chatgpt.com) \[9\] LLM-BSCVM: Large Language Model-Based Smart Contract Vulnerability Management Framework — *(use the URL you already cite in your draft, or send it and I’ll format it consistently)* \[10\] SmartLLM: Smart Contract Auditing using Custom Generative AI —[https://arxiv.org/abs/2406.09677](https://arxiv.org/abs/2406.09677) \[11\] Vul-RAG: Leveraging Knowledge-Level RAG for Vulnerability Detection — *(use the URL you already cite in your draft, or send it and I’ll format it consistently)* \[12\] Prompt to Pwn (ReX): Automated Exploit Generation for Smart Contracts —[https://arxiv.org/abs/2408.04556](https://arxiv.org/abs/2408.04556) \[13\] “Identifying Smart Contract Security Issues in Code Snippets from Stack Overflow” (introduces SOChecker) — arXiv:2407.13271


‍
