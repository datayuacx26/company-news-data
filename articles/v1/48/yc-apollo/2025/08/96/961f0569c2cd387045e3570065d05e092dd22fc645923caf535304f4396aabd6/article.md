---
schema_version: "1.0.0"
document_id: "961f0569c2cd387045e3570065d05e092dd22fc645923caf535304f4396aabd6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/mcp-server-builder-drop-july-highlights-from-san-francisco-and-new-york"
published_at: "2025-08-12T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:b3a37e699c828980395d0016d604ddc1f4789e9cb71cef07c04e064726d05910"
---

# MCP Server Builder Drop: July Highlights from San Francisco and New York

The MCP Server Builder community is on the move. In July, we hosted two packed meetups: one in San Francisco, one in New York that brought together engineers, toolmakers, and AI developers to explore what it means to build with and for LLMs using the Model Context Protocol (MCP).


Below, you’ll find a recap of every talk and demo, plus links to replays. Whether you joined in person or are just catching up, we hope this gives you a taste of the momentum and inspires you to join us for the next one.


Looking ahead to August, we’ve got two events you won’t want to miss. On **August 14th** , we’re hosting our first virtual session, where we’ll walk through[adding auth](https://www.apollographql.com/docs/apollo-mcp-server/guides/auth) to the community MCP server. We’re also back in San Francisco for our next in-person meetup.


[Subscribe to our Luma calendar](https://lu.ma/mcp-server?k=c) and RSVP to stay in the loop.


## San Francisco Builder Sessions


### Context engineering with MCP: Improving tool selection for and integrating a context layer through MCP – Nina Lopatina (Contextual AI)


Nina demonstrated how Contextual AI is building tools to help agents dynamically select the best MCP server for a given task and then use it in complex, multi-agent workflows.


**Key Highlights:**


- Introduced *Dynamic MCP Server Selection* using Contextual AI’s instruction-following reranker. Instead of guessing, the system evaluates 5,000+ MCP servers using metadata and semantic matching.
- Showcased a demo using this reranker to identify the best MCP server to retrieve biomedical information, send emails, and perform other complex tasks, outperforming baseline in-context methods.
- Demonstrated *multi-agent collaboration* using remote MCP servers: a movie RAG agent found critic-vs-audience score disparities, and another agent sent an email (including an auto-generated haiku!) using “activepieces” server.


**Why it matters:** With so many MCP servers to choose from, manually selecting or in-context prompting often falls short. Nina showed how metadata-aware reranking and remote MCP workflows can save developers hours—and unlock richer, agentic orchestration.


Check out Nina’s[full session on YouTube](https://youtu.be/tA0-xf8https:/youtu.be/SM_E0Q1ZkGM?feature=shared5FuM?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/ninalopatina) .


### How to build with Google’s Agent Development Kit – Annie (Qingyue) Wang (Google)


Annie showed how to use Google’s Agent Development Kit (ADK) to build, expose, and consume MCP tools by walking through a full-stack demo that creates and connects agents with minimal boilerplate.


**Key Highlights:**


- Demonstrated how ADK simplifies the entire agent lifecycle, with built-in evaluation, multi-modal input, and fast deployment via Cloud Run or Vertex AI.
- Built an app with two tools (“Create Post” and “Create Event”), exposed them as MCP tools using ADKToMCPToolType, and connected them back to an ADK-based agent—all live on stage.
- Highlighted how ADK can act as both an MCP client (to consume tools) and an MCP server (to expose tools), allowing for modular, testable, and production-ready agent systems.


**Why it matters:** Annie made the case for ADK as the fastest path to production agent tooling especially for teams that want to test locally, deploy globally, and plug into the MCP ecosystem with minimal glue code.


Check out Annie’s[full session on YouTube](https://youtu.be/5Mnw92JvTyo) and connect with them on[LinkedIn](https://www.linkedin.com/in/anniewangtech) .


### Building an AI QA Engineer with AI – YK Sugi (Sourcegraph)


YK walked through how he’s building a “computer-use” MCP server to give agents the power to simulate manual testing via UI actions ultimately bridging CLI agents, containers, and interactive frontends.


**Key Highlights:**


- Identified the challenge: agents often write code but can’t test it via UI interactions. His solution? A computer-use MCP server running in a container that supports clicking, typing, dragging, and more.
- Designed a modular system where agents delegate to the computer-use server for UI testing, keeping LLM context clean and extensible.
- Showed how to build each piece: a dev container using Podman, an Anthropic-powered MCP server, and a VS Code environment with Sourcegraph AMP extension culminating in a system where agents write and test code interactively.


**Why it matters:** AI coding agents aren’t complete until they can validate their own output. YK’s demo pushed the boundaries of agent autonomy by turning UI-level testing into an MCP-capable service any agent can use.


Check out YK’s[full session on YouTube](https://youtu.be/5Mnw92JvTyo) and connect with them on[LinkedIn](https://www.linkedin.com/in/anniewangtech) .


### Getting under the hood of the Apollo Community MCP Server – Michael Watson (Apollo)


Michael took us behind the scenes of the Apollo Community MCP Server, showing how it’s deployed, updated, and hot-reloaded without ever dropping a client connection.


**Key Highlights:**


- Explained the architecture: built with Apollo MCP Server, deployed on Railway using Docker and GraphQL with no custom code required.
- Demoed hot-reloading of tools via Apollo REST Connectors: add, modify, or remove GraphQL operations, and they instantly update the exposed tools to your MCP client without restarting the server.
- Walked through a live example of adding a getLastThreeEvents tool, modifying it to remove a field, and deleting it—all with instant reflection in the MCP Inspector UI.


**Why it matters:** Michael showcased a best-in-class developer workflow for tool management—hot reloading, zero downtime, and fine-grained field control. Ideal for fast-moving teams iterating on production-grade agent tooling.


Check out Michael’s[full session on YouTube](https://youtu.be/Z0CkmK_dIy0?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/michael-watson-%F0%9F%96%A5-85844442) .


### Using MCP to Power Durable, Data-Intensive Agent Workflows – Diptanu Gon Choudhury (Tensorlake)


Diptanu shared how MCP enables more than just tool invocation It’s the foundation for scalable, stateful workflows powering real-world agents, like those used in production-grade infrastructure monitoring.


**Key Highlights:**


- Made the case that MCP’s biggest value is standardizing agent-to-tool communication, including streaming, resumability, and secure resource access.
- Presented a **r** eal-world use case: a hedge fund using an agent to detect outages by coordinating logs, alerts, and reporting via long-running MCP workflows.
- Showed how TensorLake’s serverless workflow engine can run durable Python-based tasks, stream updates, and expose them as MCP tools using FastMCP with minimal glue code.


**Why it matters:** A compelling argument that MCP is not just about tools—it’s a protocol for turning agents into reliable, cost-efficient orchestrators of real-world data workflows.


Check out Diptanu’s[full session on YouTube](https://youtu.be/9DHLqqEsKKs?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/diptanu) .


## New York Builder Sessions


### Fast MCP Servers for DevOps – Michael Browning (Postman)


Michael showed how Postman’s visual tool, Flows, can serve as an MCP server to streamline DevOps workflows with LLM-driven automation.


**Key Highlights:**


- Demoed three small MCP servers powered by Postman Flows: one for GitHub Actions monitoring, one for container scaling, and one for NixOS package diffs.
- Explained how flows actions can expose workflows as APIs and MCP-compatible tools, enabling agents like Claude to execute DevOps routines with context propagation.
- Advocated for LLMs to reduce the cognitive load of tool orchestration, while deterministic logic (e.g. guardrails, policy checks) ensures safety in infrastructure mutations.


**Why it matters:** A compelling vision for turning DevOps workflows into conversational, tool-driven automations without sacrificing control or auditability.


Check out Michael’s[full session on YouTube](https://youtu.be/bROBRCJ3pzk) and connect with them on[LinkedIn](https://www.linkedin.com/in/michael-browning-7bb50364) .


### OAuth gets you in, Zero Trust keeps you safe – Nick Taylor (Pomerium)


Nick broke down how to secure MCP servers with zero trust principles using an identity-aware proxy, drawing from real-world practices at Pomerium.


**Key Highlights:**


- Explained zero trust: authenticate and authorize *every* request with a policy engine. This is ideal for agentic and human interactions alike.
- Demoed an MCP server for dev.to, gated with Pomerium’s proxy and policy engine, showcasing per-tool access rules (e.g. support agents can’t mutate data).
- Highlighted the need for security postures that scale beyond VPNs—especially when public AI agents (e.g. Claude, ChatGPT) access your MCP servers.


**Why it matters:** Security is often overlooked in early agentic tooling. This talk grounded MCP development in real-world security infrastructure and practices.


Check out Nick’s[full session on YouTube](https://youtu.be/b42dQgaL734?feature=shared) and connect with them on[LinkedIn](https://ca.linkedin.com/in/nickytonline) .


### MCP tool design best practices with the Apollo Community MCP Server – Amanda Martin (Apollo)


Amanda live-demoed the evolution of the MCP server that powers the Builder Series, showing how connectors and GraphQL tools can create intelligent workflows from REST.


**Key Highlights:**


- Used Apollo Server and REST connectors to compose a GraphQL API from Supabase and Luma event data.
- Taught best practices for MCP tools: clean query names, meaningful field aliases, and minimal context size to avoid LLM confusion.
- Showed hot reloading of a live MCP server showcasing the ability to add new tooling and instantly expose it to your MCP clients
- Showed how adding a new tool (getAllEventsWithSpeakingSessions) let Claude build a tailored speaker abstract using prior session data.


**Why it matters:** A practical and well-scoped demo of how to build and scale a real MCP server with community impact while showing off real-time updates and agent usability.


Check out Amanda’s[full session on YouTube](https://youtu.be/DO9hbf13vRA) and connect with them on[LinkedIn](https://www.linkedin.com/in/amandamartin-dev) .


### Voice is evolving how we interact with computers – Hermes Frangoudis (AgoraIO)


Hermes explored voice-to-voice agent interactions, demonstrating how cascading speech pipelines and real-time infrastructure can bring MCP servers to life.


**Key Highlights:**


- Differentiated voice-to-voice (black-box) vs cascading (modular, streamable) architectures using Agora’s low-latency infrastructure.
- Showcased a live agent that could submit talks and query speaker data using voice—and showed how voice calls can invoke MCP tools.
- Emphasized real-time streaming across STT, LLM, and TTS providers, using Agora’s network to maintain low latency and reliable scaling.


**Why it matters:** Voice-based agentic interaction is becoming real. This talk grounded that future in current tech—and reminded us how fun and human it can feel.


Check out Hermes’s[full session on YouTube](https://youtu.be/MSZMrMAGvq0) and connect with them on[LinkedIn](https://www.linkedin.com/in/hermesfrangoudis) .


**Content Note** : Due to technical difficulties, we were unable to capture the footage from our final NYC speaker this month, Anthony Giuliano from Block. His session on tool layering and the Square MCP Server was a standout. While a replay isn’t available, you can explore similar themes by[watching the June session from Jordan Bergero](https://youtu.be/tA0-xf85FuM) , which also covers the MCP Server at Square and the Goose AI agent.


## What’s Next: Join the MCP Server Builder Community


This was just the beginning. Our mission is to support and grow a community of developers building smarter MCP tools.


**Check out our MCP Server**


Apollo’s own[MCP server is open source](https://github.com/apollographql/apollo-mcp-server) and we have a[full tutorial on Odyssey](https://www.apollographql.com/tutorials/intro-mcp-graphql) that will get you up and running building your own tools.


**Join future events**


We host meetups regularly. Check out our[Luma event page](https://lu.ma/mcp-server) to RSVP for the next one in SF, NYC, or virtually.


**Get involved**


Got something to show? Built a server? Join our talks or share in the[Apollo Community](https://community.apollographql.com/c/mcp-server/41) .


Written by


Amanda Martin


[Read more by Amanda Martin](https://www.apollographql.com/blog/author/amanda)
