---
schema_version: "1.0.0"
document_id: "99769b15e152db783dfc771cd071dc529edc4cf627e09ae0eb5478917db37762"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/launching-nao-enterprise/"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:174504346a7867d1a7328695dc892a408baf663a9870578270f5fb2412a7591f"
---

# Launching nao Enterprise

[Blog](https://getnao.io/blog/) /


product updates


# Launching nao Enterprise


nao Enterprise gives data teams SSO, row-level security, branded UI, and implementation support - while staying open source at the core.


20 May 2026


By Claire Gouze


Founder @ nao


nao is open source. Apache 2.0. Self-host it, connect your warehouse, deploy the agent. That doesn't change.


But as more companies move nao into production, we keep hearing the same requests: SSO, row-level security, a branded interface, and someone to help get the context layer right from day one.


Today we're launching **nao Enterprise** - the production-grade layer for teams that need security, governance, and hands-on support on top of the open-source agent.


## Why Enterprise


nao's open-source edition already powers 50+ companies in production. Data teams use[nao-core](https://getnao.io/blog/open-source-analytics-agent-launch) to build their[context layer](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) , deploy the chat UI, and connect it to[Slack](https://getnao.io/blog/setup-ai-analytics-slack-bot-open-source) ,[Teams](https://getnao.io/blog/setup-ai-analytics-teams-bot-open-source) , WhatsApp, or any[MCP-compatible tool](https://getnao.io/blog/launching-nao-mcp) .


That works well for small data teams running a proof of concept. But when you're rolling out an analytics agent to 200 people across sales, finance, and ops, three things come up fast:


**Security requirements.** Your IT team needs SSO. Your compliance team needs row-level security so the sales director can't see finance data. Your CISO needs to know that data access is governed, not wide open.


**Brand and trust.** Business users adopt internal tools faster when they look like internal tools. A company-branded UI with your logo and colors signals that this is an official, vetted tool - not someone's side project.


**Getting the context right.**[Context engineering](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) is the single biggest driver of agent reliability. The difference between a 60% accurate agent and a 95% accurate one is the quality of the context layer. Most teams benefit from expert guidance on their first implementation.


## What's in Enterprise


- **Self-hosted or Cloud**
- **SSO (Microsoft, Okta)**
- **Row level security**
- **White label**
- **SOC 2 Type II reports**
- **Priority support & roadmap input**
- **Implementation services** (optional)


## Implementation services


Enterprise customers can optionally add a **forward deployment** engagement to get to production faster.


**Context setup**


1. We connect your warehouse, repos, and business tools
2. We build the initial[context layer](https://getnao.io/blog/how-to-build-context-stack-for-agentic-analytics) on a first scope of up to 30 tables
3. We run[agent evaluation](https://getnao.io/blog/improve-analytics-agent-reliability-steps) with unit tests to measure accuracy
4. We assess your data model quality for agent readiness
5. You get a production-ready agent, deployed and tested


**Ongoing roll-out support**


- Weekly touchpoints on agent quality and adoption
- Support on the[context feedback loop](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) - iterating on rules, definitions, and documentation as your team uses the agent
- Guidance on expanding scope to new data domains


The goal is simple: you should be live in production within two weeks, not two quarters.


## Open source vs Enterprise: what's different


nao uses a **dual license model** . The core product is Apache 2.0 - fully open source, free to use, modify, and self-host. Enterprise features are licensed under the nao Labs Commercial License.


Capability Open Source Enterprise


**Open-source agent + chat UI, all LLM providers** Yes Yes


**Slack / Teams / WhatsApp / Telegram bots** Yes Yes


**Self-hosting on your infrastructure** Yes Yes


**Admin / Member / Viewer roles** Yes Yes


**Microsoft SSO (Azure AD / Entra ID)** - Yes


**Okta SSO** - Coming soon


**Per-user identity passthrough to Redshift (RLS)** - Yes


**White-label branding** (logos, favicon, app name) - Yes


**SOC 2 Type II reports** - Yes


**Priority support and roadmap input** - Yes


The open-source edition is not a demo or a trial. It's the full product. Thousands of agents have been created with it. Enterprise adds the security, branding, and support layer that larger organizations need to go company-wide.


You can read the full license in the[GitHub repository](https://github.com/getnao/nao/blob/main/LICENSE) . Files under the commercial license are marked with` @license Enterprise` in the source code. Everything else is Apache 2.0.


## How to get started


**If you're already using nao open source** - you're most of the way there. Your context layer, your warehouse connections, your tests - everything carries over. Enterprise is an upgrade, not a migration.


**If you're starting fresh** - even better. Add implementation services and you'll have a production agent faster than building it yourself.


Here's the process:


1. **Reach out** -[Contact us](https://getnao.io/contact/) to start the conversation
2. **Scoping call** - we assess your data stack, identify the first use case, and define the scope
3. **Implementation** - we build the context layer together, run tests, and deploy
4. **Go live** - your team starts querying data in plain English


For technical details on Enterprise features, check the[Enterprise documentation](https://docs.getnao.io/nao-agent/enterprise/overview) .


[Contact us](https://getnao.io/contact/) |[GitHub](https://github.com/getnao/nao) |[Docs](https://docs.getnao.io/nao-agent/enterprise/overview)


## Related articles


[insights The Agentic Analytics Playbook is out Learn how to choose your harness, build your context layer, plan your rollout, measure success, and get examples from 7 real-life companies.](https://getnao.io/blog/agentic-analytics-playbook/)[product updates We're launching the first Open Source Analytics Agent Builder We're open sourcing nao — an analytics agent framework built on context engineering. Here's our vision for what comes after black-box BI.](https://getnao.io/blog/open-source-analytics-agent-launch/)[product updates nao has a new look We rebuilt the nao interface from the ground up. New home screen, a prompt queue, visible agent reasoning, redesigned charts and stories.](https://getnao.io/blog/nao-redesign/)


Claire


For nao team
