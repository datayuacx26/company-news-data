---
schema_version: "1.0.0"
document_id: "626406afcdb548e23ca38126bc6cfb2fde8522824a4b606f5a4fe73408101f4f"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/bits-infrastructure-operations/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:d6514764f4ecab3671b76bd1d21a239e967f5b3eb9048f1e9a6f6be3069661f6"
---

# Remediate issues autonomously with Bits Infrastructure Operations

Jessica Hsiao


Product Manager


Eli Kalish


Product Manager


Ananth Vaidyanathan


Group Product Manager


As environments grow in size and scale and new AI workloads are deployed every day, infrastructure teams must constantly adapt to and manage new resource patterns, scaling behavior, and operational risks. When application teams don’t have the expertise to respond to issues confidently on their own, infrastructure teams shoulder the burden to remediate issues across their infrastructure stack, including hosts, Kubernetes, serverless, and network infrastructure. These issues can include disk saturation on hosts,` CrashLoopBackOff` and` OOMKilled` errors in Kubernetes, concurrency limits on AWS Lambda, expiring TLS certificates on networks, memory pressure on Amazon ECS, and much more.


Datadog Bits Infrastructure Operations autonomously detects, investigates, and safely remediates common infrastructure issues before they impact your production environments and escalate into incidents. When Bits can safely act, it remediates issues automatically. When approval is required, it surfaces the highest-priority issues with the information your team needs to review and approve the next step. This reduces handoffs between application and infrastructure teams. Application engineers can identify infrastructure issues affecting their services and safely remediate them, while platform engineers control the guardrails.


In this post, we’ll show you how Bits Infrastructure Operations can help you:


-


Remediate infrastructure issues from wherever you work


-


Define guardrails for safe remediation


-


Approve remediation with a human in the loop


-


Autonomously prevent issues from recurring


-


Flag risky infrastructure-as-code (IaC) changes before they reach production


## Remediate infrastructure issues from wherever you work


Bits Infrastructure Operations keeps you informed about ongoing issues and remediations in several ways and across multiple surfaces, including:


-


Routine digests and critical issue notifications submitted in a chosen Slack thread


-


Natural-language queries in your dev environment using Claude Code and[Datadog MCP Server](https://www.datadoghq.com/blog/datadog-remote-mcp-server/)


-


Live-updated lists in the Bits Infrastructure Operations “Open Issues” and “History” pages


For instance, let’s say you’ve logged into Slack one morning and discovered that Bits autonomously detected, investigated, and remediated a recurring` CrashLoopBackOff` issue in the Kubernetes infrastructure your team’s service relies on. You can open a Claude Code session to review all the issues impacting your environment, and take action on any open issues that need your approval to remediate.


Alternatively, you can use the “Open Issues” page to quickly browse through all the infrastructure issues currently affecting your environment and drill into high-priority issues to view Bits’s autonomous diagnostics and remediation suggestions.


Bits Infrastructure Operations auto-remediates issues beyond just Kubernetes, such as zombie processes on EC2 and other cloud hosts, misconfigured task definitions on ECS, and Lambda cold starts and timeouts on serverless functions. It also surfaces network issues such as expiring certificates, increased latency, and more.


## Define guardrails for safe remediation


Bits Infrastructure Operations uses guardrails to define where, when, and how remediation actions can run. These guardrails let teams control remediation by resource, environment, action type, and other tag scopes to ensure that the agent follows the operational boundaries your team has already defined. This model gives teams a gradual path toward self-healing infrastructure.


For example, a team might create a guardrail that allows Bits to automatically apply Kubernetes patch actions for memory-limit adjustments in` env:staging` workloads tagged with` service:checkout-api` , while requiring human approval before applying the same patch in` env:prod` . This lets teams automate well-understood fixes in lower-risk environments while maintaining additional oversight for customer-facing services.


Bits Infrastructure Operations lets teams configure guardrails around the resources and actions that matter to them. Actions are categorized by resource type (ECS, hosts, Kubernetes, or serverless) to make this process clear and painless. And to make it easy for team members to quickly review guardrailed remediations, you can configure Slack channels for Bits to report in when a new remediation needs approval.


Teams can start with approval-based remediation, review the actions Bits proposes, and expand automation over time as they gain confidence in the workflows and safeguards. For help getting started with writing effective guardrails, you can start from included “Conservative,” “Balanced,” or “Permissive” guardrail templates.


## Approve remediation with a human in the loop


When Bits cannot auto-remediate an issue because of a guardrail you’ve defined, it surfaces the most important issues ranked by business impact. For these issues, Bits performs the investigation, prepares the remediation plan, and waits for an authorized user to approve the fix. Remediation plans are surfaced both in the Datadog UI and via the Slack integration.


For example, let’s say that your platform team has allowed Bits to automatically patch Kubernetes workloads in staging environments, while requiring human approval before applying the same change in production. When Bits detects a new issue—for instance, persistent` OOMKilled` errors caused by an undersized memory limit in a staging workload—it investigates the issue, reviews the workload context, and prepares a remediation plan to patch the deployment with a higher memory limit. Because the production guardrail requires approval, Bits surfaces the proposed change for review instead of applying it automatically.


From there, an engineer can approve the action. Bits can apply the fix directly to the Kubernetes cluster through the Datadog Agent to stabilize the workload quickly. This path helps teams mitigate the immediate issue when an application is degraded or unavailable.


In many cases, however, teams also need to persist the fix in code. Bits Infrastructure Operations can help teams go back to the source by opening a pull request that updates the relevant infrastructure as code configuration. This helps reduce configuration drift because the live cluster state and the desired configuration stay aligned. It also gives teams a reviewable Git-based workflow for changes that should become permanent.


This human-in-the-loop workflow is designed for teams that want faster remediation without removing engineering judgment from the process. Bits gathers context and prepares the action, while your team decides whether to approve it, apply it directly, persist it in code, or adjust the recommendation before proceeding.


## Autonomously prevent issues from recurring


Bits Infrastructure Operations can help reduce repetitive operational work by learning from issues your team has already reviewed and approved. If your team approves the same class of fix several times, Bits can recommend updating the relevant guardrail so it can remediate that issue type automatically the next time it appears.


Returning to the Kubernetes memory example, suppose that your team has repeatedly approved the same remediation for workloads that enter` CrashLoopBackOff` because their memory limits are too low. After Bits observes this approval pattern, it can prompt you to allow automatic remediation for the issue type within a defined scope. You can then update the guardrail so Bits can self-heal similar issues in the future, while still requiring approval for higher-risk resources or environments. The next time the issue crops up, Bits will self-heal the system and report a low-priority issue so you can review the root cause analysis and applied fix.


As teams expand guardrail automation for autonomous remediation, the number of issues that require manual attention decreases. Platform teams can redirect focus from routine firefighting toward systemic improvements like better capacity planning, architecture changes, and proactive observability, which reduce how often those issues arise in the first place.


## Flag risky IaC changes before they reach production


Even when you can rapidly identify and remediate them in production, IaC changes can still trigger outages, degrade performance, and consume on-call time. This makes the pull request the most reliable checkpoint for catching IaC issues safely. Proactive Remediation extends Bits Infrastructure Operations into the pull request workflow, catching issues before they are ever merged.


When a developer opens a Kubernetes or Helm PR, Bits analyzes the proposed change against live Datadog telemetry data, including CPU and memory usage, pod counts, and historical traffic patterns, to assess whether the change is safe to deploy. Unlike static configuration linters, Bits grounds every finding in real production behavior: A CPU limit that looks reasonable in isolation may be flagged as unsafe if Datadog shows the service regularly peaks above it. Findings appear as PR comments with supporting evidence and a suggested fix, giving engineers the context to act before a change reaches production.


Bits Infrastructure Operations also handles other common scenarios, such as insufficient replica headroom for node drains or traffic spikes, minimum[Horizontal Pod Autoscaler (HPA)](https://www.datadoghq.com/blog/kubernetes-autoscaling-datadog/#use-the-horizontal-pod-autoscaler-to-fit-pod-replica-count-to-demand) replica count set below the historically required pod count, or the readiness probe path changed from a known-healthy path. By combining automatic remediation with guardrails and reviewable history, Bits Infrastructure Operations helps platform teams move routine remediation out of their daily interrupt queue. Engineers can spend less time repeating known fixes and more time improving the systems that generate those issues in the first place.


## Start remediating infrastructure issues with Bits


Bits Infrastructure Operations helps your team move more quickly without getting bogged down in repetitive investigations and remediations. It detects issues across infrastructure domains, explains what happened, recommends next steps, and either takes action automatically or asks for approval based on the guardrails you define.


This helps platform teams reduce repetitive investigation and remediation work while giving application teams clearer context when infrastructure issues affect their services. Bits Infrastructure Operations works across all infra types and is available wherever you work (Datadog, Slack, or Claude Code).


To get started with Bits Infrastructure Operations, sign up for the[Preview](https://www.datadoghq.com/product-preview/bits-infrastructure-operations) . If you’re new to Datadog,sign up for a 14-day free trial.


-
-
-
