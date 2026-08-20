---
schema_version: "1.0.0"
document_id: "a9cb0ba35c9a6f7141141885c6e98f0b97c0ad535a422cb860a24c84c34aaa52"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-build-ai-agents-using-your-graphql-schema"
published_at: "2026-03-12T10:00:55+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:99bd2ec18c2d04db758cac518ec2a81cf0db4ec1d5c6372411395412c9f3af11"
---

# How to Build AI Agents Using Your GraphQL Schema

AI agents are evolving at a remarkable pace. In just the past year, they’ve gone from experimental demos to production tools capable of planning and executing multi-step workflows. But for most engineering teams, the real question isn’t whether agents are powerful. It’s how to connect them to the applications and data you already have.


If your team already maintains a mature GraphQL API, you’re closer than you think. GraphQL schemas describe your domain in a declarative, structured way, making them a natural fit for AI agents that need context, clear boundaries, and predictable data contracts. The challenge has been bridging the gap between GraphQL’s query language and the natural language reasoning of large language models (LLMs).


[Apollo MCP Server](https://www.apollographql.com/docs/graphos/mcp) solves this by treating your GraphQL schema as a first-class tool in the agent’s environment, without requiring any changes to your existing backend.


In this tutorial, we’ll walk through why GraphQL is uniquely suited for AI agent integration, how the Model Context Protocol (MCP) standardizes agent-to-tool communication, and exactly what it looks like to connect an AI agent to a live GraphQL API using Apollo MCP Server. You’ll leave with a working setup and a clear path to extending it against your own schema.


## **Why GraphQL and MCP Work Well Together**


GraphQL’s strongly-typed schema and built-in introspection give AI agents something most APIs can’t: a complete, machine-readable map of your domain that they can query at runtime. Instead of guessing at endpoint behavior or relying on documentation that may be out of date, agents get deterministic specifications for every operation: what inputs are required, what shape the response takes, and what relationships exist between types.


The[Model Context Protocol (MCP)](https://modelcontextprotocol.io/) is what makes this accessible to AI applications. It standardizes how agents discover and invoke external tools, so any MCP-compatible AI system can connect to any MCP server without custom integration code. Apollo MCP Server implements this standard for GraphQL, translating your existing operations into callable tools while handling schema validation, protocol translation, and secure execution in between. Your API stays unchanged; agents just get a new way in.


If you want a deeper look at how GraphQL and MCP fit together architecturally, see[How to Orchestrate Multiple APIs for Your LLM Agent](https://apollographql.com/how-to-orchestrate-multiple-apis-for-your-llm-agent/) . For schema design best practices that make your API more effective for AI agents, see[How To Make Your Existing GraphQL API AI-Ready With Apollo](https://apollographql.com/how-to-make-your-existing-graphql-api-ai-ready-with-apollo/) . This article focuses on the implementation: here’s how to build it.


## **Tutorial: Connecting an AI Agent to a GraphQL API**


To make this concrete, we’ll walk through a full setup using a public GraphQL API that provides information about countries worldwide:[https://countries.trevorblades.com/graphql](https://countries.trevorblades.com/graphql) .


We’ll run Apollo MCP Server as a Docker container that exposes GraphQL operations as MCP tools, then connect it to Claude Code (Anthropic’s command-line AI coding tool) so we can query the API using natural language.


**Here’s our roadmap:**


1. Introspect the GraphQL schema to understand available data structures
2. Create a schema file defining the types and queries
3. Define GraphQL operations that become AI-callable tools
4. Configure Apollo MCP Server with the endpoint, schema, and operations
5. Run the MCP server as a Docker container on port 5000
6. Register the server with Claude Code so it can discover the tools
7. Test the integration by asking natural language questions that trigger tool calls


**Prerequisites:** To follow along, you’ll need[Docker](https://docs.docker.com/get-docker/) , the[Claude Code CLI](https://docs.anthropic.com/claude-code) , and a local project folder (e.g.,` mkdir ~/countries-mcp` ). Alternatively, clone the[GitHub repository](https://github.com/apollographql/apollo-mcp-server) containing all code used in this walkthrough.


### **Step 1: Introspect the Schema**


GraphQL’s built-in introspection system is what makes it such a natural fit for AI agents. The API tells you exactly what’s possible. Running an introspection query against the endpoint returns a complete representation of the type system:


```text
query IntrospectionQuery {
__schema {
queryType { name }
mutationType { name }
types {
name
kind
fields {
name
type { name kind }
}
}
}
}
```


This returns a JSON representation of all types, queries, fields, and relationships in the Countries API, essentially a machine-readable map of your domain that both Apollo MCP Server and your AI agent can reason against.


```text
{
"data": {
"__schema": {
"queryType": {
"name": "Query"
},
"mutationType": null,
"types": [
{
"name": "Boolean",
"kind": "SCALAR",
"fields": null
},
{
"name": "Continent",
"kind": "OBJECT",
"fields": [
{
"name": "code",
"type": {
"name": null,
"kind": "NON_NULL"
}
},
{
"name": "countries",
"type": {
"name": null,
"kind": "NON_NULL"
}
},
{
"name": "name",
"type": {
"name": null,
"kind": "NON_NULL"
}
}
]
},
…
```


### **Step 2: Create the Schema File**


Based on the introspection result, create an` api.graphql` file that defines the schema in GraphQL SDL. Several tools can assist with this:[build_client_schema](https://graphql-core-3.readthedocs.io/en/latest/modules/utilities.html#graphql.utilities.build_client_schema) from the[graphql-core](https://github.com/graphql-python/graphql-core) Python library can construct a schema from an introspection result, or you can use[Apollo Studio Sandbox](https://studio.apollographql.com/sandbox) to point at any GraphQL API and copy the schema definition directly.


Save the resulting file as` ~/countries-mcp/api.graphql` . It will define the core types (` Continent` ,` Country` ,` Language` ,` State` ,` Subdivision` ) along with filter inputs and the root` Query` type:


```text
type Continent {
code: ID!
name: String!
countries: [Country!]!
}


type Country {
awsRegion: String!
capital: String
code: ID!
continent: Continent!
currencies: [String!]!
currency: String
emoji: String!
emojiU: String!
languages: [Language!]!
name: String!
native: String!
phone: String!
phones: [String!]!
states: [State!]!
subdivisions: [Subdivision!]!
}


type Language {
code: ID!
countries: [Country!]!
name: String!
native: String!
rtl: Boolean!
}


type State {
code: String
country: Country!
name: String!
}


type Subdivision {
code: String!
emoji: String
name: String!
}


input ContinentFilterInput {
code: StringQueryOperatorInput
}


input CountryFilterInput {
code: StringQueryOperatorInput
continent: StringQueryOperatorInput
currency: StringQueryOperatorInput
}


input LanguageFilterInput {
code: StringQueryOperatorInput
}


input StringQueryOperatorInput {
eq: String
ne: String
in: [String]
nin: [String]
regex: String
glob: String
}


type Query {
continent(code: ID!): Continent
continents(filter: ContinentFilterInput): [Continent!]!
countries(filter: CountryFilterInput): [Country!]!
country(code: ID!): Country
language(code: ID!): Language
languages(filter: LanguageFilterInput): [Language!]!
}
```


Apollo MCP Server uses this schema to understand the available data structures and how they relate to each other, forming the basis for tool generation.


### **Step 3: Define GraphQL Operations as MCP Tools**


Each GraphQL operation you define becomes a distinct, callable MCP tool that AI agents can discover and invoke. Create an` operations` subfolder in your project:


```text
~/countries-mcp$ mkdir operations
```


For example, to give agents the ability to look up countries by continent, create` operations/GetCountriesByContinent.graphql` :


```text
query GetCountriesByContinent($continentCode: String!) {
countries(filter: { continent: { eq: $continentCode } }) {
code
name
native
capital
emoji
currency
continent {
code
name
}
languages {
code
name
native
}
}
}
```


Or a name-based search in` operations/SearchCountriesByName.graphql:`


```text
query SearchCountriesByName($searchTerm: String!) {
countries(filter: { name: { regex: $searchTerm } }) {
code
name
native
capital
emoji
currency
continent {
code
name
}
languages {
code
name
native
}
}
}
```


Each operation’s structure (parameters, return fields, and name) becomes part of the tool description that the agent reasons about when deciding which tool to call.


### **Step 4: Configure Apollo MCP Server**


The` config.yaml` file tells Apollo MCP Server where everything lives and how agents should connect:


```text
endpoint: https://countries.trevorblades.com/graphql
transport:
type: streamable_http
port: 5000
operations:
source: local
paths:
- ./operations
schema:
source: local
path: ./api.graphql
```


This configuration connects the dots: the GraphQL endpoint to query, the local schema file for validation, the operations folder defining which tools to expose, and the transport configuration (streamable HTTP on port 5000) for MCP clients to connect over.


At this point, your project structure should look like this:


```text
~/countries-mcp$ tree
.
├── api.graphql
├── config.yaml
└── operations
├── GetAllContinents.graphql
├── GetAllCountries.graphql
├── GetAllLanguages.graphql
├── GetContinentByCode.graphql
├── GetCountriesByContinent.graphql
├── GetCountriesByCurrency.graphql
├── GetCountryByCode.graphql
├── GetLanguageByCode.graphql
└── SearchCountriesByName.graphql
```


### **Step 5: Run Apollo MCP Server with Docker**


With all files in place, start Apollo MCP Server as a Docker container, following the instructions found[here](https://www.apollographql.com/docs/apollo-mcp-server/run#with-docker) :


```text
$ docker run   \
-it \
--rm \
--name apollo-mcp-server \
-p 5000:5000 \
-v $HOME/countries-mcp/config.yaml:/config.yaml \
-v $HOME/countries-mcp:/data \
--pull always \
ghcr.io/apollographql/apollo-mcp-server:latest \
/config.yaml
```


Once running, you’ll see:


```text
INFO Starting MCP server in Streamable HTTP mode port=5000 address=0.0.0.0
```


Apollo MCP Server is now live, exposing your GraphQL operations as discoverable MCP tools on port 5000.


### **Step 6: Register the MCP Server with Claude Code**


In a separate terminal, connect Claude Code to the running MCP server:


```text
~$ claude mcp add --transport http countries http://localhost:5000/mcp


Added HTTP MCP server countries with URL: http://localhost:5000/mcp to local config
File modified: $HOME/.claude.json [project: $HOME/countries-mcp]
```


This registers the server with Claude Code, making all the country data tools available in your terminal sessions.


### **Step 7: Query Your GraphQL API with Natural Language**


You can now interact with the GraphQL API using natural language. To ensure Claude uses the MCP tools rather than searching the web directly, instruct it not to use its built-in WebSearch tool:


```text
Claude Code v2.0.1
Sonnet 4.5 · Claude Pro


> What MCP servers do you have registered?


● I have the countries MCP server registered. This server provides access to country, continent, and language data through the following functions:


- GetAllCountries - Get all countries with details
- GetCountryByCode - Get a specific country by its code
- SearchCountriesByName - Search for countries by name
- GetCountriesByContinent - Get countries in a specific continent
- GetCountriesByCurrency - Get countries using a specific currency
- GetAllContinents - Get all continents
- GetContinentByCode - Get a specific continent by code
- GetAllLanguages - Get all languages
- GetLanguageByCode - Get a specific language by code


> For this session, I don't want you to use WebSearch.


● Understood. I won't use the WebSearch during this session.
```


Now ask a question that requires real data:


```text
> What countries are in North America that start with the letter C?


● countries - GetCountriesByContinent (MCP)(continentCode: "NA")


╭───────────────────────────────────────────────────────────────────────╮
│ Tool use                                                              │
│                                                                       │
│   countries - GetCountriesByContinent(continentCode: "NA") (MCP)      │
│   The returned value is an array of type `Country`                    │
│   ---                                                                 │
│   type Continent {                                                    │
│     code: ID!                                                         │
│     name: String!                                                     │
│   }                                                                   │
│                                                                       │
│   type Country {                                                      │
│     capital: String                                                   │
│     code: ID!                                                         │
│     continent: Continent!                                             │
│     currency: String                                                  │
│     emoji: String!                                                    │
│     languages: [Language!]!                                           │
│     name: String!                                                     │
│     native: String!                                                   │
│   }                                                                   │
│                                                                       │
│   type Language {                                                     │
│     code: ID!                                                         │
│     name: String!                                                     │
│     native: String!                                                   │
│   }                                                                   │
│                                                                       │
│                                                                       │
│ Do you want to proceed?                                               │
│ > 1. Yes                                                              │
│   2. Yes, and don't ask again for countries - GetCountriesByContinent │
│      commands in /home/alvin/repositories/claude                      │
│   3. No, and tell Claude what to do differently (esc)                 │
│                                                                       │
╰───────────────────────────────────────────────────────────────────────╯


● countries - GetCountriesByContinent (MCP)(continentCode: "NA")
⎿  {
"data": {
"countries": [
… +832 lines (ctrl+o to expand)


● The North American countries starting with "C" are:


- Canada 🇨🇦
- Costa Rica 🇨🇷
- Cuba 🇨🇺
- Curacao 🇨🇼
- Cayman Islands 🇰🇾


> What is the national language of Trinidad and Tabago?


● countries - SearchCountriesByName (MCP)(searchTerm: "Trinidad and Tobago")


│ Tool use                                                              │
│                                                                       │
│   countries - SearchCountriesByName(searchTerm: "Trinidad and         │
│   Tobago") (MCP)                                                      │
│   The returned value is an array of type `Country`                    │
│   ---                                                                 │
│   type Continent {                                                    │
│     code: ID!                                                         │
│     name: String!                                                     │
│   }                                                                   │
│                                                                       │
│   type Country {                                                      │
│     capital: String                                                   │
│     code: ID!                                                         │
│     continent: Continent!                                             │
│     currency: String                                                  │
│     emoji: String!                                                    │
│     languages: [Language!]!                                           │
│     name: String!                                                     │
│     native: String!                                                   │
│   }                                                                   │
│                                                                       │
│   type Language {                                                     │
│     code: ID!                                                         │
│     name: String!                                                     │
│     native: String!                                                   │
│   }                                                                   │
│                                                                       │
│                                                                       │
│ Do you want to proceed?                                               │
│ > 1. Yes                                                              │
│   2. Yes, and don't ask again for countries - SearchCountriesByName   │
│   commands in /home/alvin/repositories/claude                         │
│   3. No, and tell Claude what to do differently (esc)                 │
│                                                                       │


● countries - SearchCountriesByName (MCP)(searchTerm: "Trinidad and Tobago")
⎿  {
"data": {
"countries": [
… +22 lines (ctrl+o to expand)


● The national language of Trinidad and Tobago is English.
```


Success!


## **What Just Happened**


When you asked about Trinidad and Tobago’s national language, Claude Code:


1. Identified the need for external data and checked available MCP tools
2. Selected` SearchCountriesByName` based on its tool description
3. Invoked the tool with the correct parameter (` searchTerm: "Trinidad and Tobago"` )
4. Apollo MCP Server validated the query against the schema, executed it against the GraphQL endpoint, and returned structured data via the MCP protocol
5. Claude synthesized a natural language answer from the JSON response


This architecture keeps concerns cleanly separated. Your GraphQL API remains unchanged. No special agent endpoints, no custom resolvers. The AI agent focuses on reasoning and natural language understanding. Apollo MCP Server sits between them, handling protocol translation, validation, and secure execution.


## **Production Deployment Patterns**


Apollo MCP Server integrates with your existing GraphQL infrastructure, so deployment follows familiar patterns.


**Local development with Rover CLI.** For rapid iteration, use rover dev –mcp to run Apollo MCP Server alongside your local graph in a single process. This makes it easy to test operations and debug agent behavior during development without additional configuration.


**Docker containers.** Apollo provides Docker images that work in any containerized environment. Mount your configuration and operations as volumes and the container handles the rest, giving you consistent, reproducible builds across development, staging, and production.


**Apollo Runtime Container.** For production, the Apollo Runtime Container runs the Apollo Router and MCP Server together. Agent traffic flows through the same routing, caching, and federation infrastructure as your application traffic, ensuring consistent behavior and unified observability.


**Serverless environments.** Agents running in serverless functions (AWS Lambda, Vercel, Cloudflare Workers) can connect to Apollo MCP Server via its HTTP transport. The stateless MCP protocol makes this straightforward: your function makes HTTP requests to the MCP endpoint just like any other API call.


## **Observability and Security**


When Apollo MCP Server runs within the Apollo Runtime Container alongside Apollo Router, agent queries flow through the same infrastructure as application traffic and surface in Apollo Studio. This gives you visibility into agent query patterns, performance metrics, and error rates through the same monitoring tools you already use, with no separate observability stack required.


Security follows your existing GraphQL model. Apollo MCP Server can be configured to pass authentication tokens and headers to your GraphQL endpoint, ensuring that field-level authorization rules, rate limiting policies, and query complexity analysis apply to agent requests exactly as they do for application traffic. Agents operate as controlled clients governed by your existing security policies.


## **Performance and Scaling**


Agent workloads can differ from typical application patterns. Agents may issue multiple queries in quick succession as they reason through complex tasks. Apollo MCP Server handles this through several mechanisms:


- **Query batching** : When agents make multiple independent queries, Apollo MCP Server can batch them into a single request to your GraphQL endpoint, reducing round-trip overhead.
- **Persisted queries** : Frequently-used agent patterns can be persisted, reducing network payload and improving cache hit rates. This is especially valuable when multiple agents request similar data across different sessions.
- **Response caching** : Your existing Apollo caching configuration applies automatically. If an agent requests data that’s already cached, it gets the cached response, preventing redundant API calls.
- **Horizontal scaling** : As agent usage grows, add more Apollo MCP Server instances behind a load balancer. The stateless design makes this straightforward, each instance maintains its own schema cache, and requests can be routed to any available instance.


## **Moving Forward**


AI agents are moving from experimental features to production infrastructure. For teams with existing GraphQL APIs, the path forward doesn’t require rebuilding anything. Your schema already contains the structured knowledge agents need: types, relationships, operations, and constraints.


Apollo MCP Server makes that knowledge accessible through the Model Context Protocol, automatically converting GraphQL operations into tools that any MCP-compatible AI agent can discover and use. Schema introspection is automatic. Query validation is transparent. Observability integrates with your existing monitoring. Security follows your established policies.


The result is an architecture where agents interact with your data in a controlled, observable way while your GraphQL API continues serving applications unchanged. Human users and AI agents operate through the same infrastructure, governed by the same rules.


If you’re already invested in GraphQL, you’re well-positioned to make your API work for the next generation of AI-powered applications.


Written by


Kaitlyn Barnard


[Read more by Kaitlyn Barnard](https://www.apollographql.com/blog/author/kbarnard)
