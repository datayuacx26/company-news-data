---
schema_version: "1.0.0"
document_id: "23f1e725d54bcb7fb4105afcd6d47608cbb095f401367fa149119760e5c44b28"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/launch-week-1"
published_at: "2023-05-19T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:5d6be5f540cd2393ddfe72bd341acb8f48974e6b46b5a4d3078a51071daeb0d1"
---

# Launch Week 1

# Launch Week 1


*Our first launch week at Evidence focusses on business reviews, for which we’ve added features, a new guide and a template.*


[Archie Wood May 19th, 2023 · 4 min read](https://www.linkedin.com/in/archiesarrewood/)


Today marks the end of our first launch week at Evidence. We’re excited to share what we’ve been working on and what we’re launching today.


## Wait, Launch Week? Evidence already launched


Yes, fair point. Starting now we’re going to be more explicitly launching *themed bundles of features, guides, and content* on a more regular basis.


The aim here is to deliver more concrete units of usable product to you, our users, rather than a drip feed of cool features.


We understand that making Evidence better serve use cases in businesses means more than just merging code to our main branch. To make something maximally usable Evidence needs:


- The feature itself
- Documentation on how to use it
- An articulation of why and when you should use it
- Templates and guides to help you get started


## What’s the theme of this launch?


This launch is centred on **business reviews** . That is, the process that companies run to review their performance against their goals. We’re starting here for two reasons:


- **Evidence’s first class text support makes it well suited for the process** - and we know some companies are already using Evidence for it
- **It’s a process close to my heart** - having been heavily involved in them in my previous roles


## What are we launching?


### A Business Reviews Guide


- [Business Review Guide](https://evidence.dev/blog/business-reviews) : shares what the aims of a business review should be, what common pitfalls there are, and solutions and tools that help here.


### New Annotation Components


Most business reviews involve comparing KPIs to targets. This is much easier to do when you can add visual elements such as target lines, zones and comparisons to your data.


We’ve released our first two **annotation** components to help you do this in Evidence: ReferenceLine and ReferenceArea.


- [Reference Line](https://docs.evidence.dev/components/annotations#reference-line) : Add a line to your chart to show a target or comparison


- [Reference Area](https://docs.evidence.dev/components/annotations#reference-area) : Add a shaded area to your chart to show a zone or region of interest


### An Automated Business Review Template


Part of the key workflow when running a business review is having stakeholders add commentary to their data, as well as recording actions. This is poorly supported in most BI tools, but we’ve put together an example of how you can use AirTable to make it easy for business users to add their commentary to an automatically refreshing report.


[Weekly Business Review Template](https://business-review-demo.netlify.app/) : This template shows three core bits of functionality:


- **Automatically Generated Weekly Report Outlines** : with charts, tables and KPIs for each week, and much of the commentary pre-filled
- **Business User Commentary** : Via a custom component with an AirTable integration
- **Action tracking** : that persists across weeks to ensure actions are followed up on


The source code is available[here](https://github.com/evidence-dev/business-reviews-demo) .


### Other Excellent Content on Business Reviews


We also want to point to some other content and tools discussing business reviews that we’ve found helpful, or mentions Evidence:


- [Building a business review program from scratch](https://www.datacouncil.ai/talks/building-a-business-review-program-from-scratch?hsLang=en) : an excellent talk I attended by Katie Bauer and Greg Johnson at Data Council 2023, Austin.
- [Rollstack](https://www.rollstack.io/) which is also tackling this problem head on with a focus on automating slide creation.
- [Therese Moriarty’s guide](https://medium.com/eyeful/how-to-automate-data-driven-presentations-b012c97622c7) to automating data driven presentations.
