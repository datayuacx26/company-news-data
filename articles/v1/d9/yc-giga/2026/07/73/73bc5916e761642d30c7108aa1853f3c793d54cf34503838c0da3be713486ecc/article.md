---
schema_version: "1.0.0"
document_id: "73bc5916e761642d30c7108aa1853f3c793d54cf34503838c0da3be713486ecc"
company_key: "yc-giga"
company: "Giga"
source_id: "yc-giga-news-import-92619addbaaf"
canonical_url: "https://giga.ai/news/cli"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-21T21:36:21.137589+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:d4fcee66320d730400527b1be76e1de9fc573a20d66df5c85e6f527ad42b1634"
---

# Introducing the Giga CLI

Improving a production support agent usually means a lot of context switching: open the dashboard, find the conversation, figure out what went wrong, edit the config, push, wait for the sync, discover a broken reference, and start over. If you spend your day in a terminal, or your coding agent does, that loop is slower than it needs to be.


Today we’re launching the Giga CLI. It’s a single tool,` giga` , that gives you the full Giga platform from the command line: agents, tickets, tests, KPIs, secrets, knowledge base, and deploys. Anything you can do in the console, you can now do from your terminal, a script, a CI job, or a coding agent.


```text
pip install giga-sdk
giga login
```


That’s the whole install. The package ships both the CLI and the Python SDK (` from giga import Agent` ) from one wheel, so the tool you script with and the library you build with stay in lockstep.
