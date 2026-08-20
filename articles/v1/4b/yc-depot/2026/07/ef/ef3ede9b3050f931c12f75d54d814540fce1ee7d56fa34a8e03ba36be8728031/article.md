---
schema_version: "1.0.0"
document_id: "ef3ede9b3050f931c12f75d54d814540fce1ee7d56fa34a8e03ba36be8728031"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/failure-fingerprints"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:3549a53e6f6e24bd9f0ab9e230731cabc41a18b8e89355a0b219deacdfa5afdf"
---

# Failure fingerprints: Comparing test failures over time

CI jobs are ephemeral by design. They start, run a set of commands, produce logs, report a result, and disappear into history. That model works well for executing the job, but it makes test history harder to reason about.


Structured test results are easier to filter and bubble up relevant ones. But filtering is not the same thing as history. A failed test tells you what happened in one run, which is useful for one-off errors but less useful when you are trying to understand recurring failures, flaky tests, or reliability problems over time.


## Borrowing from error tracking


When we started working with[structured test results](https://depot.dev/changelog/2026-06-02-test-results-beta) , the more interesting question was how to show more than "what happened in this run." I kept thinking about how Sentry handles application errors. For a reported error, it can show when it first appeared, when it last appeared, which commits or releases it was seen in, and other instances of the same error. That context is useful because it lets you debug the error as part of a pattern, not just as a single event.


The same idea applies to test failures. It is useful to know whether a failure is new or recurring, whether a test is flaky, when the issue first appeared, and where else it has happened. To answer those questions, we need to reliably identify the same test across runs and identify when that test failed in the same way.


That is where fingerprinting comes in. We derive two stable handles for comparison:


- An identity for the test itself.
- A fingerprint for the way it failed.


A test failure already contains most of the information about what went wrong. The hard part is deciding which details are stable enough to compare and which ones are just noise from a specific run.


## Finding the same test again


A test is not identified by just a test name. Depending on the test runner, framework, and result format, the same display name may appear in multiple files, suites, and classes, or across packages. To compare a test against its own history, we need to identify the same logical test across runs.


For every parsed test result, we can derive a stable test identity from the fields that define that logical test: suite name, file name, class name, and test name. This answers the first question of "Is this the same test as before?" Without that, history is unreliable. You can accidentally compare two different tests with the same name, or treat the same test as new because one piece of metadata shifted. Test identity gives us the baseline record to attach history to.


But identity is only half the problem since the same test can fail for different reasons. An assertion might have changed, a fixture could be missing, a timeout might have occurred, or perhaps the runner crashed. These should not be treated as the same failure just because they came from the same test. This is where failure fingerprinting comes in.


## Finding the same failure


Once we know we are looking at the same logical test, we want to know if it failed the same way. A failure fingerprint is a stable identifier derived from the failure data. We only generate it for failed or errored tests. Its job is to answer the narrower question of "has this test failed this same way before?" That distinction matters because "this test failed before" and "this test failed for the same reason" are different claims. The test identity tells us we are looking at the same logical test. The failure fingerprint tells us whether the failure looks like the same underlying problem.


## How we build a stable failure fingerprint


Raw failure strings are too brittle to compare directly. Failure messages, stack traces, temporary paths, timestamps, IDs, line numbers, and more can all change between runs even when the underlying failure is the same. This means we need to build a normalized representation first.


Our failure fingerprint uses three main pieces:


- The reported failure or error type, such as` AssertionError` or` TimeoutError` .
- The normalized failure message.
- The first relevant source stack frame.


Normalization is mostly about removing values that change from run to run but do not identify the underlying failure. For example, we want to collapse whitespace, normalize path separators, and replace noisy values like certain directories, UUIDs, memory addresses, timestamps, and durations.


We also include part of the stack trace, but not every stack frame is equally useful. Framework and dependency frames often tell us where the test runner reported the failure, not where the failure is most meaningfully connected to the code under test. When possible, we use the first frame that points to application code. A frame like` src/session/create.test.ts` is usually a better fingerprint input than` node_modules/vitest/dist/runner.js` because it stays closer to the test or code path that actually failed.


Then we hash the normalized pieces together for easier storage and comparisons.


## Fingerprinting is a balance


Failure fingerprinting is not about collapsing every similar-looking error into one bucket. Its value comes from choosing which differences are meaningful.


If the fingerprint is too strict, every failure looks unique. A line number or generated ID changes and the system reports a brand-new failure even though the underlying bug is the same. That destroys the value of the historical context.


If the fingerprint is too broad, unrelated failures get grouped together. This can unintentionally make a new issue look like an old recurring one, which can make it harder to debug because it hides the fact that something new may have happened.


The goal is to normalize noise while preserving signal. For example, the following failure messages should usually produce the same fingerprint because the request ID and worker ID are probably incidental.


```text
request id 123456789 failed on worker 987654321
request id 923456789 failed on worker 887654321
```


But the following assertion messages should not necessarily collapse together because those numbers may be the assertion itself. Normalizing them away could hide the actual difference between failures.


```text
expected total 1234567 to equal 7654321
expected total 1234568 to equal 7654321
```


The same applies to stack traces. We strip line numbers because line churn should not create a new failure. But we keep different relevant source frames distinct because a failure in` SessionService.create` is not necessarily the same as a failure in` SessionService.delete` .


## Why this is useful


Fingerprinting gives us a stable handle for connecting a failure to its history. Instead of showing only the output from the current run, we can give someone investigating the failure a starting point: whether this exact test has failed before, whether it failed with the same fingerprint, when that fingerprint first appeared, and whether the test later passed on another attempt.


Noisy or poorly understood test failures waste engineering time. People end up rereading the same logs, retrying jobs to see if the failure disappears, or learning to distrust failures that may actually be pointing at something real. A fingerprint does not explain the bug by itself, but it gives us a compact way to compare one failure with another without starting from the full failure output every time.


Fingerprinting helps make test results more than artifacts from a single run. It lets them become part of the history of how the suite behaves.


## FAQ


What is a test failure fingerprint?


A failure fingerprint is a stable identifier derived from a failed test's data: the error type, a normalized failure message, and the first relevant source stack frame, hashed together. It answers the narrow question of whether a test failed the same way it has failed before, rather than just whether it failed.


How do you identify the same test across CI runs?


We derive a test identity from the fields that define a logical test: suite name, file name, class name, and test name. A display name alone is not enough, since the same name can show up in multiple files, suites, or packages. The identity gives us a reliable baseline to attach history to so we are not accidentally comparing two different tests or treating the same test as new because one piece of metadata shifted.


Why not just compare the raw failure messages directly?


Raw failure strings are too brittle. Messages, stack traces, temporary paths, timestamps, IDs, and line numbers all change between runs even when the underlying failure is identical. Comparing them directly would report a brand-new failure every time one of those incidental values changed, so we normalize the noise out first and then hash what's left.


How does fingerprinting help with flaky tests?


Because the fingerprint is stable across runs, you can see whether the same test failed the same way repeatedly and whether it later passed on another attempt. A test that fails with one fingerprint and then passes on a retry looks very different from a test that consistently fails the same way, and that difference is what tells you flakiness from a real, persistent break.


## Related posts


- [Accelerating your test suite in CI](https://depot.dev/blog/accelerating-test-suites)
- [Depot CI run visibility in the dashboard](https://depot.dev/blog/run-visibility-depot-ci)
- [Now available: Step retry in Depot CI](https://depot.dev/blog/now-available-step-retry-depot-ci)


Iris Scholten


Staff Engineer at Depot
