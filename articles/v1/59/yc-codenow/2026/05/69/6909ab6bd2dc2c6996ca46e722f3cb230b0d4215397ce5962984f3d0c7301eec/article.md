---
schema_version: "1.0.0"
document_id: "6909ab6bd2dc2c6996ca46e722f3cb230b0d4215397ce5962984f3d0c7301eec"
company_key: "yc-codenow"
company: "CodeNow"
source_id: "yc-codenow-news-import-a318b6d3535f"
canonical_url: "https://www.codenow.com/blog/release-notes-codenow-7-18---may-2026"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-08-09T20:50:58.691009+00:00"
fetched_at: "2026-08-09T20:50:59.843294+00:00"
content_hash: "sha256:c3acbb16f90cf821210a4432c1208563e83a010c7a032b078559efdae79c0885"
---

# Release Notes CodeNOW 7.18 – May 2026

Release Notes


May 15, 2026


# Release Notes CodeNOW 7.18 – May 2026


CodeNOW 7.18 introduces account-wide labels for all resources, simultaneous deployment to multiple environments, and a new MCP server that wraps the CodeNOW API for AI agents.


## Labels for All CodeNOW Resources


Labels are now available across every CodeNOW resource type, providing a single, consistent way to organize, filter, and group entities throughout an account. Teams can attach the same label to applications, components, libraries, containers, deployments, environments, packages, builds, managed services, and more.


### Key Capabilities:


- **Universal Resource Coverage:** Attach labels to applications, application components, libraries, containers, environments, deployments, packages, builds, managed services, and CI configurations
- **Centralized Label Management:** Create, update, and delete labels from a single account-wide registry with consistent naming and color coding
- **Multi-Resource Operations:** Attach or detach labels across multiple resources of different types in a single operation
- **Powerful Filtering:** Filter every resource list by one or more labels to quickly narrow down large catalogs
- **Bulk Tagging:** Apply labels to multiple resources at once to standardize organization at scale
- **Clear Visual Identification:** Color-coded labels make ownership, lifecycle stage, or domain context recognizable at a glance
- **Permission-Aware Management:** Label administration follows the standard CodeNOW permission model


### Benefits:


- Consistent organization model across the entire platform — one mental model for every resource type
- Faster navigation and discovery in accounts with hundreds or thousands of resources
- Improved collaboration across teams through shared tagging conventions (team ownership, environment, criticality, compliance scope)
- Better governance and reporting by grouping resources for cost, security, or operational reviews
- Reduced onboarding time — new team members can locate the right resources by their labels
- Foundation for future automation that can target resources by label selectors


## Deployment to Multiple Environments


Deployments can now be promoted to several environments at the same time. Instead of repeating the same release process for every target environment, teams configure the set of target environments once and trigger a single, coordinated deployment.


### Key Capabilities:


- **Multi-Environment Targeting:** Select multiple environments as deployment targets in a single deployment configuration
- **Coordinated Rollout:** Trigger a single deployment action that fans out to every selected environment
- **Per-Environment Status:** Track progress, health, and outcome for each target environment independently from a unified view
- **Consistent Configuration:** The same deployment definition is applied across all targets, eliminating drift between environments
- **Environment-Specific Overrides:** Per-environment configuration values are still respected during the multi-environment rollout
- **Unified History:** All environments deployed in one action share a common deployment record for auditability


### Benefits:


- Significantly faster promotion across staging, QA, pre-production, and production environments
- Reduced operational toil — one click instead of repeating the same workflow for every environment
- Lower risk of human error and configuration drift between environments
- Better release coordination for teams running parallel test environments or multi-region setups
- Clear, side-by-side visibility of how a single release performs across the environment landscape
- Faster feedback loops in testing pipelines that depend on multiple environments being updated together


## CodeNOW MCP Server


CodeNOW now ships with an official MCP (Model Context Protocol) server that wraps the CodeNOW API. AI assistants and agents — including Claude, Claude Code, and other MCP-compatible clients — can interact with CodeNOW resources directly through standardized tools.


### Key Capabilities:


- **Full API Coverage:** MCP tools cover applications, components, libraries, containers, deployments, environments, packages, builds, CI configurations, managed services, labels, teams, and more
- **Standardized Tool Interface:** Each CodeNOW operation is exposed as a discrete MCP tool with typed parameters and structured responses
- **Account-Aware Sessions:** Switch between accounts and operate within the correct account context from a single MCP connection
- **Read and Write Operations:** Query the state of resources, trigger builds, manage deployments, and update configurations through the same protocol
- **MCP-Compatible Clients:** Works with Claude Desktop, Claude Code, and any other client that supports the Model Context Protocol
- **Consistent Authentication:** Uses standard CodeNOW credentials and respects the existing permission model


### Benefits:


- Build AI-assisted workflows on top of CodeNOW without writing custom API integrations
- Let AI agents perform real platform operations — from listing resources to triggering deployments — under the user's existing permissions
- Faster prototyping of automations and assistants that combine CodeNOW with other MCP servers
- Lower barrier for teams to add conversational interfaces to their delivery workflows
- Consistent, machine-readable contract for every CodeNOW operation, simplifying integration testing and tooling
- Foundation for richer AI features such as guided troubleshooting, release summaries, and natural-language platform administration


Written by CodeNOW
