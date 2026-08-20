---
schema_version: "1.0.0"
document_id: "4831d20b187ed2edac75ceb2457ec3adad28b9d1fae671f4fc035b7bcae2bda6"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/managed-databases-updates-h2"
published_at: "2025-12-17T16:37:44.320+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:462a79212eef8a3c883e2b168914f310ae72051b559f3e5f7185711df39c373a"
---

# A Year of Innovation: DigitalOcean Managed Databases in 2025

2025 was a big year for DigitalOcean Managed Databases, packed with meaningful enhancements designed to deliver more power, flexibility, and simplicity for developers and businesses of all sizes. With performance boosts and new database engines introduced to improve scalability, automation, and observability, this year’s releases focused on making it easier than ever to build and run reliable data-backed applications on DigitalOcean. In this recap, we’ll walk through the most impactful launches in both the first and second half of the year and how they help teams move faster while keeping complexity low.


First, let’s take a look at[our releases in the first half of the year (H1)](https://www.digitalocean.com/blog/managed-databases-updates-h1) :


–[Managed PostgreSQL support for v17 \[February\]](https://www.digitalocean.com/blog/postgresql-17)


–[Managed MongoDB support for v8 \[March\]](https://www.digitalocean.com/blog/introducing-mongodb-8)


–[Support for up to 20TB for Managed MySQL and 30TB for PostgreSQL \[March\]](https://www.digitalocean.com/blog/larger-plan-sizes-for-mysql-and-postgresql)


–[Introducing DigitalOcean Managed Caching for Valkey \[April\]](https://www.digitalocean.com/blog/introducing-managed-valkey)


–[Introducing Role-Based Access Control to DigitalOcean Managed MongoDB with Predefined Roles \[May\]](https://www.digitalocean.com/blog/rbac-and-predefined-roles-for-mongodb)


–[Database Observability, Monitoring, and Hardening Advancements \[June\]](https://www.digitalocean.com/blog/managed-databases-observability-updates)


–[Support for Kafka Schema Registry \[July\]](https://www.digitalocean.com/blog/introducing-kafka-schema-registry)


The second half (H2) of 2025 saw even more big developments for DigitalOcean Managed Databases:


## MCP Server (August)


The DigitalOcean MCP (Model Context Protocol) Server lets you manage your cloud resources using simple, natural-language commands through AI-powered tools like Cursor, Claude, or your own custom LLMs. Running locally, it streamlines tasks such as provisioning Managed Databases, making cloud operations faster, easier, and more intuitive for developers. DigitalOcean MCP Server marks a major step forward in bringing AI directly into cloud infrastructure workflows. It allows users to seamlessly integrate cloud management into AI assistants or custom-built agents while eliminating context switching, manual API calls, and constant console navigation. MCP Servers make managing cloud infrastructure with LLMs faster, more secure, and far more accessible.


## Advanced configuration for Managed Databases UI (August)


We introduced advanced configuration options in the Managed Databases UI, giving developers more control without needing API calls or manual setup. With this update, you can fine-tune performance settings, customize connection parameters, and tailor your database environment directly from the dashboard. It’s a faster, more intuitive way to optimize your databases for your specific workloads.


## Storage autoscaling (October)


Launched at our Deploy conference in London, this feature marks another major milestone for automation in database environments. Storage autoscaling automatically increases a database’s storage size when needed, so users don’t have to do it manually. It’s an automated, proactive safety measure implemented to prevent downtime. This feature continuously monitors a database’s disk utilization, so when a configurable threshold is exceeded, it automatically scales up storage in the background. This seamless, automated process helps to ensure that a database remains available without requiring any manual intervention from users.


## PostgreSQL 18 (November)


DigitalOcean Managed PostgreSQL began supporting the latest version, PostgreSQL v18. This new version brings significant improvements to performance, developer experience, and reliability. Key features include a new Asynchronous I/O (AIO) subsystem that speeds up ready-heavy workloads, virtual-generated columns for more efficient schema design, and a new uuidv() function to improve performance for UUIDs.


## Support for Remote MCP Server (December)


DigitalOcean now offers support for Remote MCP Server, meaning you can connect your AI tools to DigitalOcean services without installing any binaries locally. Remote MCP endpoints are live for 9 DigitalOcean services: Accounts, App Platform, Databases, DigitalOcean Kubernetes, Droplets, Insights, Marketplace, Networking, and Spaces. Each service runs as its own MCP server at a dedicated HTTPS endpoint (example:[https://apps.mcp.digitalocean.com/mcp](https://apps.mcp.digitalocean.com/mcp) for App Platform). The setup is easy: all you need to do is update your MCP client configuration to point at our hosted endpoints, include your DigitalOcean API token and you’re ready to go with immediate, authenticated access to your infrastructure. Please note that all existing DigitalOcean MCP tutorials and videos continue to work–the only change is your mcp.json configuration. For teams, Remote MCP will simplify standardization, helping to ensure that every developer uses the same configuration without maintaining local tooling. Learn more about it in the blog post[here](https://www.digitalocean.com/blog/remote-mcp-server) , as well as check out our[documentation](https://docs.digitalocean.com/reference/mcp/configure-mcp/) for more information on how to get started.


## Improved migration tooling in the cloud console (December)


We enhanced our migration tooling in the Cloud Console, making it easier than ever to move data into DigitalOcean Managed Databases. The updated experience provides clearer guidance, improved validation, and a more streamlined, self-service migration workflow–ensuring you have everything you need to migrate databases to DigitalOcean with confidence.


Beyond migrations, we delivered several reliability and resiliency improvements across our managed database offerings. For MongoDB single-node clusters, we introduced automatic restarts, reducing manual intervention and improving availability during failure scenarios. We also shipped additional platform enhancements focused on single-node uptime, resulting in an improvement in availability from October 2024 to October 2025.


We further improved our mean time to detect (MTTD) database issues by enhancing monitoring and alerting for scenarios such as resource exhaustion, replication lag, and query performance degradation, enabling faster response and minimizing customer impact.


Finally, we introduced MySQL incremental backups, significantly improving backup speed and efficiency while giving customers greater control over their data protection strategy. Customers can now configure which days backups are taken, reducing overhead and ensuring backups better align with their operational needs.


## What’s to come in 2026


As we wrap up 2025, we’re excited about what 2025 has in store for our users. Here are some ways you can stay connected with us so you never miss what’s new with Managed Databases:


-


[Sail to Success Webinar series:](https://www.digitalocean.com/try/sailtosuccess) Sign up to be notified about our weekly webinars that we host with external guest speakers and some of DigitalOcean’s very own cloud experts.


-


[Sign up for our customer newsletter](https://www.digitalocean.com/blog) to get the monthly scoop on new releases, developer and community meetups, tutorials, and more.


-


[Visit our Managed Databases homepage](https://www.digitalocean.com/products/managed-databases) to learn more about use cases, case studies, and more.


-


Thinking of migrating?[Learn more about our Migrations Program](https://www.digitalocean.com/migrate) , where you can migrate to DigitalOcean for free.
