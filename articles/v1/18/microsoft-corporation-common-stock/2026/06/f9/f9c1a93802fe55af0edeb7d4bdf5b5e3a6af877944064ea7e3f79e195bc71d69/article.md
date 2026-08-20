---
schema_version: "1.0.0"
document_id: "f9c1a93802fe55af0edeb7d4bdf5b5e3a6af877944064ea7e3f79e195bc71d69"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0d567709f64e"
canonical_url: "https://www.microsoft.com/en-us/power-platform/blog/2026/06/10/bulk-deletion-in-dataverse/"
published_at: "2026-06-10T15:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.280378+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:75ed3977a16a9701a392585019f525d2f02568953cd077b9c57d3136b83f8f5d"
---

# Bulk Deletion in Microsoft Dataverse: New Capabilities for Data Lifecycle Management

Every Dataverse environment generates data that outlives its usefulness, workflow logs, audit trails, system jobs, plug-in traces, test records, stale transactional data. Left unmanaged, this data accumulates, consumes storage, and eventually forces administrators into reactive, large-scale cleanups.


**Bulk Deletion** is the native Dataverse capability built to prevent exactly that. In this post, we’ll cover what Bulk Deletion is, how to use it as part of a data lifecycle management, and the improvements that are now general available beginning June 2026.


## **What is Bulk Deletion?**


Bulk Deletion is a native Dataverse capability that lets administrators define and run jobs to remove large volumes of records based on a query. Instead of writing custom scripts or one-off automations, admins configure a query, for example, *“all completed system jobs older than 90 days” and* let the platform execute the deletion in the background.


A bulk deletion job can be:


- **Run once on demand** for ad-hoc cleanup.


- **Scheduled to recur** on a daily, weekly, monthly, half-yearly, or yearly cadence.


- **Configured with notifications** so administrators get email alerts when a job completes.


- **Targeted at any table** including system tables and custom tables.


Under the hood, Bulk Deletion respects security, cascading rules, plug-ins, and workflows. It behaves like a regular delete, just at scale and on a schedule.


## **When should Bulk Deletion be used?**


Use Bulk Deletion any time you need to remove a meaningful volume of records based on a repeatable, query-based rule. Common scenarios:


- **Staying storage compliant.** Keep your environment within Dataverse storage entitlements by routinely removing data that no longer needs to be retained, before it pushes you into overage.


- **Routine system hygiene.** Purge data from system tables, completed system jobs, workflow logs, plug-in traces, audit records, once they pass their retention window.


- **Post-migration cleanup.** Remove staging records, or test data after a migration has been validated.


- **Sandbox refresh follow-up.** After copying production into a sandbox, remove PII, large transactional tables, or data not relevant to dev/test.


- **End-of-lifecycle data.** Clear out closed cases, expired leads, or transactional records past their business retention period.


- **Enforcing custom rules.** Implement organization-specific rules like *“delete all inactive accounts older than 60 days.”*


If the rule for what to delete can be expressed as a query, Bulk Deletion is almost always the right answer.


## **How Bulk Deletion should be used — setup deletion jobs on day one**


The single most important guideline: **define data deletion jobs** the day an environment is provisioned for any table likely to accumulate data that will eventually no longer be needed.


A data deletion job is a documented rule, per table, for what to delete, when to delete it, and how often the rule runs. It is also called a bulk delete job. Without one, environments tend to follow a predictable pattern:


- Transactional and log tables grow unchecked.


- Audit and workflow data is never purged.


- Custom tables built for transient processing become permanent stores.


- Storage usage climbs.


- Cleanup eventually stops being routine and becomes a project.


Treat data deletion as a Day-1 design decision, alongside security roles, solution architecture, and integration design.


## **Setting a data deletion job**


For every table, system or custom, one should answer these three questions:


1. **Does this table accumulate transactional or log data?**


1. **How long does the business need to retain this data?**


1. **Is there a recurring bulk deletion job in place to enforce that?**


If the answer to (3) is *“no”* for any table that grows, you are accumulating storage and operational debt. Schedule a recurring bulk deletion job up front. Even a simple weekly job that removes records older than your retention window will hold the table at a steady state.


Think of a data deletion job the way you’d think of garbage collection in a running application, a routine, automated process that keeps the system healthy, not an afterthought once memory runs out.


## **What administrators have been telling us**


As Dataverse adoption has scaled, three themes have come up consistently:


- *“My job stopped, and it wasn’t clear why.”* Jobs could stop or hit issues mid-run, but the reason wasn’t always visible. Admins often re-ran jobs to move forward, which added guesswork.
- *“I had to recreate the same job in every environment.”* As solutions moved from dev to test to production, bulk deletion configurations had to be set up manually in each environment. Small differences, a filter, a schedule, required careful revalidation.
- *“Large cleanups take time.”* After full environment copies, especially into sandboxes, admins needed to remove large volumes of non-essential data before follow-up work could begin.


These themes shaped the updates now reaching general availability.


## **What’s new**


### **1. Error handling and run visibility**


Every bulk deletion job now includes a[Run details](https://learn.microsoft.com/en-us/power-platform/admin/delete-bulk-records#error-handling) tab. Open a job and you’ll see a summary at the top — start time, end time, status, records deleted, records failed, and errors encountered. Specific errors are listed inline:


- **Completed** — the job ran to completion but may have hit errors along the way.


- **Failed** — the job never started; reasons are visible when you open it.


Diagnose, fix the root cause, and move on without guessing.


### **2. Solution-aware bulk deletion jobs**


Bulk deletion jobs are now[solution-aware](https://learn.microsoft.com/en-us/power-platform/admin/delete-bulk-records#solution-aware-bulk-deletion-jobs) . Build and validate cleanup logic in development or sandbox, then move the same configuration to pre-production and production using standard solution export and import. The full job definition, filters, schedule, and name, travels with the solution.


What this means in practice:


- Configure once, promote everywhere.


- No need to recreate jobs environment by environment.


- Bulk deletion configurations follow the same lifecycle as the rest of your solution components.


**Step 1** – Go to maker portal, create a new solution and edit it to add an existing bulk delete job.


**Step 2** – Go to Add existing> More > Other > Data Life Cycle Config to add an existing bulk delete job.


**Step 3** – Select the bulk deletion job to add to the solution.


**Step 4** – With the bulk delete job in a solution, export the solution as you would for any other component.


### **3. Permanent deletion checkbox in the Bulk Deletion Wizard**


[Deleted records keeping](https://www.microsoft.com/en-us/power-platform/blog/2026/03/25/restore-deleted-records/?msockid=0dc7bfa7e41e65351937a893e5f6643a) is one of the most valuable safeguards for your business-critical data. As it moves from public preview to general availability, bulk deletion jobs in environments where deleted records keeping is enabled will copy records to the deleted records tables before removing them, giving you a recovery window if something is deleted in error. For data that matters to your business, that safety net is well worth the small amount of additional storage it uses.


That said, not every record needs to be recoverable once it reaches the end of its data lifecycle. Old system logs, expired workflow records, and transient telemetry are unlikely to ever be restored, yet keeping copies of them still consumes storage and adds processing overhead to every deletion job.


For exactly these situations, the new[Permanent deletion](https://learn.microsoft.com/en-us/power-platform/admin/delete-bulk-records) checkbox in the Bulk Deletion Wizard lets you opt out of deleted records keeping for a specific job. When selected, it not only reduces the storage consumed by stale records, but also eliminates certain processing steps, which speeds up the deletion job itself.


The checkbox is available only for one-shot, non-recurring jobs, by design. Limiting it this way ensures admins make a conscious choice every time and avoids a scenario where a recurring job configured long ago keeps permanently deleting data without anyone realizing.


When[Permanent deletion](https://learn.microsoft.com/en-us/power-platform/admin/delete-bulk-records) is selected:


- Deleted records cannot be recovered.
- No additional storage is consumed by deleted records.
- The bulk delete job runs faster.


Use it for non-recurring cleanup of data with a known expiration, the kind of data you would never need to restore anyway.


***Caution:** permanent deletion is exactly that. There is no undo. Verify the data targeted by your job is truly disposable before enabling this option.*


### **4. Engine refinements and a new sandbox deletion mode**


We’ve made foundational updates to the Bulk Deletion framework, smarter record fetching, more efficient progress tracking, and refined thread management. These changes apply automatically; no configuration is required.


For sandbox environments, particularly after a full production copy, we’ve introduced[sandbox deletion mode](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/delete-data-bulk#control-bulk-delete-processing) . Enabled through the **RunJobForSandbox** option in the BulkDelete API, it:


- Skips plug-ins, workflows, and deleted records keeping.


- Uses the cascade engine directly.


- Still respects cascade rules and referential integrity.


This provides a leaner execution path for large-scale sandbox cleanup where business logic and recoverability are not required.


**Caution:** *Sandbox deletion mode is specifically designed for Sandbox. This deletion mode permanently deletes records with no recovery path, and plug-ins and workflows won’t fire. Use it only when the data is no longer needed and no business logic depends on delete-time events.*


## Bulk Deletion keeps a Dataverse environment healthy


Bulk Deletion is the built-in way to keep a Dataverse environment healthy at scale, but it is only as effective as the data deletion jobs behind it. Schedule these recurring jobs from the day each table is provisioned and avoid letting transactional and log data accumulate.


With the updates landing beginning June 2026, clearer run visibility, solution-aware portability, an opt-in permanent deletion path, and refinements to the underlying execution model — Bulk Deletion is more transparent to operate and easier to promote across environments.


If you haven’t reviewed your data deletion jobs and data retention strategy in a while, now is a good time.


**Learn more**


- [Delete bulk records — Power Platform | Microsoft Learn](https://learn.microsoft.com/power-platform/admin/delete-bulk-records)


- [Delete Data in Bulk to Reduce Storage Use — Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/delete-data-bulk#control-bulk-delete-processing-preview)


- [Restore deleted Microsoft Dataverse table records — Power Platform | Microsoft Learn](https://learn.microsoft.com/power-platform/admin/restore-deleted-table-records)
