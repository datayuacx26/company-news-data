---
schema_version: "1.0.0"
document_id: "af3e122d8a8fff40e1e4a910c7a510a658cb68c15b9df92059a9a817ab59dfc2"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/are-you-paying-for-unnecessary-extended-support-fees"
published_at: "2026-03-13T09:15:39+00:00"
first_seen_at: "2026-07-23T23:57:22.274812+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:e463c530eb238a24839c427de3a5386849cc4b60f99bfee5f011c05b1d92e2fc"
---

# A Hidden Tax: Are You Paying for Unnecessary Extended Support Fees?

Version lifecycle management. It’s not the most glamorous part of DevOps, but it’s a lot easier when you write infrastructure as code. Upgrading instance types, load balancers, databases, or any cloud service becomes both easier and safer when it’s part of a GitOps workflow.


Avoiding upgrades isn’t just a security concern — it comes with an actual cost. All three major cloud providers — AWS, Azure, and Google Cloud — charge extended support fees when you continue running end-of-life versions of their managed services. These charges are automatic, often surprising, and compound quickly across your infrastructure footprint. For organizations running hundreds of clusters and database instances across multiple clouds, the math scales into the millions.


## The Hidden Surcharge You Didn’t Budget For


Cloud providers have gotten more aggressive about charging premiums for older service versions. Here’s what that looks like in practice.


-


**Amazon EKS** is one of the most striking examples. Running a Kubernetes cluster on a version within the standard support window costs $0.10 per cluster per hour. Once that version enters extended support, the price jumps to $0.60 per cluster per hour — a 6x increase. For a single cluster, that’s an extra $360 per month you weren’t planning for. Every 25 clusters running on extended support versions adds roughly $100,000 per year in control-plane fees alone.


-


**Amazon RDS** follows a tiered model. Once a database engine version like PostgreSQL 11 or MySQL 5.7 exits standard support, AWS charges an additional $0.10 per vCPU per hour for the first two years, then $0.20 per vCPU per hour in year three. For a single db.r6g.4xlarge instance with 16 vCPUs, that’s approximately $14,000 in additional annual charges — and the same fee applies to every standby in a Multi-AZ deployment and every read replica.


-


**Google Cloud** introduced extended support charges for Cloud SQL in May 2025. Any Cloud SQL instance running a major version that has reached end-of-life is automatically enrolled and billed at an additional per-vCPU-per-hour rate on top of your existing instance costs. GKE carries a similar cost structure for older Kubernetes versions, adding $0.50 per cluster per hour on top of the standard $0.10 rate — bringing the total to the same $0.60 per cluster per hour as EKS.


-


Microsoft Azure is rolling out extended support billing for Azure Database for MySQL through 2026. MySQL 5.7 standard support ends March 31, 2026, after which servers are automatically enrolled in extended support and billed per vCore-hour following a one-month grace period. Even versions as new as MySQL 8.0 follow close behind, with standard support ending May 31, 2026 and paid extended support beginning June 1, 2026. Both versions are covered by extended support through 2029.


In every case, enrollment is *automatic.* You don’t opt in. You just start paying.


## How You Know You Have Them?


Chances are you’ve already been warned. Cloud providers send email notifications when your services are approaching or have entered end-of-life status. AWS sends targeted emails about EKS cluster versions entering extended support. Google Cloud notifies you when Cloud SQL instances are running EOL major versions. Azure alerts you about upcoming support transitions for MySQL and PostgreSQL.


The problem is these emails often land in an infrastructure team inbox, get flagged as low-priority, or simply get lost in the noise. By the time someone notices, the charges have already started accruing.


## How Fast the Costs Add Up


The math gets uncomfortable quickly when you’re running more than a handful of affected services.


Consider a mid-size organization running 30 EKS clusters on an older Kubernetes version and 20 RDS instances on PostgreSQL 11 or MySQL 5.7, each averaging 8 vCPUs. The EKS extended support alone adds roughly $130,000 per year. The RDS charges add another $140,000 or more, depending on Multi-AZ configurations and read replicas. That’s over $270,000 annually in fees that deliver zero new functionality — just the privilege of staying on an older version.


And that doesn’t account for Cloud SQL instances on GCP, ElastiCache Redis clusters running deprecated versions — which carry an 80% surcharge in years one and two and a 160% surcharge in year three — or Azure databases approaching their own extended support deadlines. For larger organizations running hundreds of clusters and database instances across multiple clouds, extended support fees can quietly climb into the millions.


AWS recognized the scale of this problem enough to publish a dedicated cost estimator tool for RDS extended support charges and build extended support cost projections into their Cloud Intelligence Dashboards.


## How Infracost Helps You Get Ahead of It


This is where Infracost comes in. Rather than waiting for a surprise on your monthly bill or hoping someone catches a vendor email, Infracost shifts this problem left into the engineering workflow where it can actually be fixed.


Infracost’s FinOps policy engine scans your infrastructure-as-code configurations and flags resources running on versions that are approaching or have already entered extended support. When an engineer opens a pull request that deploys an EKS cluster on an older Kubernetes version or provisions an RDS instance on a soon-to-be-deprecated engine, Infracost surfaces the cost impact directly in the code review. No surprise bills. No after-the-fact audits.


This is the difference between reactive cost management — scrambling to explain a spike in your cloud bill — and proactive governance that prevents the cost from being incurred in the first place. Infracost gives engineering teams the visibility to make informed decisions at the moment those decisions are being made: in the pull request, before anything gets deployed.


For organizations managing infrastructure across AWS, Azure, and Google Cloud, Infracost provides a single operational workflow to catch extended support risks across all three providers before they turn into budget line items.


## Stop Paying Extra to Stand Still


Extended support fees are one of the most avoidable costs in cloud infrastructure. Every dollar spent on them is a dollar that delivers no new capability, no better performance, and no competitive advantage. It’s just the cost of not upgrading.


[Schedule a demo with Infracost](https://www.infracost.io/finops/#LiveDemo) to see how your team can identify extended support charges hiding in your infrastructure, catch them before they ship, and build upgrade paths into your normal engineering workflow — before the next bill arrives.
