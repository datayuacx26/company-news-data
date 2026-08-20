---
schema_version: "1.0.0"
document_id: "bb0b29327ee67b4f922d755de626dd066afa48c076293310b3830579671e15d4"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/multi-project-dbt-lineage-see-dependencies-across-your-entire-data-stack"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:0a8f73b67b46c675834f459b39400f4f5beb220b34c9d7d06eab8cafd3db38e6"
---

# Multi-Project dbt Lineage: See Dependencies Across Your Entire Data Stack

If you're running data at scale, you're probably working with multiple dbt projects. Splitting by domain or ownership reduces coordination overhead and makes ownership clearer. Most teams doing serious dbt work end up here eventually.


But this creates a visibility problem for anyone responsible for reliability.


## The handoff problem


When something breaks in production, you need answers fast. Where did this problem start? What else is affected? Who needs to know?


These questions are hard to answer when lineage stops at project boundaries. A model in one project references a table built by another project as a source. From a workflow perspective, that's fine. From a debugging perspective, it's a gap.


You're missing the dependency chain. You don't know the full downstream impact. You can't route the issue to the right team without digging through code or pinging people in Slack.


When teams own different parts of the transformation layer, incomplete lineage makes incidents slower to triage and harder to resolve. You're assembling context manually instead of seeing it.


## Why this matters for reliability


Elementary's goal is to detect, surface, and triage data issues. Lineage is core to that.


When a test fails or data looks wrong, lineage tells you the root cause: where the problem actually originated, even if it surfaced somewhere else. It also tells you the blast radius: every downstream table, dashboard, or model that's now affected.


And it tells you who should handle it. If an issue started in the Finance team's models but broke something in Marketing's reports, you need to know that immediately. Not after three Slack threads.


For this to work, lineage can't stop at the edges of individual dbt projects. Real dependencies cross those boundaries constantly. If your lineage graph doesn't reflect that, you're working with incomplete information when it matters most.


## **Common patterns that need cross-project lineage**


Two architectural patterns show up repeatedly when teams scale dbt.


### **Bronze-Silver-Gold by stage**


Some teams organize projects by transformation stage. A data engineering team owns the bronze layer, which handles raw ingestion and light cleaning. An analytics engineering team owns silver, building conformed models and business logic. Domain teams or BI developers own gold, creating specific data products.Each layer is a separate dbt project with different owners, different deployment schedules, and different responsibilities. Silver references bronze as sources. Gold references silver as sources. When a bronze table changes shape or a silver model breaks, the impact ripples through projects that you can't see without unified lineage.


### **Domain-oriented mesh**


Other teams split by business domain. Finance, Marketing, Product, and Sales each run their own dbt projects. They build models specific to their needs using their own resources and shared datasets from a central platform team.Cross-domain dependencies are common. Marketing needs revenue data from Finance. Product analytics needs campaign attribution from Marketing. When these references cross project boundaries, you need lineage that follows them. Otherwise, you're blind to how domains depend on each other and who's affected when something changes.Both patterns make organizational sense. Both create the same lineage problem.


## Lineage that follows the data


Elementary now builds lineage across all dbt projects in the same environment.


When you connect multiple projects, Elementary treats them as a unified graph. If a table is produced in one project and consumed as a source in another, that dependency is surfaced automatically. You can trace flows end-to-end without switching contexts or reconstructing relationships by hand.


This doesn't change how you organize your work. It just makes the connections between projects visible in the places where visibility matters: when you're debugging an incident, assessing impact, or trying to understand what broke and why.


## Beyond dbt


Most real data stacks include more than what dbt manages. There are tables built by reverse ETL tools, views created by BI teams, staging tables written directly in SQL, Python data science pipelines and other datasets maintained outside the transformation layer entirely.


Elementary lets you add any table or view from your warehouse and see it in the lineage graph alongside your dbt projects, along with its test results and data quality scores. This gives you a more complete picture of how data actually flows, not just how it flows through the parts of your stack that dbt controls.


We're continuing to expand lineage coverage to reflect the systems teams actually use. The goal is full visibility into your data stack, however it's structured.


## How it works


Setup is straightforward. When you create an Elementary environment, you add the dbt projects that belong to it. You can add or remove projects later directly in the UI. No redeployment, no special configuration.


Once Elementary understands how your projects connect, it also knows where code changes should go. If you connect the repos behind your dbt projects, Elementary will open pull requests in the right place when generating tests or updating metadata. Changes land with the teams that own the models, without manual routing.


## Part of a longer path


This is one piece of a broader direction we're moving toward with Elementary 2.0: lineage that covers the full data stack, not just dbt.


But dbt is still the center of most transformation workflows, and understanding how data moves through it - across projects, across teams, across domains - is foundational. Multi-project lineage makes that understanding complete.


If you're running dbt at scale and tired of stitching context together manually, this is worth trying.
