---
schema_version: "1.0.0"
document_id: "7797e1051378dc9b4a579cc3852cadce5b5c23bc9f0845725d8418d4bb6729da"
company_key: "yc-paragon"
company: "Paragon"
source_id: "yc-paragon-news-import-425709159824"
canonical_url: "https://www.useparagon.com/blog/what-are-agent-skills"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-13T22:52:47.126118+00:00"
fetched_at: "2026-08-13T22:52:50.044387+00:00"
content_hash: "sha256:4c997d2f7afb39ddd1c24704eb671f74d38e87eeb16ff8b5d3d6dcc2ce35eaba"
---

# What Are Agent Skills?

# What Are Agent Skills?


Agent Skills are a specification from Anthropic for packaging an AI agent's specialized
instructions, reference files, and executable scripts into a single folder the agent loads only
when a task requires it. Each Skill is defined by a` SKILL.md` file: the agent reads its name and
description first, then loads the rest of the folder on demand — a pattern called progressive
disclosure.


*Checked against Anthropic's documentation in August 2026. Agent Skills is a young, fast-moving spec, and field requirements or platform support can change — confirm against the current docs before relying on specifics here.*


## What are agent skills?


A Skill packages what would otherwise be re-explained to an agent every session: instructions for
the task, whatever reference material backs them up, and — when the job calls for it — a script to
run. Anthropic introduced the format as a way to give agents "modular capabilities" without loading
all of that context into every conversation, per[Anthropic's own framing of the spec](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) .


A Skill is usually one of:


-


A domain procedure — how a specific team reviews code, formats a report, or files a support ticket.


-


A tool-usage pattern — how to work with a specific file format, spreadsheet layout, or API.


-


A reusable script — a small program that does the same job the same way every time, bundled
alongside the instructions that tell the agent when to run it.


Before Skills, an agent either carried all of that guidance in its system prompt on every turn —
degrading performance as unrelated instructions piled up — or the guidance lived nowhere and had to
be re-explained each session. Skills solve this by keeping the guidance on disk and loading it in
stages: a short summary is always present, the full detail loads only when the task calls for it, and
any bundled script runs without its source code ever entering the conversation.


## How agent skills are structured


Every Skill is a directory, and every Skill directory requires exactly one file:` SKILL.md` . Its YAML
front matter carries two required fields —` name` and` description` — and those are the only parts of
the Skill loaded into the agent's context by default, for every Skill it has installed, before any
task starts.


```text
---
name  :    proofreading
description :    Reviews   written   articles   for    grammar  ,    tone  ,    and   clarity  .  Use    when   the   user   asks   for    a   proofread  ,    grammar   check  ,    or   writing   feedback


```


```text
---
name  :    proofreading


```


```text
---
name  :    proofreading


```


```text
---
name  :    proofreading


```


The` description` field does the real work: the agent matches an incoming request against it to
decide whether the Skill is relevant. Only once triggered does the agent read the rest of` SKILL.md`
— the body, containing the actual instructions — into context. If that body references other files,
the agent reads those too, but only the ones the task touches. Anthropic's[Skill authoring guidance](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) describes
three loading levels: metadata (always loaded, roughly 100 tokens per Skill), instructions (loaded
when triggered, kept under a token ceiling), and bundled resources or scripts (loaded only as
referenced).


Scripts are the third piece. A Skill folder can bundle executable code — a Python script that
validates a form, a script that formats output a specific way — and the agent runs it directly rather
than generating equivalent code from scratch each time. Critically, only the script's *output* enters
the agent's context; the script's source code does not. A representative Skill folder looks like this:


```text
proofreading  /
├──   SKILL  . md
├──   STYLE  - GUIDE  . md
└──   scripts  /
├──   grammar  - check  . py
└──   format  - output  . py
```


```text
proofreading  /
├──   SKILL  . md
├──   STYLE  - GUIDE  . md
└──   scripts  /
├──   grammar  - check  . py
└──   format  - output  . py
```


```text
proofreading  /
├──   SKILL  . md
├──   STYLE  - GUIDE  . md
└──   scripts  /
├──   grammar  - check  . py
└──   format  - output  . py
```


```text
proofreading  /
├──   SKILL  . md
├──   STYLE  - GUIDE  . md
└──   scripts  /
├──   grammar  - check  . py
└──   format  - output  . py
```


## When to use agent skills (and when not to)


Agent Skills fit well when:


-


**A capability is narrow and situational.** The agent needs specialized guidance only sometimes —
Skills stay off the context budget until that moment arrives.


-


**The task is long or complex enough that unmanaged context degrades output quality.** Long-running,
multi-step work accumulates unrelated instructions over time; progressive disclosure keeps a Skill's
detail out of context until it's needed.


-


**The agent should be user-extensible.** Because a Skill is just a folder of markdown and scripts,
people without deep familiarity with an SDK or protocol can author one, which lets an agent's own
users add capabilities.


Agent Skills fit poorly when:


-


**The data behind the task is dynamic and needs to stay current.** A Skill's content is static
markdown on disk. Retrieving live, frequently-changing information — records in a database, files
that change hourly — is a retrieval-augmented generation (RAG) problem: vector search, graph
queries, or text-to-SQL. A Skill's static markdown can't do that job.


-


**The code needs to run the same way on every single call.** Tool descriptions travel with every
relevant prompt, so a tool is more reliably invoked than a script buried a level or two inside a
Skill folder. Skills that aren't triggered simply never load —[independent testing by Vercel](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals) found Skills
weren't always reliably triggered in benchmark tasks, where an always-present system prompt
performed more consistently.


-


**The work requires multi-tenant, authenticated, server-side execution.** A Skill's scripts run in
the agent's own local environment with whatever access that environment already has — there's no
built-in notion of isolating one user's credentials from another's. Serving many authenticated
clients from one process needs a separate server process — commonly an MCP server — not a Skill's
local scripts.


## Agent skills vs. tools vs. MCP


The three terms describe different mechanisms for extending what an agent can do, and they are not
interchangeable:


-


A **tool call** is a single, declared function the model invokes directly. Its description is sent
with every prompt (or every prompt where the tool is available), which makes it consistently
discoverable but also a fixed context cost regardless of whether the task needs it.


-


[MCP](https://www.useparagon.com/blog/ai-agent-tool-calling-access-saas-apps) (Model Context Protocol) is a client-server
protocol: an MCP server exposes tools, prompts, and resources over a connection a client calls into
— the fuller mechanics are covered at that link. The spec was written stdio-first, for a server
running as a local process; a remote HTTP transport was added later, and the two coexist today.
Either transport can carry an authenticated, per-client session, but the protocol makes that
optional — a given server still has to be built to support it.


-


An **agent Skill** is a directory-based bundle of instructions and, optionally, scripts, loaded on
demand into the agent's own local environment rather than served by a separate process. A Skill has
no built-in concept of a second tenant; whatever environment the agent is running in is the only
environment the Skill's scripts execute in.
