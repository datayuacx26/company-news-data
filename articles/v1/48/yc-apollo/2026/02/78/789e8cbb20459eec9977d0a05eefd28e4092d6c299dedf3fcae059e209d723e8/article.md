---
schema_version: "1.0.0"
document_id: "789e8cbb20459eec9977d0a05eefd28e4092d6c299dedf3fcae059e209d723e8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-skills-teaching-ai-agents-how-to-use-apollo-and-graphql"
published_at: "2026-02-03T12:00:35+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:22:19.644306+00:00"
content_hash: "sha256:5702df8dc51bf37f715244c8939f2c6dfafab6ccbfbe12668619590d3dd4b2da"
---

# Apollo Skills: Teaching AI Agents How to Use Apollo and GraphQL

You’ve been here before: you ask an AI agent to add a GraphQL query, and it generates code using patterns from 2019. You correct it. Next session, same mistake. The agent is powerful but has no memory, and no way to learn your team’s conventions.


What if you could teach it once and have that knowledge persist everywhere?


## **Why We Built Apollo Skills**


When[skills.sh](https://skills.sh/) launched as an open ecosystem for sharing AI agent expertise across the AI community, we saw an opportunity. As the company behind GraphQL tooling used by thousands of developers, we asked: what if AI agents could learn Apollo’s patterns and best practices directly from us?


Instead of writing a proposal or roadmap, we took the shortest path to validating the idea: ship a real public artifact, see if people use it, and iterate based on real feedback.


That question led us to build[apollographql/skills](https://github.com/apollographql/skills) .


## **The Problem: AI Agents Are Powerful but Forgetful**


If you’ve used Claude Code, Cursor, or GitHub Copilot, you’ve experienced both the magic and the frustration. These tools can write code, refactor systems, and even debug complex issues. But they have a knowledge problem.


**AI agents require lots of repeated prompts.** Every new conversation starts from zero. You find yourself re-explaining the same patterns: “GraphQL can return partial data with errors—check both data and error from useQuery, not just one.” “Use Apollo Client’s` useQuery` and` useMutation` hooks, not the old HOC pattern.” “The Connectors syntax uses` $this.userId` instead of` $args.userId` when the value comes from the parent type, not the query arguments”


**Knowledge about how to use tools is scattered.** Best practices live in documentation, blog posts, GitHub issues, and institutional knowledge. An AI agent might know *about* Apollo Server, but it doesn’t necessarily know *how* Apollo recommends you use it.


**It’s hard to make AI consistently follow best practices.** Without explicit guidance, agents make reasonable but suboptimal choices. They might use deprecated patterns, skip error handling, or miss performance optimizations that an experienced developer would catch.


This isn’t just anecdotal.[Studies show](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report) AI-co-authored pull requests have significantly higher defect rates, and[research into automated GraphQL generation](https://research.ibm.com/publications/graphql-query-generation-a-large-training-and-benchmarking-dataset) describes it as “a hard problem” that models consistently struggle with.


The result? Developers become prompt engineers, manually injecting the same context into every session. It works, but it doesn’t scale.


Here’s a concrete example. Ask an AI agent without Skills to “define a User type with posts,” and you might get:


```text
# Without skills: nullable by default


type User {
id: ID
name: String
email: String
posts: [Post]
}
```


With the` graphql-schema` skill activated, the same request produces:


```text
# With skills: intentional nullability, list pattern


type User {
id: ID!
name: String!
email: String
posts: [Post!]!
}
```


GraphQL fields are nullable by default, which helps with error resilience. When one field fails, the error doesn’t cascade up the tree. However, being too permissive with nullability shifts burden to clients. The` \[Post!\]!` pattern (a non-null list of non-null items) tells clients exactly what to expect. Leaving` email` nullable is intentional: it signals that some users may not have one. The skill helps agents think through these tradeoffs rather than defaulting to nullable everywhere.


The same principle applies to operations. Ask an agent to “fetch a user by ID,” and you might get:


```text
# Without skills: anonymous query, hardcoded value


{
user(id: "123") {
name
email
}
}
```


With the graphql-operations skill:


```text
# With skills: named operation, variables, includes id for caching


query GetUser($id: ID!) {
user(id: $id) {
id
name
email
}
}
```


Named operations show up in server logs and performance traces. Variables enable query reuse and prevent injection. Including id allows Apollo Client to cache and update the result automatically. Small details, big impact. These are exactly the kind of patterns that Skills encode.


Let’s take a look at a short demo showing Skills activating during generation in a real workflow:


## **What Are Agent Skills?**


[Agent Skills](https://agentskills.io/) are an open standard that solves this problem, and[skills.sh](https://skills.sh/) is an ecosystem for discovering and distributing reusable Skills. Think of Skills as **procedural knowledge modules for AI agents** —comparable to plugins or npm packages, but for AI assistants instead of code. Skills turn general-purpose agents into specialists when you need them.


A Skill is a directory containing a` SKILL.md` file plus optional folders for references, scripts, and assets. It includes:


- **Trigger conditions** : When should the agent activate this knowledge?
- **Step-by-step processes** : What workflow should the agent follow?
- **Best practices** : What patterns should it use (and avoid)?
- **Reference material** : Where can it find deeper context?


Skills load on demand, following a three-phase model:


1. **Discovery** : The agent sees a brief description (~100 tokens) and decides if the Skill is relevant
2. **Activation** : If relevant, it loads the full` SKILL.md` instructions (<5000 tokens)
3. **Execution** : Reference files, scripts, and assets load only when needed


This means agents don’t pay a context tax for knowledge they aren’t using. The Skill system acts like a just-in-time knowledge loader.


## **Enter Apollo Skills**


We built[apollographql/skills](https://github.com/apollographql/skills) —A curated collection of Agent Skills for Apollo and GraphQL development. Here’s what’s currently available:


### **Product Skills**


**Skill** **What It Teaches**


` apollo-client` How to use Apollo Client for React, including queries, mutations, caching, and state management


` apollo-server` How to set up Apollo Server, write resolvers, handle authentication, configure plugins, and manage subscription


` apollo-connectors` How to integrate REST APIs using` @source` and` @connect` directives


` apollo-mcp-server` How to configure MCP Server for AI agent integration with Apollo tools


` rover` How to use Rover CLI for schema management, federation, and local development


### **Convention Skills**


**Skill** **What It Teaches**


` graphql-schema` Schema design patterns: types, naming, pagination, errors, security


` graphql-operations` Query and mutation patterns, fragments, variables, error handling


` rust-best-practices` Idiomatic Rust coding based on Apollo’s internal handbook


### **Meta Skills**


**Skill** **What It Teaches**


` skill-creator` How to create new skills for this repository (more on this below)


## **Installing Skills**


If you’re using AI coding tools, getting started takes one command:


```text
# Install all Apollo skills
npx skills add apollographql/skills


# Or install a specific skill
npx skills add apollographql/skills@apollo-client
```


Running the first command lets you browse and select from all available Skills interactively:


```text
$ npx skills add apollographql/skills


███████╗██╗  ██╗██╗██╗     ██╗     ███████╗
██╔════╝██║ ██╔╝██║██║     ██║     ██╔════╝
███████╗█████╔╝ ██║██║     ██║     ███████╗
╚════██║██╔═██╗ ██║██║     ██║     ╚════██║
███████║██║  ██╗██║███████╗███████╗███████║
╚══════╝╚═╝  ╚═╝╚═╝╚══════╝╚══════╝╚══════╝


┌   skills
│
◇  Source: https://github.com/apollographql/skills.git
│
◇  Repository cloned
│
◇  Found 9 skills
│
◆  Select skills to install (space to toggle)
│  ◼ apollo-client (Guide for building React applications with Apollo Client ...)
│  ◼ apollo-connectors (Help users write Apollo Connectors schemas to integrate R...)
│  ◼ apollo-mcp-server (Guide for using Apollo MCP Server to connect AI agents wi...)
│  ◼ apollo-server (Guide for building GraphQL servers with Apollo Server 4.x...)
│  ◼ graphql-operations (Guide for writing GraphQL operations (queries, mutations,...)
│  ◼ graphql-schema (Guide for designing GraphQL schemas following industry be...)
│  ◼ rover (Guide for using Apollo Rover CLI to manage GraphQL schema...)
│  ◼ rust-best-practices (Guide for writing idiomatic Rust code based on Apollo Gra...)
│  ◼ skill-creator (Guide for creating effective skills for Apollo GraphQL an...)
└
```


During installation, the CLI asks which AI coding agents you’re using (with support for over 26 tools including Claude Code, Cursor, GitHub Copilot, and Windsurf) and automatically configures each one to load the skills. Once set up, skills activate automatically based on context, though you can also invoke them manually:


- **Claude Code** : Use commands like` /graphql-schema` or` /apollo-client`
- **Cursor** : Reference Skills in chat or Composer
- **GitHub Copilot** : Skills apply during code suggestions and chat


## **What a Skill Looks Like**


Here’s a simplified view of how the` apollo-connectors` skill is structured:


```text
apollo-connectors/
├── SKILL.md              # Main instructions
└── references/
├── grammar.md        # @source/@connect syntax
├── methods.md        # HTTP method patterns
├── variables.md      # Selection mapping
├── entities.md       # Federation entities
├── validation.md     # Schema validation
└── troubleshooting.md
```


The` SKILL.md` file starts with YAML frontmatter that tells the agent when to activate:


```text
---
name: apollo-connectors
description: >
Help users integrate REST APIs with GraphQL using Apollo Connectors.
Use this skill when: (1) adding @source or @connect directives to a schema,
(2) mapping REST endpoints to GraphQL types,
(3) troubleshooting connector validation errors.
license: MIT
metadata:
author: apollographql
version: "1.0.1"
---
```


Notice how the description includes explicit trigger conditions. This helps the agent decide whether to activate the Skill based on what the developer is asking.


The body includes a step-by-step process that guides the agent through a consistent workflow:


```text
## Process


Follow this workflow when implementing connectors:


- [ ] **Research**: Read the existing schema and identify REST endpoints to integrate
- [ ] **Implement**: Add `@source` for base URLs and `@connect` for field mappings
- [ ] **Validate**: Run `rover dev` to check for composition errors
- [ ] **Execute**: Test queries in Apollo Sandbox
- [ ] **Test**: Verify response mapping and error handling
```


Finally, ground rules encode the best practices that experienced developers know but AI agents often miss:


```text
## Ground Rules


- ALWAYS define `@source` before using `@connect` on fields
- NEVER hardcode credentials in connector URLs; use headers with environment variables
- PREFER `$this` over `$args` when the data already exists on the parent type
- USE selection mapping syntax (`id: $.user_id`) for JSON-to-GraphQL field mapping
```


## **The Meta Moment: Using a Skill to Create Skills**


After creating several Skills, we recognized that the skill-creation process itself could be encoded as a Skill. This is an emerging pattern in the community: using skills to bootstrap the creation of new skills. The result is` skill-creator` , a Skill that teaches AI agents how to create Skills for Apollo and GraphQL.


Now when adding a new Skill, there’s no need to start from scratch. Simply tell the agent: “Create a new Skill for Apollo Federation.” The` skill-creator` skill activates and guides through:


1. Setting up the directory structure
2. Writing proper frontmatter with trigger conditions
3. Following Apollo’s voice and style guidelines
4. Organizing reference files by topic
5. Adding ground rules in the correct format


The agent creates a well-structured skill draft that follows all conventions—ready for review and merge.


This is the promise of Agent Skills in action: **encoding expertise once, then reusing it infinitely across any AI agent** .


## **Skills and MCP: How They Work Together**


Agent Skills and MCP (Model Context Protocol) serve different but complementary roles in AI-assisted development.


**Skills provide knowledge.** They teach AI agents *how* to approach problems: patterns to follow, conventions to respect, pitfalls to avoid. When an agent activates the` graphql-operations` skill, it learns how to structure queries, handle errors, and use fragments effectively. This knowledge is static: it doesn’t change based on the specific API being used.


**MCP provides capabilities.**[Apollo MCP Server](https://www.apollographql.com/docs/apollo-mcp-server) gives AI agents the ability to *actually interact* with GraphQL APIs—introspecting schemas, validating operations, and executing queries against live endpoints. This is dynamic: the agent works with real data and real schemas.


Together, they create a complete workflow:


**Layer** **What It Provides** **Example**


Skills Knowledge and patterns “Use cursor-based pagination for large lists”


MCP Runtime capabilities “Execute this query and return the first 10 results”


Consider a developer asking an AI agent to “add a query for fetching user orders.” Here’s how Skills and MCP work together:


1. The` graphql-schema` skill activates, guiding naming conventions and type design
2. The` graphql-operations` skill provides query structure best practices
3. MCP Server’s` search` tool finds relevant types in the schema (User, Order)
4. MCP Server’s` validate` tool checks the query against the actual schema
5. MCP Server’s` execute` tool tests the query against the endpoint


Skills ensure the agent writes *good* GraphQL. MCP ensures it writes *working* GraphQL.


We even have a Skill for this: The` apollo-mcp-server` skill teaches agents how to configure and use MCP Server effectively—a Skill that makes other Skills more powerful.


## **Momentum and What’s Next**


We’re excited to share that since launching, our Skills have already found an audience:


- **Trending on skills.sh** within the first week of launch
- **Thousands of installs** across the Apollo Skill set
- **Internal adoption** at Apollo—our own engineers use these Skills daily


One pattern we’ve noticed: developers install the Skills, forget about them, and then are surprised when their AI agent suddenly “knows” Apollo conventions. That’s the goal: invisible expertise that surfaces when needed.


This creates a feedback loop. When our team uses the Skills, we notice gaps. Missing patterns get added. Confusing instructions get clarified. The Skills become living documentation that improves with use.


We’re just getting started. On the roadmap:


- **Enterprise Skills** : Deeper coverage of GraphOS, Apollo Federation, and advanced schema patterns
- **Documentation integration** : Tighter links between agentic skills and Apollo’s official docs
- **Community expansion** : More Skills from the community, covering specialized use cases


## **Try It Yourself**


Ready to give your AI agent Apollo expertise? Run` npx skills add apollographql/skills` and start a conversation about GraphQL. Want to contribute or explore the source? Check out the[repository on GitHub](https://github.com/apollographql/skills) or browse the[skills directory](https://skills.sh/apollographql/skills) .


## **Conclusion**


Agent Skills encode developer expertise once and make it available everywhere. For Apollo, this means AI agents that understand GraphQL the way we intend: with current best practices, proper patterns, and awareness of common pitfalls.


The shift is already happening. Developer tools that actively teach AI agents how to use them will see better adoption. Those that don’t will watch their users fight the same battles, session after session.


We’d rather be in the first group.


Written by


Dale Seo


[Read more by Dale Seo](https://www.apollographql.com/blog/author/dale-seo)
