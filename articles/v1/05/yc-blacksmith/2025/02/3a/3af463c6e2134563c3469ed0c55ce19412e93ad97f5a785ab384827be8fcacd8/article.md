---
schema_version: "1.0.0"
document_id: "3af463c6e2134563c3469ed0c55ce19412e93ad97f5a785ab384827be8fcacd8"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/launch-github-actions-analytics"
published_at: "2025-02-25T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:00eab2e86690b214dac0f872465bd106e96caf51a08f870667d09fa9445f4cd2"
---

# Launch: GitHub Actions Analytics

Most engineering teams today are stuck piecing together CI metrics from a bunch of different places: GitHub's UI, expensive monitoring tools, cloud dashboards, and scattered logs. It's a pain just to answer basic questions about your pipelines and build systems. We wanted to fix that by building analytics that actually makes sense and helps teams understand what's happening in their CI.


Our journey started with providing compute and caching primitives that make CI faster and cheaper. But we also realize that that isn't the whole story - teams also need to spot those pernicious CI issues that gradually eat away at productivity. That's why we've built analytics that gives you a clear picture of your CI health and helps you catch problems before they become headaches.


## The next iteration


We've just shipped a new version of our CI analytics that makes it way easier to see what's going on in your pipelines. The interface is straightforward - you can sort and filter by the metrics that matter to you, toggle between current stats and historical trends, and zoom in from org-wide views down to specific repos. However, the breadth of questions these simple primitives allow you to answer is quite incredible. Here's a non-exhaustive list of questions you can now answer with Blacksmith:


- Have any of my pipelines significantly slowed down in the last week?
- Has job` foo` become flakier over time? Did it become flakier after we moved it to a smaller runner type?
- Which repositories are we spending most of our CI budget on?
- Does job` foo` have a tendency to time out after it has run for a while, or does it fail early on?


## Talk is cheap


Here's a real example of how this helps. Say you're checking your CI metrics on Monday morning and notice something off - your p90 had a huge spike recently. This piques your curiosity, but how do you dig in?


At the organization level view, you can quickly figure out which repos are causing trouble by clicking the` Split` button next to the chart. You might find that most failures are coming from one repository. Click into that repository and using the split view again, you could quickly discover that a specific set of Docker builds started acting up at around the same time.


What used to take hours of digging through different tools now takes minutes to figure out. That's exactly what we built these analytics for - helping you catch and fix issues before they become real problems.


## **Exemplars**


Investigating long-tail job instances (i.e. jobs that lie in the tail end of the distribution of durations) can often be very time-consuming. In addition to letting you view the high level distribution of your job runtimes, Blacksmith also points out exemplars at each interesting percentile of your job distribution.


As before, these exemplars are available across all the combinations of filters that can be applied at any level — making your CI data truly observable.


## What's next


If you're already using Blacksmith, these analytics are available in your dashboard today. For new users, it's still just a one-line change to your workflows. Email us athello@blacksmith.sh for a demo.


We believe we’ve just begun scratching the surface of what CI observability could be, and there’s a lot more coming to help our users identify the root cause of regressions.
