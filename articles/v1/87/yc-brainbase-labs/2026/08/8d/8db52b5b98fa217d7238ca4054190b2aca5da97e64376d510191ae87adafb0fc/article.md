---
schema_version: "1.0.0"
document_id: "8db52b5b98fa217d7238ca4054190b2aca5da97e64376d510191ae87adafb0fc"
company_key: "yc-brainbase-labs"
company: "Brainbase Labs"
source_id: "yc-brainbase-labs-news-import-4e6e4e598b0a"
canonical_url: "https://brainbaselabs.com/blog/universal-managed-agents"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T23:50:10.484155+00:00"
fetched_at: "2026-08-12T23:50:15.713808+00:00"
content_hash: "sha256:140dfcccdc1b6a5ff07969b1bf7ac4943bd911a46a2ab222fd9182560e06d181"
---

# Universal Managed Agents

Today we're introducing Universal Managed Agents: a single API to deploy agents across 8+ harnesses, including Claude Code, Codex, and OpenCode, without managing infrastructure or credentials.


One request creates an agent, provisions it a computer, and puts it to work:


Request · POST /v2/threads


bash


```text
curl https://api.brainbaselabs.com/v2/threads \
-H   "Authorization: Bearer $BRAINBASE_API_KEY"   \
-H   "Content-Type: application/json"   \
-d   '  {
"agent"  :   {
"harness"  :     "claude_code"  ,
"model"  :     "claude-sonnet-5"  ,
"instructions"  :     "You are a concise research assistant."
},
"input"  :     "In two sentences, what is MCP?"
}   '
```


Response · 201 Created


json


```text
{
"thread_id"  :     "a4fe9c41-7fea-4b93-91e4-081329e35865"  ,
"agent_id"  :     "54850f1a-e68f-46ac-a85c-01f977101f91"  ,
"status"  :     "running"
}
```


The response returns before the agent finishes. From that point on you are talking to a running colleague: poll the thread, append messages, or subscribe to a live event stream of everything it does.


Universal Managed Agents expands on Anthropic's Claude Managed Agents, which pairs Claude with an Anthropic-run agent loop and sandbox. We think that shape — agents as a managed resource rather than a pile of infrastructure — is right. Universal Managed Agents generalizes it to any model, any harness, and any sandbox provider.


Agents are customized at creation using the open-source[Universal Agent Protocol (UAP)](https://universalagentprotocol.io/) : one spec that declares instructions, MCP servers, skills, permissions, and secrets, independent of the harness that will run them. Once created, agents live in secure sandboxes with their own computers, and you can message them for as long as the thread lives.


## # Supported models and harnesses


A harness is the runtime an agent works inside: how it receives work, touches files, runs commands, and reports progress. Universal Managed Agents launches with eight, all behind the same request shape — the same payload, the same response, switched with a single` harness` field.


Harness API value Built for


Kafka


` kafka_cloud` Production knowledge work, surfaces, and orchestrations


Claude Code


` claude_code` Codebase-focused work and developer iteration


Codex


` codex` Code implementation, review, and workspace tasks


Cursor


` cursor` Agentic coding on Cursor's runtime


Factory Droid


` factory` End-to-end software engineering tasks


OpenCode


` opencode` Open-source coding agent workflows


Qoder


` qoder` Coding tasks on Qoder's agentic runtime


Qwen Code


` qwen` Coding workflows on Qwen models


Models are just as interchangeable. Pass any model ID as the` model` field: GPT-5.6 Sol, Terra, and Luna from OpenAI, Claude Opus 4.8 and Sonnet 5 from Anthropic, Gemini 3.5 Flash from Google, Grok 4.5 from xAI, and open-weights models like GLM 5.2, DeepSeek V4, and Kimi K2.7 Code served through Baseten. Because the model and the harness are independent fields, pairings that used to take an integration — Kimi inside OpenCode, Gemini inside Codex — are now a one-line change.


## # Example agents


The fastest way to get a feel for the API is to read a few requests.


Give an engineering agent a red test suite and a warm sandbox. Its entrypoint prepares the repository before the first turn; the agent runs the tests, fixes the defect, and keeps iterating until the suite is green:


Fix a failing test suite


json


```text
{
"agent"  :   {
"harness"  :     "claude_code"  ,
"model"  :     "claude-sonnet-5"  ,
"instructions"  :     "Turn failing test suites green without changing test files."  ,
"entrypoint"  :     "git clone $REPOSITORY_URL /workspace/app && cd /workspace/app && pip install -e '.[test]'"
},
"input"  :     "Run pytest in /workspace/app, fix the defect, and verify the full suite passes."
}
```


A product agent can build a small web app, start its server, and leave it running in the sandbox. Resolve port 3000 through the machine preview API and the result is live immediately, with no deployment step:


Ship a live app


json


```text
{
"agent"  :   {
"harness"  :     "claude_code"  ,
"model"  :     "claude-sonnet-5"  ,
"instructions"  :     "Build small web apps and keep their server running."
},
"input"  :     "Build a single-page TODO app, then serve it on 0.0.0.0:3000 in the background. Leave the server running."
}
```


To compare harnesses on your own work, send the same task to several agents in parallel. Each gets an isolated sandbox; when they finish, download the same output file from every thread and grade the results against one hidden test suite:


One task, four harnesses


json


```text
[
{
"agent"  :   {   "harness"  :     "claude_code"   },
"input"  :     "Fix search() in solution.py. Write the final implementation to solution.py."
},
{
"agent"  :   {   "harness"  :     "codex"   },
},
{
"agent"  :   {   "harness"  :     "cursor"   },
},
{
"agent"  :   {   "harness"  :     "opencode"   },
}
]
```


These are three real patterns built from the same primitives: create a thread, let the agent work in its sandbox, then continue the thread or inspect its machine. Follow-up messages reuse the warm sandbox, every turn is available over a live SSE event stream, and file operations let you pull artifacts back out. The full surface is documented at[docs.brainbaselabs.com/api](https://docs.brainbaselabs.com/api) .


## # Architecture


When a request arrives, the UAP spec is normalized and content-addressed: identical specs resolve to the same` agent_id` , so retries and fan-outs never mint duplicate agents. The control plane then provisions a machine with your chosen sandbox provider, translates the spec into the target harness's native configuration, and starts the run.


Your application


POST /v2/threads


Universal API


UAP agent spec · content-addressed agents · threads, messages, events


Harness adapters


kafka_cloud


claude_code


codex


cursor


factory


opencode


qoder


qwen


Sandbox providers


modal


daytona


e2b


One request shape in; any harness, any model, any sandbox underneath.


Everything after creation hangs off the thread. A thread carries the transcript, the event log, and the machine. Its status moves through` running` ,` success` ,` fail` ,` need_more_info` , and` idle` — an agent that needs a decision from you parks itself instead of guessing. Files move in and out of the agent's machine through a files API, and long-lived agents can share a mounted folder across threads.


## # Comparison to Claude Managed Agents


Claude Managed Agents got the managed-agent shape right: persisted agent configs, server-run sessions, skills and MCP, secrets that never touch your client. Universal Managed Agents keeps that shape and removes the constraints on what runs inside it.


Universal Managed Agents Claude Managed Agents


Models Configurable:


Any provider — OpenAI, Anthropic, Google, xAI, plus open-weights models via Baseten


Fixed:


Claude models


Harnesses Configurable:


Eight at launch: Kafka, Claude Code, Codex, Cursor, Factory Droid, OpenCode, Qoder, Qwen Code


Fixed:


Anthropic's Claude harness


Sandboxes Configurable:


Modal, Daytona, and E2B, natively


Fixed:


Anthropic-hosted containers, or self-hosted


Configuration Configurable:


One UAP spec: instructions, MCP servers, skills, secrets, entrypoint


Configurable:


Agent objects: system prompt, MCP servers, skills, tools, permission policies


Conversations Configurable:


Threads with messages and an SSE event stream


Configurable:


Sessions with messages and an SSE event stream


Secrets Configurable:


Planted into the sandbox environment at creation


Configurable:


Vault credentials substituted at egress


Agent identity Configurable:


Content-addressed — the same spec is the same agent


Configurable:


Versioned configs — sessions pin a version


**Configurable** — pick any option, per agent


**Fixed** — one vendor-decided option


If you're already on Claude Managed Agents, the mental model carries over almost one-to-one: agents map to agents, sessions to threads, environments to machines. The difference is that each layer is now a field you can change.


## # Sandboxes


Universal Managed Agents has native support for Modal, Daytona, and E2B sandboxes. Every thread runs on its own machine — a real computer with a filesystem, a shell, and network access — never on shared infrastructure, and never on yours.


The provider is one more field. Set` machine_kind` on the agent, override the base image per provider with a` snapshot` , and use` entrypoint` to install dependencies before the first turn. Machines are first-class API objects too, with their own lifecycle, status, and preview endpoints:


POST /v2/machines


bash


```text
curl https://api.brainbaselabs.com/v2/machines \
-H   "Authorization: Bearer $BRAINBASE_API_KEY"   \
-H   "Content-Type: application/json"   \
-d   '  {
"kind"  :     "daytona"  ,
"agent_id"  :     "54850f1a-e68f-46ac-a85c-01f977101f91"  ,
"snapshot"  :     "python-3.12-playwright"
}   '
```


Universal Managed Agents is available today for every Brainbase workspace. Start at[app.brainbaselabs.com](https://app.brainbaselabs.com/) , or create your first thread straight from the[API docs](https://docs.brainbaselabs.com/api) .


Managed agents made agents feel like a resource instead of a project. Universal Managed Agents makes that resource universal.
