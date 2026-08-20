---
schema_version: "1.0.0"
document_id: "74ed52a2b64876aea46e92dac61ad7de3016ae0f421031d7840760355d73112b"
company_key: "blend-labs-inc-class-a-common-stock"
company: "Blend Labs Inc."
source_id: "blend-labs-inc-class-a-common-stock-rss-4631133ca4a9"
canonical_url: "https://full-stack.blend.com/encrypting-streams-in-go.html"
published_at: "2019-05-07T08:00:00+00:00"
first_seen_at: "2026-07-20T23:18:43.300114+00:00"
fetched_at: "2026-08-20T00:34:47.711311+00:00"
content_hash: "sha256:1ced18d9cd48ae0ecf3d8dab789c16ee7d3959b652d32169cdec39fb56c6a5b3"
---

# Encrypting Streams in Go

At Blend, we deal with highly sensitive consumer financial data. We use several data stores — Postgres, MongoDB, CockroachDB, and Etcd — all of which need to be backed up. While MongoDB and Postgres give us prebuilt tools for encrypting backups, Etcd and CockroachDB do not. Our standard practice is to encrypt these backups before storing them. This became more challenging as our backups grew. Encrypting backups in memory At the beginning the backups were small, and we were able to use Vault’s transit features to encrypt them.
