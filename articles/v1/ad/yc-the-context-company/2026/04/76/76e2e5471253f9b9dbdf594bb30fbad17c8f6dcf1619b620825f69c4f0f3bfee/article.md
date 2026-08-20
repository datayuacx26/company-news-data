---
schema_version: "1.0.0"
document_id: "76e2e5471253f9b9dbdf594bb30fbad17c8f6dcf1619b620825f69c4f0f3bfee"
company_key: "yc-the-context-company"
company: "The Context Company"
source_id: "yc-the-context-company-rss-63dbe5673379"
canonical_url: "https://www.thecontextcompany.com/blog/introducing-insight-search"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-26T02:09:01.567080+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:91cca4a36f7ee51856380fc46e7a92a8be6d7dd34d30374c2abec6fb87e67af0"
---

# Introducing Insight Search

Today we are introducing Insight Search.


You can ask questions in plain English across your production agent data and get answers grounded in the sessions, traces, topics, costs, feedback, users, and organizations behind them.


Most teams start with the question they actually care about.


Where are users getting stuck? Which workflows are creating the most failures? What changed after the last release? Why did cost spike? Which accounts are showing signs of churn risk? Which production examples should become evals?


Those answers usually cross several systems. Insight Search makes that production context queryable.


## What good questions look like


The best Insight Search questions are specific about the decision the team needs to make.


Good questions sound like this:


- Which production failures increased after our last release?
- What are users repeatedly asking the agent to do that it cannot complete?
- Which accounts saw unresolved conversations this week?
- Why did cost increase for this workflow?
- Which runs should we turn into evals?


The answer should narrow the work. It should show representative examples, preserve links to the source sessions, and make the next owner easier to identify. If it says cost moved, the runs and model calls should be there. If it says a tool path is failing, the trace should be there.


## Insight Search through MCP and API


A clear product signal: Insight Search gets more powerful when it leaves the dashboard.


That matches the workflow. Teams are using[The Context Company MCP and API server](https://www.thecontextcompany.com/blog/tcc-mcp-and-api-server) to bring production context into the same place where they fix agents.


A coding agent can ask what failed in production before touching code. It can pull representative sessions and traces, inspect cost paths, understand what customers are asking for, and propose PRs that reduce cost or fix recurring failures.


That is the kind of production context we want teams to be able to pull into the systems where they actually make changes.


This is the same belief behind our[Monitoring for Production AI Agents](https://www.thecontextcompany.com/blog/monitoring-for-production-ai-agents) launch: production behavior should change what the team does next.


## A step toward agent adaptivity


This workflow is one of our first major steps toward[agent adaptivity](https://www.thecontextcompany.com/blog/ai-agent-observability-is-insufficient#the-next-phase-agent-adaptivity) .


Agent adaptivity means production failures and customer behavior can feed back into the systems that improve agents. Sometimes a human makes the change. Increasingly, a coding agent helps investigate, propose the fix, and check production behavior afterward.


That is why Insight Search matters to us. It lets teams cross-reference agent runs, traces, sessions, topics, costs, users, organizations, and feedback from one question. It surfaces the signals that decide whether an agent is improving or quietly creating churn risk.


The dashboard is still useful when a team wants to inspect. Insight Search is for the moment before that, when someone knows the production question but does not yet know where the answer lives.
