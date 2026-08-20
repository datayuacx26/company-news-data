---
schema_version: "1.0.0"
document_id: "a398f84062a469c4b7dfa892e6b71c7e47885d88ecd8c1677106e7745edb3180"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/docker-ai-governance-audit-logs-now-where-your-security-team-already-works/"
published_at: "2026-08-03T13:00:00+00:00"
first_seen_at: "2026-08-03T13:07:48.193676+00:00"
fetched_at: "2026-08-03T14:10:52.670502+00:00"
content_hash: "sha256:9eba361304ab37b52d0a76238b0592307e0a39676eda33e69a33056c3b74cfbc"
---

# Docker AI Governance: Audit Logs, Now Where Your Security Team Already Works

*Now in Docker AI Governance: a single searchable record of every policy decision your agents trigger, streamed to the SIEM your security team already runs, so you can show what your agents did and what your policy stopped.*


Today, Docker AI Governance now streams every policy decision in your organization into the SIEM your security team already runs, with a searchable record of all of it in Docker Cloud. You can see what your agents did, and what your policy stopped them from doing.


## Enforcement is step one


When we launched[AI Governance in May](https://www.docker.com/blog/docker-ai-governance-unlock-agent-autonomy-safely/) , our perspective was that controls have to live at the runtime layer where the agent actually executes, not as advisory rules a clever prompt can route around. Audit was one of the three layers we shipped on that principle, and the enforcement point has produced a structured event for every policy evaluation since day one.


**Today, we’re making it easier to view and consume those events.**


## Why audit records matter


Security leads need to answer questions about agent behavior: what did that agent do, was it allowed, and which policy made the call.


Answering it should not require assembling evidence from machines they don’t administer. It should mean querying a system they already use. Increasingly it also comes first rather than after: security teams want a demonstrable audit record before they approve agent deployment at all.


## What only the enforcement point can see


A policy decision has three outcomes. The action was allowed, it was denied, or it was held for a human.


A log collector can reconstruct the first one. Nothing outside the enforcement point can see the other two. A collector reads what an agent produced, so it never sees the tool call that was refused, the domain that was unreachable, or the credential that was requested and withheld. Those events leave no trace in output, because the process that would have produced the output never ran.


That is the difference between a record generated at the point of decision and logs gathered after the fact. A record of allowed actions shows that agents are active. A record of denials shows whether your controls are doing anything.


## Audit Logs: Streamable to your SIEM tools


Audit logs are now available in Docker Cloud, and audit events can stream directly to your SIEM. Both are included with Docker AI Governance.


**Audit logs in Docker Cloud.** One searchable view for the whole organization, with 90 day retention and CSV export. Local disk delivery keeps working, and both modes can run at once.


**Native SIEM streaming.** Point Docker at your endpoint and forward audit records to the tools you already run, including Splunk, Datadog, Dynatrace, and Grafana.


## Coverage


Records cover Docker Sandboxes policy decisions and sandbox session events, for users with an AI Governance license under an enforced organization policy. Other source records (MCP Gateway enforcement decisions, for example) will share records through the same schema as they become available, so coverage will expand without extra integrations on your end.


Records are metadata only. They never contain your prompt content, agent output, or parameter values.


## What’s next


Records are step one.


Once every decision an organization makes about its agents lands in one place, the useful question stops being what happened and starts being what should change. That is the direction we’re building toward: a system that tells you when something is off and what to do about it. More on that soon.


## Available today


Audit logs are live for organizations on Docker AI Governance with an enforced organization policy. Read more[here](https://docs.docker.com/ai/sandboxes/governance/audit/) .
