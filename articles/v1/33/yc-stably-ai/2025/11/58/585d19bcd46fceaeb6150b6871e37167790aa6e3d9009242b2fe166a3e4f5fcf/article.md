---
schema_version: "1.0.0"
document_id: "585d19bcd46fceaeb6150b6871e37167790aa6e3d9009242b2fe166a3e4f5fcf"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/real-auto-heal-for-end-to-end-tests"
published_at: "2025-11-20T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:750ae4b002ee5c32b52c8943665bc0aebf7a97ecb713950e3e763620a930419d"
---

# Real Auto-Heal for End-to-End Tests (Inside Your Infra)

Your team ships fast.


Your end-to-end tests break faster.


On large suites, **60–80% of failures come from outdated tests and flakes** , not real bugs. Engineers burn hours each week nursing tests back to green. QA leads spend more time triaging noise than preventing regressions.


Most “AI test healing” tools only patch selectors. They can’t reliably tell **test drift vs real bugs** , don’t see your repo or observability stack, and usually **don’t run inside your VPC** .


## Why E2E test maintenance collapses at scale


You add end-to-end tests to increase confidence.
Fast forward a few quarters, and they’re mostly a source of noise.


- The app changes weekly; tests don’t.
- Failures pile up from DOM changes and brittle locators.
- Nobody quite “owns” fixing them, so people start ignoring red builds or turning suites off.


What was supposed to protect velocity quietly becomes a **tax on velocity** .


That’s exactly the layer you want an agent to take over—if it’s safe and reliable.


## Why real auto-heal is rare


"Auto-heal" is easy to say and hard to do. Most tools fail because they can't:


- **Classify failures reliably** – without controlled re-runs, everything looks like a selector issue
- **Understand test intent** – safely editing tests requires knowing the user journey and assertions
- **Access full-stack context** – distinguishing drift from bugs needs traces, logs, git history, and observability
- **Run in your infra** – real context requires VPC access, RBAC compliance, and the ability to test applications in internal sandboxes that aren't publicly accessible


Stably solves all four by running as a **CLI inside your CI pipeline** , with access to the same code, artifacts, and tools your engineers already use.


## What we built at Stably


At Stably, we built **real auto-heal** : an agent that runs in your CI, uses full context from your code and infra, and actually **updates test code** —not just selectors.


## How Stably auto-heal works in your pipeline


At a high level, Stably does three jobs for your Playwright suite:


1. **Automatically triage failures**
2. **Edits tests to match your evolving product**
3. **Escalates real regressions with full context**


You wire it into CI alongside your existing` npx playwright test` runs and invoke it on a run ID. Everything happens **inside your infra** .


### 1. Integrated with Playwright inside your CI


You don't replace anything. You layer Stably on top:


- Your pipeline still runs` npx playwright test` as usual.
- Stably ingests the results and artifacts from that run.
- You call the healer agent (for example,` npx stably heal <run-id>` ) as a post-step.


From there, it talks to:


- Your git repo
- Your Playwright traces, logs, DOM snapshots, screenshots
- Optional observability tools


all under your current IAM, networking, and audit policies.


### 2. Automatically triage failures into flakes, drift, and real bugs


**Outcome:** you get a typed list of issues—flakes, drift, and real bugs—instead of a single red blob.


Stably fingerprints each failure using:


- Playwright artifacts: traces, network, console, DOM, screenshots
- Git history: recent commits, diffs touching relevant files
- Optional observability: logs, traces, deployment metadata


Then it tags each failure as:


- **Flaky** – intermittent, timing or environment issues
- **Test drift** – consistent failure caused by a legitimate UI/flow change
- **Real bug** – consistent regression aligned with code changes and broken behavior


**Example:**


A checkout test starts failing overnight.


Stably sees that:


- The failure is 100% reproducible.
- Yesterday's commit added a consent checkbox to the payment modal.
- No backend errors or 5xx responses show up in traces.


It tags this as **test drift** , not a flaky test and not a backend outage.


You see that classification immediately, without having to reverse-engineer the failure yourself.


### 3. Edits tests to match your evolving product


**Outcome:** you can let it touch your test code and still sleep at night.


Stably's auto-heal uses the same engine as our **Test Creation agent** —the system that already learns your app by exploring real flows. Over time, it sees more of your product than any single engineer or QA on the team.


That lets it confidently update tests, even for complex UIs:


- Interacts with **iframes** , **file uploads** , **drag-and-drop** , and other rich components
- Converts fragile, DOM-dependent flows into **super stable locators and assertions**
- Keeps tests aligned with how the product actually behaves today, not six months ago


You get small, reviewable diffs that feel like a senior engineer kept your suite up to date in the background.


### 4. Escalates real regressions with full context


**Outcome:** real bugs don't get "auto-healed away." They get promoted.


When Stably identifies a **real bug** , it:


- Groups related failures into a single incident.
- Generates a concise summary: what broke, where, and likely why.
- Attaches the key evidence:


- Playwright traces, screenshots, DOM snapshots
- Relevant logs and stack traces (if integrated)
- The commit that most likely introduced it


You can plug that into Jira, Slack, PagerDuty—whatever you already use.


**Example:**


A login test starts failing with a consistent 500 after a specific deploy. Stably spots the server error in traces, links it to a new auth-service commit, and files it as a **real regression** , instead of trying to "fix" the test.


The autonomous part is simple: the suite knows when to heal itself—and when to call a human.


## What we've seen in practice


Teams using Stably auto-heal have cut end-to-end test maintenance by **50–75%** while keeping suites **9%+ green** —often enough to **defer QA/SDET hires** and turn CI back into a real quality gate.


From early deployments on Playwright-based E2E suites:


**Fintech (~2,000 tests)**


- ~75% reduction in recurring test-maintenance effort.
- CI stability high enough to confidently block PRs on the suite again.


**Enterprise SaaS (multi-team monorepo)**


- Green runs improved from ~40% to **95%+** within a few weeks.
- Leadership explicitly delayed **two QA hires** because auto-heal + auto-triage absorbed the expected maintenance load.


Teams describe the shift like this:


- "We stopped treating flaky tests as the tax we pay for E2E coverage."
- "Most of our test time now goes into real bugs, not fiddling with selectors and waits."


In other words: the suite starts to feel like an asset again.


## Beyond healing


Auto-heal is one part of Stably's AI testing platform.


The same foundation also powers:


- **Test creation** – generating high-quality Playwright tests from real user flows and specs
- **Scalable test running** – managed cloud infrastructure to run those tests in parallel at scale, without your team babysitting browsers or CI capacity


So your tests don't just stay green; they're also easy to create and cheap to run, even as your product and coverage grow.


## Try it on your own suite


Stably layers on top of your existing Playwright setup—no rip-and-replace. Teams typically see impact within a single sprint.


Real auto-heal lets your end-to-end tests move at the same pace as your product— **without turning your engineers into test janitors.**
