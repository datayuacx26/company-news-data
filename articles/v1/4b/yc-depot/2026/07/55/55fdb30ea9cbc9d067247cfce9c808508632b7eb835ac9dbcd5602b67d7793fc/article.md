---
schema_version: "1.0.0"
document_id: "55fdb30ea9cbc9d067247cfce9c808508632b7eb835ac9dbcd5602b67d7793fc"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-test-results"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T19:14:15.820819+00:00"
fetched_at: "2026-07-29T19:14:18.672801+00:00"
content_hash: "sha256:9767316de21c0dce775535f877ff11651ec176e1a75c14598b8ea5d0ebffbcc6"
---

# Now available: Test results in your CI jobs

Test results are now generally available: point Depot at your JUnit XML, and pass/fail/error/skip counts show up on the job that produced them. Use test results from Depot CI or Depot GitHub Actions runners to get reporting, analytics, timing-based splitting, and a single command that runs your suite and uploads the report.


## Structured results vs. a wall of log lines


CI job logs are a good starting point for understanding what tests ran in your job and where they failed, but they're noisy and don't give you data that can improve future runs. Structured test results give you something you can filter, join across runs, and reason about over time rather than just another wall of log lines. They help answer the questions:


- Which tests are failing most frequently?
- Is this a new failure, or the same flake from last week?
- Which tests are slow enough to drag the whole suite?
- Across the org, what's getting worse?


## What you can do today


**Report results from any Depot job (Depot CI or Depot GHA).** Upload JUnit XML directly with[depot/test-report-action](https://github.com/depot/test-report-action) or` depot tests report` , or run tests and upload them in a single step with[depot/tests-run-action](https://github.com/depot/tests-run-action) or the CLI` depot tests run` . Results show up inline on the job, with a full page to filter by status, suite, class, file, or test name.


**See history, not just this run.** Recurring failure context, latest failure links, and passed-on-rerun badges help you tell a brand-new break from the flake you've already seen.


**Look across the org.** The[Test Results Analytics](https://depot.dev/orgs/_/test-results/analytics) page surfaces frequent failures, new failures, slow tests, and flaky tests across Depot CI and GitHub Actions. Filter by repo, branch, suite, or timeframe.


**Pull failures from the terminal.**` depot tests <job-or-attempt-id>` prints the same parsed results the dashboard shows, so you (or an agent) can stay in the shell.


**Split suites by how long they take.** Naive sharding by file count doesn't consider the slowest shard. Depot uses durations from earlier reports to assign a time-balanced, non-overlapping subset of candidates to each matrix job. New suites with no data fall back to file size or deterministic weights until timing data accumulates.


Want the matrix example? The split tests guide walks through candidates, shards, and` depot/tests-run-action` end to end.


Split tests guide


## Split, run, and report


The beta started as reporting: run your tests however you already do, then upload the XML. That still works, and it's still the right path when you only need visibility.


Based on feedback during beta, we added timing-based splits so those same reports can balance shards across a matrix. For ergonomics, we also folded split, run, and report into one command and action so you don't have to wire the three yourself:


```text
jobs  :
test  :
runs-on  :   depot-ubuntu-24.04
strategy  :
fail-fast  :   false
matrix  :
shard  : [  0  ,   1  ,   2  ,   3  ]
steps  :
-   uses  :   actions/checkout@v4
-   uses  :   actions/setup-go@v5
with  :
go-version-file  :   go.mod
-   name  :   Install gotestsum
run  :   |
go install gotest.tools/gotestsum@latest
echo "$(go env GOPATH)/bin" >> "$GITHUB_PATH"
-   name  :   Run test shard
uses  :   depot/tests-run-action@v1
with  :
candidates-command  :   go list ./...
command  :   mkdir -p reports && xargs gotestsum --junitfile reports/junit.xml --
report-path  :   reports/junit.xml
```


Every matrix job uses the same candidate list. Depot assigns each shard a different subset based on historical timings, runs your command with those candidates on stdin, and uploads the report. Next run gets a better balance.


Same thing from the CLI:


```text
depot   tests   run   \
--candidates-command   'go list ./...'   \
--index   0   --total   4   \
--command   'xargs gotestsum --junitfile reports/junit.xml --'   \
--report-path   reports/junit.xml
```


Use` depot tests split` when you only need the candidate list and want to hand it to your runner yourself.` depot tests report` remains available if you want to upload without running.


` depot tests split` ,` depot tests run` , and` depot tests report` only work inside Depot CI and Depot GitHub Actions runner jobs (with` id-token: write` for GHA). They are not for local runs outside CI.


## Pricing


Every plan includes 1,000,000 passing tests each month. Beyond that, passing tests are billed at $0.000001 per test, which works out to $1.00 per 1 million tests. Failed, errored, and skipped tests are free. See[pricing](https://depot.dev/pricing) .


## Getting started


1. Produce JUnit XML from your test runner.
2. Either add[depot/test-report-action](https://github.com/depot/test-report-action) after your test step, or switch the step to[depot/tests-run-action](https://github.com/depot/tests-run-action) /` depot tests run` .
3. Open the job in the Depot dashboard to see your test results summary. From there, jump into the full results page or org analytics.


Docs:


- [Depot CI test results](https://depot.dev/docs/ci/observability/depot-ci-test-results)
- [GitHub Actions test results](https://depot.dev/docs/github-actions/observability/github-actions-test-results)
- [Split tests by timing](https://depot.dev/docs/ci/how-to-guides/split-tests)


## What's next


If you've been on the beta, nothing about your existing` depot/test-report-action` setup needs to change. You're already on the GA path. If you haven't tried splitting yet, start with` depot/tests-run-action` on a matrix and watch your suite get faster as Depot learns how long your tests take.


## Related posts


- [Failure fingerprints: Comparing test failures over time](https://depot.dev/blog/failure-fingerprints)
- [Accelerating your test suite in CI](https://depot.dev/blog/accelerating-test-suites)
- [Depot CI run visibility in the dashboard](https://depot.dev/blog/run-visibility-depot-ci)


Iris Scholten


Staff Engineer at Depot
