---
schema_version: "1.0.0"
document_id: "585e07da2000d0ad1c5933936aceb035a627fe8d87bca1a50c11e38b16f1bff4"
company_key: "yc-codenow"
company: "CodeNow"
source_id: "yc-codenow-news-import-a318b6d3535f"
canonical_url: "https://www.codenow.com/blog/release-notes-codenow-7-14---october-2025"
published_at: "2025-10-16T00:00:00+00:00"
first_seen_at: "2026-08-09T20:50:58.691009+00:00"
fetched_at: "2026-08-09T20:50:59.843294+00:00"
content_hash: "sha256:1b116dea03105c9f93f772ba647c873a078b2e3d6995994e0ea197e74358591b"
---

# Release Notes CodeNOW 7.14 – October 2025

Release Notes


October 16, 2025


# Release Notes CodeNOW 7.14 – October 2025


Users can now maintain a single identity across multiple accounts, eliminating the need to create and manage separate identities for each account.


## Unified Identity Management


Users can maintain a single identity across multiple accounts, eliminating the need to create and manage separate identities for each account.


### Key Capabilities:


- **Single Identity for Multiple Accounts:** Link one user identity to multiple CodeNOW accounts, simplifying access management
- **Default Account Selection:** Configure which account automatically activates upon login for streamlined access to the primary workspace
- **Centralized Profile Management:** Maintain user profile information in one place across all associated accounts
- **Streamlined Authentication:** Log in once and access all linked accounts without switching identities
- **Seamless Account Switching:** Easily switch between different accounts while maintaining the same user identity
- **Consolidated User Experience:** Consistent identity information across all account contexts


### Benefits:


- Reduced administrative overhead by eliminating duplicate identity management
- Faster access to the primary workspace through automatic default account activation
- Improved user experience with simplified login and account access
- Better security through centralized identity control and reduced credential sprawl
- Enhanced productivity by eliminating manual account selection after each login
- Enhanced user tracking and audit capabilities across multiple accounts
- Lower maintenance burden for users managing multiple account memberships


## Cloud-Control Account for Enterprise Management


Enterprise installations of CodeNOW now include a dedicated cloud-control account providing centralized administration capabilities for managing all accounts and dataplanes across the entire CodeNOW installation.


### Key Capabilities:


- **Centralized Account Management:** Oversee and administer all CodeNOW accounts from a single cloud-control interface
- **Multi-Account Administration:** Manage multiple account configurations, settings, and resources across the enterprise installation
- **Unified Dataplane Management:** Configure, monitor, and maintain dataplanes for all accounts from one centralized location
- **Enterprise-Wide Visibility:** Gain complete oversight of all accounts and infrastructure components
- **Administrative Control Hub:** Access dedicated admin tools and features specifically designed for enterprise-level management


### Benefits:


- Reduced operational complexity through centralized administration
- Improved efficiency by managing all accounts and dataplanes from a single location
- Better governance with enterprise-wide visibility and control
- Streamlined dataplane maintenance and configuration management
- Enhanced scalability for large multi-tenant CodeNOW deployments
- Lower administrative overhead for enterprise installations
- Faster troubleshooting and support with consolidated management tools


## Resource Type Filtering for User Permissions


Account administrators can filter permissions by CodeNOW resource type when configuring individual user access, enabling more efficient permission management and faster administrative workflows.


### Key Capabilities:


- **Resource Type Filter:** Quickly filter permissions by specific CodeNOW resource types (components, libraries, external services, domains, etc.) when managing user access
- **Streamlined Permission Assignment:** Focus on relevant permissions for specific resource categories without scrolling through unrelated options
- **Enhanced Permission Discovery:** Easily locate and review permissions associated with particular resource types
- **Granular Access Control:** Configure user permissions with better visibility into resource-specific access rights
- **Improved Administration Workflow:** Reduce time spent navigating permission lists through targeted filtering


### Benefits:


- Faster permission configuration for individual users through targeted filtering
- Reduced administrative time when managing complex permission structures
- Improved accuracy in permission assignments with better visibility
- Enhanced user experience for administrators managing multiple user accounts
- Better governance through more efficient permission review processes
- Simplified onboarding when setting up permissions for new team members


## Enhanced Preview URLs with Custom Domains


Preview URLs displayed in deployment details now include custom domains connected to the deployment, providing immediate access to applications through their production-ready domain names alongside existing CodeNOW technical domains.


### Key Capabilities:


- **Custom Domain Display:** Preview URLs now show all custom domains associated with the deployment in addition to technical domains
- **Multi-Domain Visibility:** View both CodeNOW technical domains and custom domains in a single, consolidated preview URL section
- **Direct Production Access:** Click custom domain URLs to immediately access applications through their configured production domains
- **Enhanced Deployment Overview:** Deployment details provide complete visibility of all access points for the application
- **Preserved Technical Domains:** CodeNOW technical domains remain available for development and testing purposes
- **Contextual Domain Information:** Easily identify and access the appropriate domain for different use cases


### Benefits:


- Improved user experience with direct access to production-ready URLs
- Faster deployment verification through custom domain links
- Better visibility of all application access points in one location
- Reduced confusion about which URL to use for production access
- Enhanced collaboration by sharing production URLs directly from deployment details
- Streamlined testing and validation workflows with multiple domain options readily available


## MariaDB Service Discontinuation


CodeNOW will discontinue provisioning new MariaDB managed component instances. All currently running MariaDB instances will continue to operate without interruption and remain fully supported, requiring no immediate action from teams using existing deployments. However, customers will no longer be able to create new MariaDB instances through CodeNOW. For new database deployments, teams should utilize other supported database services such as PostgreSQL.


Written by CodeNOW
