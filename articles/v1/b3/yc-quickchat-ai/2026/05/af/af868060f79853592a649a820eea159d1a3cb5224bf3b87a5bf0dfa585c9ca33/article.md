---
schema_version: "1.0.0"
document_id: "af868060f79853592a649a820eea159d1a3cb5224bf3b87a5bf0dfa585c9ca33"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/mcp-complete-guide"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:15:23.630573+00:00"
content_hash: "sha256:627630d2081a6cb357fe267b9b32ede785bff0c2b71935b6fb41c1cffcc47776"
---

# The Complete Guide to Model Context Protocol (MCP)

Model Context Protocol (MCP) is an open standard for connecting AI applications to external tools, APIs, and data sources. It was[released by Anthropic in November 2024](https://www.anthropic.com/news/model-context-protocol) and has since been adopted by multiple AI platforms including Claude Desktop, Cursor, Windsurf, and Quickchat AI.


This guide covers MCP from the protocol specification level up through practical server implementation, client integration, security considerations, and production deployment patterns.


## What MCP is (and is not)


MCP is a protocol, not a framework, library, or API. It defines the message format and communication rules that an AI application (client) and a tool provider (server) use to exchange information. The distinction matters:


- **MCP is not an API.** An API is a specific interface to one system. MCP is a standardized way for AI applications to talk to many different APIs through a common interface.
- **MCP does not execute anything.** It defines messages. The server that receives those messages decides what to do with them.
- **MCP is model-agnostic.** It works with any LLM that supports tool calling. The protocol does not care whether the AI behind the client is Claude, GPT-4, Llama, or anything else.


The closest analogy is HTTP. HTTP does not store web pages or render them. It defines how browsers and servers communicate. MCP does the same thing for AI tool use.


For a gentler introduction to what MCP is and why it exists, see our earlier post[How Model Context Protocol works. MCP Explained](https://quickchat.ai/post/mcp-explained) .


## Architecture overview


MCP follows a client-server architecture. The components are:


**Host** : The AI application the user interacts with (e.g., Claude Desktop, Cursor, a chatbot platform). The host manages one or more MCP clients.


**Client** : A component within the host that maintains a connection to a single MCP server. Each client handles one server connection.


**Server** : A program that exposes tools, resources, or prompts to clients. A server connects to one external system (a database, an API, a file system) and makes its capabilities available through MCP.


```text
┌─────────────────────────────────────────┐
│              Host (AI App)              │
│                                         │
│  ┌──────────┐   ┌──────────┐            │
│  │ Client 1 │   │ Client 2 │  ...       │
│  └────┬─────┘   └────┬─────┘            │
│       │              │                  │
└───────┼──────────────┼──────────────────┘
│              │
┌────┴────┐    ┌────┴────┐
│ Server  │    │ Server  │
│ (Slack) │    │ (DB)    │
└─────────┘    └─────────┘
```


A single host can connect to many servers simultaneously. Each server is independent and focused on one integration.


## The MCP message format


MCP uses[JSON-RPC 2.0](https://www.jsonrpc.org/specification) as its wire format. Every message is a JSON object with a specific structure.


### Request


```text
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"method"  :   "tools/call"  ,
"params"  : {
"name"  :   "get_weather"  ,
"arguments"  : {
"location"  :   "London"
}
}
}
```


### Response


```text
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"result"  : {
"content"  : [
{
"type"  :   "text"  ,
"text"  :   "Current weather in London: 12°C, cloudy"
}
]
}
}
```


### Error


```text
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"error"  : {
"code"  :   -32602  ,
"message"  :   "Invalid params: location is required"
}
}
```


JSON-RPC 2.0 was chosen because it is simple, well-specified, and supports both request-response and notification patterns. It also has wide library support across programming languages.


## Protocol lifecycle


An MCP session goes through three phases:


### 1. Initialization


The client sends an` initialize` request with its capabilities and protocol version. The server responds with its own capabilities.


```text
// Client → Server
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"method"  :   "initialize"  ,
"params"  : {
"protocolVersion"  :   "2025-03-26"  ,
"capabilities"  : {
"roots"  : {   "listChanged"  :   true   }
},
"clientInfo"  : {
"name"  :   "my-ai-app"  ,
"version"  :   "1.0.0"
}
}
}


// Server → Client
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"result"  : {
"protocolVersion"  :   "2025-03-26"  ,
"capabilities"  : {
"tools"  : {   "listChanged"  :   true   }
},
"serverInfo"  : {
"name"  :   "weather-server"  ,
"version"  :   "0.1.0"
}
}
}
```


After the server responds, the client sends an` initialized` notification to confirm the handshake is complete.


### 2. Operation


During normal operation, the client can:


- **List tools** (` tools/list` ): Discover what tools the server offers
- **Call tools** (` tools/call` ): Execute a tool with specific arguments
- **List resources** (` resources/list` ): Discover available data resources
- **Read resources** (` resources/read` ): Fetch resource content
- **List prompts** (` prompts/list` ): Discover available prompt templates
- **Get prompts** (` prompts/get` ): Retrieve a specific prompt


The server can also send notifications to the client, for example when the available tools change (` notifications/tools/list_changed` ).


### 3. Shutdown


Either side can close the connection. The client sends a` shutdown` request or simply closes the transport.


## MCP primitives


MCP defines three types of things a server can expose:


### Tools


Tools are functions the AI can call. Each tool has a name, description, and an input schema defined using[JSON Schema](https://json-schema.org/) .


```text
{
"name"  :   "search_contacts"  ,
"description"  :   "Search for contacts in the CRM by email or name"  ,
"inputSchema"  : {
"type"  :   "object"  ,
"properties"  : {
"query"  : {
"type"  :   "string"  ,
"description"  :   "Email address or name to search for"
},
"limit"  : {
"type"  :   "integer"  ,
"description"  :   "Maximum number of results"  ,
"default"  :   10
}
},
"required"  : [  "query"  ]
}
}
```


When the AI decides to use a tool, the client sends a` tools/call` request. The server executes the operation and returns the result.


### Resources


Resources are data that the AI can read. They are identified by URIs and can represent files, database records, API responses, or any other data.


```text
{
"uri"  :   "file:///config/settings.json"  ,
"name"  :   "Application Settings"  ,
"mimeType"  :   "application/json"
}
```


Resources are read-only. If the AI needs to modify data, it uses a tool.


### Prompts


Prompts are reusable prompt templates the server can provide. They are useful for standardizing how the AI interacts with a specific domain.


```text
{
"name"  :   "code_review"  ,
"description"  :   "Review code for bugs and style issues"  ,
"arguments"  : [
{
"name"  :   "code"  ,
"description"  :   "The code to review"  ,
"required"  :   true
}
]
}
```


## Transport layers


MCP supports multiple transport mechanisms for communication between client and server.


### stdio (Standard I/O)


The client launches the server as a subprocess and communicates via stdin/stdout. Each JSON-RPC message is a single line terminated by a newline.


```text
Client                    Server (subprocess)
│                           │
│── stdin: JSON-RPC msg ──→ │
│                           │
│←── stdout: JSON-RPC msg ──│
│                           │
```


stdio is the simplest transport. It requires no network configuration and works locally. Most MCP servers used with Claude Desktop and Cursor use this transport.


**When to use stdio** : Local development, desktop AI apps, single-user scenarios.


### SSE (Server-Sent Events)


The client connects to the server over HTTP. The server uses SSE to push messages to the client, and the client sends messages via HTTP POST.


**When to use SSE** : Remote servers, multi-user scenarios, web-based clients.


### Streamable HTTP


Introduced in the[2025-03-26 protocol revision](https://modelcontextprotocol.io/specification/2025-03-26) , Streamable HTTP is the recommended transport for remote MCP servers. It uses standard HTTP POST requests for client-to-server messages and optionally upgrades to SSE for server-to-client streaming.


The client sends requests to a single endpoint (typically` /mcp` ). The server can respond with either a regular HTTP response or open an SSE stream for ongoing communication.


**When to use Streamable HTTP** : Production remote servers, cloud deployments, cases where you want standard HTTP infrastructure (load balancers, API gateways, auth middleware).


### Transport comparison


Transport Network Complexity Best for


**stdio** Local only Low Desktop apps, development


**SSE** Remote Medium Web clients, legacy setups


**Streamable HTTP** Remote Medium Production remote servers


## Building an MCP server


Here is a minimal MCP server in Python using the official[MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) . This server exposes a single tool that looks up the current time in a given timezone.


### Installation


```text
pip   install   mcp
```


### Server code


```text
from   mcp.server.fastmcp   import   FastMCP
from   datetime   import   datetime
import   pytz


mcp   =   FastMCP(  "timezone-server"  )


@mcp.tool  ()
def   get_current_time  (timezone:   str  ) ->   str  :
"""Get the current time in a specific timezone.


Args:
timezone: IANA timezone name (e.g., 'America/New_York', 'Europe/London')
"""
try  :
tz   =   pytz.timezone(timezone)
now   =   datetime.now(tz)
return   now.strftime(  "%Y-%m-  %d   %H:%M:%S %Z"  )
except   pytz.exceptions.UnknownTimeZoneError:
return   f  "Unknown timezone:   {  timezone  }  "


if   __name__   ==   "__main__"  :
mcp.run(  transport  =  "stdio"  )
```


### What happens when this runs


1. The` FastMCP` class sets up a JSON-RPC server that speaks the MCP protocol.
2. The` @mcp.tool()` decorator registers` get_current_time` as an MCP tool. The SDK automatically generates the JSON Schema for the tool’s input from the function signature and docstring.
3. ` mcp.run(transport="stdio")` starts the server, reading JSON-RPC messages from stdin and writing responses to stdout.


### Testing with MCP Inspector


The[MCP Inspector](https://github.com/modelcontextprotocol/inspector) is a developer tool for testing MCP servers interactively. It connects to a server and lets you list tools, call them, and inspect responses.


```text
npx   @modelcontextprotocol/inspector   python   server.py
```


This opens a web UI where you can browse the server’s tools, fill in parameters, and see the raw JSON-RPC exchange.


## Connecting to Claude Desktop


To use your MCP server with Claude Desktop, add it to the configuration file.


**macOS** :` ~/Library/Application Support/Claude/claude_desktop_config.json`


**Windows** :` %APPDATA%\\Claude\\claude_desktop_config.json`


```text
{
"mcpServers"  : {
"timezone"  : {
"command"  :   "python"  ,
"args"  : [  "/path/to/server.py"  ]
}
}
}
```


After restarting Claude Desktop, the timezone tool will be available. When you ask Claude “What time is it in Tokyo?”, it will call the` get_current_time` tool with` timezone: "Asia/Tokyo"` and include the result in its response.


## Connecting to Cursor


Cursor supports MCP servers through its settings. Add a server configuration in` .cursor/mcp.json` in your project root:


```text
{
"mcpServers"  : {
"timezone"  : {
"command"  :   "python"  ,
"args"  : [  "./server.py"  ]
}
}
}
```


The server’s tools then become available to the AI when you use Cursor’s chat or Composer features.


## Remote MCP servers


Local stdio servers are fine for personal use, but production deployments need remote servers that multiple clients can connect to. Remote MCP servers use HTTP-based transports (SSE or Streamable HTTP) and typically run as web services.


Here is the same timezone server configured for Streamable HTTP:


```text
from   mcp.server.fastmcp   import   FastMCP
from   datetime   import   datetime
import   pytz


mcp   =   FastMCP(  "timezone-server"  )


@mcp.tool  ()
def   get_current_time  (timezone:   str  ) ->   str  :
"""Get the current time in a specific timezone."""
try  :
tz   =   pytz.timezone(timezone)
now   =   datetime.now(tz)
return   now.strftime(  "%Y-%m-  %d   %H:%M:%S %Z"  )
except   pytz.exceptions.UnknownTimeZoneError:
return   f  "Unknown timezone:   {  timezone  }  "


if   __name__   ==   "__main__"  :
mcp.run(  transport  =  "streamable-http"  ,   host  =  "0.0.0.0"  ,   port  =  8000  )
```


Clients connect by sending HTTP POST requests to` http://your-server:8000/mcp` .


### Authentication for remote servers


Remote MCP servers need authentication. The MCP specification does not mandate a specific auth mechanism, leaving it to the transport layer. Common patterns:


Method How it works When to use


**Bearer token** Client sends` Authorization: Bearer <token>` header Simple, widely supported


**OAuth 2.0** Client obtains token via OAuth flow Third-party integrations, user-scoped access


**API key** Client sends key in header or query param Internal services, simple deployments


**mTLS** Mutual TLS certificates High-security environments


For Streamable HTTP servers, authentication can be handled by standard HTTP middleware (e.g., an API gateway or reverse proxy).


## MCP in Quickchat AI


Quickchat AI supports MCP in two directions:


### As an MCP server (exposing your AI Agent)


You can expose your Quickchat AI Agent as an MCP server, allowing other MCP clients (ChatGPT, Claude, Cursor, custom applications) to send messages to your agent and receive responses. This is the pattern shown in our[MCP in Action](https://quickchat.ai/post/mcp-in-action) post.


Every agent’s MCP server is hosted by Quickchat AI at a stable URL, so there is nothing to install or run. To set this up:


1. Go to **External Apps** in your dashboard and open the **MCP** panel.
2. Turn on **Enable MCP** and configure the **Name** , **Description** and **Command** that AI apps will see.
3. Open **Connect MCP** and copy your MCP server URL (` https://app.quickchat.ai/mcp/<your-agent-id>` ).
4. Paste the URL into the AI app as a connector.


Access is private by default: connecting requires signing in with your Quickchat AI account, and you can flip a single toggle to make the server public. Quickchat AI handles the remote transport, authentication, and session management, and MCP is included in every plan (Free included). The full walkthrough is in[Turn Your AI Agent into an MCP Server for ChatGPT, Claude & Cursor](https://quickchat.ai/post/expose-ai-agent-as-mcp-server) .


### As an MCP client (Remote MCP Actions)


Quickchat AI can also connect to external MCP servers as a client. This is done through **Remote MCP Actions** :


1. Go to **Actions** in your AI Agent settings.
2. Click **+ Add Action** and select the MCP action type.
3. Provide the MCP server URL and any required authentication headers.
4. The system discovers available tools from the server and lets you select which ones to enable.


When the AI agent encounters a situation where an MCP tool is relevant, it calls the tool through the Remote MCP connection, receives the result, and incorporates it into its response.


For more on how this compares to HTTP-based actions, see[GPT Actions vs MCP](https://quickchat.ai/post/gpt-actions-vs-mcp) .


## Security considerations


### Tool approval


MCP clients should implement user approval for tool calls. When an AI decides to call a tool that has side effects (creating a record, sending a message, deleting data), the user should see what the tool call will do before it executes. Claude Desktop does this with a confirmation dialog. Custom implementations should follow the same pattern.


### Input validation


MCP servers should validate all inputs from clients. The AI generates the tool arguments, and LLMs can produce unexpected values. Treat tool inputs the same way you would treat untrusted user input: validate types, check ranges, sanitize strings.


### Credential isolation


MCP servers often need credentials to access external systems (API keys, database passwords, OAuth tokens). These credentials should never be exposed to the AI model. The server resolves credentials internally and only returns sanitized results to the client. In Quickchat AI, for example, OAuth tokens for integrations like HubSpot are[injected server-side](https://quickchat.ai/post/connect-ai-agent-to-hubspot) and never appear in the action configuration or AI context.


### Scope limitation


Each MCP server should expose the minimum set of tools needed for its purpose. A server that provides read access to a database should not also expose write operations unless explicitly required. Principle of least privilege applies.


## Debugging and observability


### Logging


The MCP Python SDK supports logging through the standard Python` logging` module. Set the log level to DEBUG to see all JSON-RPC messages:


```text
import   logging
logging.basicConfig(  level  =  logging.  DEBUG  )
```


### MCP Inspector


For interactive debugging, the[MCP Inspector](https://github.com/modelcontextprotocol/inspector) lets you connect to a running server and manually call tools, list resources, and inspect the protocol exchange.


### Common issues


Problem Cause Fix


Server not discovered Config file path wrong or JSON malformed Check config file location and validate JSON


Tool call returns error Invalid arguments from LLM Add better tool descriptions, validate inputs on server


Connection drops Server process crashes Check server logs, add error handling


Slow responses External API latency Add timeouts, consider caching


Tool not appearing Server capabilities not refreshed Restart client or send` notifications/tools/list_changed`


## When to use MCP vs. direct API integration


MCP adds a layer of abstraction. Like any abstraction, it has costs and benefits.


**Use MCP when:**


- Your AI application needs to connect to many external tools
- You want tool integrations to be portable across different AI platforms
- You are building a developer-facing product where users bring their own tools
- You need dynamic tool discovery (tools can change at runtime)


**Use direct API integration when:**


- You are connecting to one or two specific APIs with well-defined needs
- You need very tight control over request construction and error handling
- You are optimizing for minimal latency (every abstraction layer adds some overhead)
- The integration is purely backend-to-backend with no AI in the loop


For a more detailed comparison, see[MCP vs HTTP: When to Use Each](https://quickchat.ai/post/mcp-vs-http) .


## Available MCP servers


The MCP ecosystem has grown rapidly. Directories of pre-built servers:


- [Official MCP Servers Repository](https://github.com/modelcontextprotocol/servers) : Reference implementations maintained by the MCP project
- [Smithery](https://smithery.ai/) : Community directory of MCP servers
- [MCP.so](https://mcp.so/) : Another community-maintained server directory


Popular pre-built servers include integrations for GitHub, Slack, Google Drive, PostgreSQL, Stripe, and many more. These can be used directly or as reference implementations for building your own.


## Further reading


- [MCP Specification](https://modelcontextprotocol.io/specification) : The official protocol spec
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) : Official Python implementation
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk) : Official TypeScript implementation
- [How Model Context Protocol works. MCP Explained](https://quickchat.ai/post/mcp-explained) : Introductory overview
- [MCP in Action](https://quickchat.ai/post/mcp-in-action) : Practical demo with Quickchat AI
- [GPT Actions vs MCP](https://quickchat.ai/post/gpt-actions-vs-mcp) : Comparison with OpenAI’s approach
- [MCP vs HTTP](https://quickchat.ai/post/mcp-vs-http) : When to use each
