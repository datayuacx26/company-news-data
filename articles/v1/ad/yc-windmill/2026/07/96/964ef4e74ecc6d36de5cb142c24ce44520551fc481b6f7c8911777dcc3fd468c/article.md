---
schema_version: "1.0.0"
document_id: "964ef4e74ecc6d36de5cb142c24ce44520551fc481b6f7c8911777dcc3fd468c"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ce-workspace-storage-quota"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:8f06e1ddebd1b0a49c2c9c8dcd8f84173d972474f3cf82a06d2d4962fef3a69b"
---

# 10GiB workspace storage quota replaces 50MB upload cap on Community Edition

### [10GiB workspace storage quota replaces 50MB upload cap on Community Edition](https://www.windmill.dev/changelog/ce-workspace-storage-quota)


S3


Persistent storage


Workspace


[v1.746.0](https://github.com/windmill-labs/windmill/releases/tag/v1.746.0)


[Docs](https://www.windmill.dev/docs/core_concepts/object_storage_in_windmill#workspace-object-storage)


The Community Edition 50MB per-file upload cap on workspace object storage is replaced by a 10GiB total storage quota per workspace. Files of any individual size can be uploaded up to the quota, usage is shown in the workspace settings under S3 Storage with a recount button, and reads and deletes are never blocked even when over quota. Enterprise Edition remains unlimited.


#### New features


- Upload files of any individual size to workspace object storage on Community Edition, up to 10GiB of total workspace storage.
- Storage usage meter in workspace settings under S3 Storage, with a button to recount usage by listing the storage.
- Writes exceeding the quota are rejected, but reads and deletes are never blocked, so an over-quota workspace can always free up space.
- The quota counts all objects under the configured storages except the reserved volumes/ prefix; Enterprise Edition remains unlimited.
