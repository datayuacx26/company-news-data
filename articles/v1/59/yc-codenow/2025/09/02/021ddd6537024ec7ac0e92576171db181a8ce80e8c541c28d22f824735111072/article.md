---
schema_version: "1.0.0"
document_id: "021ddd6537024ec7ac0e92576171db181a8ce80e8c541c28d22f824735111072"
company_key: "yc-codenow"
company: "CodeNow"
source_id: "yc-codenow-news-import-a318b6d3535f"
canonical_url: "https://www.codenow.com/blog/release-notes-codenow-7-13---september-2025"
published_at: "2025-09-01T00:00:00+00:00"
first_seen_at: "2026-08-09T20:50:58.691009+00:00"
fetched_at: "2026-08-09T20:50:59.843294+00:00"
content_hash: "sha256:78140fa89d437bb0ca5d020bfaab890a6802aa9d6a420ca3dfabe3a4e2569ebe"
---

# Release Notes CodeNOW 7.13 – September 2025

Release Notes


September 1, 2025


# Release Notes CodeNOW 7.13 – September 2025


CodeNOW users can now create and manage custom templates for application components and libraries.


## Custom Templates


CodeNOW users can now create and manage custom templates for application components and libraries, enabling standardized development patterns across teams and projects.


### Key Capabilities:


- **Template Creation** : Define reusable templates for application components and libraries directly within your account
- **Dry Run Testing** : Validate your templates before publishing with built-in dry run functionality to ensure they work as expected
- **Customizable Structure** : Configure template scaffolding, file structures, and boilerplate code to match your organization's standards
- **Team Standardization** : Ensure consistent coding patterns and project structures across development teams
- **Accelerated Development** : Reduce setup time for new components and libraries with pre-configured templates


### Benefits:


- Streamline component and library creation process
- Test and validate templates before deployment with dry run capability
- Maintain consistency across projects and team members
- Reduce repetitive setup tasks
- Enforce organizational coding standards and best practices


## Domain Management Redesign


The domain creation process has been completely redesigned to provide greater flexibility and control over gateway deployment and SSL certificate management.


### Key Capabilities:


- **Environment-Specific Gateway Deployment** : Choose the target environment where your domain gateway will be deployed
- **External Secret Store Integration** : Enhanced certificate and private key management with support for loading from external secret stores
- **Flexible Certificate Options** : Multiple methods for providing SSL certificates and private keys based on your security requirements
- **Dataplane-Dependent Features** : Certificate management options adapt based on the capabilities supported by your specific CodeNOW dataplanes


### Benefits:


- Improved security through external secret store integration
- Greater flexibility in gateway placement across environments
- Enhanced certificate management workflows
- Better alignment with enterprise security practices
- Streamlined domain configuration process


## CI Pipeline Duration


The application component dashboard now displays detailed duration information for CI pipelines, providing developers with immediate insights into build performance and optimization opportunities.


### Key Capabilities:


- **Real-Time Duration Metrics** : View current and historical CI pipeline execution times directly on the component dashboard
- **Performance Monitoring** : Track pipeline performance trends to identify bottlenecks and optimization opportunities
- **At-a-Glance Status** : Quick visibility into build times alongside existing pipeline status information
- **Historical Tracking** : Monitor duration changes over time to assess the impact of code changes on build performance


### Benefits:


- Improved visibility into CI/CD pipeline performance
- Faster identification of build performance issues
- Data-driven optimization of CI pipeline configurations
- Enhanced developer productivity through better build insights
- Proactive monitoring of deployment pipeline health


## Account-Wide Permission Management


Enhanced permission system that extends previously admin-only capabilities to any account user through granular permission assignments, enabling better delegation and role management across teams.


### Key Capabilities:


- **Custom Component Template Creator** : Grant users the ability to create and manage custom component templates
- **Custom Library Template Creator** : Assign permissions for creating and maintaining custom library templates
- **Custom External Service Template Creator** : Enable users to develop custom external service templates
- **Report Viewer** : Provide access to reporting functionality for non-admin users
- **Granular Permission Control** : Assign specific permissions independently, allowing for tailored role definitions


### Benefits:


- Enhanced team collaboration through flexible permission delegation
- Reduced administrative overhead by distributing template creation responsibilities
- Improved governance with role-based access to sensitive features
- Greater autonomy for development teams while maintaining security controls
- Scalable permission model that grows with your organization


## Audit Trail for External Services and Domains


Full audit logging for external service templates, external services, and domains, providing complete visibility into configuration changes for compliance, security, and troubleshooting purposes.


### Key Capabilities:


- **External Service Template Auditing** : Track all modifications to external service templates including creation, updates, and deletions
- **External Service Change Logging** : Monitor configuration changes to external service instances and their settings
- **Domain Modification Tracking** : Audit domain creation, updates, certificate changes, and gateway reconfigurations
- **User Attribution** : Record which user made each change with timestamps for accountability
- **Change History** : Maintain comprehensive history of all modifications for compliance and rollback purposes


### Benefits:


- Enhanced security through complete change visibility
- Improved compliance with audit trail requirements
- Faster troubleshooting with detailed change history
- Better governance and accountability for infrastructure changes
- Support for regulatory compliance and security audits


## Enhanced Git Committer Attribution


Improved Git integration that properly attributes commits to the actual CodeNOW users who made changes, ensuring accurate version control history and better traceability across all Git operations.


### Key Capabilities:


- **User Attribution Propagation** : Automatically propagate CodeNOW user information to Git commits for all resource modifications
- **Accurate Git History** : Maintain proper commit authorship that reflects the actual user who made changes through CodeNOW
- **Cross-Feature Integration** : Apply user attribution consistently across all CodeNOW features that trigger Git commits
- **Enhanced Traceability** : Link CodeNOW actions directly to corresponding Git commits with proper user identification
- **Version Control Integrity** : Ensure Git logs accurately represent who made what changes and when


### Benefits:


- Improved accountability through accurate Git commit attribution
- Better collaboration with clear visibility of who made specific changes
- Enhanced debugging and troubleshooting with proper change tracking
- Consistent user identification across CodeNOW and Git workflows
- Compliance with version control best practices and audit requirements


Written by CodeNOW
