---
schema_version: "1.0.0"
document_id: "38e78b7a4b27bd4ddb46dfc6dd9aee0eaf3d620edd9faef019d20232b20d2e8d"
company_key: "yc-codenow"
company: "CodeNow"
source_id: "yc-codenow-news-import-a318b6d3535f"
canonical_url: "https://www.codenow.com/blog/release-notes-codenow-7-10---march-2025"
published_at: "2025-03-01T00:00:00+00:00"
first_seen_at: "2026-08-09T20:50:58.691009+00:00"
fetched_at: "2026-08-09T20:50:59.843294+00:00"
content_hash: "sha256:faf0b4ba4470c70e26fb3187ad7c0320753e7095432578875861d31e7e07193d"
---

# Release Notes CodeNOW 7.10 – March 2025

Release Notes


March 1, 2025


# Release Notes CodeNOW 7.10 – March 2025


CodeNOW now supports Bitbucket Cloud and Bitbucket Data Center as Git providers.


## Support BitBucket Cloud and BitBucket DataCenter as Git providers


CodeNOW now integrates with "Bitbucket Cloud and Bitbucket Data Center as Git providers" enabling developers to manage repositories and DevOps workflows seamlessly.


### Key Capabilities:


- **Repository Integration:** Connect Bitbucket Cloud or Data Center repositories for version control and CI/CD workflows
- **Authentication & Access Management:** Support for OAuth, personal access tokens (PATs) for Cloud, and native authentication for Data Center
- **Pipeline Compatibility:** CodeNOW CI/CD pipelines work with Bitbucket repositories for builds and deployments
- **Branching Strategy Support:** Feature branches, pull requests, and mainline development within CodeNOW
- **Automatic Webhook Configuration:** Webhooks trigger builds and deployments based on repository events


## Environment Quality Gates - Deployment Approvals


A new feature introducing "Deployment Approvals" that enforces the 4-eyes principle—requiring approval from a second team member before deploying to selected environments.


### Key Capabilities:


- Define environments requiring mandatory additional approval steps
- Enable deployment approvals per application
- Second team member must review and approve requests
- Complete audit trail tracking approvals and denials for compliance


## User Task List - Pending Approvals Overview


The User Task List provides personalized task visibility, notifying users of pending approvals requiring action.


### Key Capabilities:


- Displays pending deployment approvals
- Shows merge requests needing user attention
- Centralizes workflow management
- Prevents delays through improved visibility


## Account-Wide Permissions


Administrators can now grant specific permissions to users beyond default admin and owner roles.


### Key Capabilities:


- **Report Viewer Role:** Access to user and application activity logs
- **Custom External Service Template Creator:** Permission to create new external service templates
- Granular access control maintaining security and governance


## Redesigned Account Settings Page


The Account Settings interface now features organized tabs for improved navigation:


- **Permissions:** Account-wide permission management
- **Users and Teams:** User access and team configuration
- **Integrations:** External service integration setup
- **Continuous Integrations:** CI-related settings
- **Templates:** Application and library template management
- **Artifact Repositories:** Dependency resolution configuration


## Remote Debugging with Mirrord


CodeNOW introduces remote debugging capabilities enabling developers to "debug their services running inside a Kubernetes cluster directly from their local development environment."


### Key Capabilities:


- Mirrord tool integration in application templates
- Environment-specific debugging configuration
- Cluster-wide service access for local development
- Real-time service inspection and troubleshooting


Written by CodeNOW
