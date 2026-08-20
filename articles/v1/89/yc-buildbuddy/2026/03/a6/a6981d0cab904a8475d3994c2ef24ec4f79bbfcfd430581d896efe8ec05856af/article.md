---
schema_version: "1.0.0"
document_id: "a6981d0cab904a8475d3994c2ef24ec4f79bbfcfd430581d896efe8ec05856af"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/compare-invocations"
published_at: "2026-03-09T09:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:c04853aec1924fc01359a56dc7a719cea2f62ba6b37022ee7cd7c0757c6dc041"
---

# Compare invocations: compare two builds side-by-side

[← Back to changelog](https://www.buildbuddy.io/changelog)


# Compare invocations: compare two builds side-by-side


March 9, 2026 ·


Siggi Simonarson


The compare invocations view allows you to diff two invocations side-by-side.


It can help debug why two builds produced different results by showing exactly what changed: different flags, startup options, cache/remote execution settings, status, and more.


To compare invocations:


1. From one invocation link, select **Compare -> Select for comparison** in the top right corner.
2. From another invocation link, select **Compare -> Compare with selected** .
3. For even more details on differences between the two invocations, click the **Run bb explain** button on the compare invocations page.


bazel


debugging


featured
