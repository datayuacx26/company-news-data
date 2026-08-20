---
schema_version: "1.0.0"
document_id: "c59335cc0e34137567c780a3edaec05dd8e2ec55612c26364acfc178a1f61c4b"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/announcing-table-and-column-output-name-overrides-for-data-warehouses"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:df20156e16e3f0762d558c26e673592f23513595044feef0a338e09e135e2d31"
---

# Announcing table and column output name overrides for data warehouses

Polytomic supports ELT, ETL, and CDC streaming workloads to data warehouses. Yet source databases and applications have all sorts of naming conventions, so which one should the resulting tables and columns in your warehouse have?


When it comes to naming, consistency is key. Arguments about` snake_case` versus` camelCase` are dwarfed by the maddening special-casing in queries to handle a mix of` updated_at` ,` updatedAt` , and` updatedat` .


To address this, Polytomic automatically normalizes all names for you using a[documented](https://docs.polytomic.com/docs/bulk-sync-naming-conventions#default-name-normalization) convention.


But exceptions in preferences abound, so today we released options that give you full control of the resulting table and column names, no matter your preferences:


1. By default, Polytomic will automatically normalize names for you per the above convention.


2. However, you now get an option to turn this off per-sync if you don't want any normalization:


3. For even more fine-grained control, you also have the ability to specify your own custom output names for specific tables and columns of your choice:


With these changes, you now have complete freedom to decide how Polytomic names the tables and columns it writes to your data warehouse. You can see more in our documentation here:[https://docs.polytomic.com/docs/bulk-sync-naming-conventions](https://docs.polytomic.com/docs/bulk-sync-naming-conventions) .


‍


[Back to blog](https://www.polytomic.com/blog)
