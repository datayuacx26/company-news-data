---
schema_version: "1.0.0"
document_id: "ce1938080a8c16038867e09f479e2b2e719ba622e36a79a1650e2ecf84b204ad"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/stop-building-scaffolding-mcp-vs-cli/"
published_at: "2026-03-14T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:93fe6054a78fb8aeb781afc9da4e9357c5fca0ab72d06bbff3e262607946d960"
---

# Stop Building Scaffolding: Who Cares About MCP vs CLI

The bitter lesson of AI infrastructure over the past three years is simple: any scaffolding built to compensate for model limitations is invariably destined to become obsolete. Not because the scaffolding was badly built. But because the models got smarter.


The debate about whether AI agents should use MCP or CLIs is the latest instance of this pattern, and it’s missing the point entirely. Both sides are arguing about scaffolding.


As AI agents become capable of taking real actions (querying databases, calling APIs, running commands) teams need a way to connect them to tools. One camp argues for MCP (Model Context Protocol), a standardized protocol that lets models discover and invoke tools through a common interface. The other argues that models should just use CLIs and existing shell tooling directly, the same interfaces humans use. MCP proponents say standardization brings consistency and safety. CLI proponents say why add a new layer when the tooling already exists.


Both camps are debating the adapter. Neither is asking whether adapters are the right frame at all.


## The bitter lesson nobody learns1


Remembers RAG? LlamaIndex? Prompt engineering?


Hundreds of millions of dollars flowed into vector databases, elaborate retrieval pipelines, chunking strategies, and embedding stores. Entire companies were built on the premise that models couldn’t hold enough context, so you had to surgically inject the right information at the right time.


GPT-3 launched in June 2020 with a 4,096 token context window. Gemini 3 Pro today supports 2 million tokens. That’s a 488x increase in less than five years. The RAG thesis was built on a constraint that’s erodes every six months with the next model release.2


Reasoning scaffolding went the same way. Chain-of-thought prompt engineering, multi-step workflow engines, orchestration frameworks — all existed because models couldn’t reason reliably on their own. Then reasoning got baked into the model itself. The scaffolding didn’t survive. The capability did.


## MCP is scaffolding. Models are already moving past it.


Anthropic introduced MCP in November 2024 to solve a real problem: models had no standardized way to connect to external tools and data sources. Every integration was custom-built. MCP was the universal adapter, one protocol, implemented once, that any model could use to talk to any tool. RAG was scaffolding to get information into the model. MCP is scaffolding to get actions out of it. Both exist for the same reason: to paper over a capability gap.


By March 2026, models are already acting on the world without being told how. I asked Claude Code to find vulnerabilities in my codebase and generate a PDF report. Claude downloaded a Python package, installed it, wrote a custom script to convert the markdown output to PDF, and delivered the report. No MCP server. No CLI wrapper. It just figured it out.


Last month, I onboarded our Head of GTM into our monorepo. I told her to install Claude. Claude installed the GitHub CLI, walked her through authentication, cloned the repo, and opened a new branch. The model adapted to the world. It didn’t need the world adapted to it. At Formal, our customers just drop our gRPC docs into Claude and interact with our API that way. No MCP server, no wrapper. The type system is enough.


So: were MCPs useful? For a moment, yes. Will we still use them in the future? Maybe. Maybe not. The models are getting smarter. That’s the only variable that actually matters.


## If models will figure out how to use tools, the real question is who controls what they do


The most common pushback is that MCP provides security and governance guarantees that raw tool use doesn’t. This conflates two things that should be kept separate.


MCP is a connectivity protocol. It defines how a model discovers and invokes tools. It was never designed to be a security boundary3 , but somehow it’s being positioned as the governance layer for AI agents. This is a category error, and it’s a dangerous one, because it gives teams a false sense of control.


MCP is already a bad abstraction for a lot of protocols. Take the example of SSH. SSH is a stream of bytes with its own authentication and encryption model. You can wrap it in JSON-RPC, but you lose the efficiency, you lose the native security model, and you gain nothing except a new abstraction your team has to maintain. The same applies to battle-tested layer 7 protocols we’ve optimized for decades, such as pg-wire, gRPC, the Kubernetes API, and every other protocol that already has a well-designed wire format. These protocols already carry their contracts in their type systems. The type is all the documentation you need.


The question isn’t whether MCP makes tool discovery easier. It does. The question is whether tool discovery is the bottleneck that matters. And increasingly, it isn’t, because models are figuring out tool discovery on their own, and security was never MCP’s job in the first place.


## Ask what won’t change


When thinking about the future, I’ve found it more useful to ask what won’t change rather than what will. Some safe bets:


- **Operating systems** : the kernel, userspace, filesystem, and process model aren’t going anywhere.
- **The OSI model** : the layered abstraction of how bytes move across a network has survived every wave of infrastructure reinvention.
- **Cryptographic primitives** : public key infrastructure, symmetric encryption, certificate chains. The math and the trust model are stable. We already have quantum-resistant cipher suites.


What do these have in common? They’re not abstractions over a temporary limitation. They’re models that reflect something true about how computation, networks, and trust actually work at a fundamental level.


The same pattern holds at the protocol layer. CORBA and SOAP promised universal interoperability. Both dead. The protocols that survive model something structurally true: TCP models reliable byte streams, TLS models authenticated encryption, gRPC models typed procedure calls.


MCP doesn’t belong on either list. It’s an abstraction over a capability gap, the gap between what models can do today and what they’ll be able to do next quarter. And those gaps close.


## The real problem agents expose


The reason to stop caring about MCP vs CLI isn’t nihilism. It’s so you can focus on the problem that actually won’t go away.


From first principles, the biggest risk with AI agents, the one everyone is attempting to fix, is least-privilege. And least-privilege, at its core, is just about controlling bytes that move between two processes. You can call the client process an agent, a script, or a GUI. You can call the server-side process a database, an API, or a VM. The wire can be localhost or the internet. Fundamentally, none of that changes the problem.


We’ve always had this problem. We just didn’t care enough to solve it properly, because humans are slow. A human engineer executes maybe a few dozen meaningful actions in a workday. An AI agent can execute thousands of actions an hour. The least-privilege problem we’ve been quietly ignoring for decades is now under a very bright light.


At Formal, we believe least-privilege implementations have always failed because enforcement happens in the wrong place. Permissions are scattered across systems that don’t talk to each other, applied without context about who’s asking or why, and too coarse to match what any given identity actually needs. If you can inspect and manipulate packets on the wire across any protocol, for any identity, with full context about what that identity is allowed to do and why, you solve the problem at the layer that was always there. No new protocol required.


That’s the conversation worth having. Not MCP vs CLI.


---


1


Rich Sutton,[The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) (2019). Sutton’s original argument is about AI research broadly: any approach that bakes in human knowledge is eventually overtaken by general methods that scale with computation. The same dynamic applies to AI infrastructure. Scaffolding is the infrastructure equivalent of hand-engineered features.


2


A caveat worth noting: if energy and compute constraints tighten significantly over the next five years, through supply chain pressure, inference demand outpacing energy production, or resource scarcity, efficiency will reassert itself as a design constraint. In that scenario, RAG and other context-optimization techniques may prove more durable than the context window trajectory alone suggests.


3


Anthropic,[Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) (2024). MCP was designed as an open standard for connecting AI models to external data sources and tools, not as a security or governance layer.
