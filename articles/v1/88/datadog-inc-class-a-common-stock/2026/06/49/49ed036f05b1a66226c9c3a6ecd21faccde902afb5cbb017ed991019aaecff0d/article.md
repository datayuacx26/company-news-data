---
schema_version: "1.0.0"
document_id: "49ed036f05b1a66226c9c3a6ecd21faccde902afb5cbb017ed991019aaecff0d"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/datadog-api-authentication/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:b39424b93340e4efca19cee1699e1119b1ae0900438d441134946f3a2e9a9403"
---

# Modernize Datadog API authentication with scoped credentials

Anthony Dagneaux


Bowen Chen


Senior Technical Content Writer


For years, authenticating to[Datadog APIs](https://docs.datadoghq.com/api/latest/?tab=java) meant managing two separate credentials: an API key and an application key. That model worked well when most API activity came from developers running scripts or a small number of integrations. But modern infrastructure looks very different. Today, Datadog APIs are accessed by[CI/CD pipelines](https://www.datadoghq.com/blog/best-practices-for-ci-cd-monitoring/) , Terraform workflows, autonomous systems, and increasingly, AI agents operating across teams and environments.


To support those new access patterns, Datadog is introducing a new authentication model built around four purpose-specific approaches:[Personal Access Tokens (PATs)](https://docs.datadoghq.com/account_management/personal-access-tokens/) ,[Service Access Tokens (SATs)](https://docs.datadoghq.com/account_management/service-access-tokens/) , workload identity federation, and customer-managed OAuth clients. Together, these capabilities provide scoped, identity-aware authentication designed for both human users and non-human workloads.


In this post, we’ll discuss:


-


Why Datadog is updating its API authentication


-


Datadog’s new API authentication model


-


What happens to existing application keys


-


How to choose the right authentication model for your use case


## Why Datadog is updating its API authentication


The original Datadog authentication model separated telemetry ingestion from API authorization. API keys identified the Datadog organization receiving data, while application keys identified the user performing an action. This dual-key model worked well when user identities mapped to individual developers. However, this model did not map cleanly to automated workflows such as CI/CD pipelines, which do not have a single human owner.


AI agents acting on behalf of human users introduced another challenge for the existing model. When[the Datadog MCP Server](https://docs.datadoghq.com/bits_ai/mcp_server/) calls an API to retrieve a monitor’s status, how do we determine who authorized the request, what scope was granted, and how long that access should last? Security and compliance teams also need to know exactly *who* accessed *what* data, and *when* that access occurred. Application key usage alone does not provide enough context for detailed audit attribution.


To address these challenges, Datadog is introducing authentication models designed around specific access patterns. Instead of relying on a single credential type for every workflow, teams can now use credentials tailored to developers, automation systems, cloud-native workloads, and delegated application access. In the following sections, we’ll look at how each authentication model works and where it fits best.


## Introducing Datadog’s new API authentication model


Our updated authentication model replaces the application key with four targeted alternatives, each designed for a specific access pattern.


### Personal Access Tokens: for developer workflows


PATs are designed for developers who directly interact with Datadog APIs. They are intended for user-driven workflows such as building CLI tools, writing scripts against the API, or pulling data from Datadog into custom applications and dashboards.


PATs are scoped to the permissions of the user who creates them and require an expiration window to avoid reliance on long-lived credentials. Because PATs are tied directly to a user identity,[Audit Trail](https://docs.datadoghq.com/account_management/audit_trail/) logs can attribute API activity to a specific user. When a user loses access to an organization, their PATs are revoked alongside their account access.


### Service Access Tokens: for automation and autonomous AI agents


SATs are designed for automation systems and non-human workloads that need persistent access to Datadog APIs. Common examples include CI/CD pipelines, Terraform runs, infrastructure automation, and autonomous AI agents operating without a human in the loop.


SATs are tied to a service account, not an individual user. The service account has its own permissions, independent of any individual on your team. Like PATs, SATs are scoped and support configurable expiration policies. Unlike old application keys, SATs include readable prefixes (` ddsat_` ) and identifiable public portions, making credentials easier to track and manage.


### Workload identity federation: for cloud-native workloads


If your workload runs on AWS,[it already has an IAM identity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) . Workload identity federation, a system that establishes trust between two parties for user authentication and resource access, enables AWS workloads to authenticate directly to Datadog by binding an AWS IAM role to a Datadog service account. Instead of creating and storing static Datadog credentials, the workload uses its existing cloud identity to access Datadog APIs.


This removes the need to store API credentials in secrets managers, CI/CD pipelines, or configuration files for workloads such as Terraform runners and the Datadog Agent. Teams can manage access through existing IAM policies rather than rotating and distributing long-lived credentials.


Workload identity federation is generally available for AWS workloads, with support for additional cloud providers planned for the future.


### OAuth clients: for delegated and temporary AI agent access


OAuth clients are designed for AI agents that need temporary, scoped access to Datadog APIs. OAuth supports both **delegated access** , where an application acts on behalf of a user, and **machine-to-machine (M2M) access** for autonomous systems operating without a human in the loop.


In delegated workflows, users explicitly approve the scopes an application or AI agent can access before the application receives a short-lived token. This gives organizations more granular control over who authorized the request, what permissions were granted, and how long that access should last.


For autonomous systems, Datadog is actively investing in M2M OAuth support built around dedicated agent identities rather than long-lived shared credentials. OAuth clients also provide a foundation for emerging interoperability standards such as Okta Cross-App Access, helping AI agents operate across enterprise tooling without accumulating standing access. OAuth client support is planned for release later this year.


## What happens to application keys?


Application keys will continue to work after Q3 2026, but they will be considered legacy features and no new capabilities will be added. There will not be a forced migration or any immediate impact on existing integrations. Our new authentication model covers almost every use case application keys previously handled, with better scoping, clearer identity, and proper time-to-live (TTL) support. We encourage customers to migrate to the new authentication model as soon as possible to begin taking advantage of these added benefits.


## How to choose the right credential for your use case


With multiple authentication options available, the right credential depends on who is accessing Datadog APIs, how long that access should last, and where the workload is running.


**1. Who is authenticating?**


-


A developer directly accessing Datadog APIs → PAT


-


A Datadog first-party client (e.g.,[pup-cli](https://github.com/DataDog/pup) , Datadog MCP Server) → OAuth


-


An automated pipeline or service → SAT


-


An autonomous AI agent operating without a human in the loop → SAT today; M2M OAuth support planned


-


A cloud-native workload running on AWS → workload identity federation


-


An application or AI agent acting on behalf of a user → OAuth


**2. How long does access need to last?**


-


Short-lived or interactive → PAT


-


Long-running service → SAT


-


Temporary delegated access → OAuth client


-


Ephemeral AWS workloads without stored credentials → workload identity federation


**3. What are they accessing?**


-


Datadog APIs (configuration, querying, management) → PAT, SAT, OAuth, or Workload Identity Federation


-


Intake endpoints (metrics, logs, traces, agent data) → API key (this remains the same)


**4. From what environment?**


-


Developer laptop or local tooling → PAT


-


CI/CD system or pipeline → SAT


-


AWS infrastructure → workload identity federation


You can also refer to the following reference table to help you get started:


Use Case Credential Why


Developer script / CLI tool PAT Tied to your identity, scoped, short-lived


CI/CD pipeline / Terraform SAT Service account identity, fully auditable


Autonomous AI agent (headless, no user in the loop) SAT (today) → M2M OAuth (coming) SATs support persistent automation today; OAuth supports temporary per-run access


Datadog Agent or Terraform on AWS (more providers coming) Workload Identity Federation No credentials to manage


A Datadog first-party client OAuth client Delegated, no credentials to manage


AI agent acting on behalf of a user OAuth client Delegated, user-authorized, scoped


Submitting metrics, logs, or traces API key Unchanged, optimized for high-throughput intake


## Modernize Datadog API authentication


Modern infrastructure requires authentication models designed for both human users and automated systems. With PATs, SATs, workload identity federation, and upcoming OAuth support, Datadog’s new authentication model gives teams more granular control over identity, credential scope, and access life cycles across developer workflows, automation systems, and AI agents.


To learn more about PATs and SATs,[check out our documentation](https://docs.datadoghq.com/account_management/personal-access-tokens/) . If you’re new to Datadog,sign up for a free 14-day trial to get started .


-
-
-
