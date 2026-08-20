---
schema_version: "1.0.0"
document_id: "3dcfdd9b526e457773a5111cc3a885ad7688438ce4549aae7671fb8d5f145b2c"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/context-layer"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:641649cd3f3502fbb63427c432d2c1e3b77826b8b639c3b9f72dd0975c8e4bae"
---

# Your data team is already building a context layer. They just don't call it that.

[← Back to all posts](https://deepnote.com/blog)


AI agents are failing because they lack business context. The industry is scrambling to build "context layers" as a fix. But if your team works in notebooks, the context layer already exists - inside the analytical work itself.


## **Agents fail without context**


Over the past year, organizations have tried to put AI agents on top of their data stacks. Chat-with-your-data bots, automated analysts, agentic workflows. Most hit the same wall.


The models were capable enough. The data was accessible. What was missing was the layer between raw data and useful answers - the business definitions, tribal knowledge, governance rules, and analytical reasoning that an experienced analyst carries in their head but no system captures.


Ask an agent "what was revenue growth last quarter?" and it immediately needs answers to a dozen sub-questions: Is this ARR or run-rate? Which fiscal calendar? Are the new product lines included? Which table is canonical -` fct_revenue` or` mv_revenue_monthly` ? Without this context, the agent either guesses (confidently wrong) or stalls.


> Gartner's 2026 D&A Summit quantified the gap: only 1 in 5 AI investments currently show measurable ROI. Their finding is that the organizations getting the most from AI invest nearly 2x more in foundations - data quality, governance, semantics - than in AI tools themselves. The bottleneck is not models. It is context.


## **What a context layer actually is**


The term "context layer" has gained traction quickly - sometimes called a context engine, contextual data layer, or ontology. Strip away the naming, and the concept breaks into three pillars:


The three pillars of a context layer: what things mean, what's happening, and what happened. An effective context layer captures all three.


A traditional semantic layer is a subset of this - metric definitions in YAML or LookML. A real context layer is broader. It encodes how the business works, tracks what changed, records agent behavior and reasoning, and makes all of this consumable by both humans and machines.


For years, this knowledge lived in people's heads, scattered across Slack threads, stale wiki pages, and one-off SQL queries. That was fine when only humans needed it. It is not fine when agents do.


## **Why context that lives inside the work actually works**


The history of standalone semantic layers and metrics layers offers a clear lesson for anyone evaluating context layer solutions: if context has to be built and maintained as a separate system, it will not get built and maintained.


This is not a vendor problem. It is an operational one. Data teams are already stretched. Adding a dedicated context-construction workflow - writing YAML, populating a knowledge graph, keeping definitions in sync with the actual analysis - means asking people to do the same work twice: once to get the answer, and again to document how they got it. In practice, the documentation step gets skipped, deferred, or done once and never updated. The context layer goes stale within weeks.


The alternative is an environment where context is a byproduct of the analytical work itself. When your team writes a revenue definition in a notebook, that notebook *is* the semantic record. When they document why they chose one data source over another, that *is* the tribal knowledge capture. When they set permissions and freeze a snapshot, that *is* governance and traceability. Nothing extra to maintain - the work and its context are the same artifact.


For teams deploying agents, this distinction is practical: your agent's context is only as good as the last time it was updated. If that update is a separate task, it will lag. If it happens automatically because the team's daily work produces it, it stays current. And with[scheduled runs](https://deepnote.com/docs/scheduling) , notebooks re-execute on a cadence - so even context that depends on fresh data stays up to date without anyone manually triggering it.


## **Deepnote is the context layer**


Deepnote is an AI workspace - notebooks, SQL, Python, visualizations - where data teams and their AI agents collaborate on analytical work. That work produces the context layer. When a team writes a revenue definition in a Deepnote notebook, documents a data source decision, or publishes a reusable module, they are constructing exactly the kind of governed, human-curated context that agents need - without a second workflow.


Deepnote connects to data sources and MCP servers, and serves governed context to agents, tools, teams, and workflows above.


**The Deepnote context layer, decomposed**


Here are the features of the Deepnote context layer broken down by pillar. These are shipping capabilities, not roadmap items.


**Pillar** **What agents need** **Deepnote feature** **How it works**


**Semantics** Metric definitions and business rules Notebooks +[Modules](https://deepnote.com/docs/modules) Plain-English definitions live alongside executable code. Modules let teams publish reusable metric libraries that other projects import.


**Semantics** Business logic with reasoning Notebooks (Markdown + code) Narrative context ("why we define churn this way") and computational logic live in the same document. An agent can read both.


**Governance** Ownership and access policies[Permissions](https://deepnote.com/docs/team-permissions) ,[secured connections](https://deepnote.com/docs/securing-connections) Role-based access at project, notebook, and connection level. Controls who can see, edit, execute, and share.


**Operational** Canonical entities Notebooks as versioned assets Each notebook is an addressable, versioned, permissioned entity - a structured unit of analytical work.


**Operational** Activity tracking Version history, audit logs Every edit, execution, and collaboration event is recorded. Full activity log for any asset.


**Operational** Environment and connections Machine types, MCP connections, Deepnote CLI Compute environments, MCP server integrations, and CLI-based programmatic access are part of the context surface. Agents and automation connect through the same interfaces humans use.


**Operational** Automated freshness[Scheduling](https://deepnote.com/docs/scheduling) Notebooks run on a schedule - hourly, daily, weekly. Context stays current automatically, not just when someone remembers to re-run the analysis.


**Traceability** Reproducibility and audit trail Snapshots Point-in-time captures of data + code + results together. Reproducible, auditable, shareable.


**Traceability** Agent observability Agent traces Full record of what the agent did, which context it consumed, which tools it called, and what it returned. Traceable reasoning from input to output.


## **Building your context layer in Deepnote**


If your team already uses Deepnote, you are further along than you think. The steps below formalize what many teams already do informally:


### **1. Codify your canonical definitions**


Create a[module](https://deepnote.com/docs/modules) for your core business metrics - revenue, churn, active users, LTV, whatever your organization's key measures are. Write the definitions in Markdown (the "why") and the computation in code (the "how"). Other notebooks import the module; agents read it for context.


### **2. Use notebooks as systems of record**


For recurring analyses - monthly reporting, quarterly business reviews, ad hoc investigations that become canonical - treat the notebook as the authoritative record. It includes the question, the reasoning, the code, the result, and the decision.[Schedule it](https://deepnote.com/docs/scheduling) to re-run on a cadence so the context stays fresh automatically. Snapshots freeze point-in-time versions for audit and comparison.


### **3. Govern access to where the work lives**


Use[team permissions](https://deepnote.com/docs/team-permissions) to control who can see and modify what. When an agent queries Deepnote for context, it inherits these same access rules - no separate governance product needed.


### **4. Expose context to agents and automation**


Connect external agents and workflows via MCP or the Deepnote CLI. The same notebooks and modules your team writes become the context that agents programmatically consume. One asset, two audiences.


## **The bottom line**


Every organization deploying AI agents will need a context layer. The question is whether you build it as a separate system - expensive, fragile, prone to staleness - or recognize that the place where your team already defines metrics, documents reasoning, and governs analytical work *is* the context layer.


Deepnote is not a notebook with some context features. It is the context layer that happens to have a notebook interface. The notebook is how humans author context. The platform is how everyone - and everything - consumes it.


Jakub Jurovych


CEO @ Deepnote


Follow Jakub on[LinkedIn](https://www.linkedin.com/in/jakubjurovych/)
