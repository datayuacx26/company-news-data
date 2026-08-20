---
schema_version: "1.0.0"
document_id: "27cfcf7989c454644ffd430fa11dd7e58223bd60faaab807650a637f6f2ceda2"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/how-promptfoo-scaled-test-coverage-with-tusk"
published_at: "2026-03-09T17:52:40.640+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:a8c92cef4927ea0b8cf82309534261655d184e4fe56e5bf320a394b52d235ec0"
---

# How Promptfoo Scaled Test Coverage Without Slowing Development

48%


Test incorporation rate


56


Bugs prevented


2,000+


Tests added


## About Promptfoo


[Promptfoo](https://www.promptfoo.dev/?utm_source=tuskblog) is an open-source platform for AI security testing. They provide tools for red teaming, guardrails, and evaluations that help AI companies catch vulnerabilities during their development lifecycle.


Backed by Andreessen Horowitz and Insight Partners, their project has over 10.8k stars, with more than 300,000 developers using it, including teams at over a quarter of the Fortune 500.


When your tools help other companies secure their AI systems, bugs in your own codebase must be squashed quickly since they can break the security workflows that users depend on.
‍


## Cost of Manual Backfilling


Promptfoo's engineering team wanted to move fast as a rapidly-growing company, but did not want to sacrifice reliability in the process. Thousands of developers watch every commit on their public repo, and Fortune 500 teams run production workloads on it.


Michael D’Angelo, CTO & Co-founder of Promptfoo, wanted a solution for maintaining test coverage that involved backfilling unit tests for existing code, and adding patch coverage for new PRs. Doing this manually meant pulling engineers off of building the security features that their customers were asking for.
‍


## Automating Unit Testing


Promptfoo added Tusk by syncing their repo and creating a Tusk-templated[GitHub workflow file](https://github.com/promptfoo/promptfoo/blob/main/.github/workflows/tusk-test-runner-app-vitest-unit-tests.yml) to provision GitHub Actions runners.


Tusk now runs in two modes on their[core repo](https://github.com/promptfoo/promptfoo) :


1. CoverBot analyzes the repo to find under-tested files, then opens PRs with test suites automatically. Engineers review and merge without having to write the tests themselves.


2. PR-level tests provide code coverage for changed files on every pull request, so new code doesn't ship without tests.
‍


## Detecting Bugs


Beyond generating unit tests for preventing future regressions, Tusk also flags bugs detected during PR analysis.


In[one CoverBot PR](https://github.com/promptfoo/promptfoo/pull/5087) , for example, Tusk added 20 tests across 3 files (taking them from 0% to 99% line coverage) and identified 3 bugs in a React component:


- Dialog crashes when the data array changes while open
- URL search params don't update after initial mount
- Error messages get hidden when data is also present


Each bug came with a description detailed enough to paste into Claude Code or Cursor. The team then fixed all three in the same PR before merging.
‍


## Impact on Code Reliability


Over half a year, Tusk added over 2,000 tests to Promptfoo's codebase and caught 56 bugs, improving code reliability before PRs were merged. As system behavior changed, Tusk would auto-maintain the test suite to remove flaky tests and update assertions.


> "We've had a great experience with Tusk. The team is incredibly responsive and genuinely helpful. Onboarding was smooth, and support didn't drop off after setup. It's clear they care about their customers' success."
>
>
> - Michael D'Angelo, CTO & Co-Founder


**In 7 months:**


- 48% of Tusk runs resulted in committed tests
- 56 verified bugs caught in PRs
- 2,000+ tests added (inclusive of CoverBot and PR test generation)
‍


## Try Tusk Today


Add unit tests to every PR and backfill coverage automatically with Tusk. Connect your repo and set up a test environment in one click. No prompt engineering required.[‍](https://cal.com/team/tusk/demo) *‍*[Try free for 14 days.](https://app.usetusk.ai/)


---


*Promptfoo helps enterprises and startups ship secure AI applications. Visit*[promptfoo.dev](https://www.promptfoo.dev/?utm_source=tuskblog) *to learn more about their platform.*
