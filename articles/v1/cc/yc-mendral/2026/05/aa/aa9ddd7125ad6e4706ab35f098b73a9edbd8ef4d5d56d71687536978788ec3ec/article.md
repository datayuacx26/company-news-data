---
schema_version: "1.0.0"
document_id: "aa9ddd7125ad6e4706ab35f098b73a9edbd8ef4d5d56d71687536978788ec3ec"
company_key: "yc-mendral"
company: "Mendral"
source_id: "yc-mendral-news-import-538be81b6878"
canonical_url: "https://mendral.com/blog/flaky-tests-cost"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-08-10T00:32:33.388808+00:00"
fetched_at: "2026-08-10T00:32:34.981599+00:00"
content_hash: "sha256:560c06b270052dd4559ed86dd660e5d5204aa2b35d153ce39322aeabaca67c54"
---

# What Flaky Tests Cost a 75-Engineer Team

A 75-engineer team loses about $375,000 a year to flaky tests. That figure is the developer-time line only. It's anchored in a five-year ICST 2024 industrial case study at a ~30-developer commercial codebase, which put the flaky-test tax at 2.5% of productive developer time. This post is the math: where the 2.5% comes from, what it works out to at $200K loaded cost per engineer, what the academic studies don't try to count, and what avoiding the cost looks like in production at PostHog.


## Where 2.5% comes from


Leinen et al. presented "Cost of Flaky Tests in CI: An Industrial Case Study" at ICST 2024. They analyzed five years of CI logs, commits, issue tickets, and tracked work time at a ~30-developer, ~1M SLoC commercial project. Their conservative finding: flaky tests consumed at least 2.5% of productive developer time. The split: 1.1% investigating false failures, 1.3% repairing flaky tests.


The 2.5% is the dev-time line specifically. Other published numbers come at the same problem from different angles. Google has reported that 16% of their tests show some level of flakiness, and that 2 to 16% of their CI compute budget goes to rerunning flaky tests. Parry et al. (2025) found developers in one industrial case study spent 1.28% of their time on flaky-test repairs alone, costing roughly $2,250/month at that team's rate.


These numbers come from different teams with different test cultures, so they don't all line up. The shape is consistent: flaky tests cost about 1-3% of productive engineering time, plus a measurable slice of CI compute. The 2.5% figure is a reasonable middle anchor and it's the one the recent academic literature defends most carefully.


## The calculator


Here's the math.


```text
Annual cost (dev time) = Engineers × Loaded cost per engineer × 2.5%


```


Worked example, 75 engineers at $200,000 loaded cost:


```text
75 × $200,000 × 0.025 = $375,000 / year


```


Loaded cost means salary plus payroll taxes, benefits, equity, tooling, and facilities. $200K is a defensible mid-market figure, and a conservative one: once you add benefits, equity, and overhead on top of a market salary, the real loaded cost of a software engineer often lands well north of it. Bay Area teams should plug in $250K to $300K. Distributed or earlier-career-heavy orgs can use $150K to $175K. The 2.5% doesn't move with the cost base, so the multiplier is linear.


Sensitivity table for a 75-engineer team:


Loaded cost per engineer Annual flaky-test tax


$150,000 $281,250


$200,000 $375,000


$250,000 $468,750


$300,000 $562,500


## CI compute is the second line item


Every flaky retry is a re-run. Sometimes the failing test, sometimes the whole job, sometimes the entire pipeline. The cost lands on your CI bill.


Google's published range puts 2 to 16% of CI compute on flaky-test reruns. The variance is high because it depends on test architecture, retry policy, and whether your team re-runs the whole pipeline or just the failing job. Most teams re-run the whole job, because doing it surgically is annoying.


Plug in your own bill. A 75-engineer team running real GitHub Actions volume usually sits between $10K and $50K/month on CI compute. Take the midpoint of Google's range (around 8%) and your monthly bill, and you're looking at $10K to $50K/year going to flaky-test reruns specifically. Smaller than the dev-time line. Easier to verify, because it's one query against your billing dashboard.


## The costs you can't put in a spreadsheet


The 2.5% and the CI compute line are the parts a CFO will accept. Everything else is real but harder to defend in a budget meeting.


Shipped-late deployments. A red build at 4pm on Friday becomes a Monday release, even when the failure was flaky. The half-hour spent re-running and the lost weekend deploy don't show up in the developer-time line, but they show up in deployment frequency and lead-time-for-changes. Orgs that track DORA metrics see this directly.


Trust erosion. Once developers expect CI to be flaky, they stop reading the signal. They retry without looking, they merge through known-flake failures, and they ship the real bugs that hide behind the noise. Datadog's writeup on flaky-test costs calls this the "psychological cost" and it's the one engineering leaders feel hardest. It's also the cost that grows with team size, because every new hire absorbs the team's habit of distrusting red builds.


Senior IC tax. The engineer who actually fixes the flaky test isn't the average engineer. It's the staff engineer who knows the test framework, the race condition, and the team's testing norms. That's the most expensive person's time you can spend, and the 2.5% calculation prices them at the team's average loaded cost. The true number is higher.


One honest caveat. The 2.5% assumes the alternative use of that time was shipping product. Sometimes it was a coffee break. Even granting that, the deep-focus cost of recovering from a context switch is poorly measured by the academic studies and points in the direction of underestimating, not overestimating, the tax.


## What avoiding the cost looks like


PostHog runs 22,477 tests across one of the largest public monorepos we work with. Their pass rate is 99.98%. At their CI volume, that still produces thousands of failed jobs every week that someone has to triage.


In the last month, Mendral accepted 104 fixes on PostHog's codebase. Not quarantines, fixes. A quarantined test is a flaky test you've stopped looking at, which lowers the noise but doesn't lower the underlying risk. A fix is one less flake forever, one less re-run forever, one less false alarm that erodes trust in CI forever.


The model is simple. Mendral's reliability agent watches CI, investigates failures, opens a PR with a proposed fix, and iterates on the review comments the team leaves. It behaves like a teammate. The team merges what's good and rejects what isn't. The accepted fixes leave the test suite healthier than they found it.


Run the calculator at the top of this post against your own headcount and loaded cost. Whatever number comes out, the dev-time line is the part you can already defend in a budget meeting, and it's only the floor.
