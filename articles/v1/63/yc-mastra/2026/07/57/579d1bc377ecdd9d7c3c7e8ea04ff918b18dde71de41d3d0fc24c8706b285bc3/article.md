---
schema_version: "1.0.0"
document_id: "579d1bc377ecdd9d7c3c7e8ea04ff918b18dde71de41d3d0fc24c8706b285bc3"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/mastra-platform-pricing"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:3e7673db21f115aee673dcbe4d0b4cb14e5aa5666e60d076ab98b9b49aef8e24"
---

# How Mastra Platform Pricing Works

The Mastra platform is the best place to deploy, observe, and iterate on your agents. And we wanted to take a minute or two to help explain how billing works on the platform so that you can make the best decision about what plan you pick as you scale up usage.


By default, our platform is pay-as-you-go with monthly allowances to make sure you can test the platform before swiping a card. Everything runs on a single pool of credits, including model and token spend, so everything lives in the same place. Top up on credits whenever you need, or set an auto-top-up to keep up with your usage.


By default you're on **Starter** , which is free and pay-as-you-go. It comes with a monthly allowance baked in:


- 100K observability events, then $10 per 100K
- 24 CPU hours, then $0.35 per hour
- 10GB of data egress, then $0.10 per GB
- 15 days of data retention
- Unlimited users, projects, and deployments


You only start spending credits once you go past those amounts.


Once you're ready to ship or monitor your agents in production, the **Team** plan ($250/month) is usually the better deal. It includes larger monthly allowances and also discounts usage after that.


- 1M observability events, then $8 per 100K (down from $10)
- 250 CPU hours, then $0.25 per hour (down from $0.35)
- 100GB of data egress, then $0.08 per GB
- 6 months of data retention
- Multiple teams, SSO, and SOC 2 docs


Long story short, if your Starter usage is climbing toward $250 a month, you'll likely save money by switching.


A couple of other things worth knowing:


- **Server Always-On ($100/project):** servers run serverless by default, but this keeps yours warm around the clock and folds in a full month of CPU hours.
- **Idle behavior & CPU time:** even with no incoming requests your project may show CPU usage from background tasks like cron jobs, keep-alive pings, or memory synchronization. We explain exactly how idle and active time are metered in our docs.
- **Enterprise:** when you outgrow the rest, this adds volume discounts, RBAC, audit logs, SLAs, and a dedicated support engineer — just reach out to sales.


That's really it. Get started at[projects.mastra.ai](https://projects.mastra.ai/) , and if you want more details on our pricing plans and what's included, see our pricing page:[https://mastra.ai/pricing](https://mastra.ai/pricing)
