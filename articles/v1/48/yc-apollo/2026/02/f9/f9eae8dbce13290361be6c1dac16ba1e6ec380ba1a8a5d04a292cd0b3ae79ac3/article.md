---
schema_version: "1.0.0"
document_id: "f9eae8dbce13290361be6c1dac16ba1e6ec380ba1a8a5d04a292cd0b3ae79ac3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/connect-ai-agents-to-your-graphql-api-using-mcp-and-type-safe-tool-configuration"
published_at: "2026-02-26T12:00:45+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:fe9f000a734f686afb9685651a9db4bd5611708c63b33dfca787c5d4df79a71d"
---

# Connect AI Agents to Your GraphQL API Using MCP and Type-Safe Tool Configuration

***Prerequisites:** This post assumes familiarity with GraphQL (queries, mutations, schemas) and the basics of MCP (what tools are, how agents call them). If you’re looking for a simpler starting point, see the[Apollo MCP Server quickstart guide](https://www.apollographql.com/docs/apollo-mcp-server/quickstart) .*


Connecting AI agents to APIs creates a configuration problem: how do you control which operations agents can execute, validate their inputs before runtime, and enforce security boundaries? REST wrappers leave these concerns to application code. GraphQL’s type system handles them at the configuration layer.


Apollo MCP Server provides three patterns for exposing GraphQL operations as MCP tools. The simplest pattern works with any GraphQL API, no Apollo infrastructure required. We recommend starting with Pattern 1 and adding complexity only when you need it.


## **Pattern 1: Pre-defined operations from files as MCP tools**


**Works with any GraphQL API. No GraphOS required. Start here.**


```text
# apollo-mcp-server.yaml
graphql:
endpoint: https://your-api.example.com/graphql
operations:
source: local
paths:
- ./operations/*.graphql
```


Write your operations:


```text
# operations/GetUserOrders.graphql
query GetUserOrders($userId: ID!, $limit: Int = 10) {
user(id: $userId) {
name
orders(first: $limit) {
id
total
createdAt
}
}
}
```


**Required and optional parameters are determined automatically** from GraphQL’s type system:


- Non-null types (` ID!` ,` String!` ) become required parameters
- Nullable types (` ID` ,` String` ) become optional parameters
- Variables with default values (` $limit: Int = 10` ) are optional with the specified default


**When to use this pattern:**


- You want explicit control over every operation agents can execute
- You’re working with mutations or operations with significant side effects
- You’re using any GraphQL API, regardless of infrastructure


**The tradeoff:** You must define every operation upfront. For large APIs, this means maintaining many .graphql files. Adding a new capability requires writing, testing, and redeploying.


## **Pattern 2: Persisted queries from Apollo GraphOS as MCP tools**


If you’re already using Apollo GraphOS, you can expose persisted queries as MCP tools. The server fetches operations from GraphOS and converts each into a tool.


```text
# apollo-mcp-server.yaml
operations:
source: uplink
```


**This provides safelisting by default.** Apollo Router only accepts operations from the manifest, preventing arbitrary query execution.


**When to use this pattern:**


- Your organization already uses GraphOS with persisted queries
- Safelisting is a hard security requirement
- You want agent capabilities to stay in sync with deployed client applications


**The tradeoff:** Requires[Apollo GraphOS](https://www.apollographql.com/graphos) infrastructure. Operations must be published before agents can use them.


## **Pattern 3: Dynamic operations via MCP introspection tools**


Instead of pre-defining operations, you give agents tools to explore the schema and construct operations at runtime.


Apollo MCP Server provides four introspection tools:


**Tool** **Purpose**


` introspect` Navigate schema structure from root types


` search` Find types and fields by semantic search


` validate` Check an operation against the schema before running


` execute` Run any valid GraphQL operation


**When to use this pattern:**


- Your schema is self-documenting with descriptive field names
- You cannot predict which operations agents will need
- You want to reduce the overhead of maintaining operation files


**The tradeoff:** Agents can construct arbitrary queries. This flexibility requires security configuration (covered below).


## **Security: Controlling query cost and mutation access**


**GraphQL’s flexibility creates risks when agents construct queries dynamically.** Apollo Router’s demand control feature addresses this by assigning a cost to each field and type in an operation, rejecting operations that exceed a configured threshold. You can tune cost calculations with the` @cost` directive on expensive fields or types and the` @listSize` directive on list fields — particularly important when agents can’t predict result set sizes.


### **Mutation mode options**


For mutations, Apollo MCP Server controls what agents can execute through a mutation mode setting:


**Mode** **Behavior**


` none` (default) Block all mutations via the execute tool


` explicit` Allow only pre-defined mutations, block agent-constructed ones


` all` Allow agents to construct and execute mutations


## **Combining MCP configuration patterns**


A common configuration uses pre-defined operations for writes and introspection for reads:


```text
# apollo-mcp-server.yaml
operations:
source: local
paths:
- ./mutations/*.graphql
introspection:
execute:
enabled: true
search:
enabled: true
overrides:
mutation_mode: explicit
```


This gives you flexible reads with controlled writes. Agents can explore the schema and run ad-hoc queries, but mutations require explicit approval through pre-defined operations.


## **Why GraphQL for agent APIs**


If agents write code to call APIs, why does the data layer matter?


**Type-safe validation before execution.** When an agent calls REST endpoints, type errors surface at runtime. With GraphQL, the validate tool checks operations against the schema before any data is fetched:


```text
const validation = await validate({
operation: `query { user(id: "123") { orders { items { name } } } }`
});
// Returns: "Cannot query field 'name' on type 'OrderItem'."
//          "Did you mean 'productName'?"
```


Agents can iterate on query construction without making costly API calls for each attempt.


**Precise data fetching.** REST endpoints return fixed response shapes. An agent wanting user orders must fetch` /users/123` and` /users/123/orders` separately, then correlate results. GraphQL composes these in a single request, returning exactly the fields requested. Smaller payloads mean fewer tokens consumed and less post-processing code.


**Schema discovery via semantic search.** The search tool finds relevant types and fields:


```text
search({ terms: ["user", "order", "history"] })
User.orders(first: Int, after: String): OrderConnection
Order.createdAt: DateTime
Order.status: OrderStatus
```


Agents get a structured path from intent to operation without reading endpoint documentation.


**Agents that follow best practices.** When agents construct GraphQL operations, they make choices: anonymous vs. named queries, inline values vs. variables, which fields to request. Without guidance, agents default to patterns that work but miss optimizations. Apollo Skills encodes these decisions as reusable knowledge modules. An agent with the` graphql-operations` skill uses named` operations` (visible in server logs),` variables` (enabling query reuse), and includes id fields (enabling cache normalization). These details compound across a codebase. If you’re using Claude Code, Cursor, or GitHub Copilot, install Skills with` npx skills add apollographql/skills` .


## **A note on MCP maturity**


MCP is early. The protocol continues to evolve, and today’s patterns may change as the ecosystem matures. We’ve designed Apollo MCP Server with this in mind:


- Pattern 1 has no framework lock-in. It’s configuration over your existing GraphQL API.
- The server is open source, so you can fork and adapt as needed.
- We’re tracking protocol changes and will document migration paths.


If you’re evaluating whether to invest in MCP tooling, Pattern 1 offers a low-commitment starting point.


## **For AI coding tool users: Apollo Skills**


MCP Server gives agents the **capability** to interact with your GraphQL API. Apollo Skills teaches them **how** to do it well.


Skills are reusable knowledge modules for AI agents, comparable to plugins or npm packages but for AI assistants instead of code. When an agent writes a GraphQL schema, the` graphql-schema` skill guides it toward intentional nullability and proper list patterns` (\[Item!\]!` instead of` \[Item\])` . When configuring MCP Server, the` apollo-mcp-server` skill provides step-by-step workflows and common pitfalls to avoid.


```text
# Install all Apollo skills
npx skills add apollographql/skills


# Or install specific skills
npx skills add apollographql/skills@apollo-mcp-server
npx skills add apollographql/skills@graphql-operations
```


The CLI detects which tools you’re using (Claude Code, Cursor, GitHub Copilot, and others) and configures them automatically. Once installed, skills activate automatically based on context—for example, when you mention “MCP server configuration” or “GraphQL schema design” in your request, Claude will automatically load the relevant skill.


Skills and MCP Server address different layers of the same problem:


**Layer** **What it provides** **Example**


Skills Knowledge and patterns “Use cursor-based pagination for large lists”


MCP Server Runtime capabilities “Execute this query and return the first 10 results”


Together, they create a complete workflow: Skills ensure agents write good GraphQL, and MCP Server ensures they write working GraphQL.


## **Summary**


Apollo MCP Server provides three configuration patterns:


**1. Pre-defined operations** (recommended starting point): Explicit control, works with any GraphQL API


**2. Persisted queries:** Leverage existing GraphOS infrastructure and safelisting


**3. Dynamic introspection:** Flexibility with explicit security configuration


GraphQL’s type system provides guarantees about data shape, query validity, and response structure that agents can rely on. Start with Pattern 1, add complexity when you need it.


Apollo MCP Server is open source. Explore the[Apollo MCP Server documentation](https://www.apollographql.com/docs/apollo-mcp-server) for configuration details, or clone the[GitHub repository](https://github.com/apollographql/apollo-mcp-server) to try it locally.


Written by


Camille Lawrence


[Read more by Camille Lawrence](https://www.apollographql.com/blog/author/clawrence)
