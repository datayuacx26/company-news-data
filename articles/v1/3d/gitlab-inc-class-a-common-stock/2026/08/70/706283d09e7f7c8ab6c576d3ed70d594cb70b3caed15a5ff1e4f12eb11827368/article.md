---
schema_version: "1.0.0"
document_id: "706283d09e7f7c8ab6c576d3ed70d594cb70b3caed15a5ff1e4f12eb11827368"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-53e8505a9d97"
canonical_url: "https://about.gitlab.com/blog/claude-security-and-gitlab/"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T20:17:13.425117+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:39b2e600ec54b15fe83472296357028430026a6a8d5403c46e8b51eabf995c07"
---

# Secure every commit to production with Claude and GitLab

Agentic coding is moving faster than many enterprise governance programs can keep up with. Coding assistants, like the[Claude security guidance plugin](https://code.claude.com/docs/en/security-guidance) and[Claude Security](https://code.claude.com/docs/en/claude-security) , can flag and fix common vulnerabilities in code as it's written, in the same session. This is valuable for writing more secure code, but security doesn't stop there. A commit is one stage in the path to production: Merges, dependency updates, infrastructure changes, and audits happen after the session ends.


GitLab covers securing the remainder of the path to production. There are five handoffs in the typical Anthropic Claude-to-GitLab security workflow. This article walks through each one to illustrate how to govern agentic coding at scale.


Teams already using Claude security guidance and Claude Security can plug that context directly into GitLab through the[GitLab MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/) and keep their existing workflow. Claude handles the moment of authoring; GitLab handles everything from there through production, on one platform.


## From flagged to enforced controls


The Claude security guidance plugin reviews code within a single developer's session, catching issues fast enough to keep an agent moving. Once that code leaves a session, security teams need a record of what happened inside it and controls over what happens next.


GitLab gives teams the visibility and the control to define guardrails for secure coding, before code reaches production, no matter where the code originated.


- **Define controls once, enforce scale.**[Security configuration profiles](https://docs.gitlab.com/user/application_security/configuration/security_configuration_profiles/) apply the scans you require across every project and pipeline from outside the repository, so coverage is consistent and can't be bypassed.
- **Enforce separation of duties, even for agents.**[Merge request approval policies](https://docs.gitlab.com/user/application_security/policies/merge_request_approval_policies/) ensure that the agent that wrote a change can't approve it. An agent, or the developer who prompted it, can't merge its own work without a designated approver.
- **Block critical vulnerabilities before they ship.** Merge request approval policies hold any merge with unresolved critical findings until a named approver signs off, so dismissed or missed vulnerabilities can't reach production quietly.
- **Track every finding's status, permanently.** The[Vulnerability report](https://docs.gitlab.com/user/application_security/vulnerability_report/) and[Security dashboard](https://docs.gitlab.com/user/application_security/security_dashboard/) show whether each finding was detected, dismissed with a reason, or resolved.


*Enable scanners across every in-scope project at scale, with no way to bypass them*


## From scanned in session to audit evidence


Change management audit requirements are expanding to include agents. Compliance frameworks such as SOC 2, PCI DSS, and FedRAMP require documented evidence that every change was tested, reviewed, and approved before it shipped.


GitLab makes compliance controls enforceable and evidence collection automatic for auditors:


- **Prove the scan ran and review.**[Compliance controls](https://docs.gitlab.com/user/compliance/compliance_frameworks/#gitlab-compliance-controls) guarantee a scan runs on every merge request, and every finding surfaces in the merge request and vulnerability report, visible to a human.
- **Answer auditors in minutes.**[Pipeline logs](https://docs.gitlab.com/ci/jobs/job_logs/) ,[approval records](https://docs.gitlab.com/user/project/merge_requests/approvals/) , and[audit events](https://docs.gitlab.com/user/compliance/audit_events/) give you reproducible history of what was scanned and who approved it, change by change, tied to the people and agents involved.
- **Map evidence to the framework your auditor requests.**[Compliance frameworks](https://docs.gitlab.com/user/compliance/compliance_frameworks/#gitlab-compliance-controls) group evidence into named requirements, such as SOC 2 or a custom framework, each built from specific controls, and the[compliance status report](https://docs.gitlab.com/user/compliance/compliance_center/compliance_status_report/) shows which controls passed, are pending, or failed, per framework.


*Audit log of agent activity, showing session-level events and start times*


## Control what sensitive data is sent


You probably send more of your code, and the business context around it, to a model for review than you do a person. Regulated, government, and IP-sensitive teams need to decide what leaves their environment, such as credentials, proprietary logic, and regulated data. That decision has to happen before any scan runs, and across every tool that touches their code.


With GitLab, you decide what data reaches a model before it ever leaves your environment:


- **Keep secrets and sensitive code out of what you send to a model.**[Context exclusions](https://docs.gitlab.com/user/duo_agent_platform/context/) hold secrets and sensitive files back from everything an agent sends to models.
- **Keep code and inference inside your boundary, on models you approve.** Run a self-managed environment with self-hosted models so nothing leaves your environment.[Select the model per flow](https://docs.gitlab.com/user/duo_agent_platform/model_selection/) , restrict which models are permitted, and keep your code out of training.
- **Filter what gets sent.**[GitLab Duo's prompt guardrails](https://docs.gitlab.com/user/gitlab_duo/prompt_guardrails/) scan code suggestions for secrets before they reach a model, and isolate the content a prompt can act on to reduce prompt injection risk, on top of whatever you've already excluded.


*Define what files or directories should be excluded from being sent to AI models*


## From one scan to full scanning coverage across the development lifecycle


Anthropic's[documentation](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/security-guidance) is explicit that the Claude security guidance plugin is a best-effort assistive tool, meant to sit alongside human code review and various security scanners, not replace them. That scoping is important to pay attention to, because some vulnerabilities don't exist at the moment code ships. A dependency you ship today can have a critical vulnerability disclosed against it next year, with no change to your own code.[Log4Shell](https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-356a) is the clearest example; applications that shipped years earlier were suddenly exploitable the moment the vulnerability became public knowledge in December 2021.


Dependencies, container images, infrastructure configuration, and secrets already sitting in your commit history need scanning that runs independently of any single session.


GitLab secures the entire software delivery lifecycle:


- **Cover the whole attack surface.**[Dependency](https://docs.gitlab.com/user/application_security/dependency_scanning/) ,[container](https://docs.gitlab.com/user/application_security/container_scanning/) ,[infrastructure-as-code](https://docs.gitlab.com/user/application_security/iac_scanning/) ,[secret](https://docs.gitlab.com/user/application_security/secret_detection/) , and[dynamic application security testing (DAST)](https://docs.gitlab.com/user/application_security/dast/) scanning check the parts of an application a session-based review never reaches: the dependencies you pull in, the images you ship, the infrastructure you provision, the secrets that leak into commits, and the running app.
- **Catch the flaws that scanners might miss.** Deterministic scanners cannot catch business logic errors, broken authorization, or race conditions.[Security Review Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/security_review/) reasons about intent to catch that category directly, posted as comments on the affected code for a human to act on.
- **Have a deterministic scan the results don't drift on.** LLM-based review could return different findings on the same code from one run to the next. A deterministic scan, such as advanced SAST, traces tainted data across function boundaries using a fixed algorithm and returns reproducible, CWE-mapped results — the consistent evidence a compliance audit needs.


*SAST, DAST, dependency, container, and secret scanning enforced to be run on the pipeline*


## One set of guardrails, for every agent and every developer


The Claude security guidance plugin reviews the code Claude writes and commits inside a session. Commits made from a developer's own shell, including the "!" shell escape inside a session,[fall outside what the plug-in reviews](https://code.claude.com/docs/en/security-guidance) . Claude Security extends that to a full codebase or human-written code, on demand, when a developer or admin runs it.


GitLab’s scan execution and merge request approval policies run on the pipeline for every change, so coverage doesn't depend on whether a human or an agent wrote the code, or whether they remembered to initiate a scan.


*Configure security scans to be run on every default branch*


## Govern what ships


Claude security guidance and Claude Security help developers catch and fix issues the moment code is written. Once that code leaves the session, enterprise security and platform teams are accountable for shipping it securely to production. They need visibility into what an agent did, proof that security procedures were followed, and the ability to stop a problematic change before it ships.


GitLab bridges your Claude workflow. Set guardrails once in GitLab, and every agent and every developer ships faster and more securely inside them. Claude helps write secure code. GitLab governs everything from there to production, without slowing teams down.


> [Start a free trial of GitLab Ultimate!](https://about.gitlab.com/free-trial/?utm_medium=native&utm_source=integrate-market&utm_campaign=eg_global_cmp_content-syndication_security_en_)
>
>
> Already on Ultimate?[Set up scan execution and merge request approval policies](https://docs.gitlab.com/user/application_security/policies/) to start enforcing guardrails today.


## Learn more


- [Claude Code and GitLab: Three workflows that ship](https://about.gitlab.com/blog/claude-code-and-gitlab/)
- [GitLab Duo Agent Platform with Claude accelerates development](https://about.gitlab.com/blog/gitlab-duo-agent-platform-with-claude-accelerates-development/)
- [How to use GitLab's Custom Compliance Frameworks in your DevSecOps environment](https://about.gitlab.com/blog/how-to-use-gitlabs-custom-compliance-frameworks-in-your-devsecops/)


##


Are you trading speed for security?


[Get your security maturity score](https://about.gitlab.com/assessments/security-modernization-assessment/)


Quiz will take 5 minutes or less


## More to explore


[View all blog posts](https://about.gitlab.com/blog/)


[Security GitLab Duo Security Review spots logic flaws scanners miss](https://about.gitlab.com/blog/gitlab-duo-security-review-flow/)[Security When a version bump breaks your build, GitLab fixes it](https://about.gitlab.com/blog/dependency-scanning-auto-remediation/)[Security One vulnerability view: From scanner coverage to AI governance](https://about.gitlab.com/blog/one-vulnerability-view/)


## We want to hear from you


Enjoyed reading this blog post or have questions or feedback? Share your thoughts by creating a new topic in the GitLab community forum.


[Share your feedback](https://forum.gitlab.com/)
