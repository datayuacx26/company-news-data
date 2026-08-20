---
schema_version: "1.0.0"
document_id: "b68ea924c6f38ae669f5458b853621ee06a00f9c8ea94a5bf64e28e1d0de7531"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/announcing-the-polytomic-metadata-source"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:23947bc4e1e85c4c84b85a9cf95b8ec2f45d1fbb2073d10bf36d6ae9f1ea3cfa"
---

# Announcing the Polytomic Metadata source

Today we're releasing our new Polytomic Metadata source. This is a source connection that lets you sync all the data in your Polytomic environments to your data warehouses, databases, and cloud storage buckets like S3.


The tables cover data like:


- Sync configurations (Model and Bulk Syncs, selected fields, sync schedules, etc)
- Sync runs and their statistics (run times, tables involved, number of rows synced, etc)
- Model definitions


This also means that, for[Polytomic Connect](https://www.polytomic.com/connect) customers, you're now able to sync all historical and current data about your customers' syncs to single tables you can query and build dashboards on top of. Just instantiate a[Polytomic Metadata](https://apidocs.polytomic.com/guides/configuring-your-connections/connections/polytomic-metadata) connection and you're off to the races.


See our documentation for details and table descriptions:[https://docs.polytomic.com/docs/polytomic-metadata](https://apidocs.polytomic.com/guides/configuring-your-connections/connections/polytomic-metadata) .


Enjoy! And, as always, do email us with any questions: support@polytomic.com.


[Back to blog](https://www.polytomic.com/blog)
