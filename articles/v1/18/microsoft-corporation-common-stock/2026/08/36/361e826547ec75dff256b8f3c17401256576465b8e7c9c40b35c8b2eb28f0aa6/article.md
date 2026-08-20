---
schema_version: "1.0.0"
document_id: "361e826547ec75dff256b8f3c17401256576465b8e7c9c40b35c8b2eb28f0aa6"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0524baf4a828"
canonical_url: "https://devblogs.microsoft.com/cosmosdb/azure-cosmos-db-in-the-agentic-era-data-tools-for-developers-and-ai-agents/"
published_at: "2026-08-19T17:01:55+00:00"
first_seen_at: "2026-08-19T18:46:24.443743+00:00"
fetched_at: "2026-08-19T18:46:26.199437+00:00"
content_hash: "sha256:71d52308e25c39fb1cb4543441262537f1d76770d9fc63688b54d06947e7e6c8"
---

# Data tools for developers and AI agents

[Anthropic’s 2026 Agentic Coding Trends Report captures](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf?hsLang=en) an important tension in how developers use AI. Engineers report using AI in roughly 60% of their work, yet say they can fully delegate only 0–20% of their tasks. AI is becoming a constant collaborator, but it still needs setup, supervision, validation, and human judgment.


That gap is especially relevant when an agent starts working with a database.


[https://devblogs.microsoft.com/cosmosdb/wp-content/uploads/sites/52/2026/08/devtools-1.mp4](https://devblogs.microsoft.com/cosmosdb/wp-content/uploads/sites/52/2026/08/devtools-1.mp4)


Database requests often sound easy.


**“Show me the failed orders from last week.” “Why is this query expensive?” “Does this container have the field I need?”**


For a developer, answering those questions usually means opening the right account, finding the container, checking a document, writing a query, and looking at the RU charge. A coding agent can take on much of that tactical work, but only if it has more than a blank chat box. It needs a way to see the editor context, understand the actual schema, and run a query with the developer’s permission.


That is the direction of the recent[Azure Cosmos DB extension updates for Visual Studio Code](https://learn.microsoft.com/en-us/azure/cosmos-db/vscode-extension/overview) . They bring GitHub Copilot tools and Cosmos DB skills into the Query Editor, add an optional MCP path through[Azure Cosmos DB Shell](https://learn.microsoft.com/en-us/azure/cosmos-db/shell/) , support local work with the[Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/emulator) , and extend the experience into migration and account health.


The interesting part isn’t that an AI can produce query text. We’ve had that for a while. The interesting part is that it can now participate in the surrounding workflow without hiding what it is doing. It fits a broader shift from writing every implementation detail toward directing agents and evaluating their work.


## A query is more than query text


Asking an AI assistant to “show me all pending orders from the last seven days” sounds simple.


The hard part sits behind the sentence. Which account should Copilot use? Which database and container are open? Is the property named status, orderStatus, or something else? Is the timestamp a string or a number? And what will the query cost?


A model can’t reliably infer any of that from the prompt, and it shouldn’t pretend otherwise.


The Azure Cosmos DB Visual Studio Code extension now gives GitHub Copilot tools for working with the Cosmos DB for NoSQL Query Editor. Copilot can:


Find an open Query Editor connection


- Open or focus the correct Query Editor
- Inspect the current query and connection context
- Sample the container schema
- Apply a generated query to the editor
- Execute the query and wait for its completion


So the result doesn’t have to sit in chat waiting to be copied. Copilot can put the query into the active editor and, when asked, run it there.


There is no separate @cosmosdb personality or another chat experience to learn. The regular Copilot agent uses the tools when the task calls for them.


## Check the schema before guessing


One of the fastest ways to lose trust in an AI-generated query is for it to invent a property.


A developer asks for active customers, and the model confidently writes:


```text
SELECT * FROM c WHERE c.isActive = true
```


It looks perfectly reasonable. It is also useless if the real property is accountStatus.


Before generating a query, Copilot can sample the active container and infer the property names and types it actually finds. The query is then based on the developer’s data instead of the model’s best guess.


The sampled schema is useful outside chat too. It also improves editor autocompletion and result-schema inference.Sampling isn’t silent. It reads data and consumes request units, so the extension asks first. Query execution follows the same rule. That small pause matters. Copilot can prepare the work, but the developer decides when it touches data or spends RUs.


## Tools still need database judgment


Tool access solves only half of the problem.


An agent may know how to execute a query and still generate one that is inefficient or invalid for Cosmos DB. It might use relational JOIN semantics, write unsupported DML, scan every partition, or query when a point read would be cheaper.


This is why the extension also ships[dedicated agent skills](https://github.com/AzureCosmosDB/cosmosdb-agent-kit) . They give Copilot the Cosmos DB-specific guidance that a generic model won’t always have. The **Azure Cosmos DB for NoSQL Query Generation** skill teaches Copilot the NoSQL query dialect, including projections, array-unwind joins, aggregates, full-text search, vector search, hybrid ranking, pagination, and supported built-in functions. The **Query Editor** skill teaches the agent how to use the editor tools: inspect context, sample the schema, apply the query, and execute it when the developer asks to see results.


The[Azure Cosmos DB Agent Kit skill](https://github.com/AzureCosmosDB/cosmosdb-agent-kit) covers more than 100 recommendations across data modeling, partition-key design, query optimization, SDK usage, indexing, throughput, global distribution, monitoring, vector search, full-text search, and agent application patterns.


I find the distinction useful: tools give the agent hands; skills give it a working knowledge of the database.


That knowledge is also available during ordinary coding work. A[developer can ask about a repository implementation, a partition key, or an indexing policy](https://www.youtube.com/watch?v=GWid8x3i9Cc) without pasting the same documentation into every conversation.


The Agent Kit brings that knowledge into everyday coding across GitHub Copilot, Claude Code, Codex, Cursor, Gemini CLI, and other compatible agents, letting developers ask about repository implementations, partition keys, or indexing policies without repeatedly pasting documentation into each conversation.


## Keep a person in the loop


The moment an agent can reach a database, the boundaries need to be obvious. Anthropic calls this the “collaboration paradox.” Developers use AI frequently and see meaningful productivity gains, but they tend to delegate work that is well-defined, low-risk, or easy to verify. More consequential work stays collaborative. Database access belongs firmly in that second category.


The extension doesn’t quietly keep entire query results in its agent-facing history. It records the query text and useful metadata, including row counts, request charges, and inferred schemas, rather than raw documents. Schema sampling asks for consent. Query execution asks for consent. The generated query lands in the Query Editor, where it can be read and changed before anything happens.


It is a practical division of responsibility: Copilot handles discovery, syntax, and the repetitive steps; the developer keeps control of data access and execution. Human attention is spent on the moments that carry cost, security, or business impact rather than on every mechanical step.


Connections can also use Microsoft Entra ID, managed identity, connection strings, Azure CLI authentication, and read-only keys depending on the tool and environment. Existing Cosmos DB permissions continue to determine what the connected identity is allowed to do.


An agent shouldn’t become a back door around database security. It should work through the same identities and permissions as the person or application it represents. As agents become more capable, security has to be part of the architecture from the beginning rather than added after the workflow is automated.


## When the Query Editor isn’t the whole workflow


The Query Editor is a natural home for query work, but not every agent stays inside it.


Another forecast in the report is that agent tasks will grow from short, one-shot requests into workflows that run for hours or days and recover from failures along the way. Those longer-running workflows need durable, explicit ways to reach their tools; a pasted query and a short-lived chat context are not enough.


[Azure Cosmos DB Shell](https://learn.microsoft.com/en-us/azure/cosmos-db/shell/overview) provides a lightweight, command-line experience for navigating accounts, databases, containers, and items. It supports queries, management operations, scripting, and multiple authentication methods.


ark-themed Azure Cosmos DB Shell interface showing a terminal with commands for browsing databases and containers and displaying JSON documents, alongside highlights for CLI, real-time data exploration, JSON-first workflows, and performance.


The shell also has an optional MCP server mode. Once a developer enables it, GitHub Copilot and other MCP-compatible clients can use shell operations as tools.


That leaves developers with two sensible paths: native, schema-aware Query Editor tools inside VS Code, and Cosmos DB Shell through MCP for workflows that reach beyond the editor.


One detail is worth being plain about: this is not a hosted, managed MCP service. It is optional, runs under the developer’s control, and must be enabled explicitly. Teams still need to decide where it runs, which identity it uses, and what that identity is allowed to do.


## Start with fake data and a local database


An agent shouldn’t need production data to prove it can write a query.


The[Azure Cosmos DB Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/how-to-develop-emulator) gives developers a local service for exactly this kind of work. Attach it to the VS Code extension, create a few realistic containers, and exercise the Query Editor workflow before pointing an agent at a cloud account.


The Linux-based vNext Emulator runs in Docker and includes Data Explorer and Azure Cosmos DB Shell. Seed scripts make test data repeatable, while health probes make the container usable in automated tests. Teams can run it in CI as well, avoiding the familiar problem of one shared development database slowly becoming everybody’s mystery state.


For agent work, use synthetic documents that have the awkward parts of real data: missing fields, inconsistent shapes, and values no one expected. Then test whether schema sampling keeps the generated query honest. This makes active validation concrete. If the agent goes wrong, it goes wrong locally, and the failure can become a repeatable test.


The Emulator isn’t the cloud in a Docker container. The vNext version supports the API for NoSQL in gateway mode and only a subset of cloud capabilities. Request-unit behavior and some production features aren’t fully represented. Use it for functional confidence, then validate performance, indexing, security, scale, and regional behavior in Azure.


## Stay in the flow


The best database tool is usually the one that doesn’t pull a developer away from the problem they were solving.


From the Cosmos DB Query Editor, a developer can select **Generate query** , describe what they need in ordinary language, review the generated NoSQL query, and run it. They can also ask Copilot to explain an existing query.


The results remain in the familiar Query Editor, with table, JSON, and tree views. Developers can inspect execution time, request-unit consumption, query metrics, and index recommendations without moving the investigation to another product.


A typical request might be:


“Show me the ten most recent failed payment attempts for this tenant.”


Copilot can identify the active container, ask to sample its schema, generate a partition-scoped query with the real property names, and place it in the editor. The developer can read it, change it, and approve execution.


The useful part isn’t the SQL-like text. It is getting through the small surrounding steps without losing sight of the query or what it costs.


## Before the first query and after the thousandth


Not every Cosmos DB project starts from an empty repository.


The[AI-assisted Migration Assistant, currently in preview](https://learn.microsoft.com/en-us/azure/cosmos-db/vscode-extension/cosmos-db-migration-assistant) , helps teams examine a relational workload before moving it to Cosmos DB. It guides developers through source-schema discovery, access-pattern analysis, workload estimates, application requirements, and conversion to a Cosmos DB target model.


That matters because moving to Cosmos DB isn’t a mechanical table-to-container conversion. The target model has to reflect how the application actually reads and writes data.


Once an application is running, the extension’s Account Overview dashboard provides a read-only view of inventory, throughput, normalized RU consumption, partition health, alerts, recommendations, and derived advisories.


These are two ends of the same job. AI can help a team reason about the move and investigate queries later, but people still need a clear view of cost, partition behavior, and account health once the system is real.


## The point isn’t autonomy


None of this removes developers from database work. It removes some of the hunting, copying, and syntax recall around that work.


This broader model can help engineers work across more of the stack, with AI filling knowledge gaps while people provide direction and judgment. Cosmos DB tools and skills are a practical example: they let a developer move from application code into data exploration, query design, local testing, and account diagnostics without pretending that database expertise no longer matters.


A developer can describe the outcome, let Copilot assemble the steps, and still inspect the query, approve access, review its RU cost, and decide what happens next.


Azure Cosmos DB now provides several ways to support that collaboration:


- Native GitHub Copilot tools in the VS Code extension
- Schema-grounded natural-language query generation
- Cosmos DB query and best-practice agent skills
- Optional MCP support through Azure Cosmos DB Shell
- Local agent development and CI testing with the Azure Cosmos DB Emulator
- AI-assisted relational migration
- Operational visibility through the Account Overview dashboard


The goal isn’t unrestricted database access for an agent. It is enough context and capability to be genuinely useful, with a person still able to see and control the consequential parts.


For me, that is the useful shape of database tooling in this new workflow: grounded in the real schema, honest about cost, governed by identity, and close to where the developer is already working.


## **About Azure Cosmos DB**


Azure Cosmos DB is a fully managed and serverless NoSQL and vector database for modern app development, including AI applications. With its SLA-backed speed and availability as well as instant dynamic scalability, it is ideal for real-time NoSQL and MongoDB applications that require high performance and distributed computing over massive volumes of NoSQL and vector data.


To stay in the loop on Azure Cosmos DB updates, follow us on[X](https://twitter.com/AzureCosmosDB) ,[YouTube](https://aka.ms/AzureCosmosDBYouTube) , and[LinkedIn](https://www.linkedin.com/company/azure-cosmos-db/) .
