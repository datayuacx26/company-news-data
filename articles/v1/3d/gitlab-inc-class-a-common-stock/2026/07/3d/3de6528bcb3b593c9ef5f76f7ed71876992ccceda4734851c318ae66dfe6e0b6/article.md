---
schema_version: "1.0.0"
document_id: "3de6528bcb3b593c9ef5f76f7ed71876992ccceda4734851c318ae66dfe6e0b6"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-8618b9f35184"
canonical_url: "https://about.gitlab.com/press/releases/2026-07-16-gitlab-19-2-brings-governed-agentic-automation/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-25T06:37:26.217349+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:4d56f7af21130b8d68459490f510f9ffc5f7b838cda832d068924c42c3829633"
---

# GitLab 19.2 Brings Governed Agentic Automation to Clear the Backlog AI Coding Creates

# GitLab 19.2 Brings Governed Agentic Automation to Clear the Backlog AI Coding Creates


---


- Dependency Scanning Auto-Remediation, now in public beta, shrinks the security backlog without diverting developers to upgrade vulnerable dependencies and fixing breaking changes in new code.
- Security Review Flow, now in public beta, catches security flaws like business-logic errors and race conditions that pattern-based scanners cannot see.
- GitLab Duo CLI, now generally available, gives developers access to agents and multi-step agentic flows for all software lifecycle tasks right where they work.
- Custom Flows, now generally available, let teams replace manual multi-step workflows with agentic automations for software development, triggered by GitLab events.


SAN FRANCISCO, July 16, 2026 (All Remote), GitLab Inc., the intelligent orchestration platform for DevSecOps, today released GitLab 19.2. As AI generates more code, dependencies, and change than developers can keep up with, GitLab 19.2 brings agentic automation to clear that load.


Developers can now use GitLab to fix vulnerable dependencies automatically, catch the logic flaws scanners miss, create custom agentic workflows, invoke agents from more surfaces they already use, and always do so under the organization’s existing controls. A Forrester Consulting study commissioned by GitLab found organizations using GitLab Duo Agent Platform can achieve 400% return on investment with payback in under six months.


**Dependency Scanning Auto-Remediation, Now in Public Beta, Helps Fix Vulnerable Dependencies Automatically**


A growing share of application security risk now comes from dependencies teams never chose directly. A study of the Maven ecosystem found vulnerabilities reaching roughly 63% of latest releases through transitive dependencies, and roughly one in eight dependency updates introduces a breaking change, even as compliance deadlines under PCI DSS and FedRAMP keep running.


[Dependency Scanning Auto-Remediation](https://about.gitlab.com/blog/dependency-scanning-auto-remediation/) , now in public beta, closes that gap. Security teams can now clear vulnerable dependencies without adding work for developers. When a scan finds a vulnerable package, GitLab opens a merge request with the suggested fix. If an upgrade breaks the build, agents iterate to fix the issue in the same merge request. New configuration controls let developers set the severity thresholds and version scope that remediation applies to. Every change stops at existing approval gates and leaves a full audit trail.


**Security Review Flow, Now in Public Beta, Brings Security Judgment to Every Merge Request**


Developers can now catch a class of vulnerabilities that pattern-based scanners structurally cannot see, on every merge request, when a fix is cheapest. Static scanners are good at identifying flaws that match a known pattern, but application-logic flaws have needed manual review that cannot scale, or penetration testing that arrives too late.


[Security Review Flow](https://about.gitlab.com/blog/gitlab-duo-security-review-flow/) , now in public beta, is a foundational flow in GitLab Duo Agent Platform. It reasons about what the code is meant to do rather than matching known patterns, and detects broken object-level and function-level authorization, missing authorization on state-changing operations, information disclosure, mass assignment, business logic errors, and race conditions. Findings include severity and a suggested fix where available. The flow never approves on its own; a person always makes the final call.


**GitLab Duo CLI, Now Generally Available, Puts Agents in Every Developer's Terminal**


Developers do much of their work in the terminal, where AI assistance has usually meant reaching for tools that lack context on their GitLab projects, pipelines, and agent configurations. GitLab 19.2 closes that gap.


[GitLab Duo CLI](https://about.gitlab.com/blog/gitlab-duo-cli-generally-available/) , now generally available across GitLab.com, Self-Managed, and Dedicated deployments, brings GitLab Duo Agent Platform's agents to the terminal with full project context. A developer can get Administrators control rollout across the organization.


**Agentic Flows Extend Automation From the Individual to the Whole Team**


GitLab Duo Agent Platform's[agentic flows](https://about.gitlab.com/blog/multi-step-software-delivery-with-agentic-flows/) are sequences that chain agents to complete multi step work. In 19.2, they advance on two fronts.


Custom Flows, the flows teams build themselves, are now generally available. Build a flow once and it runs automatically on GitLab events. Custom Flows now authenticate to external services with short-lived, job-scoped tokens, so automation reaching cloud providers or internal APIs uses the same keyless pattern GitLab CI/CD pipelines already trust.


The upcoming Flow Creation Agent can turn a natural-language description into a custom flow. GitLab's foundational flows, the ones GitLab ships ready to use, also get more capable. The Fix CI/CD Pipeline Flow, now improved, classifies failures before acting and delivers targeted fixes as inline suggestions or a new merge request. GitLab Duo Agentic Chat can now delegate multi-step work to agents.


**Controls That Keep the Automation Trustworthy**


The point of automating this work is so that teams can trust it to run autonomously. GitLab 19.2 adds the controls that make that safe at scale. The AI Audit Event Report, now in beta, records AI-assisted actions as dedicated audit events, so compliance and security teams can include AI workflows in audit reporting, access reviews, and incident investigation.


Group-level custom instructions for GitLab Duo Code Review let administrators set review behavior across projects at once, and new MCP access controls govern which agents can run and what they can reach.


To learn more, please read the[what's new page](https://about.gitlab.com/whats-new/) .


**Supporting Quote**


- "Coding agents made it possible to generate far more code and moved the bottleneck downstream to reviews and security," said Manav Khurana, chief product and marketing officer at GitLab. "GitLab 19.2 puts agents to work on that bottleneck: fixing vulnerable dependencies, catching the flaws scanners miss, and automating the steps in between with a person still approving what ships."


## About GitLab


GitLab is the intelligent orchestration platform for DevSecOps. GitLab enables organizations to increase developer productivity, improve operational efficiency, reduce security and compliance risk, and accelerate digital transformation. More than 50 million registered users and approximately 50% of the Fortune 100* trust GitLab to ship better, more secure software faster.


**Fortune 500® is a registered trademark of Fortune Media IP Limited, used under license. Claim based on GitLab data. Fortune 100 refers to the top 20% ranked companies in the 2025 Fortune 500 list, published in June 2025. Fortune and Fortune Media IP Limited are not affiliated with, and do not endorse products or services of GitLab.*


## Contacts


**Media Contact** GitLab
[\[email protected\]](https://about.gitlab.com/cdn-cgi/l/email-protection#75050710060635121c011914175b161a18)


[\[email protected\]](https://about.gitlab.com/cdn-cgi/l/email-protection#ea9a988f9999aa8d839e868b88c4898587)


##


Start building faster today


See what your team can do with the intelligent orchestration platform for DevSecOps.


[Get your free trial](https://gitlab.com/-/trial_registrations/new?glm_content=default-saas-trial&glm_source=about.gitlab.com/press/releases/2026-07-16-gitlab-19-2-brings-governed-agentic-automation)[Contact sales](https://about.gitlab.com/sales/)
