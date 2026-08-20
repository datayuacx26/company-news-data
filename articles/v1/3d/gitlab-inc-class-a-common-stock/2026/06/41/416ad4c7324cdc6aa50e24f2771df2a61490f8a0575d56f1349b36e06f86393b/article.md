---
schema_version: "1.0.0"
document_id: "416ad4c7324cdc6aa50e24f2771df2a61490f8a0575d56f1349b36e06f86393b"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/one-vulnerability-view/"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:48:41.890548+00:00"
content_hash: "sha256:e19ea61ff388161ebdd74b4eb266ef301c6e52d643803bf0cf494e963aabfaef"
---

# One vulnerability view: From scanner coverage to AI governance

Most enterprises use a handful of different security scanners, each configured and enforced, project by project. With no single view of what scanners run where, policies drift, blind spots go undetected, and important projects could silently go unprotected. With[GitLab 19.1](https://docs.gitlab.com/releases/19/gitlab-19-1-released/) , you can now integrate the security scanners you already use, giving a single view of your scanner coverage. GitLab enforces third-party scanners at scale across all of your projects, and the vulnerabilities they detect get remediated automatically. On the governance side, we're launching the beta of AI audit event streaming, so you can see whether your agents are acting safely.


## Enforce third-party scanners on every project at scale


For most security teams, the hardest part of application security is scanner coverage. Different scanners are set up project by project, so whether a scanner runs depends on individual teams setting it up. New projects can go unnoticed and can ship for weeks before teams realize they are not scanned. When coverage depends on tribal knowledge rather than policy, code ships unscanned, vulnerabilities ship to production, and audits expose gaps.


You can now enforce third-party scanners at scale across all of your GitLab projects. Any scanner that[outputs SARIF](https://docs.gitlab.com/user/application_security/detect/sarif/) runs under your policies, and the vulnerabilities identified flow into GitLab natively. Every finding lands in one vulnerability view governed by the same rules, so coverage becomes something you can prove rather than hope for.


From there, third-party scanner findings run through the same GitLab Duo Agent Platform auto-remediation workflow as GitLab native scanner findings.[SAST False Positive Detection](https://docs.gitlab.com/user/application_security/vulnerabilities/false_positive_detection/) triages findings to prioritize those with real risk, and[Agentic SAST Vulnerability Resolution](https://docs.gitlab.com/user/application_security/vulnerabilities/agentic_vulnerability_resolution/) opens a ready-to-merge fix to automatically remediate findings before they go into production. Your team gets coverage it can prove with one governed view across every scanner, and automated remediation for third-party findings.


## Catch secrets earlier, and spend less time on false positives


Secret detection runs in your pipelines to catch leaked credentials, but teams have historically struggled with two things: missed secrets and noisy findings. On a new branch, only the latest commit gets scanned, so a secret committed earlier might ship unnoticed. The findings detected come mixed with test credentials, placeholder values, and example tokens, so developers spend time clearing noise instead of addressing real exposures.


[Secret detection](https://docs.gitlab.com/user/application_security/secret_detection/pipeline/#coverage) now scans every commit on a new branch instead of only the latest one, and[Secret False Positive Detection](https://docs.gitlab.com/user/application_security/vulnerabilities/secret_false_positive_detection/) , now generally available, adds a confidence score and an explanation to each finding, shown in the vulnerability report. Your team catches secrets wherever they were introduced, and spends time reducing risk from real exposures rather than false positives.


## Decide what your AI agents can do, and prove it


Companies have adopted AI agents for coding. Agents open merge requests, call tools, and commit code alongside the developers they work for. However, once an agent is approved for a project, it can write, delete, and push without anyone reviewing the action first. Your company remains accountable for changes in the codebase, regardless of whether an agent makes them or a developer. Enterprises need to determine what an agent is allowed to do before it acts, and to show exactly what it did after.


GitLab 19.1 closes that governance gap. With[AI audit event streaming](https://docs.gitlab.com/administration/compliance/audit_event_streaming/#ai-audit-event-streaming) , now in beta, every action an agent takes is recorded as an audit event and streamed to your audit log destinations, with the rest of your audit trail. The release also gives you control over what agents can do on your platform.[Agent tool approval guardrails](https://docs.gitlab.com/user/duo_agent_platform/) , also in beta, let an administrator set each agent tool to run on its own, pause for human approval, or stay blocked, so a sensitive action like writing a file or deleting a resource waits for a team reviewer before it runs. Every approval decision is recorded as an audit event for teams to retroactively review.


The result is governed autonomy. Agents can run end to end, inside the guardrails you set, and a risky action does not reach the codebase unless a person signs off on it. When an auditor or an incident responder later asks what an agent did, the answer is already in the audit trail the team runs.


*Audit trail of agent activity showing an alert flagged for an agent dismissing a high-severity finding without human approval*


## Governed autonomy for your agents


GitLab 19.1 puts governance around the agents in your codebase, with full security scanner coverage across every project and automatic remediation of third-party scanners. You set what each agent is allowed to do before it acts, and every action lands in your audit trail.


> To see what your agents can do inside the guardrails you set, and prove what they did,[start a free trial of GitLab Duo Agent Platform today](https://about.gitlab.com/gitlab-duo-agent-platform/?utm_medium=blog&utm_source=blog&utm_campaign=eg_global_x_x_security_en_) .


##


Are you trading speed for security?


[Get your security maturity score](https://about.gitlab.com/assessments/security-modernization-assessment/)


Quiz will take 5 minutes or less
