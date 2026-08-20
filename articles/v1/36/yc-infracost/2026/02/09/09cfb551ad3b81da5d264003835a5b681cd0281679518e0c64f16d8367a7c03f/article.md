---
schema_version: "1.0.0"
document_id: "09cfb551ad3b81da5d264003835a5b681cd0281679518e0c64f16d8367a7c03f"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/getting-to-the-root-of-cloud-waste"
published_at: "2026-02-05T14:53:24+00:00"
first_seen_at: "2026-07-23T23:57:22.274812+00:00"
fetched_at: "2026-07-28T22:21:32.388193+00:00"
content_hash: "sha256:e626cdfbcb36373e02d723f7b2ff1454bf61fc277d366b499f29b245d972b90b"
---

# Burst Pipes vs Leaky Faucets: Getting to the Root of Cloud Waste

As a FinOps team, it’s easy and exciting to chase the big wins. Someone accidentally left a cluster running over the weekend? All hands on deck. A data scientist spun up a` p4d.24xlarge` and forgot about it? Emergency Slack channel. Everyone loves catching the burst pipe.


But here’s the kicker: situations like this aren’t causing your budget overruns.


Your real problem is the 3,000 engineers at your company who, every single day, are making small decisions that each waste $50-200 a month. A slightly oversized EC2 instance here. An[unoptimized RDS configuration](https://www.infracost.io/finops-policies/amazon-rds-consider-single-az-databases-in-non-production/) . EC2 instances that[should be upgraded to Graviton](https://www.infracost.io/finops-policies/amazon-ec2-consider-graviton-instances/) . Load balancers that aren’t needed. It’s death by a thousand cuts, and most FinOps teams struggle to keep up with it.


## The “Leaky Faucets” Worst Offender List


**The Default Setting Problem** : Over[40% of cloud waste comes from overprovisioned resources](https://jaychapel.medium.com/overprovisioning-always-on-resources-lead-to-26-6-billion-in-public-cloud-waste-expected-in-2021-da888ea68f74) . When an engineer creates an RDS instance and accepts the default` db.m5.large` instead of properly sizing it, that’s $175/month of waste. Multiply that by hundreds of databases across your organization, and you’re bleeding $2M+ annually on defaults alone.


**The “I’ll Come Back to This” Tax** :[27% of cloud spend is wasted](https://info.flexera.com/CM-REPORT-State-of-the-Cloud) , with the majority being resources that are simply forgotten. An engineer spins up a test environment, gets pulled into something urgent, and never shuts it down. Each one is small—maybe $300/month—but with thousands of engineers, the money ads up.


**The Terraform Copy-Paste Effect** : Self-service infrastructure is powerful, but it can be risky from a cost perspective. We see it all the time. Perhaps an engineer writes a Terraform module that has an unnecessary NAT Gateway configuration. Out of convenience, other teams reuse that module 47 times across different projects before anybody notices. At $45/month per NAT Gateway, that’s over $25,000 annually from a single copy-paste mistake.


## Building Cloud Waste Prevention into IaC Workflows


The burst pipes get fixed because they’re obvious. Someone’s manager sees the AWS bill spike and demands answers. But the leaky faucets? Very difficult to fix with traditional FinOps processes. They show up as “expected growth” in your cloud spend.


That’s why shift-left makes so much sense for IaC workflows. When an engineer submits a PR that creates an oversized resource, they see the cost impact immediately. They can right-size it before it ever gets deployed, before it becomes part of the baseline. By making cost optimization part of the IaC workflow, engineers can build a cost optimization muscle without major interruptions in their day-to-day.


## Prioritizing Leaky Faucets


Self-service infrastructure requires a mindset shift for FinOps. It’s not just about wearing the emergency plumber hat and showing up after the damage is done. Start being the building inspectors who prevent bad infrastructure from being installed in the first place. If you catch one burst pipe a month, you might save $50,000. If you prevent 10,000 leaky faucets from ever being installed, you save millions.
