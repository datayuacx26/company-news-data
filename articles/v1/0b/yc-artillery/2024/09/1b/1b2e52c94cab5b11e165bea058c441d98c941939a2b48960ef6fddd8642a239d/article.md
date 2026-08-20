---
schema_version: "1.0.0"
document_id: "1b2e52c94cab5b11e165bea058c441d98c941939a2b48960ef6fddd8642a239d"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/new-filters"
published_at: "2024-09-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:e89f12df33578f3f490c5e80b21968e6b633c58eadca8bc47397675b7dfe5e9b"
---

# Load tests filter improvements

September 2nd, 2024[Dashboard](https://www.artillery.io/changelog/tag/dashboard)


# Load tests filter improvements


Edmundo Santos


Introducing a new way to filter and organize your tests. We have updated the UI to better surface all filters and added even more options to filter by, so it should be easy to find specific test runs.


- New filters: test name, platform, user, tests with notes, and shared tests
- UI is now fully keyboard accessible
- Improve search functionality for test names and tags
- Filters can be saved as **Views** for easy access and faster team collaboration


## Comparison view improvements


- Added new filters to Comparison view so you can quickly find tests to compare
- Compare tests that have different names
- Toggle what properties are displayed on the test list with the Display dropdown
- Use **Views** to quickly get back to previous saved filters


## Test config


You can now see the` target` and` phases` of your test right from the load test view — added in Artillery CLI v2.0.20.
