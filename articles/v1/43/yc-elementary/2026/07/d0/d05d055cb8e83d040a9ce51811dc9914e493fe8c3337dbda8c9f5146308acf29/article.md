---
schema_version: "1.0.0"
document_id: "d05d055cb8e83d040a9ce51811dc9914e493fe8c3337dbda8c9f5146308acf29"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/january-product-updates"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:452adf1f442d9e98ed86af0613afde695e6868bf2c29fed0c9859f2fa77634d1"
---

# January 2026 Product Updates: Incident Management, AI Agents & Non-dbt Alerts | Elementary Data

## 🚨 Incident Management Enhancements


**Two new features make it easier to track and collaborate on incidents:**


You can now add incident summaries in your own words to keep track of status, decisions, and next steps as you work through alert resolution.


The incident page also includes a full history view showing the timeline of where alerts were sent, what failures occurred, and how the incident progressed.


## 🤖 Easier Agent Access


**AI agents are now accessible from more places in the UI.**


You can explore and manage assets directly from the main dashboard or data health screen, and leverage test recommendations in the test configuration screen.


## 📡 Alerts for Non-dbt Assets


**You can now receive alerts for non-dbt assets synced from your data warehouse.**


Tables synced directly from your warehouse appear in the same catalog and lineage graph as your dbt models.


**Volume** and **freshness** monitors on these assets now trigger alerts through the same workflows as dbt models, giving you consistent visibility across your entire pipeline.


Learn more about monitoring DWH assets[here](https://docs.elementary-data.com/cloud/features/anomaly-detection/monitor-dwh-assets) .


## 📦 New OSS Version


**Elementary dbt package 0.22.1 is now available** , with a new execution SLA test (details below), minor improvements, community contributions and bug fixes. We always recommend updating to the latest Elementary dbt version.


**Want an automatic PR whenever there’s a new dbt version available?** Let us know and we’ll take care of it!


## **⏱️ Execution SLA Test**


**A new dbt package test verifies that models complete before specified deadlines.**


The` execution_sla` test checks whether your pipeline ran successfully before a deadline on specific days. Configure it with your SLA time, timezone, and optional day filters to monitor execution timing across your critical models.


Learn more[here](https://docs.elementary-data.com/data-tests/execution-sla) .


## 🔧 Self-Serve Configuration


**We're making Elementary easier to manage on your own.**


- **Last BI sync timestamp** now appears in the environments page, making it easy to check when your BI integrations last refreshed and verify everything is up to date.
- **Environment deletion** can now be done directly from the UI. An admin can now:Head over to the environments page —> Edit environment —> Delete.
- **SSO configuration** is now self-serve, giving you full control over authentication setup.Learn more[here](https://docs.elementary-data.com/cloud/integrations/security-and-connectivity/okta) .


## **Join our upcoming webinars:**


### 🐍 Elementary Python SDK


**February 11, 2026 | 11:00 AM EST**


See how the Elementary Python SDK brings data quality and observability into Python workflows, and how Python validations fit alongside dbt and warehouse checks.


Sign up[here](https://www.elementary-data.com/webinar/elementary-python-sdk) .


### 💬 How to Prepare Your Data Platform for Analytics Agents


**Co-hosted with Flexor CEO**


**February 25, 2026 | 11:00 AM EST**


Analytics is shifting from dashboards to agents. We'll cover what this means for data platforms, the hidden traps in "just connect agents to all systems," and what the AI-era data platform must include.


Sign up[here](https://www.elementary-data.com/webinar/how-to-prepare-your-data-platform-for-analytics-agents) .
