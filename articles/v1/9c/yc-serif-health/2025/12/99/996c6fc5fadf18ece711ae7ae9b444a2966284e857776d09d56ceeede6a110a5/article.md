---
schema_version: "1.0.0"
document_id: "996c6fc5fadf18ece711ae7ae9b444a2966284e857776d09d56ceeede6a110a5"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/scaling-mcp-to-production"
published_at: "2025-12-08T23:00:00+00:00"
first_seen_at: "2026-07-24T01:12:39.675945+00:00"
fetched_at: "2026-07-28T22:25:04.749708+00:00"
content_hash: "sha256:2540246ab1b8799a9a4085681cafc994fce3ee53a0dc307f84ae1eaac184b83e"
---

# MCP From Prototype to Production: How We Built Serif Health’s Remotely Hosted and Secure MCP Servers

As we shared in our first post, the[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) (MCP) is opening up new ways for LLMs to work with structured data. For price transparency, where billions of negotiated rates change monthly, it’s a breakthrough. But moving from a local MCP prototype to a secure, scalable, cloud-hosted MCP server is far from trivial.


## A Quick MCP Refresher


MCP is an[open protocol](https://modelcontextprotocol.io/docs) that lets AI models interact with external tools and data sources in real time. Instead of treating an LLM as a static text generator, MCP gives it tools: structured actions that can reach out and interact with new sources of context—like querying an API to retrieve structured data or performing a custom backend operation.


If an API is the engine, MCP is the transmission, providing an LLM with a defined pathway for leveraging its power.


## From Prototype to Production


Most teams begin where we did: with a fast-moving local prototype using the Python[FastMCP](https://gofastmcp.com/getting-started/welcome) template and[STDIO](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#stdio) transport. With Anthropic’s[examples](https://modelcontextprotocol.io/docs/develop/build-server) , you can get Claude Desktop invoking a custom tool in under an hour, and the experience is undeniably magical.


But local MCP ≠ production MCP.


Once you decide to host servers remotely with[Streamable HTTP](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#streamable-http) transport and comply with auth standards, large sections of the prototype will need to be re-designed.


We also built our core API services in Go, so while Python helped us validate the concept quickly, we ultimately rebuilt our MCP servers using the official Go SDK to keep our infrastructure consistent.


What follows are the biggest decisions and tradeoffs we encountered during that transition.


## Stateful or Stateless? A Core Architectural Decision


When you move from a local prototype to a hosted MCP server, you must choose between:


***Stateful MCP servers***


- Maintain session state
- Support server-initiated events (sampling, elicitation)
- Enable richer conversational flows
- Harder to deploy and scale in distributed cloud environments


These advanced features are powerful—elicitation, for example, lets the MCP server call back into an LLM to ask the user for more information. But stateful servers with session logic present hosting and deployment challenges for scalable cloud deployments.


***Stateless MCP servers***


- Behave more like APIs
- Do not depend on session state
- Support core MCP actions: tool invocations, resources, prompts
- Far easier to deploy and scale in cloud environments


For Serif Health, our priority is enabling tool calls into TiC-backed datasets, not server-initiated conversation flows, so we built our MCP servers stateless. Hugging Face has a helpful[blog post](https://huggingface.co/blog/building-hf-mcp) that goes into detail on their choice to go stateless as well.


## Authentication & Authorization: The Greatest Challenge of Production MCP


Authentication is one of the biggest differences between local and remote MCP servers.


MCP for HTTP servers mandates OAuth 2.1 as the long-term standard. But the reality is:


*Many production APIs still rely on API keys.*


And many LLM applications (Claude Desktop/Code, Cursor, Copilot) support passing a key in the Authorization header.


[Stripe](https://docs.stripe.com/mcp?mcp-client=cursor#bearer-token) and[Hugging Face](https://huggingface.co/mcp) both provide this option in their MCP servers.


The most robust approach is to implement the[Dynamic Client Registration](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization#dynamic-client-registration) OAuth flow, which allows for an LLM application such as ChatGPT or Claude to register with an auth provider and create a new client and secret without any user involvement.


We’ve implemented our own custom version of this and both WorkOS and Auth0 have added it to their offerings recently.


So in practice, Serif Health supports:


- API-key-as-bearer-token auth for simplicity and developer friendliness
- [Dynamic client registration](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization#dynamic-client-registration) (OAuth 2.1) for more advanced clients


## Schema Design: The Foundation of Usefulness


MCP’s magic isn’t just that LLMs can call tools, it’s that they can call them correctly.


Clear input/output schemas are what allow a model to:


- Select the right tool parameters
- Understand required fields
- Validate requests
- Anticipate and use predictable structured responses


For this reason, schema design is a crucial aspect of an MCP server implementation.


We use the jsonschema-go builder[library](https://github.com/google/jsonschema-go) to define our tool schemas, which are then registered via the Go SDK’s AddTool[method](https://github.com/modelcontextprotocol/go-sdk/blob/1a907bcd6fddbac1116145f81fa7bb9f76d59b1a/mcp/server.go#L193) . This allows clients to validate requests before they hit our servers—and gives LLMs enough structure to reliably generate correct parameters for complex healthcare queries.


If you’re building an MCP server, treat schema design as a first-class citizen. It’s where most production reliability comes from.


## The Result: Two Production MCP Servers for Price Transparency Data


After several rounds of iteration, we now operate two hosted MCP servers, available for public preview:


[Neuron](https://app.serifhealth.com/docs/apis/neuron/production) ***MCP***


- Payer and hospital price transparency data
- Tools for market-level analytics and rate analysis


[Find Care](https://app.serifhealth.com/docs/apis/find-care/production) ***MCP***


- Provider search by payer, network, procedure, and location
- Powered by TiC + claims utilization data


It’s still early for MCP, and the ecosystem is evolving quickly, but our experience suggests that a well-architected MCP server can provide a powerful, safe way for LLMs to interact with complex healthcare datasets.


## Try It Yourself


If you’re experimenting with MCP or looking to add natural language access to healthcare cost data, we’d love for you to try our servers, now debuting for public preview:


- Neuron MCP — market analytics tools
- Find Care MCP — provider search and rate lookup tools


To get access or learn more about our API suite, reach out athello@serifhealth.com .
