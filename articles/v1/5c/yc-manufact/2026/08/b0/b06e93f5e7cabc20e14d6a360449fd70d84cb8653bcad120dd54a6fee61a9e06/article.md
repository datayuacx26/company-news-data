---
schema_version: "1.0.0"
document_id: "b06e93f5e7cabc20e14d6a360449fd70d84cb8653bcad120dd54a6fee61a9e06"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/openapi-mcp-server-with-mcp-use"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-11T20:01:10.944355+00:00"
fetched_at: "2026-08-11T20:01:12.609407+00:00"
content_hash: "sha256:1cae7f81c4dc1193e682c914b22625855625924eacf5016d532ec8368b34b3a1"
---

# Turn an OpenAPI spec into an MCP server

Agents need a way to integrate with your services. An MCP server is the answer to that. It allows any agent to easily connect and authenticate into your service.


Writing an MCP server from scratch means describing every operation, its inputs, and its outputs. If you already maintain an OpenAPI spec, that description exists.[mcp-use](https://github.com/mcp-use/mcp-use) turns it into a working MCP server with one function call.


## From OpenAPI spec to MCP server


` MCPServer


.


fromOpenAPI


()


`


takes a parsed OpenAPI document and registers each operation as an MCP tool that calls the matching HTTP endpoint:


```text
import   {   MCPServer  ,   type   OpenAPIDocument   }   from   "mcp-use"  ;


const   response   =   await   fetch  (  "https://api.weather.gov/openapi.json"  );
const   spec   =   (  await   response  .  json  ())   as   OpenAPIDocument  ;


const   server   =   MCPServer  .  fromOpenAPI  ({
spec  ,
baseUrl  :   "https://api.weather.gov"  ,
name  :   "National Weather Service"  ,
});


export   default   server  ;
```


The spec drives everything the model sees:


- Tool names come from each operation's` operationId


`


- Tool descriptions come from` summary


`


,` description


`


, and the HTTP route
- Input schemas come from parameters and JSON request bodies
- Requests are assembled from the method, path, query string, headers, and body


For more information, check out the[OpenAPI -> MCP docs](https://docs.mcp-use.com/typescript/server/openapi) , which cover the full mapping, including how path, query, and header parameters are flattened into a single tool input.


## Try it: a runnable weather example


The mcp-use repo includes a[complete OpenAPI example](https://github.com/mcp-use/mcp-use/tree/main/libraries/typescript/packages/server/examples/openapi) built on the public National Weather Service API.


When you run the server, you can test it with the built-in inspector: browse the generated tools and call them against live weather data.


## Calling APIs that need authentication


` fromOpenAPI


()


`


accepts an` auth


`


option for bearer tokens and API key headers, plus a` headers


`


option for static headers on every upstream request:


```text
const   server   =   MCPServer  .  fromOpenAPI  ({
spec  ,
baseUrl  :   "https://api.example.com"  ,
auth  :   {
type  :   "bearer"  ,
token  :   process  .  env  .  EXAMPLE_API_TOKEN  ,
},
});
```


These options authenticate your server's requests to the upstream API.


## Treat it as a starting point


A generated server is the fastest way to get an API in front of an agent, and it works well for prototyping, wrapping small internal APIs, and testing whether your operations are useful in an LLM workflow.


For a user-facing server, models perform better with deliberately designed tools: one tool that combines several HTTP calls into a single task, descriptions written for a model rather than a developer, and responses trimmed to the fields the model should reason about. A common path is to bootstrap with` fromOpenAPI


()


`


, watch which tools agents actually use, then hand-write those as focused MCP tools.


The[example](https://github.com/mcp-use/mcp-use/tree/main/libraries/typescript/packages/server/examples/openapi) and the[fromOpenAPI documentation](https://docs.mcp-use.com/typescript/server/openapi) are the places to start.
