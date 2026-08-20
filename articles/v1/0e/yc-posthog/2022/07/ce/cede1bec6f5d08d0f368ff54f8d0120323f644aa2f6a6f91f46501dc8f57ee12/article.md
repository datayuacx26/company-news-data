---
schema_version: "1.0.0"
document_id: "cede1bec6f5d08d0f368ff54f8d0120323f644aa2f6a6f91f46501dc8f57ee12"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/why-infra-is-our-competitive-advantage"
published_at: "2022-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:d11d73be49c2bc86b64075eb3a67af4f65933e724aa9203bf8823e1933d1a385"
---

# Why infrastructure is a competitive advantage for us

# Why infrastructure is a competitive advantage for us


- [Tim Glaser](https://posthog.com/community/profiles/27730)


Jul 20, 2022


#### Contents


-
-


We're hiring[Site Reliability Engineers](https://apply.workable.com/posthog/j/071DD5C05A/)


(SREs). I'm biased of course, but I think it's worth explaining why we think PostHog is the most exciting place to be an SRE right now.


##


Building a product


In most places, with the exception of outages, working in infrastructure means you're working on an invisible part of the company.


Sure, building out fast CI pipelines and continuous deployment, improving system reliability, and empowering other engineers to do more (providing methodologies, tools, etc) makes the rest of the engineering team more productive, but all your competitors do this too.


At PostHog, giving people the option to self-host is one of our biggest advantages compared to our competitors. That means we treat our self-hosted infra like a product. And, like any good product, we've written great public documentation.


To make sure our customers don't experience issues, we do automated testing of our Helm chart on all major cloud providers. We get[Issues](https://github.com/PostHog/charts/issues)


raised by real customers, not just internal 'consumers'.


It's not just that building stuff *for* our customers is fun.


Most of the people we deal with are experienced engineers and SREs themselves, and often folks will scratch their own itch by raising pull requests, like[adding backups](https://github.com/PostHog/charts-clickhouse/pull/383)


,[making improvements to security context](https://github.com/PostHog/charts-clickhouse/pull/377)


and[simplifying deployment commands](https://github.com/PostHog/charts-clickhouse/pull/366)


.


##


How to serve 20bn events for real time analytics


We don't *just* build infrastructure for other people to run.


We run one of the biggest PostHog instances ourselves, with >20 billion events. And before you ask, yes, our cloud infrastructure runs the same Helm chart that we ship to our customers.


Did you just read a sales pitch? Yes. Should you[immediately apply for the SRE role](https://apply.workable.com/posthog/j/071DD5C05A/)


at PostHog? Also yes.


Subscribe to our newsletter


#### build mode


Read by 75,000+ founders and builders


We'll share your email with Substack


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
