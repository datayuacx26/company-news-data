---
schema_version: "1.0.0"
document_id: "287a9484a297b79cfbdf5cba92fb068a054d624917ec31ff046d959ea269a6a7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-a-secure-ai-agent-api-stack-with-graphql-apollo-skills-and-mcp-server"
published_at: "2026-03-05T09:00:06+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:374d6d0607d884afa5c165fa35c05179c613b718bfe3fc10620ac4d027434c6f"
---

# Building a Secure AI Agent API Stack with GraphQL, Apollo Skills, and MCP Server

AI agents need access to your APIs. The question is how to provide that access without creating a maintenance burden, security risk, or pile of low-quality generated code.


Most teams start with the obvious approach: wrap existing REST endpoints and expose them to agents. This works initially. Then the problems emerge. Agents construct malformed requests. They call endpoints in inefficient sequences. They generate code that works but ignores your team’s conventions. Each agent session requires the same corrections, and the corrections do not persist.


The underlying issue is architectural. REST wrappers give agents access to your APIs but provide no structure for how to use them well. Agents lack three things: a typed contract they can validate against, knowledge of your patterns and conventions, and controlled capabilities with security boundaries.


GraphQL, Apollo Skills, and Apollo MCP Server address these three gaps as complementary layers of a complete agent-API stack.


## **The Three-Layer Architecture for Secure AI Agent Access to APIs**


Each layer addresses a distinct concern. Understanding what each layer does (and does not do) helps you decide where to invest.


**Layer** **What it provides** **Problem it solves**


GraphQL Typed schema, validation, precise data fetching Agents construct invalid requests; responses contain unnecessary data


Apollo Skills Reusable knowledge modules for AI coding tools Agents generate code that works but ignores best practices


Apollo MCP Server Controlled runtime capabilities with security boundaries Agents need structured access to execute operations safely


These layers are independent. You can adopt one without the others. But they compound when used together.


## **Layer 1: GraphQL as the data contract for AI agents**


REST APIs describe endpoints. GraphQL describes data.


This distinction matters for agents. When an agent constructs a REST request, correctness is verified at runtime. A typo in a field name, a missing parameter, a malformed payload: these surface only when the request executes. Agents iterate through trial and error, making API calls until something works.


GraphQL moves validation earlier. The schema defines every type, field, and argument. Agents can check an operation against the schema before execution. Invalid field? The schema says so. Wrong argument type? Caught before any data is fetched.


```text
# Agent constructs this query
query GetUser($id: ID!) {
user(id: $id) {
name
orders { items { name } }  # Error: 'name' doesn't exist on OrderItem
}
}


# Schema validation returns:
# "Cannot query field 'name' on type 'OrderItem'. Did you mean 'productName'?"
```


The agent can fix the query and try again without consuming API resources on failed requests.


GraphQL also solves the over-fetching problem. REST endpoints return fixed response shapes. An agent wanting user data and order history makes two requests, receives fields it does not need, and correlates the results in code. GraphQL composes both in a single request, returning exactly the requested fields. Smaller payloads mean fewer tokens consumed in agent context windows and less post-processing code.


**When to adopt this layer:** You have REST APIs that agents will query repeatedly. You want validation before execution. You want to reduce payload sizes and agent-side data manipulation.


## **Layer 2: Apollo Skills as the knowledge layer for improving AI-generated GraphQL code**


GraphQL ensures agents can construct valid operations. It does not ensure they construct good ones.


Consider this query:


```text
{
user(id: "123") {
name
email
}
}
```


It works. It is anonymous, making it harder to identify in server logs and debugging tools, uses a hardcoded ID (not reusable), and omits the id field (preventing cache normalization in Apollo Client). An experienced developer would write:


```text
query GetUser($id: ID!) {
user(id: $id) {
id
name
email
}
}
```


Named operation. Variables. Includes id for caching. These details compound across a codebase.


Apollo Skills encode this knowledge as reusable modules for AI coding tools. When an agent writes GraphQL, the` graphql-operations` skill guides it toward named queries, proper variables, and cache-friendly field selection. When an agent designs a schema, the` graphql-schema` skill enforces intentional nullability and proper list patterns (` \[Item!\]!` instead of` \[Item\]` ).


Skills work with Claude Code, Cursor, GitHub Copilot, Windsurf, and other AI coding tools. Install with:


```text
npx skills add apollographql/skills
```


The CLI detects your tools and configures them automatically. Skills activate based on context, or you can invoke them directly (e.g.,` /graphql-schema` in Claude Code).


**When to adopt this layer:** Your team uses AI coding tools. You want generated code to follow your conventions without re-explaining them every session. You have standards for schema design, operation structure, or Apollo product usage that you want agents to follow.


## **Layer 3: Apollo MCP Server as the capability layer for secure AI agent API execution**


Skills teach agents how to write good GraphQL. MCP Server gives them the ability to execute it.


Apollo MCP Server exposes GraphQL operations as MCP tools that agents can call. You control which operations are available through three configuration patterns:


**Pre-defined operations:** You write` .graphql` files. Each becomes an MCP tool with a typed input schema. Agents execute only what you have explicitly approved.


**Persisted queries:** If you use Apollo GraphOS, agents can execute operations from your persisted query manifest. Safelisting is automatic.


**Dynamic introspection:** Agents explore your schema and construct operations at runtime. Flexibility increases, but so does the need for security controls. Apollo Router’s demand control feature calculates operation cost and rejects expensive queries before execution.


Most teams combine patterns: pre-defined operations for mutations (explicit control over writes) and introspection for queries (flexibility for reads).


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


**When to adopt this layer:** You want agents to execute GraphQL operations, not just generate code. You need security boundaries around what agents can do. You want to control mutation access separately from query access.


## **How the layers work together**


A typical workflow using all three layers:


1. Developer asks an AI coding tool to add a feature that fetches user order history
2. **Skills layer:** The` graphql-operations` skill activates, guiding the agent to use named queries, variables, and include id fields
3. **MCP layer:** The agent uses MCP Server’s search tool to find relevant types (` User.orders` ,` Order.status` )
4. **GraphQL layer:** The agent constructs an operation and uses MCP Server’s validate tool to check it against the schema
5. **MCP layer:** The agent executes the validated operation through MCP Server’s execute tool
6. **Skills layer:** The agent writes client code following Apollo Client conventions from the` apollo-client` skill


Each layer contributes something the others cannot provide. GraphQL provides the typed contract. Skills provide knowledge. MCP Server provides the controlled execution.


## **Choosing your entry point**


Not every team needs all three layers immediately. Start where your pain is sharpest.


**If agents generate working but low-quality code:** Start with Skills. Installation takes one command, requires no infrastructure changes, and improves every AI coding session immediately.


**If agents make repeated invalid API requests:** Start with GraphQL. The validation benefits apply whether you adopt Skills or MCP Server later.


**If you need agents to execute operations in production:** Start with MCP Server. Begin with pre-defined operations (Pattern 1) for explicit control, then expand to introspection if needed.


The layers compose incrementally. You can adopt Skills today, add MCP Server next quarter, and migrate to GraphQL over time. Each layer delivers value independently while enabling the next.


## **Next steps**


To go deeper on any layer:


**GraphQL:** See[Every Token Counts: Building Efficient AI Agents with GraphQL and Apollo MCP Server](https://www.apollographql.com/blog/building-efficient-ai-agents-with-graphql-and-apollo-mcp-server) for detailed MCP Server configuration


**Skills:** See[Apollo Skills: Teaching AI Agents How to Use Apollo and GraphQL](https://www.apollographql.com/blog/apollo-skills-teaching-ai-agents-how-to-use-apollo-and-graphql) for the full skill catalog and installation guide


**MCP Server:** Explore the[Apollo MCP Server documentation](https://www.apollographql.com/docs/apollo-mcp-server) for configuration reference


Apollo MCP Server and Apollo Skills are both open source. Try them in a single project before rolling out to your team.


Written by


Camille Lawrence


[Read more by Camille Lawrence](https://www.apollographql.com/blog/author/clawrence)
