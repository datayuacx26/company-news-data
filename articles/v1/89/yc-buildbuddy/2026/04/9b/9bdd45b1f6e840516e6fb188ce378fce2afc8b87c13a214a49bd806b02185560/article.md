---
schema_version: "1.0.0"
document_id: "9bdd45b1f6e840516e6fb188ce378fce2afc8b87c13a214a49bd806b02185560"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/bazel-remote-cache-cdc"
published_at: "2026-04-23T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:0368a94b5c76e12a303bd75b45aeaf391514c42f239f62863aab5d5d9162a3e4"
---

# End-to-end CDC support

We're excited to announce end-to-end content-defined chunking (CDC) support for Bazel remote caching in BuildBuddy.


With Bazel 9.1 and 8.7 support for` --experimental_remote_cache_chunking` , large outputs like linker artifacts can be uploaded and downloaded in content-defined chunks instead of as monolithic blobs. That lets BuildBuddy deduplicate similar artifacts across builds, reducing upload bandwidth and storage usage. In one benchmark on the BuildBuddy repo, this showed roughly 40% less uploaded data and a roughly 40% smaller disk cache. Breaking large blobs into smaller reusable pieces also means fewer long-running RPCs and more granular retries.


To enable it, add this flag to your` .bazelrc` :


```text
common --experimental_remote_cache_chunking
```


To see the download-side savings, you should also set` --disk_cache` , since the downloaded chunks need to be stored somewhere in order to be reused locally. We also recommend setting` --experimental_disk_cache_gc_max_age` to a value below your remote cache TTL—for example,` 3h` , or` 1d` if your remote TTL is longer.


Bazel 9.1 and 8.7 support this flag.


For more background, see[bazelbuild/bazel#28437](https://github.com/bazelbuild/bazel/pull/28437) .


For a deeper dive, see[No More Large Outputs: Introducing Remote Cache CDC](https://www.buildbuddy.io/blog/content-defined-chunking) .
