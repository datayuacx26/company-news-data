---
schema_version: "1.0.0"
document_id: "778feb66e3c4034db3fa546d3eeb271d8540c9c0ed44b6b64bf5780ac0cb622e"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/introducing-per-table-warehouse-scheduling"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:d6b001c4fab072bbaf60bd2f0fce7b6825c848da2ef8e0fe0cb6959411168a5c"
---

# Introducing Per-Table Warehouse Scheduling

When syncing to your data warehouse (which you can do with our[Bulk Syncs](https://docs.polytomic.com/docs/bulk-syncs?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--6s6oA5nqZ04rS7PxmF7W6blklNr_vum_3TPv1DQgtT8S5HLkdHloevCB2yFbbSwif3G7F) feature), you can now specify multiple schedules for the same sync.


These extra schedules are maximally flexible. For example, when syncing to your data warehouse from any source (e.g. your production database, Salesforce, etc), you can now do the following:


- Sync all tables on a daily schedule.
- But also specify that two particular tables run every five minutes...
- ...and run a full resync-and-rebuild every Sunday at midnight.


You can add extra schedules to your Bulk Sync by simply clicking a button in the new Advanced section under your default schedule. See documentation here:[https://docs.polytomic.com/docs/bulk-sync-multiple-schedules](https://docs.polytomic.com/docs/bulk-sync-multiple-schedules?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--6s6oA5nqZ04rS7PxmF7W6blklNr_vum_3TPv1DQgtT8S5HLkdHloevCB2yFbbSwif3G7F) .


### Polytomic Connect


For those using[Polytomic Connect](https://www.polytomic.com/connect?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--6s6oA5nqZ04rS7PxmF7W6blklNr_vum_3TPv1DQgtT8S5HLkdHloevCB2yFbbSwif3G7F) , you need to upgrade to our` 2025-09-18` API to access this. This is a breaking upgrade that has two new arguments on the Bulk Sync configuration:` default_schedule` and` additional_schedules` (docs[here](https://apidocs.polytomic.com/2025-09-18/api-reference/bulk-sync/create?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--6s6oA5nqZ04rS7PxmF7W6blklNr_vum_3TPv1DQgtT8S5HLkdHloevCB2yFbbSwif3G7F#request.body.default_schedule) ).


As always, questions and comments are welcome! Email support@polytomic.com anytime.


[Back to blog](https://www.polytomic.com/blog)
