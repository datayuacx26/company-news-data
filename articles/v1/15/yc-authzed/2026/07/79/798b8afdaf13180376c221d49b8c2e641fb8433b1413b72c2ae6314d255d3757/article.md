---
schema_version: "1.0.0"
document_id: "798b8afdaf13180376c221d49b8c2e641fb8433b1413b72c2ae6314d255d3757"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-news-import-a541a2e95b08"
canonical_url: "https://authzed.com/changelog/october-2025-release"
published_at: null
first_seen_at: "2026-07-24T17:47:31.723915+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:bc71573570a4e3412cb841bb5bbcd12f75aedb6dec884a19ce6b06cbeb2a283e"
---

# AuthZed October 2025 Release

This release brings improvements to SpiceDB's reliability, new AI-assisted development tools, enhanced visibility for cloud customers, and customer stories that showcase real-world authorization implementations.


### SpiceDB v1.46.0: Reliability and Performance Improvements


[SpiceDB v1.46.0](https://github.com/authzed/spicedb/releases/tag/v1.46.0)


**Better Handling of High Traffic:** SpiceDB now intelligently manages write loads by returning clear error messages when the system is busy. (Available when using CockroachDB as the datastore.) This means authorization decisions stay reliable during traffic spikes, preventing service disruptions across applications.


**Faster Permission Checks:** We've optimized how SpiceDB processes relationship queries, allowing it to skip unnecessary work when checking permissions.


**Improved Query Engine:** The Lookup Resources engine, which powers queries like "show me everything this user can access," now uses less memory.


**Note:** If you use the` zed` command-line tool, you'll need to update to[v0.33.0](https://github.com/authzed/zed/releases/tag/v0.33.0) for full compatibility.


### AuthZed Cloud & Dedicated Cloud: Enhanced Visibility and Control


For customers using AuthZed's managed services, this release brings improvements to monitoring, alerting, and operational visibility.


**Proactive Resource Monitoring:** A new warning banner in the dashboard now alerts you when permission system resource usage is high, giving you time to take action before issues arise. New response code graphs provide visibility into API performance, helping you understand how your authorization system is behaving.


**Expanded Monitoring Coverage:** Over 25 improvements were made to the monitoring infrastructure, including new alerts that notify teams when storage capacity exceeds 90%, when too many API requests fail, or when memory issues occur. The monitoring system now tracks performance across different regions and customer environments more precisely, helping ensure consistent service quality.


### AI-Powered Development with MCP Servers


We've launched two Model Context Protocol (MCP) servers that bring SpiceDB resources directly into your AI coding assistant:


- **AuthZed Docs MCP Server:** Access comprehensive SpiceDB documentation without leaving your development environment
- **SpiceDB Dev MCP Server:** Connect to a sandboxed SpiceDB instance for interactive schema development and permission testing


These tools work with AI assistants like ChatGPT and Claude Code to help you develop schemas, test permissions, and learn SpiceDB concepts through natural conversation. The` zed` CLI tool now includes experimental MCP agent support, enabling AI-assisted schema development right from your command line.


[Learn more about MCP integration](https://authzed.com/docs/mcp)


### Client Libraries: Updated API support


All AuthZed client libraries have been updated to support the latest SpiceDB v1.45.4 API changes. Here's what's new:


**Watch API Improvements:** The Watch API now includes expanded metadata, giving you richer context about permission changes as they happen. This is useful for building audit logs, real-time dashboards, or notification systems that react immediately to authorization updates.


**Language-Specific Highlights:**


- **Python:** Faster builds with modern dependency management and improved security for PyPI releases
- **Java:** Cleaner dependency management and new Watch API code examples
- **Node.js:** Better error handling with properly exported` ErrorReason` enum
- **Go, Ruby, .NET:** All updated with the latest API support


The` zed` CLI received reliability improvements, including more robust retry mechanisms for Watch functionality and better support for the latest SpiceDB versions.


### Revisiting Customer Stories


[Turo's Success Story](https://authzed.com/blog/turos-spicedb-success-story-how-the-leading-car-sharing-platform-transformed-authorization) : The world's leading car-sharing platform shared how they transformed their authorization system with SpiceDB and AuthZed Dedicated hosting. Their team-based permissions solution solved critical security challenges where fleet owners previously had to share passwords. After two years in production, they've experienced only one incident, demonstrating the reliability of managed hosting.


[Canva Insights on the Dual Write Problem](https://authzed.com/blog/the-dual-write-problem-in-spicedb-a-deep-dive-from-google-and-canva-experience) : Artie Shevchecko from Canva presented solutions to the dual-write problem when implementing centralized authorization. The talk showed how SpiceDB simplifies data replication challenges compared to traditional event-driven architectures.


Both presentations were part of the Authorization Infrastructure event hosted by AuthZed in August.


### Design & Brand Resources


We've launched a[brand guidelines](https://authzed.com/brand) page with downloadable assets including AuthZed and SpiceDB logos in multiple formats, complete color palette documentation, and usage guidelines.
