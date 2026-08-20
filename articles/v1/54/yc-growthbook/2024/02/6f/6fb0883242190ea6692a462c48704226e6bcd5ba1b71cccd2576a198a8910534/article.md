---
schema_version: "1.0.0"
document_id: "6fb0883242190ea6692a462c48704226e6bcd5ba1b71cccd2576a198a8910534"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/code-references"
published_at: "2024-02-27T00:00:00+00:00"
first_seen_at: "2026-07-27T15:19:37.358834+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:86edc720428e593483fc71ec939ef65baf0e7068fd93a1e06b3a61a48c4d1dc5"
---

# Code references

As companies grow, they often find themselves increasingly reliant on[feature flags](https://www.growthbook.io/products/feature-flags) . While these are valuable tools, they sometimes linger in the codebase, leading to technical debt. It's important for developers to be aware of this and consider regular clean-ups. If not addressed in a timely manner, this can become a challenging issue, potentially affecting the engineering team's efficiency and effectiveness. Proactive management of these feature flags can help ensure smooth and sustainable operations.


GrowthBook Code References showing feature flag instances surfaced directly in the GrowthBook UI, with file locations


**Code References** is a new feature that allows teams to quickly see instances of feature flags being leveraged in their codebase. By scanning customers’ code bases via CLI tool and sending results to our application backend, GrowthBook can help surface valuable information early and direct devs to the exact lines of code that need addressing.


Let's take a high-level look at how Code References in GrowthBook works and how your company can get started with its GrowthBook account.


## Overview


Code References requires implementing a step in your development[CI workflow](https://en.wikipedia.org/wiki/Continuous_integration) .


Since the task of searching for multiple feature flag keys across a potentially large codebase[can be hard](https://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_algorithm) , we've provided a low-level Go utility meant to run quickly on your CI infrastructure that can produce results that your GrowthBook API can process.


This utility is called[gb-find-code-refs](https://github.com/growthbook/gb-find-code-refs) , and is a fork of an existing open-source tool created by LaunchDarkly called[ld-find-code-refs](https://github.com/launchdarkly/ld-find-code-refs) . Our changes have made the tool more general purpose, so you can use it for your own purposes in addition to using it with GrowthBook.


Using` gb-find-code-refs` , you can create a CI job that will fetch feature flags from GrowthBook, then scan your codebase for those flags using` gb-find-code-refs` , and finally submit those generated code references back to GrowthBook.


The diagram below illustrates the flow of information from` gb-find-code-refs` to GrowthBook.


Diagram showing how gb-find-code-refs scans a codebase for feature flag keys and sends results back to GrowthBook


## Getting started


To support Code References, we provide a streamlined, all-in-one GitHub Action that integrates easily with your existing GitHub workflow. For non-GitHub users, we provide all the tooling you'll need to set it up yourself.


-See the[Getting Started section](https://docs.growthbook.io/features/code-references#getting-started) in our documentation for more information.


## Conclusion


The significance of Code References underscores a broader goal of more sustainable and efficient development practices, focusing not just on introducing new features but also on the long-term health and scalability of the software.


For companies seeking to maintain a competitive edge in software development, adopting tools like Code References is essential. We hope you find Code References in GrowthBook a powerful tool in your toolkit for managing feature flags efficiently and effectively.
