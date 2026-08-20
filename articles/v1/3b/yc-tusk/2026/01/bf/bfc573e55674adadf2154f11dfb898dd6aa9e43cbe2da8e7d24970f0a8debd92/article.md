---
schema_version: "1.0.0"
document_id: "bfc573e55674adadf2154f11dfb898dd6aa9e43cbe2da8e7d24970f0a8debd92"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/how-hamming-strengthened-test-coverage-with-coverbot"
published_at: "2026-01-15T04:20:49.345+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:e373338a47323de4ed6ca87d82f9e6b82e70adae1707afceb22375357c547ba3"
---

# How Hamming Strengthened Test Coverage with CoverBot

66%


PR merge rate


1,800+


Total tests added


131


Bugs prevented


## **Introduction**


[Hamming](https://hamming.ai/) is transforming AI voice agent testing with automated testing infrastructure that's 100x more efficient than manual testing.


Backed by NEA, Y Combinator, Pioneer Fund, and Mischief, Hamming enables teams to create thousands of concurrent phone calls to AI voice agents using realistic voice characters, helping companies catch regressions instantly.


As a company building critical infrastructure for fast-growing AI companies, maintaining high code quality is fundamental to their mission.
‍


## **Problem**


The Hamming engineering team were no strangers to software testing, especially given that they themselves build testing tools for top AI companies.


**Sumanyu Sharma** , Co-Founder & CEO of Hamming, recognized that their own codebase needed stronger testing practices. Any bugs in their core evaluation system could directly impact their customers' ability to test and deploy voice agents reliably.


The team faced the challenge of any high-growth company: they needed to ship fast while also **backfilling test coverage** across their codebase.


Writing unit tests manually for existing code risked consuming engineering time that would be better spent on customer-facing features. Moreover, identifying which files needed the most testing attention was becoming a chore as their repo grew.


"We needed a way to increase our test coverage without slowing down our dev cycles," explained Sumanyu. "The challenge beyond writing the tests itself was also knowing where to focus our efforts for maximum impact."
‍


## **Solution**


> CoverBot has been ideal for our use case of getting test coverage up. We went from 2,500 tests on our core evals functionality to over 7,000+ tests in a month. CoverBot played a big part in achieving these gains.


Hamming decided to implement Tusk’s[CoverBot](https://www.usetusk.ai/coverbot) , an AI-powered workflow product designed specifically for backfilling unit tests.


CoverBot acted as their AI teammate that would create PRs twice a day to cover existing code that had minimal tests. Hamming’s engineers could then offload the work of backfilling unit tests for certain directories to the bot.


Traditionally, backfilling would mean manually identifying under-tested code by running a coverage script. CoverBot automated that end-to-end process by analyzing their repo to identify the least-tested files before creating test suites with full codebase context.


CoverBot proved to be what Hamming needed during their dedicated sprint to improve unit test coverage. The key differentiators:


1. **Proactive workflow** : As a background agent, CoverBot didn’t require a human to be in the loop. Hamming's engineers could wake up to find that their under-tested code had been automatically covered with comprehensive unit tests in a PR overnight.


2. **High test quality** : CoverBot was able to handle the complexity of their test setup without requiring prompting or manual iteration.
‍


## **Results**


In three months, Hamming enjoyed these results:


- **66% merge rate** for CoverBot PRs
- **1,800+ tests added** to their codebase
- **131 bugs prevented** before reaching production


Using CoverBot allowed their engineering team to focus on feature development while still getting the perks of robust testing. What would have taken months of consistent effort to address was shortened to a matter of days.
‍


## **Curious?**


Get PRs automatically on a daily or weekly basis with high-quality tests that raise coverage. No coverage analysis or prompting required. To find out more about Tusk,[book a demo](http://cal.com/team/tusk/demo?utm_source=tuskblog&utm_campaign=hamming-case-study) with us.


For automated AI voice agent testing that's 100x faster than manual testing, visit[Hamming](https://hamming.ai/) to learn how their platform can help you ensure reliability in your voice agents at scale.
