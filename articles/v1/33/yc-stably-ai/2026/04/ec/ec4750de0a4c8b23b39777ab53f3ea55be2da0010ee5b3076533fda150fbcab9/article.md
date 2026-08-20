---
schema_version: "1.0.0"
document_id: "ec4750de0a4c8b23b39777ab53f3ea55be2da0010ee5b3076533fda150fbcab9"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/cloud-browsers-scale-your-test-suite-without-the-infra-headache"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:c52a59c3bbac07dbe2b4e5989bc40dfe6ef728b392c2617ec947a0ca3d6f7a71"
---

# Cloud Browsers: Predictable Browser Capacity for Enterprise QA

Today we're launching **` --browser=cloud`** for Stably CLI: a way to run Stably's AI-assisted create, fix, and verify workflows in managed cloud browsers instead of on your laptop or CI runner.


For enterprise QA teams, the value is not novelty. It's operational control. You can increase parallelism for` stably create` ,` stably fix` , and` stably verify` without tying that throughput to the CPU and memory limits of the machine invoking the command.


The constraint is usually not the agent. It's browser capacity.


On a typical developer laptop, a handful of concurrent browser instances is enough to saturate CPU and memory. On standard CI runners, even modest concurrency can cause timeouts, crashes, or severe slowdown. The result is familiar to every QA team: slower maintenance work, noisier CI, and more time spent triaging infrastructure symptoms instead of product issues.


## The problem with local browsers


When multiple AI-driven browser sessions share the same machine, they compete for CPU, memory, and network bandwidth. That creates three practical problems for enterprise QA teams:


1. **Parallelism is artificially capped.** Teams avoid running many create or fix operations at once because they know the machine will become the bottleneck.
2. **Failures become harder to interpret.** A timeout may reflect an overloaded runner, not an actual product issue or stale test.
3. **CI becomes expensive in the wrong way.** The workaround is often bigger runners, lower concurrency, or both.


This is why browser-heavy maintenance work tends to drag. Teams know they need to backfill coverage or repair a broken suite, but the operational cost of running many browser sessions at once makes the work slower and less predictable than it should be.


Cloud browser mode addresses that bottleneck by moving browser execution off the local machine while keeping the CLI workflow intact.


## One flag, same CLI workflow


```text
stably create "test the checkout flow" --browser=cloud


```


The command stays the same. The difference is where the browser runs.


Stably provisions an isolated browser session in the cloud, uses that session to navigate and interact with your app, and writes the resulting test output back into your local repository.


You can also set it for an entire session:


```text
export STABLY_CLOUD_BROWSER=1
stably create "test login"
stably create "test onboarding"
stably create "test billing"
stably fix


```


Every supported Stably command in that session uses cloud browser mode.


The flag works with` stably` ,` stably create` ,` stably fix` , and` stably verify` . (` stably test` continues to run through standard Playwright execution—your existing test runner stays exactly as-is.)


## What this changes for enterprise QA


The main benefit is not that one command becomes dramatically faster. The benefit is that throughput becomes more predictable when many browser-opening workflows need to happen at once.


### Backfilling test suites in parallel


One common use case is coverage backfill after a team has adopted Stably but still has major gaps across legacy flows. Instead of serializing test creation work to avoid overwhelming a laptop or CI runner, teams can run many` stably create` commands in parallel and let each one use its own isolated browser session.


An anonymized example from one enterprise team:


- They needed to add coverage across authentication, billing, and admin workflows before a release hardening cycle.
- Their bottleneck was not prompt writing. It was the number of browser sessions they could run safely at once.
- With cloud browsers enabled, they were able to backfill more than 80 test cases across three product areas within a sprint without dedicating a separate infrastructure effort to browser capacity.


A representative workflow looks like this:


```text
export STABLY_CLOUD_BROWSER=1


# Run these in parallel
stably create "test user registration with email"
stably create "test SSO login with Okta"
stably create "test role-based access for admin"
stably create "test invoice generation"
stably create "test Stripe subscription upgrade"
stably create "test data export to CSV"
# ... 50 more


```


Each command gets an isolated browser session, which makes high-parallelism generation practical instead of fragile.


### Fixing hundreds of broken tests at once


Another common use case is suite recovery after a redesign, a front-end refactor, or a large selector churn event.


With` stably fix --browser=cloud` , Stably runs against your failing tests using cloud browsers. It reads each failure, replays the test in a fresh browser, determines whether the test needs updating or there's a real bug, and edits your test code accordingly.


That matters because large-scale repair work is operationally different from normal authoring. When 100 or 300 tests fail, the challenge is no longer "can this test be fixed?" The challenge is "can we process this backlog quickly without creating more instability in the environment doing the repair work?"


An anonymized example:


- A team had accumulated hundreds of broken or flaky end-to-end tests after a UI overhaul.
- They used cloud browser mode to run many fix operations in parallel rather than queueing them behind a small number of local browser slots.
- The practical outcome was shorter maintenance cycles and faster restoration of CI signal after large UI changes.


## Why cloud browsers matter in CI


In CI environments,` --browser=cloud` helps in ways that matter directly to QA operations:


- **More predictable maintenance throughput.** You can run more create, fix, and verify work in parallel without sizing the runner around browser load.
- **Fewer resource-induced false failures.** Timeout-heavy behavior caused by local machine saturation becomes less of a factor during browser-based repair and generation work.
- **Lower CI infrastructure pressure.** Teams do not need to overprovision runners just to keep these workflows stable.
- **Cleaner isolation model.** Each subagent gets its own browser session rather than competing within one overloaded machine.


## Rollout considerations


For enterprise teams, this is typically the right rollout pattern:


1. Start with maintenance workflows such as` stably create` ,` stably fix` , or` stably verify` .
2. Enable cloud browser mode in one CI job or one engineering team first.
3. Measure a few operational metrics that QA leadership already cares about:


- Time to backfill a target set of critical-path tests
- Time to recover a broken suite after UI changes
- Failure rate caused by runner saturation or timeout-heavy execution
- CI cost from overprovisioned runners used only for browser capacity


This tends to be an easier adoption path than reworking the whole test runner stack, because` stably test` remains unchanged. You are improving browser capacity for create/fix/verify workflows first, where the infrastructure bottleneck is usually most obvious.


## Getting started


If you're already using Stably CLI, enable it on a single command:


```text
stably create "test checkout" --browser=cloud


```


Or set it globally:


```text
export STABLY_CLOUD_BROWSER=1


```


If you're new to Stably, check out our[cloud browser docs](https://docs.stably.ai/stably-cli/cloud-browser) for the full setup guide.


For enterprise QA teams, cloud browsers are not about adding another testing surface area. They are about making high-parallelism browser work easier to operate. If your team is trying to backfill coverage, recover from a large UI change, or reduce CI instability during create/fix/verify workflows, this is the most direct place to start.
