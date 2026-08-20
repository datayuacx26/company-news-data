---
schema_version: "1.0.0"
document_id: "0fd0fdfcd04f1086e9454718fa6cd185303a59ef631aebc3d803d26696cc565e"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/the-runbook-problem-how-aura-documents-what-teams-dont-have-time-to-write"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:bcf503d6a8aeeb036c3665dd6f31b539997c5d4b0f53697d8c3f4794088edf12"
---

# The runbook problem: How AURA documents what teams don’t have time to write

Runbooks are rarely missing because teams don't value them. They're usually missing because incident response, follow-up, and platform work compete for the same limited time. By the time an issue is resolved, the knowledge is fresh, but the window to document it is already closing.


That gap creates familiar failure modes: over-reliance on senior engineers, slower handoffs, and less confidence for whoever is on call next. One practical use case for[AURA,](https://www.mezmo.com/aura) an open-source agentic harness for production AI, is closing that gap by turning incident investigation into draft documentation.


## When a runbook already exits, AURA can use it during investigation


When a runbook exists, AURA can pull it into an active investigation. An agent configured for SRE workflows — like the reference example in the[AURA repo](https://github.com/mezmo/aura) — receives the PagerDuty incident, reads the runbook link from the alert body, and pulls the document from whatever documentation source you've connected. At Mezmo, for example, the sre-agent reads the runbook through the GitHub MCP, then uses the[Mezmo MCP server](https://github.com/mezmo/mezmo-mcp) to investigate logs and system state with that runbook as working context.


The result isn't a generic answer based on model priors. It's an investigation[grounded in the service's actual procedures, dependencies, and operating assumptions,](https://www.mezmo.com/blog/why-we-open-sourced-aura-infrastructure-for-production-ai) which is what makes a recommendation easier to trust under pressure.


More useful, however, is what happens when a runbook doesn't exist yet.


## When a runbook does not exist, AURA can turn the investigation into a draft


Point[AURA](https://github.com/mezmo/aura) at the GitHub location where the runbook should live and define the behavior in the prompt: if no document exists, investigate the issue with the on-call engineer and then based on the actions taken during the incident, open a pull request with a draft runbook.


From there,[AURA pulls telemetry and system context through Mezmo MCP](https://www.mezmo.com/) , assembles a first pass, and opens a PR for review. That draft typically covers incident symptoms, likely root cause, validation steps, remediation guidance, relevant dashboards or queries, and follow-up checks for the next responder: structured enough to review, not polished enough to publish without one.


In practice, that distinction is what makes the workflow useful. An engineer reviews the branch, fills in service-specific nuance, and merges. The only thing left is to update the link in the alert body to point to the new runbook. Documentation that otherwise wouldn't get written now exists as part of the work already being done during investigation.


For existing runbooks, results tend to be stronger. AURA can follow the structure, language, and terminology already in place, which makes updates easier to review and easier to merge.


## Incident response becomes institutional memory


The value isn't just in producing one draft. It's in creating a repeatable loop.


Investigate an issue. Reference the runbook if it exists. Surface what's missing. Update the document. Merge the PR. The next responder starts from a better baseline than the last one did.


That's a more realistic model for documentation maintenance than asking engineers to create and refresh runbooks as a separate project. The work improves as a byproduct of incident response instead of competing with it. Over time, runbooks become something closer to living operational memory instead of an outdated page no one trusts.


## Why this workflow is practical for SRE and Platform teams


The value here is that it changes the job from authorship to review.


Ideally a[SRE](https://www.mezmo.com/roles/site-reliability-engineers) or[platform engineer](https://www.mezmo.com/roles/platform-engineers) shouldn't have to start from a blank page after an incident. Reviewing a draft, correcting edge cases, and approving the final version is a much more realistic ask. AURA handles the repetitive first pass; engineers keep control over judgment, accuracy, and approval.


The goal isn't to auto-publish procedures into production workflows. It's to reduce the friction that keeps useful documentation from existing at all.


Templates help here too. Bringing your own runbook template into the prompt improves consistency across services and raises the quality of the initial draft by giving the system a clearer target structure.


## Runbook generation is one useful pattern, not the whole story


Runbook generation is a good starting point for AURA because the value is immediate: investigation produces something the team can actually reuse. But it's[one use case in a broader set of operational workflows](https://www.mezmo.com/blog/aura-in-practice-real-world-use-cases-for-production-ai-agent-infrastructure) .


### To learn more or get started, check out the following:


[AURA on GitHub](https://github.com/mezmo/aura)[Explore the Quickstart](https://github.com/mezmo/aura/tree/main/examples/quickstart)[Give AURA a star](https://github.com/mezmo/aura)


‍
