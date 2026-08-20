---
schema_version: "1.0.0"
document_id: "33f70a723b1f579f4ebdb639eba530ae79b39f2b2c4cea2f9fda971943b5eedc"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-wiz-is-redefining-partner-velocity-with-apollo-mcp"
published_at: "2025-11-20T11:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:25:11.092082+00:00"
content_hash: "sha256:8f4db0f4cacea3f215c1f74375c40c97c9754e276d76c69a64f65b68215f3765"
---

# How Wiz Is Redefining Partner Velocity with Apollo MCP

## At GraphQL Summit, Wiz shared how the Apollo MCP Server helps bring security and speed together in the IDE


---


Vibe coding, or shipping with AI in the loop, is already here. Developers expect their agents to read the docs, write the call, test it, and move on. Security teams are racing to keep up. At GraphQL Summit 2025, Hen Perez, Field CTO at Wiz, showed how one of the fastest-growing cybersecurity platforms is meeting these realities head-on by integrating MCP and Apollo to keep AI-authored code safe.


## **From Manual Steps to AI-Ready Workflows**


Wiz runs a partner ecosystem with more than 200 certified integrations spanning cloud, identity, and DevOps tools. Wiz’s Integration Network (WIN) follows a clear seven-step process: apply, qualify, sign NDA, access the developer portal, build, certify, and launch joint marketing. Partners across security domains have made it work hundreds of times, but as developer workflows evolve, Wiz saw room to simplify the experience.


“\[Our current process\] is very manual, especially when we think about developers today that are using AI agents as part of their IDEs,” Perez said.


Partners were still doing repeated tasks such as authentication setup, query structure, and payload handling that could be standardized and automated. Perez’s goal was clear: bring the entire integration journey into the IDE with validated tools so agents can move quickly and safely while leveraging their use of Apollo GraphQL.


“All of our APIs are built with Apollo GraphQL. We have a single supergraph that serves both our customers and partners.” – Hen Perez, Field CTO at Wiz


## **MCP: Turning the API Into the Tool**


To achieve this, Wiz built on the Model Context Protocol (MCP), a thin layer that lets AI agents talk directly to services instead of translating documentation into code. The solution uses two MCPs together:


**Wiz MCP**


- Exposes the data from the Wiz platform (issues, vulnerabilities, configurations, inventory).
- The WIN API tool surfaces approved, scalable GraphQL queries and integration guidelines inside each tool description.
- Agents read what matters and call the right thing.


[Apollo MCP Server](https://www.apollographql.com/docs/apollo-mcp-server) ****


- Execute and introspect reveal the actual response structure in the IDE.
- Agents verify authentication and wiring against real payloads.


“All you need to do is combine it with Apollo MCP, which works right out of the box. I haven’t done anything or written a single line of code in the Apollo MCP repository, it just works.” – Hen Perez, Field CTO at Wiz


Together they turn API documentation into executable instructions. Agents gain both the validated query (from Wiz) and the real payload shape (from Apollo), providing complete context and removing guesswork.


With both MCPs connected, an agent can generate a complete integration from a simple prompt, pulling the WIN API specification, inspecting response structures through Apollo MCP, and producing Python scripts with authentication and data fetching. What once required extensive reading and manual testing now happens in minutes inside the IDE.


The generated code follows Wiz’s integration best practices because those guidelines live inside the WIN API tool description. The agent is not improvising; it is using production-ready patterns Wiz has already validated.


## **Fighting AI With AI**


Perez described a simple reality: developers are already building with AI, and attackers are too. The only viable response is to meet both where they are.


“We have to run side by side with developers,” he said. “If the developer is vibe coding, we have to introduce a guardrail. We call it “vibe security.”


Wiz built those guardrails directly into its MCP tools. Security validation happens alongside development, not after it. By grounding AI agents in validated data from Wiz’s platform rather than unpredictable training sources, every suggestion becomes more reliable and secure.


When asked about the risk of data poisoning, Perez pointed to MCP descriptions as the safeguard: by defining what the model reads and how it interprets data, Wiz ensures agents operate within trusted boundaries.


“Because the attackers are using AI, the developers are using AI, everyone is using AI. In order to really have a successful production environment to be secured and successful, you also have to introduce security within that AI.” Hen Perez, Field CTO at Wiz


## **The Transferable Pattern**


Wiz’s approach applies to any platform with API-driven integrations.


- Expose a curated, production-safe subset of your API as an MCP tool with clear descriptions and examples.
- Pair it with Apollo MCP to let agents execute and introspect live queries, binding code to real payloads instead of mock data.
- Put security checks in the same loop so compliance comes with the code.
- Evolve the tools with telemetry and add new capabilities without code changes


As Perez put it, “the beauty here is that you can just take this pattern that we invented and use it for yourself.”


Wiz’s approach turns API documentation into a living interface for agents and developers alike. It combines speed, security, and scalability in a single workflow and offers a glimpse of how modern platforms are evolving in the agentic era.


You can watch Hen Perez’s full session from GraphQL Summit 2025 and see the demo firsthand.


---


### **Watch the Playlist:**[AI in Dev Workflows | GraphQL Summit 2025](https://youtube.com/playlist?list=PLpi1lPB6opQziSvBbu7kORpqs-SpIzfti&si=b1b6xOiMTXXZ_jhE)


*Talks that show how teams are building agent-ready developer experiences.*


Developers expect their agents to read the docs, write the call, test it, and move on. These sessions from GraphQL Summit 2025 show how leading teams are bringing that reality into their day-to-day workflows.


---


Written by


Valeria Gomez


[Read more by Valeria Gomez](https://www.apollographql.com/blog/author/vgomez)
