---
schema_version: "1.0.0"
document_id: "8cadf2f4334f51908e616c2e6fb9943cae8469a5d940a92fd7ef0884f39431fa"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/let-systems-do-the-work-a-new-approach-to-contract-testing/"
published_at: "2025-05-13T21:31:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:817d77bc50315ad960f1c44e48c098940865066c0e21cc49e5ed87c295384527"
---

# Let Systems Do the Work: A New Approach to Contract Testing

Contract testing has always promised a lot, and delivered only when developers had the time, energy, and discipline to maintain it. But let’s be honest: most teams don’t. That’s why we’re launching a better way.


This week, we’re excited to announce **Smart Tests** : a new take on contract testing, and you can try it out using our[Contract Testing Quickstart Tutorial](https://www.signadot.com/docs/tutorials/quickstart/contract-testing) .


## Why Contract Testing Fails in the Real World


Ask any developer what they think about contract tests, and you’ll hear something like:


> “Yeah, we tried Pact… didn’t stick.”


> “Too much maintenance.”


> “They get out of date. Fast.”


That’s because traditional contract testing puts all the burden on you:


- **You** write the contract.
- **You** keep it up to date.
- **You** figure out when something breaks.


Meanwhile, APIs evolve. Expectations shift. And your contracts turn into dusty old tombs that no longer reflect reality. We think that’s backwards.


Traditional testing approaches like[mocks often fail to capture the real complexities](https://www.signadot.com/blog/why-developers-shouldnt-write-mocks-a-guide-to-modern-testing/) of microservice interactions, leading to false confidence and production surprises.


## Our Philosophy: Let the System Do the Work


Your test tooling shouldn’t be another thing you have to maintain. It should *maintain itself.*


With Signadot’s approach to contract testing, we flip the model:


- Instead of requiring you to write complete contracts up front, Signadot lets you write as much or as little as you want, then fills in the rest by observing real service interactions. It uses the current baseline behavior of your services to infer an expected contract and tests every change against it automatically.
- Instead of maintaining specific infrastructure to validate contracts, Signadot **validates compatibility continuously** between upstream and downstream services using[ephemeral environments](https://www.signadot.com/blog/re-imagining-test-environments-for-microservices/) under the hood, no extra mocks or custom infrastructure needed.
- Instead of breaking things in production or relying on over-engineered pipelines, you catch issues **before the merge** with low manual setup, timely feedback, and most of the value right where it matters, in your Pull Request.


We believe testing should be:


- **Effortless** – inferred from real traffic and diffs.
- **Up-to-date** – automatically regenerated as your APIs evolve.
- **Targeted** – scoped to only what matters for the change you’re testing.


## How It Works


Let’s take a real-world scenario. Imagine you’re at a large company, and your frontend team keeps getting blindsided by backend changes that break the UI or cause unexpected behavior in the app. You want a safety net, but one that doesn’t require a massive platform initiative. Here’s how Signadot helps:


1. The frontend team writes a simple contract test that defines the critical API requests the UI depends on using Starlark, a subset of Python. This might be a few well-defined GET and POST calls with expected fields in the response.


2. Now, every time there is a backend service Pull Request, the test can be automatically run in Signadot’s[Pull Request preview environments](https://www.signadot.com/docs/guides/set-up-pr-sandboxes) by wiring it up with CI as follows:


3. Signadot compares the API behavior of the Pull Request against its baseline version to identify differences. When fields are removed, renamed, or modified, it highlights these changes and classifies them as high, medium, or low importance based on historical usage patterns of that API. For instance, in the example below, the response from the /location endpoint was changed—specifically, the field name was renamed to locName in this Pull Request. This is flagged as a high-importance change and it’s likely to break consumers of the API.


*Why classify changes by relevance? Not all API differences matter equally. Some fields like timestamps, request IDs, or trace IDs change frequently and are not meaningful to surface. Classification helps filter out this noise and focus on changes that actually impact API consumers.*


4. If you want stronger guarantees, you can set up stricter validation rules with[Signadot’s contract checks](https://www.signadot.com/docs/guides/smart-tests/contract-tests) , which allow you to assert field presence, types, optionality, and more.


5. That feedback appears directly in the Pull Request, showing exactly where and how the contract would break. Most of the gain, minimal manual setup.


6. Optionally, associate test runs with labels to organize the results of the test executions. This makes it easy to trace and debug when something breaks later in staging or production.


All of this happens in a real Kubernetes-based ephemeral environment with real services—no mocks, no test doubles, no extra maintenance burden. Just clean, timely, reliable contract validation.


Learn more about[how Smart Tests work](https://www.signadot.com/docs/concepts/smart-tests-and-jobs) in our technical documentation.


## Why This Matters


Because teams move fast. APIs change. Dependencies grow. And maintaining hand-crafted contracts across dozens of teams doesn’t scale.


Our approach scales **with your velocity** , not against it.


It’s a contract testing model built for:


- **Service owners** who want safety without friction.
- **Platform teams** tired of urging developers to write one more test.
- **Organizations** looking to ship faster *and* sleep at night.


## Get Started


Check out the[quickstart guide](https://www.signadot.com/docs/tutorials/quickstart/contract-testing) to try it yourself. It takes minutes, not days.


This isn’t just a new feature, it’s a new *philosophy* for how testing should work in modern systems: **observable, automatic, and out of your way** .


You have enough to do. Let the system carry the weight.


We’re just getting started and have a lot more on our[roadmap](https://github.com/orgs/signadot/projects/7) . We’d love your feedback, join the conversation in our[Slack](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) community and help shape what comes next.


## Additional Reading


- [Contract Tests Guide](https://www.signadot.com/docs/guides/smart-tests/contract-tests)
- [Running Synthetic Tests](https://www.signadot.com/docs/guides/use-cases/run-synthetic-tests)
