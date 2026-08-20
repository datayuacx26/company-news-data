---
schema_version: "1.0.0"
document_id: "2948eb8bead3985d8ebd44493bfaac800c9775d389e1429a4657d73dfd7cae7e"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/agent-sdk"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:eaf116572f0100e5236665019493627ced3b4a6f33ec701eaf7f924d13c711ee"
---

# The Airbyte Agent SDK: Ship Agents, Not Data Plumbing

Agent demos work because the agent’s path is predetermined.


The agent knows which customer to look up, which API to call, and which fields matter. Production agents do not get that luxury. A user asks a messy question, and the agent has to discover where the knowledge lives quickly and accurately.


It pages through a list of Salesforce accounts. It navigates new and unfamiliar schemas from Stripe for the first time, and is expected to understand them. Everything gets pulled from each source endpoint into the agent’s context window, decreasing the quality of its eventual output.


True reasoning requires more than context assembly at runtime.


Raw APIs are built for applications that already know what they need: the endpoint, object ID, fields, and operation. Production agents often start one step earlier, at discovery. Without a way to discover what entities matter and where they are, they are hamstrung before they begin.


The Airbyte Agent SDK is built on a split architecture that aims to solve the toughest problems in implementing production agents. It has a context layer for discovery, live connectors for execution, and audited writes back to source systems.


## The Need for Independent Agent Operations


Discovery, fetch, and action are three separate[agent operations](https://airbyte.com/agentic-data/agent-tool-calling) . The problem with most API integrations is that they attempt these actions in one runtime loop.


Discovery is the most challenging. The agent must search and identify the relevant data sources, parameters, and entities. And it becomes increasingly complex as a business grows and integrates more systems for the agent to query.


Once the agent has discovered the relevant systems and parameters, it must fetch fresh state data from the source system. This has to be done in real-time.


Now the agent has the information it needs to act. Acting can be an updated record or newly posted ticket, or a Slack message. Acting holds the most risk of these agent operations, and so it is imperative that each action is logged and auditable.


This full loop can be executed at runtime. But it only works when application code already knows the endpoint, object ID, and operation. It fails when an agent starts with intent instead of an ID.


A production agent might be asked, “Should we refund this customer?” Before it can answer, it has to discover the customer, connect that customer to recent support tickets, check billing history, understand account status, and decide which system has the source of truth. That discovery-first sequence is the everyday reality of a[returns and refund agent](https://airbyte.com/use-case/e-commerce-returns-and-refund-agent) , which has to reconcile the order, the ticket, and the payment before approving anything.


When those steps all happen through raw APIs at runtime, the model pays for every mismatch. In tokens, latency, and tool calls. And sometimes in reliability, because the agent is forced to infer structure from API responses that were never designed for that job.


The agent should only fetch and act once discovery is executed successfully. And discovery should happen against structured, indexed context. A split architecture consisting of a context layer and live connectors is how we solve this problem.


## **What the SDK changes**


The Agent SDK brings the split architecture for discovery and execution to life. It gives developers a[Python interface](https://airbyte.com/ai-developers) to Airbyte Agent Connectors and the[Context Store](https://docs.airbyte.com/ai-agents/concepts/context-store) : a unified data index.


The Context Store handles discovery. Searchable business context is structured and indexed before the agent runs.


Connectors handle execution. Live reads, writes back to source systems, and an audit trail for every action.


That means the agent does not have to spend its runtime stitching together raw API responses. It can first find the right business entity, then fetch fresh state or take action when it matters.


The SDK is Python. Install it with one command (UV is a Python package manager) and you're up and running in minutes:


` uv add airbyte-agent-sdk`


The core loop is connect to a source, execute operations, get typed results back:


PYTHON


```text
from   airbyte_agent_sdk  import   connect


github = connect( "github"  )
result =  await   github.execute( "issues"  ,  "list"  , params={ "per_page"  :  10  })
for   row  in   result.data:
print  (row)
```


Each connector exposes entities (issues, contacts, deals, invoices) and actions (list, get, search, create, update). For typed connectors, you get IDE autocompletion and method-level docstrings.` await hubspot.contacts.list(limit=10)` works exactly the way you'd expect. For connectors without typed submodules, the same` execute(entity,action, params)` interface handles everything.


## **Works alongside your framework, not instead of it**


This is the part most teams care about. The SDK isn't a framework. It's not an orchestrator. It's the data layer underneath whatever you're already using: Claude Agent SDK, OpenAI Agents SDK, PydanticAI, LangChain, FastMCP, or your own Python loop. The pattern is the same: wrap a connector as a tool, expose it to your agent, and let the model call it. And if you'd rather drive the same connectors, schemas, and actions from a terminal, the[Agent CLI](https://airbyte.com/blog/airbyte-agent-cli) exposes all of it as simple commands an agent can call directly.


When you want broad coverage without wiring each operation individually, the` tool_utils` decorator generates a complete tool description from the connector's schema. Every entity, every action, every parameter. The LLM sees the full surface with no extra work from you:


PYTHON


```text
from   pydantic_ai  import   Agent
from   airbyte_agent_sdk  import   connect
from   airbyte_agent_sdk.connectors.github  import   GithubConnector


agent = Agent( "openai:gpt-5.4"  )
github = connect( "github"  )


@agent.tool_plain
@GithubConnector.tool_utils
async    def    github_execute  ( entity:  str  , action:  str  , params:  dict   |  None   =  None   ):
return    await   github.execute(entity, action, params  or   {})
```


When you want full control over what the agent sees, you can define one tool per connector with a hand-written docstring. Either way, the Context Store is what sits underneath.


The SDK wraps each connector as a tool your agent can call. You use connect() to establish the connection, then execute() to run operations. The agent determines which entity and action to call based on your natural language prompt.


## **What the numbers actually tell you**


When we started benchmarking the Context Store against the alternatives, the token savings were expected. What surprised us was how much they compound once you think through what they actually buy you.


Compared to agents pulling from raw APIs, using the Context Store enabled 40% fewer tool calls and up to 80% lower token consumption ([More in the launch blog](https://airbyte.com/blog/airbyte-agents) ).


Fewer tokens isn't just a discount. It's a reasoning budget. Fewer result tokens leaves more of the context window available for instructions, prior turns, intermediate reasoning, and follow-up tool calls.


In production, an agent won’t run just once. We envision a future where production agents will run thousands of times a day. Across 10,000 daily calls, the token savings turn into hundreds of millions of tokens you didn't pay for, didn't wait on, and didn't ask the model to wade through. That's the gap between an agent that works in a demo and one that will survive production.


## **Structural advantages beyond token counts**


Two things about the Context Store matter as much as the raw numbers.


First, consistency. Every vendor MCP returns information differently and without consistent standards. They have their own limits and truncation rules, and your agent has to figure them out on the fly. The Context Store returns consistently structured information so agents always know how to reason effectively.


Second, structure. The Context Store returns[typed JSON](https://airbyte.com/context-store) the agent can filter, sort, and join programmatically. Many MCP paths return loosely structured natural-language outputs or raw API payloads. A paragraph that says "Sarah opened a P1 issue last Tuesday about the checkout bug" is perfectly readable, but an agent can't sort it by severity or join it against a CRM record. Typed JSON skips the parsing step entirely. The agent gets fields it can operate on directly.


## **Read and write**


Read-only agents can tell you what's happening. They can't do anything about it. The SDK supports write operations for a growing number of connectors. Create a Jira ticket, update a Salesforce record, post a Slack message. Same execute()interface.


Every write is logged and traceable. When your agent is touching systems of record, you need to know what it did, when it did it, and which credentials it used. For production, that's table stakes.


## **Where we are today**


The product is early. The replication infrastructure underneath has been running in production for thousands of companies for years, and the Context Store is working well for the teams already building on it. But there's a lot still being shipped, and we’re excited to keep building.


We're launching with 50 production-ready connectors: Salesforce, HubSpot, Zendesk, Jira, Slack, GitHub, Stripe, Gong, Linear, and more. New connectors are shipping every week.


[Start building](https://app.airbyte.ai/) with our Free plan today. Paid plans are available as you scale your usage.


For install instructions, code examples, and the full API reference, check out the[SDK documentation](https://docs.airbyte.com/ai-agents/interfaces/sdk/) .


If you're building agents and hitting the data wall, this is what we built it for.[Give it a try](http://app.airbyte.ai/) .
