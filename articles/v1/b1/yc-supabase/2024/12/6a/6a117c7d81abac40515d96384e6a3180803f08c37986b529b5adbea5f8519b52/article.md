---
schema_version: "1.0.0"
document_id: "6a117c7d81abac40515d96384e6a3180803f08c37986b529b5adbea5f8519b52"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/restore-to-a-new-project"
published_at: "2024-12-06T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:3c8cbc7883ca9cd6dd976c1a801f8200a776adbe05b879ba5dd94fe38a6dd73d"
---

# Restore to a New Project

Today we’re adding[Restore to a New Project](https://supabase.com/docs/guides/platform/clone-project) .


You can use this new tool to copy data easily from an existing Supabase project to a new one. **Restore to a New Project** integrates seamlessly with daily physical backups and Point-in-Time Recovery (PITR) to provide flexible restoration options.


Restore to a New Project requires physical backups to be enabled. It is available to all customers on a paid plan.


## How it Works#


When physical backups are enabled, Supabase triggers daily backups of project data. You can use this backup to restore to a new Supabase project. The new project should match the original project attributes:


- Size of compute instance.
- Disk settings; volume type, size, IOPS, and throughput.
- SSL enforcement configuration.
- Network restrictions.
- Cloud region.


After launching your restored project, the rest of the process is automated. The length of time for a new project to provision will depend on the size of the source dataset.


The new project will be available in the Supabase Dashboard as soon as the copy process has completed. This project will behave as any other Supabase project and is completely independent of the source.


### Point-in-Time Recovery#


In addition to daily backups it is possible to restore from a project with PITR enabled. This allows for very fine granularity when selecting the desired point to restore from. The process is very similar as with daily backup with the exception of being asked to select a specific time.


## Unlimited Cloning from a Source Project#


To ensure maximum flexibility a source project can be *copied* as many times as required, making the tool perfect for testing, development environments etc. However, please note that cloning from an already cloned project is not currently supported (this is in the works).


New projects created with this process, as with any new Supabase project, will incur the usual compute and disk costs. These costs are displayed ahead of time to ensure there are no surprises.


## Accessing Restore to a New Project#


The Restore to a New Project feature can be found on the Supabase dashboard under[database backups](https://supabase.com/dashboard/project/_/database/backups/restore-to-new-project) .


Please be aware that Restore to a New Project is currently in Public Alpha. You can reach out to[Supabase support](https://supabase.help/) if you experience any issues.
