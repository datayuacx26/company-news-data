---
schema_version: "1.0.0"
document_id: "4acdd45333798bdc0fe858367718dccbc1ba863882e0d7f482fbe45cff21bbf0"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/what-if-your-production-traffic-was-your-test-suite"
published_at: "2026-03-09T17:52:40.640+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:18:56.741769+00:00"
content_hash: "sha256:9f7f628d04e002cd042cea84761267bd1281ed6802f49d871d3a32407c65dfcf"
---

# What if Your Production Traffic Was Your Test Suite?

## **On Engineering Excellence**


Sohil, my co-founder, and I were sharing stories several months ago about thorny quality engineering problems we faced while working in EPD teams before Tusk.


Both stories boiled down to the lack of good API tests.


Sohil had been a senior engineer at a vertical SaaS company for influencer marketing. Platform companies notoriously have to deal with external APIs, so he regaled war stories about solving P0s related to third-party integrations.


One time Instagram pushed an unannounced upstream change to their public API, which caused impression analytics on their SaaS product to falsely crater.


And because they didn't catch that API contract drift early, there wasn't any historical impression data to backfill for customers. Now their customer-facing reporting is messed up, that's bad.


My stories had more to do with internal APIs. Building product at an enterprise company regularly involved coordinating between product lines because of scope overlap.


My team, which was building an AI Sales Rep, was relying on another internal team to create a technographics and psychographics API that our service could call to personalize outbound emails with context.


While no serious incidents happened, we quickly realized the system was at the mercy of the other team. If params or values in the API response changed without us knowing, it would cause our AI BDR's email generation to fail or miss our content quality standards.


In both situations, you could easily shrug and say "Hey, that's not on me." But you can't do that in an organization that preaches Engineering Excellence. It requires that you stop these types of system-level issues blocking engineers from pushing code.
‍


## **Automating Replay Testing**


The standard approach to API testing is to write mocks with nock or MSW and then cross your fingers that external services don't drift.


Record/replay tools like VCR and Polly.js get you closer to the promised land but still require you to decide what to record. You write a test, run it once against live services, save the cassette. At the end of the day, it's manual and the developer is still the bottleneck for coverage.


From experience, most engineering teams do a good job at testing the happy path, but miss the real-world edge cases that happen in production. Unfortunately, even your principal staff engineer is victim to this because you can't always predict how users behave.


With[Tusk Drift](https://www.usetusk.ai/tusk-drift) , we've taken the opposite approach. We figured the best way to mock an external service is to capture the real response and reuse it. Instead of manually writing tests, you can:


1. Record live traffic: our[Node.js](https://github.com/Use-Tusk/drift-node-sdk) /[Python](https://github.com/Use-Tusk/drift-python-sdk) SDKs instrument your packages in-process to capture the inbound request and all outbound requests (HTTP, gRPC, DB queries, Redis, etc.), so there's no proxy or network config to deal with. Start your service with a record flag in the environment of your choice, let traffic flow for a week, and you'll get thousands of test cases derived from actual user behavior.


2. Replay traces as tests:[Tusk CLI](https://github.com/Use-Tusk/tusk-drift-cli) sends the original inbound request to your service but intercepts every outbound call and serves the recorded response as mocks. No need to spin up a live database or cache. Tests run in <50 ms each, with no side effects, and are fully idempotent.


3. Detect regressions: Tusk Drift replays traces in CI against your PR and diffs the responses. Our AI surfaces regressions by correlating the diff to your PR and ticket's context, so adding a new response field doesn't trigger false positives. When there is a regression, it suggests a fix. Additionally, when you merge a PR, the test suite gets refreshed automatically to reflect the new expected behavior.
‍


## **No More Test Code**


Replay testing takes us to a world where writing or maintaining test code is increasingly moot. You can just re-record traces whenever new code changes are merged and automatically refresh your API test suite with golden tests.


Preventing regressions is the obvious use case here. You record a week's worth of traffic, run traces in CI, then block PRs that show regressions. For midmarket teams, we're seeing Tusk Drift catch 60 regressions a month.


Refactoring safely is a use case that builds on top of that. At enterprises, you may have an untested Express monolith from 2016 that no engineer wants to touch. Recording traffic gives you an all-encompassing behavioral spec for the service. You can then refactor with AI, replay the traces locally, and verify if responses are identical.


Third-party integration monitoring also excites us. When Tusk replays newly recorded traces to curate its suite, it tests against real response shapes. If an upstream API changes its contract and your code can't handle the new shape, the trace replay flags it and sends you a Slack alert.


Node.js and Python (3.9+) backend services are supported, more languages coming soon. CLI and SDKs are open-source - come along with us on this ride as we make manual testing obsolete.


If you're tackling the thorny engineering challenges that other engineers shrug off,[talk to us](https://cal.com/team/tusk/demo) .


‍
