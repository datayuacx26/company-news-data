---
schema_version: "1.0.0"
document_id: "15b9ad2e0a974945e906a3457d8d7870ad268306e6e5aab437c1cb5b37d2b31e"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/open-source-analytics-agent-launch/"
published_at: "2026-02-10T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T22:21:12.159869+00:00"
content_hash: "sha256:f86814841737bf811968621e2d21f0708f14bb913a34eb5a650cc0ecb003987b"
---

# We're launching the first Open Source Analytics Agent Builder

Every data team we talked to has analytics agents on their 2026 roadmap.


Few of them have one in production.


We've spent a long time thinking about why. And we think the answer is simple: current solutions are black boxes. You connect a warehouse, plug in an LLM, add some prompt magic, and hope the answers are right.


When they're wrong — and they often are — you're left guessing:


- Did the agent miss a definition?
- Ignore your dbt docs?
- Invent a metric?


There's no way to know. No way to fix it systematically. No way to measure improvement over time.


That's the reliability problem. And it won't be solved by a better model or a prettier UI.


## The problem is context, not capability


We benchmarked analytics agents across the market — Snowflake Cortex, Databricks Genie, Omni, Dust, Hex, Claude + MCPs, and more.


We asked each one the same question about churn rate. Most got it wrong.


Not because the models are bad. Because they didn't have the right context: which tables to join, how we define a churned user, that the churn rate needs to be compared to the prior month.


The agents that got it right weren't the ones with the fanciest UI or the biggest model. They were the ones where we could directly control what context the agent sees. We cover the full benchmark methodology in[AI Data Agents Compared](https://getnao.io/blog/ai-data-agents-compared) .


That's the insight behind nao.


## What we're building


Today, we're open sourcing **nao** — an analytics agent framework built around one core idea: **context engineering** .


Instead of a black-box, one-size-fits-all solution, nao gives data teams the tools to decide exactly what goes into their agent's context. It's a radically different approach:


- **No vendor lock-in.** An open framework your team owns and controls.
- **A file-system approach.** Context lives in files — markdown definitions, YAML configs, dbt references, rules, example queries. Your agent can retrieve them fast. Your team can review them in a pull request.
- **A context engineering tool.** Test different contexts, measure performance, and continuously improve your agent's reliability.
- **A deployable UI.** Once your context is ready, deploy a chat interface for anyone at the company to ask questions on your data.


Two components. One framework.


**nao CLI** — build the exact context you want for your agent. Metadata, data sampling, business logic, dbt lineage, rules. Whatever works best for your company.


**nao UI** — deploy a chat interface for your stakeholders. Business users get natural-language analytics. Data teams stay in control.


## Why open source


Context engineering is a new discipline. Nobody has all the answers yet — not us, not Snowflake, not anyone.


The best way to structure context files, the right evaluation metrics for each domain, the optimal orchestration patterns — all of this needs to be discovered through real-world usage and shared learning.


That won't happen inside closed products.


We want nao to become a foundation for context engineering — a framework for data teams to discover and share how to build the best AI agents for analytics. Read the full reasoning in[Why we're making our Analytics Agent open source](https://getnao.io/blog/why-open-source-analytics-agent) and[Why data teams need an open framework for context engineering](https://getnao.io/blog/open-framework-context-engineering) . The same way dbt, Airflow, and the modern data stack were built: in the open, by the community that uses them.


Teams running nao in production should contribute what they learn back to a shared foundation. Not reinvent it behind closed doors.


## Our vision


**Today** : companies build analytics agents with black-box solutions, limited context control, and vendor lock-in. They improve agents blindly and hesitate to put them in front of business users.


**Tomorrow** : data teams engineer reliable agent context — versioned, measurable, continuously improved. They deploy agents with confidence. They share context engineering knowledge as a community.


We're not building another chatbot.


We're building the foundation for data teams to own the intelligence behind their analytics agents.


If you want to start now:


- Check out[nao on GitHub](https://github.com/getnao/nao)
- Read the[Context Engineering Playbook](https://docs.getnao.io/nao-agent/context-engineering/playbook)
- [Join our Slack community](https://join.slack.com/t/naolabs/shared_invite/zt-4080jlm79-nm52x5nZhG2N1ben8zwpiQ) and tell us what you're building


Let's build this together.
