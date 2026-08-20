---
schema_version: "1.0.0"
document_id: "d79971f04c8c4cb381401afc42c305a1f3cb118d297baeda0a85f3bde0be6dd9"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/helping-engineers-to-product"
published_at: "2023-02-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:1d521f7d74ec0cab8abb5aa76127b952abbaf2099ed7c752d711dc503607a44c"
---

# Our simpler goal: Help engineers to be better at product

# Our simpler goal: Help engineers to be better at product


- [James Hawkins](https://posthog.com/community/profiles/27732)


Feb 17, 2023


#### Contents


-
-
-
-
-
-
-


One of the things I've learned at PostHog is the simpler a strategy, the more likely it's right.


We simplified[our strategy](https://posthog.com/handbook/strategy/overview)


recently. This post explains the context.


##


The change


Our mission has always been to increase the number of successful products in the world.


The change is that we've realized that the simple way PostHog can achieve this is to help engineers be better at product.


Today, engineers building the products you use are too often treated as a resource. They don't get a say in what gets built, they just pick up ticket after ticket. Yet, there's a reason they're engineers in the first place. They're intelligent, hard-working, and have deep skills.


Our competitors near universally focus on helping non-technical product managers, who write tickets for their developers.


We think this is backwards. Look at the way most teams work on tickets today:


- Endless alignment
- Endless requirements gathering
- Endless handoffs (design/product/engineering/sales/marketing/support)


If we reduce the need for PMs and help engineers go further by themselves, we reduce time lost on the above, unproductive activities, and give great engineers the thing they crave most: autonomy.


##


What's the strategy then?


###


Provide every tool needed for evaluating feature success


The best use of an engineer's time is to ship features that have an impact on customers. Currently, this requires a large number of tools and product managers to pull all the insights together. By integrating all these tools we can make this easy – no integration needed, no extra vendors, no extra javascript, and workflows to guide engineers through feature development, from initial idea, to release, measurement, gathering qualitative data, and back to the start.


###


Get in first


It’s the technical co-founder and early engineers building the MVP and integrating the first product tools, not PMs. By focusing on engineers we can get in first, and later become the default choice for each additional tool they add.


Additionally, we can ladder our tools – session recording is used much earlier in the life cycle of the product than others, like the[customer data platforms (CDP)](https://posthog.com/blog/cdp-vs-data-warehouse)


, helping us get in earlier than competing products. As a result, we aren't heavily focused on enterprise – we even[sunsetted K8s support](https://posthog.com/blog/sunsetting-helm-support-posthog)


as part of this change.


###


Be the pipeline for product and customer data


Traditionally, as companies scale their[data warehouse](https://posthog.com/blog/data-warehouse-at-posthog)


becomes the source of truth and non-warehouse native tools (like product analytics) become less relevant. By being their core pipeline from connecting their data to their warehouses we can remain sticky for the life of our customers. And by providing this infra, we ensure the data we have remains comprehensive. We will continue pushing back the need for companies to even set up a warehouse in the first place.


##


What's to come this year?


Right now on[our roadmap](https://posthog.com/roadmap)


, we're working on a slicker core experience – we've got many team members working on reliability, scalability, and data load times. We're adding power features for more technical users, like[SQL access](https://github.com/PostHog/meta/issues/81)


inside the product. We're also plugging gaps in our product, such as[iOS session recording](https://github.com/PostHog/posthog/issues/12344)


,[json feature flags](https://github.com/PostHog/posthog/pull/13623)


,[feature flag resilience](https://github.com/PostHog/posthog/issues/13601)


and improved SDK coverage for feature flags. Finally, we're pushing for[PostHog 3000](https://github.com/PostHog/posthog/issues/12923)


– a big UI change that will ultimately connect all our tools together better.


Next quarter, we'll be working on our CDP – we want to make that a first class citizen product – like our analytics or session recording. Today, we have 50+ "apps", largely community-driven, that mainly integrate PostHog with other data sources, but we want to add more integrations, deepen their functionality, improve their reliability, and make them more intuitive to use.


It's not all product:


-


In our growth team, we made a lot of progress last year on conversion to revenue, which grew 6x, but we're now looking at ways to give away as much as we can, for free, to *get in first* .


-


In our customer success team, we're getting more targeted in our approach so we focus on high-growth companies.


-


In marketing, we aim to produce the best content on the internet for each piece that we write. We're moving paid ad spend to hire another writer, we're producing more tutorials than ever before, and we're focusing more of our writing more tightly on how to help engineers learn skills outside of coding.


##


I'm excited


Ali, on our board, told us "when you get bigger, you can see around corners". That turns out to be true.


Now we've tried a bunch of stuff and achieved product-market fit – we have 23,000+ companies who have installed PostHog, approaching 70,000 developers in the community and $MM revenue – we can clearly see what we need to do. And, seeing the results we've had so far, increases our confidence we can make it all happen.


Wish us luck, and feedback (I'm james @ you can guess it . com) is more than welcome!


Subscribe to our newsletter


#### build mode


Read by 75,000+ founders and builders


We'll share your email with Substack


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
