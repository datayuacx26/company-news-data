---
schema_version: "1.0.0"
document_id: "298d8dffc086254dbdb6c09f3e1c1e1b76f9147d8fe2bff51cca64c26cc76aa8"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-keynote/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:f3bb3ed775da1f227fc8ce99365b1a64079112d9d465c00be7182333b04064fe"
---

# DASH 2026: Guide to Datadog’s newest announcements

This year’s DASH keynote showcased Datadog’s Bits AI support for both developer and operations workflows, where agents investigate, validate, and remediate full-stack issues alongside you and your team. The keynote introduced dozens of new capabilities spanning autonomous detection and remediation, AI-driven release validation and testing, unified journey monitoring, and a new generation of Bits AI agents that retain your team’s operational knowledge and act within the guardrails you define.


Whether you’re closing monitoring gaps automatically, shipping AI-generated code safely, optimizing slow database queries, or giving non-experts a way to investigate complex systems in plain language, Datadog enables teams to build better with AI. Review all the major keynote announcements in this post, and read our other roundup posts to see how Datadog helps you harness AI, achieve end-to-end observability, operate at scale, and secure and govern your environment.


-


[End-to-end observability](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-observability/)


-


[Harnessing AI](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-ai/)


-


[Operating at scale](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-scale/)


-


[Security & compliance](https://www.datadoghq.com/blog/dash-2026-new-feature-roundup-secure/)


## Close the ops loop from detection to remediation


### Autonomously monitor for impactful degradations with Bits Detection


New services ship faster than monitoring configuration can keep pace, leaving endpoints without alerts, thresholds calibrated against old traffic patterns, and routing that goes stale as teams reorganize. Bits Detection, now available in Preview, uses the context Datadog already has about your services, endpoints, dependencies, and deployment history to create and maintain detection coverage automatically. It focuses coverage on the endpoints most likely to affect users, establishes baselines from historical behavior rather than static thresholds, adapts as services change, and connects detection to autonomous investigation and remediation when issues arise. To get started,[sign up for the Preview](https://www.datadoghq.com/product-preview/bits-detection/) or[read the blog post](https://www.datadoghq.com/blog/bits-detection) .


### Retain your team’s operational knowledge with Bits Memories


Solving the hardest incidents often depends on context that live telemetry alone cannot provide, such as the failure patterns your team has seen before, the fixes that worked or failed, and the service-specific details that make your environment unique. Bits Memories helps retain useful operational lessons from the work your team is already doing across investigations, runbooks, postmortems, Slack conversations, prior remediations, and more. Bits automatically identifies important details and saves them to memory, so when related issues come up later, Bits can use that context without responders having to rediscover or re-enter it under pressure. To get started,[sign up for the Preview](https://www.datadoghq.com/product-preview/bits-memories) .


### Automatically resolve issues with Bits Remediation


After Bits completes an investigation and identifies a root cause, Bits Remediation helps resolve the issue. Bits can execute remediation actions across your services and infrastructure by calling APIs, running fully configured remediation scripts (such as kubectl commands to restart Kubernetes deployments), and writing code fixes that teams can open as pull requests with one click. Bits Remediation follows the guardrails that your team defines, so actions are aligned with your environment and risk preferences. This helps teams spend less time translating investigation findings into next steps while keeping responders in control of what gets executed. To learn more,[review our documentation](https://docs.datadoghq.com/containers/bits_ai_kubernetes_remediation/) or[sign up for the Preview](https://www.datadoghq.com/product-preview/bits-remediation) .


### Detect and remediate issues before they escalate with Bits Infrastructure Operations


As environments grow in size and scale and new workloads are deployed every day, infrastructure teams cannot manually triage and fix issues across the breadth of their infrastructure: hosts, Kubernetes, serverless, and networks. These issues include disk saturation on hosts,` CrashLoopBackOff` and` OOMKilled` errors in Kubernetes, concurrency limits on AWS Lambda, expiring TLS certificates on Networks, memory pressure on Amazon ECS, and much more.


Bits Infrastructure Operations, now available in Preview, autonomously detects, investigates, and remediates common and repetitive infrastructure issues before they escalate into incidents. It also flags risky infrastructure changes in pull requests before they reach production. When Bits Infrastructure Operations can safely act within guardrails you define, it remediates issues automatically. When fixes require human approval, it surfaces the highest-priority issues with the full context that your team needs to review and approve the next step. Teams can start with approval-based guardrails and expand guardrails over time as Bits learns from repeated approvals. To learn more,[read our blog post](http://datadoghq.com/blog/bits-infrastructure-operations) or[sign up for the Preview](https://www.datadoghq.com/product-preview/bits-infrastructure-operations) .


## Ensure reliability


### Move from passive observability to proactive network device health and remediation


Network teams are drowning in metrics, events, traffic, and device configuration data, but volume alone doesn’t tell you what’s critical or what to do next. Datadog Network Device Health automatically correlates signals across your network devices and surfaces issues ranked by business impact before they cascade. When an issue is detected, an investigation side panel explains what happened, the blast radius, and the exact config change to roll back. From there, one click deploys the rollback while real-time metrics let you monitor the recovery with confidence. For your most critical incidents, Bits Investigation accelerates troubleshooting with step-by-step reasoning that helps your team pinpoint root cause faster. To get started with Network Device Health,[check out our blog post.](https://www.datadoghq.com/blog/end-to-end-network-operations-with-bits/)


### Trace config changes causing complex network issues with Network Configuration Management


Datadog Network Configuration Management automatically correlates device performance degradation to the exact configuration change that caused it. When a performance issue strikes, teams no longer need to manually compare configuration snapshots or switch between tools to find the root cause. Datadog tracks configuration changes over time and surfaces AI-generated summaries that translate even the most complex changes into plain language that any engineer can understand and act on. When a problematic change is identified, your team gets a one-click rollback to the last trusted configuration for immediate resolution. Explore the[Network Configuration Management documentation](https://docs.datadoghq.com/network_monitoring/devices/config_management/) to get started.


### Trace network issues from application to device with L7 to L1 visibility


A trace showing latency or errors carries the full network story behind it: the services exchanging traffic, the flows connecting them, and the exact hop where a physical device is introducing the problem. Datadog’s L7 to L1 visibility gives engineers end-to-end visibility from the application layer through the network flows between services, down to the physical hops and devices where performance breaks down. Whether the culprit is a misconfigured firewall, an unexpected cross-region route, or a device with heavy packet loss, SREs and network engineers pinpoint the problem directly inside the Network tab in APM. To get started with L7 to L1 visibility in APM traces,[read our blog post](https://www.datadoghq.com/blog/end-to-end-network-operations-with-bits) or[sign up for the Preview](https://www.datadoghq.com/product-preview/) .


### Diagnose internet underlay issues with BGP Centric View


When the internet path degrades and every signal points to the Border Gateway Protocol (BGP) layer, confirming whether a transit provider or a peering issue is the root cause means leaving the platform entirely for manual Autonomous System Number (ASN) lookups and fragmented relationship tracing. Datadog’s BGP Centric View brings that context directly into the Network Path UI in a dedicated BGP tab to surface every ASN in the flow. With a single click, engineers can uncover each ASN’s service provider, upstream neighbors, and downstream neighbors, giving teams the full routing picture without leaving the platform. Explore the[BGP Centric View documentation](https://docs.datadoghq.com/network_monitoring/network_path/as_view/) to get started.


### Automatically optimize database queries with Datadog Database Monitoring


Datadog Database Monitoring’s Bits Database Optimization gives every engineering team a complete, automated path from slow query detection to production fix without requiring deep database expertise. By validating candidate rewrites against a simulated copy of your schema, Datadog helps ensure each optimization is proven faster on your specific data before it ever reaches your codebase.


When a fix is validated, Bits Database Optimization locates the exact line of code that issued the query and opens a ready-to-merge pull request with benchmark evidence inline, so teams can review and ship improvements within their existing workflow. After the change deploys, teams can confirm that the gain holds under real production load directly in DBM Query Metrics. To learn more,[read our blog post](https://www.datadoghq.com/blog/bits-database-optimization/) .


### Query logs across storage destinations with Federated Logs


Modern systems generate massive amounts of telemetry data, and not all of it lands in one place: application and infrastructure logs flow into observability platforms, ML training jobs emit logs into lakehouses, high-volume event streams land in columnar stores, and audit archives go to object storage like Amazon S3. The resulting fragmentation can pose hurdles during investigations, forcing teams to switch contexts and rewrite queries for different syntaxes. Federated Logs lets you query external data stores—including Databricks and ClickHouse—from the Log Explorer, using the same query syntax and facets, no matter where your logs live. Paired with Observability Pipelines, which routes, transforms, and normalizes logs before they reach their destinations, Federated Logs provides a consistent investigation experience across the storage systems you already use. To get started,[sign up for the Preview](https://www.datadoghq.com/product-preview/federated-search/) or[learn more in our blog post](https://www.datadoghq.com/blog/federated-logs-databricks-clickhouse-snowflake) .


### Store and search logs at petabyte scale in your own infrastructure with Datadog BYOC Logs


Self-hosted log management gives teams data sovereignty and control, but these solutions are difficult to maintain, and they lack key SaaS platform capabilities like telemetry correlation and AI-powered analysis. Datadog BYOC Logs gives teams the best of both worlds. It runs in your own infrastructure and stays fully integrated with the Datadog platform. Datadog BYOC Logs lets teams keep full control over where their data lives without giving up petabyte-scale search, cross-telemetry correlation, AI-assisted investigation, or centralized governance.[Learn more in our blog post](https://www.datadoghq.com/blog/introducing-datadog-byoc-logs) .


## Ensure intent


### Monitor critical user journeys with Datadog Journey Monitoring


Without a unified view, engineering, product, and DevOps teams chase the same problems with different tools and arrive at different conclusions. This makes it nearly impossible to pinpoint whether a drop-off for a critical user journey is due to technical or behavioral factors. Datadog Journey Monitoring brings traffic, conversion rates, uptime, and errors from[Real User Monitoring,](https://www.datadoghq.com/product/real-user-monitoring/)[Synthetic Monitoring](https://www.datadoghq.com/product/synthetic-monitoring/) , and[Product Analytics](https://www.datadoghq.com/product/product-analytics/) into a single shared view of every critical user flow, so engineering, product, and DevOps always have a shared understanding of a journey’s performance. Journey Monitoring is currently in Preview, and if your organization is already using all three DEM products (Real User Monitoring, Synthetics, and Product Analytics), you’re eligible to[sign up today](https://www.datadoghq.com/product-preview/journey-monitoring/) . Learn more in the[Journey Monitoring documentation](https://docs.datadoghq.com/journey_monitoring/) and[read our blog post](https://www.datadoghq.com/blog/journey-monitoring/) .


## Close the dev loop from finding to fix


### Turn Datadog findings into automated code fixes with Bits Code


Engineering teams can get stuck in a reactive remediation loop. Every error spike, performance regression, flaky test, or new vulnerability kicks off the same manual cycle: triage, locate the code, write a fix, run tests, and open a pull request. Bits Code, Datadog’s platform-wide coding agent, closes that loop. It’s embedded wherever Datadog surfaces a problem, from Error Tracking and APM Recommendations to Continuous Profiler, Test Optimization, Code Security, Database Monitoring, Kubernetes Remediation, and Bits AI SRE, so the same agent fixes a recurring error one minute and remediates a vulnerability the next.


Because Bits Code investigates with the same telemetry data that engineers already trust, including logs, traces, metrics, profiles, runtime variables, and security findings, every proposed fix is grounded in real production behavior rather than the guesses generic coding assistants make from source code alone. Teams can also prompt Bits Code directly for refactors and one-off coding work, schedule recurring remediation runs, or trigger runs automatically off telemetry data. Bits Code is now generally available. To learn more,[check out our blog post](https://www.datadoghq.com/blog/bits-code/) and[Bits Code documentation](https://docs.datadoghq.com/bits_ai/bits_ai_dev_agent/) .


### Ship code safely at AI speed with Bits Release


Bits Release is an AI release validation agent that verifies every code change from pull request (PR) to production. When a PR is opened, Bits Release analyzes the intended impact of the change, generates a validation plan, runs end-to-end checks in staging, and monitors the production rollout.


Unlike traditional monitoring, Bits Release validates releases in context: It verifies that the expected improvements actually happen while detecting regressions and unintended side effects. When issues occur, it investigates likely root causes and helps generate fixes. Successful validations can be promoted into persistent production monitors, creating a continuous safety loop for high-velocity and AI-generated code.[Learn more in our blog post](https://www.datadoghq.com/blog/bits-release) , or[sign up for the Preview](https://www.datadoghq.com/product-preview/bits-release/) .


### Automate synthetic test coverage with Bits Testing


Keeping synthetic tests current is one of the most time-consuming parts of shipping fast. New flows go untested, interface changes break existing scripts, and coverage gaps quietly follow. Bits Testing Agent automates synthetic test generation and maintenance by exploring your application autonomously, identifying critical user journeys, and generating runnable test suites from a URL or natural language goal. For dynamic applications where interfaces and outputs vary, goal-based tests let you define an intended outcome rather than a fixed sequence of steps, so tests adapt instead of break. Scheduled explorations keep coverage current over time without manual intervention.[Learn more in our blog post](https://www.datadoghq.com/blog/bits-testing-test-coverage/) . To start automating your test coverage with Bits Testing Agent,[join the Preview](https://dd-corpsite.datadoghq.com/forms/share/4fc46e5cab7132791cbd2f0b63add065864e459aaf914b97ade4d77f22e363a1) .


## The agentic stack data foundation


### Get quality answers to business questions with Bits Data Analysis


Bits Data Analysis answers natural-language questions about aspects of your business, such as revenue, sales pipeline, churn, and product adoption. It’s powered by Datadog Data Context, a knowledge base that pulls table descriptions, metric definitions, freshness and quality signals, and lineage from sources like Tableau, Looker, Power BI, Fivetran, your warehouse, and[Data Observability](https://docs.datadoghq.com/data_observability/) . It then enriches that with business context from[Product Analytics](https://www.datadoghq.com/product/product-analytics/) , upstream applications, and source code, replacing months of manual semantic-layer work. Bits Data Analysis can go further than typical BI tools and explain why a metric changed, such as by tracing a revenue dip to a checkout-service deployment that had a latency spike. The Context Workbench gives data teams a dedicated place to observe how the agent is used across Slack, the Datadog web app, coding agents like Claude Code or Codex, and the Datadog API. From there, admins can define evaluations from real user questions and improve answer quality.


With Bits Data Analysis, data teams get end-to-end governance and observability: pipeline health, data quality, data context, agent answers, confidence indicators, and eval suites that gate any change to the context layer. To learn more about Bits Data Analysis,[read our blog post](https://www.datadoghq.com/blog/bits-data-analysis/) and[sign up for the Preview](https://www.datadoghq.com/product-preview/bits-data-analysis) .


### Use custom metrics for the modern age with Infinite Cardinality Metrics


Modern systems generate more telemetry data than ever. SRE teams track latency per tenant, region, and feature flag. Engineers building with AI follow signals at every step of an agent’s execution. The dimensions that teams need keep multiplying: tenant, user, device, model, region, execution path. But as telemetry data becomes more granular, cardinality becomes the wall.


Today we’re introducing Infinite Cardinality Metrics, a new pricing option for custom metrics built for the way modern systems operate. Infinite Cardinality Metrics is built for agentic querying and exploration, so you and your agents can ask anything of your metrics. It gives you the freedom to capture every attribute and dimension that matters, no matter how high the cardinality. Infinite Cardinality Metrics is priced per metric name and scales with your data volume, not cardinality, so cost stays predictable as you add context.


Infinite Cardinality Metrics is now generally available. To learn more,[visit the documentation](https://docs.datadoghq.com/account_management/billing/metric_name_pricing/) , and[read our dedicated blog post](https://www.datadoghq.com/blog/infinite-cardinality-metrics/) .


## Build and monitor the agentic stack


### Monitor agent adoption with Datadog Agent Console


As coding agent usage spreads across teams, engineering leaders need more than anecdotal wins to justify the spend: They need to see who’s using agents, whether it’s improving delivery, and where costs are going to waste. Datadog Agent Console gives you a unified view of activity across coding agents like Claude Code, Cursor, and GitHub Copilot, as well as Datadog’s own Bits AI agents, with adoption analytics, engineering impact metrics, spend attribution, and automated waste detection built in. Agent Console helps you answer three practical questions:


1.


Who in my organization is using coding agents the most?


2.


What are users doing well with agents and where are they struggling?


3.


How does AI spend correlate with engineering output?


You can get started with Agent Console today by[visiting our documentation](https://docs.datadoghq.com/ai_agents_console/) . To learn more about its features,[read our blog post](http://datadoghq.com/blog/datadog-agent-console) .


### Understand production LLM behavior with Patterns in Agent Observability


When you deploy an LLM-powered application, production traffic rarely behaves the way you expect: Users ask questions outside the intended scope, goals shift mid-conversation, and workflows emerge that you never anticipated. Patterns in Datadog LLM Observability helps you understand what’s actually happening in production by automatically clustering interactions into behavioral groups, without requiring predefined categories or manual labeling. Each cluster surfaces operational and quality signals, including traffic volume, latency, cost per interaction, error rate, and evaluation scores. This enables you to immediately identify which categories of user behavior are driving regressions or rising costs. For more information,[read our dedicated blog post](https://www.datadoghq.com/blog/patterns-agent-observability/) . To request early access,[sign up for the Preview](https://www.datadoghq.com/product-preview/ai-studio-bits-eval-patterns/) .


### Improve AI agent quality with Bits Evals


The process of debugging and improving an AI agent follows a consistent pattern: Teams collect user signals, investigate failures in traces, make changes to prompts or workflows, validate those changes with evaluations and experiments, and then monitor the results after deployment. Engineers need to do much of this work manually, with the necessary context—traces, dataset records, and prompt versions—spread across toolsets. Bits Evals is a set of agentic features that handles the repetitive parts of the AI agent development loop, while keeping engineers in control of the decisions that matter. With visibility into the complete context of your agent’s performance, Bits can form a hypothesis and immediately verify it by cross-referencing traces, dataset records, and evaluator outputs as evidence. It can also help you address the issue by suggesting a prompt change, flagging a dataset gap, proposing new evaluator coverage, or surfacing a regression you didn’t know to look for. This removes hours of manual trace-reading, so that engineers can spend their time on decisions rather than gathering the inputs needed to make them.[Learn more in our dedicated blog post](https://www.datadoghq.com/blog/bits-evals/) , or[sign up for the Preview](https://www.datadoghq.com/product-preview/ai-studio-bits-eval-patterns/) .


## Secure the agentic stack


### Protect agentic AI applications with Datadog AI Guard


[AI Guard](https://www.datadoghq.com/blog/ai-guard/) helps protect custom AI agents against prompt injection, tool misuse, data exfiltration, and other OWASP Top 10 threats. It discovers unprotected agents in your environment, analyzes behavior and historical context, and helps detect and block attacks at runtime. It also provides defense-in-depth for coding agents against[malicious skills](https://securitylabs.datadoghq.com/articles/malicious-skills-supply-chain-risks-in-coding-agents-with-dynamic-context/) , scripts, configurations, and packages. AI Guard sits directly inline with your agents to provide real-time security guardrails, so you can deploy AI agents fast without compromising security.


AI Guard is currently in Limited Availability.[Sign up to get early access](https://www.datadoghq.com/product-preview/ai-security/) .


### Cut vulnerability noise by over 95% with the Datadog Runtime Prioritization Engine


Security teams are drowning in findings, with no reliable way to know which ones pose real risk. The Datadog Runtime Prioritization Engine combines runtime behavior, reachability, service ownership, and business impact into a single prioritization model that surfaces the vulnerabilities tied to your most critical services and routes them directly to the engineering teams that can fix them. One-click remediation and Bits Code can take findings from detection to done without manual triage or chaotic handoffs. To get started,[sign up for the Preview](https://www.datadoghq.com/product-preview/runtime-prioritization-engine/) .


-
-
-
