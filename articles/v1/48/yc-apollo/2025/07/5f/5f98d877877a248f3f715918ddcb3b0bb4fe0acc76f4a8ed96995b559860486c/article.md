---
schema_version: "1.0.0"
document_id: "5f98d877877a248f3f715918ddcb3b0bb4fe0acc76f4a8ed96995b559860486c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/meet-the-builders-highlights-from-the-mcp-server-builder-meetup"
published_at: "2025-07-08T10:55:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:bc8a2d3aa89ef657876e5ce23582b3e789a1876ef2bd825a2a873e85dbcf0e8e"
---

# Meet the Builders: Highlights from the MCP Server Builder Meetup

On June 18th, we hosted our very first MCP Server Builder Meetup in San Francisco, bringing together engineers, tinkerers, and early adopters to explore the future of building AI-native developer experiences with Model Context Protocol (MCP).


This was more than just a showcase of cool demos. It marked the launch of a new community of developers building for and with LLMs using MCP servers. The energy in the room made one thing clear: this community is ready to build what’s next.


Keep reading for a recap of all the demos with links to the full sessions. Our next events in the series are on July 29th in NYC and July 31st in San Francisco. We hope to see you there. RSVP and subscribe in our[Luma calendar](https://lu.ma/mcp-server) .


## Jordan Bergero (Block): Building Reliable MCP Servers with Goose & MCP Tool Layering in Square MCP Server


Jordan shared how his team turned an internal hack week project into a production-grade MCP server at Square, which now powers real developer workflows.


**Key Highlights:**


- Introduced Goose: an open source, LLM-agnostic tool for running and testing MCP servers locally in a GUI or terminal experience.
- Explained Square’s “layered approach” (discover → plan → execute) to reduce API confusion. This structured layering helps make results predictable and reproducible.
- Goose read a raw .txt file of invoice notes, parsed out customer and payment data, called multiple Square API endpoints (customer create, order create, invoice publish), and emailed real invoice all from a single prompt.


**Why it matters:** An impressive real-world example of turning API chaos into structured tool behavior.


Check out Jordan’s[full session on YouTube](https://youtu.be/tA0-xf85FuM?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/jordan-bergero) .


## Melissa Herrera (Langflow): Langflow as a Visual MCP Client and Server


Melissa brought the energy and delivered a lightning-fast tour of Langflow, a visual IDE for building agent workflows with drag-and-drop ease.


**Key Highlights:**


- MCP Client & Server: Langflow can act as both an MCP client (calling tools via agent workflows) and a server (exposing your flows as tools), letting you chain, compose, and republish.
- Multi-agent architecture: She showcased a resume enhancer app that parsed a resume, queried live job market data via Tavily, and returned improvements all orchestrated across multiple agents.
- Tool Reuse: Demonstrated turning any Langflow component into a reusable tool and exporting it as part of a server bundle.


**Why it matters:** For devs or visual thinkers, Langflow is a developer-friendly launchpad for building composable, LLM-native tools.


Check out Melissa’s[full session on YouTube](https://youtu.be/EOXPaLF8_gw?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/herrera-melissa) .


## Lizzie Siegle (Cloudflare): Podcast Generator with Workers + MCP


Lizzie brought some joy (and a surprise) with her talk on building and deploying fun, voice-powered MCP apps on Cloudflare Workers.


**Key Highlights:**


- Serverless podcast generator: Combined Claude, Workers AI, and a Cloudflare D1 SQL database to build a podcast generator that outputs both audio and script content.
- End-to-end deployment: Demonstrated how to go from zero to a deployed MCP server using Cloudflare’s click-to-deploy button. Most code was auto-generated, including durable object support.
- Tool listing & persistence: Saved generated podcast metadata and audio URLs to SQL, and exposed a tool to query prior results..


**Why it matters:** Cloudflare’s infra stack is well-suited for lightweight AI applications. If you want a fast way to host, persist, and serve LLM workflows globally, this is a great blueprint.


Check out Lizzie’s[full session on YouTube](https://www.youtube.com/watch?v=vjgHr5temTM&list=PLpi1lPB6opQyLjI99abvDXZ-OsWbO4Yt4&index=4) and connect with them on[LinkedIn](https://www.linkedin.com/in/elsiegle/) .


## Tobin South (WorkOS): Securing MCP Servers


Tobin delivered a high-impact talk focused on the security foundations of MCP infrastructure.


**Key Highlights:**


- Highlighted common authentication pitfalls in community-deployed MCP servers and the growing need for OAuth and SSO.
- Gave a crash course in OAuth and SSO for MCP, including how to support dynamic client registration properly.
- Demoed a secure, OAuth-powered MCP server for ordering custom swag.


**Why it matters:** If you’re shipping MCP servers to users or enterprises, this talk is your must-watch.


Check out Tobins[full session on YouTube](https://youtu.be/Zk3V0QE9Uho?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/tobinsouth) .


## Michael Watson (Apollo): Token-Efficient MCP Servers with GraphQL


Watson closed the night with a deep dive into how token efficiency and schema-first tooling can make LLM interactions smarter and cheaper.


**Key Highlights:**


- Token bloat audit: Analyzed GitHub’s MCP server responses which produced 8.5k+ tokens per tool call caused by duplicate and irrelevant fields.
- Selective GraphQL queries: Used GraphQL selection sets to omit unneeded data, reducing token usage by 75% to about 2,000 tokens.
- Hot-reloadable tools: Showed how the[Apollo MCP server](https://www.apollographql.com/docs/apollo-mcp-server) can load tools directly from .graphql files with no build step needed.
- Live demo: Used Goose to introspect GitHub’s GraphQL schema and auto-generate a working tool in minutes, then hot-loaded it into the server.


**Why it matters:** GraphQL-first design gives you clean, typed, and cost-efficient tooling which is a superpower for scaling MCP use.


Check out Watson’s[full session on YouTube](https://youtu.be/_7J1A4IXh-s?feature=shared) and connect with them on[LinkedIn](https://www.linkedin.com/in/michael-watson-%F0%9F%96%A5-85844442) .


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
