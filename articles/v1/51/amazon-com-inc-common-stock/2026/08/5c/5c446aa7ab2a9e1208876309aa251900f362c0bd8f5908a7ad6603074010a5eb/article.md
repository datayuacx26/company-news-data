---
schema_version: "1.0.0"
document_id: "5c446aa7ab2a9e1208876309aa251900f362c0bd8f5908a7ad6603074010a5eb"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-healthomics-wdl-task-level-timeout/"
published_at: "2026-08-03T20:00:00+00:00"
first_seen_at: "2026-08-03T21:24:28.279899+00:00"
fetched_at: "2026-08-17T14:48:22.550886+00:00"
content_hash: "sha256:5348f9252f0a3935fc112d87d8da38e3bb544a0b6d82d9bae20b650698c93405"
---

# AWS HealthOmics now supports task-level timeout for WDL workflows

AWS HealthOmics now supports task-level timeout for Workflow Description Language (WDL) workflows, enabling you to set maximum execution duration for individual tasks.[AWS HealthOmics](https://aws.amazon.com/healthomics/) is a HIPAA-eligible service that helps healthcare and life sciences customers accelerate scientific breakthroughs at scale with fully managed bioinformatics workflows.


With task-level timeout, you can define time bounds on individual WDL tasks to control costs and enable automated error recovery. HealthOmics provides the omicsTimeout runtime attribute that you can add to any task's runtime section to specify the maximum duration a task is allowed to run. When a task exceeds the specified duration, HealthOmics stops the task and sets the task and run statuses to failed. The omicsTimeout attribute accepts duration values with standard time units (such as 90s, 2h, 1d). This prevents tasks from consuming resources and helps you set cost guardrails during workflow development.


Task-level timeout for WDL workflows is available in all supported AWS HealthOmics Regions: US East (N. Virginia, Ohio), US West (Oregon), Europe (Frankfurt, Ireland, London), Israel (Tel Aviv), and Asia Pacific (Seoul, Singapore, Tokyo). To learn more, visit the[WDL workflow definition specifics](https://docs.aws.amazon.com/omics/latest/dev/workflow-languages-wdl.html) documentation.
