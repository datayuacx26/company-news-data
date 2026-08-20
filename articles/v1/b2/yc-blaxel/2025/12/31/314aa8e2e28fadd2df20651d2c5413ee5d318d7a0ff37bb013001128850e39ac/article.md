---
schema_version: "1.0.0"
document_id: "314aa8e2e28fadd2df20651d2c5413ee5d318d7a0ff37bb013001128850e39ac"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/how-we-optimized-mcp-servers-for-serverless"
published_at: "2025-12-03T13:52:16+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T22:25:05.780563+00:00"
content_hash: "sha256:ff90e88552ea5c61fbe24d8f7a82060c30ca0dc588b0646bdb8413c6763b0e72"
---

# How we optimized MCP servers for serverless (or: From SSE to WebSockets)

Building AI agents that can seamlessly interact with external systems is a complex challenge. At Blaxel, we've been working on creating a platform that fast-tracks the developing of AI agents for developers. Along the way, we've encountered numerous technical hurdles with Model Context Protocol (MCP) servers, particularly around connection stability and scalability in cloud environments.


This article details our journey from using Server-Sent Events (SSE) to WebSockets, and the significant improvements we've seen as a result.


## Understanding MCP: the foundation of modern AI agents


Before diving into our implementation challenges, let's briefly review what MCP is and why it matters.[The Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) is an open standard developed by[Anthropic](https://www.anthropic.com/) that enables AI assistants to connect with external systems where data lives - including content repositories, business tools, and development environments.


MCP consists of three main components:


- **MCP Servers** : These act as bridges connecting APIs, databases, or code to AI models, exposing data sources as tools. At Blaxel, we wanted to provide prebuilt and custom MCP servers for users.
- **MCP Clients** : These use the protocol to interact with MCP servers and can be developed using SDKs in Python or TypeScript.
- **MCP Hosts** : These systems manage communication between servers and clients, ensuring smooth data exchange.


The beauty of MCP is that tools provided by an MCP server can be accessed via any MCP host, allowing developers to connect AI agents to new tools without custom integration code. This standardization is what makes MCP so powerful for building extensible AI systems.


## Our initial approach: HTTP handlers on Cloudflare


When we first started integrating MCP servers into Blaxel, we took what seemed like the most straightforward approach: implementing HTTP handlers on Cloudflare (which was already part of our stack) and mixing MCP with traditional APIs. This worked... sort of.


The problem? Adding a new MCP server was tedious and time-consuming. Each integration required hours of work, which is unsustainable when you're trying to build a platform batteries-included that needs to scale to support thousands of tools and millions of running agents. Furthermore, HTTP isn’t a[standard transport in MCP](https://spec.modelcontextprotocol.io/specification/2024-11-05/basic/transports/) , so there was no official support for it.


## Standardizing MCP integration


To address the tedium of adding new MCP servers, we began looking for standardized ways to register them. Two key resources were kept as a result of our search:


[Smithery](https://smithery.ai/) : A registry of MCP servers that provides a standardized packaging format


[MCP Hub](https://github.com/blaxel-ai/mcp-hub) : Our own open-source catalog of MCP servers designed to accelerate integration.


This infrastructure helped us organize our growing collection of MCP servers, but we still faced significant technical challenges with the underlying communication protocol.


## SSE: a promising start with disappointing results


During our search for better solutions, we discovered[Supergateway](https://github.com/supercorp-ai/supergateway) , a tool that wraps stdio-based MCP servers with Server-Sent Events (SSE). On paper, this looked like an elegant solution.


For those unfamiliar with SSE, it's a technology that establishes a one-way communication channel from server to client over HTTP. Unlike WebSockets, which provide full-duplex communication, SSE is designed specifically for server-to-client updates. This makes it seemingly ideal for scenarios where clients primarily need to receive updates from servers.


We implemented Supergateway with SSE, but quickly ran into significant issues:


### What are the problems with SSE in serverless environments


- **Connection instability** : In serverless environments, SSE connections dropped randomly and frequently. This is particularly problematic for AI agents that need reliable, persistent connections to function properly.
- **Scaling challenges** : As we tried to scale our platform, the limitations of SSE became increasingly apparent. The protocol wasn't designed with cloud-native architectures in mind.
- **Browser connection limits** : SSE suffers from a limitation to the maximum number of open connections, which is set to a very low number (6) per browser and domain. This became problematic when users opened multiple tabs.
- **Proxy and firewall issues** : Some proxies and firewalls block SSE connections because they don't have a Content-Length header, creating deployment challenges in enterprise environments.


After extensive testing, we concluded that while SSE might work well for simpler use cases or controlled environments, it wasn't robust enough for our cloud-based AI agent platform.


## WebSockets: a game-changer for MCP


Facing these challenges, we made the decision to switch from SSE to WebSockets for all our MCP server communications. Despite the fact that MCP documentation doesn't extensively discuss WebSockets, they are officially supported - and as we discovered, they work significantly better in cloud environments.


### Why WebSockets outperform SSE for MCP servers


WebSockets establish a persistent, full-duplex TCP connection between client and server, allowing for bidirectional communication. This architecture offers several advantages over SSE for MCP servers:


- **Connection stability** : WebSockets maintain more stable connections, with built-in mechanisms for handling disconnections and reconnections.
- **Bidirectional communication** : While MCP often doesn't require extensive client-to-server communication, having the capability for bidirectional data flow eliminates the need for separate HTTP requests for client-initiated actions.
- **Binary data support** : WebSockets can transmit both binary data and UTF-8 text, whereas SSE is limited to UTF-8. This provides more flexibility for different types of data exchange.
- **Better performance** : WebSockets typically offer lower latency and overhead compared to SSE, especially for frequent communications.
- **No connection limits** : WebSockets don't suffer from the same browser connection limits as SSE, making them more suitable for applications where users might have multiple tabs open.


### Forking Supergateway


To implement our WebSocket solution, we forked the Supergateway project and modified it to use WebSockets instead of SSE. The core changes involved:


- **Protocol adaptation** : Modifying the communication layer to use WebSocket protocol instead of HTTP streaming.
- **Connection management** : Implementing robust connection handling with automatic reconnection logic.
- **Error handling** : Enhancing error detection and recovery mechanisms to ensure reliable operation in cloud environments.
- **Scaling optimizations** : Adding features to better support horizontal scaling across multiple instances.


Our modified version of Supergateway is available on GitHub as[Blaxel's Supergateway](https://github.com/blaxel-ai/supergateway) , and we welcome contributions and feedback from the community!


## Technical implementation: WebSockets for MCP


For those interested in the technical details, here's how we implemented WebSockets for our MCP servers. Please note that the entire code can be found in open-source on our GitHub on[Blaxel's Supergateway](https://github.com/blaxel-ai/supergateway) and[Blaxel’s SDK](https://github.com/blaxel-ai/toolkit/blob/main/sdk-ts/src/functions/mcp.ts) .


### Protocol bridging without code changes


The cornerstone of our solution is a protocol bridge that allows any stdio-based MCP server to communicate over WebSockets without modification:


typescript


` const


stdioToWebSocket


=


async


(


stdioCmd


:


string


,


port


:


number


)


=>


{


// Spawn the stdio-based MCP server as a child process


child


=


spawn


(


stdioCmd


,


{


shell


:


true


}


)


;


// Create WebSocket server transport


wsTransport


=


new


WebSocketServerTransport


(


port


)


;


// Bidirectional communication between WebSocket and stdio


wsTransport


.


onmessage


=


(


msg


)


=>


{


child


!


.


stdin


.


write


(


JSON


.


stringify


(


msg


)


+


'\\\\n'


)


;


}


;


child


.


stdout


.


on


(


'data'


,


(


chunk


)


=>


{


// Parse and forward JSON messages to WebSocket clients


}


)


;


}


`


This architecture allows us to leverage the existing ecosystem of MCP servers while gaining all the benefits of WebSockets.


### Solving the connection stability problem


One of the most frustrating issues with SSE was random connection drops in cloud environments. Our WebSocket implementation addresses this with several key mechanisms:


**Managing persistent connections** : Unlike SSE connections that would frequently drop, our WebSocket implementation maintains long-lived connections:


typescript


` // Server tracks all client connections


private


clients


:


Map


<


string


,


WebSocket


>


=


new


Map


(


)


;


// Client implements reconnection logic


async


start


(


)


:


Promise


<


void


>


{


let


attempts


=


0


;


while


(


attempts


<


MAX_RETRIES


)


{


try


{


await


this


.


_connect


(


)


;


return


;


}


catch


(


error


)


{


attempts


++


;


await


delay


(


RETRY_DELAY_MS


)


;


}


}


}


`


**Proactively handling dead connection** : Rather than waiting for timeouts, we actively detect and clean up dead connections:


typescript


` // During message sending, we check and remove dead connections


for


(


const


\[


id


,


client


\]


of


this


.


clients


.


entries


(


)


)


{


if


(


client


.


readyState


!==


WebSocket


.


OPEN


)


{


deadClients


.


push


(


id


)


;


}


}


`


This approach dramatically reduced the "connection dropped randomly" issues that plagued our SSE implementation, resulting in far fewer interruptions to AI agent operations.


### Addressing cloud scaling challenges


The SSE implementation hit severe scaling limitations in cloud environments. Our WebSocket solution includes architectural decisions specifically designed for cloud scalability:


**Allowing for multi-tenancy** : We handle a specific ID converted from the message ID to allow for multi-tenancy. Messages are routed back only to the specific client that requested them.


typescript


` async


send


(


msg


:


JSONRPCMessage


,


clientId


?


:


string


)


:


Promise


<


void


>


{


const


\[


cId


,


msgId


\]


=


clientId


?.


split


(


":"


)


??


\[


\]


;


// Route message to specific client


if


(


cId


)


{


const


client


=


this


.


clients


.


get


(


cId


)


;


if


(


client


?.


readyState


===


WebSocket


.


OPEN


)


{


client


.


send


(


data


)


;


}


}


}


`


**Health monitoring** : We implemented a dedicated health check endpoint that cloud orchestration systems can use to monitor service health:


typescript


` app


.


get


(


"/health"


,


(


req


,


res


)


=>


{


if


(


child


?.


killed


)


{


res


.


status


(


500


)


.


send


(


"Child process has been killed"


)


;


}


if


(


!


isReady


)


{


res


.


status


(


500


)


.


send


(


"Server is not ready"


)


;


}


else


{


res


.


send


(


"OK"


)


;


}


}


)


;


`


### Seamless integration with AI frameworks


To make our WebSocket-based MCP servers easily usable by AI agents, we created a clean integration layer that converts MCP tools into formats compatible with popular AI frameworks:


typescript


` export


function


getMCPTool


(


client


:


MCPClient


,


name


:


string


,


description


:


string


,


schema


:


z


.


ZodType


)


{


return


tool


(


async


(


args


:


any


)


=>


{


const


result


=


await


client


.


callTool


(


name


,


args


)


;


return


JSON


.


stringify


(


result


.


content


)


;


}


,


{


name


,


description


,


schema


,


}


)


;


}


`


This integration allows AI agents to use MCP tools as if they were native tools, regardless of whether they're communicating over WebSockets or another protocol. The transport layer becomes completely transparent to the agent implementation.


## Lessons learned (and best practices)


Through our journey from SSE to WebSockets for MCP servers, we've gathered several key insights that might benefit others working with MCP:


- **Protocol selection matters** : Choose your communication protocol based on your deployment environment. While SSE might work well for simple, controlled environments, WebSockets offer superior performance and reliability in cloud deployments.
- **Connection management is critical** : Implement robust connection management with automatic reconnection logic. AI agents need stable, persistent connections to function properly.
- **Standardize your integrations** : Use tools like[Smithery](https://smithery.ai/) (or check out[Blaxel MCP Hub](https://github.com/blaxel-ai/mcp-hub) for a good source of inspiration!) to standardize how you register and manage MCP servers. This reduces integration time and maintenance overhead.
- **Monitor connection health** : Implement comprehensive monitoring for your MCP server connections. Track metrics like connection stability, latency, and error rates to identify issues early.
- **Consider scale from the start** : Design your MCP server architecture with scale in mind. WebSockets provide better scaling characteristics for cloud deployments, but they still require careful implementation to handle large numbers of concurrent connections.


## Future directions


While our switch to WebSockets has significantly improved our MCP server performance, we're continuing to explore ways to enhance our implementation:


- **Load balancing** : We're developing more sophisticated load balancing strategies for distributing MCP server connections across multiple instances.
- **Protocol optimizations** : We're exploring ways to optimize the MCP protocol over WebSockets, including message batching and compression techniques to further reduce latency and bandwidth usage.
- **Standardized authentication** : We're working on standardized authentication mechanisms for MCP servers over WebSockets to ensure secure access to sensitive tools and data.


We're excited to continue refining our WebSocket-based MCP server implementation and contributing to the broader MCP ecosystem. If you're interested in learning more or contributing to our open-source efforts, check out our[MCP Hub](https://github.com/blaxel-ai/mcp-hub) and[Supergateway fork](https://github.com/blaxel-ai/supergateway) .


*This article is based on our real-world experience building[Blaxel](https://blaxel.ai/) , a computing infrastructure for agentic AI. The technical details and performance metrics reflect our actual implementation. We hope our experiences help others in the AI agent development community build more with MCP server implementations.*
