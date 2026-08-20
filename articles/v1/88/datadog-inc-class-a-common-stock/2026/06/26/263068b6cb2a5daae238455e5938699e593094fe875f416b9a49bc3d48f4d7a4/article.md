---
schema_version: "1.0.0"
document_id: "263068b6cb2a5daae238455e5938699e593094fe875f416b9a49bc3d48f4d7a4"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/bits-agent-builder/"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:8473168d061cfd3e2eb2545838bc974f7e7349c60c19f6adbff7f03d05752a01"
---

# Introducing Bits Agent Builder: Build agentic workflows for alert response and remediation

Amber Tunnell


Nicole Parisi


Mitchell Waldman


Madhu Thayalur Venkanna


Kevin Gallardo


Tyler Anderson


Building automated workflows that adapt to real-world complexity can be a challenge. As systems scale and scenarios multiply, teams often end up hardcoding endless logic branches just to handle every potential outcome.


That’s why we’re introducing[Bits Agent Builder](https://docs.datadoghq.com/actions/agents/) , a powerful new tool that lets you create custom AI agents that are fully hosted by Datadog. These agents can analyze data, reason through complex decisions, and adapt to changing inputs in real time, all without requiring you to hardcode every possible path. With AI-driven orchestration,[Datadog Workflow Automation](https://docs.datadoghq.com/actions/workflows/) can now power automations that think, react, and scale as intelligently as your systems do.


In this blog post, we will cover the following topics:


-


What is Bits Agent Builder?


-


What types of workflows can you automate with Bits Agent Builder?


## What is Bits Agent Builder?


Bits Agent Builder lets you define AI agents that can interpret Datadog observability data and third-party signals—such as which service is affected, what metrics are spiking, or whether similar incidents have occurred—and then take action either automatically through a workflow or on demand through chat. You can describe an agent’s goals in natural language, include relevant prompts, and control which data sources and tools it can access. This makes it possible to design agents that reflect the specific architecture, technology stack, and operational patterns of your environment.


Workflows can also chain multiple agents together or trigger agents conditionally based on the situation. For example, an investigation agent might analyze logs and traces to identify a likely root cause, then trigger a remediation agent if the suggested fix matches a known pattern—such as restarting a service, rolling back a deployment, or scaling a resource. These chains allow different agents to specialize in distinct steps of an operational process while maintaining end-to-end automation and control. By orchestrating agents this way, teams can build workflows that reason, act, and adapt in real time, without sacrificing the structure and safety of traditional automation.


## What types of workflows can be automated with Bits Agent Builder?


Teams can use Bits Agent Builder to make smarter, faster decisions that draw upon the full range of Datadog’s observability data. A key feature of custom agents built with Bits Agent Builder is that they have access to the 2,000+ prebuilt, production-ready actions from Datadog’s[Action Catalog](https://docs.datadoghq.com/actions/actions_catalog/) . As shown in the image below, these actions connect to external services such as Kubernetes, cloud services, CI/CD platforms, security tools, and more.


Thanks to these built-in integrations with third-party services, you can automate virtually any task without requiring custom code or external integrations. And since your agents run natively in Datadog, your data, logic, and actions all live in one place with no context lost.


To see what this looks like in practice, let’s explore some ways you can use Bits Agent Builder to automate complex workflows.


### Automate application error investigations


Consider an agent created with Bits Agent Builder to automate the investigation of application errors. This investigator agent runs automatically when a Customer Success Manager or Support Engineer posts a Slack message such as “User reports the app crashed” about a bug. Once triggered, the agent runs a predefined playbook that pulls evidence only from trusted Datadog sources—such as logs, APM, and RUM—to ensure consistent, high-quality investigations. Following the playbook, the agent first searches recent application logs for related errors or warnings, and then checks RUM data to see if any users have experienced similar frontend issues or performance slowdowns. As it collects data, it follows strict standards: Every log or trace is hyperlinked, assumptions are clearly marked, and if nothing is found, it states that clearly.


Finally, the agent compiles a structured report—summarizing findings, linking to evidence, and suggesting possible root causes—and posts it back into Slack. By the time engineers pick up the issue, they already have a data-backed investigation that meets company best practices, powered entirely by Datadog’s observability data and AI-driven automation.


### Detect and respond to a critical vulnerability


In a second example, imagine how a custom agent could be used to automate a response when critical vulnerabilities are detected. Let’s say that[Datadog Cloud Security](https://docs.datadoghq.com/security/cloud_security_management/) detects several EC2 instances running a vulnerable version of OpenSSL that is affected by a newly disclosed critical CVE, such as CVE-2023-0464. The agent starts by creating a[Datadog case](https://docs.datadoghq.com/service_management/case_management/) , consolidating all relevant details (such as instance IDs, AWS account information, and whether the hosts are internet-facing) to give the security team immediate visibility into the blast radius.


A user interaction with a custom remediation agent. A user interaction with a custom remediation agent.


Once the vulnerability is confirmed as high-risk, the agent steps in to contain the threat. It can generate a Github PR to update the OpenSSL version or quarantine the affected EC2 instances by using AWS Systems Manager. It can then notify the responsible engineering team through Slack,[Datadog On-Call](https://www.datadoghq.com/blog/datadog-on-call/) , or PagerDuty. Every remediation action is logged back into the Datadog case, creating a complete audit trail. This automated, end-to-end response pipeline helps teams move from detection to remediation in minutes, significantly reducing exposure time and strengthening their overall cloud security posture.


### Automate service documentation with IDP Scorecards


[Scorecards](https://docs.datadoghq.com/internal_developer_portal/scorecards/) in Datadog’s Internal Developer Portal (IDP) help teams measure and enforce operational and engineering standards across their services in areas like deployment readiness, observability coverage, or documentation completeness.


In this use case, a scorecard is configured to assess whether each service repository includes a README.md file. During the scorecard evaluation, if the check detects that a service is missing a README, it triggers a custom Datadog agent. This agent uses metadata from the Service Catalog (like team name, dependencies, and runtime environment) and pulls key context from telemetry data and existing dashboards to auto-generate an initial README draft. It can include sections like “Service Overview,” “Dependencies,” and “Runbook Links,” and then open a pull request in GitHub for the owning team to review and finalize.


By combining Scorecards with automation, teams can close documentation gaps automatically—evaluating, creating, and updating service standards with minimal manual work.


## Try Bits Agent Builder Today


With[Bits Agent Builder](https://docs.datadoghq.com/actions/agents/) , engineers can create and manage purpose-built agents in minutes, accelerating how teams investigate issues and trigger actions across their environments.


Bits Agent Builder is now generally available. You can try it out by navigating to[Actions -> Agents](https://app.datadoghq.com/actions/agents) in your Datadog account.


If you don’t already have a Datadog account, get started with a 14-dayfree trial today.


-
-
-
