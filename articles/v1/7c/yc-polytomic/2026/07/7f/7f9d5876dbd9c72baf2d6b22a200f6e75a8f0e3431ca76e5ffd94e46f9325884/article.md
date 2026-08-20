---
schema_version: "1.0.0"
document_id: "7f9d5876dbd9c72baf2d6b22a200f6e75a8f0e3431ca76e5ffd94e46f9325884"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/announcing-warehouse-table-filters"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:e10ff05915264226eba667decd65c054f3d0580206db60c5a6de78ea75c014eb"
---

# Announcing warehouse table filters

Today we are releasing table filters for warehouse syncs.


When syncing to your data warehouse, there are situations where you might only want a subset of rows from your source system. For example:


- A small set of event types from a multi-billion-row event stream.


- The last two years of history from a 12-year-old Salesforce instance.


- Tickets with certain attributes created within the last three years from a ten-year-old Zendesk instance.


Now, when syncing to your data warehouse using our Bulk Syncs feature, you'll be able to specify any number of filter conditions on any number of source objects and any number of source fields, all in the same sync. This works for all[sources](https://www.polytomic.com/integrations) : whether CDC SQL database streams, SaaS apps, spreadsheets, or cloud storage like S3.


Filters can be applied to the common data types:


- Strings (equals, does not equal, matches-one-of, does-not-match, does not equal, ends with, starts with, etc).


- Numbers (greater than, less than, equals, etc).


- Dates (before, after, etc).


- Booleans (is true, is false, etc).


Once set, Polytomic will only sync data to your warehouse that matches your filter conditions, thus cutting unnecessary storage and transfer time.


Documentation on how to to set table filters is located here:[https://docs.polytomic.com/docs/bulk-sync-table-filters](https://docs.polytomic.com/docs/bulk-sync-table-filters) . As always, email us atsupport@polytomic.com with any questions!


[Back to blog](https://www.polytomic.com/blog)
