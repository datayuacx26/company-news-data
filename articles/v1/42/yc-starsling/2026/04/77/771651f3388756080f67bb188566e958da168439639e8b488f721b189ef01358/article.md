---
schema_version: "1.0.0"
document_id: "771651f3388756080f67bb188566e958da168439639e8b488f721b189ef01358"
company_key: "yc-starsling"
company: "StarSling"
source_id: "yc-starsling-atom-ebb22850b8ad"
canonical_url: "https://starsling.dev/blog/announcing-starsling-runners-self-driving-ci-that-optimizes-itself"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:38.639551+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:bafd12df4485f6e3a98bb35f1ec864699e222f3fddb80c307054e93c03dc233f"
---

# Announcing StarSling Runners: self-driving CI that optimizes itself

> **Update - June 29, 2026:** StarSling Runners are now generally available. See the[GA announcement](https://starsling.dev/blog/starsling-runners-are-now-generally-available) for current availability, pricing, and AI optimization access. For new accounts, AI-powered optimization PRs are only available on paid plans and are not enabled by default.


Today we're happy to announce[StarSling Runners](https://starsling.dev/) : self-driving CI. Since switching to StarSling, customers like[Better Auth](https://better-auth.com/) and[Mastra](https://mastra.ai/) have gotten **82% faster GitHub Actions** ! Check out their results[here](https://starsling.dev/#results) .


StarSling Runners are an AI-native drop-in replacement for` ubuntu-latest` . Just install our GitHub App and swap out the` runs-on` line in your workflows and StarSling agents do a deep scan of your CI setup and start shipping optimizations.


Here are some examples of optimizations that Better Auth and Mastra have merged in recently:


- [ci: shard E2E kitchen-sink across 3 parallel jobs](https://github.com/mastra-ai/mastra/pull/15888)
- [test(chroma): reduce fixed 2000ms waitForIndexing sleep to 200ms and add Docker healthcheck](https://github.com/mastra-ai/mastra/pull/13866)
- [test(mssql): replace TCP healthcheck with sqlcmd, simplify pretest, pin Docker image](https://github.com/mastra-ai/mastra/pull/14520)
- [chore(ci): optimize Playwright browser installs in E2E](https://github.com/better-auth/better-auth/pull/8073)
- [chore: fix turbo cache configuration in ci](https://github.com/better-auth/better-auth/pull/7950)


## Our Story


Daniel led a 12-person engineering team at Netflix that built their Internal Developer Portal (IDP) called Netflix Console. Netflix Console was the "command center" for Netflix's 3,000+ engineers, managers and leaders and became the highest DAU internal tool for developers at Netflix. Prior to that he was a Software Engineer at Facebook where he worked on the AI Camera team on a computer vision library that currently powers camera effects for 100M+ DAUs across IG, FB, and WhatsApp.


I was previously Founder & CEO of StackShare, a community of over 1.5M developers and engineers. StackShare let developers compare and discuss tech stacks and it became the top destination for anyone choosing between developer tools. StackShare's enterprise SaaS offering was used by Fortune 500s to scan across thousands of internal codebases to surface tech stack data via dashboards and APIs. StackShare was acquired by FOSSA in 2024.


Daniel and I have been friends for over 10 years and decided to start StarSling last year after realizing how big of an impact AI could have on the DevOps stack. Developers typically lose ~20% of their day on annoying tasks outside of coding: investigating CI pipelines, fixing exceptions, improving app and database performance, resolving incidents, and more. So we thought, what if agents could take these tasks off your plate so you could spend all your time building features and products.


We joined YC's first Spring batch last year with a vision to build AI agents for all the DevOps tasks developers have to take on.


YC P25 Demo Day, June 2025


Since then we've been heads down in private beta iterating with early adopters to understand which use cases and DevOps integrations are most important to them.


## CI is the new bottleneck


Coding agents now write code in 5 minutes, but CI still takes 10+ minutes.


We hit this ourselves while building StarSling's DevOps agents during YC. CI became our bottleneck for merge speed. GitHub's hosted runners use older machines which run tests slowly and there's no built-in path to improve your setup over time. So we wasted a lot of time waiting for slow builds and whenever we got time (which was very rare) we tried to improve our CI setup.


GitHub Actions was also the single most requested AI agent from our waitlist:


So we built the CI system we wanted: **fast on day one, faster as time goes on using agents** .


## Why the world needs another CI runner


GitHub announced GitHub Actions pricing changes in[December](https://resources.github.com/actions/2026-pricing-changes-for-github-actions/) that make CI even more painful for many teams with lots of jobs. And the product itself isn't getting better since GitHub's focus has clearly shifted to other priorities.


There's no shortage of developers complaining about GitHub Actions these days:


Even so, GitHub Actions probably isn't going anywhere anytime soon:


*Source:[https://amplifying.ai/research/claude-code-picks](https://amplifying.ai/research/claude-code-picks)*


We're certainly not the first to try to make GitHub Actions better. Other CI runners solve speed too, but none of them are leveraging the recent advances in the models to continuously improve your pipelines. We believe CI should be just as agentic as coding is.


## What you get with StarSling Runners


- **Better hardware** : more powerful machines than GitHub-hosted runners
- **AI in your CI** : AI agents constantly analyze your workflows, jobs, run logs, and machine telemetry then open up PRs that optimize your setup


### How we speed up your CI


tl;dr better infra + AI agents.


#### Infrastructure


StarSling Runners have better hardware:


- 5th Gen AMD EPYC (30% faster than GitHub)
- Unlimited concurrency


#### AI Agents


StarSling's AI agents use frontier models to continuously perform deep scans of your workflow runs, then open up PRs to optimize:


- **Caching** : detect missing or misconfigured caches
- **Dependency installation** : implement faster install strategies (e.g., frozen lockfiles, parallel installs)
- **Build steps** : parallelize, remove redundant and optimize slow steps
- **Tests** : detect misconfigured test jobs and test database interactions
- **Workflow structure** : implement job splitting, matrix strategies, and dependency ordering


## Pricing


If you're using GitHub-hosted runners, we give you faster CI, with AI, while saving you money. Full pricing details[here](https://starsling.dev/#pricing) .


Our runners start at $0.004/min for 2 vCPU runners and $0.008/min for 4 vCPU runners (versus GitHub's $0.012/min). This was our April launch pricing language. For current availability, pricing, and AI optimization access, see the[GA announcement](https://starsling.dev/blog/starsling-runners-are-now-generally-available) . For new accounts, AI-powered optimization PRs are only available to customers on paid plans and are not enabled by default.


## Get started


If you're paying GitHub for GitHub Actions, we'll speed up your runs, optimize your workflows, and save you money on your bill.[Get started with the StarSling GitHub App](https://github.com/apps/starslingdev) , or[read the GA announcement](https://starsling.dev/blog/starsling-runners-are-now-generally-available) for the current launch details.


Happy Slinging 💫


Yonas & Daniel
