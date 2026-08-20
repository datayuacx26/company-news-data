---
schema_version: "1.0.0"
document_id: "51cbd03f457d87efa208f254603ed0d7f69e166656f3f4fdf68b6817b55ca2df"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/november-product-updates"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:60fe6e9015a605ff97449009679d3421822f58469995d887e0a477440afbab6f"
---

# November Product Announcements | Elementary Data

‍


## **Deeper Visibility Into Your Pipeline**


### 📡 Observability for Non-dbt Assets


You can now **sync tables from your data warehouse that are not part of a dbt project** directly into Elementary. These non-dbt assets appear in the same catalog and lineage graph as your dbt models, making it easier to understand how raw sources, staging layers, and ad-hoc tables fit into the broader pipeline.


You can also **configure volume and freshness tests** on these tables, just as you would with dbt models.


Learn more[here](https://docs.elementary-data.com/cloud/features/anomaly-detection/monitor-dwh-assets) .


### **🌐 Multi-Project Support**


You can now bring **multiple dbt projects into one Elementary environment** , giving you a unified view of data health across layers, domains, and teams. This unlocks cleaner architectures: run bronze, silver, and gold as separate projects, reuse the same raw data across different pipelines, or organize work by domain.


Elementary now understands assets by their **full relation name** , so lineage connects cleanly across projects and issues can be traced much further upstream. Each project stays connected to its own repo, so everything remains grounded in code.


Reach out to the team to set up on your environment. Self-serve setup coming soon!


Learn more[here](https://docs.elementary-data.com/cloud/features/multi-env) .


‍


## **Navigation Enhancements**


### **📁 Saved Views**


You can now create and save custom views for your assets and tests, and those filters will stay with you as you move through the app. Saved views make it easy to stay in the right context without rebuilding filters each time.


Sharing views with teammates through a link will be available soon.


Also, side trees were added to the main dashboard and the Data Health screen to improve navigation flow.


‍


## Alert Improvements


### **✉️ Email Alerts**


You can now send alerts by email, giving your team the flexibility to choose the workflow and distribution that fits how your organization operates.


Learn more[here](https://docs.elementary-data.com/cloud/integrations/alerts/email) .


‍


### **🔔 Send Resolved Alerts to Channel**


“Resolved” **thread replies can now be sent to the channel** , not just in the thread, making it easier to follow issues and scan your Slack channel like a feed.


Reach out to us to activate on your account.


## MCP


### **🔐 MCP Account-Wide Tokens**


Several customers are using the Elementary MCP in their internal agents. Now, you can generate account-wide MCP access tokens, making it easier to connect shared tools and internal AI agents.


Learn more[here](https://www.notion.so/November-Product-Updates-29f621a084bf8009b623d18d380e9e83?pvs=21) .


### **🧩 MCP Recommended Rules**


We’ve added a set of recommended MCP rules. These rules make it easier for tools like Cursor to understand your use cases and provide better, more relevant assistance.


You can explore the full list[here](https://docs.elementary-data.com/cloud/mcp/recommended-rules) .


### **📊 dbt Anomaly Detection Improvements**


A new flag now lets you cleanly separate the training period from the detection window, eliminating overlap and delivering sharper, more reliable anomaly signals.


Reminder: Elementary no longer opens automatic PRs for new dbt package versions.Elementary dbt package 0.21.0 is released, make sure to update your project.


Learn more[here](https://docs.elementary-data.com/data-tests/anomaly-detection-configuration/exclude_detection_period_from_training) .


### **🔷 Native Atlan App**


**Elementary is now available as a native app in the Atlan Marketplace** , bringing data reliability signals to business users, and directly into catalog and governance workflows.


You can also control the sync cadence, choosing between a daily refresh or syncing after every pipeline run, so the information in Atlan always reflects the state of your data.


‍
