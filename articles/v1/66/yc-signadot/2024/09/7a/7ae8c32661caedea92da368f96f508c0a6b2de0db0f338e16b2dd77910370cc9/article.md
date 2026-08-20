---
schema_version: "1.0.0"
document_id: "7ae8c32661caedea92da368f96f508c0a6b2de0db0f338e16b2dd77910370cc9"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/shift-left-on-a-budget-cost-savvy-testing-for-microservices/"
published_at: "2024-09-20T15:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:9328fc0603b2e9997d5b7e645fda5c9fd4b4c30753371bcff7089be1f167d337"
---

# Shift Left on a Budget: Cost-Savvy Testing for Microservices

*Originally posted on*[The New Stack](https://thenewstack.io/shift-left-on-a-budget-cost-savvy-testing-for-microservices/) *.*


**For teams facing ongoing challenges with flaky tests and costly environments, these strategies are worth exploring.**


Developer velocity is like weather: Everyone talks about it, the basic mechanics have been understood for decades, but can anyone really control it? The reason that features come out later than expected is an example of the John Kennedy quote: “Victory has 100 fathers and defeat is an orphan.” Everyone has blame for work taking too long, but no one wants to claim responsibility.


Currently, the methodology for teams’ testing doesn’t work: It’s a poor developer experience and it’s causing pain every time we try to release code. We know this is a problem, but we can’t agree on the exact causes and solutions. Some of the current proposed changes — expansion of testing and[replication of environments](https://www.signadot.com/blog/environment-replication-doesnt-work-for-microservices/) — are expensive, ineffective measures. Here are some cost-efficient solutions.


## Why It’s Harder To Ship Code: Testing


As[platform engineers worked to measure DORA metrics](https://www.signadot.com/blog/limitations-in-measuring-platform-engineering-with-dora-metrics/) and thereby quantify the challenges of getting code out to production, meetings among platform engineers identified some common patterns hurting releases. While CI/CD pipelines have automated the release of code, too often these pipelines jam because the code being[merged encounters problems during automated testing](https://www.signadot.com/blog/the-struggle-to-test-microservices-before-merging/) .


When we speak to dev teams, they admit that code that’s merged to staging is often low-confidence without a ton of evidence that it will run on staging. The result is CI/CD pipelines that run automatically but can still take days to actually release code, with developers waiting on long-running processes to get their first realistic feedback on how their code works.


## The Problem With Current Testing Approaches


As we adopt[microservice architecture](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) , the ability to run reliable integration tests reduces, as we have many service dependencies and a more complex architecture that is harder and harder to simulate. The result is a reliance — or overreliance — on end-to-end (E2E) and unit tests, leading to the challenge of improving coverage without escalating costs.


E2E tests, where all the services that would handle a production request run on a staging environment, offer an incredibly reliable way to know your code is working. The trouble is that in a normal architecture these end-to-end tests can only be run fairly late in the process on a final staging environment, where all the changes from other teams also need to be tested.


To make E2E tests run earlier, teams adopt many expensive measures:


- Adding an environment before staging is expensive in terms of time, as platform/QA teams must then maintain two environments, and critical code releases must go through two stages.
- Replicating the whole staging environment for each developer or dev team presents two problems: the infrastructure expense of running a large number of services around the clock just for occasional testing, and the difficulty of keeping “staging clones” correctly updated.


In response to these limitations, some teams choose to implement more of the testing they already utilize in the hopes of making their testing phases more reliable.


## Why Brute-Force Testing Isn’t the Answer


Running a large number of early integration tests or an ever-expanding list of brittle, late-feedback E2E tests have a number of drawbacks as solutions for testing reliability. Integration tests that already failed to catch most problems before a merge to staging aren’t likely to fix our problems if we simply use more of them. And as implied above, E2E test feedback comes too late to make a nice, quick feedback loop for developers. Any approach that relies on expanding the number of tests also adds to one central problem: Tests are themselves code, and expanding a codebase means increasing maintenance work.


Developers are, naturally, reluctant to write a ton of new[integration or E2E tests](https://www.signadot.com/blog/the-struggle-for-microservice-integration-testing/) , being aware of their inadequacies and knowing they’ll likely have to fix these tests when they break.


How many times have you joined a team where a key metric was “test coverage,” and the only goal was that every line of code must be covered by two or three lines of testing? When it came time to run those vaunted tests, however, you’d discover that dozens or hundreds of tests were failing. With no time to fix them all and the focus on shipping new features, the list of broken, unexplained test failures grew, and the trust that a release that was “passing (most) tests” would work in production shrank each day.


## Introducing Shift-Left Testing as the Solution


If a core issue is the late feedback and low reliability of testing, then we must shift testing left so that it happens earlier in the process, with more certainty that we’re really simulating a realistic environment. If developers are running their tests directly, the same day or even the same minute that they write their code, they’ll find issues faster and with an eye on root causes from the start. And by taking direct ownership in running their tests, they’ll tend to write more realistic tests to get the best out of their tools.


We can think of “shifting left” in this context as moving more accurate testing to a staging environment (or whatever you call your environment before production) before merging code.


There are two primary components for running tests before merging that are necessary in order to enable deep testing like end-to-end before merging. First you’ll need some kind of manual testing and validation environment where you can run tests without “breaking” the rest of staging, and then you’ll need automated testing for critical paths and the integration APIs.


## Cost-Effective Testing With Preview Environments


Making preview environments available to developers can enable pre-merge testing, reducing the risk of late-stage issues. “But Nica,” I hear you say, “weren’t you railing against the cost and complexity of making multiple complete duplicates of staging?” Yes, we want our preview environments to be lightweight and only replicate the pieces we need to experiment with. The rest of our services can stay as “baseline” dependencies that don’t need to be altered.


## Enhancing Automated Testing Without Breaking the Bank


With reliable preview environments for every developer and a dedicated test runner, we can empower devs to run critical E2E tests on their pull requests before merging, and test API integration before their code goes to staging. Since developers are running tests directly, we can be more confident they’re running only necessary tests and getting useful feedback rather than adding hundreds of E2E tests to catch every possible failure on every service.


With a traditional method like mocks, a big expenditure of time is spent on creating these simulations of services and dependencies. Further the likelihood increases that mocks will fail to simulate the real world, cause a passing test that then fails on merge and slow down code deployment.


## The Impact: Quality Improvement and Easier Release Cycles


With developers running their tests directly, this frees up QA specialists to research systemic issues like test runner efficiency, test coverage and teamwide adoption of standards. The result is a product team that releases code faster with a lower overall cost of running.


## Conclusion


With solutions like Signadot, for many developers, preview environments can be managed as part of a single environment. Rather than making multiple clusters, environment replicas or namespaces, this approach keeps costs low.


Shift-left testing offers a path forward for teams dealing with the complexities of microservices. By moving testing earlier in the development cycle and providing developers with lightweight, reliable preview environments, teams can reduce the bottlenecks caused by traditional staging setups and late-stage E2E testing. This approach not only enhances the accuracy and speed of feedback but also optimizes resources by avoiding the high costs of multiple environment replicas.


The key takeaway? Testing earlier, with realistic setups and focused automation, leads to quicker feedback loops, better developer ownership and, ultimately, smoother releases. For teams facing ongoing challenges with flaky tests and costly environments, these strategies are worth exploring. They might just be the key to finally controlling that developer velocity issue everyone keeps talking about.


But don’t take my word for it — check out how[Brex](https://www.signadot.com/case-studies/brex-uses-signadot-to-scale-developer-testing-across-100s-of-engineers/) and[DoorDash](https://www.signadot.com/case-studies/how-developers-at-doordash-get-10x-faster-feedback/) have worked out how to produce better testing at a fraction of the cost.
