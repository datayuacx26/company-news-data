---
schema_version: "1.0.0"
document_id: "6e9f59fc15109007909a980b4c5f90bdb6c493257553ef57755fe172c8a8fbc5"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/infracost-now-tracks-10-million-cloud-service-skus"
published_at: "2026-03-03T07:38:29+00:00"
first_seen_at: "2026-07-23T23:57:22.274812+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:55c7762e65f02e4889759ccf97e2e02c8fcad1cc95e10287fc9106dffc607a67"
---

# Infracost Now Tracks 10 Million Cloud Service SKUs So You Don’t Have To

Over the years, cloud pricing has grown to be very complicated. AWS alone publishes pricing across hundreds of services, each with dozens of instance types, storage tiers, data transfer rates, licensing models, and regional variations. Azure and Google Cloud aren’t far behind. If you subscribe to their notifications, you’ll likely get dozens of emails about changes each year.


Infracost keeps up with the complexity so you can focus on shipping. Our service now tracks **more than 10 million cloud service SKUs** across AWS, Azure, and GCP. Everything continuously as providers add, change, and deprecate pricing.


We get it — that number sounds ridiculous. The math is pretty straightforward though. For example, a single EC2 instance type like` m6i.xlarge` isn’t one SKU. It’s dozens—one for each region, one for on-demand vs. reserved vs. spot, one for Linux vs. Windows, and so on. Multiply that across hundreds of instance families, then add hundreds of services across three cloud providers, and the complexity adds up.


With 10 million SKUs in our pricing database, Infracost can show engineers the cost of any infrastructure changes before code ever ships to production — no spreadsheets, no manual lookups, no surprises at month-end.


## So where are the big gotchas among these 10 million?


We’d be shocked to see any company using even half of the services available on AWS, Azure, or GCP. After all, there are hundreds. So in the spirit of making FinOps actionable, we’ve identified the cloud cost ptifalls we see most often — and why they’re so hard to catch without shifting FinOps left in your CI/CD pipeline.


### 1. Running Previous-Generation EC2 Instance Families


AWS regularly releases newer instance generations that are faster *and* cheaper than their predecessors. An` m4.large` costs more and performs worse than an` m6i.large` . But if no one flags it, engineers keep provisioning what they know. They’ll copy and past a code block that worked before, not realizing that there are better alternatives. Infracost surfaces instance generation warnings directly in pull requests, where the fix is easy.


### 2. Multi-AZ SQL in Dev Environments


Multi-AZ replication makes sense in production. In a dev or staging environment where an outage means a brief inconvenience rather than a customer incident, it’s just a cost multiplier. We see this constantly—configurations copy-pasted from prod templates, with nobody noticing the redundancy setting that doesn’t belong in a dev namespace. Oops.


### 3. Monitor Log Groups Retaining Logs Indefinitely


Logs are useful. Logs from three years ago that nobody queries are just expensive. It’s common to see a monitoring tool with default retention settings are often left untouched, accumulating data that adds to your bill every month. Setting a sensible retention policy is a one-line change. It rarely happens without a nudge.


### 4. Container Image Registries That Keep Everything Forever


Every image push stays in the registry. Every version. Every failed build. ECR, ACR, and GCR all charge for storage, and container registries have a way of quietly accumulating gigabytes—sometimes terabytes—of images that no running container has referenced in months. Lifecycle policies fix this, but they have to be created deliberately.


### 5. gp2 EBS Volumes (and Why gp3 Matters)


AWS introduced` gp3` EBS volumes in 2020, offering better baseline performance at a lower price than gp2. Yet` gp2` remains the default in many Terraform modules, AMIs, and runbooks. This affects not just standalone EBS volumes but RDS storage and EMR clusters as well. A simple volume type change can cut storage costs by 20% or more with no other changes.


### 6. S3 Multi-Part Upload Debris


When a large file upload fails mid-transfer, the uploaded parts don’t disappear—they sit in S3, incomplete, accruing storage charges. For buckets used for large file uploads, this debris can accumulate into meaningful costs over time. An S3 lifecycle rule to abort incomplete multi-part uploads after a defined number of days eliminates this entirely.


### 7. Default RDS Instance Sizing


When an engineer creates an RDS instance and doesn’t change the default instance type—often` db.m5.large` —they’re frequently over-provisioning for what they actually need. Development databases, internal tools, and low-traffic applications don’t need the same instance class as production workloads, but defaults don’t know that. Without infrastructure cost visibility in the development workflow, over-sized databases go unnoticed.


### 8. The Test Environment That Was Never Torn Down


An engineer spins up a test environment, gets pulled into something urgent, and never shuts it down. Infracost surfaces the monthly cost estimate in the PR, and[tagging policies](https://www.infracost.io/docs/infracost_cloud/tagging_policies/) can require an` owner` and` environment` tag before the code even merges — making every test environment attributable and auditable from day one.


### 9. Data Transfer Costs


Engineers think about compute. They think about storage. They rarely think about egress. But data transfer costs cut across almost every AWS, Azure, and GCP service—and they’re notoriously hard to estimate in advance. Traffic between availability zones, cross-region replication, CDN egress, API Gateway responses—each carries its own per-GB pricing that only becomes visible after the bill arrives.


NAT gateways and VPC endpoints just compound the problem. They’re cheap to copy-paste into a Terraform module, which means they proliferate. Hundreds of them, sometimes thousands, each quietly accruing hourly charges whether they’re handling meaningful traffic or not. No single one breaks the budget. All of them together do.


### 10. New Services With No Cost Mental Model


AWS releases new services faster than most teams can track. Something like **AWS Verified Access** or **AWS Clean Rooms** may look like a straightforward feature addition—until the bill arrives and reveals a pricing model that nobody had mapped out. Engineers adopt new managed services without a clear sense of what they cost at scale. Without[cost guardrails](https://www.infracost.io/docs/infracost_cloud/guardrails/) , it’s hard to catch these mistakes until it’s too late.


## The Common Thread


These aren’t the result of careless engineers. They’re the result of engineers making reasonable decisions without cost context at the moment those decisions are made. The billing console is a lagging indicator. By the time the surprise shows up, the infrastructure has been running for weeks.


The fix isn’t better finance reviews after the fact. It’s giving engineers:


1.) real cost information earlier in the workflow—in the pull request, before the merge, when changing a volume type or disabling a flag is a two-minute edit instead of a production change requiring a maintenance window, and


2.) the appropriate tagging guidelines and cost guardrails so they don’t have to worry about getting in trouble.


## See What Infracost Catches in Your Infrastructure


Infracost integrates with your existing CI/CD pipeline and surfaces cost estimates—and cost regressions—directly in pull requests. Infracost Cloud enables your team to set up cost guardrails and FinOps policies, so teams can think about shipping instead of worrying about cloud costs after the fact.


**Ready to see the cloud waste hiding in your Terraform, CloudFormation, or CDK configs?**


[Sign up for Infracost](https://dashboard.infracost.io/) or[schedule a demo →](https://www.infracost.io/finops/#LiveDemo)
