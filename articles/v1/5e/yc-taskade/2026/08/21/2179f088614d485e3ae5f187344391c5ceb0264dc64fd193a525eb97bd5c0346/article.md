---
schema_version: "1.0.0"
document_id: "2179f088614d485e3ae5f187344391c5ceb0264dc64fd193a525eb97bd5c0346"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/mcp-protocol-history"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-01T00:10:34.677292+00:00"
fetched_at: "2026-08-01T00:10:35.697221+00:00"
content_hash: "sha256:9a2a5a6fffc9aa6b6a3d8125a3b73bea1adbe0cc0749724cbb0be639c0234941"
---

# The History of the Model Context Protocol: How AI Got a Universal Plug (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


The History of the Model…


On this page (17)


In July 2024, an engineer at Anthropic named David Soria Parra got tired of copying and pasting. He would ask Claude Desktop a question, get an answer, and then move that answer by hand into his code editor. The model was capable. It just could not reach anything.


Seventeen months later, the protocol he sketched to fix that annoyance was the connective tissue of the AI industry.[ChatGPT, Claude, Gemini, Microsoft Copilot, VS Code and Cursor](https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/) all speak it. Its two largest SDKs pulled more than 470 million downloads in the 30 days ending July 21, 2026. And it no longer belongs to Anthropic at all.


This is the complete, primary-sourced history of the[Model Context Protocol](https://www.taskade.com/wiki/ai/model-context-protocol) .


> **TL;DR:** MCP is the open standard that lets any AI app talk to any tool through one interface, announced by Anthropic on[November 25, 2024](https://www.anthropic.com/news/model-context-protocol) and donated to the Linux Foundation on December 9, 2025. It collapsed the N times M integration problem, shipped four spec revisions in its first year and a half, and goes fully stateless with the fifth on July 28, 2026.[Connect your workspace to it](https://www.taskade.com/learn/connect/mcp-server) .


---


## What Is the Model Context Protocol?


The Model Context Protocol is an open standard that lets an AI application connect to external tools and data through one common interface, instead of one bespoke connector per pairing. It uses JSON-RPC messages, defines three things a server can offer (tools, resources, and prompts), and negotiates a version at connection time. Anthropic published it under an open license on November 25, 2024 and handed governance to the Linux Foundation just over twelve months later.


The shortest useful description is the one the industry settled on: MCP is a universal plug. Before USB, every peripheral shipped its own cable, its own connector, and its own driver. After USB, the peripheral implements the port once and every computer can use it. MCP does that for the space between a model and the world.


Three terms do most of the work, and they are worth getting straight:


Term What it is Who builds it Example


**Host** The AI application a person actually uses AI product teams Claude Desktop, ChatGPT, VS Code


**[Client](https://www.taskade.com/wiki/ai/model-context-protocol-client)** The connector inside the host that speaks MCP Same team as the host One client per connected server


**[Server](https://www.taskade.com/wiki/ai/model-context-protocol-server)** The wrapper around a tool or data source Anyone, including you A GitHub server, a database server, a workspace server


A server exposes three kinds of capability. **Tools** are actions the model can invoke, which is[tool calling](https://www.taskade.com/wiki/ai/tool-calling) with a shared wire format. **Resources** are readable context, like a file or a record. **Prompts** are reusable templates the host can surface to the user. That split matters more than it looks: it separates "things the model may do" from "things the model may read," which is the seam every permission model since has been built on.


---


## The Problem MCP Solved: N Times M Integrations


Before MCP, connecting 10 AI applications to 100 tools required up to 1,000 separate integrations, each with its own authentication, schema, retry logic, and maintenance burden. A shared protocol turns that multiplication into addition. Ten clients plus one hundred servers is 110 pieces of work, built once, by the party that actually understands each side.


This is the same arithmetic that made the Language Server Protocol succeed in code editors, and it is not a coincidence. Soria Parra brought the idea over from LSP, which his co-creator has said was new to him at the time and made immediate sense once he saw what it solved.


### MCP vs API vs Plugin vs Webhook


These four get conflated constantly, and the differences decide which one you should reach for. The clean split is direction, discovery, and who reads the documentation.


**MCP** **REST API** **Plugin** **[Webhook](https://www.taskade.com/wiki/integrations/webhooks)**


**Who initiates** The model, mid-task Your code The host app The remote system


**Discovery** At runtime, from the server Read the docs first Host-controlled catalog None, you subscribe


**Portability** Any MCP host Any code you write One vendor only Any HTTP endpoint


**Direction** Two-way, request and response Two-way Two-way One-way push


**Best for** Giving an agent live reach Deterministic system-to-system Vendor-blessed extensions Reacting to events


The distinction people miss: an API is written for a programmer who reads the documentation before writing a line. An MCP server describes itself at runtime, so the model can list what is available, read the schema, and decide what to call with nobody writing glue code first. That is why MCP servers show up inside[agent harnesses](https://www.taskade.com/wiki/ai-agents/agent-harness) rather than inside build pipelines.


---


## Before MCP: The Plugin Era That Did Not Scale


The first serious attempt at giving models hands was the ChatGPT plugin ecosystem, launched in March 2023, and it failed for a structural reason: plugins belonged to one host. A developer who built a ChatGPT plugin could not run it anywhere else. OpenAI stopped new plugin conversations on March 19, 2024 and retired the program entirely on April 9, 2024, folding the capability into GPTs and Actions.


Three things happened in that window that made MCP both possible and necessary:


- **June 2023: function calling.** Models learned to emit structured calls against a schema you supplied. This solved the model's half of the problem and left the plumbing entirely to you. See[function calling](https://www.taskade.com/wiki/ai/function-calling) and[tool use](https://www.taskade.com/wiki/ai/tool-use) for the mechanics.
- **2023 to 2024: everyone built the same connector twice.** Each AI product wrote its own GitHub integration, its own Slack integration, its own database reader. All the same work, none of it shared.
- **2024: agents made it urgent.** A chatbot with one tool is a feature. An[agent](https://www.taskade.com/agents) that plans across ten tools needs those ten tools to behave consistently, or it cannot recover from a failure.


The Language Server Protocol had already proven the fix in a neighboring domain. Microsoft published LSP in 2016 so that one language implementation could serve every editor instead of one plugin per editor. MCP is the same trade applied to models rather than editors, and it kept both halves of what made LSP work: the N times M insight, and JSON-RPC as the message format.


---


## July 2024: A Frustrated Engineer and a Protocol Borrowed From Editors


MCP started as an internal-tooling irritation, not a strategy. In[an interview with Latent Space](https://www.latent.space/p/mcp) , Soria Parra described joining Anthropic a few months earlier, appreciating what Claude Desktop could do, and being unable to extend it. He kept moving work by hand between the chat window and his editor.


He took the idea to Justin Spahr-Summers, and the two started on it almost immediately after that conversation. By Spahr-Summers' account they spent a couple of months on what he called the really unrewarding bits, the specification and plumbing work that has to exist before anything looks impressive. The public record backs the timeline: the specification repository was created on GitHub on **September 24, 2024** , two months before anyone outside Anthropic heard the name.


The moment it stopped being a side project was an internal hackathon about a month before launch. Employees built MCP servers for whatever they had lying around, including one that drove a 3D printer. That is a strange proof point and also a perfect one: if a protocol designed for documents and databases can drive a printer without changes to the protocol, the abstraction is right.


One decision from that period shaped everything after it. The first editor to run MCP was not an Anthropic product. It was[Zed](https://zed.dev/) , the third-party editor, and Soria Parra says he wrote that implementation himself and had it working about a month and a half before the public release. A protocol whose first integration lands in somebody else's product, in the open, is much harder to read as a land grab.


---


## November 25, 2024: The Launch


Anthropic announced MCP publicly on November 25, 2024, open-sourcing the specification, SDKs for Python and TypeScript, local server support in Claude Desktop, and a repository of prebuilt servers for Google Drive, Slack, GitHub, Git, Postgres, and Puppeteer. The launch spec carried the version string` 2024-11-05` .


The reference server repository had been created on GitHub six days earlier, on November 19, 2024. It now has over **88,000 stars** .


Six companies were named as early adopters on day one, and the mix was the point: **Block** and **Apollo** on the enterprise side, **Zed** , **Replit** , **Codeium** , and **Sourcegraph** on the developer-tools side. Anthropic shipped no competing product in either category, which made the "open standard" framing credible rather than defensive.


> "Open technologies like the Model Context Protocol are the bridges that connect AI to real-world applications, ensuring innovation is accessible, transparent, and rooted in collaboration."
>
>
> Dhanji R. Prasanna, Chief Technology Officer, Block ([Anthropic announcement](https://www.anthropic.com/news/model-context-protocol) , November 25, 2024)


---


## The Complete Model Context Protocol Timeline


Every date below is verified against a primary source: the protocol's own specification and blog, the announcing company's post, or a public repository record.


Date Milestone What changed Why it mattered


Jul 2024 Work begins at Anthropic Soria Parra prototypes an LSP-style protocol for Claude Desktop The origin is an internal-tooling annoyance, not a roadmap item


Sep 24, 2024 Spec repository created First public artifact appears on GitHub Two months of quiet work before anyone knew the name


~Oct 2024 Internal hackathon Employees build MCP servers, including one driving a 3D printer Proved the abstraction generalized beyond documents


Nov 19, 2024 Reference servers repo created Google Drive, Slack, GitHub, Git, Postgres, Puppeteer Launch shipped with working examples, not just a spec


**Nov 25, 2024** **MCP announced** Spec` 2024-11-05` , Python + TypeScript SDKs, Claude Desktop support Block, Apollo, Zed, Replit, Codeium, Sourcegraph on day one


Mar 26, 2025 Spec` 2025-03-26` OAuth 2.1 authorization, Streamable HTTP transport, tool annotations Made remote, authenticated, production servers possible


Mar 26, 2025 OpenAI adopts MCP Sam Altman confirms support, Agents SDK first A direct competitor blessing the standard settled the format war


Apr 9, 2025 Google adopts MCP Demis Hassabis commits Gemini models and SDK Three of the largest labs now on one wire format


Apr 2025 Tool poisoning disclosed Invariant Labs shows malicious tool descriptions enter context as trusted text First hard evidence the trust model needed work


May 1, 2025 Claude Integrations Remote MCP servers, 10 launch partners Moved MCP off the desktop and onto the web


May 19, 2025 Microsoft Build Native Windows 11 MCP support announced An operating system, not just an app, became an MCP host


Jun 18, 2025 Spec` 2025-06-18` Structured tool output, elicitation, OAuth Resource Servers, RFC 8707 Security hardening plus the ability to ask the user a question mid-task


Jul 14, 2025 MCP GA in VS Code General availability announced, shipping in VS Code 1.102 The largest editor install base moves out of preview


Sep 8, 2025 MCP Registry preview Open catalog and API for public servers Discovery gets a single source of truth


Oct 6, 2025 OpenAI Apps SDK ChatGPT apps built on MCP, announced at DevDay MCP becomes a consumer distribution surface


Nov 21, 2025 MCP Apps proposed SEP-1865, co-authored by OpenAI and Anthropic maintainers Rival labs jointly authoring one spec


Nov 25, 2025 Spec` 2025-11-25` Tasks, extensions framework, OpenID Connect Discovery, URL elicitation First birthday ships long-running work and a modular core


**Dec 9, 2025** **Donated to the Linux Foundation** Agentic AI Foundation formed by Anthropic, Block, OpenAI 97M monthly SDK downloads, 10,000+ active servers at handover


May 21, 2026` 2026-07-28` release candidate Stateless core locked, handshake and session ID removed The first intentional breaking change since launch


Jun 29, 2026 Beta SDKs ship Python v2.0.0b1, TypeScript v2 beta, Go v1.7.0-pre.1, C# v2.0.0-preview.1 Four-week community beta before the spec lands


**Jul 28, 2026** **Spec` 2026-07-28` scheduled** Stateless protocol, formal extensions track, MCP Apps, tasks Servers scale behind a plain load balancer


The last row is the only one that has not happened yet. As of July 23, 2026 the current published revision is still` 2025-11-25` , and` 2026-07-28` is a release candidate in its final validation window.


---


## How MCP Actually Works


An MCP connection is a conversation between four parties, and understanding the shape explains most of the design decisions. The host application owns the model and the user interface. It spawns one client per server. Each server wraps exactly one system and advertises what it can do. The model never touches the network directly.


Two transports matter. **stdio** runs the server as a local subprocess, which is how most desktop setups work and why the earliest MCP servers were command-line programs. **Streamable HTTP** runs the server remotely over a normal web endpoint, which arrived in the` 2025-03-26` revision and replaced the original HTTP-plus-server-sent-events design. Remote transport is what turned MCP from a power-user desktop trick into something a company can host.


Each server capability answers a different question, and hosts treat them differently:


Capability Answers Controlled by Typical use


**Tools** "What can the model do?" The model, with user approval Create a task, run a query, send a message


**Resources** "What can the model read?" The host application A document, a record, a project


**Prompts** "What can the user trigger?" The user A saved workflow the server ships with


**Sampling** "Can the server ask the model?" The client A server that needs a summary mid-operation


**Elicitation** "Can the server ask the user?" The user Confirming a destructive action, added in` 2025-06-18`


Two of those are on their way out. The` 2026-07-28` release candidate deprecates Sampling, Roots, and Logging under the protocol's new feature lifecycle policy, pointing servers at direct provider APIs, tool parameters, and standard observability tooling instead. Deprecation is annotation-only: the methods keep working, and the policy guarantees at least twelve months before anything can be removed.


---


## 2025: The Year Every Rival Adopted It


MCP won the format war in four months, and the deciding move came from a competitor. On March 26, 2025 Sam Altman[posted](https://x.com/sama/status/1904957253456941061) that OpenAI would add MCP support across its products, starting that day in the Agents SDK. Two weeks later Demis Hassabis[committed Google's Gemini models and SDK](https://techcrunch.com/2025/04/09/google-says-itll-embrace-anthropics-standard-for-connecting-ai-models-to-data/) . By May, an operating system had joined.


Host MCP support shipped Mode Source


Claude Desktop Nov 25, 2024 Local stdio servers Anthropic launch post


Zed ~Oct 2024, six weeks pre-launch First MCP implementation in an editor Creator interview


OpenAI Agents SDK Mar 26, 2025 Client, ChatGPT and Responses API to follow Altman announcement


Google Gemini Announced Apr 9, 2025 Models and SDK Hassabis announcement


Claude web and desktop May 1, 2025 Remote servers, 10 launch partners Claude Integrations post


Windows 11 Announced May 19, 2025 Native OS-level support, private preview Microsoft Build 2025


VS Code and GitHub Copilot Jul 14, 2025 (GA) Agent mode, VS Code 1.102 GitHub changelog


ChatGPT apps Oct 6, 2025 Apps SDK built on MCP OpenAI DevDay


The May 1, 2025 Claude Integrations launch is the one worth pausing on. It brought remote MCP servers to the web with ten partners at once, including Atlassian, Zapier, Cloudflare, Intercom, Asana, Square, PayPal, Linear, and Plaid. Until that day, using MCP meant editing a JSON config file on your own machine. After it, connecting a tool meant clicking a button.


### The Spec Revisions, and What Each One Fixed


Version Headline change Practical effect


` 2024-11-05` The launch specification Tools, resources, prompts over JSON-RPC and stdio


` 2025-03-26` OAuth 2.1 plus Streamable HTTP Remote authenticated servers become possible


` 2025-06-18` Structured output, elicitation, RFC 8707 Servers return typed data and can ask the user questions


` 2025-11-25` Tasks, extensions, OIDC Discovery Long-running work and a modular, versioned feature surface


` 2026-07-28` (RC) Stateless core No handshake, no session ID, ordinary load balancing


One revision reversed itself, which is a healthy sign in a young protocol. JSON-RPC batching was added in` 2025-03-26` and removed one revision later in` 2025-06-18` , three months after it arrived.


---


## Security: The Bill for Trusting Tool Descriptions


MCP's central convenience is also its central risk: a tool description written by a third party lands in the model's context as trusted text. Invariant Labs disclosed this as **tool poisoning** in April 2025, showing that instructions hidden in a` tools/list` response are read by the model and normally never displayed to the user. In May 2025 the same researchers demonstrated a[live exploit against the widely used GitHub MCP server](https://invariantlabs.ai/blog/mcp-github-vulnerability) , where a malicious issue in a public repository could steer an agent into leaking private repository contents. Notably, that attack required no compromised tool at all.


The specification has been paying that bill down ever since:


- **` 2025-06-18`** classified MCP servers as OAuth Resource Servers, added protected resource metadata for discovering the authorization server, and required clients to implement[RFC 8707](https://www.rfc-editor.org/rfc/rfc8707.html) resource indicators so a malicious server cannot harvest a token meant for someone else.
- **` 2025-11-25`** added OpenID Connect Discovery, incremental scope consent through` WWW-Authenticate` , and URL-mode elicitation so credentials can be collected out of band rather than typed into a chat.
- **` 2026-07-28`** , still a release candidate, requires clients to validate the` iss` parameter on authorization responses per RFC 9207, has clients declare an OpenID Connect` application_type` during dynamic registration, and introduces a formal deprecation policy with a minimum twelve-month window before a feature can be removed.


The practical guidance has not changed since 2025. Treat every third-party MCP server as untrusted code running with your credentials. Scope tokens to the narrowest resource that does the job, keep a human approval step on destructive actions, and prefer servers you or your vendor operate. This is the same discipline that[agent permissions](https://www.taskade.com/wiki/ai-agents/agent-permissions) and[agent sandboxes](https://www.taskade.com/wiki/ai-agents/agent-sandbox) exist to enforce, and it is why the[agent environment](https://www.taskade.com/wiki/ai-agents/agent-environment) is now a design surface rather than an afterthought.


---


## December 9, 2025: MCP Leaves Anthropic


Anthropic donated MCP to the **Agentic AI Foundation** , a directed fund under the Linux Foundation, on December 9, 2025. The founding companies were Anthropic, Block, and OpenAI, with support from Google, Microsoft, AWS, Cloudflare, and Bloomberg. At handover the project reported **97 million monthly SDK downloads** and **more than 10,000 active servers** .


The governance split is the interesting part. The foundation board handles money, membership, and new-project approval. MCP itself keeps full autonomy over technical direction, its existing maintainer structure, and its community proposal process, where changes arrive as numbered SEPs that anyone can file. The first-birthday post two weeks earlier, on November 25, 2025, counted 58 maintainers around a steering group of 9, a Discord contributor community past 2,900, and more than 100 new contributors joining every week.


Donating a standard you invented is a real cost and a rational one. A protocol controlled by one lab is a protocol every other lab has a reason to fork. Handing it to a neutral foundation is what turns adoption into permanence.


---


## 2026: MCP Goes Stateless


The` 2026-07-28` revision is the first deliberate breaking change in MCP's history, and it exists because the original design assumed a desktop. The release candidate was locked on May 21, 2026 and the final specification is scheduled for **July 28, 2026** , which makes the ten weeks in between a validation window rather than a victory lap. The maintainers describe it as the largest revision of the protocol since launch, and say plainly that it contains breaking changes.


Two removals do the work. **SEP-2575** deletes the` initialize` and` initialized` handshake. **SEP-2567** deletes the` Mcp-Session-Id` header and the protocol-level session it implied. Protocol version, client info, and client capabilities now ride along in per-request metadata, and a new` server/discover` method fetches server capabilities when a client wants them up front.


```text
BEFORE  (2024-11-05 through 2025-11-25)      AFTER  (2026-07-28)
------------------------------------------   ------------------------------------
client  --> initialize            --> srv    client --> request (+ _meta) --> any
client  <-- capabilities          <-- srv    client <-- response          <-- any
client  --> initialized           --> srv
client  --> request (Mcp-Session-Id) --> srv  no handshake
client  <-- response                 <-- srv  no session id
no sticky routing
every later request must reach THE SAME       every request may reach ANY
server instance that did the handshake        healthy server instance


```


The practical consequence is boring and enormous: an MCP server can now sit behind an ordinary round-robin load balancer with no sticky sessions and no shared session store. That is the difference between a protocol that runs on a laptop and one that runs a public service. It also reframes a long-standing design question about whether agent connections should be[stateless or stateful](https://www.taskade.com/wiki/ai/stateless-vs-stateful) , and the answer the protocol chose is to push state into task handles rather than into the connection. What used to live in a protocol-level session now lives in the[agent session](https://www.taskade.com/wiki/ai-agents/agent-session) above it, where the host can actually reason about it.


Three other changes ship alongside it. The **extensions framework** existed in` 2025-11-25` but had no formal process behind it, so the new revision gives extensions reverse-DNS identifiers, independent versioning, and their own repositories, and the core stays small. **MCP Apps** graduates from proposal to official extension, letting servers ship interactive HTML interfaces that the host renders in a sandboxed iframe. And **tasks** move from experimental core feature to an extension, reshaped around stateless operation, which means anyone who built against the` 2025-11-25` experimental tasks API has a migration to do.


MCP Apps deserves its own note. The proposal, SEP-1865, was announced on November 21, 2025 and[co-authored by core maintainers from both OpenAI and Anthropic](https://blog.modelcontextprotocol.io/posts/2025-11-21-mcp-apps/) alongside the creators of MCP-UI. Two labs competing directly on frontier models jointly wrote one interface standard. That is the clearest signal available that MCP stopped being anybody's strategy and became infrastructure.


---


## What MCP Still Does Not Solve


MCP standardizes how a model reaches a tool. It says nothing about whether that tool is any good. Four gaps remain open in July 2026:


- **Quality is unranked.** The registry is a catalog, not a review board. A server with a thoughtful schema and a server that returns 40,000 tokens of unfiltered JSON look identical from the outside.
- **Context budget is the host's problem.** Connect six servers exposing thirty tools each and you have spent a meaningful share of the[context window](https://www.taskade.com/wiki/ai/context-window) before the user has typed anything. This is exactly the territory that[context engineering](https://www.taskade.com/wiki/ai/context-engineering) covers.
- **Descriptions are unverified prose.** The model decides what to call based on text a stranger wrote. Better tool naming guidance landed in` 2025-11-25` , but the incentive to oversell a tool is structural.
- **Multi-agent coordination is out of scope.** MCP connects a model to a tool. Connecting an agent to another agent is a different problem, addressed by the[agent-to-agent protocol](https://www.taskade.com/wiki/ai/agent-to-agent-protocol) and by the[handoff patterns](https://www.taskade.com/wiki/ai-agents/agent-handoff) that[multi-agent designs](https://www.taskade.com/blog/inter-agent-communication-patterns) rely on.


None of these are protocol failures. They are the layer above the protocol, which is where product design lives, and it is the same layer that decides whether an[agent stack](https://www.taskade.com/blog/ai-agent-stack) actually works in production.


---


## How Taskade Fits: A Hosted MCP Server for Your Real Work


Taskade runs a hosted MCP server, so the AI tools you already use can work with your actual projects instead of a copy you pasted into a chat window. Connect Claude Desktop, Cursor, VS Code, Windsurf, or any MCP-compatible client, and the assistant can list your agents, projects, and automations, then pull a full project in to work with it. The[Taskade MCP server guide](https://www.taskade.com/learn/connect/mcp-server) has the setup, and a[recent release](https://www.taskade.com/changelog) widened the connection so those tools can create and change work, not only read it.


The connection runs one direction, which is the part worth being precise about:


- **Inbound, on every paid plan:** your workspace becomes an MCP server other tools can call. See[the hosted server walkthrough](https://www.taskade.com/blog/hosted-mcp-server) and[the Claude and Cursor setup](https://www.taskade.com/blog/connect-claude-cursor-mcp) .
- **Outbound is not available on any plan:** nothing inside Taskade connects out to third-party MCP servers today. Agents get their reach from 34 built-in tools and from Taskade's own integration layer instead.


That is one half of the picture. The other half is that the project on the far end of the connection is a real one, with 7 views (List, Board, Calendar, Table, Mind Map, Gantt, and Org Chart, with timelines living inside Gantt),[100+ bidirectional integrations](https://www.taskade.com/integrations) , and[automations](https://www.taskade.com/automate) that keep running after the chat window closes. Taskade EVE, the agent that orchestrates work across those surfaces, draws on 15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers. Paid plans start at $10 per month billed annually on the[pricing page](https://www.taskade.com/pricing) , and you can see what people have built on[the community gallery](https://www.taskade.com/community) .


The reason this matters for a history post: MCP made the connection standard, and the interesting question moved one layer up. Not "can my AI reach my data," but "is what it reaches worth reaching."


---


## Frequently Asked Questions


What is the Model Context Protocol?


The Model Context Protocol is an open standard that lets AI applications connect to external tools and data through one common interface instead of a custom integration per pairing. Anthropic announced it on November 25, 2024 and donated it to the Linux Foundation's Agentic AI Foundation on December 9, 2025. It uses JSON-RPC and defines three server capabilities: tools, resources, and prompts.


Who created MCP and when?


David Soria Parra and Justin Spahr-Summers, both engineers at Anthropic. Soria Parra began the work in July 2024 after getting tired of copying context between Claude Desktop and his editor, borrowing the core idea from the Language Server Protocol. The specification repository appeared on GitHub on September 24, 2024, and the protocol went public on November 25, 2024.


Why was MCP created?


To collapse the N times M integration problem. Connecting 10 AI applications to 100 tools previously meant up to 1,000 bespoke connectors, each with its own auth, schema, and error handling. A shared protocol turns that into N plus M work: every application implements the client once, every tool implements the server once, and any client can talk to any server.


What MCP specification versions exist?


Four are published as of July 23, 2026, using date-based version strings:` 2024-11-05` (launch),` 2025-03-26` (OAuth 2.1 and Streamable HTTP),` 2025-06-18` (structured output, elicitation, OAuth Resource Server classification), and` 2025-11-25` (tasks, extensions, OpenID Connect Discovery), which is the current version. A fifth,` 2026-07-28` (stateless core), is a release candidate scheduled to become final on July 28, 2026. Clients and servers negotiate a shared version at connection time.


Which AI tools support MCP?


Claude, ChatGPT, Gemini, Microsoft Copilot, Visual Studio Code, Cursor, Zed, and Windsurf all ship MCP client support. OpenAI announced support on March 26, 2025 starting with its Agents SDK. Google announced Gemini support on April 9, 2025. Microsoft announced native Windows 11 support at Build on May 19, 2025, and MCP reached general availability in VS Code on July 14, 2025.


How is MCP different from a normal API?


A normal API is written for a programmer who reads the documentation first. An MCP server describes its own capabilities at runtime, so a model can list what is available, read the schema, and decide what to call without anyone writing glue code ahead of time. MCP also standardizes transport, auth handshake, and error shape, which a plain REST API leaves to each vendor.


Is MCP secure?


MCP has genuine security tradeoffs the specification has been tightening since 2025. Invariant Labs disclosed tool poisoning in April 2025, showing that malicious instructions hidden in a tool description enter the model's context as trusted text. The` 2025-06-18` revision classified servers as OAuth Resource Servers and required RFC 8707 resource indicators. Treat third-party servers as untrusted code and scope permissions narrowly.


Who owns MCP now?


The Linux Foundation. Anthropic donated MCP to the Agentic AI Foundation on December 9, 2025, a directed fund co-founded with Block and OpenAI and supported by Google, Microsoft, AWS, Cloudflare, and Bloomberg. MCP retains full autonomy over technical direction, and its maintainer structure and community SEP process are unchanged.


What changes in the 2026-07-28 MCP specification?


The protocol core goes stateless. SEP-2575 removes the` initialize` and` initialized` handshake, SEP-2567 removes the` Mcp-Session-Id` header, and requests can route to any server instance without sticky sessions. Protocol version and client capabilities travel in per-request metadata, and` server/discover` fetches capabilities on demand. The maintainers state that the release contains breaking changes, which is why the release candidate was locked on May 21, 2026 with a ten-week validation window.


How big is the MCP ecosystem?


The Agentic AI Foundation reported over 97 million monthly SDK downloads and more than 10,000 active servers in December 2025. Measured directly on the package registries for the 30 days ending July 21, 2026, the two largest SDKs alone recorded over 470 million downloads: 173.9 million on npm and 297.5 million on PyPI. The reference server repository has over 88,000 GitHub stars.


How does Taskade work with MCP?


Taskade runs a hosted MCP server, available on every paid plan, so Claude Desktop, Cursor, VS Code, Windsurf, and other MCP clients can read and search your real projects, agents, and automations. The traffic goes one way: external clients connect in, and Taskade agents do not call out to external MCP servers, which is not available on any plan. The[Taskade MCP server guide](https://www.taskade.com/learn/connect/mcp-server) covers the setup.


What does MCP still not solve?


MCP standardizes how a model reaches a tool, not whether the tool is safe, well described, or worth calling. It does not rank server quality, guarantee stable tool names across versions, stop a misleading description from steering a model, or manage the context budget when a server exposes hundreds of tools. Those stay with the host application and with context engineering.


---


## Further Reading


Protocol and history:


- [What is the Model Context Protocol?](https://www.taskade.com/wiki/ai/model-context-protocol) and the split between[MCP servers](https://www.taskade.com/wiki/ai/model-context-protocol-server) and[MCP clients](https://www.taskade.com/wiki/ai/model-context-protocol-client)
- [Tool calling](https://www.taskade.com/wiki/ai/tool-calling) ,[function calling](https://www.taskade.com/wiki/ai/function-calling) , and[tool use](https://www.taskade.com/wiki/ai/tool-use) , the mechanics MCP standardized
- [Agent harness](https://www.taskade.com/wiki/ai-agents/agent-harness) and[agent tools](https://www.taskade.com/wiki/ai-agents/tools) , the layer that decides what an agent may reach
- [The history of workflow automation](https://www.taskade.com/blog/automation-history) and[the history of Mermaid.js](https://www.taskade.com/blog/history-of-mermaid) , two other standards that won by being boring


Building with it:


- [The best MCP servers](https://www.taskade.com/blog/mcp-servers) and[an MCP server comparison](https://www.taskade.com/blog/mcp-server-comparison)
- [Building a hosted MCP server](https://www.taskade.com/blog/hosted-mcp-server) and[turning an OpenAPI spec into one](https://www.taskade.com/blog/openapi-to-mcp-code-generator)
- [The Taskade MCP server](https://www.taskade.com/blog/taskade-mcp-server) and[what MCP means for everyday work](https://www.taskade.com/blog/mcp)
- [Agent harnesses explained](https://www.taskade.com/blog/agent-harness-explained) and[agentic design patterns](https://www.taskade.com/blog/agentic-design-patterns)
- [The developer API](https://www.taskade.com/learn/connect/developer-api) for the cases MCP does not cover


---


MCP's real achievement was not technical elegance. It was restraint. A protocol small enough that a competitor could adopt it in four months, open enough that its first client belonged to somebody else, and boring enough to hand to a foundation before it became leverage. That is how a standard wins: the same way[Memory, Intelligence, and Execution](https://www.taskade.com/wiki/dna/three-pillars) compound inside a workspace, by being reliable long enough that everything else can be built on top of it.


▲ ■ ●


[Connect your workspace to any MCP client →](https://www.taskade.com/learn/connect/mcp-server)
