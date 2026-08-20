---
schema_version: "1.0.0"
document_id: "fcca9106a10246cdb4bd488184b2a0430d6e10431845536aa473070a55cf2ed7"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/ai-finops-cost-management-governance-snowflake"
published_at: "2026-07-07T19:51:05+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b7ebe2b33e7c231d26ee87b39adfd0c16994c0b018bf918eeac35d8f6920ac2b"
---

# FinOps for AI: Snowflake's AI Cost Management and Governance Tools

A warehouse credit spike is easy to explain. An AI bill isn't.


One Snowflake Cortex Agent™ can execute a chain of reasoning steps across multiple data sets. A single prompt can trigger thousands of tokens. And unlike traditional infrastructure, many AI workloads are exploratory by design.


That's why FinOps teams are facing a new challenge: governing systems whose costs are often as dynamic as their outputs.They’re being forced to solve two problems at once:


-


Use AI to improve FinOps


-


Govern AI spending itself


At Snowflake, we've been rethinking cost management from the ground up not just to keep pace with AI-driven workloads, but to use AI itself as the mechanism to make cost governance smarter, faster and accessible to everyone. This post covers how we're doing both: embedding AI into cost management tooling to make it more powerful, and providing the governance primitives you need to keep your own AI investments in check.


## The FinOps challenge has gotten more complex — and more urgent


The FinOps Foundation's[“State of FinOps 2026 Report”](https://data.finops.org/) makes one thing unmistakably clear: AI has taken over the FinOps agenda. According to the report, **FinOps for AI is now the No. 1** **forward-looking priority** for teams over the next 12 months. AI cost management is the single most desired skillset organizations are looking to build, and **98% of FinOps teams now manage AI spend** , up from just 31% two years ago. AI has gone from an emerging concern to a universal FinOps responsibility in the span of a single product cycle.


What makes it hard isn't just the volume of AI spend — it's the nature of it. The report surfaces three challenges practitioners face most often:


-


Visibility into AI costs (pricing models vary widely)


-


Allocating those costs to business units (harder than traditional infrastructure)


-


Determining ROI (investments are often exploratory)


In addition, the top requested tooling capability in the[“State of FinOps 2026 Report”](https://data.finops.org/) is **granular monitoring of AI spend** such as tokens, LLM requests and GPU utilization. As one practitioner noted: "Teams are having to balance the complexity of managing AI spend over a more diverse range of projects with the directive to avoid limiting AI usage, which might slow down time to market."


The FinOps Foundation frames this as a dual agenda: **use AI to improve FinOps productivity and efficiency and to manage AI spend** . That duality is precisely what Snowflake has been building toward.


## Part 1: AI embedded in cost management


The most common failure mode in FinOps is a lack of time to make sense of data. Engineers can write queries against ACCOUNT_USAGE; finance leaders can look at the monthly invoice. But nobody in between has the context to quickly understand why spend went up 30% last week, which warehouse is responsible, and what to do about it.


To address this, Snowflake is embedding **Snowflake CoCo™,** our AI-powered coding agent, directly into the cost management experience.


### Natural language cost analysis with the Cost Intelligence skill


CoCo includes a purpose-built **Cost Intelligence skill** that transforms cost management from a SQL task into a conversation. You can ask it cost questions like:


-


*"Why did my compute spend spike on Wednesday?"*


-


*"Which users are burning the most warehouse credits this month?"*


-


*"Show me my top five most expensive warehouses and cost trends over the last six months."*


-


*“How are my budgets performing this month?”*


CoCo returns an answer with an explanation, surfaces the underlying data and maintains context across follow-up questions. It connects the dots between warehouse activity, query patterns, user behavior and cost attribution in a way that previously required a data analyst with deep knowledge of ACCOUNT_USAGE schema.


The skill is available in both the Snowsight UI™ and Snowflake CoCo CLI or Desktop, which means whether you're a platform engineer in a terminal or a FinOps analyst in the browser, you have access to the same natural language interface.


### Anomaly detection that explains itself


You can detect cost anomalies for your Snowflake account using the built-in Cost Anomaly feature, but knowing that an anomaly occurred is only half the battle. Understanding why the anomaly happened is where teams historically got stuck spending hours correlating warehouse history, query logs and user activity.


With CoCo embedded in the Cost Management UI, you can now highlight an anomaly on the spend chart and click “explain” for more detail. CoCo investigates the anomaly, correlates it with warehouse activity, identifies the users or workloads involved and returns a narrative explanation in plain English within seconds in many cases.


### A redesigned Account Overview that's built for action


Announced as generally available during Snowflake Summit 2026, the updated Cost Management **Account Overview** in Snowsight is designed as a unified cost command center rather than a reporting dashboard. At a glance, you can see your budget health, any open anomalies, warehouse attribution status and a breakdown of credits by service type all from a single page. Every insight is connected to an action: If an anomaly needs investigation, CoCo is one click away. If a warehouse is unattributed to a cost center, you can leverage CoCo to build a tagging plan. The goal is to collapse the distance between seeing a problem and fixing it.


## Part 2: Governing AI spend — the new imperative


While AI makes cost management smarter, it also creates a new category of costs that organizations need to govern proactively. Snowflake has built a comprehensive set of primitives specifically for AI cost governance, from visibility through to notifications and enforcement.


### Visibility first


You can’t govern what you can’t see. Snowflake has invested heavily in granular AI cost visibility at every level.


#### Seven new organization-level AI views


In the ORGANIZATION_USAGE schema, accessible from your Organization Account, we’ve launched **seven new granular AI Services views** one for each major AI capability (Cortex AI Functions™, Cortex Agents™, Snowflake CoWork™, Snowflake CoCo and more). These views give Finance, Platform Engineering and FinOps teams a centralized source for all AI-related credit consumption across your Snowflake organization.


These views show daily AI spend broken down at a higher granularity (account, user and function/model) enabling you to monitor adoption trends, compare AI usage across business units and build internal chargeback reports without writing complex JOIN queries across multiple accounts.


#### The cost management dashboard in Snowsight


The **Consumption view** in Admin > Cost Management lets account admins drill into AI spend and filter by service type to isolate specific AI feature usage from warehouse compute. Combined with the new Account Overview redesign and the Cost Intelligence skill, teams can quickly see whether AI costs are trending up, which services are driving it, and who is generating the usage — all without leaving the Snowsight UI.


### Control: Guardrails that match the speed of AI


Visibility tells you what happened, but guardrails help prevent what you don't want to happen. Snowflake's budget and quota primitives have been extended specifically to govern AI workloads.


#### Budgets for AI — govern at the feature level


Snowflake **Budgets** are monthly spending limits that monitor credit usage for defined groups of resources. With the latest updates, budgets now cover **AI-related service types** : AI Functions, Snowflake CoWork, Cortex Agents and Snowflake CoCo, in addition to traditional compute resources.


Using tag-based budgets, you can map your organizational structure directly onto your spend controls. Tag AI resources by team, cost center or project, then create a budget scoped to that tag. When spend approaches your defined threshold, notifications fire automatically to the right people via email, webhooks to Slack, Teams, PagerDuty or even to cloud service provider message queues. When a threshold is breached, a **Custom Action,** which is a stored procedure you define, can execute automatically to revoke access, write an audit log or trigger a downstream workflow based on your business needs. The result is an automated and programmable cost enforcement layer that delivers an active response instead of a passive alert.


#### Per-user quotas — the missing link for AI democratization (public preview)


One of the most common challenges organizations face when rolling out AI features broadly is the individual user who generates outsized costs running a complex Cortex function against a million rows, or triggering repeated LLM calls in a tight loop. Traditional budgets track aggregate resource-level spend, but they don't protect against the single-user outlier.


**Per-user quotas** (currently in public preview) closes this gap. A quota defines a monthly or daily credit ceiling that applies per user, enforced independently for each individual in scope. Quotas cover the fastest-accruing AI domains:


-


AI functions (all Cortex SQL functions: AI_COMPLETE, AI_CLASSIFY, AI_EXTRACT and so on)


-


Snowflake CoWork (formerly Snowflake Intelligence)


-


Cortex Agents


-


Snowflake CoCo (Snowsight, CLI and Desktop)


Users are scoped by using Snowflake **tags** to map your existing structure (cost centers, departments, teams) onto quota scope without manually enumerating individual users. To facilitate full visibility, both the quota administrator and the individual user receive automated notifications as limits are approached or fully met, keeping everyone informed based on your specific quota configurations.


For organizations seeking hard guardrails, **block enforcements** allow you to cap consumption within minutes of users reaching their designated quota, reducing the risk of runaway spend. Depending on the daily or monthly limit you establish, user access to specific AI features is automatically restricted once the ceiling is reached, resetting seamlessly at the start of the next period.


For organizations deploying AI broadly, per-user quotas are the governance primitive that makes self-service AI safer. You can give every analyst access to AI functions while tracking each user's usage to proactively take action before they generate an unexpected large bill.


## The bigger picture: AI running both sides of cost management


What makes this moment different from every previous wave of FinOps tooling is that AI is doing two distinct jobs simultaneously.


On one side, AI is powering cost management itself. CoCo's Cost Intelligence skill means any team member — not just a data engineer with deep ACCOUNT_USAGE knowledge — can understand their spend, investigate an anomaly and create a budget in plain English. The redesigned Account Overview puts AI-generated insights at the center of the cost workflow, collapsing the amount of time between "something looks wrong" to "here's what happened and here's what to do about it." Cost management is no longer a back-office reporting function but an AI-assisted decision layer embedded in how your team operates every day.


On the other side, AI is the thing being governed. The combination of granular AI usage views, tag-based AI budgets and per-user quotas gives FinOps and platform teams the primitives to deploy AI broadly while maintaining stronger financial governance. You can now see which AI capability is spending what, enforce limits at the team and individual level and automate enforcement actions when thresholds are crossed.


Together, these two sides reflect a simple but important principle: the right response to AI-driven cost complexity is not more manual processes — it's better tooling, powered by AI, built into the platform where your data already lives.


*Learn more about[performance optimization](https://www.snowflake.com/en/pricing-options/cost-and-performance-optimization/) and[cost optimization](https://www.snowflake.com/en/developers/guides/cost-optimization/) .*


*Forward-looking statements: This content contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.*
