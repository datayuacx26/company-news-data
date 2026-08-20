---
schema_version: "1.0.0"
document_id: "93040e8b34bbecb37503dd963aa78751b2a76f75d7d57cd1799f11f692798cea"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/ai-workflow-automation-natural-language"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T00:10:11.482706+00:00"
fetched_at: "2026-07-29T00:10:13.250041+00:00"
content_hash: "sha256:60259016d4f83521050f2ea5b352622cf0fc3a2f965dd661483f37c48ebc1464"
---

# One prompt, a complete workflow: Elastic's AI agent writes your automation for you

Agent Builder is available now GA. Get started with an[Elastic Cloud Trial](https://cloud.elastic.co/registration?utm_source=agentic-ai-category&utm_medium=search-labs&utm_campaign=agent-builder) , and check out the documentation for Agent Builder[here](https://www.elastic.co/docs/solutions/search/elastic-agent-builder?utm_source=agentic-ai-category&utm_medium=search-labs&utm_campaign=agent-builder) .


## One prompt, a complete workflow: Elastic's AI agent writes your automation for you


[Elastic Workflows](https://www.elastic.co/search-labs/blog/elastic-workflows-automation) now writes its own YAML. You type what you want automated in plain language, the Elastic AI Agent generates a complete workflow against a typed schema, and nothing runs until you've read it. YAML is why this works: it gives the model a constrained, well-typed target, so what comes back is actual building blocks you can edit and run.


**What is new in Elastic Workflows 9.5:**


- **Natural language authoring** is GA and on by default: describe an automation, and the Elastic AI Agent writes the workflow, you review and run it.
- **Versioning** with diff and one-click rollback is GA.
- Three new experimental previews (behind an advanced setting): a visual mode that renders a workflow as a graph, human-in-the-loop steps that reach people in Slack for input or approval, and parallel execution.
- More to build on: new connectors, event triggers that react to Cases activity,token metering for AI steps, and a queue strategy for concurrency.


Workflows is the automation engine built into the Elastic platform. It reached general availability in 9.4, enabled by default and running against your[Elasticsearch](https://elastic.co/elasticsearch) data with the connectors and access controls you already have. This post walks through what 9.5 adds.


## Why YAML makes AI workflow automation work


YAML is the authoring language for Elastic Workflows because it's declarative, version-controllable, diffable, and portable across environments. It reads the same in a pull request as it does in the editor.


It was also a bet. Large language models ([LLMs](https://www.elastic.co/what-is/large-language-models) ) are very good at generating structured, well-typed content, and a workflow language is close to an ideal target for that. Ask for prose and a model can wander. Ask for a workflow against a typed schema, with named step types and validated inputs, and there is a right shape for the answer.


In 9.5 that bet pays off, and it is GA. Inside the workflow editor, you write what you want in plain language:


When a detection alert fires for a host, pull the last 24 hours of related logs, ask the AI step to summarize what happened, and post the summary to the on-call Slack channel.


The Elastic AI Agent generates the workflow: the trigger, the Elasticsearchquery , the AI summarize step, the Slack step, wired together with the right inputs and outputs. You get inspectable, editable YAML back. Nothing runs until you read it, adjust it, and decide to run it. You can also point it at a workflow you already have and describe the change you want, and it edits in place.


*Natural language prompt on the right, the generated YAML on the left, in the 9.5 workflow editor.*


The output is worth reading because there is a rich, well-typed language underneath it. A short prompt expands into real building blocks:


- ` foreach` and` while` loops, with guardrails that stop runaway execution.
- ` switch` for clean multi-way branching.
- data steps like` data.filter` and` data.aggregate` for in-flight transforms.
- ` on-failure` handling on every step, so you can retry, continue, or abort.
- ` workflow.execute` , so one workflow can call another and you assemble new automation from pieces you have already tested.


Natural language gets you the first draft fast; the language underneath is what makes that draft real.


## Workflow versioning with diff and one-click rollback


Versioning is GA in 9.5. Every workflow now has version history: every change is tracked and diffable, and you can roll back to any prior version in one click. You see who changed what and when, compare any two versions side by side, and undo a bad edit without reconstructing it by hand. This is the change-control foundation teams asked for before they would run automation against production systems.


*The version history panel showing a diff between two workflow versions, with one-click rollback.*


Versioning pairs with the production controls already in place: granular role-based access control (RBAC) over who creates, edits, runs, and views workflows, every management action written to the security audit log, and import/export that moves workflows between environments with their connector references intact.


Versioning in the product is the near end of a longer arc. A workflow is a declarative YAML definition, plain text with a well-defined schema, which means it already fits the tooling built for code: it can be diffed, reviewed, and version-controlled. Where we are headed is full, bidirectional integration with the version control systems you already use, so that a workflow could live in your repository, move through review, and deploy the same way the rest of your software does. That is coming, and the same bet that made natural language authoring work, a declarative and well-typed language, is what will let you manage workflows as code.


## Visual mode, human-in-the-loop workflows, and parallel execution


Three of the newest 9.5 additions ship in Experimental. To try them, turn on **Elastic Workflows: Experimental Features** in **Stack Management → Advanced Settings** (it requires a page reload). Here is what each one does.


### Visual workflow editor: see the logic as a graph


You can now switch a workflow between the YAML editor and a visual mode that renders the workflow as a graph. The graph lays out your steps, branches, and flow control, so you can see the logic and the paths a run can take at a glance, alongside the YAML. It is read-only in 9.5: you still author in YAML, and the graph stays in sync as you edit. It is the first step toward a full drag-and-drop builder, which is coming next.


*Toggling one workflow between the YAML editor and the visual graph view.*


### Human-in-the-loop workflows with approval steps in Slack


Workflows could already pause for a person in 9.4 with` waitForInput` , which presents a schema-defined form and lets the response drive what happens next. 9.5 adds` waitForApproval` for the most common case of all, a binary approve or reject with labels you choose, and it extends both steps to their first external surface: Slack. Both pause the run until someone answers, with a timeout so a workflow never hangs forever. Not every decision should be fully automated, and this is how you put the human on exactly the steps that need one.


` waitForInput` is the flexible one: define a schema for the input you want back, and the response comes through typed. Reach for it when the choice is more than yes or no. Here, an Observability workflow has caught a service-level objective (SLO) burn-rate alert on` payment-service` and asks the on-call engineer which mitigation to run:


` waitForApproval` is the new one in 9.5, for a straight approve or reject. Here a security workflow has decided a host should be contained, but gates that destructive action on a human before it runs:


The new piece is the` channels` block. A wait step can now deliver to where people already are, and in 9.5 that means Slack: the workflow posts the request to a Slack channel, the person responds from Slack, and the workflow resumes with their answer. No one has to be sitting in[Kibana](https://elastic.co/kibana) for the automation to move. Slack is the first external surface, and richer delivery experiences across more channels are on the way.


### Parallel execution: run independent workflow steps at once


By default a workflow runs one step after another, which is what you want when each step depends on the last. But plenty of work does not: enriching an alert from three sources, checking a file against several reputation services, investigating a handful of leads. Run those sequentially and the workflow is only as fast as the sum of its parts, when it could be as fast as the slowest one. The new` parallel` step lets you run independent work at the same time. It works two ways.


The first is when you know the work ahead of time. You define a fixed set of tasks, and they run at the same time, so you gather all the results in one step instead of waiting for each in turn. Enriching a security alert from two sources at once is the classic case:


The second is when you do not know the work ahead of time. You give the step a list, and it runs the same work once per item, all at the same time, up to a concurrency limit you set. Root cause analysis is a good example. An earlier AI step generates a set of hypotheses for why a service is degrading, and you do not know in advance how many there will be or what they are. Rather than investigate them one after another, you pass the list into a parallel step, and it investigates every hypothesis at once:


You control how many run at once with **concurrency** , and the engine caps both the concurrency and the total number of parallel tasks so a workflow cannot spawn unbounded work. All the results are available to the next step, so the workflow runs the parallel work, then continues once every task finishes.


## More in Elastic Workflows: connectors, triggers, token metering and concurrency


Beyond the headline features, 9.5 widens what a workflow can reach and react to.


### New connectors: BigQuery, Snowflake, HubSpot, Cortex XSOAR and more


The connector catalog keeps growing, with native connectors added in 9.5 for:


- BigQuery
- Snowflake
- Box
- Dropbox
- OneDrive
- Outlook
- Azure Blob
- Google Cloud Functions
- HubSpot
- Cortex XSOAR connector for security automation


More are on the way, and when there is not a dedicated connector for the system you need, the` http` step is the escape hatch: it can securely call any APIendpoint , with credentials supplied by a connector rather than written into the YAML.


### Event-driven triggers for Elastic Cases


A workflow starts from a trigger, and 9.5 widens what a workflow can respond to. Cases now emit events a workflow can subscribe to:


- A case is created.
- A case is updated.
- Its status changes.
- A comment is added.
- An attachment is added.


So a workflow can run the moment a case opens, to enrich it, tag it, or notify the right channel, or when its status flips to a state you care about, rather than polling for changes. Alert-triggered workflows also receive richer rule context now, including the rule's tags, type, and parameters, so the workflow has more to work with before it acts.


### Token usage and cost tracking for AI workflow steps


Workflows can call AI steps:` ai.prompt` for a freeform prompt,` ai.classify` to sort something into categories,` ai.agent` to hand a task to an[Agent Builder](https://www.elastic.co/elasticsearch/agent-builder) agent. In an automation that runs thousands of times a day, those calls add up. 9.5 now reports token usage for every AI step, input, output, cached, and total, both per step and for the whole run. You can see exactly what the AI in a workflow consumes, track it over time, and tune a prompt or a model choice with the numbers in front of you.


### Workflow concurrency: cancel, drop or queue


A workflow's concurrency setting decides what happens when a new execution starts before the last one finishes. 9.5 adds a third strategy, so you can pick the behavior that fits the workflow:


Strategy Use it when


Cancel-in-progress Only the latest execution matters, like recomputing a current state


Drop An execution already in flight covers the situation and extras are redundant


Queue (new in 9.5) Every execution matters and order does, so they line up and run one after another, with a queue size and time-to-live you control


Queue is what you reach for when executions touch the same resource or should not overlap. Audit logging also covers more of the lifecycle in 9.5, including restoring a workflow from its version history.


## Get started with Elastic Workflows


The fastest way to see this is to describe something you want automated. Open the workflow editor in 9.5, type it in plain language, and read the YAML that comes back. Natural language authoring is on by default. To try the visual mode, human-in-the-loop steps, and parallel execution, turn on **Elastic Workflows: Experimental Features** in **Stack Management → Advanced Settings** .


The theme across 9.5 is a shorter path from idea to running automation. You describe what you want and AI drafts it, you see it as a graph and version it as you go, you pause it for a person in Slack when a step needs judgment, and you run independent work in parallel. For the full details, see the[Workflows documentation](https://www.elastic.co/docs/explore-analyze/workflows) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Operations](https://www.elastic.co/search-labs/blog/category/operations)


July 28, 2026


#### [Your agents have been keeping receipts: turning Elastic Agent Builder's built-in OTel traces into token cost dashboards in Kibana](https://www.elastic.co/search-labs/blog/opentelemetry-tracing-agent-builder)


Your Agent Builder agents already log every LLM call as an OTel trace, and that agent tracing data can power token cost dashboards and budget alerts before one runaway conversation quietly wrecks your month.


MMPM


By:[Meghan Murphy](https://www.elastic.co/search-labs/author/meghan-murphy)


and[Pablo Neves Machado](https://www.elastic.co/search-labs/author/pablo-neves-machado)


[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai)[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)


July 23, 2026


#### [On-prem in under 5 minutes: Jina embedding models now available for on-prem deployment](https://www.elastic.co/search-labs/blog/on-prem-ai-jina-embedding-models)


All 28 Jina AI models, including rerankers, as ready-to-deploy Docker containers, with zero telemetry and no license server. Drop-in compatible with OpenAI, Cohere, Voyage AI and Elastic Inference Service APIs.


SM


By:[Scott Martens](https://www.elastic.co/search-labs/author/scott-martens)


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Hybrid Search](https://www.elastic.co/search-labs/blog/category/hybrid-search) +1


July 20, 2026


#### [AI shopping agents: Why context comes before the query](https://www.elastic.co/search-labs/blog/ai-shopping-agents-context-engineering)


AI shopping agents that guess at your vocabulary make expensive mistakes. Pre-computed catalog context stops the guessing before the first tool call.


MA


By:[Matthew Adams](https://www.elastic.co/search-labs/author/matt-adams)


[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database) +1


July 21, 2026


#### [4 NVIDIA AI tasks, 1 Elasticsearch API: Embeddings, chat, completion, and rerank](https://www.elastic.co/search-labs/blog/elasticsearch-nvidia-inference)


Set up NVIDIA hosted models in Elasticsearch with one API key and a model ID. No custom integration code needed.


K


By:[Jan Kazlouski](https://www.elastic.co/search-labs/author/jan-kazlouski)


[ES|QL](https://www.elastic.co/search-labs/blog/category/esql)[Developer Experience](https://www.elastic.co/search-labs/blog/category/developer-experience) +1


July 1, 2026


#### [One command. Natural language. Your Elasticsearch data, straight to the terminal.](https://www.elastic.co/search-labs/blog/elasticsearch-copilot-cli-esql-natural-language)


Query your Elasticsearch data from the terminal in plain English. The official Elastic GitHub Copilot CLI plugin generates and runs ES|QL queries against your cluster. No Kibana, no manual syntax.


GC


By:[Greg Crist](https://www.elastic.co/search-labs/author/greg-crist)
