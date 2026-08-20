---
schema_version: "1.0.0"
document_id: "2867616ff621d4d95405f90e878937b417e36a3133692adfa0ee69853dc74063"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/data-mage-meet-our-ai-data-analyst-that-lives-in-slack"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T03:41:35.881970+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:608f0eeb1c85916d9c32a8542070972ff178034bb61a888dcd1544b7fdf94c9c"
---

# Data Mage: meet our AI data analyst that lives in Slack

As many other data teams do, we have a dedicated Slack channel (#data) where the whole company can ask data-related questions that get picked up by a designated, weekly-rotated data analyst (data mage). It can become tiresome quickly as the questions are fairly repetitive and it takes away precious time from other high-impact tasks. The team built clean, well-documented marts and reports in Metabase that are used regularly, but these were still missing something that makes reporting truly self-service.


In May 2025 we hooked up our internal chatbot with the BigQuery MCP toolbox, but it wasn’t agentic – it needed constant nudging, couldn’t explore the schema on its own, and couldn’t build on previous work. Finally, last November we, the data engineers, decided to carve out a week to see if we could build a Slack chatbot that can answer fairly sophisticated questions from our data warehouse.


A few months on, **a third of Bitrise has tried it** ; it averages **5.5 turns per conversation** (people don’t ask one question and leave – they have a *conversation* with their data), and the feedback has been the kind you screenshot:


“I’m chatting more with the Data Mage than with anyone else at this point.”


This post is the technical story of how it works, and – more interestingly – what we changed after launch once we understood where an LLM analyst actually goes wrong.


## The setup: a warehouse, a chat surface, and a model


Our data stack is conventional. SaaS tools, relational databases, and Dataflow pipelines land raw in **Google BigQuery** ; **dbt** transforms them through staging → intermediate → marts → reports, producing curated` mrt_*` and` rpt_*` datasets; **Metabase** sits on top for dashboards, reports, and ad-hoc queries.


Two product decisions shaped everything else. First, the surface had to be **Slack** , not email or a new web app: people had to reach the analyst where they already worked. Second, the analyst had to actually *understand Bitrise’s data model* – workspaces, users, builds, releases, the metrics that matter – and write and execute real SQL, building on the work the team had already done.


## The agent: PydanticAI + Claude


We built the agent on[PydanticAI](https://ai.pydantic.dev/) . For an internal tool that needs to be reliable more than it needs to be clever, three things mattered: **type-safe agent definitions** , **clean dependency injection** for tools, and **native Anthropic model support** .


The core of it is small. The agent is declared with a structured output type and a dependency-injected conversation context, and tools are registered by domain at startup:


```text
# data_analyst/agents/data_mage.py
agent: Agent[ConversationContext, AgentResponse] = Agent(
output_type=AgentResponse,            # structured Pydantic output
deps_type=ConversationContext,
system_prompt=build_system_prompt(),
)


# Tools registered by domain
manifest_tools.register(agent, settings)     # dbt schema exploration
bigquery_tools.register(agent, settings)     # SQL execution
metabase_tools.register(agent, settings)     # dashboard / card creation
utility_tools.register(agent)                # date utilities
memory_tool.register(agent)                  # persistent knowledge


```


That’s the whole shape of it: a model, a system prompt that teaches it our data, a handful of governed tools, and memory. The interesting engineering is in those last three.


## Teaching Claude your data


A model that can write SQL is not the same as an analyst who knows *your* warehouse. Most of the work in Data Mage is in the system prompt – encoding the procedure a good analyst follows so the model doesn’t have to guess. The core ideas (we keep the exact prompt internal, but the principles are the valuable part):


- **Clarify, don’t assume.** If a request is genuinely ambiguous – especially a bare ID that could be a build, an org, or a user – ask **one** focused question rather than guessing the entity type. A silently wrong assumption is worse than one extra message.
- **Explore the schema, never guess table names.** Search the dbt manifest by keyword first, then pull the description of a specific table. The model is explicitly told not to invent identifiers.
- **A house SQL style.** CTE-first, four-space indent, lowercase keywords: enforced in the prompt so generated queries are readable and reviewable.
- **A cost guard.** Every query is dry-run first. We’ll come back to this, because it turned out the prompt alone wasn’t enough.


## Why we wrote our own tools


The obvious move in 2026 is to point the agent at off-the-shelf MCP servers for BigQuery and Metabase and call it done. We tried; they weren’t customizable enough for a tool we were going to hand to the entire company. So we wrapped the underlying APIs with our own logic and enforced the guardrails *in the tool calls themselves* , not just in the prompt:


- **Read-only by construction.** The execution tool inspects the parsed statement type and refuses anything that isn’t a SELECT:


```text
# data_analyst/agents/tools/bigquery_tools.py
if   statement_type   not     in   [  "SELECT"  ]:
# reject UPDATE/DELETE/CREATE/... before anything runs
...


```


- **A real cost guard, not a polite request.** Every query is dry-run for a byte estimate before it executes. BigQuery on-demand pricing is roughly $6.25/TB scanned, so we turn the estimate into dollars and gate on it – under the threshold runs automatically, over it the agent has to ask permission:


```text
terabytes_estimate = dry_run_job.total_bytes_processed /   1e12
cost_estimate = terabytes_estimate *   6.25
```


- A prompt instruction can be argued with by a sufficiently motivated model. A code path cannot.


- **A bounded schema surface.** Only` mrt_*` and` rpt_*` datasets are exposed for schema exploration – the curated, consumption-ready layer – so the agent can’t go foraging in raw or staging tables and answer from the wrong source.
- **Metabase, fenced in.** Card and collection creation is restricted to a specific collection and toolset, so the agent can build dashboards without touching anything it shouldn’t.


The lesson generalizes: when you give an LLM real tools against production systems, the guardrails belong in the tools, where they’re deterministic – not only in the prompt, where they’re advisory.


## Memory: an analyst that remembers


Data Mage uses Anthropic’s **memory tool** , a file-system-style store that lets Claude create, read, update, and delete files that persist across conversations, so it builds knowledge over time instead of relearning the warehouse every session. PydanticAI supports it out of the box; we back the file system with PostgreSQL.


We run it as a **two-tier** system:


- An **admin tier** , curated by the data team – vetted notes like` backend_overview.md` ,` subscription_data_locations.md` , etc. that encode hard-won institutional knowledge.
- A **contributor tier** that everyone else can write to, so the things people teach the bot in passing don’t evaporate.


This is also where we found the rough edge. The memory tool is powerful but a bit of a black box: it needs steering, and the quality of what it retains depends heavily on how you frame it in the prompt.


## What we changed after launch – where an LLM analyst actually goes wrong


Adoption proved the concept, but watching real conversations taught us the lesson Anthropic[describes in their own writeup](https://www.anthropic.com/news/how-anthropic-uses-claude-for-self-service-data-analytics) of running self-service analytics with Claude: the hard part isn’t generating SQL – Claude is excellent at that – it’s **the ambiguity of the data** . The agent picks the wrong table for “MRR,” answers from a deprecated model that looks identical to a live one, or simply never retrieves the right thing. Most of our post-launch work attacks that. The highlights:


**The governed metric layer is now the mandatory default path.** The agent searches the governed metrics first and *starts from the metric’s SQL, adapting only the window or filter it needs* ; from-scratch SQL is a last resort it has to justify.


**Canonical tables win ties.** We rank tables by how many dashboards and metrics *directly* reference them. This is a strong signal for “this is the canonical table for the concept”:


```text
# data_analyst/agents/tools/manifest_tools.py
def     _build_exposure_dependent_counts  (  manifest_data  ):
counts = defaultdict(  int  )
for   exposure   in   manifest_data.get(  "exposures"  , {}).values():
for   node_id   in   exposure.get(  "depends_on"  , {}).get(  "nodes"  , []):
counts[node_id] +=   1       # DIRECT references only
return   counts


```


**Every answer carries a provenance footer** : a “How I answered this” trailer naming the source tier, the window/grain (and whether it was *assumed* ), filters, deprecation status, and the BigQuery job id and scan cost. It builds trust and forces the agent to *confess* when it dropped to raw SQL or touched a deprecated model.


**And a few smaller wins:** we upgraded the default model from Haiku to **Sonnet 4.6** ; and added a` bigquery_ai_key_drivers` tool wrapping BigQuery ML’s` AI.KEY_DRIVERS` for “what’s driving the change in X?” questions.


## Beyond Slack: Data Mage in Claude Desktop


The newest chapter is making Data Mage’s tools available *outside* Slack. We refactored the codebase to add an **MCP layer** on top of the same custom tools, so the exact same governed BigQuery, dbt-manifest, and Metabase logic can be driven by **Claude Desktop** . The shared-implementation design means there’s one place where the guardrails live, and two front ends:


For non-engineers, this means Data Mage’s data access can sit alongside Atlassian, Google Drive, Figma and the rest of their day-to-day tools, with Anthropic and custom **Skills** becoming easy to layer on top. Access is gated by OAuth, so each user connects with their own identity.


## Lessons learned


A few things we believe more strongly now than we did at the hackathon:


- **Documentation is table stakes.** An LLM analyst is only as good as the data warehouse documentation it can read. The investment in dbt docs, exposures, and curated memory paid off more than any prompt tweak.
- **Guardrails belong in the tools.** Cost limits, read-only access, and a bounded schema surface are deterministic code paths, not prompt suggestions.
- **The bottleneck is ambiguity, not code generation.** Structure beats raw access – a governed metric layer and canonical-table signals moved accuracy far more than giving the model more data ever did.
- **Things are getting transient.** Whole categories of glue code we’d have agonized over a year ago are now cheap enough to throw away and regenerate. Sunk-cost thinking is the trap.


The open question we’re tackling next is the one every team building on LLMs eventually hits: **how do you prove the answers are correct, and keep them correct as the warehouse drifts?** Our next investment is an evaluation harness – a set of golden questions with known answers, run on every change to the prompt or tools – so we can measure accuracy instead of vibing it. Because the failure mode here isn’t a crash. It’s a confident, well-formatted, completely wrong number. And the whole point of the data mage, human or AI, was to be the one you could trust.
