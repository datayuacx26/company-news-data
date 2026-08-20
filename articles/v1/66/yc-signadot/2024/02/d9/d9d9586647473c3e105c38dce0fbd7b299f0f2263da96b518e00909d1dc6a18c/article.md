---
schema_version: "1.0.0"
document_id: "d9d9586647473c3e105c38dce0fbd7b299f0f2263da96b518e00909d1dc6a18c"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-diagnose-flaky-tests/"
published_at: "2024-02-10T23:53:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:e95ee4b7fbd7a671d4fd29ea469739438cd11dc598e04c9c80d725b63e08f694"
---

# How to Diagnose Flaky Tests

*Flaky tests are a pervasive issue in software development, particularly as teams scale up and systems become more complex.*


‍


In a thread on the excellent r/softwaretesting subreddit, user[u/establishedcode](https://www.reddit.com/r/softwaretesting/comments/17ccj02/how_do_you_deal_with_unstable_test_automation/) asks how others deal with flaky tests:


Our company is adopting a lot more automated testing. We have almost 500 tests now, but we are struggling quite a bit with flaky test detection. When tests fail, it is not clear why tests fail. We have retries, which helps a lot but introduces other issues (like failing tests retrying for a long time unnecessarily).


‍


I am trying to understand how much of this is unique to our small startup vs universal problems, and how others deal with it.


When you’re faced with a large body of tests (500 is about the number where this starts), the whole set of tests starts to feel like a living organism that refuses to behave the same way twice in a row. If you’ve never experienced it before, you think: “this can’t possibly be normal… is this normal?”


### **Flaky Tests Happen to (Almost) Everyone**


Flaky tests, a common challenge in software development and platform engineering, are often seen as a nuisance, but their impact on developer velocity can be quite significant. In a modern application development architecture, automated tests within a CI/CD workflow are considered standard. Once your team grows large enough, releases will generally require that tests pass before deployment, so any failures block developers and require debugging. Developers must do something to make failing tests pass, but if tests are flaky, failing for reasons unrelated to the most recent update, it requires extra work by the whole team and can create bigger organizational challenges.


## **What Causes Flaky Tests**


Quite simply, scale causes flaky tests. At a certain point, a large enough system of software checks will stop acting deterministically, especially if that array of tests is owned by different teams with different testing agendas. Another source of inconsistent failures is using response time as a requirement. A million things can contribute to slow response times, including how heavily the test environment is being used. While it’s important to measure sensitivity to infrastructure issues, finding out that a test you’ve been debugging for 12 hours was caused by the test cluster running out of RAM is… frustrating to say the least.


Flakiness can range from issues with the test environment, such as non-deterministic data or timing issues, to problems within the test code, like race conditions or reliance on external services that may not always behave consistently. Removing response time expectations isn’t a satisfactory solution; after all, if responses take more than 20 seconds, you don’t want to cut off that information. However, if you measure time and your machine has limited resources, some tests will perform slightly differently on each run.


In some cases, the application code may have hidden bugs that only manifest under certain conditions, making the tests appear flaky. This implies that flaky tests are clues leading to fascinating unexpected behavior in the application code. The first few times we see flaky tests, we platform people hope to discover another[500-mile email problem](https://web.mit.edu/jemorris/humor/500-miles) . But this isn’t the case most of the time, and the timing issues mentioned above are much more frequently the cause.


Identifying the exact cause of flakiness can be a time-consuming and frustrating process, requiring deep dives into both the test and application code. This investigation often diverts valuable resources away from feature development or bug fixes, further slowing down the development process.


### **How Flaky Tests Hurt Your Team**


Once tests become flaky, failing non-deterministically with high frequency, some argue that the entire test suite has lost its value. After all, if a developer makes a commit, runs tests and gets failures, and they have any reason to suspect that their changes didn’t cause those failures, the whole process can quickly break down. Developers can spend so much time validating their code before running tests that they’re sure test failures are the result of flakiness. Instead of trying to get tests to go green, engineers waste time rerunning tests, complaining about test flakiness or otherwise approaching organizational vapor-lock.


On the[Slack engineering blog, Arpita Patel](https://slack.engineering/handling-flaky-tests-at-scale-auto-detection-suppression/) writes about how problems with flaky tests are the number one factor impacting mobile developers’ happiness. Users of the developer tools felt that tests took twice as long as expected, and all their CI/CD errors came down to test flakiness.


The impact of flaky tests on developer morale and productivity cannot be overstated. When developers constantly face test failures that have no relation to their current work, it can lead to a sense of futility and frustration. The uncertainty introduced by flaky tests can lead to a lack of trust in the testing process itself. Developers might start to ignore failing tests, assuming them to be flaky, which can allow real issues to slip through undetected. This erosion of trust in the testing process undermines one of the core principles of CI/CD, potentially leading to lower-quality releases.


## Solving the Flaky Test Problem


How do you solve the problem of flaky tests? There are a number of solutions, only some of which involve preventing inconsistent test results. In my next article, I’ll cover various solutions to flaky tests, some of which involve changing testing process, testing environments, and even the way you think about tests.


‍
