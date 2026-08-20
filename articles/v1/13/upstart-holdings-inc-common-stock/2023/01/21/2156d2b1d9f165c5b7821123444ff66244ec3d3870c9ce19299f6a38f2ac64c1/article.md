---
schema_version: "1.0.0"
document_id: "2156d2b1d9f165c5b7821123444ff66244ec3d3870c9ce19299f6a38f2ac64c1"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/flaky-tests-and-ci-build-stability-a-2022-summary-2ca92f9299ff"
published_at: "2023-01-09T20:07:14+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:6ce90caefdbc2d9b8ee05606bd3f7033697748425cb17b16179e0b8888e8079d"
---

# Flaky Tests and CI Build Stability: A 2022 Summary

# Flaky Tests and CI Build Stability: A 2022 Summary


[stephskardal](https://medium.com/@stephskardal?source=post_page---byline--2ca92f9299ff---------------------------------------)


7 min read


·


Jan 4, 2023


--


Press enter or click to view image in full size


[https://www.upstart.com/](https://www.upstart.com/)


Here at Upstart, we’ve done quite a bit of work over 2022 to improve continuous integration (CI) build stability for our monolith as part of a greater effort to improve our tech platform. We’ve done other great things, but CI build stability and dev experience is the focal lens I often choose to write about.


We are actively building out microservices outside of our Ruby on Rails monolith. In order to extract microservices, we must have stability in our CI and continuous deployment (CD) to increment out of the monolith. We see more than 100 engineers contributing to our monolith weekly. Instability in the form of tests or architecture impacts those engineers on our get-out-of-the-monolith train!


## Q2: The Merge Queue


Early in the year, an automated merge robot (‘borg’) was introduced to minimize implicit merge conflict failures for those >100 engineers. Prior to that, we had gone through many experiments around optimal merge patterns on our high velocity monolith:


- Merge when your build is green — take to production when you want!
- Merge when your build is green. Enforce a schedule to release at least once per day.
- Weeks long merge freeze to get everything under control when things have gotten out of control.
- Merge all the mergeable things in the evening. Work through conflicts the next morning to get a release out daily.
- Create a team to support daily releases to own conflict resolution. Build up a team of individuals from each product team to own daily releases.
- Merge via a queue, with high priority labels jumping to the front of the queue.


Each of these strategies had various trade offs. A major trade off introduced in merging everything at once was CI build instability due to merge conflicts and flaky tests. Once we merged everything at the end of the day, the next morning was spent actively triage errors to identify if failures were new issues, implicit conflict failures, or test flakes.


The ‘borg’ was introduced at the end of March to eliminate merge queue issues and automate many of our challenges around conflicts. Every half hour, the borg runs CI with batches of mergeable PRs. The largest passing-build batch (by PR count) gets merged at the end of the build and the next half hour the process starts all over again. This happens all day, every day.


The introduction of ‘borg’ yielded a build success rate increase from ~30% to ~70%, effectively eliminating merge conflicts and the time spent by many triaging those conflicts. The borg has been happily chugging away since March with minimal tweaks!


## Q3: Dissatisfaction in Flakes and Isolated Retries


Around the middle of the year, a productivity survey was sent out to engineers. Despite the major success with the introduction of the automated merge queue and the beloved ‘borg’, release stability and flaky tests remained a blocker per the survey results. The consumer experience required that you click a “Retry” button on your[Jenkins](https://www.jenkins.io/) build whenever you had a test failure, not knowing whether it was a result of your change or a test flake. There was an active Slack channel where people shared experiences over flakes to help triage whether they had a flake or a real failure.


Based on the survey results, we immediately enacted a plan to retry failed tests in isolation after all 54k tests were run. Credit for that idea goes to an engineer who had brought it up a few times before — this time we finally listened to him. “Isolated retries” were incrementally added for two test frameworks:


- [Rspec](http://rspec.info/) tests, via the[–only-failures tag](https://relishapp.com/rspec/rspec-core/docs/command-line/only-failures)
- [Cucumber](https://cucumber.io/) (feature) tests, via a homegrown retry feature — Cucumber does not have an equivalent to –only-failures


We surfaced isolated retries in[our test reporting tool](https://medium.com/upstart-tech/test-reporting-in-a-monolith-from-concept-to-delivery-7b9bf1b73c39) to monitor root causes. We saw an improvement in build success rate from ~70% to ~80% with isolated retries. Tests that passed in isolation experienced less data leaking (rspec) and less simultaneous resource demand (feature), leading to higher pass rates.


This 10% improvement translated to an improved user experience:


- less confusion and frustration
- less context switching
- less triaging
- less manually clicking the Jenkins retry button expecting a different outcome


Monolith build pass rate progress throughout the year.


## Q4: More Flaky Test Work


Even with the iterative improvements throughout 2022, we wanted to surpass the 80% pass rate. We aimed to increase our deployment frequency while improving stability. We set a milestone to reach and maintain 90% build pass rates. By increasing the pass rate, we build trust for end users to ensure in CI that failures are due to their change and not flaky tests.


We aimed to minimize the manual intervention required to notify and support our build stability manifested in flaky tests. Our overall strategy included a few pieces:


- Reporting/Monitoring: Examination across many builds to understand impact of flakes to prioritize or triage.
- Prevention: Linting, when we know what causes flakes.
- Notification: Notify responsible and relevant teams when flakes are failing builds. Also leveraging DataDog for automated alerts around “build health”.
- Education: Building out and evangelizing causes of flakes and strategies for flakes to support prevention when linting can not.


Throughout Q4, we made progress on many of these components and maintained a 90% build pass rate.


## 2023: What’s Next?


Our build pass rate hovers around 90% heading into 2023. The reality is that flaky tests are regularly exposed and added to our test suite of ~54k tests. I believe that we should aim to maintain a 90%+ pass rate, add tools that minimize the introduction of flakes, and adjust processes to reduce flake blast radius.


As a retrospective, I examined the last three months of flakes to guide next steps.


Press enter or click to view image in full size


Our flakes from the last few months can be categorized (with nuance) into a few groups:


- **Time specific** : This set of flakes comes from assertions that begin to fail at a specific date or time. The test may have passed at the time it was written, and usually fails at some point in the future or under a specific time condition (e.g. time of day / holiday).
- **Mock leak** : This set of flakes comes from mocks leaking between tests, e.g. allow_any_instance_of(*ClassName*) has shown to leak in our tests. We advise on using allow(object) instead of allow_any_instance_of mocking.
- **Global def leak** : This set of flakes comes from global definitions living inside the root level of feature/ or spec/ files. These methods can leak into other code with methods of the same name. We now have a blocking linter to prevent this flake cause.
- **ENV leak** : This set of flakes comes from cases where setting ENV values in one test leaks to another. We lint this now (blocking) — with the recommended approach of using ClimateControl mocking instead.
- **Constant leak** : Similar to the ENV leakages, this set of flakes comes from cases where we set a constant in one test and fail to reset it before other tests. Test ordering can then fail downstream tests that rely on this constant set upstream.
- **Data Type comparison** : This set of flakes comes from scenarios where we are asserting values from data types that are not equivalent, e.g. we have seen it most in comparing BigDecimals to Floats. We remediate these flakes by updating the data type to match.
- **Capybara / browser issue** : This category of flakes includes a variety of causes related to browser issues — including but not limited to timing issues, delay issues, or other third party reliability issues. Of our 54k tests, ~2% of them are feature tests, but we see a disproportionate amount of flakes here. We can mitigate these, but I advise shifting tests left to minimize unexpected browser behavior and third party impact.
- **Bad data assertion** : This set of flakes occurs when we are asserting around data that we expect to be in a specific state, and at test run time it’s not in that state. For example, we may assert a specific attribute on an object, but a previous test or randomized behavior has modified the data resulting in failure. This type of error can be mitigated by ensuring data is cleaned between tests.
- **Data Creation** : This is a broad category of flakes that fail due to data creation, either that the test relies on data to be created in a specific order, the data created is not random enough, the random data created breaks validation, or the assertion is not robust enough.


### Where do we go from here?


The above data reveals opportunities for further flaky test prevention, targeting high frequency issues. I also believe that many of the other flake types are difficult to lint, so my general guidelines for minimizing flaky tests are:


- Don’t write tests asserting around a specific time unless you can freeze in the past (Note: browsers don’t have awareness around server side time freezing. This means integration tests are unable to take advantage of this testing technique).
- Try to shift tests left, separating the logic and data layers. Reduce dependencies on browsers, third parties and data, generally speaking.
- Don’t leak your constants.
- Enact stricter typing in your code and tests to minimize type flakes. We are accomplishing this through a combination of 1) implementing[Sorbet](https://sorbet.org/) in our application code, and 2) raising awareness of this type of test failure.


We’ve taken great strides in improving the development experience to iterate and release code in our monolith, and I hope it enables us to support our move to a microservice ecosystem throughout 2023!
