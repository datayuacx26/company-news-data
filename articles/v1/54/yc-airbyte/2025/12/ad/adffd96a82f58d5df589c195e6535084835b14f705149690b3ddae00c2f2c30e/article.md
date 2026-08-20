---
schema_version: "1.0.0"
document_id: "adffd96a82f58d5df589c195e6535084835b14f705149690b3ddae00c2f2c30e"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/agent-connectors"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:aec290eac4713e90ac3f6c5bc12dc3c5cccdc26cb5516651f8d46152a250931f"
---

# Introducing Agent Connectors from Airbyte

Today, we're launching Agent Connectors: open-source Python SDKs purpose-built to give AI agents consistent, real-time access to operational data for context-rich decision making.


This is the start of building the data infrastructure layer that connects AI agents to enterprise systems.


## **What We're Launching**


Open-source Agent Connectors for 10 critical operational systems: Gong, Zendesk Support, GitHub, HubSpot, Salesforce, Jira, Asana, Stripe, Greenhouse, and Linear.


These are standalone Python packages you can use directly in your app or plug into any[agent framework](https://airbyte.com/ai-developers) , giving your agentic applications strongly typed, well-documented access to third-party APIs. Each connector enables critical agent operations including fetch, search, and discovery in real-time.


**Try them now:**


- Repo:[Airbyte Agent Connectors](https://github.com/airbytehq/airbyte-agent-connectors)
- Docs:[Available here](https://docs.airbyte.com/ai-agents/connectors)


## **The Problem**


[AI agents](https://airbyte.com/agentic-data/what-are-ai-agents) are limited by the tools they can call. Most context lives inside SaaS APIs that hold CRM records, support tickets, billing events, recruiting pipelines, code repositories, and operational data. Yet giving agents reliable access to these APIs remains difficult.


Every integration becomes a custom wrapper, every schema behaves differently, and every edge case becomes another source of operational risk. None of these patterns support agents that need real-time decisions and just-in-time context retrieval.


The infrastructure layer between agents and operational systems doesn't exist. Everyone's building custom integrations, duplicating work, and creating reliability nightmares.


## **How Agent Connectors Work**


Agent Connectors are standalone Python packages that wrap operational APIs with strongly-typed interfaces. Each connector[handles authentication](https://airbyte.com/agentic-data/what-is-agent-authentication) , validates schemas, and exposes operations through consistent method signatures that both developers and LLMs can interpret reliably.


PYTHON


```text
from   airbyte_agent_gong  import   GongConnector
from   airbyte_agent_gong.models  import   GongAuthConfig


# Create connector
connector = GongConnector(
auth_config=GongAuthConfig(
access_key= "..."  ,
access_key_secret= "..."
)
)


users =  await   connector.users. list  (limit= 10  )
```


Attaching this to an agent is straightforward:


PYTHON


```text
@agent.tool_plain
async    def    list_users  ( limit:  int   =  10   ):
return    await   connector.users. list  (limit=limit)
```


Under the hood, they derive typed request and response models from the underlying API specifications, enforce schema validation at runtime, and provide transparent error handling. This gives agents the low-latency primitives they need for real-time fetch, search, and action operations without the brittleness of ad hoc API wrappers or the staleness of batch ETL.


Each connector is delivered as an independent Python package with a predictable structure:


CODE


```text
connectors/
├── gong/
│   ├── airbyte_agent_gong/
│   ├── pyproject.toml
│   ├── CHANGELOG.md
│   └── README.md
│   └── REFERENCE.md
├── github/
│   └── ...
└── ...
```


The REFERENCE.md file in each connector documents all available operations from listing sources to fetching specific records and executing actions. This gives both developers and Agents a complete map of what each connector can do. For example, the[Gong connector](https://airbyte.com/agentic-data/gong-agent-connector)[reference](https://github.com/airbytehq/airbyte-agent-connectors/blob/main/connectors/gong/REFERENCE.md) details operations like ‘users.list(), users.get(), calls.list(), and more, with full type information for each.


Every connector can be used directly in a Python application, attached to frameworks like PydanticAI or LangChain, integrated into any custom agent loop, or exposed through MCP-based interfaces for tool-calling LLMs.


## **Why we’re building this**


We've built a company on moving data reliably. We have 20,000+ GitHub stars, thousands of customers, and millions of pipelines synced daily. We've built and maintained hundreds of open-source connectors with a community of 27,000+ developers.


But as AI agents have emerged, I've noticed a challenge that dramatically limits their success: most enterprise APIs weren't designed for LLMs or agent execution models. As agents must interact with real operational systems, developers write custom wrappers, manage authentication constraints, normalize schemas, and handle inconsistent error behavior across every system their agent touches. This works at small scale but breaks when agents need to call multiple systems reliably.


Agent Connectors bring our connector expertise to agent workloads, providing a consistent way to fetch and search operational data without writing custom integration code for each system.


## **Why this matters**


I think agentic AI represents the biggest shift in software since the move to cloud. But the infrastructure layer needed to make it work in production doesn't exist yet.


We're not building another agent orchestrator or LLM framework. We're building the connection between facts and intelligence: the data layer that every framework plugs into.


The companies that win with AI won't be the ones with the best models. They'll be the ones who figured out how to give those models reliable access to live business context.


## **What's Next**


This launch is just the beginning. We're expanding Agent Connectors in two key directions:


- **Write Operations:** Beyond read access, we're adding write and trigger capabilities so agents can create records, update fields, and initiate workflows across systems.
- **Connector Library Expansion:** We're scaling from the initial 10 connectors to hundreds over the coming months, bringing the full breadth of our connector ecosystem to agent workloads.


We plan to continually expand the connector library using our expertise in data movement and community contributions to the open-source GitHub repo.


Learn more[here.](https://airbyte.com/ai)


## **Get Involved**


This is open source for a reason. We are all in uncharted territory, however we all know that the future of AI is Open-Source. We want to work with all of you to define what agentic data infrastructure looks like.


- **Try the connectors:**[github.com/airbytehq/airbyte-agent-connectors](https://github.com/airbytehq/airbyte-agent-connectors)
- **Give us a star ⭐** if you find it useful
- **Contribute:** Join #agent-connectors-discussions on the[Airbyte Slack](https://slack.airbyte.com/) to share your feedback
- **Talk to us:** DM me on LinkedIn or X if you're building agents and want to discuss your data infrastructure needs


The future of AI depends on solving the data infrastructure layer. Let's build it together!
