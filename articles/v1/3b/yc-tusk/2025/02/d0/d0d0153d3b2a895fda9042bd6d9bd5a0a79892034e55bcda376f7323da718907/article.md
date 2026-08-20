---
schema_version: "1.0.0"
document_id: "d0d0153d3b2a895fda9042bd6d9bd5a0a79892034e55bcda376f7323da718907"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/february-2025-changelog"
published_at: "2025-02-28T09:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:d4864eb4efdc0076264eab0fcfe5c73568c4b4e185b4c88d8351bf66f9b23550"
---

# February '25 Changelog

# What's new in Tusk


One popular request from our customers was the ability to generate tests from within the IDE.


This month, we released a[new backfill feature](https://docs.usetusk.ai/automated-tests/test-backfill) that allows developers to generate unit and API tests by simply leaving code comments as they write code in their IDE.


Devs can mention the keyword **UseTusk** in a code comment to mark a function for test generation. Tusk will parse the comment and generate tests for the function once the PR/MR is raised.


```text
function    calculateTotal  (  items  )    {
// [UseTusk] generate unit tests
// Optional: additional context to help guide test generation
let   sum  =    0
for    (  const   item  of   items )    {
sum  +=   item .  price
}
return   sum
}


```


In other news, we've improved the clustering of PR/MR's changes for better context management and increased accuracy of test file placement. Other optimizations were made to Tusk's ability to generate full test files (combining multiple test cases) and de-duplicate tests.


## Release notes


- New backfill feature for unit and API tests
- Enhanced clustering of PR/MR's changes
- Optimizations to generating full test files
- Optimizations to test de-duplication step
- Improved consistency of test patterns
- Support for larger PR/MRs
- Tusk UI: Improved set-up flow for repos and test environments
- Tusk UI: New billing and seats management page
- Tusk UI: Ability to view agent iteration and excluded tests


---


TRY TUSK NOW


## AI test generation with codebase context for quality-obsessed teams.


Cover your blind spots, catch verified critical bugs, merge PRs 25% faster with peace of mind.
