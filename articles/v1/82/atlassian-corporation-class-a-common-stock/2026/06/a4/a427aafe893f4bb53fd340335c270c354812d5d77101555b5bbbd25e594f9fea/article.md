---
schema_version: "1.0.0"
document_id: "a427aafe893f4bb53fd340335c270c354812d5d77101555b5bbbd25e594f9fea"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-df2ad9c38740"
canonical_url: "https://www.atlassian.com/blog/company-news/gartner-devsecops-2026"
published_at: "2026-06-17T18:46:46+00:00"
first_seen_at: "2026-07-24T17:38:22.240177+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:a31315d436f7df46abbfd7cb4d1c24b7fc0ab60223dd8bb9de60266bdced9fb5"
---

# Atlassian named a 4x Leader in the 2026 Gartner® Magic Quadrant™ for DevSecOps Platforms

Today, Gartner® named Atlassian a Leader in the 2026 Magic Quadrant™ for DevSecOps Platforms, and placed us **highest in execution.** It’s our fourth consecutive year as a Leader across the Gartner DevOps and DevSecOps research – a category that has changed underneath all of us as security shifted from a downstream checkpoint to a first-class concern in every team’s delivery loop.


This one matters to me for a specific reason: it validates a bet we made four years ago. We bet that the future of software delivery wouldn’t be won by generating code faster, but by giving entire teams – humans and agents – the shared context, governance, and trust to ship better software, safely, at scale.


---


## Why DevSecOps is now an agentic problem


Every engineering leader I talk to is asking the same question: how do we get real ROI from AI? The data is sobering. Most teams report only[10–15% productivity gains](https://getdx.com/report/ai-and-engineering-velocity-a-longitudinal-analysis/) from their coding agents, even as adoption climbs far higher.


The gap comes from a simple oversight. Most AI deployments speed up the 16% of a developer’s time spent writing code and ignore the 84% spent on planning, review, security checks, incident response, and cross-team coordination.


As writing code keeps getting faster and cheaper, the pressure only moves further up the lifecycle. How do you keep agent workflows aligned across thousands of engineers? How do you make sure an agent’s output is grounded in your business context and your code standards – not generic best practice?


We work on this every day, for our customers and for ourselves. Here’s what we’ve found: context is the difference. In our internal benchmarks, agents grounded in your organization’s context graph return **44% more accurate results** than agents relying on MCP connections alone. With that context in place, the gains get real – we’ve used Rovo and Jira to automatically resolve **51% of vulnerabilities** over the past six months.


We believe the Gartner recognition reflects that shift. DevSecOps isn’t only about deployment pipelines and vulnerability scanning anymore. It’s about whether your platform can inform human judgment and govern agent action across the entire SDLC.


## Four bets that defined Atlassian’s platform approach to AI


We believe this recognition reflects four bets we’ve been making – and the products we’ve built around them.


1. **Embedded agents to unblock the full SDLC, including security and review.** Our[AI code reviewer](https://www.atlassian.com/software/rovo-dev/code-review) takes the first pass at evaluating PRs against your engineering standards, acceptance criteria, and product context, not just linting. Our Vulnerability Fixer agent auto-remediates security issues against your team’s coding standards. We’re extending the same pattern to incident management, KTLO work, and spec-driven development.
2. **Teamwork Graph, the secure context engine for agents + humans.** MCP servers can give an agent access to a Jira ticket. They can’t tell it why the ticket matters, which Confluence page holds the architectural decision, or which downstream services have dependencies.[Teamwork Graph](https://teamworkgraph.com/) , the connected graph across Atlassian and third-party tools, gives every agent and every teammate the same shared understanding.
3. **Real visibility into AI usage, cost, and risk.** How much of your code is built with AI? How many tokens are your teams burning? Where are the agents going off-script? With[DX now part of Atlassian](https://www.atlassian.com/blog/announcements/atlassian-acquires-dx) , you can answer these questions.[New DX features](https://www.atlassian.com/blog/company-news/dx-team-26) deliver AI-generated code attribution, AI Dollar Impact, agent efficacy measurement, and unified DORA metrics — the developer intelligence layer security and engineering leaders need to govern AI at scale.
4. **Enterprise foundations to safely enable the agentic software development.** Atlassian Government Cloud is GA and FedRAMP Moderate authorized. Atlassian Isolated Cloud, our dedicated single-tenant environment, is on track for GA later this year. We’re investing in IL5 and FedRAMP High for the most regulated workloads. Every agent action runs inside the same secure boundaries your teams already trust.


We’re building alongside engineering orgs already running agents at scale – and the questions they’re wrestling with shape where we invest:


> The question we’re asking isn’t how do we get engineers to use more AI. It’s how do we build a system where humans and agents are working from the same context, toward the same goals – with the governance and audit trails we need to ship safely. Assigning work directly to Cursor from Jira, with all that rich context and oversight, is a meaningful step toward orchestrating agents effectively at scale.”


**– Jason Andrews, Vice President – Engineering Operations, Cisco**


## What’s coming next


Four years in the Leaders quadrant, and I’m more excited than ever about[what we’re working on](https://www.atlassian.com/blog/company-news/founder-update-team-26) . Here’s a preview of what we’re actively shipping for engineering teams:


- **Agent orchestration in Jira:** We are extending the ability to assign work to more coding agents of choice. The agent picks up the task with full context, notifies you when it needs input, and links the PR back to Jira automatically.
- **Spec-driven development for the whole team:** Atlassian pulls from your codebase, Jira history, and team context to define requirements and generate a structured technical spec in Confluence – ready for a developer or coding agent to build.
- **Teamwork Graph CLI and MCP:** Give the agents you already use full Atlassian context in the terminal, IDE, or browser – more accurate results, fewer tokens. Local sessions are tracked straight into Jira and Confluence, so progress stays visible without anyone manually updating status.
- **Automations for agentic workflows:** Route repeatable work — bug fixes, vulnerability remediation, test generation, doc updates — directly to coding agents. They run in the background with Teamwork Graph context and notify engineers when a PR is ready for review.
- **Semantic code search:** Give agents a richer, more reliable understanding of large codebases and monorepos.
- **AIOps for automated remediation:** Agents triage incidents in Jira Service Management with Rovo, surfacing the likely root cause from prior runbooks, code, team ownership, and error details — all grounded in Teamwork Graph context.


## Context: The future of the AI-era SDLC


The teams that win in the AI era won’t be the ones generating code the fastest. They’ll be the ones whose entire lifecycle – planning, review, security, operations – is grounded in context that humans and agents trust equally.


This is the platform we keep building. It’s the bet Gartner has now recognized four years running, and the one my teams show up to make real every day.


To the engineers across Atlassian who built this, and to every developer, customer, and partner building the future of software with us: thank you. Let’s keep building.


**Access the 2026 Gartner® Magic Quadrant™ for DevSecOps Platforms report.**


[Access the 2026 report](https://atlassian.com/gartner/magic-quadrant-devsecops)


**Want to see the AI-native SDLC in action? Join us in person or online at Team ’26 Europe.**


[Attend Team ’26 Europe](https://events.atlassian.com/teameurope)


Gartner, Magic Quadrant for DevSecOps Platforms, Keith Mann, Thomas Murphy, Bill Holz, 15 June 2026


GARTNER is a trademark of Gartner, Inc. and/or its affiliates. *Magic Quadrant is a registered trademark of Gartner, Inc. and/or its affiliates and is used herein with permission. All rights reserved.*


*This graphic was published by Gartner, Inc. as part of a larger research document and should be evaluated in the context of the entire document. The Gartner document is available upon request from Atlassian.*


*Gartner does not endorse any company, vendor, product or service depicted in its publications, and does not advise technology users to select only those vendors with the highest ratings or other designation. Gartner publications consist of the opinions of Gartner’s business and technology insights organization and should not be construed as statements of fact. Gartner disclaims all warranties, expressed or implied, with respect to this publication, including any warranties of merchantability or fitness for a particular purpose.*
