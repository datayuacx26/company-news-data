---
schema_version: "1.0.0"
document_id: "ae5cdacfb8e14f7ffcd1b7c9f66c3f81a6e6e295dc8b5b99fd9409af46840103"
company_key: "yc-starsling"
company: "StarSling"
source_id: "yc-starsling-atom-ebb22850b8ad"
canonical_url: "https://starsling.dev/blog/starsling-runners-are-now-generally-available"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:38.639551+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:2a8178d0f9caaf79885f3f492bbe12175da72c69397f5017c06c97654133469c"
---

# StarSling Runners are now generally available

Today we're launching[StarSling Runners: Self-Driving CI](https://starsling.dev/) in general availability.


StarSling is AI-native CI for GitHub Actions: drop-in Ubuntu runners plus agents that open optimization PRs for your workflows.


StarSling Runners are self-serve GitHub Actions runners for teams using` ubuntu-latest` or` ubuntu-24.04` : faster Linux machines, unlimited concurrency, queue time never billed, no seat fees, and **2,000 free minutes** in your first month.


The launch is also live on Launch YC today:


After four months in private beta, StarSling has now run over **one million CI jobs** for fast-moving teams like[Better Auth](https://starsling.dev/customers/better-auth) ,[Mastra](https://starsling.dev/customers/mastra) ,[Partcl](https://starsling.dev/customers/partcl) , and more. Their CI became **6x faster** and **13x cheaper** thanks to our faster hardware and agent-authored optimization PRs.


## Why this matters now


Coding agents made writing code dramatically faster, and they are pushing PR volume up. That compounds CI time and CI cost fast.


Fast runners help, but keeping a pipeline fast is still a recurring manual chore. Caches break, dependency installs get slow, tests get flaky, build steps pile up, and someone has to notice before the backlog gets bad enough to justify a cleanup project.


We think that work should move into the CI system itself: faster infrastructure for the jobs, plus agents that keep finding and shipping pipeline improvements.


## Start with faster runners today


- **Fast infrastructure on day one** : 5th Gen AMD EPYC CPUs, about 30% faster than GitHub-hosted runners
- **Zero-config cache compatibility** : works with` actions/cache`
- **Unlimited concurrency** : run more jobs at once without waiting for a shared pool to clear
- **Usage-based pricing** : pay for runner minutes, never queue time
- **No seat fees** : CI cost scales with work, not headcount


The current 4 vCPU Linux runner is **$0.008/min** , 33% cheaper than GitHub's $0.012/min equivalent. We also offer 2 vCPU runners at **$0.004/min** , with larger sizes available when you need more compute.


The migration is one line:


```text
jobs  :
test  :
runs-on  :   starsling-ubuntu-24.04
```


## Agents keep the pipeline improving


StarSling agents scan your workflows, jobs, run logs, and machine telemetry, then[open optimization PRs](https://starsling.dev/#ai-prs) for the kinds of CI work teams know they should do but rarely get to:


- fixing caching and missing cache keys
- speeding up dependency installs
- replacing blind sleeps with Docker health checks
- sharding tests
- parallelizing build steps
- restructuring workflows and job ordering


These are the[best practices StarSling agents check for](https://starsling.dev/best-practices/github-actions) , and any team can apply them by hand.


You stay in control. Every agent-authored optimization ships as a PR your team reviews and merges.


For new accounts, AI-powered optimization PRs are only available to customers on paid plans and are not enabled by default.


## What customers are saying


Private beta customers adopted StarSling because the runners were fast, but the agent PRs are what made the gains compound.


[Mastra](https://starsling.dev/customers/mastra) , the open-source TypeScript framework for building AI agents, got **6x faster CI** . Its slowest test suite went from 29m 56s to 5m 06s at p95 after migration and optimization. Under load, its p95 wait for a runner dropped from 14m 48s to 1m 48s.


> "The runners are just handled, so nobody on my team thinks about CI infrastructure anymore, and the agents keep optimizing for the one thing I care about: minutes saved." - Abhi Aiyer, Co-founder & CTO at Mastra


[Better Auth](https://starsling.dev/customers/better-auth) got **2x faster E2E** . E2E job time dropped from 2m 22s to 1m 04s, and the team saved roughly 20,000 CI minutes per month while weekly CI volume nearly doubled.


> "StarSling agents are like a CI engineer we never had to hire. They find the slow spots, test out fixes, find the ones that work and open the PRs themselves." - Bereket Engida, Founder & CEO at Better Auth


[Partcl](https://starsling.dev/customers/partcl) got **13x cheaper CI** . Its heaviest CI jobs became 13x cheaper per run than on its old self-hosted setup, queue time fell 16x, and daily CI job volume grew about 6x.


> "Within a day of migrating to StarSling Runners, their agents opened up a PR that literally made our Rust CI tests 2x faster!" - Vamshi Balanaga, Co-founder & CTO at Partcl


Full case studies are[here](https://starsling.dev/customers) .


## Secure by default


Every StarSling job runs in its own single-use, hardware-isolated microVM that is destroyed after the run, the same isolation model GitHub-hosted runners use.


Secrets pass directly from GitHub to your job and are never stored by StarSling. Your existing GitHub Actions workflow syntax, actions, secrets, permissions, and review process stay the same.


## Install guide


If your team runs CI on GitHub Actions using` ubuntu-latest` , you are one line away from faster runs:[Install the StarSling GitHub App](https://github.com/apps/starslingdev) .


Head over to our[docs for full instructions](https://docs.starsling.dev/#step-2-update-your-workflows) .


## Related guides


- [Fast GitHub Actions](https://starsling.dev/fast-github-actions)
- [GitHub Actions alternatives](https://starsling.dev/github-actions-alternatives)
- [GitHub Actions CI best practices](https://starsling.dev/best-practices/github-actions)
