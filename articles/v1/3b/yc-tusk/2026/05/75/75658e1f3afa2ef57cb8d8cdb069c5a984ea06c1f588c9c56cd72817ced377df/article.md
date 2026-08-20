---
schema_version: "1.0.0"
document_id: "75658e1f3afa2ef57cb8d8cdb069c5a984ea06c1f588c9c56cd72817ced377df"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/how-metricwire-catches-api-regressions-with-tusk-drift"
published_at: "2026-05-22T17:22:58.426+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:12:57.660175+00:00"
content_hash: "sha256:1c42d41ecdd4dbe1805992ef16352a32719856a305adfb83f4cdfd9c48d12286"
---

# How Metricwire Catches API Regressions Before Merge With Tusk Drift

50%


API test coverage added


325


Regressions caught


760


Deviations surfaced


## About Metricwire


[Metricwire](https://metricwire.com/) is a clinical research data-collection platform used by universities and pharma sponsors to run remote studies. Their mobile app captures survey responses, sensor streams, cognitive task submissions, and e-consent signatures from participants. This data eventually anchors published research and regulatory submissions.


Regressions in the API layer, therefore, run the risk of corrupting longitudinal study data, breaking shared-device handoffs between participants, or silently polluting responses that hundreds of mobile clients depend on.
‍


## Running Into Regressions


> "With Tusk, our team can easily anticipate real world edge cases. We run Tusk Drift in CI and sometimes locally to spot anomalies and fix them before merging changes.
>
>
> - Charles De Souza, Founder & CTO


‍


Metricwire ships fast on their Node.js backend, working on features like live document sync and task submissions. Most of these changes touch shared helpers inside controllers that fan out to Mongo, Firebase, S3, and Sendbird.


The failure mode the team kept running into was subtle API contract drift. It could be a new Mongoose schema field with` default: null` that quietly changes the shape of every write, or a post-sanitization helper that re-populates a field that a cleanup step had just deleted.


None of these show up as test failures in a unit test run because the unit tests use mocked data that don’t exercise the exact Mongoose write or the exact object-shape that the mobile client sends. They only surface when users experience the bug in production and report them.
‍


## Covering Real World Edge Cases


Metricwire's team decided to automate API testing to solve the drift problem. They installed and initialized the Tusk Drift SDK on their backend repo to record live traffic with PII redacted.


Within minutes of activation, Tusk curated an API test suite containing 300 tests covering a representative set of endpoints. The agent would run this suite in CI and post deviation reports back on the PR to help catch regressions before merge.


The regressions that Tusk caught for Metricwire had a common pattern. Each regression was introduced by an otherwise-sensible change and was only detected when production-shaped traffic was replayed against new code. Two examples:


**Incomplete migrations:** When Metricwire migrated from one database provider to another, a rewritten function quietly dropped the defensive null checks the original implementation had relied on. The new code looked correct on its own, but one of its pre-existing callers passed only a subset of the optional inputs. This resulted in a 500 error on an endpoint. Tusk caught the contract drift because it replayed real upstream callers against the new code.


**Side effects that fire too broadly:** A PR adding a hook for auto-provisioning a default site introduced a post-save trigger that looked surgical in the diff, but fired on every site save. This added an unexpected DB lookup on a hot write path and broke an existing "create site" endpoint. Tusk flagged the overly broad trigger on the first run. Metricwire then scoped the hook down to new-record creation only; Tusk re-ran to verify that the behavior was correct.
‍


## Impact


Across one month of active use:


- **50% API test coverage added** by Tusk and auto-maintained as code logic evolves
- **325 unintended API changes** flagged before they reached production
- **760 total deviations** surfaced for engineers’ review


For a platform where a regression risks corrupting research data for large institutions, replaying real traffic against every PR has become a guardrail that is table stakes for Metricwire.
‍


## Try Tusk Drift


Create a self-maintaining API test suite from your production traffic in minutes.[Try free for 14 days](https://app.usetusk.ai/) .
