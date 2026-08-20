---
schema_version: "1.0.0"
document_id: "27e471c598633a17510c1242bfec920139c1fb27a761fa4d3bbb4a9efc0d3da7"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/build-summary-details"
published_at: "2023-06-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:4b5dc731b32cf1ec118c92ebbb2a561426969473b6b84d3346e403c221a3b4c3"
---

# New build summary link and a refreshed UI

A refreshed build details UI has launched! We've also added a build summary link in the output of every build that links to the UI, to make it easier to associate the build output with the build details.


## Build summary link


The Depot CLI now prints a` Build Summary` link after every build, with a direct link to the build details page:


## UI refresh for Builds


We've also added some additional details to the build summary page to make it easier to understand at a glance what happened during the builds:


1.


Any tags that were applied to the build with` -t` or` --tag` :


2.


The percentage of the build that was cached, represented by a pie chart:


3.


If the build was pushed to a remote registry with` --push` :


4.


If the build was loaded back from the remote builder to the local machine with` --load` :


## Availability


The new summary link is available in versions 2.16.0 and above of the[Depot CLI](https://github.com/depot/cli) , and the UI refresh is now available for all Depot organizations.


Jacob Gillespie


CTO & Co-founder of Depot
