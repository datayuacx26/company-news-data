---
schema_version: "1.0.0"
document_id: "52d83a7acfda92227297318f3f9433613aa5c5199a6ae71862b12c19cb6ae01d"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/windows-support-in-beta/"
published_at: "2025-04-28T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:3fed5ff0f22eaaf1b4025c277480800bc922384126a03fac770636c35f2ec044"
---

# Windows support in beta

Previously, SST v3 was only supported on Linux/macOS and Windows with WSL. With[v3.14](https://github.com/sst/sst/releases/tag/v3.14.0) , native Windows support is now in beta.


The main difference between Windows and Linux/macOS is that[sst dev](https://sst.dev/docs/reference/cli#dev) does not support the tabbed terminal UI. So while, it does spawn multiple processes, it does show their output in separate tabs.


If you are on Windows, we recommend upgrading to v3.14 and giving it a try. If you run into any problems, feel free to[open a GitHub issue](https://github.com/sst/sst/issues/new) .
