---
schema_version: "1.0.0"
document_id: "644d1df76403b9738136a500ab9bbbc31edf4a22a29085c87898fa66d625f3dd"
company_key: "yc-orange-slice"
company: "Orange Slice"
source_id: "yc-orange-slice-news-import-fc722c545c85"
canonical_url: "https://orangeslice.ai/blog/future-of-automation"
published_at: null
first_seen_at: "2026-07-22T07:43:56.547862+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:6764ace12fe2e393296a7fe80359e892e25270cddb201c829af61d22963ef26d"
---

# The future of automation isn't no-code. And it isn't agents either.

If we take a look at the processes most businesses run (whether in sales, finance, or engineering) almost all of them can be broken down into workflows.


By my definition, a workflow is a series of actions based on a specific input. The path forward branches based on decision points that are handled deterministically (if/else logic), by AI, or by a human (human-in-the-loop).


### Agent vs. Workflow


Much of knowledge work can be represented by either workflows or agents, and the difference between the two is subtle—and may even become irrelevant with time.


Right now, many think of an agent as a recursive decision tree of unknown length, with the possibility of an action at each node, all in hopes of completing a specific task. A workflow, on the other hand, is a decision tree that is at least partially deterministic (meaning certain steps are hardcoded and must be taken).


You can see where this gets fuzzy fast. A workflow can easily have an “agent” embedded inside of it to take over a more complex, ambiguous part of the task.


You can imagine that as agents get smarter and cheaper, there may be less need for rigid workflows and determinism. Why map out every step when you can just have an intelligent agent orchestrate the whole thing? However, there is a strong counter-argument: for the sake of security, speed, and safety, workflows are here to stay.


### Agentic Code Execution & Subagents


It’s no coincidence that large corporations tend to organize themselves in a hierarchy, with the CEO at the top and Individual Contributors (ICs) at the bottom. This structure works best because it allows for higher quality decision making at specific nodes in the tree.


For example, a software engineer is equipped to make much more informed decisions about database architecture than the CTO. Likewise, the CTO can make better decisions about which new product to allocate engineering resources to than the IC. Context rot is a vulnerability shared by both agents and humans.


Notice also that the more senior decision makers in an organization tend to live higher up the tree. We can think of this as highly intelligent agents spawning lesser intelligent (and less costly) subagents to complete smaller, scoped tasks.


If we’ve learned anything from human enterprises about how work gets done efficiently, it’s that work is just a series of decisions. When a massive amount of work needs to be done, smaller decisions are delegated to local decision-makers who— by having a narrower, highly focused context— are able to make higher quality choices than the chief decision maker.


## Why Code?


In my last article, I argued that[the way agents should perform tasks is via durable code execution](https://www.notion.so/Why-the-future-of-work-is-durable-code-execution-323b517fbd018061ace4dcc67f0da590) . It is simply better for context management, flexibility, and tool composition.


Now, I’m arguing that work is completed best through subagent orchestration. And it’s exactly here that the traditional visual workflow model starts to break down. Code isn’t just better because it’s “more powerful”—it’s better for strictly operational reasons:


- **Native handling of complex logic:** Code handles branching, loops, dynamic fanout, and nesting naturally. Try building a dynamic “for-each” loop that triggers an unknown number of nested subagents in a visual drag-and-drop builder; it quickly becomes an unmaintainable mess.
- **AI generation:** LLMs natively “speak” code. It is far easier for an AI to generate, edit, and debug a Python script than it is to wrangle rigid visual configs or massive, highly specific JSON schemas.
- **Abstractions and reuse:** Code can express abstractions seamlessly. You can write a function or a subagent once and reuse it across a hundred different automations, rather than copying and pasting nodes across multiple UI canvases.
- **True version control:** You get proper versioning, testing, peer review, and rollbacks with GitHub — standard software engineering practices that visual builders struggle to replicate.


Historically, one of the biggest benefits of a visual workflow tool was state management — they were automatically checkpointed and retried. If a workflow broke halfway through, you didn’t have to start from the top; you could just resume from the last successful step. But today, this kind of durable state management is completely possible in pure code, thanks to platforms like[Inngest](https://www.inngest.com/) and[Hatchet](https://hatchet.run/) .


Ultimately, it comes down to this: The more autonomy you add to a system, the less its execution path can be fully known ahead of time. Once that happens, a static visual workflow stops being the source of truth. The source of truth has to be code.
