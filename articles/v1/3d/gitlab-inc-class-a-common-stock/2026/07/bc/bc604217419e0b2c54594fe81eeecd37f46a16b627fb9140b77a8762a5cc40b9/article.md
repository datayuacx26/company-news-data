---
schema_version: "1.0.0"
document_id: "bc604217419e0b2c54594fe81eeecd37f46a16b627fb9140b77a8762a5cc40b9"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/govern-agentic-ai-mcps-code-assistants/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T17:04:31.046224+00:00"
fetched_at: "2026-07-31T17:04:32.779795+00:00"
content_hash: "sha256:5d83bd335eff6207aeb9eea3342e306ac07eb367ffd100fc6d86afcd55bdebe7"
---

# How to govern agentic AI, MCPs, and AI code assistants

AI code completion built human review into the process by design. A developer types, a suggestion appears, and a human decides whether to accept it. A person looked at every line before it shipped.


Agentic AI breaks that review loop. An agent can open a merge request, call a tool, modify a CI/CD configuration, and push a change, sometimes without a person reviewing each individual step. Add in the[Model Context Protocol (MCP)](https://about.gitlab.com/topics/ai/model-context-protocol/) , which lets agents connect to external tools and data sources on their own, and the question engineering leaders are asking shifts. It is no longer "which model writes the best code?" It is "what is this agent allowed to do, and how can we prove what it did?"


[GitLab's own research](https://about.gitlab.com/resources/ai-accountability-survey-2026/) across more than 1,500 developers and technology leaders found that 73% of respondents are concerned about the long-term maintainability of code and 86% agree that without clear governance, AI-generated code can compound technical debt faster than traditional development practice.


In this article, you'll learn how to proactively address some of the challenges organizations are starting to see with governing agentic AI. We'll introduce you to a governance framework for agentic AI in software development and explore what to control, where human review still belongs, how to measure a rollout, and a practical checklist for teams standardizing on[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) .


## Why agentic AI needs a different approach to governance


The security model for an interactive coding assistant is straightforward because a human is in the loop at every step: a developer asks a question, reviews a suggestion, and accepts or rejects it.


[Agentic AI](https://about.gitlab.com/topics/agentic-ai/) in automated workflows requires a different approach to governance. When an agent can run tests, modify configurations, and take[multi-step actions across the software delivery lifecycle](https://about.gitlab.com/blog/multi-step-software-delivery-with-agentic-flows/) without a human reviewing each step, the relevant questions change to:


- What can this agent access?
- What is it authorized to do?
- What actions did it take, and can that be proven after the fact?


Most teams are already feeling this challenge.[Ninety-two percent of DevSecOps professionals](https://about.gitlab.com/resources/ai-accountability-survey-2026/) report some governance challenge with AI-generated code, and the specific concerns point directly to the questions above.


The top concerns include:


- **Code attribution:** The ability to tell which code was AI-generated versus human-written in the first place.
- **Traceability to intent:** Connect AI-generated code back to the business requirement it was meant to satisfy.
- **Documentation that scales:** Manual documentation practices don't hold up once agents are generating a growing share of the codebase.


Once an agent is approved for a project, it can typically write, delete, and push changes without someone reviewing the action before it happens. However, you are still accountable for what lands in the codebase regardless of whether a person or an agent made the change.


**Governance for agentic AI is not an add-on to code completion governance. It is a different approach, built around identity, permissions, and auditability.**


## Setting controls for MCPs, agents, model access, and tool permissions


Once agents can call tools and connect to external systems through protocols like MCP, permissioning becomes the control point. A useful governance model answers three questions before any agent runs:


1. Which agents and flows are allowed?
2. Where are they allowed to operate?
3. Which models can the agents use?


In practice, this looks like a few layers working together:


- [A central catalog for agents and flows](https://about.gitlab.com/blog/ai-catalog-discover-and-share-agents/) : Rather than every team standing up its own agent integrations, a shared AI catalog lets administrators decide what gets published, aligned to the organization's existing roles and group structure.
- [Composite identity](https://about.gitlab.com/blog/improve-ai-security-in-gitlab-with-composite-identities/) **:** Every AI agent's identity should be linked to the human user who requested the action, so activity is never attributable to the agent alone. When an agent tries to access a resource, both principals, the agent and the human who instructed it, need to be authenticated and authorized before access is granted.
- [Tool approval guardrails](https://docs.gitlab.com/user/duo_agent_platform/agents/tool-governance/) **:** Individual agent tools can be set to run autonomously, pause for a human reviewer, or stay blocked outright, so a sensitive action like writing a file or deleting a resource waits for sign-off before it executes.
- [Prompt guardrails](https://about.gitlab.com/blog/how-gitlab-uses-prompt-guardrails-to-help-protect-customers/) **:** Because agents can process untrusted input (a webpage, an issue comment, a file an attacker controls), the platform needs to detect attempts to hijack agent behavior mid-workflow, not just log it after the fact.


The goal is a control plane that treats agent permissions the same way an organization already treats human permissions: role-based, auditable, and consistent across every project.


## Data privacy and self-hosted AI: The questions worth asking


Source code is one of the most sensitive assets an enterprise has, and every AI feature that reads it raises a data-handling question. Before rolling out agentic AI broadly, engineering and security leaders typically want clear answers to a short list of questions:


- Does the vendor train models on our code?
- Who owns the inputs and outputs?
- Where do our subprocessors sit, and can we be notified when that list changes?


For organizations in regulated industries, the answer to "where do our subprocessors sit" often needs to be "nowhere outside our own infrastructure." That is why self-hosting matters as a governance lever, not just a deployment preference.[Self-hosted options](https://about.gitlab.com/blog/agentic-ai-enterprise-control-self-hosted-duo-agent-platform-and-byom/) let a team run its AI agents entirely on infrastructure it controls, while still accurately tracking team usage and satisfying regulators.


Bring-your-own-model support extends that further, letting administrators connect models they have already validated internally and map specific models to certain agent flows. With this method, a sensitive workflow can be pinned to a model the organization trusts while less sensitive workflows use a managed option.


## Human-in-the-loop: Decide where review still belongs


Governance does not mean blocking agents from acting autonomously. It means deciding, deliberately, where autonomy ends and review begins. A workable policy usually separates two modes:


- **Interactive work** , where a developer is present, sees each suggestion, and approves or rejects it directly. This is the mode most teams already understand from[AI code completion](https://about.gitlab.com/topics/devops/ai-powered-code-completion/) .
- **Automated or headless work** , such as agents running inside CI/CD pipelines without a developer watching in real time. Here, the review has to happen either before the action (through tool approval guardrails) or immediately after (through an[audit trail](https://about.gitlab.com/blog/one-vulnerability-view/) a human can inspect).


### Key decision points worth defining


Code review, testing and validation, and deployment approval should all be defined by answering questions like:


- Which of these can an agent complete unassisted?
- Which require a named human sign-off before the change proceeds?


In practice, each checkpoint needs its own enforcement mechanism. This could look like:


- [Merge request approval policies](https://docs.gitlab.com/user/application_security/policies/merge_request_approval_policies/) determine who has to sign off before a merge request lands, regardless of whether an agent or a developer opened it.
- [Tool approval guardrails](https://docs.gitlab.com/user/duo_agent_platform/agents/tool-governance/) for agents decide, tool by tool, whether an agent's action runs autonomously, pauses for review, or stays blocked.
- [Scanner enforcement](https://docs.gitlab.com/user/application_security/policies/scan_execution_policies/) holds a change at the pipeline level until it clears the security and quality checks your organization requires.


We recommend creating an organization-wide[AI governance policy](https://about.gitlab.com/blog/a-developers-guide-to-building-an-ai-security-governance-framework/) rather than leaving it to individual team norms. This helps create consistent usage and policies, and helps auditors verify your use of AI.


### Added context from the decision layer


There is a longer-term benefit to capturing these approvals as structured records rather than letting them evaporate in a chat thread or a reviewer's memory. An exception granted last quarter, the policy version it was granted under, and who approved it are exactly the kind of organizational judgment that both auditors and future agents need to reference.


Enterprises that treat decisions as durable, queryable events get the added benefit that the next reviewer, human or agent, starts with additional context instead of starting from zero.


> Learn more about[the decision layer](https://learn.gitlab.com/transcend-evergreen/whitepaper-use-case-for-context-graphs) and how capturing decision events can impact your software development.


## 5 metrics that measure an AI rollout


Five categories are worth tracking from the start, and they should be reviewed together rather than in isolation, since a spike in adoption without a matching look at risk metrics is itself a warning sign.


1. **Adoption:** Active users of AI features week over week, number of flows or agents run, and which teams have turned agentic capabilities on versus which have not.
2. **Acceptance and quality:** Downstream signals like revert rate on AI-assisted changes and how often AI-authored merge requests pass review without rework. A single acceptance-rate number is a weak proxy for effectiveness on its own, since it says nothing about what happens to the code after it is accepted.
3. **Risk:** Track how often tool approval guardrails pause an action for review, how often those pauses result in a blocked or modified action, and whether any agent activity triggers a policy violation.
4. **Remediation** : Keep an eye on the scanner coverage across projects, the share of vulnerabilities that get auto-remediated versus manually triaged, and time to resolve findings once flagged.
5. **ROI:** Once you have a full quarter of data, explore the ROI of your AI investment. Track how much time developers save on tasks agents now handle and weigh that against the cost per resolved issue or merged change, in credits or compute spent. Check out this tutorial for an[in-depth look at your AI ROI](https://about.gitlab.com/blog/measuring-ai-roi-at-scale-a-practical-guide-to-gitlab-duo-analytics/) — you’ll learn how to transform raw usage data into actionable business insights and ROI calculations.


By tracking all five metrics together, you can catch issues with the AI rollout early. A rollout can look successful on adoption and acceptance while accumulating risk that only shows up in an audit six months later. With the above metrics, you get a holistic view of the value of AI to your organization.


## Your GitLab Duo Agent Platform governance checklist


For teams standardizing on GitLab Duo Agent Platform, here is a practical starting checklist before expanding agentic AI beyond a pilot:


- Review the[AI Transparency Center](https://about.gitlab.com/ai-transparency-center/) and confirm your understanding of data usage, model vendors, and subprocessor commitments.
- Decide, at the platform level, which agents and flows are approved for use, and publish them through GitLab’s[AI Catalog](https://about.gitlab.com/blog/ai-catalog-discover-and-share-agents/#creating-sharing-and-managing-visibility) rather than letting teams configure their own.
- Set[tool approval guardrails](https://docs.gitlab.com/user/duo_agent_platform/agents/tool-governance/) as always allow, always ask, or always deny, based on the sensitivity of the tool.
- Set up[composite identity](https://docs.gitlab.com/development/ai_features/composite_identity/) so every agent action is linked to the human who requested it, and access requires both to be authorized.
- For regulated workloads, evaluate[self-hosted deployment and bring-your-own-model options](https://about.gitlab.com/blog/agentic-ai-enterprise-control-self-hosted-duo-agent-platform-and-byom/) against your data residency requirements.
- Document explicit human-in-the-loop checkpoints for code review, testing and validation, and deployment approval.
- Turn on[audit event streaming](https://docs.gitlab.com/user/compliance/audit_event_streaming/) for agent activity so every action lands in the same audit trail your organization already reviews.
- Define your[rollout metrics](https://docs.gitlab.com/user/analytics/) (adoption, acceptance, risk, remediation, ROI) before expanding past a pilot, and review them together on a recurring cadence.


Revisit the checklist each release cycle. Governance for agentic AI is not a one-time setup. New agents, new tools, and new models often reopen governance questions.


## Where agentic AI speed meets enterprise control


Agentic AI changes what needs governing. Code completion asked a developer whether a suggestion was good. Agentic AI asks what an agent is allowed to touch, who approved it, and whether that can be proven later.


Answering those questions fully requires governance built into the platform itself, not layered on afterward.[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) provides an AI Catalog, approval guardrails, and audit event streaming directly into the platform where the work happens, so governance isn't bolted on after the fact. Teams get AI-assisted speed and enterprise control together, because the guardrails are part of the workflow, not a separate process running alongside it.


> Start a[free trial of GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) . On the Free tier, you can[sign up in a few simple steps](https://docs.gitlab.com/subscriptions/gitlab_credits/#for-the-free-tier-on-gitlabcom) . If you're already on GitLab Premium or Ultimate, you can[turn on Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/) and use the[GitLab Credits included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits/) .


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less


## More to explore


[View all blog posts](https://about.gitlab.com/blog/)


[AI Claude Opus 5 on GitLab: Reasoning built for the hard tasks](https://about.gitlab.com/blog/claude-opus-5-on-gitlab-duo-agent-platform/)[AI Modernize Java with Cursor and GitLab](https://about.gitlab.com/blog/modernize-java-with-cursor-and-gitlab/)[AI GitLab Transcend Hackathon: What developers built on GitLab Orbit](https://about.gitlab.com/blog/gitlab-transcend-hackathon-orbit/)


## We want to hear from you


Enjoyed reading this blog post or have questions or feedback? Share your thoughts by creating a new topic in the GitLab community forum.


[Share your feedback](https://forum.gitlab.com/)
